---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: "Instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente (nível 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e3151a8beee2870ecc747a64241df9d562ad4cc2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>Instalar um controlador de domínio da réplica Windows Server 2012 em um domínio existente (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico abrange as etapas necessárias para atualizar um domínio ou floresta existente para o Windows Server 2012, usando o Gerenciador do servidor ou do Windows PowerShell. Ela aborda como adicionar controladores de domínio que executam o Windows Server 2012 a um domínio existente.  
  
-   [Atualização e fluxo de trabalho da réplica](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Atualização e da réplica do Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [Implantação](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>Atualização e fluxo de trabalho da réplica  
O diagrama a seguir ilustra o processo de configuração de serviços de domínio do Active Directory quando você já tiver instalado a função AD DS e iniciar o assistente domínio Active Directory serviços de configuração usando o Gerenciador do servidor para criar um novo controlador de domínio em um domínio existente.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>Atualização e da réplica do Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**Bold** argumentos são necessários. *Em itálico* argumentos podem ser especificados usando o Windows PowerShell ou o Assistente de configuração do AD DS.)|  
|Instalar AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-Nome do site*<br /><br />*-ADPrepCredential*<br /><br />-/ApplicationPartitionsToReplicate<br /><br />*-AllowDomainControllerReinstall*<br /><br />-Confirmar<br /><br />*-CreateDNSDelegation*<br /><br />***-Credenciais***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-Força<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-/LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />-Nome do site<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> O **-credenciais** argumento só é necessário se você não já efetuou logon como um membro dos grupos de administradores de empresa e administradores de esquema (se você estiver atualizando a floresta) ou do grupo Admins. do domínio (se você estiver adicionando um novo controlador de domínio a um domínio existente).  
  
## <a name="BKMK_Dep"></a>Implantação  
  
### <a name="deployment-configuration"></a>Configuração da implantação  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
Gerenciador do servidor começa cada promoção de controlador de domínio com o **implantação configuração** página. As opções restantes e campos obrigatórios alterar essa página e as páginas subsequentes, dependendo de qual operação de implantação que você selecionar.  
  
Para atualizar uma floresta existente ou adicionar um controlador de domínio gravável a um domínio existente, clique em **adicionar um controlador de domínio a um domínio existente** e clique em **selecione** para **especificar as informações de domínio para este domínio**. Gerenciador do servidor solicita credenciais válidas se necessário.  
  
Atualizando a floresta requer credenciais que incluem participações em grupos os administradores corporativos e os administradores de esquema no Windows Server 2012. O Assistente de configuração de serviços de domínio Active Directory solicita que você mais tarde se suas credenciais atuais não tiver as permissões adequadas ou membros do grupo.  
  
O processo de Adprep automático é a diferença apenas operacional entre adicionando um controlador de domínio a um domínio existente do Windows Server 2012 e um domínio em que os controladores de domínio executam uma versão anterior do Windows Server.  
  
O cmdlet ADDSDeployment de configuração de implantação e os argumentos são:  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
Executam alguns testes em cada página, alguns dos quais repetir mais tarde como discretas verificações de pré-requisito. Por exemplo, se o domínio selecionado não atender os níveis funcionais mínimo, você não precise ir completamente por meio de promoção para a verificação de pré-requisitos para descobrir:  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>Opções de controlador de domínio  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
O **opções de controlador de domínio** página especifica as funcionalidades de controlador de domínio para o novo controlador de domínio. As funcionalidades de controlador de domínio configurável são **servidor DNS**, **Catálogo Global**, e **controlador de domínio somente leitura**. A Microsoft recomenda que todos os controladores de domínio fornecem serviços DNS e GC para alta disponibilidade em ambientes distribuídos. GC está sempre selecionada por padrão e o servidor DNS é selecionado por padrão, se os hosts atual do domínio DNS já em seus controladores de domínio com base na consulta de início de autoridade. O **opções de controlador de domínio** página também permite que você escolha Active Directory apropriados lógico **nome do site** da configuração do floresta. Por padrão, ele selecionará o site com a sub-rede mais correta. Se houver apenas um site, ele selecionará automaticamente.  
  
> [!NOTE]  
> Se o servidor não pertence a uma sub-rede do Active Directory e não há mais de um site do Active Directory, nada é selecionado e o **próxima** botão fica indisponível até que você escolha um site na lista.  
  
Especificado **senha do modo de restauração dos serviços de diretório** devem cumprir a política de senha aplicada ao servidor. Sempre escolha uma senha forte e complexa ou preferencialmente, uma senha.  
  
O **opções de controlador de domínio** ADDSDeployment argumentos são:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> O nome do site já deve existir quando fornecido como um argumento para **- nome do site**. O **instalar AddsDomainController** cmdlet não criar sites. Você pode usar o cmdlet **novo adreplicationsite** para criar novos sites.  
  
O **SafeModeAdministratorPassword** operação do argumento é especial:  
  
-   Se *não especificado* como um argumento, o cmdlet solicita que você insira e confirme a senha mascarada. Esse é o uso preferencial ao executar o cmdlet interativamente.  
  
    Por exemplo criar um controlador de domínio adicional no domínio treyresearch.net e ser solicitado a inserir e confirmar uma senha mascarada:  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
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
> Fornecendo ou armazenar uma senha de texto clara ou ofuscados não é recomendado. Qualquer pessoa executando esse comando em um script ou observando sabe a senha DSRM do controlador de domínio.  Qualquer pessoa com acesso ao arquivo poderia inverter essa senha ofuscada. Com esse conhecimento, eles podem fazer logon em um controlador de domínio iniciado no DSRM e eventualmente representar o controlador de domínio em si, elevar seus privilégios de nível mais alto em uma floresta do Active Directory. Um conjunto adicional de etapas usando **Cryptography** para criptografar o arquivo de texto dados são aconselhável, mas fora do escopo. A prática recomendada é evitar totalmente o armazenamento de senhas.  
  
O cmdlet ADDSDeployment oferece uma opção adicional para ignorar a configuração automática de configurações do cliente DNS, encaminhadores e dicas de raiz. Você não pode ignorar essa opção de configuração ao usar o Gerenciador do servidor. Esse argumento é importante somente se você instalou a função de servidor DNS antes de configurar o controlador de domínio:  
  
```  
-SkipAutoConfigureDNS  
```  
  
O **opções de controlador de domínio** página avisa que você não pode criar controladores de domínio somente leitura se os controladores de domínio existentes executam o Windows Server 2003. Isso é esperado, e você pode ignorar o aviso.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opções de DNS e credenciais de delegação de DNS  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
O **DNS opções** página permite que você configure delegação de DNS, se você selecionou o **servidor DNS** opção no *opções de controlador de domínio* página e se apontando para uma zona onde delegações DNS são permitidas. Você talvez precise fornecer credenciais alternativas de um usuário que é um membro do **administradores DNS** grupo.  
  
O **DNS opções** ADDSDeployment cmdlet argumentos são:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
Para obter mais informações sobre se você precisa criar uma delegação de DNS, consulte [delegação de zona de Noções básicas sobre](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opções adicionais  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
O **opções adicionais** página fornece a opção de configuração para nomear um controlador de domínio como a origem de replicação, ou você pode usar qualquer controlador de domínio como a origem de replicação.  
  
Você também pode escolher instalar o controlador de domínio usando o backup de mídia usando a instalação da opção de mídia (IFM). O **instalar da mídia** checkbox fornece uma opção de procurar uma vez selecionada e você deve clicar **verificar** para garantir que o caminho fornecido é mídia válida. Mídia usada pela opção IFM é criada com o Backup do Windows Server ou Ntdsutil.exe de outro existente Windows Server 2012 computador Você não pode usar um Windows Server 2008 R2 ou o sistema operacional anterior para criar mídia para um controlador de domínio do Windows Server 2012. Para obter mais informações sobre alterações no IFM, consulte [apêndice a administração simplificada](../../ad-ds/deploy/Simplified-Administration-Appendix.md). Se usar mídia protegida com um SYSKEY, Gerenciador do servidor solicita senha da imagem durante a verificação.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
O **opções adicionais** ADDSDeployment cmdlet argumentos são:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>Caminhos  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
O **caminhos** página permite substituir os locais de pastas padrão do AD DS banco de dados, os logs de transação de banco de dados, e o SYSVOL compartilhar. Os locais padrão estão sempre em subpastas da pasta % systemroot %.  
  
Os argumentos de cmdlet do Active Directory caminhos ADDSDeployment são:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opções de preparação  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
O **opções de preparação** página alertando que a configuração do AD DS inclui estender o esquema (forestprep) e atualizar o domínio (domainprep).  Esta página foi exibida apenas quando floresta e no domínio não foram preparados pela instalação anterior de controlador de domínio do Windows Server 2012 ou executem Adprep.exe manualmente. Por exemplo, o Assistente de configuração de serviços de domínio Active Directory suprime nesta página, se você adicionar um novo controlador de domínio a um domínio de raiz de floresta existente do Windows Server 2012.  
  
Estender o esquema e atualizando o domínio não ocorrer quando você clica em **próxima**. Esses eventos ocorrem somente durante a fase de instalação. Esta página simplesmente traz reconhecimento sobre os eventos que ocorrerá posteriormente na instalação.  
  
Esta página também valida que as credenciais do usuário atual são membros dos grupos Administradores corporativos e da administração do esquema, conforme necessário associação nesses grupos para estender o esquema ou preparar um domínio. Clique em **alteração** para fornecer as credenciais do usuário adequado, se a página informa que as credenciais atuais não fornecer permissões suficientes.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
O argumento de cmdlet ADDSDeployment de opções adicionais é:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Como com versões anteriores do Windows Server, preparação de domínio automatizados para controladores de domínio que executam o Windows Server 2012 não executa GPPREP. Executar **adprep.exe /gpprep** manualmente para todos os domínios que não foram preparados anteriormente para o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2. Você deve executar GPPrep apenas uma vez no histórico de um domínio, não com cada atualização. Adprep.exe não executa /gpprep automaticamente porque sua operação pode fazer com que todos os arquivos e pastas na pasta SYSVOL replicar novamente em todos os controladores de domínio.  
>   
> RODCPrep automático é executado quando você promove o primeiro RODC cancelou preparado em um domínio. Ele não ocorre quando você promove o primeiro controlador de domínio gravável do Windows Server 2012. Você pode também ainda manualmente **adprep.exe /rodcprep** se você planeja implantar controladores de domínio somente leitura.  
  
### <a name="review-options-and-view-script"></a>Opções de revisão e exibir Script  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
O **opções de revisão** página permite que você validar as configurações e certifique-se de que atendam aos requisitos antes de iniciar a instalação. Isso não é a última oportunidade de parar a instalação usando o Gerenciador do servidor. Esta página simplesmente permite que você revise e confirme as configurações antes de continuar a configuração.  
  
O **opções de revisão** página no Gerenciador do servidor também oferece um recurso opcional **Exibir Script** botão para criar um arquivo de texto Unicode que contém a configuração ADDSDeployment atual como um único script do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de configuração de serviços de domínio Active Directory para configurar as opções, exportar a configuração e, em seguida, cancelá-lo.  Esse processo cria um exemplo válido e sintaticamente correto para ainda mais a modificação ou uso direto.  
  
Por exemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "root.fabrikam.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Gerenciador do servidor normalmente preenche todos os argumentos com valores ao promover e não dependem de padrões (como eles podem mudar entre as versões futuras do Windows ou service packs). A única exceção a isso é o **- safemodeadministratorpassword** argumento. Para forçar um prompt de confirmação omitir o valor ao executar o cmdlet interativamente  
>   
> Use opcional **Whatif** argumento com o **instalar ADDSDomainController** cmdlet para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos de um cmdlet.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>Seleção de pré-requisitos  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
O **pré-requisitos verificar** é um novo recurso na configuração de domínio do AD DS. Essa nova fase valida que o domínio e a floresta são capazes de dar suporte a um novo controlador de domínio do Windows Server 2012.  
  
Ao instalar um novo controlador de domínio, o servidor Manager Assistente domínio Active Directory Services configuração invoca uma série de testes modulares serializados. Esses testes alertarão-lo com opções de reparo sugeridos. Você pode executar os testes quantas vezes for necessário. O processo de controlador de domínio não pode continuar até que todos os pré-requisitos testes passar.  
  
O **pré-requisitos verificar** superfícies também informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos.  
  
Para saber mais sobre as verificações de pré-requisito específicas, consulte [pré-requisito verificando](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Você não pode ignorar a **Verificar pré-requisito** quando usar o Gerenciador do servidor, mas você pode ignorar o processo ao usar o cmdlet do AD DS implantação usando o argumento a seguir:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft desestimula ignorar a verificação de pré-requisitos como ele pode levar a uma promoção de controlador de domínio parcial ou danificado floresta do AD DS.  
  
Clique em **instalar** para iniciar o processo de promoção de controlador de domínio. Esta é a última oportunidade para cancelar a instalação. Você não pode cancelar o processo de promoção depois que ele é iniciado. O computador é reinicializado automaticamente no final da promoção, independentemente dos resultados da promoção. O **pré-requisitos verificar** página exibe quaisquer problemas que ele encontrou durante o processo e orientações para resolver o problema.  
  
### <a name="installation"></a>Instalação  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
Quando o **instalação** página exibe, a configuração de controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas exibem nesta página e são gravadas nos logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\debug\netsetup.log (se o servidor está em um grupo de trabalho)  
  
Para instalar uma nova floresta do Active Directory usando o módulo ADDSDeployment, use o seguinte cmdlet:  
  
```  
Install-addsdomaincontroller  
```  
  
Consulte [atualização e da réplica do Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS) para argumentos necessários e opcionais.  
  
O **instalar AddsDomainController** cmdlet só tem duas fases (verificação de pré-requisitos e instalação). As duas figuras a seguir mostram a fase de instalação com os argumentos mínimos necessários de **- domainname** e **-credenciais**. Observe como a operação Adprep ocorre automaticamente como parte da inclusão o primeiro controlador de domínio do Windows Server 2012 para uma floresta existente do Windows Server 2003:  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
Observe como, assim como Gerenciador do servidor, **instalar ADDSDomainController** lembra que promoção reinicializará o servidor automaticamente. Para aceitar o prompt de reinicialização automaticamente, use o **-forçar** ou **-confirmar: $false** argumentos com qualquer cmdlet ADDSDeployment Windows PowerShell. Para impedir que o servidor reiniciar automaticamente no final da promoção, use o **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> Não é recomendado substituindo a reinicialização. Reinicialize o controlador de domínio para funcionar corretamente.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
Para configurar um controlador de domínio usando o Windows PowerShell, encapsule a **instalar adddomaincontroller** cmdlet *dentro* do **comando invocar** cmdlet. Isso requer o uso de chaves.  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
Por exemplo:  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> Para obter mais informações sobre como a instalação e Adprep processam funciona, consulte o [solução de problemas de implantação de controlador de domínio](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
### <a name="results"></a>Resultados  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
O **resultados** página mostra o sucesso ou fracasso de promoção e todas as informações administrativas importantes. Se for bem-sucedida, o controlador de domínio é reinicializado automaticamente após 10 segundos.  
  
Como nas versões anteriores do Windows Server, preparação de domínio automatizados para controladores de domínio que executam o Windows server 2012 não executa GPPREP. Executar **adprep.exe /gpprep** manualmente para todos os domínios que não foram preparados anteriormente para o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2. Você deve executar GPPrep apenas uma vez no histórico de um domínio, não com cada atualização. Adprep.exe não executa /gpprep automaticamente porque sua operação pode fazer com que todos os arquivos e pastas na pasta SYSVOL replicar novamente em todos os controladores de domínio.  
  

