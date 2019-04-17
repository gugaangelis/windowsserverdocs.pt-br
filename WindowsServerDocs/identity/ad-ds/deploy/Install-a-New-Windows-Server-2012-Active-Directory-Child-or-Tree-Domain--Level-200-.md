---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: "Instalar um novo filho do Windows Server 2012 do Active Directory ou o domínio de árvore (nível 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc0eecc44bbc5f7459f22aceb5ebe41cd61948b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>Instalar um novo filho do Windows Server 2012 do Active Directory ou o domínio de árvore (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como adicionar domínios filho e árvore a uma floresta existente do Windows Server 2012, usando o Gerenciador do servidor ou do Windows PowerShell.  
  
-   [Filho e fluxo de trabalho de domínio de árvore](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Filho e do Windows PowerShell árvore domínio](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [Implantação](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="BKMK_Workflow"></a>Filho e fluxo de trabalho de domínio de árvore  
O diagrama a seguir ilustra o processo de configuração de serviços de domínio do Active Directory quando você já tiver instalado a função AD DS e iniciar o assistente domínio Active Directory serviços de configuração usando o Gerenciador do servidor para criar um novo domínio em uma floresta existente.  
  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="BKMK_PS"></a>Filho e do Windows PowerShell árvore domínio  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**Bold** argumentos são necessários. *Em itálico* argumentos podem ser especificados usando o Windows PowerShell ou o Assistente de configuração do AD DS.)|  
|**Instalar AddsDomain**|-SkipPreChecks<br /><br />***-NewDomainName***<br /><br />***-ParentDomainName***<br /><br />***-SafeModeAdministratorPassword***<br /><br />*-ADPrepCredential*<br /><br />-AllowDomainReinstall<br /><br />-Confirmar<br /><br />*-CreateDNSDelegation*<br /><br />***-Credenciais***<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-NoDNSOnNetwork<br /><br />*-DomainMode*<br /><br />***-DomainType***<br /><br />-Força<br /><br />*-InstallDNS*<br /><br />*-/LogPath*<br /><br />*-NewDomainNetBIOSName*<br /><br />*-NoGlobalCatalog*<br /><br />-NoNorebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-Nome do site*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> O **-credenciais** argumento só é necessária quando você não fez logon como um membro do grupo Administradores corporativos. O **- NewDomainNetBIOSName** argumento é necessário se você quiser alterar o nome de 15 caracteres gerado automaticamente com base no prefixo de nome de domínio DNS ou se o nome exceder 15 caracteres.  
  
## <a name="BKMK_Deployment"></a>Implantação  
  
### <a name="deployment-configuration"></a>Configuração da implantação  
Captura de tela a seguir mostra as opções para adicionar um domínio filho:  
  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
Captura de tela a seguir mostra as opções para adicionar um domínio de árvore:  
  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
Gerenciador do servidor começa cada promoção de controlador de domínio com o **implantação configuração** página. As opções restantes e campos obrigatórios alterar essa página e as páginas subsequentes, dependendo de qual operação de implantação que você selecionar.  
  
Este tópico combina duas operações distintas: promoção de domínio do filho e promoção do domínio de árvore. A única diferença entre as duas operações é o tipo de domínio que você queira criar. Todas as outras etapas são idênticas entre as duas operações.  
  
-   Para criar um novo domínio filho, clique em **adicionar um domínio a uma floresta existente** e escolha **domínio filho**. Para **nome do domínio pai**, digite ou selecione o nome do domínio pai. Digite o nome do novo domínio no **novo nome de domínio** caixa. Fornecer um filho válido, de rótulo único nome de domínio; o nome deve usar requisitos de nome de domínio DNS.  
  
-   Para criar um domínio de árvore em uma floresta existente, clique em **adicionar um domínio a uma floresta existente** e escolha **árvore domínio**. Digite o nome do domínio raiz da floresta e, em seguida, digite o nome do novo domínio. Forneça um nome de domínio totalmente qualificado raiz; o nome não pode ser rotulada única e deve usar requisitos de nome de domínio DNS.  
  
Para obter mais informações sobre nomes DNS, consulte [convenções no Active Directory de nomenclatura para computadores, domínios, sites e UOs](https://support.microsoft.com/kb/909264).  
  
O servidor Manager Assistente domínio Active Directory serviços de configuração solicitará as credenciais de domínio se suas credenciais atuais não são do domínio. Clique em **alteração** para fornecer as credenciais de domínio para a operação de promoção.  
  
O cmdlet ADDSDeployment de configuração de implantação e os argumentos são:  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opções de controlador de domínio  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
O **opções de controlador de domínio** página especifica as opções de controlador de domínio para o novo controlador de domínio. As opções de controlador de domínio configuráveis incluem **servidor DNS** e **Catálogo Global**; Você não pode configurar o controlador de domínio somente leitura como o primeiro controlador de domínio em um novo domínio.  
  
A Microsoft recomenda que todos os controladores de domínio fornecem serviços DNS e GC para alta disponibilidade em ambientes distribuídos. GC está sempre selecionada por padrão e DNS é selecionado por padrão, se o domínio atual hospeda DNS já em seus controladores de domínio, com base em uma consulta de início de autoridade. Você também deve especificar um **nível funcional do domínio**. O nível funcional padrão é o Windows Server 2012, e você pode escolher qualquer outro valor é igual ou maior que o nível funcional da floresta atual.  
  
O **opções de controlador de domínio** página também permite que você escolha Active Directory apropriados lógico **nome do site** da configuração do floresta. Por padrão, o site com a sub-rede mais correta é selecionado. Se houver apenas um site, ele será selecionado automaticamente.  
  
> [!IMPORTANT]  
> Se o servidor não pertence a uma sub-rede do Active Directory e não há mais de um site do Active Directory, nada é selecionado e o **próxima** botão fica indisponível até que você escolha um site na lista.  
  
Especificado **senha do modo de restauração dos serviços de diretório** devem cumprir a política de senha aplicada ao servidor. Sempre escolha uma senha forte e complexa ou preferencialmente, uma senha.  
  
O **opções de controlador de domínio** ADDSDeployment cmdlet argumentos são:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> O nome do site já deve existir quando fornecido como um valor para o **nome do site** argumento. O **instalar AddsDomainController** cmdlet não cria nomes de site. Você pode usar o **novo adreplicationsite** cmdlet para criar novos sites.  
  
O **instalar ADDSDomainController** cmdlet argumentos siga os mesmos padrões como Gerenciador do servidor, se não for especificado.  
  
O **SafeModeAdministratorPassword** operação do argumento é especial:  
  
-   Se *não especificado* como um argumento, o cmdlet solicita que você insira e confirme a senha mascarada. Esse é o uso preferencial ao executar o cmdlet interativamente.  
  
    Por exemplo, para criar um novo filho domínio denominado América do Sul da floresta Contoso.com e ser solicitado a inserir e confirmar uma senha mascarada:  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
  
O módulo ADDSDeployment oferece uma opção adicional para ignorar a configuração automática de configurações do cliente DNS, encaminhadores e dicas de raiz. Isso não é configurável ao usar o Gerenciador do servidor. Esse argumento é importante apenas se você já instalou o serviço de servidor DNS antes de configurar o controlador de domínio:  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opções de DNS e credenciais de delegação de DNS  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
O **DNS opções** página permite que você forneça credenciais de administrador do DNS alternativas para delegação.  
  
Ao instalar um novo domínio em uma floresta existente - onde selecionado na instalação do DNS a **opções de controlador de domínio** página - você não pode configurar todas as opções; a delegação ocorre automaticamente e de forma irrevogável. Você tem a opção de fornecer credenciais administrativas de DNS alternativas com direitos para atualizar essa estrutura.  
  
O **DNS opções** ADDSDeployment Windows PowerShell argumentos são:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
Para obter mais informações sobre a delegação de DNS, consulte [delegação de zona de Noções básicas sobre](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opções adicionais  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
O **opções adicionais** página mostra o nome NetBIOS do domínio e permite que você para ignorá-la. Por padrão, o nome NetBIOS do domínio corresponde ao rótulo de extrema esquerda do nome do domínio totalmente qualificado fornecido no **implantação configuração** página. Por exemplo, se você tiver fornecido o nome de domínio totalmente qualificado do corp.contoso.com, o nome de domínio padrão NetBIOS é CORP.  
  
Se o nome é 15 caracteres ou menos e não entre em conflito com outro nome NetBIOS, sejam alterado. Se ele está em conflito com outro nome NetBIOS, um número é acrescentado ao nome. Se o nome tiver mais de 15 caracteres, o assistente fornece uma sugestão exclusiva, truncada. Em ambos os casos, o assistente primeiro valida o nome não ainda estiver em uso por meio de uma pesquisa WINS e NetBIOS transmitido.  
  
Para obter mais informações sobre nomes DNS, consulte [convenções no Active Directory de nomenclatura para computadores, domínios, sites e UOs](https://support.microsoft.com/kb/909264).  
  
O **instalar AddsDomain** argumentos siga os mesmos padrões como Gerenciador do servidor, se não for especificado. O **DomainNetBIOSName** operação é especial:  
  
1.  Se o **NewDomainNetBIOSName** argumento não for especificado com um nome de domínio NetBIOS e o nome de domínio de rótulo único prefixo no **DomainName** argumento é 15 caracteres ou menos, em seguida, promoção continua com um nome gerado automaticamente.  
  
2.  Se o **NewDomainNetBIOSName** argumento não for especificado com um nome de domínio NetBIOS e o nome de domínio de rótulo único prefixo no **DomainName** argumento é 16 caracteres ou mais e depois promoção falha.  
  
3.  Se o **NewDomainNetBIOSName** argumento é especificado com um nome de domínio NetBIOS de 15 caracteres ou menos, em seguida, promoção continua com esse nome especificado.  
  
4.  Se o **NewDomainNetBIOSName** argumento é especificado com um nome de domínio NetBIOS de 16 caracteres ou mais e, em seguida, promoção falha.  
  
O **opções adicionais** ADDSDeployment cmdlet argumento é:  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>Caminhos  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
O **caminhos** página permite substituir os locais padrão da pasta de compartilhamento SYSVOL, os logs de transação de base de dados e o banco de dados do AD DS. Os locais padrão estão sempre em subpastas da pasta % systemroot %.  
  
O **caminhos** ADDSDeployment cmdlet argumentos são:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Opções de revisão e exibir Script  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
O **opções de revisão** página permite que você validar as configurações e certifique-se de que eles atender às suas necessidades antes de iniciar a instalação. Isso não é a última oportunidade de parar a instalação ao usar o Gerenciador do servidor. Isso é simplesmente uma opção para confirmar as configurações antes de continuar a configuração  
  
O **opções de revisão** página no Gerenciador do servidor também oferece um recurso opcional **Exibir Script** botão para criar um arquivo de texto Unicode que contém a configuração ADDSDeployment atual como um único script do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de configuração de serviços de domínio Active Directory para configurar as opções, exportar a configuração e, em seguida, cancelá-lo.  Esse processo cria um exemplo válido e sintaticamente correto para ainda mais a modificação ou uso direto. Por exemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomain `  
-NoGlobalCatalog:$false `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainType "ChildDomain" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NewDomainName "research" `  
-NewDomainNetBIOSName "RESEARCH" `  
-ParentDomainName "corp.contoso.com" `  
-Norebootoncompletion:$false `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Gerenciador do servidor normalmente preenche todos os argumentos com valores ao promover e não dependem de padrões (como eles podem mudar entre as versões futuras do Windows ou service packs). A única exceção a isso é o **- safemodeadministratorpassword** argumento (que é omitido deliberadamente do script). Para forçar um prompt de confirmação, omita o valor ao executar o cmdlet interativamente.  
  
Use opcional **Whatif** argumento com o **instalar ADDSForest** cmdlet para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos de um cmdlet.  
  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>Seleção de pré-requisitos  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
O **pré-requisitos verificar** é um novo recurso na configuração de domínio do AD DS. Essa nova fase valida se a configuração do servidor é capaz de dar suporte a um novo domínio do AD DS.  
  
Ao instalar um novo domínio raiz, o servidor Manager Assistente domínio Active Directory Services configuração invoca uma série de testes modulares serializados. Esses testes alertarão-lo com opções de reparo sugeridos. Você pode executar os testes quantas vezes for necessário. O processo de controlador de domínio não pode continuar até que todos os pré-requisitos testes passar.  
  
O **pré-requisitos verificar** superfícies também informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos.  
  
Para obter mais informações sobre as verificações de pré-requisito específicas, consulte [pré-requisito verificando](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Você não pode ignorar a **Verificar pré-requisito** quando usar o Gerenciador do servidor, mas você pode ignorar o processo ao usar o cmdlet do AD DS implantação usando o argumento a seguir:  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft desestimula ignorar a verificação de pré-requisitos como ele pode levar a uma promoção de controlador de domínio parcial ou danificado floresta do AD DS.  
  
Clique em **instalar** para iniciar o processo de promoção de controlador de domínio. Esta é a última oportunidade para cancelar a instalação. Você não pode cancelar o processo de promoção depois que ele é iniciado. O computador é reinicializado automaticamente no final da promoção, independentemente dos resultados da promoção.  
  
### <a name="installation"></a>Instalação  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
Quando o **instalação** página exibe, a configuração de controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas exibem nesta página e são gravadas nos logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Para instalar um novo domínio do Active Directory usando o módulo ADDSDeployment, use o seguinte cmdlet:  
  
```  
Install-addsdomain  
```  
  
Consulte [filho e do Windows PowerShell árvore domínio](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS) para argumentos necessários e opcionais. O **instalar addsdomain** cmdlet só tem duas fases (verificação de pré-requisitos e instalação). As duas figuras a seguir mostram a fase de instalação com os argumentos mínimos necessários de **- domaintype**, **- newdomainname**, **- parentdomainname**, e **-credenciais**. Observe como, assim como Gerenciador do servidor, **instalar ADDSDomain** lembra que promoção reinicializará o servidor automaticamente.  
  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
Para aceitar o prompt de reinicialização automaticamente, use o **-forçar** ou **-confirmar: $false** argumentos com qualquer cmdlet ADDSDeployment Windows PowerShell. Para impedir que o servidor reiniciar automaticamente no final da promoção, use o **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> Substituindo a reinicialização não é recomendado. O controlador de domínio é necessário reinicializar para funcionar corretamente  
  
### <a name="results"></a>Resultados  
![Instalar um novo anúncio filho](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
O **resultados** página mostra o sucesso ou fracasso de promoção e todas as informações administrativas importantes. O controlador de domínio é reinicializado automaticamente após 10 segundos.  
  

