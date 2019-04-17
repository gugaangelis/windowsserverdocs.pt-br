---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: "Rebaixando controladores de domínio e domínios (nível 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c254a01da5534c1ddc673bc1e60382c166ddeda7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="demoting-domain-controllers-and-domains-level-200"></a>Rebaixando controladores de domínio e domínios (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica como remover o AD DS, usando o Gerenciador do servidor ou do Windows PowerShell.  
  
-   [Fluxo de trabalho de remoção do AD DS](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Workflow)  
  
-   [Rebaixamento e remoção de função do Windows PowerShell](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_PS)  
  
-   [Rebaixar](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Demote)  
  
## <a name="BKMK_Workflow"></a>Fluxo de trabalho de remoção do AD DS  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  
  
> [!CAUTION]  
> Depois de promoção para um controlador de domínio não é suportada e impedir que o servidor seja inicializado normalmente, removendo as funções do AD DS com Dism.exe ou o módulo DISM do Windows PowerShell.  
>   
> Ao contrário do Gerenciador do servidor ou o módulo ADDSDeployment para Windows PowerShell, o DISM é um sistema de manutenção nativo que tem nenhum conhecimento inerente do AD DS ou sua configuração. Não use Dism.exe ou o módulo DISM do Windows PowerShell para desinstalar a função AD DS, a menos que o servidor não é um controlador de domínio.  
  
## <a name="BKMK_PS"></a>Rebaixamento e remoção de função do Windows PowerShell  
  
|||  
|-|-|  
|**Cmdlets de ServerManager e ADDSDeployment**|Argumentos (**Bold** argumentos são necessários. *Em itálico* argumentos podem ser especificados usando o Windows PowerShell ou o Assistente de configuração do AD DS.)|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-Confirmar<br /><br />***-Credenciais***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-Força<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|Desinstale-WindowsFeature/Remove-WindowsFeature|***-Name***<br /><br />***-IncludeManagementTools***<br /><br />*-Reiniciar*<br /><br />-Remover<br /><br />-Força<br /><br />-ComputerName<br /><br />-Credenciais<br /><br />-/LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> O **-credenciais** argumento é somente necessário se você não já efetuou logon como um membro do grupo Administradores corporativos (rebaixando última DC em um domínio) ou o grupo Admins. do domínio (rebaixando uma réplica DC).The **- includemanagementtools** argumento só é necessário se você deseja remover todos os utilitários de gerenciamento do AD DS.  
  
## <a name="BKMK_Demote"></a>Rebaixar  
  
### <a name="remove-roles-and-features"></a>Remover funções e recursos  
Gerenciador do servidor oferece duas interfaces para remoção da função de serviços de domínio do Active Directory:  
  
-   O **gerenciar** menu no painel principal, usando **remover funções e recursos**  
  
 ![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  
  
-   Clique em **AD DS** ou **todos os servidores** no painel de navegação. Role para baixo até o **funções e recursos** seção. Clique com botão direito **Active Directory Domain Services** no **funções e recursos** lista e clique em **Remover função ou recurso **. Essa interface ignora o **seleção de servidor** página.  
  
 ![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  
  
Os cmdlets ServerManager **desinstalar-WindowsFeature** e **Remove-WindowsFeature** impedi-lo de removendo a função AD DS até que você rebaixar o controlador de domínio.  
  
### <a name="server-selection"></a>Seleção de servidor  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  
  
O **seleção de servidor** caixa de diálogo permite que você escolha de um dos servidores adicionados anteriormente para o pool, desde que seja acessível. O servidor local que executa o Gerenciador do servidor sempre está disponível automaticamente.  
  
### <a name="server-roles-and-features"></a>Recursos e funções de servidor  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  
  
Limpar o **Active Directory Domain Services** caixa de seleção para rebaixar um controlador de domínio; Se o servidor está atualmente um controlador de domínio, isso não remove a função AD DS e em vez disso, alterna para um **resultados da validação** caixa de diálogo com a oferta rebaixar. Caso contrário, ele simplesmente remove os binários como qualquer outro recurso de função.  
  
-   Não remova quaisquer outras funções do AD DS relacionados ou recursos - como DNS, GPMC ou as ferramentas RSAT - se você pretende promover o controlador de domínio novamente imediatamente. Removendo recursos e funções adicionais aumenta o tempo para promover novamente, como o Gerenciador do servidor reinstala esses recursos quando você reinstalar a função.  
  
-   Remova funções desnecessárias do AD DS e os recursos a seu próprio critério, se você pretende rebaixar o controlador de domínio permanentemente. Isso requer desmarcando as caixas de seleção para essas funções e recursos.  
  
    A lista completa de recursos e funções do AD DS relacionadas incluem:  
  
    -   Ativado recursos do módulo de diretório para o Windows PowerShell  
  
    -   Recurso do AD DS e ferramentas do AD LDS  
  
    -   Recurso de centro administrativo do Active Directory  
  
    -   Recurso de Snap-ins do AD DS e as ferramentas de linha de comando  
  
    -   Servidor DNS  
  
    -   Console de gerenciamento de política de grupo  
  
Os cmdlets equivalentes ADDSDeployment e ServerManager do Windows PowerShell são:  
  
```  
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
  
```  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  
  
### <a name="credentials"></a>Credenciais  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  
  
Configurar opções de rebaixamento no **credenciais** página. Forneça as credenciais necessárias para executar o rebaixamento na lista a seguir:  
  
-   Rebaixar um controlador de domínio adicionais requer credenciais de administrador do domínio. Selecionando **forçar a remoção neste controlador de domínio** rebaixa o controlador de domínio sem remover os metadados do objeto de controlador de domínio do Active Directory.  
  
    > [!WARNING]  
    > Não selecione essa opção, a menos que o controlador de domínio não pode entrar em contato com outros controladores de domínio e não há *nenhuma maneira razoável* para resolver esse problema de rede. Rebaixamento forçado deixa órfã metadados no Active Directory nos controladores de domínio restantes na floresta. Além disso, todas as alterações não replicadas no controlador de domínio, como senhas ou novas contas de usuário, são perdidas para sempre. Metadados órfã são a causa raiz em uma porcentagem significativa dos casos de atendimento ao cliente Microsoft para o AD DS, Exchange, SQL e outro software.  
    >   
    > Se você rebaixar forçadamente um controlador de domínio, você *deve* executar manualmente a limpeza de metadados imediatamente. Para obter etapas, examine [limpa metadados do servidor](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
   ![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
-   Rebaixar o último controlador de domínio em um domínio requer a membros do grupo Administradores corporativos, como isso remove o próprio domínio (se o último domínio na floresta, isso remove a floresta). Gerenciador do servidor informa se o controlador de domínio atual é o último controlador de domínio no domínio. Selecione o **último controlador de domínio no domínio** caixa de seleção para confirmar o controlador de domínio é o último controlador de domínio no domínio.  
  
Os argumentos ADDSDeployment Windows PowerShell equivalente são:  
  
```  
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
  
```  
  
### <a name="warnings"></a>Avisos  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  
  
O **avisos** página avisa sobre as possíveis consequências de remoção neste controlador de domínio. Para continuar, você deve selecionar **prosseguir com a remoção de**.  
  
> [!WARNING]  
> Se você tiver selecionado **forçar a remoção neste controlador de domínio** sobre o **credenciais** página, o **avisos** página mostra todas as funções de operações de mestre único flexíveis hospedadas neste controlador de domínio. Você *deve* executar as funções de controlador de domínio outro *imediatamente* após rebaixando esse servidor. Para obter mais informações sobre captura das funções FSMO, consulte [assumir a função mestre de operações](https://technet.microsoft.com/library/cc816779(WS.10).aspx).  
  
Esta página não tem um argumento ADDSDeployment Windows PowerShell equivalente.  
  
### <a name="removal-options"></a>Opções de remoção  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  
  
O **opções de remoção** aparecerá dependendo anteriormente selecionando **último controlador de domínio no domínio** no **credenciais** página. Essa página permite que você configurar as opções de remoção adicionais. Selecione **Ignorar último servidor DNS zona**, **remover partições**, e **remover delegação de DNS** para expor o **próxima** botão.  
  
As opções aparecem apenas se aplicável para esse controlador de domínio. Por exemplo, se não houver nenhuma delegação DNS para esse servidor, essa caixa de seleção não serão exibidas.  
  
Clique em **alteração** para especificar as credenciais administrativas de DNS alternativas. Clique em **exibir partições** para exibir o assistente remove durante o rebaixamento de partições adicionais. Por padrão, as únicas opções adicionais partições são zonas de DNS de floresta e domínio DNS. Todas as outras partições são partições não são do Windows.  
  
Os argumentos de cmdlet ADDSDeployment equivalente são:  
  
```  
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```  
  
### <a name="new-administrator-password"></a>Nova senha de administrador  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  
  
O **nova senha de administrador** página exige que você forneça uma senha de conta de administrador do computador local interno, depois que o rebaixamento é concluída e o computador se torna um servidor membro do domínio ou o computador do grupo de trabalho.  
  
O **desinstalar ADDSDomainController** cmdlet e argumentos seguem os mesmos padrões como Gerenciador do servidor se não for especificado.  
  
O **LocalAdministratorPassword** argumento é especial:  
  
-   Se *não especificado* como um argumento, em seguida, o cmdlet solicita que você insira e confirme a senha mascarada. Esse é o uso preferencial ao executar o cmdlet interativamente  
  
-   Se especificado *com um valor*, em seguida, o valor deve ser uma cadeia de caracteres segura. Isso não é o uso preferencial ao executar o cmdlet interativamente  
  
Por exemplo, você pode manualmente solicitar uma senha usando o **Read-Host** cmdlet para solicitar ao usuário uma cadeia de caracteres segura  
  
```  
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Como as duas opções anteriores não confirme a senha, tenha muito cuidado: a senha não está visível  
  
Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora isso é recomendado. Por exemplo:  
  
```  
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
> [!WARNING]  
> Não é recomendável fornecer ou armazenar uma senha de texto não criptografado. Qualquer pessoa executando esse comando em um script ou observando sabe a senha de administrador local do computador. Com esse conhecimento, eles têm acesso a todos os seus dados e podem representar o servidor em si.  
  
### <a name="confirmation"></a>Confirmação  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)  
  
O **confirmação** página mostra o rebaixamento planejado; a página não lista as opções de configuração rebaixamento. Esta é a última página o assistente mostra antes de começa o rebaixamento. O botão Exibir Script cria um script de rebaixamento do Windows PowerShell.  
  
Clique em **diminuir níveis** para executar o seguinte cmdlet de implantação do AD DS:  
  
```  
Uninstall-DomainController  
  
```  
  
Use opcional **Whatif** argumento com o **desinstalar ADDSDomainController** e cmdlet para examinar as informações de configuração. Isso permite que você veja os valores explícitos e implícitos dos argumentos do cmdlet.  
  
Por exemplo:  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)  
  
O prompt para reiniciar é sua última oportunidade de cancelar essa operação ao usar ADDSDeployment Windows PowerShell. Para substituir esse prompt, use o **-forçar** ou **confirmar: $false** argumentos.  
  
### <a name="demotion"></a>Rebaixamento  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  
  
Quando o **rebaixamento** página exibe, a configuração de controlador de domínio começa e não pode ser interrompida ou cancelada. Operações detalhadas exibem nesta página e gravar logs:  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Desde que **desinstalar AddsDomainController** e **desinstalar-WindowsFeature** só têm uma ação relativamente, eles são mostrados aqui na fase de confirmação com os argumentos mínimos necessários. Pressionar ENTER inicia o processo de rebaixamento irrevogável e reinicia o computador.  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)  
  
Para aceitar o prompt de reinicialização automaticamente, use o **-forçar** ou **-confirmar: $false** argumentos com qualquer cmdlet ADDSDeployment Windows PowerShell. Para impedir que o servidor reiniciar automaticamente no final da promoção, use o **- norebootoncompletion: $false** argumento.  
  
> [!WARNING]  
> Não é recomendado substituindo a reinicialização. O servidor membro é necessário reinicializar para funcionar corretamente.  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)  
  
Aqui está um exemplo de forçadamente rebaixando com os argumentos mínimo necessários de **- forceremoval** e **- demoteoperationmasterrole**. O **-credenciais** argumento não é necessário porque o usuário conectado como membro do grupo Administradores de empresa:  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)  
  
Aqui está um exemplo de removendo o último controlador de domínio no domínio com os argumentos mínimo necessários de **- lastdomaincontrollerindomain** e **- removeapplicationpartitions**:  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)  
  
Se você tentar remover a função AD DS antes de rebaixá o servidor, o Windows PowerShell bloqueia um erro intencional:  
  
```  
Uninstall-WindowsFeature : An uninstallation prerequisite step failed duringthe removal of AD-Domain-Services, and uninstallation cannot continue.1. The domain controller needs to be demoted before the Active DirectoryDomain Services Role can be uninstalled.  
```  
  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)  
  
> [!IMPORTANT]  
> Você deve reiniciar o computador após rebaixar o servidor para poder remover os binários da função Serviços de domínio do AD.  
  
### <a name="results"></a>Resultados  
![Rebaixar DC](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)  
  
O **resultados** página mostra o sucesso ou fracasso de promoção e todas as informações administrativas importantes. O controlador de domínio é reinicializado automaticamente após 10 segundos.  
  

