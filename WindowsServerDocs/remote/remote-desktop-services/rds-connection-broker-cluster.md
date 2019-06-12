---
title: Adicionar um servidor do agente de Conexão de área de trabalho remota para configurar a alta disponibilidade no RDS
description: Saiba como adicionar um agente de Conexão de área de trabalho remota para uma implantação do RDS para alta disponibilidade.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/10/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: b1e5726e3976527278b11f105007a32548da0bc4
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805156"
---
# <a name="add-the-rd-connection-broker-server-to-the-deployment-and-configure-high-availability"></a>Adicionar o servidor do Agente de Conexão de Área de Trabalho Remota à implantação e configurar a alta disponibilidade

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016

Você pode implantar um cluster do agente de Conexão de área de trabalho remota (agente de Conexão de área de trabalho remota) para melhorar a disponibilidade e escala de sua infraestrutura de serviços de área de trabalho remota. 

## <a name="pre-requisites"></a>Pré-requisitos

Configurar um servidor para atuar como um segundo agente de Conexão de área de trabalho remota — isso pode ser um servidor físico ou em uma VM.

Configure um banco de dados para o agente de Conexão. Você pode usar [banco de dados SQL](https://azure.microsoft.com/documentation/articles/sql-database-get-started/#create-a-new-aure-sql-database) instância ou o SQL Server em seu ambiente local. Falamos sobre como usar o SQL do Azure abaixo, mas as etapas ainda se aplicam ao SQL Server. Você precisará encontrar a cadeia de conexão para o banco de dados e verifique se que você tem o driver ODBC correto.

## <a name="step-1-configure-the-database-for-the-connection-broker"></a>Etapa 1: Configurar o banco de dados para o agente de Conexão

1. Encontrar a cadeia de conexão para o banco de dados que você criou - você precisa-la para identificar a versão do driver ODBC, você precisa e mais tarde, quando você estiver configurando o agente de Conexão em si (etapa 3), portanto, salve a cadeia de caracteres em algum lugar em que você possa referenciá-la facilmente. Aqui está como você encontrar a cadeia de caracteres de conexão para o SQL Azure:  
    1. No portal do Azure, clique em **procurar > grupos de recursos** e clique no grupo de recursos para a implantação.   
    2. Selecione o banco de dados do SQL que você acabou de criar (por exemplo, DB1 CB).   
    3. Clique em **as configurações** > **as propriedades** > **Mostrar cadeias de conexão de banco de dados**.   
    4. Copie a cadeia de conexão para **ODBC (inclui Node. js)** , que deve ser assim:   
      
        ```
        Driver={SQL Server Native Client 13.0};Server=tcp:cb-sqls1.database.windows.net,1433;Database=CB-DB1;Uid=sqladmin@contoso;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
        ```
  
    5. Substitua "your_password_here" com a senha real. Você usará essa cadeia de caracteres inteira, com sua senha incluída, ao se conectar ao banco de dados. 
2. Instale o driver ODBC no novo agente de Conexão: 
   1. Se você estiver usando uma VM para o agente de Conexão, crie um endereço IP público para o primeiro agente de Conexão de área de trabalho. (Você só precisará fazer isso se a máquina virtual RDMS ainda não tiver um endereço IP público para permitir conexões de RDP.)
       1. No portal do Azure, clique em **navegue** > **grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique na primeira máquina virtual de agente de Conexão de área de trabalho (por exemplo, Contoso-Cb1).
       2. Clique em **Configurações > interfaces de rede**e, em seguida, clique em interface de rede correspondente.
       3. Clique em **Configurações > endereço IP**.
       4. Para **endereço IP público**, selecione **Enabled**e, em seguida, clique em **endereço IP**.
       5. Se você tiver um endereço IP público existente que você deseja usar, selecione-o na lista. Caso contrário, clique em **criar novo**, insira um nome e, em seguida, clique em **Okey** e, em seguida, **salvar**.
   2. Conecte-se para o primeiro agente de Conexão de área de trabalho remota:
       1. No portal do Azure, clique em **navegue** > **grupos de recursos**, clique no grupo de recursos para a implantação e, em seguida, clique na primeira máquina virtual de agente de Conexão de área de trabalho (por exemplo, Contoso-Cb1).
       2. Clique em **Connect > Abrir** para abrir o cliente de área de trabalho remota.
       3. No cliente, clique em **Connect**e, em seguida, clique em **usar outra conta de usuário**. Insira o nome de usuário e senha para uma conta de administrador de domínio.
       4. Clique em **Sim** quando avisado sobre o certificado.
   3. Baixe o [ODBC driver para SQL Server](https://www.microsoft.com/download/confirmation.aspx?id=50420) que corresponde à versão na cadeia de caracteres de conexão ODBC. Para a cadeia de exemplo acima, é necessário instalar o driver ODBC 13 da versão.
   4. Copie o arquivo de sqlincli.msi para o primeiro servidor do agente de Conexão de área de trabalho.   
   5. Abra o arquivo sqlincli.msi e instale o native client.  
   6. Repita as etapas 1 a 5 para cada agentes de Conexão de área de trabalho remota (por exemplo, Contoso-Cb2) adicionais.
   7. Instale o driver ODBC em cada servidor que será executado o agente de conexão.

## <a name="step-2-configure-load-balancing-on-the-rd-connection-brokers"></a>Etapa 2: Configurar o balanceamento de carga em que os agentes de Conexão de área de trabalho remota 

Se você estiver usando a infraestrutura do Azure, você pode criar uma [balanceador de carga do Azure](#create-a-load-balancer); se não, você pode definir até [round robin DNS](#configure-dns-round-robin).

### <a name="create-a-load-balancer"></a>Criar um balanceador de carga  
1. Criar um balanceador de carga do Azure   
      1. No portal do Azure clique **procurar > balanceadores de carga > Adicionar**.   
      2. Insira um nome para o novo balanceador de carga (por exemplo, hacb).   
      3. Selecione **Internal** para o **esquema**, **rede Virtual** para sua implantação (por exemplo, Contoso-VNet) e o **sub-rede** com todos os de seus recursos (por exemplo, o padrão).   
      4. Selecione **estáticos** para o **atribuição de endereço IP** e insira um **endereço IP privado** que é não em uso no momento (por exemplo, 10.0.0.32).   
      5. Selecionar as devidas **assinatura**, o **grupo de recursos** com todos os seus recursos e apropriado **local**.   
      6. Selecione **Criar**.   
2. Criar uma [investigação](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) monitorar quais servidores estão ativos:   
      1. No portal do Azure, clique em **procurar > balanceadores de carga**e, em seguida, clique o balanceador de carga que você acabou de criar (por exemplo, CBLB). Clique em **Configurações**.   
      2. Clique em **investigações > Adicionar**.   
      3. Insira um nome para o teste (por exemplo, **RDP**), selecione **TCP** como o **protocolo**, digite **3389** para o **porta**e, em seguida, clique em **Okey**.   
3. Crie o pool de back-end dos agentes de Conexão:   
      1. Na **as configurações**, clique em **pools de endereços de back-end > Adicionar**.   
      2. Insira um nome (por exemplo, CBBackendPool) e, em seguida, clique em **adicionar uma máquina virtual**.  
      3. Escolha um conjunto de disponibilidade (por exemplo, CbAvSet) e, em seguida, clique em **Okey**.   
      3. Clique em **escolher as máquinas virtuais**, selecione cada máquina virtual e, em seguida, clique em **Selecionar > Okey > Okey**.   
4. Crie regra de balanceamento de carga RDP:   
      1. Na **as configurações**, clique em **regras de balanceamento de carga**e, em seguida, clique em **adicionar**.   
      2. Insira um nome (por exemplo, RDP), selecione **TCP** para o **protocolo**, insira **3389** para ambos **porta** e **porta de back-end** e clique em **Okey**.   
5. Adicione um registro DNS para o balanceador de carga:   
      1. Conecte-se à máquina virtual de servidor RDMS (por exemplo, Contoso-CB1). Confira a [preparar a VM de agente de Conexão de área de trabalho remota](Prepare-the-RD-Connection-Broker-VM-for-Remote-Desktop.md) artigo para obter etapas sobre como conectar-se à VM.   
      2. No Gerenciador do servidor, clique em **Ferramentas > DNS**.   
      3. No painel esquerdo, expanda **DNS**, clique na máquina DNS, clique em **zonas de pesquisa direta**e, em seguida, clique em seu nome de domínio (por exemplo, Contoso.com). (Pode levar alguns segundos para processar a consulta para o servidor DNS para obter as informações.)  
      4. Clique em **ação > Novo Host (A ou AAAA)** .   
      9. Insira o nome (por exemplo, hacb) e o endereço IP especificado anteriormente (por exemplo, 10.0.0.32).   

### <a name="configure-dns-round-robin"></a>Configurar DNS round robin  
  
As etapas a seguir são uma alternativa para criar um balanceador de carga interno do Azure.   
  
1. Conecte-se ao servidor RDMS no portal do Azure. usando o cliente de Conexão de área de trabalho remota   
2. Crie registros DNS:   
      1. No Gerenciador do servidor, clique em **Ferramentas > DNS**.   
      2. No painel esquerdo, expanda **DNS**, clique na máquina DNS, clique em **zonas de pesquisa direta**e, em seguida, clique em seu nome de domínio (por exemplo, Contoso.com). (Pode levar alguns segundos para processar a consulta para o servidor DNS para obter as informações.)  
      3. Clique em **ação** e **novo Host (A ou AAAA)** .   
      4. Insira o **nome DNS** para o agente de Conexão de área de trabalho de cluster (por exemplo, hacb) e, em seguida, insira o **endereço IP** do agente de Conexão de área de trabalho remota primeiro.   
      5. Repita as etapas 3 a 4 para cada agente de Conexão de área de trabalho adicionais, fornecendo a cada endereço IP exclusivo para cada registro adicional.


Por exemplo, se os endereços IP para as duas máquinas virtuais de agente de Conexão de área de trabalho são 10.0.0.8 e 10.0.0.9, você criaria dois registros de host DNS:
 - Nome do host: hacb.contoso.com, endereço IP: 10.0.0.8
 - Nome do host: hacb.contoso.com, endereço IP: 10.0.0.9

## <a name="step-3-configure-the-connection-brokers-for-high-availability"></a>Etapa 3: Configurar os agentes de Conexão para alta disponibilidade

1. Adicione o novo servidor do agente de Conexão de área de trabalho ao Gerenciador do servidor:
   1. No Gerenciador do servidor, clique em **Gerenciar > Adicionar servidores**.
   2. Clique em **Localizar Agora**.
   3. Clique no servidor do agente de Conexão de área de trabalho recém-criado (por exemplo, Contoso-Cb2) e clique em **Okey**.
2. Configure a alta disponibilidade para o agente de Conexão de área de trabalho remota:
   1. No Gerenciador do servidor, clique em **Remote Desktop Services > Visão geral do**.
   2. Clique com botão direito **agente de Conexão de área de trabalho**e, em seguida, clique em **configurar alta disponibilidade**.
   3. Página com o assistente até chegar à seção de tipo de configuração. Selecione **servidor de banco de dados compartilhado**e, em seguida, clique em **próxima**.
   4. Insira o nome DNS para o cluster do agente de Conexão de área de trabalho.
   5. Insira a cadeia de conexão para o banco de dados SQL e, em seguida, a página do Assistente para estabelecer a alta disponibilidade.
3. Adicionar o novo agente de Conexão de área de trabalho remota para a implantação
   1. No Gerenciador do servidor, clique em **Remote Desktop Services > Visão geral do**.
   2. O agente de Conexão de área de trabalho remota com o botão direito e, em seguida, clique em **Adicionar servidor de agente de Conexão de área de trabalho remota**.
   3. Página por meio do assistente até chegar à seleção de servidor, em seguida, selecione o servidor do agente de Conexão de área de trabalho recém-criado (por exemplo, Contoso-CB2).
   4. Conclua o assistente, aceitando os valores padrão.
4. Configure certificados confiáveis em clientes e servidores do agente de Conexão de área de trabalho remota.

