---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: Instalar um RODC (controlador de domínio somente leitura) do Active Directory do Windows Server 2012 (nível 200)
description: Este tópico explica como criar uma conta RODC pré-configurada e, em seguida, anexar um servidor a essa conta durante a instalação do RODC. Este tópico também explica como instalar um RODC sem executar uma instalação em etapas.
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5b3dbe22e5db03d76916218317d5e9ffe0640042
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519513"
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>Instalar um RODC (controlador de domínio somente leitura) do Active Directory do Windows Server 2012 (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como criar uma conta RODC pré-configurada e, em seguida, anexar um servidor a essa conta durante a instalação do RODC. Este tópico também explica como instalar um RODC sem executar uma instalação em etapas.

## <a name="stage-rodc-workflow"></a>Preparar o fluxo de trabalho do RODC
Uma instalação em etapas do RDOC (Controlador de Domínio Somente Leitura) funciona em duas fases distintas:

1.  Preparar uma conta de computador desocupada

2.  Anexar um RODC a essa conta durante a promoção

O diagrama a seguir ilustra o processo de preparo do Controlador de Domínio Somente Leitura dos Serviços de Domínio Active Directory, onde você pode criar uma conta de computador RODC vazia no domínio usando o Centro Administrativo do Active Directory (Dsac.exe).

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)

## <a name="stage-rodc-windows-powershell"></a><a name=BKMK_StagePS></a>Preparar o Windows PowerShell para o RODC

| Cmdlet ADDSDeployment | Argumentos (os argumentos em **Negrito** são necessários. Os argumentos em *Itálico* podem ser especificados usando o Windows PowerShell ou o Assistente de Configuração do AD DS.) |
|--|--|
| Add-addsreadonlydomaincontrolleraccount | -SkipPreChecks<p>***-DomainControllerAccountName***<p>***-Nome_do_domínio***<p>***-SiteName***<p>*-AllowPasswordReplicationAccountName*<p>***-Credential***<p>*-DelegatedAdministratorAccountName*<p>*-DenyPasswordReplicationAccountName*<p>*-NoGlobalCatalog*<p>*-InstallDNS*<p>-ReplicationSourceDC |

> [!NOTE]
> O argumento **-credential** só será necessário se você já não estiver registrado como membro do grupo Admins. do Domínio.

## <a name="attach-rodc-workflow"></a>Anexar o fluxo de trabalho do RODC
O diagrama abaixo ilustra o processo de configuração dos Serviços de Domínio Active Directory, onde você já instalou a função AD DS, preparou a conta RODC e começou a **Promover este Servidor a um Controlador de Domínio** usando o Gerenciador do Servidor para criar um novo RODC em um domínio existente, anexando-o à conta de preparo do computador.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)

## <a name="attach-rodc-windows-powershell"></a><a name=BKMK_AttachPS></a>Anexar o Windows PowerShell ao RODC

| Cmdlet ADDSDeployment | Argumentos (argumentos em**negrito** são necessários. Os argumentos em *Itálico* podem ser especificados usando o Windows PowerShell ou o Assistente de Configuração do AD DS.) |
|--|--|
| Install-AddsDomaincontroller | -SkipPreChecks<p>***-Nome_do_domínio***<p>*-safemodeadministratorpassword*<p>*-ApplicationPartitionsToReplicate*<p>*-CreateDNSDelegation*<p>***-Credential***<p>-CriticalReplicationOnly<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>*-InstallationMediaPath*<p>*-LogPath*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>*-SystemKey*<p>*-SYSVOLPath*<p>***-UseExistingAccount*** |

> [!NOTE]
> O argumento **-credential** só será necessário se você já não estiver registrado como membro do grupo Admins. do Domínio.

## <a name="staging"></a>Preparo
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)

Você executa a operação de preparo de uma conta de computador do controlador de domínio somente leitura abrindo o Centro Administrativo do Active Directory (**Dsac.exe**). Clique no nome do domínio no painel de navegação. Clique duas vezes em **Controladores de Domínio** na lista de gerenciamento. Clique em **Pré-criar uma conta do controlador de domínio somente leitura** no painel de tarefas.

Para obter mais informações sobre o Centro Administrativo do Active Directory, consulte [Gerenciamento de AD DS avançado usando Centro Administrativo do Active Directory &#40;nível 200&#41;](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md) e revisar [centro administrativo do Active Directory: introdução](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560651(v=ws.10)).

Se você tiver experiência na criação de controladores de domínio somente leitura, descobrirá que o assistente de instalação tem a mesma interface gráfica, como visto ao usar os snap-ins mais antigos de Usuários e Computadores do Active Directory do Windows Server 2008 e usa o mesmo código, que inclui exportação da configuração no formato de arquivo autônomo usado pelo antigo dcpromo.

O Windows Server 2012 introduz um novo cmdlet ADDSDeployment para preparar as contas de computador do RODC, mas o assistente não usa o cmdlet para seu funcionamento. As seções a seguir exibem o cmdlet e os argumentos equivalentes, a fim de tornar as informações associadas a cada um mais fáceis de entender.

O link **pré-criar uma conta do controlador de domínio somente leitura** no painel de tarefas do centro administrativo do Active Directory é equivalente ao cmdlet ADDSDeployment do Windows PowerShell:

```
Add-addsreadonlydomaincontrolleraccount

```

### <a name="welcome"></a>Bem-Vindo
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)

O diálogo **Bem-vindo ao Assistente de Instalação dos Serviços de Domínio Active Directory** tem uma opção chamada **Usar a instalação em modo avançado**. Selecione essa opção e clique em **Avançar** para mostrar as opções de política de replicação de senha. Desmarque essa opção para usar os valores padrão para as opções de política de replicação de senha (isso será discutido em mais detalhes mais adiante nesta seção).

### <a name="network-credentials"></a>Credenciais de rede
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)

A opção de nome de domínio no diálogo **Credenciais de Rede** exibe o domínio de destino do Centro Administrativo do Active Directory por padrão. Suas credenciais atuais são usadas ​​por padrão. Se elas não incluírem a associação ao grupo de administradores do domínio, clique em **Credenciais Alternativas** e em **Definir** para fornecer ao assistente um nome de usuário e uma senha que estejam associados a Admins. do Domínio.

O argumento equivalente de ADDSDeployment do Windows PowerShell é:

```
-credential <pscredential>
```

Lembre-se de que o sistema de preparo é uma porta direta do Windows Server 2008 R2 e não fornece a nova funcionalidade Adprep. Se você planeja implantar contas pré-configuradas do RODC, deve primeiro implantar um RODC não pré-configurada nesse domínio para que a operação rodcprep automática seja executada, ou execute primeiro manualmente o dprep.exe /rodcprep.

Caso contrário, você receberá um erro você não poderá instalar um controlador de domínio somente leitura nesse domínio, pois adprep/rodcprep ainda não foi executado.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)

### <a name="specify-the-computer-name"></a>Especificar o nome do computador
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)

O diálogo **Especificar o Nome do Computador** exige que você insira o **Nome do computador** de rótulo único de um controlador de domínio que não existe. O controlador de domínio que você configurar e anexar a essa conta posteriormente deve ter o mesmo nome, ou a operação de promoção não detectará a conta pré-configurada.

O argumento equivalente de ADDSDeployment do Windows PowerShell é:

```
-domaincontrolleraccountname <string>
```

### <a name="select-a-site"></a>Selecionar um site
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)

O diálogo **Selecionar um Site** mostra uma lista de sites do Active Directory para a floresta atual. A operação de pré-configuração do controlador de domínio somente leitura requer que você selecione um único site na lista. O RODC usa essas informações para criar o objeto Configurações NTDS na partição de configuração e juntar-se ao site correto na primeira inicialização após ser implantado.

O argumento equivalente de ADDSDeployment do Windows PowerShell é:

```
-sitename <string>
```

### <a name="additional-domain-controller-options"></a>Opções do controlador de domínio adicionais
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)

O diálogo **Opções Adicionais do Controlador de Domínio** permite que você especifique que um controlador de domínio inclua execução como um **Servidor DNS** e um **Catálogo Global**. A Microsoft recomenda que os controladores de domínio somente leitura forneçam serviços DNS e GC, então, ambos são instalados por padrão. Uma intenção da função RODC são os cenários de filiais, onde a rede de longa distância pode não estar disponível, e sem esses DNS e os serviços de catálogo global, os computadores na filial não serão capazes de usar os recursos e as funcionalidades do AD DS.

A opção **RODC (Controlador de domínio somente leitura)** é pré-selecionada e não pode ser desabilitada. Os argumentos equivalentes de ADDSDeployment do Windows PowerShell são:

```
-installdns <string>
-NoGlobalCatalog <{$true | $false}>

```

> [!NOTE]
> Por padrão, o valor **-NoGlobalCatalog** é $false, o que significa que o controlador de domínio será um servidor de catálogo global se o argumento não for especificado.

### <a name="specify-the-password-replication-policy"></a>Especificar a política de replicação de senha
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)

O diálogo **Especificar a Política de Replicação de Senha** permite que você modifique a lista padrão de contas que têm permissão para armazenar em cache as senhas neste controlador de domínio somente leitura. As contas na lista configuradas com **Negar** ou que não estejam na lista (implícita) não armazenam senha em cache. As contas sem permissão para armazenar em cache as senhas no RODC, e que não podem se conectar e autenticar em um controlador de domínio gravável, não podem acessar recursos ou funcionalidades fornecidas pelo Active Directory.

> [!IMPORTANT]
> O assistente só mostrará esse diálogo se você marcar a caixa de seleção **Usar a instalação em modo avançado** na tela de boas-vindas. Se você desmarcar essa caixa de seleção, o assistente usará os seguintes grupos e valores padrão:
>
> -   Administradores - Negar
> -   Operadores do servidor - Negar
> -   Operadores de backup - Negar
> -   Operadores de conta - Negar
> -   Grupo de replicação de senha RDOC negado - Negar
> -   Grupo de replicação de senha RDOC permitido - Permitir

Os argumentos equivalentes de ADDSDeployment do Windows PowerShell são:

```
-allowpasswordreplicationaccountname <string []>
-denypasswordreplicationaccountname <string []>
```

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)

### <a name="delegation-of-rodc-installation-and-administration"></a>Delegação de instalação e administração do RODC
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)

O diálogo **Delegação de Instalação e Administração do RODC** permite configurar um usuário ou grupo que contém os usuários com permissão para anexar o servidor à conta de computador do RODC. Clique em **Definir** para procurar o domínio para um usuário ou grupo. O usuário ou grupo especificado neste diálogo ganha permissões administrativas locais para o RODC. O usuário especificado ou os membros do grupo especificado podem executar operações no RODC com privilégios equivalentes ao grupo de administradores do computador. Eles *não* são membros dos grupos Admins. do Domínio ou Administradores internos de domínio.

Use esta opção para delegar a administração da filial, sem conceder a associação de administrador de filial ao grupo Admins. do Domínio. A delegação de administração do RODC não é necessária.

O argumento equivalente de ADDSDeployment do Windows PowerShell é:

```
-delegatedadministratoraccountname <string>
```

### <a name="summary"></a>Resumo
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)

O diálogo **Resumo** permite que você confirme suas configurações. Esta é a última oportunidade para interromper a instalação antes de o assistente criar a conta pré-configurada. Clique em **Avançar** quando estiver pronto para criar a conta pré-configurada de computador do RODC.  Clique em **Exportar Configurações** para salvar um arquivo de resposta no formato de arquivo autônomo dcpromo obsoleto.

### <a name="creation"></a>Criação
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)

O **Assistente de Instalação dos Serviços de Domínio Active Directory** cria o controlador de domínio somente leitura pré-configurado no Active Directory. Não é possível cancelar essa operação depois de iniciada.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)

Use o seguinte cmdlet para preparar uma conta de computador de controlador de domínio somente leitura usando o módulo ADDSDeployment do Windows PowerShell:

```
Add-addsreadonlydomaincontrolleraccount

```

Consulte [Preparar o Windows PowerShell para o RODC](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS) para obter os argumentos opcionais e obrigatórios.

Como o **Add-addsreadonlydomaincontrolleraccount** só tem uma ação com duas fases (verificação e instalação de pré-requisito), as seguintes telas mostram a fase de instalação com os argumentos mínimos necessários.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)

A operação de preparação do RODC cria a conta de computador do RODC no Active Directory. O Centro Administrativo do Active Directory mostra o **Tipo de Controlador de Domínio** com uma **Conta de Controlador de Domínio Desocupada**. Este tipo de controlador de domínio indica que a conta RODC preparada está pronta para ser anexada a um servidor como um controlador de domínio somente leitura.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)

> [!IMPORTANT]
> O Centro Administrativo do Active Directory não é mais necessário para anexar um servidor a uma conta de computador do controlador de domínio somente leitura. Use o Gerenciador do Servidor e o Assistente de Configuração dos Serviços de Domínio Active Directory ou o cmdlet do módulo ADDSDeployment do Windows PowerShell **Install-AddsDomainController** para anexar um novo RODC à conta pré-configurada. As etapas são semelhantes a adicionar um novo controlador de domínio gravável a um domínio existente, com a exceção de que a conta de computador pré-configurada do RODC contém opções de configuração decididas no momento em que você preparou a conta de computador do RODC.

## <a name="attaching"></a>anexação

### <a name="deployment-configuration"></a>Configuração de Implantação
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)

O Gerenciador do Servidor começa toda a promoção do controlador de domínio com a página **Configuração de Implantação**. As demais opções e campos exigidos mudam nessa página e nas páginas subsequentes, dependendo da operação de implantação selecionada.

Para adicionar um controlador de domínio somente leitura a um domínio existente, selecione **Adicionar um controlador de domínio a um domínio existente** e clique no botão **Selecionar** para **Especificar as informações de domínio para este domínio**. O Gerenciador do Servidor solicita automaticamente as credenciais válidas, ou você pode clicar em **Alterar**.

A anexação de um RODC exige a associação aos grupos Admins. do Domínio no Windows Server 2012. O Assistente de Configuração dos Serviços de Domínio Active Directory avisará você se as suas credenciais não tiverem as permissões ou as associações de grupo adequadas.

Os cmdlet e os argumentos do ADDSDeployment do Windows PowerShell na **Configuração de Implantação** são:

```
Install-AddsDomainController
-domainname <string>
-credential <pscredential>
```

### <a name="domain-controller-options"></a>Opções de Controlador de Domínio
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)

A página **Opções do Controlador de Domínio** mostra as opções para o novo controlador de domínio. Quando esta página é carregada, o Assistente de Configuração de Serviços de Domínio Active Directory envia uma consulta LDAP para um controlador de domínio existente para verificar se há contas desocupadas. Se a consulta encontrar uma conta de computador de controlador de domínio desocupada que compartilha o mesmo nome que o computador atual, o assistente exibirá uma mensagem informativa na parte superior da página que lê **uma conta RODC pré-criada que corresponde ao nome do servidor de destino existente no diretório. Escolha se deseja usar essa conta do RODC existente ou reinstalar este controlador de domínio**. O assistente usa a opção **Usar conta de RODC existente** como configuração padrão.

> [!IMPORTANT]
> Você pode usar a opção **Reinstalar este controlador de domínio** quando um controlador de domínio tiver sofrido um problema físico e não puder voltar à funcionalidade. Isso economiza tempo ao configurar o controlador de domínio de substituição, deixando a conta de computador do controlador de domínio e os metadados do objeto no Active Directory. Instale o novo computador com o *mesmo nome*, e promova-o como controlador de domínio no domínio. A opção **reinstalar este controlador de domínio** não estará disponível se você tiver removido os metadados do objeto do controlador de domínio de Active Directory (limpeza de metadados).

Não é possível configurar opções do controlador de domínio quando você está anexando um servidor a uma conta de computador do RODC. Você pode configurar as opções do controlador de domínio ao criar a conta de computador do RODC pré-configurada.

A **Senha do Modo de Restauração dos Serviços de Diretório** deve aderir à política de senha aplicada ao servidor. Escolha sempre uma senha forte e complexa.

Os argumentos de ADDSDeployment do Windows PowerShell nas **Opções do Controlador de Domínio** são:

```
-UseExistingAccount <{$true | $false}>
-SafeModeAdministratorPassword <secure string>
```

> [!IMPORTANT]
> O nome do site já deve existir quando fornecido como um argumento para **-sitename**. O cmdlet **install-AddsDomainController** não cria nomes de site. Você pode usar o cmdlet **new-adreplicationsite** para criar novos sites.

Os argumentos **Install-ADDSDomainController** seguirão os mesmos padrões do Gerenciador do Servidor, se não especificado.

A operação do argumento **SafeModeAdministratorPassword** é especial:

-   Se *nenhum argumento for especificado*, o cmdlet solicitará que você insira e confirme uma senha mascarada. Este é o uso preferencial ao executar o cmdlet interativamente.

    Por exemplo, para criar um novo RODC no corp.contoso.com e ser solicitado a digitar e confirmar uma senha mascarada:

    ```
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)
    ```

-   Se especificado *com um valor*, esse valor deverá ser uma cadeia de caracteres segura. Este não é o uso preferencial ao executar o cmdlet interativamente.

Por exemplo, você pode solicitar manualmente uma senha usando o cmdlet **Read-Host** para solicitar ao usuário uma cadeia de caracteres segura:

```
-safemodeadministratorpassword (read-host -prompt Password: -assecurestring)

```

> [!WARNING]
> Como a opção anterior não confirma a senha, seja extremamente cuidadoso: a senha não fica visível.

Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora seja altamente recomendável não fazer isso.

```
-safemodeadministratorpassword (convertto-securestring Password1 -asplaintext -force)
```

Finalmente, você pode armazenar a senha ofuscada em um arquivo e depois reutilizá-la mais tarde, sem a senha com texto não criptografado aparecendo. Por exemplo:

```
$file = c:\pw.txt
$pw = read-host -prompt Password: -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> O fornecimento ou o armazenamento de uma senha com texto não criptografado ou ofuscado não é recomendável. Qualquer pessoa que executar esse comando em um script ou que estiver por perto tomará conhecimento da senha DSRM desse controlador de domínio.  Qualquer pessoa com acesso ao arquivo pode reverter essa senha ofuscada. Com esse conhecimento, ela pode fazer logon em um DC iniciado em DSRM e, eventualmente, representar o próprio controlador de domínio, elevando seus privilégios ao nível mais alto em uma floresta AD. Um conjunto adicional de etapas, usando **System.Security.Cryptography** para criptografar os dados do arquivo de texto é aconselhável, mas fora do escopo. A melhor prática é evitar totalmente o armazenamento de senha.

### <a name="additional-options"></a>Opções adicionais
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)

A página **Opções Adicionais** fornece opções de configuração para nomear um controlador de domínio como origem de replicação, ou você pode usar qualquer controlador de domínio como origem de replicação.

Também é possível optar pela instalação do controlador de domínio usando uma mídia de backup e escolhendo a opção Instalar da mídia (IFM). A caixa de seleção **Instalar da mídia** fornece uma opção de navegação quando selecionada e você deve clicar em **Verificar** para garantir que o caminho fornecido é uma mídia válida.

Diretrizes para a fonte IFM:
*    A mídia usada pela opção IFM é criada com Backup do Windows Server ou Ntdsutil.exe de outro controlador de domínio do Windows Server existente com a mesma versão do sistema operacional somente. Por exemplo, você não pode usar um sistema operacional Windows Server 2008 R2 ou anterior para criar mídia para um controlador de domínio do Windows Server 2012.
*    Os dados de origem IFM devem ser de um controlador de domínio gravável. Embora uma fonte do RODC funcione tecnicamente para criar um novo RODC, há avisos de replicação falsos positivos que o RODC de origem IFM não está replicando.

Para mais informações sobre alterações na IFM, consulte [Instalação do Ntdsutil.exe de alterações de mídia](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Se a mídia for protegida por SYSKEY, o Gerenciador do Servidor solicitará a senha da imagem durante a verificação.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)

Os argumentos do cmdlet ADDSDeployment em **Opções Adicionais** são:

```
-replicationsourcedc <string>
-installationmediapath <string>
-systemkey <secure string>
```

### <a name="paths"></a>Caminhos
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)

A página **Rotas** permite substituir os locais de pasta padrão do banco de dados AD DS, os logs de transação de banco de dados e o compartilhamento SYSVOL. Os locais padrão estão sempre em subdiretórios do %systemroot%. Os argumentos de cmdlet do ADDSDeployment **Caminhos** são:

```
-databasepath <string>
-logpath <string>
-sysvolpath <string>
```

### <a name="review-options-and-view-script"></a>Examinar opções e exibir script
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)

A página **Opções de revisão** permite que você valide suas configurações e se assegure de que elas cumprem os requisitos, antes de iniciar a instalação. Esta não é a última oportunidade de interromper a instalação usando o Gerenciador do Servidor. Essa página simplesmente permite que você examine e confirme suas configurações antes de continuar. A página **Opções de revisão** do Gerenciador do Servidor também oferece um botão **Exibir Script** para criar um arquivo de texto Unicode contendo a configuração atual de ADDSDeployment como um script simples do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do Servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de Configuração dos Serviços de Domínio do Active Directory para configurar opções, exportar a configuração e então cancelar o assistente. Esse processo cria um exemplo válido e sintaticamente correto para modificações adicionais ou uso direto. Por exemplo:

```
#
# Windows PowerShell Script for AD DS Deployment
#

Import-Module ADDSDeployment
Install-ADDSDomainController `
-Credential (Get-Credential) `
-CriticalReplicationOnly:$false `
-DatabasePath C:\Windows\NTDS `
-DomainName corp.contoso.com `
-LogPath C:\Windows\NTDS `
-SYSVOLPath C:\Windows\SYSVOL `
-UseExistingAccount:$true `
-Norebootoncompletion:$false
-Force:$true

```

> [!NOTE]
> O Gerenciador do Servidor geralmente preenche todos os argumentos com valores quando promove e não depende de padrões (já que eles podem ser alterados entre versões futuras do Windows ou service packs). A única exceção a isso é o argumento **-safemodeadministratorpassword**. Para forçar um pedido de confirmação, omita o valor ao executar o cmdlet interativamente

Use o argumento opcional **Whatif** com o cmdlet **Install-ADDSDomainController** para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos de um cmdlet.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)

### <a name="prerequisites-check"></a>Verificação de pré-requisitos
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)

A **Verificação de Pré-requisitos** é um novo recurso na configuração do domínio AD DS. Essa nova fase confirma que a configuração do servidor é capaz de dar suporte a uma nova floresta AD DS.

Ao instalar um novo domínio raiz da floresta, o Assistente de Configuração dos Serviços de Domínio do Active Directory invoca diversos testes modulares serializados. Esses testes o alertam com opções de reparo sugeridas. Você pode executar os testes quantas vezes forem necessárias. O processo de instalação do controlador de domínio não pode continuar até que todos os testes de pré-requisitos sejam feitos.

A **Verificação de Pré-requisitos** também dá superfície a informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos. Para mais informações sobre as verificações de pré-requisitos, consulte [Verificação de pré-requisito](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).

Não é possível ignorar a **Verificação de Pré-requisitos** ao usar o Gerenciador do Servidor, mas você pode ignorar o processo ao usar o cmdlet de Implantação do AD DS com o seguinte argumento:

```
-skipprechecks

```

> [!WARNING]
> A Microsoft desencoraja ignorar a verificação de pré-requisito, pois isso pode levar a uma promoção parcial do controlador de domínio ou danificar a floresta AD DS.

Clique em **Instalar** para começar o processo de promoção do controlador de domínio. Esta é a última oportunidade de cancelar a instalação. Não é possível cancelar o processo de promoção uma vez que ele é iniciado. O computador reiniciará automaticamente no final da promoção, independentemente dos resultados da promoção.

### <a name="installation"></a>Instalação
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)

Quando a página Instalação é exibida, a configuração do controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas são exibidas nesta página e gravadas em logs:

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

Para instalar uma nova floresta do Active Directory utilizando o módulo ADDSDeployment, use o seguinte cmdlet:

```
Install-addsdomaincontroller

```

Consulte [Anexar o Windows PowerShell ao RODC](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS) para argumentos opcionais e obrigatórios.

O cmdlet **Install-addsdomaincontroller** possui somente duas fases (verificação de pré-requisitos e instalação). As duas figuras abaixo mostram a fase de instalação com os argumentos mínimos exigidos de **-domainname**, **-useexistingaccount**, e **-credential**. Observe que, assim como o Gerenciador do Servidor, o **Install-ADDSDomainController** informa que a promoção reiniciará o servidor automaticamente:

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)

Para aceitar o prompt de reinicialização automática, use os argumentos **-force** ou **-confirm:$false** com qualquer cmdlet ADDSDeployment do Windows PowerShell. Para evitar que o servidor reinicie automaticamente no final da promoção, use o argumento **-norebootoncompletion**.

> [!WARNING]
> Não é recomendável substituir a reinicialização. O controlador de domínio deve reiniciar para funcionar corretamente.

### <a name="results"></a>Resultados
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)

A página **Resultados** mostra o sucesso ou o fracasso da promoção e qualquer informação administrativa importante. O controlador de domínio reiniciará automaticamente após 10 segundos.

## <a name="rodc-without-staging-workflow"></a>RODC sem fluxo de trabalho de preparo
O diagrama a seguir ilustra o processo de configuração dos Serviços de Domínio Active Directory, quando você instalou anteriormente a função AD DS e iniciou o Assistente de Configuração de Serviços de Domínio Active Directory usando o Gerenciador do Servidor para criar um novo controlador de domínio somente leitura sem pré-configuração em um domínio Windows Server 2012 existente.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)

## <a name="rodc-without-staging-windows-powershell"></a>RODC sem o Windows PowerShell de pré-configurado

| Cmdlet ADDSDeployment | Argumentos (argumentos em**negrito** são necessários. Os argumentos em *Itálico* podem ser especificados usando o Windows PowerShell ou o Assistente de Configuração do AD DS.) |
|--|--|
| Install-AddsDomainController | -SkipPreChecks<p>***-Nome_do_domínio***<p>*-safemodeadministratorpassword*<p>***-SiteName***<p>*-ApplicationPartitionsToReplicate*<p>*-CreateDNSDelegation*<p>***-Credential***<p>*-CriticalReplicationOnly*<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>-DNSOnNetwork<p>*-InstallationMediaPath*<p>*-InstallDNS*<p>*-LogPath*<p>-MoveInfrastructureOperationMasterRoleIfNecessary<p>*-NoGlobalCatalog*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>-SkipAutoConfigureDNS<p>*-SystemKey*<p>*-SYSVOLPath*<p>*-AllowPasswordReplicationAccountName*<p>*-DelegatedAdministratorAccountName*<p>*-DenyPasswordReplicationAccountName*<p>***-ReadOnlyReplica*** |

> [!NOTE]
> O argumento **-credential** só será necessário se você já não estiver registrado como membro do grupo Admins. do Domínio.

## <a name="rodc-without-staging-deployment"></a>RODC sem implantação de preparo

### <a name="deployment-configuration"></a>Configuração de Implantação
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)

O Gerenciador do Servidor começa toda a promoção do controlador de domínio com a página **Configuração de Implantação**. As demais opções e campos exigidos mudam nessa página e nas páginas subsequentes, dependendo da operação de implantação selecionada.

Para adicionar um controlador de domínio somente leitura sem pré-configuração a um domínio do Windows Server 2012 existente, selecione **Adicionar um controlador de domínio a um domínio existente** e clique no botão **Selecionar** para **Especificar as informações de domínio para este domínio**. O Gerenciador do Servidor solicita automaticamente as credenciais válidas, ou você pode clicar em **Alterar**.

A anexação de um RODC exige a associação aos grupos Admins. do Domínio no Windows Server 2012. O Assistente de Configuração dos Serviços de Domínio Active Directory avisará você se as suas credenciais não tiverem as permissões ou as associações de grupo adequadas.

Os cmdlet e os argumentos do ADDSDeployment do Windows PowerShell na **Configuração de Implantação** são:

```
Install-AddsDomainController
-domainname <string>
-credential <pscredential>
```

### <a name="domain-controller-options"></a>Opções de Controlador de Domínio
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)

A página **Opções do Controlador de Domínio** especifica os recursos de controlador de domínio para o novo controlador de domínio. Os recursos configuráveis do controlador de domínio incluem **Servidor DNS**, **Catálogo Global** e **Controlador de domínio somente leitura**. A Microsoft recomenda que todos os controladores de domínio forneçam serviços DNS e GC para alta disponibilidade em ambientes distribuídos. GC é sempre selecionado por padrão e o servidor DNS é selecionado por padrão se o domínio atual já hospedar o DNS em seus DCs com base na consulta Início de autoridade.

A página **Opções do Controlador de Domínio** também permite que você escolha o **nome de site** lógico do Active Directory, na configuração da floresta. Por padrão, o site é selecionado com a sub-rede mais correta. Se houver apenas um site, ele será selecionado automaticamente.

> [!IMPORTANT]
> Se o servidor não pertencer a uma sub-rede do Active Directory e houver mais de um site do Active Directory, nada será selecionado e o botão **Avançar** ficará indisponível até você escolher um site na lista.

A **Senha do Modo de Restauração dos Serviços de Diretório** deve aderir à política de senha aplicada ao servidor. Escolha sempre uma senha forte, complexa ou, de preferência, uma frase secreta. Os argumentos de ADDSDeployment do Windows PowerShell **Opções do Controlador de Domínio** são:

```
-UseExistingAccount <{$true | $false}>
-SafeModeAdministratorPassword <secure string>
```

> [!IMPORTANT]
> O nome do site já deve existir quando fornecido como um argumento para **-sitename**. O cmdlet **install-AddsDomainController** não cria nomes de site. Você pode usar o cmdlet **new-adreplicationsite** para criar novos sites.

Os argumentos **Install-ADDSDomainController** seguirão os mesmos padrões do Gerenciador do Servidor, se não especificado.

A operação do argumento **SafeModeAdministratorPassword** é especial:

-   Se *nenhum argumento for especificado*, o cmdlet solicitará que você insira e confirme uma senha mascarada. Este é o uso preferencial ao executar o cmdlet interativamente.

    Por exemplo, para criar um novo RODC no corp.contoso.com e ser solicitado a digitar e confirmar uma senha mascarada:

    ```
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)
    ```

-   Se especificado *com um valor*, esse valor deverá ser uma cadeia de caracteres segura. Este não é o uso preferencial ao executar o cmdlet interativamente.

Por exemplo, você pode solicitar manualmente uma senha usando o cmdlet **Read-Host** para solicitar ao usuário uma cadeia de caracteres segura:

```
-safemodeadministratorpassword (read-host -prompt Password: -assecurestring)

```

> [!WARNING]
> Como a opção anterior não confirma a senha, seja extremamente cuidadoso: a senha não fica visível.

Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora seja altamente recomendável não fazer isso.

```
-safemodeadministratorpassword (convertto-securestring Password1 -asplaintext -force)
```

Finalmente, você pode armazenar a senha ofuscada em um arquivo e depois reutilizá-la mais tarde, sem a senha com texto não criptografado aparecendo. Por exemplo:

```
$file = c:\pw.txt
$pw = read-host -prompt Password: -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> O fornecimento ou o armazenamento de uma senha com texto não criptografado ou ofuscado não é recomendável. Qualquer pessoa que executar esse comando em um script ou que estiver por perto tomará conhecimento da senha DSRM desse controlador de domínio.  Qualquer pessoa com acesso ao arquivo pode reverter essa senha ofuscada. Com esse conhecimento, ela pode fazer logon em um DC iniciado em DSRM e, eventualmente, representar o próprio controlador de domínio, elevando seus privilégios ao nível mais alto em uma floresta AD. Um conjunto adicional de etapas, usando **System.Security.Cryptography** para criptografar os dados do arquivo de texto é aconselhável, mas fora do escopo. A melhor prática é evitar totalmente o armazenamento de senha.

### <a name="rodc-options"></a>Opções de RODC
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)

A página **Opções de RODC** possibilita que você modifique as configurações:

-   Conta de administrador delegado

-   Contas que têm permissão para replicar senhas para o RODC

-   Contas que não têm permissão para replicar senhas para o RODC

As contas de administrador delegadas obtêm permissões administrativas locais para o RODC. Esses usuários podem operar com privilégios equivalentes ao grupo Administradores do computador local.  Eles não são membros do grupo Admins. do Domínio ou dos grupos Administradores embutidos no domínio. Essa opção é útil para a administração de filiais de delegação, mas sem emitir permissões administrativas de domínio. Não é preciso configurar a delegação de administração.

O argumento equivalente de ADDSDeployment do Windows PowerShell é:

```
-delegatedadministratoraccountname <string>
```

As contas sem permissão para armazenar em cache as senhas no RODC, e que não podem se conectar e autenticar em um controlador de domínio gravável, não podem acessar recursos ou funcionalidades fornecidas pelo Active Directory.

> [!IMPORTANT]
> Se não for modificado, os grupos e as configurações padrão são usados:
>
> -   Administradores - Negar
> -   Operadores do servidor - Negar
> -   Operadores de backup - Negar
> -   Operadores de conta - Negar
> -   Grupo de replicação de senha RDOC negado - Negar
> -   Grupo de replicação de senha RDOC permitido - Permitir

Os argumentos equivalentes de ADDSDeployment do Windows PowerShell são:

```
-allowpasswordreplicationaccountname <string []>
-denypasswordreplicationaccountname <string []>
```

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)

### <a name="additional-options"></a>Opções adicionais
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)

A página **Opções Adicionais** fornece opções de configuração para nomear um controlador de domínio como origem de replicação, ou você pode usar qualquer controlador de domínio como origem de replicação.

Também é possível optar pela instalação do controlador de domínio usando uma mídia de backup e escolhendo a opção Instalar da mídia (IFM). A caixa de seleção **Instalar da mídia** fornece uma opção de navegação quando selecionada e você deve clicar em **Verificar** para garantir que o caminho fornecido é uma mídia válida.

Diretrizes para a fonte IFM:
*    A mídia usada pela opção IFM é criada com Backup do Windows Server ou Ntdsutil.exe de outro controlador de domínio do Windows Server existente com a mesma versão do sistema operacional somente. Por exemplo, você não pode usar um sistema operacional Windows Server 2008 R2 ou anterior para criar mídia para um controlador de domínio do Windows Server 2012.
*    Os dados de origem IFM devem ser de um controlador de domínio gravável. Embora uma fonte do RODC funcione tecnicamente para criar um novo RODC, há avisos de replicação falsos positivos que o RODC de origem IFM não está replicando.

Para mais informações sobre alterações na IFM, consulte [Instalação do Ntdsutil.exe de alterações de mídia](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM). Se a mídia for protegida por SYSKEY, o Gerenciador do Servidor solicitará a senha da imagem durante a verificação.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)

Os argumentos de cmdlet do ADDSDeployment Opções Adicionais são:

```
-replicationsourcedc <string>
-installationmediapath <string>
-systemkey <secure string>
```

### <a name="paths"></a>Caminhos
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)

A página **Rotas** permite substituir os locais de pasta padrão do banco de dados AD DS, os logs de transação de banco de dados e o compartilhamento SYSVOL. Os locais padrão estão sempre em subdiretórios do %systemroot%. Os argumentos de cmdlet do ADDSDeployment **Caminhos** são:

```
-databasepath <string>
-logpath <string>
-sysvolpath <string>
```

### <a name="preparation-options"></a>Opções de preparação
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)

A página **Opções de Preparação** avisa que a configuração do AD DS inclui estender o esquema (forestprep) e atualizar o domínio (domainprep). Essa página só é vista quando a floresta ou o domínio não foi preparado pela instalação anterior do controlador de domínio do Windows Server 2012 anterior ou em uma execução manual de Adprep.exe. Por exemplo, o Assistente de Configuração dos Serviços de Domínio Active Directory suprimirá essa página se você adicionar um novo controlador de domínio de réplica a um domínio raiz da floresta do Windows Server 2012 existente.

A ampliação do esquema e a atualização do domínio não ocorrem quando você clica em **Avançar**. Esses eventos só ocorrem na fase de instalação. Esta página simplesmente faz conhecer os eventos que ocorrerão no final da instalação.

Esta página também confirma que as credenciais atuais do usuário estão associadas aos grupos de Administração de Esquema e Administradores de Empresa, já que você precisa estar associado a esses grupos para estender o esquema ou preparar um domínio. Clique em **Alterar** para fornecer credenciais de usuário adequadas, se a página informar que as credenciais atuais não oferecem permissões suficientes.

O argumento do cmdlet ADDSDeployment em Opções Adicionais é:

```
-adprepcredential <pscredential>
```

> [!IMPORTANT]
> Como nas versões anteriores do Windows Server, a preparação do domínio automatizado Windows Server 2012 não executa GPPREP. Execute **adprep.exe /gpprep** manualmente para todos os domínios que não foram previamente preparados para o Windows Server 2003, o Windows Server 2008 ou o Windows Server 2008 R2. Você deve executar GPPrep apenas uma vez no histórico de um domínio, não com cada atualização. O Adprep.exe não executa /gpprep automaticamente porque seu funcionamento pode fazer com que todos os arquivos e pastas na pasta SYSVOL sejam replicados em todos os controladores de domínio.
>
> O RODCPrep automático é executado quando você promove o primeiro RODC sem preparo em um domínio. Ele não ocorre quando você promove o primeiro controlador de domínio gravável do Windows Server 2012. Você também pode executar manualmente **adprep.exe /rodcprep** se planeja implantar controladores de domínio somente leitura.

### <a name="review-options-and-view-script"></a>Examinar opções e exibir script
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)

A página **Opções de revisão** permite que você valide suas configurações e se assegure de que elas cumprem os requisitos, antes de iniciar a instalação. Esta não é a última oportunidade de interromper a instalação usando o Gerenciador do Servidor. Essa página simplesmente permite que você examine e confirme suas configurações antes de continuar.

A página **Opções de revisão** do Gerenciador do Servidor também oferece um botão **Exibir Script** para criar um arquivo de texto Unicode contendo a configuração atual de ADDSDeployment como um script simples do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do Servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de Configuração dos Serviços de Domínio do Active Directory para configurar opções, exportar a configuração e então cancelar o assistente. Esse processo cria um exemplo válido e sintaticamente correto para modificações adicionais ou uso direto. Por exemplo:

```
#
# Windows PowerShell Script for AD DS Deployment
#

Import-Module ADDSDeployment
Install-ADDSDomainController `
-AllowPasswordReplicationAccountName @(CORP\Allowed RODC Password Replication Group, CORP\Chicago RODC Admins, CORP\Chicago RODC Users and Computers) `
-Credential (Get-Credential) `
-CriticalReplicationOnly:$false `
-DatabasePath C:\Windows\NTDS `
-DelegatedAdministratorAccountName CORP\Chicago RODC Admins `
-DenyPasswordReplicationAccountName @(BUILTIN\Administrators, BUILTIN\Server Operators, BUILTIN\Backup Operators, BUILTIN\Account Operators, CORP\Denied RODC Password Replication Group) `
-DomainName corp.contoso.com `
-InstallDNS:$true `
-LogPath C:\Windows\NTDS `
-ReadOnlyReplica:$true `
-SiteName Default-First-Site-Name `
-SYSVOLPath C:\Windows\SYSVOL
-Force:$true

```

> [!NOTE]
> O Gerenciador do Servidor geralmente preenche todos os argumentos com valores quando promove e não depende de padrões (já que eles podem ser alterados entre versões futuras do Windows ou service packs). A única exceção a isso é o argumento **-safemodeadministratorpassword**. Para forçar um prompt de confirmação, omita o valor ao executar o cmdlet interativamente.

Use o argumento opcional Whatif com o cmdlet Install-ADDSDomainController para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos de um cmdlet.

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)

### <a name="prerequisites-check"></a>Verificação de pré-requisitos
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)

A **Verificação de Pré-requisitos** é um novo recurso na configuração do domínio AD DS. Essa nova fase confirma que a configuração do servidor é capaz de dar suporte a uma nova floresta AD DS.

Ao instalar um novo domínio raiz da floresta, o Assistente de Configuração dos Serviços de Domínio do Active Directory invoca diversos testes modulares serializados. Esses testes o alertam com opções de reparo sugeridas. Você pode executar os testes quantas vezes forem necessárias. O processo do controlador de domínio não pode continuar até que todos os testes de pré-requisitos sejam feitos.

A **Verificação de Pré-requisitos** também dá superfície a informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos.

Não é possível ignorar a **Verificação de Pré-requisitos** ao usar o Gerenciador do Servidor, mas você pode ignorar o processo ao usar o cmdlet de Implantação do AD DS com o seguinte argumento:

```
-skipprechecks

```

Clique em **Instalar** para começar o processo de promoção do controlador de domínio. Esta é a última oportunidade de cancelar a instalação. Não é possível cancelar o processo de promoção uma vez que ele é iniciado. O computador reiniciará automaticamente no final da promoção, independentemente dos resultados da promoção.

### <a name="installation"></a>Instalação
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)

Quando a página **Instalação** é exibida, a configuração do controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas são exibidas nesta página e gravadas em logs:

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

Para instalar uma nova floresta do Active Directory utilizando o módulo ADDSDeployment, use o seguinte cmdlet:

```
Install-addsdomaincontroller

```

Consulte a tabela **Cmdlet ADDSDeployment** no começo desta seção para os argumentos necessários e opcionais.

O cmdlet **Install-addsdomaincontroller** possui somente duas fases (verificação de pré-requisitos e instalação). As duas figuras abaixo mostram a fase de instalação com os argumentos mínimos exigidos de **-domainname**, **-readonlyreplica**, **-sitename**, e **-credential**. Observe que, assim como o Gerenciador do Servidor, o **Install-ADDSDomainController** informa que a promoção reiniciará o servidor automaticamente:

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)

![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)

Para aceitar o prompt de reinicialização automática, use os argumentos **-force** ou **-confirm:$false** com qualquer cmdlet ADDSDeployment do Windows PowerShell. Para evitar que o servidor reinicie automaticamente no final da promoção, use o argumento **-norebootoncompletion**.

> [!WARNING]
> Não é recomendável substituir a reinicialização. O controlador de domínio deve reiniciar para funcionar corretamente. Se você sair do controlador de domínio, não poderá fazer logon novamente interativamente até que ele seja reiniciado.

### <a name="results"></a>Resultados
![Instalar RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)

A página **Resultados** mostra o sucesso ou o fracasso da promoção e qualquer informação administrativa importante. O controlador de domínio reiniciará automaticamente após 10 segundos.
