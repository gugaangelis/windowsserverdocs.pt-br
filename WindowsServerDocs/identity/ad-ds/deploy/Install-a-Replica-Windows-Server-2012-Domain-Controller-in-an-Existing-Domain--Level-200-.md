---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: Instalar uma réplica de controlador de domínio do Windows Server 2012 em um domínio existente (nível 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: cb4432084386cb3296163f24c801be1c74b379df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883037"
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>Instalar uma réplica de controlador de domínio do Windows Server 2012 em um domínio existente (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico aborda as etapas necessárias para atualizar uma floresta ou um domínio existente no Windows Server 2012 usando o Gerenciador do Servidor ou o Windows PowerShell. Ele aborda como adicionar controladores de domínio que executam o Windows Server 2012 a um domínio existente.  
  
-   [Atualização e o fluxo de trabalho da réplica](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Atualização e réplica Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [Implantação](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>Atualização e o fluxo de trabalho da réplica  
O diagrama a seguir ilustra o processo de configuração dos Serviços de Domínio Active Directory, quando você instalou anteriormente a função AD DS e iniciou o Assistente de Configuração de Serviços de Domínio Active Directory usando o Gerenciador do Servidor para criar um novo controlador de domínio em um domínio existente.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>Atualização e réplica Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (os argumentos em **Negrito** são necessários. Os argumentos em*Itálico* podem ser especificados usando o Windows PowerShell ou o Assistente de Configuração do AD DS.)|  
|Install-AddsDomainController|-SkipPreChecks<br /><br />***-DomainName***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-SiteName*<br /><br />*-ADPrepCredential*<br /><br />-ApplicationPartitionsToReplicate<br /><br />*-AllowDomainControllerReinstall*<br /><br />-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />***-Credential***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-Force<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />-SiteName<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> O argumento **-credential** somente é requerido se você ainda não tiver entrado como membro dos grupos Administradores de Empresa e Administradores de Esquema (se você estiver atualizando a floresta) ou o grupo Admins. do Domínio (se você estiver adicionando um novo controlador de domínio a um domínio existente).  
  
## <a name="BKMK_Dep"></a>Implantação  
  
### <a name="deployment-configuration"></a>Configuração de Implantação  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
O Gerenciador do Servidor começa toda a promoção do controlador de domínio com a página **Configuração de Implantação** . As demais opções e campos exigidos mudam nessa página e nas páginas subsequentes, dependendo da operação de implantação selecionada.  
  
Para atualizar uma floresta existente ou adicionar um controlador de domínio gravável, clique em **Adicionar um controlador de domínio ou um domínio existente** e clique em **Selecionar** para **Especificar as informações de domínio para este domínio**. O Gerenciador do Servidor solicitará credenciais válidas, se necessário.  
  
Atualizar a floresta requer credenciais que incluem associações a grupos nos grupos Administradores de Empresa e Administradores de Esquema no Windows Server 2012. O Assistente de Configuração dos Serviços de Domínio Active Directory avisará você se as suas credenciais não tiverem as permissões ou as associações de grupo adequadas.  
  
O processo automático Adprep é a única diferença operacional entre adicionar um controlador de domínio a um domínio do Windows Server 2012 existente e um domínio onde os controladores de domínio executam uma das primeiras versões do Windows Server.  
  
O cmdlet ADDSDeployment e os argumentos da Configuração de Implantação são:  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
Certos testes são realizados em cada página, sendo que alguns são repetidos posteriormente como verificações de pré-requisito discreto. Por exemplo, se o domínio selecionado não cumprir o nível funcional mínimo, você não precisará da promoção para a verificação de pré-requisitos para descobrir:  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>Opções de controlador de domínio  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
A página **Opções do Controlador de Domínio** especifica os recursos de controlador de domínio para o novo controlador de domínio. Os recursos configuráveis do controlador de domínio incluem **Servidor DNS**, **Catálogo Global** e **Controlador de domínio somente leitura**. A Microsoft recomenda que todos os controladores de domínio forneçam serviços DNS e GC para alta disponibilidade em ambientes distribuídos. GC é sempre selecionado por padrão e o servidor DNS é selecionado por padrão se o domínio atual já hospedar o DNS em seus DCs com base na consulta Início de autoridade. A página **Opções do Controlador de Domínio** também permite que você escolha o **nome de site** lógico do Active Directory, na configuração da floresta. Por padrão, o site é selecionado com a sub-rede mais correta. Se houver apenas um site, ele será selecionado automaticamente.  
  
> [!NOTE]  
> Se o servidor não pertencer a uma sub-rede do Active Directory e houver mais de um site do Active Directory, nada será selecionado e o botão **Avançar** ficará indisponível até você escolher um site na lista.  
  
A **Senha do Modo de Restauração dos Serviços de Diretório** deve aderir à política de senha aplicada ao servidor. Escolha sempre uma senha forte e complexa.  
  
Os argumentos ADDSDeployment nas **Opções do Controlador de Domínio** são:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> O nome do site já deve existir quando fornecido como um argumento para **-sitename**. O cmdlet **install-AddsDomainController** não cria sites. Você pode usar o cmdlet **new-adreplicationsite** para criar novos sites.  
  
A operação do argumento **SafeModeAdministratorPassword** é especial:  
  
-   Se *nenhum argumento for especificado* , o cmdlet solicitará que você insira e confirme uma senha mascarada. Este é o uso preferencial ao executar o cmdlet interativamente.  
  
    Por exemplo, para criar um controlador de domínio adicionar no domínio treyresearch.net e ser solicitado a digitar e confirmar uma senha mascarada:  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
    ```  
  
-   Se especificado *com um valor*, esse valor deverá ser uma cadeia de caracteres segura. Este não é o uso preferencial ao executar o cmdlet interativamente.  
  
Por exemplo, você pode solicitar manualmente uma senha usando o cmdlet **Read-Host** para solicitar ao usuário uma cadeia de caracteres segura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Como a opção anterior não confirma a senha, seja extremamente cuidadoso: a senha não fica visível.  
  
Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora seja altamente recomendável não fazer isso.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
Finalmente, você pode armazenar a senha ofuscada em um arquivo e depois reutilizá-la mais tarde, sem a senha com texto não criptografado aparecendo. Por exemplo:   
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> O fornecimento ou o armazenamento de uma senha com texto não criptografado ou ofuscado não é recomendável. Qualquer pessoa que executar esse comando em um script ou que estiver por perto tomará conhecimento da senha DSRM desse controlador de domínio.  Qualquer pessoa com acesso ao arquivo pode reverter essa senha ofuscada. Com esse conhecimento, ela pode fazer logon em um DC iniciado em DSRM e, eventualmente, representar o próprio controlador de domínio, elevando seus privilégios ao nível mais alto em uma floresta Active Directory. Um conjunto adicional de etapas, usando **System.Security.Cryptography** para criptografar os dados do arquivo de texto é aconselhável, mas fora do escopo. A melhor prática é evitar totalmente o armazenamento de senha.  
  
O cmdlet ADDSDeployment oferece uma opção adicional para pular a configuração automática das configurações de cliente DNS, encaminhadores e dicas de raiz. Não é possível pular essa opção de configuração quando usar o Gerenciador do Servidor. Esse argumento aplica-se somente se você instalou a função do Servidor DNS antes de configurar o controlador de domínio:  
  
```  
-SkipAutoConfigureDNS  
```  
  
A página **Opções do Controlador de Domínio** avisa que não é possível criar controladores de domínio somente de leitura se os controladores de domínio existentes forem executados no Windows Server 2003. Isso é esperado e você pode ignorar o aviso.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opções de DNS e credenciais de delegação de DNS  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
A página **Opções de DNS** permite configurar a delegação de DNS se você selecionou a opção **Servidor de DNS** na página *Opções do Controlador de Domínio* e se apontou para uma zona onde as delegações de DNS são permitidas. Você pode fornecer credenciais alternativas de um usuário membro do grupo **Administradores de DNS** .  
  
Os argumentos do cmdlet ADDSDeployment nas **Opções de DNS** são:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
Para saber se você precisa criar uma delegação de DNS, consulte [Noções básicas de delegação de zona](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opções adicionais  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
A página **Opções Adicionais** fornece a opção de configuração para nomear um controlador de domínio como origem de replicação, ou você pode usar qualquer controlador de domínio como origem de replicação.  
  
Também é possível optar pela instalação do controlador de domínio usando uma mídia de backup e escolhendo a opção Instalar da mídia (IFM). A caixa de seleção **Instalar da mídia** fornece uma opção de navegação quando selecionada e você deve clicar em **Verificar** para garantir que o caminho fornecido é uma mídia válida. A mídia usada pela opção IFM deve ser criada com o Backup do Windows Server ou com o Ntdsutil.exe somente em outro computador Windows Server 2012 existente; não é possível usar o Windows Server 2008 R2 ou um sistema operacional anterior para criar mídia para um controlador de domínio do Windows Server 2012. Para mais informações sobre as alterações no IFM, consulte [Apêndice de Administração Simplificado](../../ad-ds/deploy/Simplified-Administration-Appendix.md). Se a mídia for protegida por SYSKEY, o Gerenciador do Servidor solicitará a senha da imagem durante a verificação.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
Os argumentos do cmdlet ADDSDeployment em **Opções Adicionais** são:  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>Caminhos  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
A página **Rotas** permite substituir os locais de pasta padrão do banco de dados AD DS, os logs de transação de banco de dados e o compartilhamento SYSVOL. Os locais padrão estão sempre em subdiretórios do %systemroot%.  
  
Os argumentos do cmdlet ADDSDeployment nos caminhos do Active Directory são:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>Opções de preparação  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
A página **Opções de Preparação** avisa que a configuração do AD DS inclui estender o esquema (forestprep) e atualizar o domínio (domainprep).  Essa página só é vista quando a floresta e o domínio não foram preparados pela instalação anterior do controlador de domínio do Windows Server 2012 ou em uma execução manual de Adprep.exe. Por exemplo, o Assistente de Configuração dos Serviços de Domínio Active Directory suprimirá essa página se você adicionar um novo controlador de domínio a um domínio raiz da floresta do Windows Server 2012 existente.  
  
A ampliação do esquema e a atualização do domínio não ocorrem quando você clica em **Avançar**. Esses eventos só ocorrem na fase de instalação. Esta página simplesmente faz conhecer os eventos que ocorrerão no final da instalação.  
  
Esta página também confirma que as credenciais atuais do usuário estão associadas aos grupos de Administração de Esquema e Administradores de Empresa, já que você precisa estar associado a esses grupos para estender o esquema ou preparar um domínio. Clique em **Alterar** para fornecer credenciais de usuário adequadas, se a página informar que as credenciais atuais não oferecem permissões suficientes.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
O argumento do cmdlet ADDSDeployment em Opções Adicionais é:  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> Com as versões anteriores do Windows Server, a preparação automatizada de domínio para controladores de domínio que executam o Windows Server 2012 não executa o GPPREP. Execute **adprep.exe /gpprep** manualmente para todos os domínios que não foram previamente preparados para o Windows Server 2003, o Windows Server 2008 ou o Windows Server 2008 R2. Você deve executar GPPrep apenas uma vez no histórico de um domínio, não com cada atualização. O Adprep.exe não executa /gpprep automaticamente porque seu funcionamento pode fazer com que todos os arquivos e pastas na pasta SYSVOL sejam replicados em todos os controladores de domínio.  
>   
> O RODCPrep automático é executado quando você promove o primeiro RODC sem preparo em um domínio. Ele não ocorre quando você promove o primeiro controlador de domínio gravável do Windows Server 2012. Você também pode executar manualmente o **adprep.exe /rodcprep** se planejar implantar controladores de domínio somente leitura.  
  
### <a name="review-options-and-view-script"></a>Examinar opções e exibir script  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
A página **Opções de revisão** permite que você valide suas configurações e se assegure de que elas cumprem os requisitos, antes de iniciar a instalação. Esta não é a última oportunidade de interromper a instalação usando o Gerenciador do Servidor. Essa página simplesmente permite que você examine e confirme suas configurações antes de continuar.  
  
A página **Opções de revisão** do Gerenciador do Servidor também oferece um botão **Exibir Script** para criar um arquivo de texto Unicode contendo a configuração atual de ADDSDeployment como um script simples do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do Servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de Configuração dos Serviços de Domínio do Active Directory para configurar opções, exportar a configuração e então cancelar o assistente.  Esse processo cria um exemplo válido e sintaticamente correto para modificações adicionais ou uso direto.  
  
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
> O Gerenciador do Servidor geralmente preenche todos os argumentos com valores quando promove e não depende de padrões (já que eles podem ser alterados entre versões futuras do Windows ou service packs). A única exceção a isso é o argumento **-safemodeadministratorpassword** . Para forçar um pedido de confirmação, omita o valor ao executar o cmdlet interativamente  
>   
> Use o argumento opcional **Whatif** com o cmdlet **Install-ADDSDomainController** para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos de um cmdlet.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>Verificação de pré-requisitos  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
A **Verificação de Pré-requisitos** é um novo recurso na configuração do domínio AD DS. Esta nova fase valida se o domínio e a floresta são capazes de dar suporte a um novo controlador de domínio do Windows Server 2012.  
  
Ao instalar um novo controlador de domínio, o assistente de configuração dos Serviços de Domínio Active Directory invoca uma série de testes modulares serializados. Esses testes o alertam com opções de reparo sugeridas. Você pode executar os testes quantas vezes forem necessárias. O processo do controlador de domínio não pode continuar até que todos os testes de pré-requisitos sejam feitos.  
  
A **Verificação de Pré-requisitos** também dá superfície a informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos.  
  
Para mais informações sobre as verificações de pré-requisitos específicas, consulte [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Não é possível ignorar a **Verificação de Pré-requisitos** ao usar o Gerenciador do Servidor, mas você pode ignorar o processo ao usar o cmdlet de Implantação do AD DS com o seguinte argumento:  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> A Microsoft desencoraja ignorar a verificação de pré-requisito, pois isso pode levar a uma promoção parcial do controlador de domínio ou danificar a floresta AD DS.  
  
Clique em **Instalar** para começar o processo de promoção do controlador de domínio. Esta é a última oportunidade de cancelar a instalação. Não é possível cancelar o processo de promoção uma vez que ele é iniciado. O computador será reiniciado automaticamente no fim da promoção, independentemente dos resultados dela. A página **Verificação de pré-requisitos** exibe quaisquer problemas encontrados durante o processo e orienta sobre como resolvê-los.  
  
### <a name="installation"></a>Instalação  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
Quando a página **Instalação** é exibida, a configuração do controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas são exibidas nesta página e gravadas em logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\debug\netsetup.log (se o servidor estiver em um grupo de trabalho)  
  
Para instalar uma nova floresta do Active Directory utilizando o módulo ADDSDeployment, use o seguinte cmdlet:  
  
```  
Install-addsdomaincontroller  
```  
  
Consulte [Atualização e réplica do Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS) para obter os argumentos opcionais e obrigatórios.  
  
O cmdlet **Install-AddsDomainController** possui somente duas fases (verificação de pré-requisitos e instalação). As duas figuras abaixo mostram a fase de instalação com os argumentos mínimos requeridos de **-domainname** e **-credential**. Observe que a operação Adprep ocorre automaticamente como parte da adição do primeiro controlador de domínio do Windows Server 2012 a uma floresta existente do Windows Server 2003:  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
Observe que, assim como o Gerenciador do Servidor, o **Install-ADDSDomainController** informa que a promoção reiniciará o servidor automaticamente. Para aceitar o prompt de reinicialização automática, use os argumentos **-force** ou **-confirm:$false** com qualquer cmdlet ADDSDeployment do Windows PowerShell. Para evitar que o servidor reinicie automaticamente no final da promoção, use o argumento **-norebootoncompletion** .  
  
> [!WARNING]  
> Não é recomendável substituir a reinicialização. O controlador de domínio deve reiniciar para funcionar corretamente.  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
Para configurar um controlador de domínio remotamente usando o Windows PowerShell, encapsule o cmdlet **install-adddomaincontroller** *dentro* do cmdlet **invoke-command**. Isso requer o uso de chaves.  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
Por exemplo:   
  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> Para obter mais informações sobre como a instalação e o processo Adprep funcionam, consulte [Troubleshooting Domain Controller Deployment](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md).  
  
### <a name="results"></a>Resultados  
![Instalar uma réplica](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
A página **Resultados** mostra o sucesso ou o fracasso da promoção e qualquer informação administrativa importante. Se executado com êxito, o controlador de domínio reiniciará automaticamente após 10 segundos.  
  
Com as versões anteriores do Windows Server, a preparação automatizada de domínio para controladores de domínio que executam o Windows Server 2012 não executa o GPPREP. Execute **adprep.exe /gpprep** manualmente para todos os domínios que não foram previamente preparados para o Windows Server 2003, o Windows Server 2008 ou o Windows Server 2008 R2. Você deve executar GPPrep apenas uma vez no histórico de um domínio, não com cada atualização. O Adprep.exe não executa /gpprep automaticamente porque seu funcionamento pode fazer com que todos os arquivos e pastas na pasta SYSVOL sejam replicados em todos os controladores de domínio.  
  

