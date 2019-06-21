---
title: Associação offline de domínio DirectAccess
description: Este guia explica as etapas para realizar um ingresso no domínio offline com o DirectAccess no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 59b5933a81c7021e58ea14e6ea4c4da374ce35cb
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283659"
---
# <a name="directaccess-offline-domain-join"></a>Associação offline de domínio DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este guia explica as etapas para realizar um ingresso no domínio offline com o DirectAccess. Durante uma junção de domínio offline, um computador é configurado para ingressar em um domínio sem físico ou da conexão VPN.  
  
Este guia inclui as seguintes seções:  
  
- Visão geral de associação offline de domínio  
  
- Requisitos para o ingresso no domínio offline
  
- Processo de ingresso no domínio offline
  
- Etapas para executar um ingresso no domínio offline  
  
## <a name="offline-domain-join-overview"></a>Visão geral de associação offline de domínio  
Introduzido no Windows Server 2008 R2, os controladores de domínio incluem um recurso chamado de ingresso no domínio Offline. Um utilitário de linha de comando chamado Djoin.exe permite que você ingressar um computador em um domínio sem fisicamente contatar um controlador de domínio ao concluir a operação de junção de domínio. As etapas gerais para usar Djoin.exe são:  
  
1.  Execute **djoin /provision** para criar os metadados da conta de computador. A saída desse comando é um arquivo. txt que inclui um blob de codificado em base 64.  
  
2.  Execute **djoin /requestODJ** para inserir os metadados da conta de computador do arquivo. txt no diretório do Windows do computador de destino.  
  
3.  Reinicialize o computador de destino e o computador será ingressado no domínio.  
  
### <a name="BKMK_ODJOverview"></a>Ingresso no domínio offline com visão geral de cenário de políticas do DirectAccess  
Ingresso no domínio offline do DirectAccess é um processo que computadores que executam o Windows Server 2016, Windows Server 2012, Windows 10 e Windows 8 podem usar para ingressar em um domínio sem que é adicionada fisicamente à rede corporativa, ou conectados por meio de VPN. Isso torna possível ingressar computadores em um domínio de locais em que não há nenhuma conectividade com uma rede corporativa. Ingresso no domínio offline para o DirectAccess fornece as políticas do DirectAccess para clientes para permitir o provisionamento remoto.  
  
Ingresso no domínio cria uma conta de computador e estabelece uma relação de confiança entre um computador executando um sistema de operacional do Windows e um domínio do Active Directory.  
  
## <a name="BKMK_ODJRequirements"></a>Preparar para o ingresso no domínio offline  
  
1.  Crie a conta do computador.  
  
2.  A participação de todos os grupos de segurança ao qual pertence a conta do computador de estoque.  
  
3.  Colete os certificados de computador necessária, as diretivas de grupo e objetos de diretiva de grupo a ser aplicado a novos clientes.  
  
. As seções a seguir explicam os requisitos de credencial para executar uma junção de domínio offline do DirectAccess usando Djoin.exe e requisitos do sistema operacional.  
  
### <a name="operating-system-requirements"></a>Requisitos de sistema operacional  
Você pode executar Djoin.exe para o DirectAccess apenas em computadores que executam o Windows Server 2016, Windows Server 2012 ou Windows 8. O computador no qual você executa Djoin.exe para provisionar dados da conta de computador no AD DS deve estar executando Windows Server 2016, Windows 10, Windows Server 2012 ou Windows 8. O computador que você deseja ingressar no domínio também deve estar executando o Windows 8, Windows 10, Windows Server 2012 ou Windows Server 2016.  
  
### <a name="credential-requirements"></a>Requisitos de credenciais  
Para executar um ingresso no domínio offline, você deve ter os direitos que são necessários para ingressar em estações de trabalho ao domínio. Por padrão, os membros do grupo Admins. do domínio têm esses direitos. Se você não for um membro do grupo Admins. do domínio, um membro do grupo Admins. do domínio deve concluir uma das seguintes ações para que você possa adicionar estações de trabalho ao domínio:  
  
-   Use diretiva de grupo para conceder os direitos de usuário necessário. Esse método permite que você crie os computadores no contêiner de computadores padrão e em qualquer unidade organizacional (UO) que é criada mais tarde (se não há entradas de controle de acesso (ACEs) negar são adicionadas).  
  
-   Edite a lista de controle de acesso (ACL) do contêiner de computadores padrão para o domínio delegar as permissões corretas para você.  
  
-   Criar uma UO e editar a ACL no UO em conceder a você as **criar filho - permitir** permissão. Passe o **/machineOU** parâmetro para o **djoin /provision** comando.  
  
Os procedimentos a seguir mostram como conceder os direitos de usuário com a diretiva de grupo e como delegar as permissões corretas.  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>Concedendo direitos de usuário para ingressar em estações de trabalho ao domínio  
Você pode usar o grupo de política de gerenciamento GPMC (Console) para modificar a política de domínio ou crie uma nova política que tem configurações que concedem os direitos de usuário Adicionar estações de trabalho a um domínio.  
  
Associação na **Admins. do domínio**, ou equivalente, é o mínimo necessário para conceder direitos de usuário.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477) (https://go.microsoft.com/fwlink/?LinkId=83477).   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>Para conceder direitos para Adicionar estações de trabalho a um domínio  
  
1.  Clique em **Iniciar**, em **Ferramentas Administrativas** e em **Gerenciamento de Diretiva de Grupo**.  
  
2.  Duas vezes no nome da floresta, clique duas vezes **domínios**, clique duas vezes o nome do domínio no qual você deseja ingressar um computador, clique com botão direito **política de domínio padrão**e, em seguida, clique em  **Editar**.  
  
3.  Na árvore de console, clique duas vezes **configuração do computador**, clique duas vezes em **diretivas**, clique duas vezes em **as configurações do Windows**, clique duas vezes em **segurança As configurações**, clique duas vezes em **políticas locais**e, em seguida, clique duas vezes **atribuição de direitos de usuário**.  
  
4.  No painel de detalhes, clique duas vezes **Adicionar estações de trabalho ao domínio**.  
  
5.  Selecione o **definir estas configurações de política** caixa de seleção e, em seguida, clique em **adicionar usuário ou grupo**.  
  
6.  Digite o nome da conta que você deseja conceder os direitos de usuário para e, em seguida, clique em **Okey** duas vezes.  
  
## <a name="BKMK_ODKSxS"></a>Processo de ingresso no domínio offline  
Execute Djoin.exe em um prompt de comando elevado para provisionar os metadados da conta de computador. Quando você executar o comando de provisionamento, os metadados da conta de computador é criado em um arquivo binário que você especificar como parte do comando.  
  
Para obter mais informações sobre a função NetProvisionComputerAccount que é usada para provisionar a conta de computador durante uma junção de domínio offline, consulte [função NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426) (https://go.microsoft.com/fwlink/?LinkId=162426). Para obter mais informações sobre a função NetRequestOfflineDomainJoin que é executado localmente no computador de destino, consulte [função NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427) (https://go.microsoft.com/fwlink/?LinkId=162427).  
  
## <a name="BKMK_ODJSteps"></a>Etapas para executar uma junção de domínio offline do DirectAccess  
O processo de associação offline de domínio inclui as seguintes etapas:  
  
1.  Criar uma nova conta de computador para cada um dos clientes remotos e gerar um pacote de provisionamento usando o comando Djoin.exe de uma já ingressado no domínio computador na rede corporativa.  
  
2.  Adicione o computador cliente ao grupo de segurança DirectAccessClients  
  
3.  Transfira o pacote de provisionamento com segurança para computadores remotos (s) que serão ingressado no domínio.  
  
4.  Aplique o pacote de provisionamento e ingressar no domínio do cliente.  
  
5.  Reinicie o cliente para concluir o ingresso no domínio e estabelecer a conectividade.  
  
Há duas opções a considerar ao criar o pacote de provisionamento para o cliente. Se você usou o Assistente de Introdução para instalar o DirectAccess sem PKI, você deve usar a opção 1 abaixo. Se você usou o Assistente de instalação avançada ao instalar o DirectAccess com a PKI, você deve usar a opção 2 abaixo.  
  
Conclua as seguintes etapas para realizar o ingresso no domínio offline:  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Opção 1: Criar um pacote de provisionamento para o cliente sem PKI  
  
1.  Em um prompt de comando do servidor de acesso remoto, digite o seguinte comando para provisionar a conta de computador:  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Option2: Criar um pacote de provisionamento para o cliente com a PKI  
  
1.  Em um prompt de comando do servidor de acesso remoto, digite o seguinte comando para provisionar a conta de computador:  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>Adicione o computador cliente ao grupo de segurança DirectAccessClients  
  
1.  No controlador de domínio, do **inicie** tela, digite **Active** e selecione **Active Directory Users and Computers** do **aplicativos** tela .  
  
2.  Expanda a árvore sob o seu domínio e, em seguida, selecione a **usuários** contêiner.  
  
3.  No painel de detalhes, clique com botão direito **DirectAccessClients**e clique em **propriedades**.  
  
4.  Na guia **Membros** , clique em **Adicionar**.  
  
5.  Clique em **Tipos de Objeto**, selecione **Computadores** e clique em **OK**.  
  
6.  Digite o nome do cliente para adicionar e, em seguida, clique em **Okey**.  
  
7.  Clique em **Okey** para fechar o **DirectAccessClients** caixa de diálogo Propriedades e então feche **Active Directory Users and Computers**.  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>Copiar e, em seguida, aplicar o pacote de provisionamento para o computador cliente  
  
1.  Copie o pacote de provisionamento de c:\files\provision.txt no servidor de acesso remoto, onde ele foi salvo, a c:\provision\provision.txt no computador cliente.  
  
2.  No computador cliente, abra um prompt de comando com privilégios elevados e digite o comando a seguir para solicitar o ingresso no domínio:  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  Reinicialize o computador cliente. O computador será ingressado no domínio. Após a reinicialização, o cliente será associado ao domínio e ter conectividade com a rede corporativa com o DirectAccess.  
  
## <a name="see-also"></a>Consulte também  
[Função NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426)  
[Função NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427)  
  


