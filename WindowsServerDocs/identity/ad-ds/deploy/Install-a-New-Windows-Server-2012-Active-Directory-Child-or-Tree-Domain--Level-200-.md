---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: Instalar um novo domínio de árvore ou filho do Active Directory do Windows Server 2012 (nível 200)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7a3ea43b513535d6b7d5a815039869b71c7d7c92
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965778"
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>Instalar um novo domínio de árvore ou filho do Active Directory do Windows Server 2012 (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como adicionar domínios filho e de árvore a uma floresta existente do Windows Server 2012 usando o Gerenciador do Servidor ou o Windows PowerShell.  
  
-   [Fluxo de trabalho de domínios filho e de árvore](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Domínios filho e de árvore no Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [Implantação](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="child-and-tree-domain-workflow"></a><a name="BKMK_Workflow"></a>Fluxo de trabalho de domínios filho e de árvore  
O diagrama a seguir ilustra o processo de configuração dos Serviços de Domínio do Active Directory, na instalação prévia da função AD DS e inicialização do Assistente de Configuração dos Serviços de Domínio do Active Directory com o Gerenciador do Servidor, para criar um novo domínio em uma floresta existente.  
  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="child-and-tree-domain-windows-powershell"></a><a name="BKMK_PS"></a>Domínios filho e de árvore no Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlet ADDSDeployment**|Argumentos (os argumentos em **Negrito** são necessários. Os argumentos em *Itálico* podem ser especificados usando o Windows PowerShell ou o Assistente de Configuração do AD DS.)|  
|**Install-AddsDomain**|-SkipPreChecks<p>***-NewDomainName***<p>***-ParentDomainName***<p>***-safemodeadministratorpassword***<p>*-ADPrepCredential*<p>-AllowDomainReinstall<p>-Confirm<p>*-CreateDNSDelegation*<p>***-Credential***<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>-NoDNSOnNetwork<p>*-DomainMode*<p>***-DomainType***<p>-Force<p>*-InstallDNS*<p>*-LogPath*<p>*-NewDomainNetBIOSName*<p>*-NoGlobalCatalog*<p>-NoNorebootoncompletion<p>*-ReplicationSourceDC*<p>*-SiteName*<p>-SkipAutoConfigureDNS<p>*-SYSVOLPath*<p>*-Whatif*|  
  
> [!NOTE]  
> O argumento **-credential** será preciso apenas se você não tiver feito logon como membro do grupo Administradores de Empresa, o argumento **-NewDomainNetBIOSName** será necessário se você desejar alterar o nome de 15 caracteres gerado automaticamente com base no prefixo do nome do domínio DNS (Sistema de Nomes de Domínio) ou se o nome exceder 15 caracteres.  
  
## <a name="deployment"></a><a name="BKMK_Deployment"></a>Implantação  
  
### <a name="deployment-configuration"></a>Configuração de Implantação  
A captura de tela a seguir mostra as opções para adicionar um domínio filho:  
  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
A captura de tela a seguir mostra as opções para adicionar um domínio de árvore:  
  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
O Gerenciador do Servidor começa toda a promoção do controlador de domínio com a página **Configuração de Implantação**. As demais opções e campos exigidos mudam nessa página e nas páginas subsequentes, dependendo da operação de implantação selecionada.  
  
Este tópico combina duas operações discretas: promoção de domínio filho e promoção de domínio de árvore. A única diferença entre essas operações será o tipo de domínio que você criar. Todas as outras etapas dessas duas operações são idênticas.  
  
-   Para criar um novo domínio filho, clique em **Adicionar um domínio a uma floresta existente** e escolha **Domínio Filho**. Em **Nome de domínio pai**, digite ou escolha o nome do domínio pai. Depois, digite o nome do novo domínio na caixa **Nome do novo domínio**. Forneça um nome válido e de rótulo único para o domínio filho; o nome deve usar os requisitos de nome de domínio DNS.  
  
-   Para criar um domínio de árvore em uma floresta existente, clique em **Adicionar um domínio a uma floresta existente** e escolha **Domínio de Árvore**. Digite o nome do domínio raiz da floresta e o nome do novo domínio. Forneça um nome válido e totalmente qualificado para o domínio raiz; o nome não pode ser de rótulo único e deve usar os requisitos de nome de domínio DNS.  
  
Para saber mais sobre nomes de DNS, confira [Convenções de nomeação do Active Directory para computadores, domínios, sites e unidades organizacionais](https://support.microsoft.com/kb/909264).  
  
O Assistente de Configuração dos Serviços de Domínio do Active Directory do Gerenciador do Servidor solicitará as credenciais de domínio se suas credenciais atuais não forem do domínio. Clique em **Alterar** para fornecer as credenciais de domínio para a operação de promoção.  
  
O cmdlet ADDSDeployment e os argumentos da Configuração de Implantação são:  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opções de Controlador de Domínio  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
A página **Opções do Controlador de Domínio** especifica as opções do novo controlador de domínio. As opções configuráveis de controlador de domínio incluem **Servidor DNS** e **Catálogo Global**; não é possível configurar um controlador de domínio somente leitura como o primeiro controlador de domínio em um novo domínio.  
  
A Microsoft recomenda que todos os controladores de domínio forneçam serviços DNS e GC para alta disponibilidade em ambientes distribuídos. O GC está sempre selecionado por padrão, e o DNS estará selecionado por padrão se o domínio atual já hospedar o DNS em seus DCs (controladores de domínio), com base em uma consulta SOA (Start-of-Authority). Você também deve especificar um **Nível funcional do domínio**. O nível funcional padrão é o Windows Server 2012 e você pode escolher qualquer outro valor igual ou maior que o nível funcional atual da floresta.  
  
A página **Opções do Controlador de Domínio** também permite que você escolha o **nome de site** lógico do Active Directory, na configuração da floresta. Por padrão, o site com a sub-rede mais correta será escolhido. Se houver apenas um site, ele será escolhido automaticamente.  
  
> [!IMPORTANT]  
> Se o servidor não pertencer a uma sub-rede do Active Directory e houver mais de um site do Active Directory, nada será selecionado e o botão **Avançar** ficará indisponível até você escolher um site na lista.  
  
A **Senha do Modo de Restauração dos Serviços de Diretório** deve aderir à política de senha aplicada ao servidor. Escolha sempre uma senha forte e complexa.  
  
Os argumentos do cmdlet ADDSDeployment nas **Opções do Controlador de Domínio** são:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> O nome do site já deverá existir quando fornecido como um valor para o argumento **sitename**. O cmdlet **install-AddsDomainController** não cria nomes de site. Você pode usar o cmdlet **new-adreplicationsite** para criar novos sites.  
  
Os argumentos do cmdlet **Install-ADDSDomainController** seguirão os mesmos padrões do Gerenciador do Servidor, se não especificado.  
  
A operação do argumento **SafeModeAdministratorPassword** é especial:  
  
-   Se *nenhum argumento for especificado*, o cmdlet solicitará que você insira e confirme uma senha mascarada. Este é o uso preferencial ao executar o cmdlet interativamente.  
  
    Por exemplo, para criar um novo domínio filho chamado NorthAmerica na floresta Contoso.com e ser solicitado a digitar e confirmar uma senha mascarada:  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
> O fornecimento ou o armazenamento de uma senha com texto não criptografado ou ofuscado não é recomendável. Qualquer pessoa que executar esse comando em um script ou que estiver por perto tomará conhecimento da senha DSRM desse controlador de domínio.  Qualquer pessoa com acesso ao arquivo pode reverter essa senha ofuscada. Com esse conhecimento, ela pode fazer logon em um DC iniciado em DSRM e, eventualmente, representar o próprio controlador de domínio, elevando seus privilégios ao nível mais alto em uma floresta AD. Um conjunto adicional de etapas, usando **System.Security.Cryptography** para criptografar os dados do arquivo de texto é aconselhável, mas fora do escopo. A melhor prática é evitar totalmente o armazenamento de senha.  
  
O módulo ADDSDeployment oferece uma opção adicional para pular a configuração automática de definições do cliente DNS, encaminhadores e dicas de raiz. Isso não pode ser configurado com o Gerenciador do Servidor. Este argumento será válido apenas você já tiver instalado o serviço Servidor DNS antes de configurar o controlador de domínio:  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opções de DNS e credenciais de delegação de DNS  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
A página **Opções de DNS** permite fornecer credenciais de administrador de DNS alternativas para delegação.  
  
Ao instalar um novo domínio em uma floresta existente, em que você tiver escolhido a instalação do DNS na página **Opções do Controlador de Domínio**, você não conseguirá configurar as opções, pois as delegações são automáticas e irrevogáveis. Você tem a opção de fornecer credenciais administrativas de DNS alternativas com direitos para atualizar a estrutura.  
  
Os argumentos de ADDSDeployment do Windows PowerShell nas **Opções de DNS** são:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
Para obter mais informações sobre delegação de DNS, consulte [Noções básicas sobre delegação de zona](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771640(v=ws.11)).  
  
### <a name="additional-options"></a>Opções adicionais  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
A página **Opções Adicionais** mostra o nome NetBIOS do domínio e permite que você o substitua. Por padrão, o nome de domínio NetBIOS corresponde ao rótulo mais à esquerda do nome de domínio totalmente qualificado fornecido na página **Configuração de Implantação**. Por exemplo, se você forneceu o nome de domínio totalmente qualificado corp.contoso.com, o nome de domínio padrão do NetBIOS é CORP.  
  
Se o nome tiver 15 caracteres ou menos e não entrar em conflito com outro nome NetBIOS, ele ficará inalterado. Se ocorrer conflito com outro nome NetBIOS, um número será acrescentado ao nome. Se o nome tiver mais de 15 caracteres, o assistente fornecerá uma sugestão truncada exclusiva. Em ambos os casos, o assistente primeiro valida o nome que ainda não está em uso por meio de uma consulta WINS e difusão de NetBIOS.  
  
Para saber mais sobre nomes de DNS, confira [Convenções de nomeação do Active Directory para computadores, domínios, sites e unidades organizacionais](https://support.microsoft.com/kb/909264).  
  
Os argumentos de **Install-AddsDomain** seguirão os mesmos padrões do Gerenciador do Servidor, se não especificados. A operação **DomainNetBIOSName** é especial:  
  
1.  Se o argumento **NewDomainNetBIOSName** não for especificado com um nome de domínio NetBIOS e o nome do domínio de rótulo único com prefixo no argumento **DomainName** tiver 15 caracteres ou menos, a promoção continuará com um nome gerado automaticamente.  
  
2.  Se o argumento **NewDomainNetBIOSName** não for especificado com um nome de domínio NetBIOS e o nome do domínio de rótulo único com prefixo no argumento **DomainName** tiver 16 caracteres ou mais, a promoção falhará.  
  
3.  Se o argumento **NewDomainNetBIOSName** for especificado com um nome de domínio NetBIOS de 15 caracteres ou menos, a promoção continuará com o nome especificado.  
  
4.  Se o argumento **NewDomainNetBIOSName** for especificado com um nome de domínio NetBIOS de 16 caracteres ou mais, a promoção falhará.  
  
O argumento do cmdlet ADDSDeployment em **Opções Adicionais** é:  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>Caminhos  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
A página **Caminhos** permite substituir os locais padrão das pasta do banco de dados AD DS, dos logs de transação de banco de dados e do compartilhamento SYSVOL. Os locais padrão estão sempre em subdiretórios do %systemroot%.  
  
Os argumentos de cmdlet do ADDSDeployment **Caminhos** são:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Examinar opções e exibir script  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
A página **Examinar Opções** permite que você valide suas configurações e verifique se elas cumprem os requisitos, antes de iniciar a instalação. Esta não é a última oportunidade de interromper a instalação ao usar o Gerenciador do Servidor. Essa página simplesmente permite que você confirme suas configurações antes de continuar a configuração.  
  
A página **Opções de revisão** do Gerenciador do Servidor também oferece um botão **Exibir Script** para criar um arquivo de texto Unicode contendo a configuração atual de ADDSDeployment como um script simples do Windows PowerShell. Isso permite que você use a interface gráfica do Gerenciador do Servidor como um estúdio de implantação do Windows PowerShell. Use o Assistente de Configuração dos Serviços de Domínio do Active Directory para configurar opções, exportar a configuração e então cancelar o assistente.  Esse processo cria um exemplo válido e sintaticamente correto para modificações adicionais ou uso direto. Por exemplo:  
  
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
> O Gerenciador do Servidor geralmente preenche todos os argumentos com valores quando promove e não depende de padrões (já que eles podem ser alterados entre versões futuras do Windows ou service packs). Uma exceção a isso é o argumento **-safemodeadministratorpassword** (que é deliberadamente omitido do script). Para forçar um prompt de confirmação, omita o valor ao executar o cmdlet interativamente.  
  
Use o argumento **Whatif** opcional com o cmdlet **Install-ADDSForest** para examinar as informações da configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos de um cmdlet.  
  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>Verificação de pré-requisitos  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
A **Verificação de Pré-requisitos** é um novo recurso na configuração do domínio AD DS. Essa nova fase confirma que a configuração do servidor consegue dar suporte a um novo domínio do AD DS.  
  
Ao instalar um novo domínio raiz da floresta, o Assistente de Configuração dos Serviços de Domínio do Active Directory invoca diversos testes modulares serializados. Esses testes o alertam com opções de reparo sugeridas. Você pode executar os testes quantas vezes forem necessárias. O processo do controlador de domínio não pode continuar até que todos os testes de pré-requisitos sejam feitos.  
  
A **Verificação de Pré-requisitos** também dá superfície a informações relevantes, como alterações de segurança que afetam os sistemas operacionais mais antigos.  
  
Para mais informações sobre as verificações de pré-requisitos, consulte [Verificação de pré-requisito](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
Não é possível ignorar a **Verificação de Pré-requisitos** ao usar o Gerenciador do Servidor, mas você pode ignorar o processo ao usar o cmdlet de Implantação do AD DS com o seguinte argumento:  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> A Microsoft desencoraja ignorar a verificação de pré-requisito, pois isso pode levar a uma promoção parcial do controlador de domínio ou danificar a floresta AD DS.  
  
Clique em **Instalar** para começar o processo de promoção do controlador de domínio. Esta é a última oportunidade de cancelar a instalação. Não é possível cancelar o processo de promoção uma vez que ele é iniciado. O computador reiniciará automaticamente no final da promoção, independentemente dos resultados da promoção.  
  
### <a name="installation"></a>Instalação  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
Quando a página **Instalação** é exibida, a configuração do controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas são exibidas nesta página e gravadas em logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Para instalar um novo domínio do Active Directory utilizando o módulo ADDSDeployment, use o seguinte cmdlet:  
  
```  
Install-addsdomain  
```  
  
Confira [Domínio filho e de árvore no Windows PowerShell)](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS) para ver os argumentos pedidos e opcionais. O cmdlet **Install-addsdomain** tem apenas duas fases (verificação de pré-requisitos e instalação). As duas figuras abaixo mostram a fase de instalação com os argumentos mínimos pedidos para **-domaintype**, **-newdomainname**, **-parentdomainname** e **-credential**. Observe que, assim como o Gerenciador do Servidor, o **Install-ADDSDomain** informa que a promoção reiniciará o servidor automaticamente.  
  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
Para aceitar o prompt de reinicialização automática, use os argumentos **-force** ou **-confirm:$false** com qualquer cmdlet ADDSDeployment do Windows PowerShell. Para evitar que o servidor reinicie automaticamente no final da promoção, use o argumento **-norebootoncompletion**.  
  
> [!WARNING]  
> Não é recomendável substituir a reinicialização. O controlador de domínio deve reiniciar para funcionar corretamente.  
  
### <a name="results"></a>Resultados  
![Instalar um novo filho do AD](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
A página **Resultados** mostra o sucesso ou o fracasso da promoção e qualquer informação administrativa importante. O controlador de domínio reiniciará automaticamente após 10 segundos.  
  
