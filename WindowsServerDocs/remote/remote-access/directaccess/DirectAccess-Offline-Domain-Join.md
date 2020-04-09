---
title: Associação offline de domínio DirectAccess
description: Este guia explica as etapas para executar uma junção de domínio offline com o DirectAccess no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 55528736-6c19-40bd-99e8-5668169ef3c7
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 43f0c606f16af00797f0325b793d476e6c2892f8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815659"
---
# <a name="directaccess-offline-domain-join"></a>Associação offline de domínio DirectAccess

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este guia explica as etapas para executar uma junção de domínio offline com o DirectAccess. Durante um ingresso no domínio offline, um computador é configurado para ingressar em um domínio sem conexão física ou VPN.  
  
Este guia inclui as seguintes seções:  
  
- Visão geral do ingresso no domínio offline  
  
- Requisitos para ingresso no domínio offline
  
- Processo de ingresso no domínio offline
  
- Etapas para executar um ingresso no domínio offline  
  
## <a name="offline-domain-join-overview"></a>Visão geral do ingresso no domínio offline  
Introduzido no Windows Server 2008 R2, os controladores de domínio incluem um recurso chamado ingresso no domínio offline. Um utilitário de linha de comando chamado Djoin. exe permite que você Ingresse um computador em um domínio sem contatar fisicamente um controlador de domínio durante a conclusão da operação de ingresso no domínio. As etapas gerais para usar o Djoin. exe são:  
  
1.  Execute **Djoin/Provision** para criar os metadados da conta do computador. A saída desse comando é um arquivo. txt que inclui um blob codificado em base 64.  
  
2.  Execute **Djoin/requestodj** para inserir os metadados de conta de computador do arquivo. txt no diretório do Windows do computador de destino.  
  
3.  Reinicialize o computador de destino e o computador será ingressado no domínio.  
  
### <a name="offline-domain-join-with-directaccess-policies-scenario-overview"></a><a name="BKMK_ODJOverview"></a>Visão geral do cenário de ingresso offline no domínio com políticas do DirectAccess  
O ingresso no domínio offline do DirectAccess é um processo que os computadores que executam o Windows Server 2016, o Windows Server 2012, o Windows 10 e o Windows 8 podem usar o para ingressar em um domínio sem ser fisicamente ingressado na rede corporativa ou conectado por meio de VPN. Isso possibilita a adição de computadores a um domínio a partir de locais onde não há conectividade com uma rede corporativa. O ingresso no domínio offline para o DirectAccess fornece políticas do DirectAccess para os clientes para permitir o provisionamento remoto.  
  
Um ingresso no domínio cria uma conta de computador e estabelece uma relação de confiança entre um computador que executa um sistema operacional Windows e um domínio Active Directory.  
  
## <a name="prepare-for-offline-domain-join"></a><a name="BKMK_ODJRequirements"></a>Preparar para ingresso no domínio offline  
  
1.  Crie a conta da máquina.  
  
2.  Inventariar a associação de todos os grupos de segurança aos quais a conta do computador pertence.  
  
3.  Reúna os certificados de computador, as diretivas de grupo e os objetos de diretiva de grupo necessários a serem aplicados aos novos clientes.  
  
. As seções a seguir explicam os requisitos do sistema operacional e os requisitos de credencial para executar um ingresso no domínio offline do DirectAccess usando o Djoin. exe.  
  
### <a name="operating-system-requirements"></a>Requisitos de sistema operacional  
Você pode executar o Djoin. exe para o DirectAccess somente em computadores que executam o Windows Server 2016, o Windows Server 2012 ou o Windows 8. O computador no qual você executa o Djoin. exe para provisionar os dados da conta do computador em AD DS deve estar executando o Windows Server 2016, Windows 10, Windows Server 2012 ou Windows 8. O computador que você deseja ingressar no domínio também deve estar executando o Windows Server 2016, Windows 10, Windows Server 2012 ou Windows 8.  
  
### <a name="credential-requirements"></a>Requisitos de credenciais  
Para executar uma junção de domínio offline, você deve ter os direitos necessários para ingressar estações de trabalho no domínio. Os membros do grupo Admins. do domínio têm esses direitos por padrão. Se você não for um membro do grupo Admins. do domínio, um membro do grupo Admins. do domínio deverá concluir uma das seguintes ações para permitir que você ingresse estações de trabalho no domínio:  
  
-   Use Política de Grupo para conceder os direitos de usuário necessários. Esse método permite que você crie computadores no contêiner computadores padrão e em qualquer unidade organizacional (OU) que seja criada posteriormente (se nenhuma ACE (entrada de controle de acesso) Deny for adicionada).  
  
-   Edite a lista de controle de acesso (ACL) do contêiner computadores padrão para o domínio para delegar as permissões corretas para você.  
  
-   Crie uma UO e edite a ACL nessa UO para conceder a você a permissão **criar filho-permitir** . Passe o parâmetro **/machineou** para o comando **Djoin/Provision** .  
  
Os procedimentos a seguir mostram como conceder os direitos de usuário com Política de Grupo e como delegar as permissões corretas.  
  
#### <a name="granting-user-rights-to-join-workstations-to-the-domain"></a>Concedendo direitos de usuário para ingressar estações de trabalho no domínio  
Você pode usar o Console de Gerenciamento de Política de Grupo (GPMC) para modificar a política de domínio ou criar uma nova política com configurações que concedem aos direitos de usuário para adicionar estações de trabalho a um domínio.  
  
A associação em **Admins**. do domínio, ou equivalente, é o mínimo necessário para conceder direitos de usuário.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477) (https://go.microsoft.com/fwlink/?LinkId=83477).   
  
###### <a name="to-grant-rights-to-join-workstations-to-a-domain"></a>Para conceder direitos para ingressar estações de trabalho em um domínio  
  
1.  Clique em **Iniciar**, em **Ferramentas Administrativas** e em **Gerenciamento de Diretiva de Grupo**.  
  
2.  Clique duas vezes no nome da floresta, clique duas vezes em **domínios**, clique duas vezes no nome do domínio no qual você deseja ingressar em um computador, clique com o botão direito do mouse em **política de domínio padrão**e clique em **Editar**.  
  
3.  Na árvore de console, clique duas vezes em **configuração do computador**, clique duas vezes em **políticas**, clique duas vezes em **configurações do Windows**, clique duas vezes em **configurações de segurança**, clique duas vezes em **políticas locais**e clique duas vezes em **atribuição de direitos de usuário**.  
  
4.  No painel de detalhes, clique duas vezes em **Adicionar estações de trabalho ao domínio**.  
  
5.  Marque a caixa de seleção **definir estas configurações de política** e clique em **Adicionar usuário ou grupo**.  
  
6.  Digite o nome da conta à qual você deseja conceder os direitos de usuário e clique em **OK** duas vezes.  
  
## <a name="offline-domain-join-process"></a><a name="BKMK_ODKSxS"></a>Processo de ingresso no domínio offline  
Execute Djoin. exe em um prompt de comando elevado para provisionar os metadados da conta do computador. Quando você executa o comando de provisionamento, os metadados da conta de computador são criados em um arquivo binário que você especifica como parte do comando.  
  
Para obter mais informações sobre a função NetProvisionComputerAccount que é usada para provisionar a conta de computador durante uma junção de domínio offline, consulte a [função NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426) (https://go.microsoft.com/fwlink/?LinkId=162426). Para obter mais informações sobre a função NetRequestOfflineDomainJoin que é executada localmente no computador de destino, consulte [NetRequestOfflineDomainJoin function](https://go.microsoft.com/fwlink/?LinkId=162427) (https://go.microsoft.com/fwlink/?LinkId=162427).  
  
## <a name="steps-for-performing-a-directaccess-offline-domain-join"></a><a name="BKMK_ODJSteps"></a>Etapas para executar um ingresso no domínio offline do DirectAccess  
O processo de ingresso no domínio offline inclui as seguintes etapas:  
  
1.  Crie uma nova conta de computador para cada um dos clientes remotos e gere um pacote de provisionamento usando o comando Djoin. exe de um computador que já tenha ingressado no domínio na rede corporativa.  
  
2.  Adicionar o computador cliente ao grupo de segurança DirectAccessClients  
  
3.  Transfira o pacote de provisionamento com segurança para os computadores remotos que ingressarão no domínio.  
  
4.  Aplique o pacote de provisionamento e ingresse o cliente no domínio.  
  
5.  Reinicialize o cliente para concluir o ingresso no domínio e estabelecer a conectividade.  
  
Há duas opções a serem consideradas ao criar o pacote de provisionamento para o cliente. Se você usou o assistente de Introdução para instalar o DirectAccess sem PKI, use a opção 1 abaixo. Se você usou o assistente de instalação avançada para instalar o DirectAccess com PKI, use a opção 2 abaixo.  
  
Conclua as seguintes etapas para executar a junção de domínio offline:  
  
##### <a name="option1-create-a-provisioning-package-for-the-client-without-pki"></a>Opção 1: criar um pacote de provisionamento para o cliente sem PKI  
  
1.  Em um prompt de comando do seu servidor de acesso remoto, digite o seguinte comando para provisionar a conta do computador:  
  
    ```  
    Djoin /provision /domain <your domain name> /machine <remote machine name> /policynames DA Client GPO name /rootcacerts /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="option2-create-a-provisioning-package-for-the-client-with-pki"></a>Opção 2: criar um pacote de provisionamento para o cliente com PKI  
  
1.  Em um prompt de comando do seu servidor de acesso remoto, digite o seguinte comando para provisionar a conta do computador:  
  
    ```  
    Djoin /provision /machine <remote machine name> /domain <Your Domain name> /policynames <DA Client GPO name> /certtemplate <Name of client computer cert template> /savefile c:\files\provision.txt /reuse  
    ```  
  
##### <a name="add-the-client-computer-to-the-directaccessclients-security-group"></a>Adicionar o computador cliente ao grupo de segurança DirectAccessClients  
  
1.  Em seu controlador de domínio, na tela **inicial** , digite **ativo** e selecione **Active Directory tela usuários e computadores** da **aplicativo** .  
  
2.  Expanda a árvore em seu domínio e selecione o contêiner **usuários** .  
  
3.  No painel de detalhes, clique com o botão direito do mouse em **DirectAccessClients**e clique em **Propriedades**.  
  
4.  Na guia **Membros**, clique em **Adicionar**.  
  
5.  Clique em **Tipos de Objeto**, selecione **Computadores** e clique em **OK**.  
  
6.  Digite o nome do cliente a ser adicionado e clique em **OK**.  
  
7.  Clique em **OK** para fechar a caixa de diálogo Propriedades do **DirectAccessClients** e feche **Active Directory usuários e computadores**.  
  
##### <a name="copy-and-then-apply-the-provisioning-package-to-the-client-computer"></a>Copiar e, em seguida, aplicar o pacote de provisionamento ao computador cliente  
  
1.  Copie o pacote de provisionamento do c:\files\provision.txt no servidor de acesso remoto, onde ele foi salvo, para c:\provision\provision.txt no computador cliente.  
  
2.  No computador cliente, abra um prompt de comando com privilégios elevados e digite o seguinte comando para solicitar o ingresso no domínio:  
  
    ```  
    Djoin /requestodj /loadfile C:\provision\provision.txt /windowspath %windir% /localos  
    ```  
  
3.  Reinicialize o computador cliente. O computador será ingressado no domínio. Após a reinicialização, o cliente será ingressado no domínio e terá conectividade com a rede corporativa com o DirectAccess.  
  
## <a name="see-also"></a>Consulte também  
[Função NetProvisionComputerAccount](https://go.microsoft.com/fwlink/?LinkId=162426)  
[Função NetRequestOfflineDomainJoin](https://go.microsoft.com/fwlink/?LinkId=162427)  
  


