---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: Gerenciamento avançado de replicação e topologia do Active Directory com o Windows PowerShell (nível 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: eeac84fb4e875ffe31b560bc72190895cd0527bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402674"
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Gerenciamento avançado de replicação e topologia do Active Directory com o Windows PowerShell (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica a nova replicação do AD DS e os cmdlets de gerenciamento de topologia em mais detalhes, e fornece exemplos adicionais. Para obter uma introdução, consulte [introdução à replicação Active Directory e gerenciamento de topologia usando &#40;o Windows&#41;PowerShell nível 100](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [Introdução](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [Replicação e metadados](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [Get-ADReplicationQueueOperation e Get-ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [Sincronizar-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [Visualização](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>Apresentações  
O Windows Server 2012 estende o módulo do Active Directory para o Windows PowerShell com vinte e cinco novos cmdlets para gerenciar replicação e topologia de floresta. Antes disso, você foi forçado a usar os substantivos **\*-ADObject** genéricos ou chamar funções .net.  
  
Como todos os cmdlets do Active Directory do Windows PowerShell, essa nova funcionalidade requer a instalação do [Serviço de gateway de gerenciamento do Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) em pelo menos um controlador de domínio (e preferencialmente, todos os controladores de domínio).  
  
A tabela a seguir lista novos cmdlets de replicação e topologia adicionados ao módulo Active Directory para Windows PowerShell.  
  
|||  
|-|-|  
|Cmdlet|Explicação|  
|Get-ADReplicationAttributeMetadata|Retorna metadados de replicação de atributos para um objeto|  
|Get-ADReplicationConnection|Retorna detalhes do objeto de conexão do controlador de domínio|  
|Get-ADReplicationFailure|Retorna a falha mais recente de replicação para um controlador de domínio|  
|Get-ADReplicationPartnerMetadata|Retorna configuração de replicação de um controlador de domínio|  
|Get-ADReplicationQueueOperation|Retorna a lista de pendências da fila de replicação atual|  
|Get-ADReplicationSite|Retorna informações do site|  
|Get-ADReplicationSiteLink|Retorna informações de link do site|  
|Get-ADReplicationSiteLinkBridge|Retorna informações de ponte de link do site|  
|Get-ADReplicationSubnet|Retorna informações de sub-rede AD|  
|Get-ADReplicationUpToDatenessVectorTable|Retorna o vetor UTD para um controlador de domínio|  
|Get-ADTrust|Retorna informações sobre uma relação de confiança entre domínios e entre florestas|  
|New-ADReplicationSite|Cria um novo site|  
|New-ADReplicationSiteLink|Cria um novo link de site|  
|New-ADReplicationSiteLinkBridge|Cria uma nova ponte de link de site|  
|New-ADReplicationSubnet|Cria uma nova sub-rede AD|  
|Remove-ADReplicationSite|Exclui um site|  
|Remove-ADReplicationSiteLink|Exclui um link de site|  
|Remove-ADReplicationSiteLinkBridge|Exclui uma ponte de link de site|  
|Remove-ADReplicationSubnet|Exclui uma sub-rede AD|  
|Set-ADReplicationConnection|Modifica uma conexão|  
|Set-ADReplicationSite|Modifica um site|  
|Set-ADReplicationSiteLink|Modifica um link de site|  
|Set-ADReplicationSiteLinkBridge|Modifica uma ponte de link de site|  
|Set-ADReplicationSubnet|Modifica uma sub-rede AD|  
|Sync-ADObject|Força a replicação de um único objeto|  
  
A maioria destes cmdlets tem sua base em Repadmin.exe. Outros cmdlets (não listados) processam recursos como Controle de Acesso Dinâmico e Contas de Serviço Gerenciado de Grupo.  
  
Para obter uma lista completa de todos os cmdlets do Active Directory para Windows PowerShell, execute:  
  
```  
Get-command -module ActiveDirectory  
```  
  
Para obter uma lista completa de todos os argumentos de cmdlets do Active Directory para Windows PowerShell, consulte a ajuda. Por exemplo:  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
Usar o cmdlet `Update-Help` para baixar e instalar arquivos de ajuda  
  
### <a name="BKMK_Repl"></a>Replicação e metadados  
Repadmin.exe valida a integridade e consistência de replicação do Active Directory. Repadmin.exe oferece opções simples de manipulação de dados – alguns argumentos dão suporte a saídas CSV, por exemplo – mas a automação geralmente exigia a análise por meio de saídas de arquivo de texto. O módulo Active Directory para o Windows PowerShell é a primeira tentativa de oferecer uma opção que permite o controle real sobre os dados retornados. Antes disso, era necessário criar scripts ou usar ferramentas de terceiros.  
  
Além disso, os seguintes cmdlets implementam um novo conjunto de parâmetros de **Target**, **Scope** e **EnumerationServer**:  
  
-   **Get-ADReplicationFailure**  
  
-   **Get-ADReplicationPartnerMetadata**  
  
-   **Get-ADReplicationUpToDatenessVectorTable**  
  
O argumento **Target** aceita uma lista separada por vírgulas de cadeias de caracteres que identificam os servidores de destino, sites, domínios ou florestas especificados pelo argumento **Scope** . Um asterisco (\*) também é permitido e significa todos os servidores dentro do escopo especificado. Se nenhum escopo for especificado, ele implica todos os servidores na floresta do usuário atual. O argumento **Scope** especifica a latitude da pesquisa. Os valores aceitáveis ​​são **Server**, **Site**, **Domain** e **Forest**. **EnumerationServer** especifica o servidor que enumera a lista de controladores de domínio especificada em **Target** e **Scope**. Funciona da mesma forma que o argumento **Server** e requer que o servidor especificado execute o serviço Web do Active Directory.  
  
Para apresentar os novos cmdlets, seguem alguns exemplos de cenários que mostram funcionalidades impossíveis para o repadmin.exe. Munidas dessas ilustrações, as possibilidades administrativas tornam-se evidentes. Examine a ajuda do cmdlet para obter requisitos específicos de uso.  
  
### <a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata  
Este cmdlet é semelhante ao **repadmin.exe /showobjmeta**. Ele permite que você retorne metadados de replicação, como quando um atributo é alterado, o controlador de domínio de origem, informações de versão e USN e dados de atributo. Esse cmdlet é útil para fiscalizar onde e quando ocorreu uma alteração.  
  
Ao contrário do Repadmin, o Windows PowerShell oferece pesquisa flexível e controle de saída. Por exemplo, você pode dar saída aos metadados do objeto de administradores de domínio, ordenados como uma lista legível:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
Você também pode organizar os dados para serem semelhantes a repadmin em uma tabela:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
Você pode também obter metadados para toda uma classe de objetos, canalizando o cmdlet **Get-Adobject** com um filtro, como todos os grupos – em seguida, combinar isso com uma data específica. O pipeline é um canal usado entre vários cmdlets para passar dados. Para ver todos os grupos modificados de alguma forma em 13 de janeiro de 2012:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
Para obter outras informações sobre mais operações do Windows PowerShell com pipelines, consulte [Canalização e pipeline no Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Como alternativa, para descobrir cada grupo que tem Tony Wang como membro e quando o grupo foi modificado pela última vez:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
Como alternativa, para encontrar todos os objetos restaurados com autoridade usando um backup do estado do sistema no domínio, com base em sua alta versão artificial:  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
Você também pode enviar todos os metadados do usuário para um arquivo CSV para exame posterior no Microsoft Excel:  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata  
Este cmdlet retorna informações sobre a configuração e o estado de replicação de um controlador de domínio, o que lhe permite monitorar, preparar inventários ou solucionar problemas. Ao contrário do Repadmin.exe, o uso do Windows PowerShell significa que você vê apenas os dados que são importantes para você, no formato desejado.  
  
Por exemplo, o estado de replicação legível de um único controlador de domínio:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
Alternativamente, a última vez que um controlador de domínio replicou entrada e seus parceiros, em um formato de tabela:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
Se preferir, entre em contato com todos os controladores de domínio na floresta e exiba qualquer tentativa de replicação com falha, por qualquer motivo:  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>Get-ADReplicationFailure  
Este cmdlet pode ser usado para retornar informações sobre erros recentes na replicação. É semelhante ao **Repadmin.exe /showreplsum**, mas, novamente, com muito mais controle, graças ao Windows PowerShell.  
  
Por exemplo, você pode retornar a maioria das falhas recentes de um controlador de domínio e os parceiros que ele não contatou:  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
Se preferir, retorne uma exibição de tabela para todos os servidores em um site lógico de AD específico, ordenado para facilitar a visualização e contendo apenas os dados mais críticos:  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>Get-ADReplicationQueueOperation e Get-ADReplicationUpToDatenessVectorTable  
Ambos os cmdlets retornam outros aspectos de atualização do controlador de domínio, que incluem replicação pendente e informações de vetor da versão.  
  
### <a name="BKMK_Sync"></a>Sincronizar-ADObject  
Este cmdlet é análogo à execução do **Repadmin.exe /replsingleobject**. É muito útil quando você faz alterações que necessitam de replicação fora da banda, especialmente para corrigir um problema.  
  
Por exemplo, se alguém excluir a conta de usuário do CEO e depois restaurá-la com a lixeira do Active Directory, provavelmente desejará replicá-la em todos os controladores de domínio imediatamente. Você provavelmente desejará fazer isso sem forçar a replicação de todas as outras alterações feitas aos objetos. Afinal, é por isso que você tem uma programação de replicação – para evitar sobrecarregar links de WAN.  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>Visualização  
Enquanto o Repadmin.exe é bom para retornar informações sobre topologia de replicação como sites, links de site, pontes de link de site e conexões, ele não tem um conjunto abrangente de argumentos para fazer alterações. Na verdade, nunca houve um utilitário nativo, programável do Windows, projetado especificamente para criação e modificação de topologia AD DS pelos administradores. Como o Active Directory amadureceu em milhões de ambientes de cliente, a necessidade de modificar em massa as informações lógicas do Active Directory torna-se aparente.  
  
Por exemplo, depois de uma rápida expansão de novas filiais, em conjunto com a consolidação de outras, você pode ter uma centena de alterações de site por fazer com base em locais físicos, mudanças de rede e novos requisitos de capacidade. Em vez de usar Dssites.msc e Adsiedit.msc para fazer alterações, você pode automatizar. Isso é especialmente atraente quando você começa com uma planilha de dados fornecida pela rede e pelas equipes.  
  
Os cmdlets **Get-Adreplication\\** * retornam informações sobre a topologia de replicação e são úteis para o pipeline nos cmdlets **Set-Adreplication\\** * em massa. Os cmdlets **Get** não alteram dados, apenas mostram dados ou criam objetos de sessão do Windows PowerShell que podem ser canalizados para cmdlets **Set-Adreplication\\** *. Os cmdlets **New** e **Remove** são úteis para criar ou remover objetos de topologia do Active Directory.  
  
Por exemplo, você pode criar novos sites usando um arquivo CSV:  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
Você pode também criar um novo link de site entre dois sites existentes, com um intervalo de replicação personalizado e custo do site:  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
Como alternativa, encontre todos os sites na floresta e substitua seus atributos de **Opções** pelo sinalizador para permitir notificação de alteração entre sites a fim de replicar na velocidade máxima com compactação:  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> Defina **-bor 5** para desabilitar a compactação nos links do site também.  
  
Se preferir, encontre todos os sites sem atribuição de sub-redes para conciliar a lista com as sub-redes reais desses locais:  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![gerenciamento avançado com o PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>Consulte também  
[Introdução à replicação Active Directory e ao gerenciamento de topologia usando &#40;o Windows PowerShell nível 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

