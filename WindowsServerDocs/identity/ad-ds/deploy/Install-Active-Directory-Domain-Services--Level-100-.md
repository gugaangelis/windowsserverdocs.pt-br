---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: "Instalar os serviços de domínio do Active Directory (nível 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f76aa1e5200a9fc2f47a559c4a318aa619d31557
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-active-directory-domain-services-level-100"></a>Instalar os serviços de domínio do Active Directory (nível 100)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como instalar o AD DS no Windows Server 2012, usando qualquer um dos seguintes métodos:  
  
-   [Requisitos de credenciais para executar Adprep.exe e instalar os serviços de domínio do Active Directory](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  
  
-   [Instalando o AD DS usando o Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  
  
-   [Instalando o AD DS, usando o Gerenciador do servidor](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  
  
-   [Executando uma instalação de RODC preparados usando a Interface gráfica do usuário](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  
  
## <a name="BKMK_Creds"></a>Requisitos de credenciais para executar Adprep.exe e instalar os serviços de domínio do Active Directory  
As credenciais seguintes são necessários para executar Adprep.exe e instalar o AD DS.  
  
-   Para instalar uma nova floresta, você deve estar conectado como administrador local para o computador.  
  
-   Para instalar um novo domínio filho ou nova árvore de domínio, você deve estar conectado como membro do grupo Administradores de empresa.  
  
-   Para instalar um controlador de domínio adicional em um domínio existente, você deve ser um membro do grupo Admins. do domínio.  
  
    > [!NOTE]  
    > Se você não executar o comando adprep.exe separadamente e você estiver instalando o primeiro controlador de domínio que executa o Windows Server 2012 em um domínio existente ou floresta, você precisará fornecer as credenciais para executar comandos Adprep. Os requisitos de credenciais são da seguinte maneira:  
    >   
    > -   Para apresentar o primeiro controlador de domínio do Windows Server 2012 na floresta, você precisa fornecer as credenciais de um membro do grupo Administradores corporativos, do grupo Administradores de esquema, e os administradores de domínio de grupo no domínio que hospeda o mestre de esquema.  
    > -   Para apresentar o primeiro controlador de domínio do Windows Server 2012 em um domínio, você precisa fornecer as credenciais de um membro do grupo Admins. do domínio.  
    > -   Para apresentar o primeiro controlador de domínio somente leitura (RODC) na floresta, você precisa fornecer as credenciais de um membro do grupo Administradores corporativos.  
    >   
    >     > [!NOTE]  
    >     > Se você já executou adprep /rodcprep no Windows Server 2008 ou Windows Server 2008 R2, você não precisa executá-la novamente para o Windows Server 2012.  
  
## <a name="BKMK_PS"></a>Instalando o AD DS usando o Windows PowerShell  
A partir do Windows Server 2012, você pode instalar o AD DS usando o Windows PowerShell. Dcpromo.exe foi preterido a partir do Windows Server 2012, mas você ainda pode executar o dcpromo.exe usando um arquivo de resposta (dcpromo /unattend:<answerfile> ou dcpromo /answer:<answerfile>). A capacidade de continuar executando o dcpromo.exe com um arquivo de resposta fornece as organizações que têm recursos investidos em tempo de automação existente para converter a automação de dcpromo.exe em Windows PowerShell. Para obter mais informações sobre como executar o dcpromo.exe com um arquivo de resposta, veja [https://support.microsoft.com/kb/947034](https://support.microsoft.com/kb/947034).  
  
Para obter mais informações sobre como remover o AD DS usando o Windows PowerShell, consulte [remover o AD DS usando o Windows PowerShell](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS).  
  
Comece com adicionando a função usando o Windows PowerShell. Esse comando instala a função de servidor do AD DS e instala os AD DS e do AD LDS servidor ferramentas de administração, incluindo baseado em GUI ferramentas como o Active Directory usuários e computadores e as ferramentas de linha de comando como dcdia.exe. Ferramentas de administração de servidor não são instaladas por padrão quando você usa o Windows PowerShell. Você precisa especificar **"IncludeManagementTools** para gerenciar o servidor local ou instalar [ferramentas de administração de servidor remoto](https://www.microsoft.com/download/details.aspx?id=28972) para gerenciar um servidor remoto.  
  
```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  
  
Não há nenhuma reinicialização é necessária até após a conclusão da instalação do AD DS.  
  
Em seguida, você pode executar esse comando para ver os cmdlets disponíveis no módulo ADDSDeployment.  
  
```  
Get-Command -Module ADDSDeployment
```  
  
Para ver a lista de argumentos que podem ser especificados para uma sintaxe e cmdlets:  
  
```  
Get-Help <cmdlet name>  
```  
  
Por exemplo, para ver os argumentos para a criação de uma conta de domínio somente leitura não ocupada controlador (RODC), digite  
  
```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  
  
Argumentos opcionais aparecem entre colchetes.  
  
Você também pode baixar os exemplos de ajuda mais recentes e os conceitos para cmdlets do Windows PowerShell. Para obter mais informações, consulte [about_Updatable_Help](https://technet.microsoft.com/library/hh847735.aspx).  
  
Você pode executar cmdlets do Windows PowerShell para servidores remotos:  
  
-   No Windows PowerShell, use Invoke-Command com o cmdlet ADDSDeployment. Por exemplo, para instalar o AD DS em um servidor remoto chamado ConDC3 no domínio contoso.com, digite:  
  
    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  
  
- ou -  
  
-   No Gerenciador do servidor, crie um grupo de servidores que inclui o servidor remoto. Clique com botão direito o nome do servidor remoto e clique em **do Windows PowerShell**.  
  
As seções a seguir explicam como executar cmdlets do módulo ADDSDeployment para instalar o AD DS.  
  
-   [ADDSDeployment cmdlet argumentos](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  
  
-   [Especificando as credenciais do Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  
  
-   [Usar os cmdlets de teste](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  
  
-   [Instalar um novo domínio raiz usando o Windows PowerShell](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  
  
-   [Instalar um novo domínio filho ou árvore usando o Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  
  
-   [Instalando um controlador de domínio adicional (réplica) usando o Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  
  
### <a name="BKMK_Params"></a>ADDSDeployment cmdlet argumentos  
A tabela a seguir lista os argumentos para os cmdlets ADDSDeployment no Windows PowerShell. Argumentos em negrito são necessários. Argumentos equivalente para dcpromo.exe estão listados em parênteses se eles são chamados de diferentes no Windows PowerShell.  
  
Opções do Windows PowerShell aceitam argumentos $TRUE ou $FALSE. Argumentos que são $TRUE por padrão não precisa ser especificado.  
  
Para substituir os valores padrão, você pode especificar o argumento com um valor de $False. Por exemplo, pois **- installdns** é executada automaticamente em uma instalação nova floresta se não for especificado, a única maneira de *evitar* instalação DNS quando você instala uma nova floresta é usar:  
  
```  
-InstallDNS:$false  
```  
  
Da mesma forma, pois **"installdns** tem um valor padrão de $False se você instalar um controlador de domínio em um ambiente que não hospeda Windows servidor DNS servidor, você precisa especificar o argumento a seguir para instalar o servidor DNS:  
  
```  
-InstallDNS:$true  
```  
  
|Argumento|Descrição|  
|------------|---------------|  
|**ADPrepCredential <PS Credential> ** **Observação:** necessária se você estiver instalando o primeiro controlador de domínio do Windows Server 2012 em um domínio ou floresta e as credenciais do usuário atual não são suficientes para executar a operação.|Especifica a conta com o membro do grupo Administradores corporativos e administradores de esquema pode preparar a floresta, acordo com as regras de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) e um objeto PSCredential.<br /><br />Se nenhum valor for especificado, o valor da **"credenciais** argumento é usado.|  
|AllowDomainControllerReinstall|Especifica se deve continuar a instalação neste controlador de domínio gravável, apesar do fato de que a outra conta de controlador de domínio gravável com o mesmo nome é detectada.<br /><br />Use **$True** somente se você tiver certeza de que a conta não está em uso por outro controlador de domínio gravável.<br /><br />O padrão é **$False**.<br /><br />Esse argumento não é válido para um RODC.|  
|AllowDomainReinstall|Especifica se um domínio existente é recriado.<br /><br />O padrão é **$False**.|  
|AllowPasswordReplicationAccountName < cadeia de caracteres [] >|Especifica os nomes de contas de usuário, contas de grupo e contas de computador cujas senhas podem ser replicadas para esse RODC. Usar uma cadeia de caracteres vazia "" Se você quiser manter o valor vazio. Por padrão, somente o grupo Senha RODC permitido replicação é permitido e ele é criado originalmente vazio.<br /><br />Fornece valores como uma matriz de cadeia de caracteres. Por exemplo:<br /><br />Código - AllowPasswordReplicationAccountName "JSmith", "JSmithPC", "Usuários ramificação"|  
|/ApplicationPartitionsToReplicate < cadeia de caracteres [] > **Observação:** não existe nenhuma opção equivalente na interface do usuário. Se você instalar usando a interface do usuário, ou usando IFM, todas as partições de aplicativo serão replicadas.|Especifica as partições de diretório do aplicativo para replicar. Esse argumento é aplicado somente quando você especifica o **- InstallationMediaPath** argumento para instalar da mídia (IFM). Por padrão, todos os aplicativos replicarão partições com base em suas próprias escopos.<br /><br />Fornece valores como uma matriz de cadeia de caracteres. Por exemplo:<br /><br />Código:<br /><br />-/ApplicationPartitionsToReplicate "partition1", "partition2", "partition3"|  
|Confirmar|Solicita que você confirme antes de executar o cmdlet.|  
|CreateDnsDelegation **Observação:** não é possível especificar esse argumento quando você executa o cmdlet Add-ADDSReadOnlyDomainController.|Indica se é necessário criar uma delegação de DNS que referencia o novo servidor DNS que está sendo instalada junto com o controlador de domínio. Válido para "integradas ao Active Directory DNS apenas. Registros de delegação podem ser criados apenas em servidores DNS da Microsoft que estão online e acessível. Registros de delegação não podem ser criados para domínios que estão imediatamente subordinados para domínios de nível superior, como. com, gov,. Biz,. edu ou domínios de código de país de duas letras como .nz e AU.<br /><br />O padrão é calculado automaticamente com base no ambiente.|  
|**Credenciais <PS Credential> ** **Observação:** obrigatório somente se as credenciais do usuário atual não são suficientes para executar a operação.|Especifica a conta de domínio que possa fazer logon no domínio, acordo com as regras de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) e um objeto PSCredential.<br /><br />Se nenhum valor for especificado, as credenciais do usuário atual são usadas.|  
|CriticalReplicationOnly|Especifica se a operação de instalação do AD DS executa somente crítica replicação antes da reinicialização e, em seguida, continua. A replicação não crítica acontece após a conclusão da instalação e o computador for reinicializado.<br /><br />Usar esse argumento não é recomendado.<br /><br />Não há nenhum equivalente para essa opção na interface do usuário (IU).|  
|DatabasePath <string>|Especifica o totalmente qualificado, não "caminho de convenção de nomenclatura Universal (UNC) para um diretório em um disco fixo do computador local que contém o banco de dados de domínio, por exemplo, **C:\Windows\NTDS.**<br /><br />O padrão é **%systemroot%\ntds.**. **Importante:** enquanto você pode armazenar os AD DS banco de dados e arquivos de log em volume formatado com ReFS () do sistema de arquivos resiliente, não existem nenhum vantagens específicas para hospedar o AD DS no ReFS, que não sejam os benefícios normais de resiliência você obter todos os dados ReFS de hospedagem.|  
|DelegatedAdministratorAccountName <string>|Especifica o nome do usuário ou grupo que pode instalar e administrar o RODC.<br /><br />Por padrão, somente membros do grupo Admins. do domínio podem administrar um RODC.|  
|DenyPasswordReplicationAccountName < cadeia de caracteres [] >|Especifica os nomes de contas de usuário, contas de grupo e contas de computador cujas senhas não serão replicados para este RODC. Usar uma cadeia de caracteres vazia "" se não quiser recusar a replicação de credenciais de todos os usuários ou computadores. Por padrão, administradores, operadores de servidor, operadores de Backup, operadores de conta e o grupo de replicação de Senha RODC Negado serão negado. Por padrão, o grupo de replicação de Senha RODC Negado inclui editores de certificados, Admins. do domínio, administradores corporativos, controladores de domínio da empresa, controladores de domínio somente leitura da empresa, grupo Proprietários criadores de política, a conta krbtgt e administradores de esquema.<br /><br />Fornece valores como uma matriz de cadeia de caracteres. Por exemplo:<br /><br />Código:<br /><br />-DenyPasswordReplicationAccountName "RegionalAdmins", "AdminPCs"|  
|DnsDelegationCredential <PS Credential> **Observação:** não é possível especificar esse argumento quando você executa o cmdlet Add-ADDSReadOnlyDomainController.|Especifica o nome de usuário e senha para a criação de delegação de DNS, acordo com as regras de [Get-Credential](https://technet.microsoft.com/library/dd315327.aspx) e um objeto PSCredential.|  
|DomainMode <DomainMode> {Win2003 & #124; Win2008 & #124; Win2008R2 & #124; Win2012 & #124; Win2012R2}<br /><br />Ou<br /><br />DomainMode <DomainMode> {2 #124; 3 & #124; 4 #124; 5 & #124; 6}|Especifica o nível funcional do domínio durante a criação de um novo domínio.<br /><br />O nível funcional do domínio não pode ser menor que o nível funcional da floresta, mas pode ser maior.<br /><br />O valor padrão é calculado e definir o nível funcional da floresta existente ou o valor definido para automaticamente **- ForestMode**.|  
|**DomainName**<br /><br />Obrigatório para cmdlets ADDSForest de instalação e instalar ADDSDomainController.|Especifica o FQDN do domínio no qual você deseja instalar um controlador de domínio adicional.|  
|**DomainNetbiosName <string>**<br /><br />Obrigatório para instalar ADDSForest se o nome do prefixo FQDN tiver mais de 15 caracteres.|Uso com ADDSForest de instalação. Atribui um nome NetBIOS ao domínio raiz da floresta de novo.|  
|DomainType <DomainType> {ChildDomain & #124; TreeDomain} ou {filho & #124; árvore}|Indica o tipo de domínio que você deseja criar: uma nova árvore de domínio em existente da floresta, um filho de um domínio existente ou uma nova floresta.<br /><br />O padrão para DomainType é ChildDomain.|  
|Força|Quando este parâmetro for especificado de quaisquer avisos que podem aparecer normalmente durante a instalação e a adição do controlador de domínio serão suprimidos para permitir que o cmdlet concluir sua execução. Esse parâmetro pode ser útil para incluir ao executar o script de instalação.|  
|ForestMode <ForestMode> {Win2003 & #124; Win2008 & #124; Win2008R2 & #124; Win2012 & #124; Win2012R2}<br /><br />Ou<br /><br />ForestMode <ForestMode> {2 #124; 3 & #124; 4 #124; 5 & #124; 6}|Especifica o nível funcional da floresta quando você cria uma nova floresta.<br /><br />O valor padrão é Win2012.|  
|InstallationMediaPath|Indica o local da mídia de instalação que será usado para instalar um novo controlador de domínio.|  
|InstallDns|Especifica se o serviço de servidor DNS deve ser instalado e configurado no controlador de domínio.<br /><br />Para uma nova floresta, o padrão é **$True** e servidor DNS será instalado.<br /><br />Para um novo domínio filho ou árvore de domínio, se o domínio pai (ou o domínio raiz de uma árvore de domínio) já hospeda e armazena os nomes DNS para o domínio, o padrão para esse parâmetro será $True.<br /><br />Para uma instalação de controlador de domínio em um domínio existente, se este parâmetro for especificado e o domínio atual já hospeda e armazena os nomes DNS para o domínio, em seguida, o padrão para esse parâmetro é **$True**. Caso contrário, se os nomes de domínio DNS são hospedados fora do Active Directory, o padrão é **$False** e nenhum servidor DNS será instalado.|  
|/LogPath <string>|Especifica o caminho UNC não totalmente qualificado, para um diretório em um disco fixo do computador local que contém os arquivos de log do domínio, por exemplo, **C:\Windows\Logs**.<br /><br />O padrão é **%systemroot%\ntds.**. **Importante:** não armazenar os arquivos de log do Active Directory em um volume de dados formatado com o sistema de arquivos resiliente (ReFS).|  
|MoveInfrastructureOperationMasterRoleIfNecessary|Especifica se deseja transferir a função de mestre de operações de mestre infraestrutura (operações mestre únicas também conhecido como flexíveis ou FSMO) para o controlador de domínio que você está criando "no caso ele atualmente está hospedado em um servidor de catálogo global" e você não pretende fazer o controlador de domínio que você está criando um servidor de catálogo global. Especificá-lo para transferir a função mestre de infraestrutura para o controlador de domínio que você está criando caso a transferência é necessária; Nesse caso, especifique o **NoGlobalCatalog** opção se quiser que a função mestre de infraestrutura para permanecer onde ele está no momento.|  
|**NewDomainName <string> ** **Observação:** necessário somente para instalar ADDSDomain.|Especifica o nome de domínio único para o novo domínio.<br /><br />Por exemplo, se você quiser criar um novo domínio filho denominado **emea.corp.fabrikam.com**, você deve especificar **África** como o valor desse argumento.|  
|**NewDomainNetbiosName <string>**<br /><br />Obrigatório para instalar ADDSDomain se o nome do prefixo FQDN tiver mais de 15 caracteres.|Uso com ADDSDomain de instalação. Atribui um nome NetBIOS para o novo domínio. O valor padrão é derivado do valor de **"NewDomainName**.|  
|NoDnsOnNetwork|Especifica que o serviço DNS não está disponível na rede. Esse parâmetro é usado somente quando a configuração de IP do adaptador de rede para este computador não está configurada com o nome de um servidor DNS para resolução de nomes. Isso indica que um servidor DNS será instalado no computador para resolução de nomes. Caso contrário, as configurações de IP do adaptador de rede devem primeiro ser configuradas com o endereço de um servidor DNS.<br /><br />Omitir esse parâmetro (o padrão) indica que as configurações de cliente de TCP/IP do adaptador de rede neste computador servidor serão usadas para entrar em contato com um servidor DNS. Portanto, se você não especificar esse parâmetro, certifique-se de que as configurações de cliente de TCP/IP primeiro estejam configuradas com um endereço de servidor DNS preferencial.|  
|NoGlobalCatalog|Especifica que você não deseja que o controlador de domínio para ser um servidor de catálogo global.<br /><br />Controladores de domínio que executam o Windows Server 2012 são instalados com o catálogo global por padrão. Em outras palavras, isso é executado automaticamente sem cálculos, a menos que você especifique:<br /><br />Código:<br /><br />-NoGlobalCatalog|  
|NoRebootOnCompletion|Especifica se é necessário reiniciar o computador após a conclusão do comando, independentemente de sucesso. Por padrão, o computador será reiniciado. Para impedir que o servidor reiniciar, especifique:<br /><br />Código:<br /><br />-NoRebootOnCompletion: $True<br /><br />Não há nenhum equivalente para essa opção na interface do usuário (IU).|  
|**ParentDomainName <string> ** **Observação:** necessários para o cmdlet Install-ADDSDomain|Especifica o FQDN de um domínio pai existente. Use esse argumento quando você instala um domínio filho ou nova árvore de domínio.<br /><br />Por exemplo, se você quiser criar um novo domínio filho denominado **emea.corp.fabrikam.com**, você deve especificar **corp.fabrikam.com** como o valor desse argumento.|  
|ReadOnlyReplica|Especifica se é necessário instalar um controlador de domínio somente leitura (RODC).|  
|ReplicationSourceDC <string>|Indica o FQDN do controlador de domínio parceiro do qual você replica as informações de domínio. O padrão é calculado automaticamente.|  
|**SafeModeAdministratorPassword <securestring>**|Fornece a senha da conta de administrador quando o computador é iniciado no modo de segurança ou uma variante do modo de segurança, como o modo de restauração de serviços de diretório.<br /><br />O padrão é uma senha vazia. Você deve fornecer uma senha. A senha deve ser fornecida em um formato de System.Security.SecureString, como fornecido pelo host de leitura - assecurestring ou ConvertTo-SecureString.<br /><br />Operação do argumento SafeModeAdministratorPassword é especial: se não especificado como um argumento, o cmdlet solicita que você insira e confirme a senha mascarada. Esse é o uso preferencial ao executar o cmdlet interativamente. Se especificado sem um valor e há outros sem argumentos especificado para o cmdlet, o cmdlet solicita que você insira uma senha mascarada sem confirmação. Isso não é o uso preferencial ao executar o cmdlet interativamente. Se especificado com um valor, o valor deve ser uma cadeia de caracteres segura. Isso não é o uso preferencial ao executar o cmdlet interativamente. Por exemplo, você pode manualmente solicitar uma senha, usando o cmdlet Read-Host para solicitar ao usuário uma cadeia de caracteres segura:-safemodeadministratorpassword (host de leitura - prompt "senha:" - assecurestring) você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora isso é recomendado. -safemodeadministratorpassword (convertto-securestring "Senha1" - asplaintext-forçar)|  
|**Nome do site <string>**<br /><br />Obrigatório para que o cmdlet Add-addsreadonlydomaincontrolleraccount|Especifica o site em que o controlador de domínio será instalado. Não há nenhum **"nome do site** argumento quando você executa **instalar ADDSForest** porque o primeiro site criado é Default-First-Site-Name.<br /><br />O nome do site já deve existir quando fornecido como um argumento para **- nome do site**. O cmdlet não criará o site.|  
|SkipAutoConfigureDNS|Ignora a configuração automática de configurações do cliente DNS, encaminhadores e dicas de raiz. Esse argumento está em vigor somente se o serviço de servidor DNS é já instalado ou instalado automaticamente com **- InstallDNS**.|  
|SystemKey <string>|Especifica a chave do sistema para a mídia do qual você replica os dados.<br /><br />O padrão é **none**.<br /><br />Dados devem estar no formato fornecido pelo host de leitura - assecurestring ou ConvertTo-SecureString.|  
|SysvolPath <string>|Especifica o caminho UNC não totalmente qualificado, para um diretório em um disco fixo do computador local, por exemplo, **C:\Windows\SYSVOL**.<br /><br />O padrão é **%SYSTEMROOT%\SYSVOL**. **Importante:** SYSVOL não pode ser armazenada em um volume de dados formatado com o sistema de arquivos resiliente (ReFS).|  
|SkipPreChecks|Não execute as verificações de pré-requisito antes de iniciar a instalação. Não é recomendável usar essa configuração.|  
|WhatIf|Mostra o que aconteceria se o cmdlet fosse executado. O cmdlet não é executado.|  
  
### <a name="BKMK_PSCreds"></a>Especificando as credenciais do Windows PowerShell  
Você pode especificar as credenciais sem revelá-los em texto sem formatação na tela usando [Get-credential](https://technet.microsoft.com/library/dd315327.aspx).  
  
A operação para os argumentos de LocalAdministratorPassword e - SafeModeAdministratorPassword é especial:  
  
-   Se não for especificado como um argumento, o cmdlet solicita que você insira e confirme a senha mascarada. Esse é o uso preferencial ao executar o cmdlet interativamente.  
  
-   Se especificado com um valor, o valor deve ser uma cadeia de caracteres segura. Isso não é o uso preferencial ao executar o cmdlet interativamente.  
  
Por exemplo, você pode manualmente solicitar uma senha usando o **Read-Host** cmdlet para solicitar ao usuário uma cadeia de caracteres segura  
  
```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  
  
> [!WARNING]  
> Como a opção anterior não confirme a senha, tenha muito cuidado: a senha não estiver visível.  
  
Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora isso seja recomendado:  
  
```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  
  
> [!WARNING]  
> Não é recomendável fornecer ou armazenar uma senha de texto não criptografado. Qualquer pessoa executando esse comando em um script ou observando sabe a senha DSRM do controlador de domínio. Com esse conhecimento, eles podem representar o controlador de domínio e elevar seus privilégios de nível mais alto em uma floresta do Active Directory.  
  
### <a name="BKMK_TestCmdlets"></a>Usar os cmdlets de teste  
Cada cmdlet ADDSDeployment tem um correspondente testar o cmdlet. O teste cmdlets é executado somente as verificações de pré-requisito para a operação de instalação; Nenhuma configuração de instalação é configuradas. Os argumentos para cada cmdlet teste são as mesmas para o cmdlet de instalação correspondente, mas **"SkipPreChecks** não está disponível para teste cmdlets.  
  
|Cmdlet do teste|Descrição|  
|---------------|---------------|  
|Test-ADDSForestInstallation|Executa os pré-requisitos para instalar uma nova floresta do Active Directory.|  
|Test-ADDSDomainInstallation|Executa os pré-requisitos para instalar um novo domínio no Active Directory.|  
|Test-ADDSDomainControllerInstallation|Executa os pré-requisitos para instalar um controlador de domínio no Active Directory.|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|Executa os pré-requisitos para adicionar um controlador de domínio somente leitura conta (RODC).|  
  
### <a name="BKMK_PSForest"></a>Instalar um novo domínio raiz usando o Windows PowerShell  
A sintaxe de comando para instalar uma nova floresta é o seguinte. Argumentos opcionais aparecem dentro de colchetes.  
  
```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> O argumento - DomainNetBIOSName é necessário se você quiser alterar o nome de 15 caracteres que é gerado automaticamente com base no prefixo de nome de domínio DNS ou se o nome exceder 15 caracteres.  
  
Por exemplo, para instalar uma nova floresta denominado corp.contoso.com e com segurança ser solicitado a fornecer a senha DSRM, digite:  
  
```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  
  
> [!NOTE]  
> Servidor DNS é instalado por padrão quando você executa ADDSForest instalar.  
  
Para instalar uma nova floresta denominado corp.contoso.com, criar uma delegação DNS no domínio contoso.com, defina o nível funcional do domínio para o Windows Server 2008 R2 e definir o nível funcional da floresta para o Windows Server 2008, instalar o banco de dados do Active Directory e SYSVOL na unidade D:\, instalar os arquivos de log na unidade E:\ e ser solicitado a fornecer a senha de modo de restauração de serviços de diretório e tipo :  
  
```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="BKMK_PSDomain"></a>Instalar um novo domínio filho ou árvore usando o Windows PowerShell  
A sintaxe de comando para instalar um novo domínio é o seguinte. Argumentos opcionais aparecem dentro de colchetes.  
  
```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> O **-credenciais** argumento só é necessária quando você não fez logon como um membro do grupo Administradores corporativos.  
>   
> O **- NewDomainNetBIOSName** argumento é necessário se você quiser alterar o nome de 15 caracteres gerado automaticamente com base no prefixo de nome de domínio DNS ou se o nome exceder 15 caracteres.  
  
Por exemplo, para usar as credenciais de corp\EnterpriseAdmin1 para criar um novo domínio filho denominado child.corp.contoso.com, instalar o servidor DNS, criar uma delegação DNS no domínio corp.contoso.com, definir o nível funcional do domínio para o Windows Server 2003, transformar em um servidor de catálogo global do controlador de domínio em um site chamado Houston, use DC1.corp.contoso.com como o controlador de domínio de origem de replicação, instalar o banco de dados do Active Directory e SYSVOL na unidade D:\ , instale os arquivos de log na unidade E:\ e ser solicitado a fornecer a senha do modo de restauração de serviços de diretório, mas não é solicitado a confirmar o comando, digite:  
  
```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  
  
### <a name="BKMK_PSReplica"></a>Instalando um controlador de domínio adicional (réplica) usando o Windows PowerShell  
A sintaxe de comando para instalar um controlador de domínio adicional é o seguinte. Argumentos opcionais aparecem dentro de colchetes.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Para instalar um controlador de domínio e o servidor DNS no domínio corp.contoso.com e ser solicitado a fornecer as credenciais de administrador do domínio e a senha DSRM, digite:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  
  
Se o computador já está integrado ao domínio e você for um membro do grupo Admins. do domínio, você pode usar:  
  
```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  
  
Para ser solicitado para o nome de domínio, digite:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  
  
O comando a seguir usará as credenciais de Contoso\EnterpriseAdmin1 para instalar um controlador de domínio gravável e um servidor de catálogo global em um site chamado Boston, instalar o servidor DNS, criar uma delegação DNS no domínio contoso.com, instalar da mídia que é armazenada na pasta c:\ADDS IFM, instalar o banco de dados do Active Directory e SYSVOL na unidade D:\, instale os arquivos de log na unidade E:\ , tenha o servidor reiniciar automaticamente após a instalação do AD DS foi concluída e ser solicitada a fornecer a senha do modo de restauração de serviços de diretório:  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>Executando uma instalação RODC preparada usando o Windows PowerShell  
A sintaxe de comando para criar uma conta RODC é o seguinte. Argumentos opcionais aparecem dentro de colchetes.  
  
```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  
  
A sintaxe de comando para anexar um servidor a uma conta RODC é o seguinte. Argumentos opcionais aparecem dentro de colchetes.  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
Por exemplo criar uma conta RODC denominado RODC1:  
  
```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  
  
Em seguida, execute os seguintes comandos no servidor que você deseja conectar à conta RODC1. O servidor não pode ingressar no domínio. Primeiro, instale as ferramentas de gerenciamento e a função de servidor do AD DS:  
  
```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  
  
Executar o comando a seguir para criar o RODC:  
  
```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  
  
Pressione **Y** confirmar ou incluem o **"Confirmar** argumento para impedir que o prompt de confirmação.  
  
## <a name="BKMK_GUI"></a>Instalando o AD DS, usando o Gerenciador do servidor  
AD DS podem ser instalados no Windows Server 2012, usando o Assistente para adicionar funções no Gerenciador do servidor, seguido pelo assistente domínio Active Directory serviços de configuração, que é o início de novo no Windows Server 2012. O assistente domínio Active Directory serviços de instalação (dcpromo.exe) foi preterido a partir do Windows Server 2012.  
  
As seções a seguir explicam como criar pools de servidor para instalar e gerenciar o AD DS em vários servidores e como usar os assistentes para instalar o AD DS.  
  
### <a name="BKMK_ServerPools"></a>Criando pools de servidor  
Gerenciador do servidor pode pool outros servidores na rede, desde que eles sejam acessíveis no computador que executa o Gerenciador do servidor. Depois que agrupado, você escolher os servidores de instalação remota do AD DS ou outras opções de configuração possíveis dentro do Gerenciador do servidor. O computador que executa o Gerenciador do servidor automaticamente pools em si. Para saber mais sobre pools de servidor, veja [adicionar servidores para Gerenciador do servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
> [!NOTE]  
> Para gerenciar um computador ingressado no domínio usando o Gerenciador do servidor em um servidor de grupo de trabalho, ou vice-versa, são necessárias etapas adicionais de configuração. Para obter mais informações, consulte "adicionar e gerenciar servidores em grupos de trabalho" em [adicionar servidores para Gerenciador do servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
### <a name="BKMK_installADDSGUI"></a>Instalando o AD DS  
**Credenciais administrativas**  
  
Os requisitos de credenciais para instalar o AD DS variam dependendo de qual configuração de implantação que você escolher. Para obter mais informações, consulte [requisitos para executar Adprep.exe e instalar os serviços de domínio do Active Directory de credenciais](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
Use os procedimentos a seguir para instalar o AD DS usando o método de GUI. As etapas podem ser executadas local ou remotamente. Para obter uma explicação mais detalhada dessas etapas, consulte os seguintes tópicos:  
  
-   [Implantando uma floresta com o Gerenciador do servidor](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente & #40; Nível 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  
-   [Instalar um novo filho do Active Directory do Windows Server 2012 ou domínio de árvore & #40; Nível 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
  
-   [Instalar um controlador de domínio somente leitura do servidor 2012 Active Directory do Windows & #40; RODC & #41; & #40; Nível 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  
  
##### <a name="to-install-ad-ds-by-using-server-manager"></a>Para instalar o AD DS, usando o Gerenciador do servidor  
  
1.  No Gerenciador do servidor, clique em **gerenciar** e clique em **adicionar funções e recursos** para iniciar o Assistente para adicionar funções.  
  
2.  Sobre o **antes de começar** página, clique em **próxima**.  
  
3.  No **selecionar o tipo de instalação** página, clique em **instalação baseada em função ou recurso baseado** e, em seguida, clique em **próxima**.  
  
4.  No **servidor de destino Select** página, clique em **selecionar um servidor do pool servidor**, clique no nome do servidor onde deseja instalar o AD DS e, em seguida, clique em **próxima**.  
  
    Para selecionar servidores remotos, primeiro crie um pool de servidores e adicione os servidores remotos para ele. Para obter mais informações sobre como criar pools de servidor, veja [adicionar servidores para Gerenciador do servidor](https://technet.microsoft.com/library/hh831453.aspx).  
  
5.  No **selecionar funções de servidor** página, clique em **serviços de domínio do Active Directory**, em seguida, no **assistente Adicionar funções e recursos** caixa de diálogo, clique em **adicionar recursos**e, em seguida, clique em **próxima**.  
  
6.  Sobre o **Selecione recursos** , selecione recursos adicionais que você deseja instalar e clique em **próxima**.  
  
7.  No **Active Directory Domain Services** página, examine as informações e, em seguida, clique em **próxima**.  
  
8.  Sobre o **confirmar seleções de instalação** página, clique em **instalar**.  
  
9. Sobre o **resultados** página, verifique se a instalação foi bem-sucedida e clique em **promover esse servidor para um controlador de domínio** para iniciar o Assistente de configuração de serviços de domínio Active Directory.  
  
    ![Instalar o AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  
  
    > [!IMPORTANT]  
    > Se você fechar o Assistente para adicionar funções nesse ponto sem iniciar o Assistente de configuração de serviços de domínio Active Directory, você pode reiniciá-lo clicando em tarefas no Gerenciador do servidor.  
  
    ![Instalar o AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
10. Sobre o **implantação configuração** página, escolha uma das seguintes opções:  
  
    -   Se você estiver instalando um controlador de domínio adicional em um domínio existente, clique em **adicionar um controlador de domínio a um domínio existente**e digite o nome do domínio (por exemplo, emea.corp.contoso.com) ou clique em **selecionar... ** para escolher um domínio e credenciais (por exemplo, especifique uma conta que seja um membro do grupo Admins. do domínio) e, em seguida, clique em **próxima**.  
  
        > [!NOTE]  
        > O nome do domínio e as credenciais do usuário atual são fornecidos por padrão, somente se o computador está ingressado no domínio e você estiver executando uma instalação local. Se você estiver instalando o AD DS em um servidor remoto, você precisa especificar as credenciais, por design. Se as credenciais do usuário atual não forem suficientes para executar a instalação, clique em **alteração... ** para especificar credenciais diferentes.  
  
        Para obter mais informações, consulte [instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente & #40; Nível 200 & #41; ](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
    -   Se você estiver instalando um novo domínio filho, clique em **adicionar um novo domínio a uma floresta existente**, para **selecione tipo de domínio**, selecione **domínio filho**, digite ou procure o nome do nome do DNS do domínio pai (por exemplo, corp.contoso.com), digite o nome relativo do novo domínio filho (por exemplo) Europa, Oriente Médio tipo credenciais para usar para criar o novo domínio e, em seguida, clique em **próxima**.  
  
        Para obter mais informações, consulte [instalar um novo Windows Server 2012 Active Directory filho ou árvore de domínio & #40; Nível 200 & #41; ](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Se você estiver instalando uma nova árvore de domínio, clique em **adicionar novo domínio a uma floresta existente**, para **selecione tipo de domínio**, escolha **árvore domínio**, digite o nome do domínio raiz (por exemplo, corp.contoso.com), digite o nome DNS do novo domínio (por exemplo, fabrikam.com), as credenciais do tipo usar para criar o novo domínio e, em seguida, clique em **próxima**.  
  
        Para obter mais informações, consulte [instalar um novo Windows Server 2012 Active Directory filho ou árvore de domínio & #40; Nível 200 & #41; ](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   Se você estiver instalando uma nova floresta, clique em **adicionar uma nova floresta** e, em seguida, digite o nome do domínio raiz (por exemplo, corp.contoso.com).  
  
        Para obter mais informações, consulte [instalar um novo Windows Server 2012 floresta do Active Directory & #40; Nível 200 & #41; ](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
11. Sobre o **opções de controlador de domínio** página, escolha uma das seguintes opções:  
  
    -   Se você estiver criando uma nova floresta ou domínio, selecione os níveis funcionais de domínio e da floresta, clique em **servidor de sistema de nome de domínio (DNS)**, especificar a senha DSRM e, em seguida, clique em **próxima**.  
  
    -   Se você estiver adicionando um controlador de domínio a um domínio existente, clique em **servidor de sistema de nome de domínio (DNS)**, **Catálogo Global (GC)**, ou **leitura somente RODC controlador de domínio ()** conforme necessário, escolha o nome do site, digite a senha DSRM e, em seguida, clique em **próxima**.  
  
    Para obter mais informações sobre quais opções nesta página estão disponíveis ou não está disponível em diferentes condições, consulte [opções de controlador de domínio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage).  
  
12. Sobre o **DNS opções** página (que aparece somente se você instalar um servidor DNS), clique em **delegação de DNS de atualização** conforme necessário. Nesse caso, forneça as credenciais que têm permissão para criar registros de delegação DNS na zona DNS pai.  
  
    Se não puder ser contatado um servidor DNS que hospeda a zona pai, o **atualização DNS delegação** opção não estiver disponível.  
  
    Para obter mais informações sobre se você precisa atualizar a delegação de DNS, consulte [delegação de zona de Noções básicas sobre](https://technet.microsoft.com/library/cc771640.aspx). Se você tentar atualizar a delegação de DNS e encontrar um erro, veja [DNS opções](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
13. Sobre o **RODC opções** página (que aparece somente se você instalar um RODC), especifique o nome de um grupo ou usuário que gerencia o RODC, adicionar contas para ou remover contas de grupos de replicação de senha permitido ou negado e, em seguida, clique em **próxima**.  
  
    Para obter mais informações, consulte [política de replicação de senha](https://technet.microsoft.com/library/cc730883(v=ws.10)).  
  
14. Sobre o **opções adicionais** página, escolha uma das seguintes opções:  
  
    -   Se você estiver criando um novo domínio, digite um novo nome NetBIOS ou verifique o nome NetBIOS do padrão do domínio e, em seguida, clique em **próxima**.  
  
    -   Se você estiver adicionando um controlador de domínio a um domínio existente, selecione o controlador de domínio que você deseja replicar os dados de instalação do AD DS do (ou permitir que o assistente selecionar qualquer controlador de domínio). Se você estiver instalando a partir da mídia, clique em **instalar do caminho de mídia** digite e verifique se o caminho para os arquivos de origem da instalação, clique em **próxima**.  
  
        Você não pode usar instalar da mídia (IFM) para instalar o primeiro controlador de domínio em um domínio. IFM não funciona em versões de sistema operacional diferente. Em outras palavras, para instalar um controlador de domínio adicional que executa o Windows Server 2012 usando IFM, você deve criar a mídia de backup em um controlador de domínio do Windows Server 2012. Para saber mais sobre IFM, consulte [instalando um controlador de domínio adicional pelo uso IFM](https://technet.microsoft.com/library/cc816722(WS.10).aspx).  
  
15. Sobre o **caminhos** página, digite os locais para o banco de dados do Active Directory, arquivos de log e SYSVOL pasta (ou aceitar locais padrão) e clique em **próxima**.  
  
    > [!IMPORTANT]  
    > Não armazene o banco de dados do Active Directory, arquivos de log ou pasta SYSVOL em um volume de dados formatado com o sistema de arquivos resiliente (ReFS).  
  
16. Sobre o **opções de preparação** página, as credenciais de tipo que são suficientes para executar adprep. Para obter mais informações, consulte [requisitos para executar Adprep.exe e instalar os serviços de domínio do Active Directory de credenciais ](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds).  
  
17. No **opções de revisão** página, confirme suas seleções, clique em **exibir script** se você quiser exportar as configurações para um script do Windows PowerShell e clique em **próxima**.  
  
18. Sobre o **pré-requisitos verificar** página, confirme que a validação pré-requisito concluída e, em seguida, clique em **instalar**.  
  
19. Sobre o **resultados** página, verifique se o servidor com êxito foi configurado como um controlador de domínio. O servidor será reiniciado automaticamente para concluir a instalação do AD DS.  
  
## <a name="BKMK_UIStaged"></a>Executando uma instalação de RODC preparados usando a Interface gráfica do usuário  
Um acontecer preparado permite que você crie um RODC em dois estágios. Na primeira etapa, um membro do grupo Admins. do domínio cria uma conta RODC. Na segunda etapa, um servidor é conectado à conta RODC. O segundo estágio pode ser feito por um membro do grupo Administradores do domínio ou um usuário de domínio delegado ou grupo.  
  
#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>Para criar uma conta RODC usando as ferramentas de gerenciamento do Active Directory  
  
1.  Você pode criar a conta RODC usando o Centro Administrativo do Active Directory ou Active Directory usuários e computadores.  
  
    1.  Clique em **iniciar**, clique em **ferramentas administrativas**e clique em **Centro Administrativo do Active Directory**.  
  
    2.  No painel de navegação (painel esquerdo), clique no nome do domínio.  
  
    3.  Na lista de gerenciamento (painel central), clique no **controladores de domínio** UO.  
  
    4.  No painel de tarefas (painel direito), clique em **previamente criar uma conta de controlador de domínio somente leitura**.  
  
    - Ou -  
  
    1.  Clique em **iniciar**, clique em **ferramentas administrativas**e clique em **usuários e Active Directory computadores**.  
  
    2.  O botão direito do mouse a **controladores de domínio** unidade organizacional (UO) ou clique no **controladores de domínio** UO e, em seguida, clique em **ação**.  
  
    3.  Clique em **conta de controlador de domínio somente leitura pré-criar**.  
  
2.  Sobre o **bem-vindo ao Assistente de instalação dos serviços de domínio Active Directory** página, se você quiser modificar o padrão a replicação política PRP (senha), selecione **usar a instalação em modo avançado**e, em seguida, clique em **próxima**.  
  
3.  No **credenciais de rede** página, em **especificar as credenciais da conta para usar para executar a instalação**, clique em **credenciais de logon do meu atual** ou clique em **alternativo credenciais**e clique em **definir**. No **segurança do Windows** caixa de diálogo caixa, forneça o nome de usuário e senha para uma conta que pode instalar o controlador de domínio adicionais. Para instalar um controlador de domínio adicional, você deve ser um membro do grupo Administradores de empresa ou do grupo Admins. do domínio. Quando tiver terminado de fornecer as credenciais, clique em **próxima**.  
  
4.  Sobre o **especificar o nome do computador** página, digite o nome do computador do servidor que será o RODC.  
  
5.  Sobre o **selecionar um Site** de página, selecione um site na lista ou selecione a opção de instalar o controlador de domínio no site que corresponde ao endereço IP do computador no qual você está executando o assistente e, em seguida, clique em **próxima**.  
  
6.  Sobre o **opções adicionais de controlador de domínio** de página, faça as seguintes seleções e clique em **próxima**:  
  
    -   **Servidor DNS**: essa opção é selecionada por padrão para que o controlador de domínio pode funcionar como um servidor de sistema de nome de domínio (DNS). Se você não quiser que o controlador de domínio para ser um servidor DNS, desmarque essa opção. No entanto, se você não instalar a função de servidor DNS sobre o RODC e este é o controlador de domínio somente na filial, os usuários na filial não será capazes de executar a resolução de nome quando a rede de longa distância (WAN) no site do hub estiver offline.  
  
    -   **Catálogo global**: essa opção é selecionada por padrão. Ele adiciona o catálogo global, partições de diretório somente leitura para o controlador de domínio, e ele permite que a funcionalidade de pesquisa do catálogo global. Se você não quiser que o controlador de domínio para ser um servidor de catálogo global, desmarque essa opção. No entanto, se você não instale um servidor de catálogo global na filial ou habilitar o cache de membros de grupos universais para o site que inclui o RODC, os usuários na filial não poderão fazer logon no domínio quando a WAN no site do hub estiver offline.  
  
    -   **Controlador de domínio somente leitura**. Quando você cria uma conta RODC, essa opção é selecionada por padrão e você não pode limpá-lo.  
  
7.  Se você selecionou o **usar a instalação em modo avançado** caixa de seleção no **boas-vindas** página, o **especificar a política de replicação de senha** página será exibida. Por padrão, nenhuma senha de conta é replicada no RODC e contas de segurança confidenciais (como membros do grupo Admins. do domínio) são explicitamente negadas nunca suas senhas replicadas no RODC.  
  
    Para adicionar outras contas a política, clique em **adicionar**, clique em **permita que as senhas da conta replicar este RODC** ou clique em **negará todas as senhas da conta de replicação para esse RODC** e, em seguida, selecione as contas.  
  
    Quando concluir (ou, para aceitar a configuração padrão), clique em **próxima**.  
  
8.  Sobre o **delegação de acontecer e a administração** página, digite o nome do usuário ou grupo que será anexado o servidor à conta RODC que você está criando. Você pode digitar o nome da entidade de segurança apenas um.  
  
    Para pesquisar o diretório para um determinado usuário ou grupo, clique em **definir**. Em **Selecionar usuário ou grupo**, digite o nome do usuário ou grupo. Recomendamos que você delegar acontecer e administração para um grupo.  
  
    Esse usuário ou grupo também terá direitos administrativos locais no RODC após a instalação. Se você não especificar um usuário ou grupo, somente membros do grupo Admins. do domínio ou do grupo Administradores corporativos será capazes de anexar o servidor para a conta.  
  
    Quando tiver terminado, clique em **próxima**.  
  
9. Sobre o **resumo** página, revise suas seleções. Clique em **novamente** alterar todas as seleções, se necessário.  
  
    Para salvar as configurações que você selecionou em um arquivo de resposta que você pode usar para automatizar operações subsequentes do AD DS, clique em **exportar configurações**. Digite um nome para seu arquivo de resposta e, em seguida, clique em **salvar**.  
  
    Quando tiver certeza de que as seleções são precisas, clique em **próxima** para criar a conta RODC.  
  
10. Sobre o **concluir o Assistente de instalação dos serviços de domínio Active Directory** página, clique em **concluir**.  
  
Após a criação dessa conta, você pode anexar um servidor de conta para concluir a instalação RODC. Esse segundo estágio pode ser concluído na filial onde o RODC estarão localizado. O servidor em que você executa este procedimento não deve ter ingressado no domínio. A partir do Windows Server 2012, você usar o Assistente para adicionar funções no Gerenciador do servidor para anexar um servidor a uma conta RODC.  
  
#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>Para anexar um servidor a uma conta RODC usando o Gerenciador do servidor  
  
1.  Faça logon como administrador local.  
  
2.  No Gerenciador do servidor, clique em **adicionar funções e recursos**.  
  
3.  Sobre o **antes de começar** página, clique em **próxima**.  
  
4.  No **selecionar o tipo de instalação** página, clique em **instalação baseada em função ou recurso baseado** e, em seguida, clique em **próxima**.  
  
5.  No **servidor de destino Select** página, clique em **selecionar um servidor do pool servidor**, clique no nome do servidor onde deseja instalar o AD DS e, em seguida, clique em **próxima**.  
  
6.  No **selecionar funções de servidor** página, clique em **Active Directory Domain Services**, clique em **adicionar recursos** e, em seguida, clique em **próxima**.  
  
7.  Sobre o **Selecione recursos** , selecione recursos adicionais que você deseja instalar e clique em **próxima**.  
  
8.  No **Active Directory Domain Services** página, examine as informações e, em seguida, clique em **próxima**.  
  
9. Sobre o **confirmar seleções de instalação** página, clique em **instalar**.  
  
10. Sobre o **resultados** página, verifique se **instalação foi bem-sucedida**e clique em **promover esse servidor para um controlador de domínio** para iniciar o Assistente de configuração de serviços de domínio Active Directory.  
  
    > [!IMPORTANT]  
    > Se você fechar o Assistente para adicionar funções nesse ponto sem iniciar o Assistente de configuração de serviços de domínio Active Directory, você pode reiniciá-lo clicando em tarefas no Gerenciador do servidor.  
  
    (media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
11. No **implantação configuração** página, clique em **adicionar um controlador de domínio a um domínio existente**, digite o nome do domínio (por exemplo, emea.contoso.com) e as credenciais (por exemplo, especificam uma conta que seja delegada a gerenciar e instalar o RODC) e, em seguida, clique em **próxima**.  
  
12. Sobre o **opções de controlador de domínio** página, clique em **usar conta RODC existente**, digite e confirme a senha do modo de restauração de serviços de diretório e, em seguida, clique em **próxima**.  
  
13. No **opções adicionais** página, se você estiver instalando de mídia, clique em **instalar do caminho de mídia** digite e verifique se o caminho para os arquivos de origem da instalação, selecione o controlador de domínio que você deseja replicar os dados de instalação do AD DS do (ou permitir que o assistente selecionar qualquer controlador de domínio) e, em seguida, clique em **próxima**.  
  
14. Sobre o **caminhos** página, digite os locais para o banco de dados do Active Directory, arquivos de log e SYSVOL pasta, ou aceitar locais padrão e clique em **próxima**.  
  
15. No **opções de revisão** página, confirme suas seleções, clique em **Exibir Script** para exportar as configurações para um script do Windows PowerShell e clique em **próxima**.  
  
16. Sobre o **pré-requisitos verificar** página, confirme que a validação pré-requisito concluída e, em seguida, clique em **instalar**.  
  
    Para concluir a instalação do AD DS, o servidor será reiniciado automaticamente.  
  
## <a name="see-also"></a>Consulte também  
[Solucionando problemas de implantação do controlador de domínio](Troubleshooting-Domain-Controller-Deployment.md)  
[Instalar uma nova floresta do Active Directory do Windows Server 2012 & #40; Nível 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[Instalar um novo filho do Active Directory do Windows Server 2012 ou domínio de árvore & #40; Nível 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[Instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente & #40; Nível 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  



