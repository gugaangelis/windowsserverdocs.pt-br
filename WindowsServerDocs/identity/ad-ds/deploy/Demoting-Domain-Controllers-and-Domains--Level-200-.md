---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: Rebaixando controladores de domínio e domínios (nível 200)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7fc5b8b2f29c0eee2f11f2b581e6ccdd56635236
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954288"
---
# <a name="demoting-domain-controllers-and-domains"></a>Rebaixando controladores de domínio e domínios

>Aplica-se a: Windows Server

Este tópico explica como remover o AD DS usando o Gerenciador do Servidor ou o Windows PowerShell.
  
## <a name="ad-ds-removal-workflow"></a>Fluxo de trabalho de remoção de AD DS

![Gráfico de fluxo de trabalho de remoção de AD DS](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  

> [!CAUTION]
> Remover as funções AD DS com Dism.exe ou o módulo DISM do Windows PowerShell após a promoção para um Controlador de Domínio não é possível e impedirá que o servidor seja reinicializado normalmente.
>
> Diferente do Gerenciador do Servidor ou do módulo de implantação do AD DS para Windows PowerShell, o DISM é um sistema de instalação nativo que não possui conhecimento inerente de AD DS ou de sua configuração. Não use Dism.exe ou o módulo do DISM do Windows PowerShell para desinstalar a função AD DS, a menos que o servidor não seja mais um controlador de domínio.

## <a name="demotion-and-role-removal-with-powershell"></a>Remoção de rebaixamento e função com o PowerShell

|||  
|-|-|  
|**Cmdlets do ADDSDeployment e ServerManager**|Argumentos (os argumentos em **Negrito** são necessários. Os argumentos em *Itálico* podem ser especificados usando o Windows PowerShell ou o Assistente de Configuração do AD DS.)|  
|Uninstall-ADDSDomainController|-SkipPreChecks<p>*-LocalAdministratorPassword*<p>-Confirm<p>***-Credential***<p>-DemoteOperationMasterRole<p>*-DNSDelegationRemovalCredential*<p>-Force<p>*-ForceRemoval*<p>*-IgnoreLastDCInDomainMismatch*<p>*-IgnoreLastDNSServerForZone*<p>*-LastDomainControllerInDomain*<p>-Norebootoncompletion<p>*-RemoveApplicationPartitions*<p>*-RemoveDNSDelegation*<p>-RetainDCMetadata|  
|Uninstall-WindowsFeature/Remove-WindowsFeature|***-Nome***<p>***-IncludeManagementTools***<p>*-Reiniciar*<p>-Remove<p>-Force<p>-ComputerName<p>-Credential<p>-LogPath<p>-Vhd|  
  
> [!NOTE]  
> O argumento **-credential** somente é necessário se você ainda não tiver entrado como membro do grupo Administradores de Empresa (rebaixando o último DC em um domínio) ou o grupo Admins. do Domínio (rebaixando um DC de réplica). O argumento **-includemanagementtools** é necessário somente se você também desejar remover todos os utilitários de gerenciamento do AD DS.  
  
## <a name="demote"></a>Rebaixar  
  
### <a name="remove-roles-and-features"></a>Remover funções e recursos

O Gerenciador do Servidor oferece duas interfaces para remover a função do Serviços de Domínio Active Directory:  
  
* O menu **Gerenciar** no dashboard, usando **Remover funções e recursos**  

   ![Gerenciador do Servidor-remover funções e recursos](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  

* Clique em **AD DS** ou **Todos os servidores** no painel de navegação. Arraste para baixo até a seção **Função e recursos**. Clique com o botão direito do mouse em **Serviços de Domínio Active Directory** na lista **Funções e recursos** e clique em **Remover funções e recursos**. Esta interface ignora a página **Seleção do servidor**.  

   ![Gerenciador do Servidor-todos os servidores – remover funções e recursos](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  

Os cmdlets do ServerManager **Uninstall-WindowsFeature** e **Remove-WindowsFeature** impedirão que você remova a função de AD DS até rebaixar o controlador de domínio.
  
### <a name="server-selection"></a>Seleção do servidor

![Assistente para remover funções e recursos selecionar servidor de destino](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  

O diálogo **Seleção do Servidor** permite que você escolha um dos servidores previamente adicionados ao pool, desde que ele esteja acessível. O servidor local que executa o Gerenciador do Servidor sempre está disponível automaticamente.  

### <a name="server-roles-and-features"></a>Funções e recursos do servidor

![Assistente para remover funções e recursos – selecionar funções a serem removidas](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  

Desmarque a caixa de seleção **Serviços de Domínio Active Directory** para rebaixar um controlador de domínio; se o servidor for um controlador de domínio no momento, isso não remove a função do AD DS, mas sim alterna para uma caixa de diálogo **Resultados de validação** que apresenta a opção de rebaixamento. Caso contrário, ele remove os binários como qualquer outro recurso de função.  

* Não remova ou adicione outras funções ou recursos relacionados ao AD DS, como DNS, GPMC ou as ferramentas RSAT, se pretender promover o controlador de domínio novamente imediatamente. Remover as funções e recursos adicionais eleva o tempo necessário para a nova promoção, pois o Gerenciador do Servidor reinstala esses recursos ao reinstalar a função.  
* Remova as funções e recursos desnecessários do AD DS livremente se você pretende manter o controlador de domínio rebaixado permanentemente. Isso requer desmarcar todas as caixas de seleção para as funções e recursos em questão.  

   A lista completa de funções e recursos relacionados ao AD DS inclui:  
  
   * Recurso do módulo Active Directory para o Windows PowerShell  
   * Recurso AD DS e AD LDS  
   * Recurso do Centro Administrativo do Active Directory  
   * Recurso de snap-ins AD DS e ferramentas de linha de comando  
   * Servidor DNS  
   * Console de gerenciamento de política de grupo  
  
Os cmdlets equivalentes do ADDSDeployment e ServerManager do Windows PowerShell são:  
  
```
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
```

![Assistente para remover funções e recursos-caixa de diálogo de confirmação](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  

![Assistente para remover funções e recursos-validação](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  

### <a name="credentials"></a>Credenciais

![Assistente de configuração de Active Directory Domain Services-seleção de credenciais](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  

Você pode configurar opções de rebaixamento na página **Credenciais**. Forneça as credenciais necessárias à execução do rebaixamento na lista a seguir:  

* O rebaixamento de um controlador de domínio adicional exige credenciais de Admin. do Domínio. Selecionar **forçar a remoção deste controlador de domínio** rebaixa o controlador de domínio sem remover os metadados do objeto do controlador de domínio do Active Directory.  

   > [!WARNING]  
   > Não selecione essa opção, a menos que o controlador de domínio não possa contatar outros controladores de domínio e *não haja outra maneira razoável* de solucionar o problema de rede. O rebaixamento forçado resulta em metadados órfãos no Active Directory, nos controladores de domínio remanescentes na floresta. Além disso, todas as alterações que não foram replicadas nesse controlador de domínio, como senhas ou novas contas de usuário, serão perdidas definitivamente. Metadados órfãos são a principal causa da significativa porcentagem de casos no Atendimento ao Cliente da Microsoft para AD DS, Exchange, SQL e outros softwares.  
   >
   > Se você forçar o rebaixamento de um controlador de domínio, *execute* manual e imediatamente a limpeza dos metadados. Para conhecer as etapas, examine [Limpar metadados do servidor](ad-ds-metadata-cleanup.md).  

   ![Assistente de configuração de Active Directory Domain Services – remoção de credenciais de força](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
* O rebaixamento do último controlador de domínio em um domínio exige associação ao grupo Administradores de Empresa, pois esse processo remove o domínio propriamente dito (e se este for o último domínio da floresta, ele removerá a floresta). O Gerenciador do Servidor informará se o atual controlador de domínio for o último no domínio. Marque a caixa de seleção **Último controlador de domínio no domínio** para confirmar que o controlador de domínio é o último controlador de domínio no domínio.  

Os argumentos equivalentes de ADDSDeployment do Windows PowerShell são:  

```
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
```

### <a name="warnings"></a>Avisos

![Assistente de configuração de Active Directory Domain Services-impacto das funções FSMO de credenciais](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  

A página **Avisos** alerta sobre as possíveis consequências de remover este controlador de domínio. Para continuar, selecione **Prosseguir com a remoção**.

> [!WARNING]  
> Se **Forçar a remoção deste controlador de domínio** na página **Credenciais** tiver sido marcado anteriormente, a página **Avisos** mostrará as funções de Operações de mestre único flexível hospedadas por este controlador de domínio. Você *deverá* executar as funções de outro controlador de domínio *imediatamente* após rebaixar este servidor. Para obter mais informações sobre como executar funções FSMO, consulte [Capturar a função de mestre de operações](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816779(v=ws.10)).

Esta página não possui um argumento equivalente para o ADDSDeployment no Windows PowerShell.

### <a name="removal-options"></a>Opções de remoção

![Assistente de configuração do Active Directory Domain Services-credenciais remover partições de DNS e de aplicativo](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  

A página **Opções de remoção** será exibida se o **Último controlador de domínio no domínio** tiver sido selecionado na página **Credenciais**. Esta página permite configurar as opções adicionais de remoção. Selecione **ignorar o último servidor DNS para a zona**, **remover partições de aplicativo**e **remover delegação de DNS** para habilitar o botão **Avançar** .

As opções somente aparecerão se forem aplicáveis a este controlador de domínio. Por exemplo, se não houver delegação de DNS para este servidor, a caixa de seleção em questão não será exibida.

Clique em **Alterar** para especificar credenciais administrativas de DNS alternativas. Clique em **Ver partições** para ver as partições adicionais que o assistente remove durante o rebaixamento. Por padrão, as únicas partições adicionais são DNS de domínio e Zonas de DNS de floresta. Todas as demais partições não pertencem ao Windows.

Os argumentos de cmdlet equivalentes do ADDSDeployment são:  

```
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```

### <a name="new-administrator-password"></a>Nova Senha do Administrador

![Assistente de configuração do Active Directory Domain Services-credenciais de nova senha de administrador](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  

A página **nova senha do administrador** exige que você forneça uma senha para a conta de administrador do computador local, depois que o rebaixamento for concluído e o computador se tornar um servidor membro do domínio ou um computador do grupo de trabalho.

Os cmdlets e os argumentos **Uninstall-ADDSDomainController** seguirão os mesmos padrões do Gerenciador do Servidor, se não for especificado.

O argumento **LocalAdministratorPassword** é especial:

* Se *não especificado* como um argumento, o cmdlet solicitará que você insira e confirme uma senha mascarada. Este é o uso preferencial ao executar o cmdlet interativamente.
* Se *com um valor* for especificado, o valor deverá ser uma cadeia de caracteres segura. Este não é o uso preferencial ao executar o cmdlet interativamente.

Por exemplo, você pode solicitar manualmente uma senha usando o cmdlet **Read-Host** para solicitar ao usuário uma cadeia de caracteres segura.

```
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> Como as duas opções anteriores não confirmam a senha, tenha muito cuidado: a senha não está visível.

Você também pode fornecer uma cadeia de caracteres segura como uma variável de texto não criptografado convertida, embora seja altamente recomendável não fazer isso. Por exemplo:

```
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)
```

> [!WARNING]
> O fornecimento ou o armazenamento de uma senha com texto não criptografado não é recomendável. Qualquer pessoa que executar esse comando em um script ou que estiver por perto tomará conhecimento da senha do administrador local daquele computador. Sabendo disto, ela terá acesso a todos os seus dados e poderá passar-se pelo próprio servidor.

### <a name="confirmation"></a>Confirmação

![Assistente de configuração do Active Directory Domain Services – opções de revisão](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)

A página **Confirmação** mostra o rebaixamento planejado. Ela não lista as opções de configuração de rebaixamento. Esta é a última página mostrada pelo assistente antes do início do rebaixamento. O botão Ver script cria um script de rebaixamento do Windows PowerShell.

Clique em **Rebaixar** para executar o seguinte cmdlet de Implantação do AD DS:

```
Uninstall-ADDSDomainController
```

Use o argumento opcional **Whatif** com o **Uninstall-ADDSDomainController** e o cmdlet para examinar as informações da configuração. Isso permite que você veja os valores explícitos e implícitos de argumento de um cmdlet.

Por exemplo:

![Desinstalação do PowerShell-exemplo de ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)

o prompt para reiniciar é sua última oportunidade de cancelar esta operação ao usar o ADDSDeployment no Windows PowerShell. Para substituir este prompt, use os argumentos **-force** ou **confirm:$false**.  

### <a name="demotion"></a>Rebaixamento

![Assistente de configuração de Active Directory Domain Services-rebaixamento em andamento](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  

Quando a página **Rebaixamento** é exibida, a configuração do controlador de domínio começa e não pode ser interrompida ou cancelada. As operações detalhadas são exibidas nesta página e gravadas em logs:  

* %systemroot%\debug\dcpromo.log
* %systemroot%\debug\dcpromoui.log

Como o **Uninstall-ADDSDomainController** e o **Uninstall-WindowsFeature** têm apenas uma ação cada, eles são mostrados aqui na fase de confirmação com os argumentos mínimos necessários. Pressionar ENTER inicia o processo irrevogável de rebaixamento e reinicia o computador.

![Desinstalação do PowerShell-exemplo de ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)

![Desinstalação do PowerShell-exemplo de WindowsFeature](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)

Para aceitar o prompt de reinicialização automática, use os argumentos **-force** ou **-confirm:$false** com qualquer cmdlet ADDSDeployment do Windows PowerShell. Para evitar que o servidor reinicie automaticamente no final da promoção, use o argumento **-norebootoncompletion:$false**.

> [!WARNING]
> Não é recomendável substituir a reinicialização. O servidor membro deve ser reiniciado para funcionar corretamente.

![Desinstalação do PowerShell-exemplo de força ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)

Aqui está um exemplo de rebaixamento forçado com o mínimo necessário de argumentos **-forceremoval** e **-demoteoperationmasterrole**. O argumento **-credential** não é necessário porque o usuário fez logon como membro do grupo Administradores Corporativos:

![Desinstalação do PowerShell-exemplo de ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)

Aqui está um exemplo de remoção do último controlador de domínio, no domínio, usando o mínimo necessário de argumentos **-lastdomaincontrollerindomain** e **-removeapplicationpartitions**:

![Desinstalação do PowerShell – exemplo de ADDSDomainController-LastDomainControllerInDomain](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)

Se você tentar remover a função de AD DS antes de rebaixar o servidor, o Windows PowerShell o bloqueará com um erro:

![Uma etapa de pré-requisito de desinstalação falhou durante a remoção do AD-Domain-Services, e a desinstalação não pode continuar. 1. O controlador de domínio precisa ser rebaixado para que a função active DirectoryDomain Services possa ser desinstalada.](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)

> [!IMPORTANT]
> O computador deverá ser reiniciado depois de rebaixar o servidor antes de ser possível remover os binários de função do AD DS.

### <a name="results"></a>Resultados

![Você está prestes a ser desconectado do aviso após a remoção do AD DS](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)

A página **Resultados** mostra o sucesso ou o fracasso da promoção e qualquer informação administrativa importante. O controlador de domínio reiniciará automaticamente após 10 segundos.
