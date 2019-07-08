---
title: Migrar suas Licenças de Acesso para Cliente de Serviços de Área de Trabalho Remota (RDS CALs)
description: Este artigo descreve como migrar suas Licenças de Acesso para Cliente dos Serviços de Área de Trabalho Remota para novos servidores de licença do Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
msreviewer: ''
nams.suite: ''
nams.technology: remote-desktop-services
ms.author: chrimo
ms.date: 11/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91bdedce-6145-469f-b72e-7e113c4391e9
author: christianmontoya
manager: scottman
ms.openlocfilehash: c947375b58c0ad88781335b799055e101bd2a193
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66447102"
---
# <a name="migrate-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Migrar suas Licenças de Acesso para Cliente de Serviços de Área de Trabalho Remota (RDS CALs)

Você tem três opções para migrar as CALs para Serviços de Área de Trabalho Remota:
1. Método de conexão automática: Este método recomendado comunica-se por meio da Internet diretamente à Microsoft Clearinghouse, com saída pela porta TCP 443.  
2. Usar um navegador da Web: Esse método permite a migração quando o servidor que executa a ferramenta de Gerenciador de Licenciamento de Área de Trabalho Remota não tem conectividade com a Internet, mas o administrador tem conectividade com a Internet em um dispositivo separado. A URL para o método de migração da Web é exibida no Assistente de Gerenciamento de CALs para Serviços de Área de Trabalho Remota. 
3. Usar um telefone: Esse método permite ao administrador concluir o processo de migração por telefone com um representante da Microsoft. O número de telefone apropriado é determinado pelo país/região que você escolheu no Assistente para Ativação do Servidor e é exibido no Assistente de Gerenciamento de CALs para Serviços de Área de Trabalho Remota.

Neste artigo, [Estabelecer método de migração de CAL para Serviços de Área de Trabalho Remota](#establish-rds-cal-migration-method) destaca as etapas gerais comuns em qualquer método de migração de CAL para Serviços de Área de Trabalho Remota, enquanto [Migrar CALs para Serviços de Área de Trabalho Remota](#migrate-rds-cals) destaca as etapas específicas a cada método de migração.

Independentemente do método de migração, você deve, no mínimo, ser um membro do grupo Administradores local para executar as etapas de migração.

## <a name="establish-rds-cal-migration-method"></a>Estabelecer o método de migração de CAL para Serviços de Área de Trabalho Remota

1. No servidor de licença, abra o **Gerenciador de Licenciamento de Área de Trabalho Remota**. Clique em **Iniciar > Ferramentas Administrativas**. Insira o diretório dos **Serviços de Área de Trabalho Remota** e inicie o **Gerenciador de Licenciamento de Área de Trabalho Remota**.)
2. Verifique o método de conexão para o servidor de licenças de Área de Trabalho Remota: clique com o botão direito do mouse no servidor de licenças para o qual você deseja migrar as CALs para Serviços de Área de Trabalho Remota e, em seguida, clique em **Propriedades**. Na guia **Método de Conexão**, verifique o **Método de conexão** – você pode alterá-lo no menu suspenso. Clique em **OK**.
3. Clique com o botão direito do mouse no servidor de licença para o qual deseja migrar as CALs para Serviços de Área de Trabalho Remota e clique em **Gerenciar CALs para Serviços de Área de Trabalho Remota**.
4. Siga as etapas no assistente para a página **Seleção de Ação**. Clique em **Migrar CALs para Serviços de Área de Trabalho Remota de outro servidor de licença para este servidor de licença**.
6. Escolha o motivo para migrar as CALs para Serviços de Área de Trabalho Remota e clique em **Avançar**. Você tem as seguintes opções:
    - O servidor de licença de origem está sendo substituído por este servidor de licença.
    - O servidor de licença de origem não está mais em funcionamento.
7. A próxima página do assistente depende do motivo de migração que você escolheu.
    - Se você escolheu **o servidor de licença de origem está sendo substituído por este servidor de licença** como o motivo para migrar as CALs para Serviços de Área de Trabalho Remota, a página **Informações do servidor de licença de origem** é exibida.
    
       Na página de Informações do servidor de licença de origem, insira o nome ou endereço IP do servidor de licença de origem.

       Se o servidor de licença de origem está disponível na rede, clique em **Avançar**. O assistente entra em contato com o servidor de licença de origem. Se o servidor de licença de origem estiver executando um sistema operacional anterior ao Windows Server 2008 R2 ou o estiver desativado, será exibido um lembrete de que você deve remover as CALs para Serviços de Área de Trabalho Remota manualmente do servidor de licença de origem após a conclusão do assistente. Depois de confirmar que você compreende esse requisito, a página **Obter pacote de chaves de licença do cliente** será exibida.

       Se o servidor de licença de origem não está disponível na rede, selecione **O servidor de licença de origem especificado não está disponível na rede**. Especifique o sistema operacional que o servidor de licença de origem está executando e, em seguida, forneça a ID do servidor de licença para o servidor de licença de origem. Depois de clicar em **Avançar**, será exibido um lembrete de que você deve remover as CALs para Serviços de Área de Trabalho Remota manualmente do servidor de licença de origem após a conclusão do assistente. Depois de confirmar que você compreende esse requisito, a página **Obter pacote de chaves de licença do cliente** será exibida.

    - Se você escolher **O servidor de licença de origem não está mais funcionando** como o motivo para migrar as CALs para Serviços de Área de Trabalho Remota, será exibido um lembrete de que você deve remover as CALs para Serviços de Área de Trabalho Remota manualmente do servidor de licença de origem após a conclusão do assistente. Depois de confirmar que você compreende esse requisito, a página **Obter pacote de chaves de licença do cliente** será exibida.

A próxima etapa é migrar as CALs – use as informações abaixo para concluir o assistente. Observe que o que você vê no assistente depende do método de conexão que você identificou na etapa 2 acima.

## <a name="migrate-rds-cals"></a>Migrar CALs para Serviços de Área de Trabalho Remota

Existem três mecanismos para migrar licenças para o servidor de licença de destino. Continue as etapas correspondentes ao **Método de conexão** verificado na etapa 2:
  - [Método de conexão automática](#automatic-connection-method)
  - [Usar um navegador da Web](#using-a-web-browser)
  - [Usar um telefone](#using-a-telephone)

### <a name="automatic-connection-method"></a>Método de conexão automática

1. Na página **Programa de Licenciamento**, selecione o programa apropriado através do qual as Licenças de Acesso do Cliente para Serviços de Área de Trabalho Remota foram adquiridas e clique em **Avançar**.
2. Insira as informações necessárias (normalmente um código de licença ou um número de contrato, dependendo do **Programa de licenciamento**) e, em seguida, clique em **Avançar**. Consulte a documentação fornecida quando as CALs para Serviços de Área de Trabalho Remota foram compradas.
4. Selecione a versão do produto apropriada, o tipo de licença e a quantidade de CALs para Serviços de Área de Trabalho Remota para o seu ambiente baseado no contrato de compra destas e clique em **Avançar**.
5. A Microsoft Clearinghouse é contatada automaticamente e processa sua solicitação. As CALs para Serviços de Área de Trabalho Remota são então migradas para o servidor de licença.
6. Clique em **Concluir** para concluir o primeiro processo de migração de CAL para Serviços de Área de Trabalho Remota.

### <a name="using-a-web-browser"></a>Usar um navegador da Web
1. Na página **Obter pacote de chaves de licença de cliente**, clique no hiperlink para se conectar ao site Licenciamento de Serviços de Área de Trabalho Remota.
   Se você estiver executando o Gerenciador de Licenciamento de Área de Trabalho Remota em um computador que não tem conectividade com a Internet, anote o endereço do site Licenciamento de Serviços de Área de Trabalho Remota e, em seguida, conecte-se ao site da Web por meio de um computador que tenha conectividade com a Internet. 
2. Na página da Web de Licenciamento do Serviços de Área de Trabalho Remota, em **Selecionar Opção**, selecione **Gerenciar CALs** e, em seguida, clique em **Avançar**.
3. Forneça as seguintes informações obrigatórias e depois clique em **Avançar**:
    - **ID do servidor de licença de destino**: Um número de 35 dígitos, em grupos de 5 algarismos, que é exibido na página **Obter pacote de chaves de licença do cliente** no Assistente de Gerenciamento de CALs para Serviços de Área de Trabalho Remota.
    - **Motivo para a recuperação**: Escolha o motivo para migrar as CALs para Serviços de Área de Trabalho Remota.
    - **Programa de licenciamento**: Escolha o programa por meio do qual você adquiriu as CALs para Serviços de Área de Trabalho Remota.
4. Forneça as seguintes informações obrigatórias e depois clique em **Avançar**:
   - Sobrenome
   - Nome
   - Nome da empresa
   - País/região

     Você também pode fornecer as informações opcionais solicitadas, tais como endereço, endereço de email e número de telefone da empresa. No campo unidade organizacional, você pode descrever a unidade de sua organização que é atendida por este servidor de licença.

5. O programa de licenciamento selecionado na página anterior determina quais informações devem ser fornecidas na próxima página. Na maioria dos casos, será necessário fornecer um código de licença ou um número de contrato. Consulte a documentação fornecida quando as CALs para Serviços de Área de Trabalho Remota foram compradas. Além disso, você precisa especificar o tipo de CAL para Serviços de Área de Trabalho Remota e a quantidade que você deseja migrar para o servidor de licença.
6. Depois de inserir as informações necessárias, clique em **Avançar**.
7. Verificar se todas as informações que você inseriu estão corretas e, em seguida, clique em **Avançar** para enviar sua solicitação para a Microsoft Clearinghouse. A página da Web exibe então uma ID de pacote de chaves de licença gerada pela Microsoft Clearinghouse.

   > [!IMPORTANT] 
   > Mantenha uma cópia da ID do pacote de chaves de licença. A posse dessas informações facilita a comunicação com a Microsoft Clearinghouse caso você necessite de assistência para recuperar as CALs para Serviços de Área de Trabalho Remota.

8. Na mesma página **Obter pacote de chaves de licença do cliente**, insira a ID do pacote de chaves de licença e, em seguida, clique em **Avançar** para migrar as CALs para Serviços de Área de Trabalho Remota para o servidor de licença.
9. Clique em **Concluir** para concluir o primeiro processo de migração de CAL para Serviços de Área de Trabalho Remota.

### <a name="using-a-telephone"></a>Usar um telefone
1. Na página **Obter pacote de chaves de licença do cliente**, use o número de telefone exibido para chamar a Microsoft Clearinghouse. Forneça ao representante sua ID de servidor de licenças de Área de Trabalho Remota e as informações necessárias para o programa de licenciamento por meio do qual você adquiriu as CALs para Serviços de Área de Trabalho Remota. O representante processará sua solicitação para migrar as CALs para Serviços de Área de Trabalho Remota e fornecerá a você uma ID exclusiva para elas. Essa ID exclusiva é conhecida como a **ID do pacote de chaves de licença**.

   > [!IMPORTANT]
   > Mantenha uma cópia da ID do pacote de chaves de licença. A posse dessas informações facilita a comunicação com a Microsoft Clearinghouse caso você necessite de assistência para recuperar as CALs para Serviços de Área de Trabalho Remota.

2. Na mesma página **Obter pacote de chaves de licença do cliente**, insira a ID do pacote de chaves de licença e, em seguida, clique em **Avançar** para migrar as CALs para Serviços de Área de Trabalho Remota para o servidor de licença.
3. Clique em **Concluir** para concluir o primeiro processo de migração de CAL para Serviços de Área de Trabalho Remota.
