---
title: Adicionar um servidor de Agente de Conexão de Área de Trabalho Remota para configurar a alta disponibilidade em RDS
description: Saiba como adicionar um agente de Conexão de área de trabalho remota para uma implantação do RDS para alta disponibilidade.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 511f852568aa4cc7498e3a0b8deacea83db22c08
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404055"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>Adicionar o servidor do Agente de Conexão de Área de Trabalho Remota à implantação e configurar a alta disponibilidade

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Você pode implantar um cluster do Agente de Conexão RD (Agente de Conexão da Área de Trabalho Remota) para melhorar a disponibilidade e escala de sua infraestrutura de Serviços de Área de Trabalho Remota. 

## <a name="pre-requisites"></a>Pré-requisitos

Configure um servidor para atuar como um segundo Agente de Conexão RD, que pode ser um servidor físico ou estar em uma VM.

Configure um banco de dados para o Agente de Conexão. É possível usar uma instância do [Banco de dados do Azure SQL](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database) ou o SQL Server em seu ambiente local. Falamos a seguir sobre como usar o Azure SQL, mas as etapas também se aplicam ao SQL Server. Você precisará encontrar a cadeia de conexão do banco de dados e verificar se você tem o driver ODBC correto.

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>Etapa 1: configurar um banco de dados para o Agente de Conexão

1. Encontre a cadeia de conexão do banco de dados que você criou - você precisa de ambos para identificar a versão adequada do driver ODBC e depois, ao configurar o Agente de Conexão em si (etapa 3), portanto, salve a cadeia de caracteres em algum lugar em que possa consultá-la com facilidade. Aqui está como encontrar a cadeia de conexão para o Azure SQL:  
    1. No portal do Azure, clique em **Procurar > Grupos de recursos** e clique no grupo de recursos para a implantação.   
    2. Selecione o banco de dados do SQL que você acabou de criar (por exemplo, CB-DB1).   
    3. Clique em **Configurações** > **Propriedades** > **Mostrar cadeias de conexão de banco de dados**.   
    4. Copie a cadeia de conexão para **ODBC (inclui Node.js)** , que deve parecer com o seguinte:   
      
        ```
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
        ```
  
    5. Substitua "your_password_here" pela senha real. Você usará essa cadeia de caracteres inteira, com sua senha incluída, ao se conectar ao banco de dados. 
2. Instale o driver ODBC no novo Agente de Conexão: 
   1. Se você estiver usando uma VM como Agente de Conexão, crie um endereço IP público para o primeiro Agente de Conexão RD. (Você só precisa fazer isso se a máquina virtual RDMS ainda não tiver um endereço IP público para permitir conexões RDP.)
       1. No portal do Azure, clique em **Procurar** > **Grupos de recursos**, clique no grupo de recursos para a implantação e depois na VM criada para o Agente de Conexão RD (por exemplo, Contoso-Cb1).
       2. Clique em **Configurações > Interfaces de rede** e no adaptador de rede correspondente.
       3. Clique em **Configurações > Endereço IP**.
       4. Em **Endereço IP público**, selecione **Habilitado** e clique em **Endereço IP**.
       5. Se tiver um endereço IP público que deseja usar, selecione-o na lista. Caso contrário, clique em **Criar novo**, insira um nome e clique em **OK** e em **Salvar**.
   2. Conecte-se ao primeiro Agente de Conexão RD:
       1. No portal do Azure, clique em **Procurar** > **Grupos de recursos**, clique no grupo de recursos para a implantação e depois na VM criada para o Agente de Conexão RD (por exemplo, Contoso-Cb1).
       2. Clique em **Conectar > Abrir** para abrir o cliente de Área de Trabalho Remota.
       3. No cliente, clique em **Conectar** e em **Usar outra conta de usuário**. Insira o nome de usuário e a senha da conta de administrador de domínio.
       4. Clique em **Sim** quando for avisado sobre o certificado.
   3. Baixe o [Driver ODBC para SQL Server](https://www.microsoft.com/download/confirmation.aspx?id=50420) que corresponde à versão na cadeia de conexão ODBC. Para a cadeia de exemplo acima, é necessário instalar o driver ODBC da versão 13.
   4. Copie o arquivo sqlincli.msi no primeiro servidor do Agente de Conexão RD.   
   5. Abra o arquivo sqlincli.msi e instale o cliente nativo.  
   6. Repita as etapas de 1 a 5 para cada Agente de Conexão RD adicional (por exemplo, Contoso-Cb2).
   7. Instale o driver ODBC em cada servidor que executará o agente de conexão.

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>Etapa 2: Configurar o balanceamento de carga nos Agentes de Conexão de Área de Trabalho Remota 

Se estiver usando a infraestrutura do Azure, você poderá criar um [Balanceador de carga do Azure](#create-a-load-balancer); caso contrário, pode definir o [round robin de DNS](#configure-dns-round-robin).

### <a name="create-a-load-balancer"></a>Criar o balanceador de carga  
1. Criar um balanceador de carga do Azure   
      1. No portal do Azure, clique em **Procurar > Balanceadores de carga > Adicionar**.   
      2. Insira um nome para o novo balanceador de carga (por exemplo, hacb).   
      3. Selecione **Interno** para o **Esquema**, a **Rede Virtual** da sua implantação (por exemplo, Contoso-VNet) e a **Sub-rede** com todos os seus recursos (por exemplo, padrão).   
      4. Selecione **Estático** para a **Atribuição de endereço IP** e insira um **Endereço IP Privado** que não esteja atualmente em uso (por exemplo, 10.0.0.32).   
      5. Selecione a devida **Assinatura**, o **Grupo de recursos** com todos os seus recursos e o **Local** apropriado.   
      6. Selecione **Criar**.   
2. Crie uma [Investigação](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) para monitorar os servidores que estão ativos:   
      1. No portal do Azure, clique em **Procurar > Balanceadores de carga** e no balanceador de carga recém-criado, por exemplo, CBLB. Clique em **Configurações**.   
      2. Clique em **Investigações > Adicionar**.   
      3. Insira um nome para a investigação (por exemplo, **RDP**), selecione **TCP** como o **Protocolo**, digite **3389** na **Porta** e clique em **OK**.   
3. Crie o pool de back-end dos Agentes de Conexão:   
      1. Nas **Configurações**, clique em **Pools de endereços de back-end > Adicionar**.   
      2. Insira um nome (por exemplo, CBBackendPool) e clique em **Adicionar uma máquina virtual**.  
      3. Escolha um conjunto de disponibilidade (por exemplo, CbAvSet) e clique em **OK**.   
      3. Clique em **Escolher as máquinas virtuais**, selecione cada máquina virtual e clique em **Selecionar > OK > OK**.   
4. Crie a regra de balanceamento de carga RDP:   
      1. Nas **Configurações**, clique em **Regras de balanceamento de carga** e em **Adicionar**.   
      2. Insira um nome (por exemplo, RDP), selecione **TCP** como o **Protocolo**, digite **3389** na **Porta** e na **Porta de back-end** e clique em **OK**.   
5. Adicione um registro DNS para o balanceador de carga:   
      1. Conecte-se à máquina virtual de servidor RDMS (por exemplo, Contoso-CB1). ***Confira o artigo [Preparar a VM do Agente de Conexão de Área de Trabalho Remota](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) para obter as etapas de conexão à VM.   
      2. No Gerenciador do Servidor, clique em **Ferramentas > DNS**.   
      3. No painel esquerdo, expanda **DNS**, clique na máquina DNS, clique em **Zonas de pesquisa direta** e em seu nome de domínio (por exemplo, Contoso.com). Pode levar alguns segundos para o servidor DNS processar a consulta para obter as informações.  
      4. Clique em **Ação > Novo Host (A ou AAAA)** .   
      9. Insira o nome (por exemplo, hacb) e o endereço IP especificado anteriormente (por exemplo, 10.0.0.32).   

### <a name="configure-dns-round-robin"></a>Configurar o round robin de DNS  
  
As etapas a seguir são uma alternativa para criar um balanceador de carga interno do Azure.   
  
1. Conecte-se ao servidor RDMS no portal do Azure. Usando o cliente da Conexão de Área de Trabalho Remota   
2. Crie registros de DNS:   
      1. No Gerenciador do Servidor, clique em **Ferramentas > DNS**.   
      2. No painel esquerdo, expanda **DNS**, clique na máquina DNS, clique em **Zonas de pesquisa direta** e em seu nome de domínio (por exemplo, Contoso.com). Pode levar alguns segundos para o servidor DNS processar a consulta para obter as informações.  
      3. Clique em **Ação** e **Novo Host (A ou AAAA)** .   
      4. Insira o **Nome DNS** para o Agente de Conexão RD do cluster (por exemplo, hacb) e insira o **Endereço IP** do Agente de Conexão RD.   
      5. ***Repita as etapas 3 e 4 para cada Agente de Conexão RD adicional, fornecendo um endereço IP exclusivo para cada registro adicional.


Por exemplo, se os endereços IP das duas máquinas virtuais de Agente de Conexão RD forem 10.0.0.8 e 10.0.0.9, você criaria dois registros de host DNS:
 - Nome do host: hacb.contoso.com, endereço IP: 10.0.0.8
 - Nome do host: hacb.contoso.com, endereço IP: 10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>Etapa 3: Configurar os Agentes de Conexão para alta disponibilidade

1. Adicione o servidor do Agente de Conexão RD ao Gerenciador de Servidor:
   1. No Gerenciador do Servidor, clique em **Gerenciar > Adicionar Servidores**.
   2. Clique em **Localizar Agora**.
   3. Clique no servidor recém-criado (por exemplo, Contoso-Cb2) do Agente de Conexão RD e clique em **OK**.
2. Configure a alta disponibilidade do Agente de Conexão RD:
   1. No Gerenciador de Servidor, clique em **Serviços de Área de Trabalho Remota > Visão geral**.
   2. Clique com botão direito do mouse no **Agente de Conexão de Área de Trabalho Remota** e clique em **Configurar Alta Disponibilidade**.
   3. Passe pelo assistente até chegar à seção Configuração de tipo. Selecione **Servidor de banco de dados compartilhado** e clique em **Avançar**.
   4. Insira o nome DNS do cluster do Agente de Conexão RD.
   5. Insira a cadeia de conexão do banco de dados SQL e prossiga pelo assistente até estabelecer a alta disponibilidade.
3. Adicionar o novo Agente de Conexão de Área de Trabalho Remota à implantação
   1. No Gerenciador de Servidor, clique em **Serviços de Área de Trabalho Remota > Visão geral**.
   2. Clique com o botão direito do mouse no Agente de Conexão da Área de Trabalho Remota e clique em **Adicionar Servidor do Agente de Conexão de Área de Trabalho Remota**.
   3. Prossiga no assistente até chegar à Seleção de Servidor, em seguida, selecione o servidor recém-criado (por exemplo, Contoso-CB2) do Agente de Conexão RD.
   4. Conclua o assistente, aceitando os valores padrão.
4. Configurar certificados confiáveis em clientes e servidores do Agente de Conexão RD.

