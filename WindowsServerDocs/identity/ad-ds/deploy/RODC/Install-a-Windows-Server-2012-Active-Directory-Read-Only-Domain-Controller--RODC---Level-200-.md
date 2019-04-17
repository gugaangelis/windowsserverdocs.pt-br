---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: "Instalar um controlador de domínio somente leitura do servidor 2012 Active Directory do Windows (RODC) (nível 200)"
description: "Este tópico explica como criar uma conta RODC preparada e, em seguida, anexe um servidor para essa conta durante acontecer. Este tópico também explica como instalar um RODC sem executar uma instalação em etapas."
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 78281ca845f79955aaa25aa45394284c59e639cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>Instalar um controlador de domínio somente leitura do servidor 2012 Active Directory do Windows (RODC) (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como criar uma conta RODC preparada e, em seguida, anexe um servidor para essa conta durante acontecer. Este tópico também explica como instalar um RODC sem executar uma instalação em etapas.  
  
## <a name="stage-rodc-workflow"></a>Fluxo de trabalho do estágio RODC  
Um preparados ler somente o domínio controlador (RODC) instalação funciona em duas fases distintas:  
  
1.  Preparo de uma conta de computador não ocupada  
  
2.  Anexando um RODC para essa conta durante a promoção  
  
O diagrama a seguir ilustra o processo de preparo do controlador de domínio do Active Directory domínio serviços somente leitura, em que você criar uma conta de computador RODC vazia no domínio usando o Centro Administrativo do Active Directory (Dsac.exe).  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="BKMK_StagePS"></a>Estágio RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**Bold** argumentos são necessários. *Em itálico* argumentos podem ser especificados usando o Windows PowerShell ou o Assistente de configuração do AD DS.)|  
|Adicionar addsreadonlydomaincontrolleraccount|-SkipPreChecks<br /><br />***-DomainControllerAccountName***<br /><br />***-DomainName***<br /><br />***-Nome do site***<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />***-Credenciais***<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />*-NoGlobalCatalog*<br /><br />*-InstallDNS*<br /><br />-ReplicationSourceDC|  
  
> [!NOTE]  
> O **-credenciais** argumento só é necessário se você não já efetuou logon como um membro do grupo Admins. do domínio.  
  
## <a name="attach-rodc-workflow"></a>Anexe o fluxo de trabalho RODC  
O diagrama a seguir ilustra o processo de configuração de serviços de domínio do Active Directory, em que você já instalou a função AD DS, você preparada conta RODC e iniciado **promover esse servidor para um controlador de domínio** usando o Gerenciador do servidor para criar um novo RODC em um domínio existente, anexá-lo para a conta de computador preparados.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="BKMK_AttachPS"></a>Anexar RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**Bold** argumentos são necessários. *Em itálico* argumentos podem ser especificados usando o Windows PowerShell ou o Assistente de configuração do AD DS.)|  
|Instalar AddsDomaincontroller|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-/ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credenciais***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />*-InstallationMediaPath*<br /><br />*-/LogPath*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />***-UseExistingAccount***|  
  
> [!NOTE]  
> O **-credenciais** argumento só é necessário se você não já efetuou logon como um membro do grupo Admins. do domínio.  
  
## <a name="staging"></a>Preparo  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
Executar a operação de preparo de uma conta de computador do controlador de domínio somente leitura, abrindo o Centro Administrativo do Active Directory (**Dsac.exe**). Clique no nome do domínio no painel de navegação. Clique duas vezes em **controladores de domínio** na lista de gerenciamento. Clique em **previamente criar uma conta de controlador de domínio somente leitura** no painel de tarefas.  
  
Para saber mais sobre o Centro Administrativo do Active Directory, consulte [avançado AD DS gerenciamento usando o Active Directory Centro Administrativo & #40; Nível 200 & #41; ](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md) e examine [Centro Administrativo do Active Directory: Introdução](https://technet.microsoft.com/library/dd560651(WS.10).aspx).  
  
Se você tem experiência na criação de controladores de domínio somente leitura, você descobrirá que o Assistente para instalação tem a mesma interface gráfica, como visto quando usando o snap-in computadores do Windows Server 2008 e mais antigos, o Active Directory Users e usa o mesmo código, que inclui exportar a configuração no formato de arquivo unattend usado pelo dcpromo obsoleto.  
  
Windows Server 2012 introduz um novo cmdlet ADDSDeployment para contas de computador RODC estágio, mas o assistente não usar o cmdlet para sua operação. As seções a seguir exibem o cmdlet equivalente e argumentos para tornar as informações associadas a cada mais fáceis de entender.  
  
O **previamente criar uma conta de controlador de domínio somente leitura** link no painel de tarefas do centro do Active Directory administrativa é equivalente ao cmdlet ADDSDeployment Windows PowerShell:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>Bem-vindo  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
O **bem-vindo ao Assistente de instalação dos serviços de domínio Active Directory** diálogo tem uma opção denominada **usar a instalação em modo avançado**. Selecione essa opção e clique em **próxima** para mostrar as opções de política de replicação de senha. Desmarque essa opção para usar os valores padrão para opções de política de replicação de senha (Isso é discutido em mais detalhes posteriormente nesta seção).  
  
### <a name="network-credentials"></a>Credenciais de rede  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
A opção de nome de domínio no **credenciais de rede** caixa de diálogo exibe o domínio visado pelo Centro Administrativo do Active Directory por padrão. Suas credenciais atuais são usadas por padrão. Se elas não incluem associação do grupo Admins. do domínio, clique em **Alternate Credentials**e clique em **definir** para fornecer o assistente com um nome de usuário e senha que seja um membro do grupo Administradores de domínio.  
  
O argumento ADDSDeployment Windows PowerShell equivalente é:  
  
```  
-credential <pscredential>  
```  
  
Tenha em mente que o sistema de preparo é uma porta direta do Windows Server 2008 R2 e não fornece a nova funcionalidade Adprep. Se você planeja implantar contas RODC preparadas, você deve primeiro implantar um RODC cancelou preparado nesse domínio para que a operação de rodcprep automática seja executada ou execute manualmente adprep.exe /rodcprep pela primeira vez.  
  
Caso contrário, você receberá erro "Não será capaz de instalar um controlador de domínio somente leitura neste domínio, pois"adprep /rodcprep"ainda não foi executada".  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>Especifique o nome do computador  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
O **especificar o nome do computador** diálogo exige que você insira o rótulo único **nome do computador** de um controlador de domínio que não existe. O controlador de domínio, configure e anexar mais tarde para essa conta deve ter o mesmo nome ou a operação de promoção não detecta a conta preparada.  
  
O argumento ADDSDeployment Windows PowerShell equivalente é:  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>Selecione um Site  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
O **selecionar um Site** caixa de diálogo mostra uma lista de sites do Active Directory para a floresta atual. A operação de controlador de domínio somente leitura preparados exige que você selecione um único site na lista. O RODC usa essas informações para criar seu objeto de configurações NTDS na partição de configuração e se associar ao site correto ao ser iniciado pela primeira vez após está sendo implantado.  
  
O argumento ADDSDeployment Windows PowerShell equivalente é:  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>Opções de controlador de domínio adicional  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
O **opções adicionais de controlador de domínio** caixa de diálogo permite que você especifique um controlador de domínio incluem execução como um **servidor DNS** e um **Catálogo Global**. A Microsoft recomenda que os controladores de domínio somente leitura prestam serviços DNS e GC, portanto, ambos são instalados por padrão; uma intenção da função RODC é cenários de filiais, onde a rede de longa distância pode não estar disponível e sem os DNS e os serviços de catálogo global, computadores na ramificação não será capazes de usar a funcionalidade e os recursos do AD DS.  
  
O **controlador de domínio somente leitura (RODC)** opção previamente é selecionada e não pode ser desabilitada. Os argumentos ADDSDeployment Windows PowerShell equivalente são:  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> Por padrão, o **- NoGlobalCatalog** valor é $false, que significa que o controlador de domínio será um servidor de catálogo global se o argumento não for especificado.  
  
### <a name="specify-the-password-replication-policy"></a>Especificar a política de replicação de senha  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
O **especificar a política de replicação de senha** caixa de diálogo permite que você modifique a lista padrão de contas que têm permissão para armazenar em cache as senhas no controlador de domínio somente leitura. Contas na lista configurado com **negar** ou que não estão em lista (implícita) não armazenar em cache sua senha. Contas que não são permitidas senhas em cache no RODC e não podem se conectar e se autenticar em um controlador de domínio gravável não podem acessar recursos ou funcionalidade fornecida pelo Active Directory.  
  
> [!IMPORTANT]  
> O assistente mostra essa caixa de diálogo somente se você selecionar o **usar a instalação do modo avançado** caixa de seleção na tela de boas-vindas. Se você desmarcar essa caixa de seleção, o assistente usa após os valores e grupos padrão:  
>   
> -   Os administradores - negar  
> -   Operadores do servidor - negar  
> -   Backup operadores - negar  
> -   Conta operadores - negar  
> -   Negar acesso negado grupo de replicação de Senha RODC-  
> -   Permitido grupo de replicação de Senha RODC - permitir  
  
Os argumentos ADDSDeployment Windows PowerShell equivalente são:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>Delegação de administração e acontecer  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
O **delegação de acontecer e a administração** caixa de diálogo permite que você configure um usuário ou grupo que contém os usuários que têm permissão para anexar o servidor para a conta de computador RODC. Clique em **definir** encontrar o domínio para um usuário ou grupo. O usuário ou grupo especificado nessa caixa de diálogo ganhos permissões administrativas locais no RODC. O usuário especificado ou membros do grupo especificado podem executar operações no RODC com privilégios equivalentes ao grupo de administradores do computador. Eles são *não* membros dos grupos de administradores internos do domínio ou Admins. do domínio.  
  
Use esta opção para delegar a administração do office de ramificação sem conceder a associação ao administrador de ramificação ao grupo Admins. do domínio. Delegação de administração do RODC não é necessária.  
  
O argumento ADDSDeployment Windows PowerShell equivalente é:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>Resumo  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
O **resumo** caixa de diálogo permite que você confirme suas configurações. Esta é a última oportunidade para parar a instalação antes do assistente cria a conta preparada. Clique em **próxima** quando você estiver pronto para criar a conta de computador RODC preparada.  Clique em **exportar configurações** para salvar um arquivo de resposta no formato de arquivo unattend dcpromo obsoletos.  
  
### <a name="creation"></a>Criação  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
O **Assistente de instalação dos serviços do Active Directory domínio** cria o controlador de domínio somente leitura preparados no Active Directory. Você não pode cancelar essa operação após ele ser iniciado.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
Use o seguinte cmdlet para testar uma conta de computador do controlador de domínio somente leitura usando o módulo ADDSDeployment Windows PowerShell:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
Consulte [do Windows PowerShell estágio RODC](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS) para argumentos necessários e opcionais.  
  
Porque **adicionar addsreadonlydomaincontrolleraccount** só tem uma ação com duas fases (pré-requisito verificação e instalação), as capturas de tela a seguir mostram a fase de instalação com os argumentos mínimos necessários.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
O estágio de operação RODC cria a conta de computador RODC no Active Directory. O Centro Administrativo do Active Directory mostra o **tipo de controlador de domínio** como um **conta de controlador de domínio não ocupada**. Esse tipos de controlador de domínio indica que a conta RODC preparada está pronta para um servidor para anexá-lo como um controlador de domínio somente leitura.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> O Centro Administrativo do Active Directory não é mais necessário para anexar um servidor para uma conta de computador do controlador de domínio somente leitura. Use o Gerenciador do servidor e o Assistente de configuração de serviços de domínio Active Directory ou o cmdlet de módulo ADDSDeployment Windows PowerShell **instalar AddsDomainController** para conectar um novo RODC à sua conta preparada. As etapas são semelhantes para adicionar um novo controlador de domínio gravável a um domínio existente, com a exceção de que a conta de computador RODC preparada contém decididas ao tempo preparados a conta de computador RODC as opções de configuração.  
  
## <a name="attaching"></a>Anexando  
  
### <a name="deployment-configuration"></a>Configuração da implantação  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Gerenciador do servidor começa cada promoção de controlador de domínio com o **implantação configuração** página. As opções restantes e campos obrigatórios alterar essa página e as páginas subsequentes, dependendo de qual operação de implantação que você selecionar.  
  
Para adicionar um controlador de domínio somente leitura a um domínio existente, selecione **adicionar um controlador de domínio a um domínio existente** e clique no **selecione** botão para **especificar as informações de domínio para este domínio**. Gerenciador do servidor automaticamente solicita credenciais válidas, ou você pode clicar em **alteração**.  
  
Anexar um RODC requer a associação as grupos de administradores de domínio no Windows Server 2012. O Assistente de configuração de serviços de domínio Active Directory solicita que você mais tarde se suas credenciais atuais não tiver as permissões adequadas ou membros do grupo.  
  
O **implantação configuração** cmdlet ADDSDeployment do Windows PowerShell e os argumentos são:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opções de controlador de domínio  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
O **opções de controlador de domínio** página mostra o domínio opções de controlador para o novo controlador de domínio. Quando esta página é carregada, o Assistente de configuração de serviços de domínio Active Directory envia uma consulta LDAP para um controlador de domínio existente para verificar se há contas não ocupadas. Se a consulta encontra um controlador de domínio não ocupada conta de computador que compartilha o mesmo nome que o computador atual, o assistente exibe uma mensagem informativa na parte superior da página que lê "**uma conta RODC pré-criada que corresponde ao nome do servidor de destino estiver no diretório. Escolha se deseja usar essa conta RODC existente ou reinstalar neste controlador de domínio**. " O assistente usa o **usar conta RODC existente** como a configuração padrão.  
  
> [!IMPORTANT]  
> Você pode usar o **reinstalar neste controlador de domínio** opção quando um controlador de domínio sofreu um problema físico e não pode retornar à funcionalidade. Isso poupa tempo ao configurar o controlador de domínio de substituição, deixando o controlador de domínio a conta do computador e metadados no Active Directory do objeto. Instalar o novo computador com o *mesmo nome*e promovê-lo como um controlador de domínio no domínio. O **reinstalar neste controlador de domínio** opção não estará disponível se você tiver removido metadados do objeto de controlador de domínio do Active Directory (Limpeza de metadados).  
  
Você não pode configurar opções de controlador de domínio quando você estiver anexando um servidor para uma conta de computador RODC. Configurar opções de controlador de domínio quando você cria a conta de computador RODC preparada.  
  
Especificado **senha do modo de restauração dos serviços de diretório** devem cumprir a política de senha aplicada ao servidor. Sempre escolha uma senha forte e complexa ou preferencialmente, uma senha.  
  
O **opções de controlador de domínio** ADDSDeployment Windows PowerShell argumentos são:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> O nome do site já deve existir quando fornecido como um argumento para **- nome do site**. O **instalar AddsDomainController** cmdlet não cria nomes de site. Você pode usar o cmdlet **novo adreplicationsite** para criar novos sites.  
  
O **instalar ADDSDomainController** argumentos siga os mesmos padrões como Gerenciador do servidor, se não for especificado.  
  
O **SafeModeAdministratorPassword** operação do argumento é especial:  
  
-   Se *não especificado* como um argumento, o cmdlet solicita que você insira e confirme a senha mascarada. Esse é o uso preferencial ao executar o cmdlet interativamente.  
  
    Por exemplo criar um novo RODC no corp.contoso.com e ser solicitado a inserir e confirmar uma senha mascarada:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
    ```  
  
-   Se especificado *com um valor*, o valor deve ser uma cadeia de caracteres segura. Isso não é o uso preferencial ao executar o cmdlet interativamente.  
  
Por exemplo, você pode manualmente solicitar uma senha usando o **Read-Host** cmdlet para solicitar ao usuário uma cadeia de caracteres segura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Como a opção anterior não confirme a senha, tenha muito cuidado: a senha não estiver visível.  
  
Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora isso é recomendado.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Por fim, você poderia armazenar a senha ofuscada em um arquivo e reutilizá-lo mais tarde, sem a senha de texto não criptografado nunca aparece. Por exemplo:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Fornecendo ou armazenar uma senha de texto clara ou ofuscados não é recomendado. Qualquer pessoa executando esse comando em um script ou observando sabe a senha DSRM do controlador de domínio.  Qualquer pessoa com acesso ao arquivo poderia inverter essa senha ofuscada. Com esse conhecimento, eles podem fazer logon em um controlador de domínio iniciado no DSRM e eventualmente representar o controlador de domínio em si, elevar seus privilégios de nível mais alto em uma floresta do AD. Um conjunto adicional de etapas usando **Cryptography** para criptografar o arquivo de texto dados são aconselhável, mas fora do escopo. A prática recomendada é evitar totalmente o armazenamento de senhas.  
  
### <a name="additional-options"></a>Opções adicionais  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
O **opções adicionais** página fornece opções de configuração para nomear um controlador de domínio como a origem de replicação, ou você pode usar qualquer controlador de domínio como a origem de replicação.  
  
Você também pode escolher instalar o controlador de domínio usando o backup de mídia usando a instalação da opção de mídia (IFM). O **instalar da mídia** checkbox fornece uma opção de procurar uma vez selecionada e você deve clicar **verificar** para garantir que o caminho fornecido é mídia válida. Mídia usada pela opção IFM é criada com o Backup do Windows Server ou Ntdsutil.exe de outro existente Windows Server 2012 computador Você não pode usar um Windows Server 2008 R2 ou o sistema operacional anterior para criar mídia para um controlador de domínio do Windows Server 2012. Para obter mais informações sobre alterações no IFM, consulte [Ntdsutil.exe instalar contra alterações de mídia](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Se usar mídia protegida com um SYSKEY, Gerenciador do servidor solicita senha da imagem durante a verificação.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
O **opções adicionais** ADDSDeployment cmdlet argumentos são:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Caminhos  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
O **caminhos** página permite substituir os locais de pastas padrão do AD DS banco de dados, os logs de transação de banco de dados, e o SYSVOL compartilhar. Os locais padrão estão sempre em subpastas da pasta % systemroot %. O **caminhos** ADDSDeployment cmdlet argumentos são:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Opções de revisão e exibir Script  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
O **opções de revisão** página permite que você validar as configurações e certifique-se de que atendam aos requisitos antes de iniciar a instalação. Isso não é a última oportunidade de parar a instalação usando o Gerenciador do servidor. Esta página simplesmente permite que você revise e confirme as configurações antes de continuar a configuração. O **opções de revisão** página no Gerenciador do servidor também oferece um recurso opcional **Exibir Script** botão para criar um arquivo de texto Unicode que contém a configuração ADDSDeployment atual como um único script do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de configuração de serviços de domínio Active Directory para configurar as opções, exportar a configuração e, em seguida, cancelá-lo. Esse processo cria um exemplo válido e sintaticamente correto para ainda mais a modificação ou uso direto. Por exemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "corp.contoso.com" `  
-LogPath "C:\Windows\NTDS" `  
-SYSVOLPath "C:\Windows\SYSVOL" `  
-UseExistingAccount:$true `  
-Norebootoncompletion:$false  
-Force:$true  
  
```  
  
> [!NOTE]  
> Gerenciador do servidor normalmente preenche todos os argumentos com valores ao promover e não dependem de padrões (como eles podem mudar entre as versões futuras do Windows ou service packs). A única exceção a isso é o **- safemodeadministratorpassword** argumento. Para forçar um prompt de confirmação omitir o valor ao executar o cmdlet interativamente  
  
Use opcional **Whatif** argumento com o **instalar ADDSDomainController** cmdlet para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos de um cmdlet.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>Seleção de pré-requisitos  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
O **pré-requisitos verificar** é um novo recurso na configuração de domínio do AD DS. Essa nova fase valida se a configuração do servidor é capaz de dar suporte a uma nova floresta do AD DS.  
  
Ao instalar um novo domínio raiz, o servidor Manager Assistente domínio Active Directory Services configuração invoca uma série de testes modulares serializados. Esses testes alertarão-lo com opções de reparo sugeridos. Você pode executar os testes quantas vezes for necessário. O processo de instalação de controlador de domínio não pode continuar até que todos os pré-requisitos testes passar.  
  
O **pré-requisitos verificar** superfícies também informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos. Para saber mais sobre as verificações de pré-requisito, consulte [pré-requisito verificando](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Você não pode ignorar a **Verificar pré-requisito** quando usar o Gerenciador do servidor, mas você pode ignorar o processo ao usar o cmdlet do AD DS implantação usando o argumento a seguir:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft desestimula ignorar a verificação de pré-requisitos como ele pode levar a uma promoção de controlador de domínio parcial ou danificado floresta do AD DS.  
  
Clique em **instalar** para iniciar o processo de promoção de controlador de domínio. Esta é a última oportunidade para cancelar a instalação. Você não pode cancelar o processo de promoção depois que ele é iniciado. O computador é reinicializado automaticamente no final da promoção, independentemente dos resultados da promoção.  
  
### <a name="installation"></a>Instalação  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
Quando a página de instalação exibe, a configuração de controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas exibem nesta página e são gravadas nos logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Para instalar uma nova floresta do Active Directory usando o módulo ADDSDeployment, use o seguinte cmdlet:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Consulte [do Windows PowerShell anexar RODC](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS) para argumentos necessários e opcionais.  
  
O **instalar addsdomaincontroller** cmdlet só tem duas fases (verificação de pré-requisitos e instalação). As duas figuras a seguir mostram a fase de instalação com os argumentos mínimos necessários de **- domainname**, **- useexistingaccount**, e **-credenciais**. Observe como, assim como Gerenciador do servidor, **instalar ADDSDomainController** lembra que promoção reinicializará o servidor automaticamente:  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
Para aceitar o prompt de reinicialização automaticamente, use o **-forçar** ou **-confirmar: $false** argumentos com qualquer cmdlet ADDSDeployment Windows PowerShell. Para impedir que o servidor reiniciar automaticamente no final da promoção, use o **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> Não é recomendado substituindo a reinicialização. Reinicialize o controlador de domínio para funcionar corretamente.  
  
### <a name="results"></a>Resultados  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
O **resultados** página mostra o sucesso ou fracasso de promoção e todas as informações administrativas importantes. O controlador de domínio é reinicializado automaticamente após 10 segundos.  
  
## <a name="rodc-without-staging-workflow"></a>RODC sem fluxo de trabalho de teste  
O diagrama a seguir ilustra o processo de configuração de serviços de domínio do Active Directory, quando você já tiver instalado a função AD DS e iniciar o assistente domínio Active Directory serviços de configuração usando o Gerenciador do servidor para criar um novo controlador de domínio somente leitura não testada em um domínio existente do Windows Server 2012.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>RODC sem preparo do Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**Bold** argumentos são necessários. *Em itálico* argumentos podem ser especificados usando o Windows PowerShell ou o Assistente de configuração do AD DS.)|  
|Instalar AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />***-Nome do site***<br /><br />*-/ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***-Credenciais***<br /><br />*-CriticalReplicationOnly*<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-DNSOnNetwork<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-/LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />***-ReadOnlyReplica***|  
  
> [!NOTE]  
> O **-credenciais** argumento só é necessário se você não já efetuou logon como um membro do grupo Admins. do domínio.  
  
## <a name="rodc-without-staging-deployment"></a>RODC sem preparo implantação  
  
### <a name="deployment-configuration"></a>Configuração da implantação  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
Gerenciador do servidor começa cada promoção de controlador de domínio com o **implantação configuração** página. As opções restantes e campos obrigatórios alterar essa página e as páginas subsequentes, dependendo de qual operação de implantação que você selecionar.  
  
Para adicionar um controlador de domínio somente leitura preparados cancelou a um domínio do Windows Server 2012 existente, selecione **adicionar um controlador de domínio a um domínio existente** e clique no **selecione** botão para **especificar as informações de domínio para este domínio**. Gerenciador do servidor automaticamente solicita credenciais válidas, ou você pode clicar em **alteração**.  
  
Anexar um RODC requer a associação as grupos de administradores de domínio no Windows Server 2012. O Assistente de configuração de serviços de domínio Active Directory solicita que você mais tarde se suas credenciais atuais não tiver as permissões adequadas ou membros do grupo.  
  
O **implantação configuração** cmdlet ADDSDeployment do Windows PowerShell e os argumentos são:  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opções de controlador de domínio  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
O **opções de controlador de domínio** página especifica as funcionalidades de controlador de domínio para o novo controlador de domínio. As funcionalidades de controlador de domínio configurável são **servidor DNS**, **Catálogo Global**, e **controlador de domínio somente leitura**. A Microsoft recomenda que todos os controladores de domínio fornecem serviços DNS e GC para alta disponibilidade em ambientes distribuídos. GC está sempre selecionada por padrão e o servidor DNS é selecionado por padrão, se os hosts atual do domínio DNS já em seus controladores de domínio com base na consulta de início de autoridade.  
  
O **opções de controlador de domínio** página também permite que você escolha Active Directory apropriados lógico **nome do site** da configuração do floresta. Por padrão, ele selecionará o site com a sub-rede mais correta. Se houver apenas um site, ele selecionará esse site automaticamente.  
  
> [!IMPORTANT]  
> Se o servidor não pertence a uma sub-rede do Active Directory e não há mais de um site do Active Directory, nada é selecionado e o **próxima** botão fica indisponível até que você escolha um site na lista.  
  
Especificado **senha do modo de restauração dos serviços de diretório** devem cumprir a política de senha aplicada ao servidor. Sempre escolha uma senha forte e complexa ou preferencialmente, uma senha. O **opções de controlador de domínio** ADDSDeployment Windows PowerShell argumentos são:  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> O nome do site já deve existir quando fornecido como um argumento para **- nome do site**. O **instalar AddsDomainController** cmdlet não cria nomes de site. Você pode usar o cmdlet **novo adreplicationsite** para criar novos sites.  
  
O **instalar ADDSDomainController** argumentos siga os mesmos padrões como Gerenciador do servidor, se não for especificado.  
  
O **SafeModeAdministratorPassword** operação do argumento é especial:  
  
-   Se *não especificado* como um argumento, o cmdlet solicita que você insira e confirme a senha mascarada. Esse é o uso preferencial ao executar o cmdlet interativamente.  
  
    Por exemplo criar um novo RODC no corp.contoso.com e ser solicitado a inserir e confirmar uma senha mascarada:  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
    ```  
  
-   Se especificado *com um valor*, o valor deve ser uma cadeia de caracteres segura. Isso não é o uso preferencial ao executar o cmdlet interativamente.  
  
Por exemplo, você pode manualmente solicitar uma senha usando o **Read-Host** cmdlet para solicitar ao usuário uma cadeia de caracteres segura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Como a opção anterior não confirme a senha, tenha muito cuidado: a senha não estiver visível.  
  
Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora isso é recomendado.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Por fim, você poderia armazenar a senha ofuscada em um arquivo e reutilizá-lo mais tarde, sem a senha de texto não criptografado nunca aparece. Por exemplo:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Fornecendo ou armazenar uma senha de texto clara ou ofuscados não é recomendado. Qualquer pessoa executando esse comando em um script ou observando sabe a senha DSRM do controlador de domínio.  Qualquer pessoa com acesso ao arquivo poderia inverter essa senha ofuscada. Com esse conhecimento, eles podem fazer logon em um controlador de domínio iniciado no DSRM e eventualmente representar o controlador de domínio em si, elevar seus privilégios de nível mais alto em uma floresta do AD. Um conjunto adicional de etapas usando **Cryptography** para criptografar o arquivo de texto dados são aconselhável, mas fora do escopo. A prática recomendada é evitar totalmente o armazenamento de senhas.  
  
### <a name="rodc-options"></a>Opções de RODC  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
O **RODC opções** página permite que você modifique as configurações:  
  
-   Conta de administrador delegado  
  
-   Contas que têm permissão para replicar senhas no RODC  
  
-   Contas que não têm permissão de replicação de senhas no RODC  
  
Contas de administrador delegado obtém permissões administrativas locais no RODC. Esses usuários podem operar com privilégios equivalentes ao grupo de administradores do computador local.  Eles não são membros do Admins. do domínio ou os grupos de administradores internos do domínio. Essa opção é útil para a delegação de administração do office ramificação sem divulgar permissões administrativas do domínio. Não é necessário configurar a delegação de administração.  
  
O argumento ADDSDeployment Windows PowerShell equivalente é:  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
Contas que não são permitidas senhas em cache no RODC e não podem se conectar e se autenticar em um controlador de domínio gravável não podem acessar recursos ou funcionalidade fornecida pelo Active Directory.  
  
> [!IMPORTANT]  
> Se não modificado, os grupos padrão e as configurações são usadas:  
>   
> -   Os administradores - negar  
> -   Operadores do servidor - negar  
> -   Backup operadores - negar  
> -   Conta operadores - negar  
> -   Negar acesso negado grupo de replicação de Senha RODC-  
> -   Permitido grupo de replicação de Senha RODC - permitir  
  
Os argumentos ADDSDeployment Windows PowerShell equivalente são:  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>Opções adicionais  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
O **opções adicionais** página fornece opções de configuração para nomear um controlador de domínio como a origem de replicação, ou você pode usar qualquer controlador de domínio como a origem de replicação.  
  
Você também pode escolher instalar o controlador de domínio usando o backup de mídia usando a instalação da opção de mídia (IFM). O **instalar da mídia** checkbox fornece uma opção de procurar uma vez selecionada e você deve clicar **verificar** para garantir que o caminho fornecido é mídia válida. Mídia usada pela opção IFM é criada com o Backup do Windows Server ou Ntdsutil.exe de outro existente Windows Server 2012 computador Você não pode usar um Windows Server 2008 R2 ou o sistema operacional anterior para criar mídia para um controlador de domínio do Windows Server 2012.  Apêndices fornece mais informações sobre alterações no IFM. Se usar mídia protegida com um SYSKEY, Gerenciador do servidor solicita senha da imagem durante a verificação.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
Os argumentos do cmdlet ADDSDeployment de opções adicionais são:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>Caminhos  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
O **caminhos** página permite substituir os locais de pastas padrão do AD DS banco de dados, os logs de transação de banco de dados, e o SYSVOL compartilhar. Os locais padrão estão sempre em subpastas da pasta % systemroot %. O **caminhos** ADDSDeployment cmdlet argumentos são:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opções de preparação  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
O **opções de preparação** página alertando que a configuração do AD DS inclui estender o esquema (forestprep) e atualizar o domínio (domainprep). Esta página foi exibida apenas quando a floresta ou domínio não foi preparado pela instalação anterior de controlador de domínio do Windows Server 2012 ou executem Adprep.exe manualmente. Por exemplo, o Assistente de configuração de serviços de domínio Active Directory suprime nesta página, se você adicionar um novo controlador de domínio de réplica a um domínio de raiz de floresta existente do Windows Server 2012.  
  
Estender o esquema e atualizando o domínio não ocorrer quando você clica em **próxima**. Esses eventos ocorrem somente durante a fase de instalação. Esta página simplesmente traz reconhecimento sobre os eventos que ocorrerá posteriormente na instalação.  
  
Esta página também valida que as credenciais do usuário atual são membros dos grupos Administradores corporativos e da administração do esquema, conforme necessário associação nesses grupos para estender o esquema ou preparar um domínio. Clique em **alteração** para fornecer as credenciais do usuário adequado, se a página informa que as credenciais atuais não fornecer permissões suficientes.  
  
O argumento de cmdlet ADDSDeployment de opções adicionais é:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Como com versões anteriores do Windows Server, preparação de domínio automatizada do Windows Server 2012 não executa GPPREP. Executar **adprep.exe /gpprep** manualmente para todos os domínios que não foram preparados anteriormente para o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2. Você deve executar GPPrep apenas uma vez no histórico de um domínio, não com cada atualização. Adprep.exe não executa /gpprep automaticamente porque sua operação pode fazer com que todos os arquivos e pastas na pasta SYSVOL replicar novamente em todos os controladores de domínio.  
>   
> RODCPrep automático é executado quando você promove o primeiro RODC cancelou preparado em um domínio. Ele não ocorre quando você promove o primeiro controlador de domínio gravável do Windows Server 2012. Você também pode ainda executar manualmente **adprep.exe /rodcprep** se você planeja implantar controladores de domínio somente leitura.  
  
### <a name="review-options-and-view-script"></a>Opções de revisão e exibir Script  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
O **opções de revisão** página permite que você validar as configurações e certifique-se de que atendam aos requisitos antes de iniciar a instalação. Isso não é a última oportunidade de parar a instalação usando o Gerenciador do servidor. Esta página simplesmente permite que você revise e confirme as configurações antes de continuar a configuração.  
  
O **opções de revisão** página no Gerenciador do servidor também oferece um recurso opcional **Exibir Script** botão para criar um arquivo de texto Unicode que contém a configuração ADDSDeployment atual como um único script do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de configuração de serviços de domínio Active Directory para configurar as opções, exportar a configuração e, em seguida, cancelá-lo. Esse processo cria um exemplo válido e sintaticamente correto para ainda mais a modificação ou uso direto. Por exemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-AllowPasswordReplicationAccountName @("CORP\Allowed RODC Password Replication Group", "CORP\Chicago RODC Admins", "CORP\Chicago RODC Users and Computers") `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DelegatedAdministratorAccountName "CORP\Chicago RODC Admins" `  
-DenyPasswordReplicationAccountName @("BUILTIN\Administrators", "BUILTIN\Server Operators", "BUILTIN\Backup Operators", "BUILTIN\Account Operators", "CORP\Denied RODC Password Replication Group") `  
-DomainName "corp.contoso.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-ReadOnlyReplica:$true `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Gerenciador do servidor normalmente preenche todos os argumentos com valores ao promover e não dependem de padrões (como eles podem mudar entre as versões futuras do Windows ou service packs). A única exceção a isso é o **- safemodeadministratorpassword** argumento. Para forçar um prompt de confirmação, omita o valor ao executar o cmdlet interativamente.  
  
Use o argumento de Whatif opcional com o cmdlet Install-ADDSDomainController para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos de um cmdlet.  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>Seleção de pré-requisitos  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
O **pré-requisitos verificar** é um novo recurso na configuração de domínio do AD DS. Essa nova fase valida se a configuração do servidor é capaz de dar suporte a uma nova floresta do AD DS.  
  
Ao instalar um novo domínio raiz, o servidor Manager Assistente domínio Active Directory Services configuração invoca uma série de testes modulares serializados. Esses testes alertarão-lo com opções de reparo sugeridos. Você pode executar os testes quantas vezes for necessário. O processo de controlador de domínio não pode continuar até que todos os pré-requisitos testes passar.  
  
O **pré-requisitos verificar** superfícies também informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos.  
  
Você não pode ignorar a **Verificar pré-requisito** quando usar o Gerenciador do servidor, mas você pode ignorar o processo ao usar o cmdlet do AD DS implantação usando o argumento a seguir:  
  
```  
-skipprechecks  
  
```  
  
Clique em **instalar** para iniciar o processo de promoção de controlador de domínio. Esta é a última oportunidade para cancelar a instalação. Você não pode cancelar o processo de promoção depois que ele é iniciado. O computador é reinicializado automaticamente no final da promoção, independentemente dos resultados da promoção.  
  
### <a name="installation"></a>Instalação  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
Quando o **instalação** página exibe, a configuração de controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas exibem nesta página e são gravadas nos logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Para instalar uma nova floresta do Active Directory usando o módulo ADDSDeployment, use o seguinte cmdlet:  
  
```  
Install-addsdomaincontroller  
  
```  
  
Consulte o **ADDSDeployment Cmdlet** tabela na begininng desta seção para argumentos necessários e opcionais.  
  
O **instalar addsdomaincontroller** cmdlet só tem duas fases (verificação de pré-requisitos e instalação). As duas figuras a seguir mostram a fase de instalação com os argumentos mínimos necessários de **- domainname**, **- readonlyreplica**, **- nome do site**, e **-credenciais**. Observe como, assim como Gerenciador do servidor, **instalar ADDSDomainController** lembra que promoção reinicializará o servidor automaticamente:  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
Para aceitar o prompt de reinicialização automaticamente, use o **-forçar** ou **-confirmar: $false** argumentos com qualquer cmdlet ADDSDeployment Windows PowerShell. Para impedir que o servidor reiniciar automaticamente no final da promoção, use o **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> Substituindo a reinicialização não é recomendado. Reinicialize o controlador de domínio para funcionar corretamente. Se você desconectar o controlador de domínio, você não pode logon interativamente até ser reiniciado.  
  
### <a name="results"></a>Resultados  
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
O **resultados** página mostra o sucesso ou fracasso de promoção e todas as informações administrativas importantes. O controlador de domínio é reinicializado automaticamente após 10 segundos.  
  

