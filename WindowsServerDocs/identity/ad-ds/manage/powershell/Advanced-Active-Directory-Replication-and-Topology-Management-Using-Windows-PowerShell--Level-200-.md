---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: "Advanced replicação do Active Directory e gerenciamento de topologia usando o Windows PowerShell (nível 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1e05616b4b594ae54fcaa3ec6496c0917ecde38b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Advanced replicação do Active Directory e gerenciamento de topologia usando o Windows PowerShell (nível 200)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica a nova replicação do AD DS e cmdlets de gerenciamento de topologia mais detalhadamente e fornece exemplos adicionais. Para obter uma introdução, consulte [Introdução a replicação do Active Directory e o gerenciamento de topologia usando o Windows PowerShell e 40; Nível de 100 & #41; ](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [Introdução](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [Replicação e metadados](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [Get-ADReplicationQueueOperation e Get-ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [Sincronizar ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [Topologia](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>Introdução  
Windows Server 2012 estende o módulo do Active Directory para Windows PowerShell com 25 novos cmdlets para gerenciar a replicação e topologia de floresta. Antes disso, você foi forçado a usar o genérico **\*-AdObject** substantivos ou chamada .NET funções.  
  
Como todos os cmdlets do Active Directory do Windows PowerShell, essa nova funcionalidade requer a instalação do [serviço de Gateway de gerenciamento do Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) pelo menos um controlador de domínio (e preferencialmente, todos os controladores de domínio).  
  
A tabela a seguir lista nova replicação e cmdlets topologia adicionados ao módulo do Active Directory do Windows PowerShell.  
  
|||  
|-|-|  
|Cmdlet|Explicação|  
|Get-ADReplicationAttributeMetadata|Retorna metadados de replicação de um objeto de atributo|  
|Get-ADReplicationConnection|Retorna detalhes do objeto de conexão do controlador de domínio|  
|Get-ADReplicationFailure|Retorna o mais recente Falha na replicação para um controlador de domínio|  
|Get-ADReplicationPartnerMetadata|Retorna a configuração de replicação de um controlador de domínio|  
|Get-ADReplicationQueueOperation|Retorna a lista de pendências de fila de replicação atual|  
|Get-ADReplicationSite|Retorna informações do site|  
|Get-ADReplicationSiteLink|Retorna informações de link do site|  
|Get-ADReplicationSiteLinkBridge|Retorna informações de ponte de link de site|  
|Get-ADReplicationSubnet|Retorna informações de sub-rede de anúncios|  
|Get-ADReplicationUpToDatenessVectorTable|Retorna o vetor de UTD para um controlador de domínio|  
|Get-ADTrust|Retorna informações sobre uma relação de confiança entre domínios ou entre florestas|  
|Novo ADReplicationSite|Cria um novo site|  
|Novo ADReplicationSiteLink|Cria um novo link de site|  
|Novo ADReplicationSiteLinkBridge|Cria uma nova ponte de link de site|  
|Novo ADReplicationSubnet|Cria uma nova sub-rede AD|  
|Remover ADReplicationSite|Exclui um site|  
|Remover ADReplicationSiteLink|Exclui um link de site|  
|Remover ADReplicationSiteLinkBridge|Exclui uma ponte de link de site|  
|Remover ADReplicationSubnet|Exclui uma sub-rede AD|  
|Conjunto ADReplicationConnection|Modifica uma conexão|  
|Conjunto ADReplicationSite|Modifica um site|  
|Conjunto ADReplicationSiteLink|Modifica um link de site|  
|Conjunto ADReplicationSiteLinkBridge|Modifica uma ponte de link de site|  
|Conjunto ADReplicationSubnet|Modifica uma sub-rede AD|  
|Sincronizar ADObject|Replicação de força de um único objeto|  
  
A maioria desses cmdlets tem sua base em Repadmin.exe. Outros cmdlets (não listados) lidar com recursos como controle de acesso dinâmico e contas de serviço gerenciado do grupo.  
  
Para obter uma lista completa de todos os cmdlets do Active Directory do Windows PowerShell, execute:  
  
```  
Get-command -module ActiveDirectory  
```  
  
Para uma lista completa de todos os argumentos do Active Directory Windows PowerShell cmdlet, consulte a Ajuda. Por exemplo:  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
Use o `Update-Help` cmdlet para baixar e instalar arquivos de ajuda  
  
### <a name="BKMK_Repl"></a>Replicação e metadados  
Repadmin.exe valida a integridade e a consistência da replicação do Active Directory. Repadmin.exe oferece opções de manipulação de dados simples - alguns argumentos suportam saídas CSV, por exemplo - mas automação geralmente necessário análise saídas de arquivo de texto. O módulo do Active Directory para Windows PowerShell é a primeira tentativa de oferecer uma opção que permite que o controle real sobre os dados retornados; Antes disso, você precisava criar scripts ou usar ferramentas de terceiros.  
  
Além disso, os seguintes cmdlets implementar um novo conjunto de parâmetro de **destino**, **escopo**, e **EnumerationServer**:  
  
-   **Get-ADReplicationFailure**  
  
-   **Get-ADReplicationPartnerMetadata**  
  
-   **Get-ADReplicationUpToDatenessVectorTable**  
  
O **destino** argumento aceita uma lista separada por vírgulas de cadeias de caracteres que identificam os servidores de destino, sites, domínios ou florestas especificadas pelo **escopo** argumento. Um asterisco (\ *) também é permitido e significa que todos os servidores dentro do escopo especificado. Se nenhum escopo for especificado, ele implica em todos os servidores na floresta do usuário atual. O **escopo** argumento especifica a latitude da pesquisa. Os valores aceitáveis são **servidor**, **Site**, **domínio**, e **floresta**. O **EnumerationServer** Especifica o servidor que enumera a lista de controladores de domínio especificado no **destino** e **escopo**. Ele funciona da mesma maneira como o **servidor** argumento e requer que o servidor especificado executar o serviço de Web do Active Directory.  
  
Para introduzir novos cmdlets, aqui estão alguns cenários de exemplo mostrando recursos impossíveis para repadmin.exe; as possibilidades administrativas contando com estas ilustrações, fica evidente. Examine a Ajuda do cmdlet para requisitos de uso específicos.  
  
### <a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata  
Este cmdlet é semelhante ao **repadmin.exe /showobjmeta**. Ele permite que você retorne metadados de replicação, como quando um atributo alterado, o controlador de domínio de origem, a versão e informações de USN e dados do atributo. Este cmdlet é útil para auditoria onde e quando uma alteração ocorreu.  
  
Diferentemente Repadmin, pesquisa flexível do Windows PowerShell oferece e saída de controle. Por exemplo, você pode gerar os metadados do objeto Admins. do domínio, ordenado como uma lista legível:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
Como alternativa, você pode organizar os dados como repadmin, em uma tabela:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
Como alternativa, você pode obter metadados para uma classe inteira de objetos, por pipelining o **Get-Adobject** cmdlet com um filtro, como todos os grupos - combine que, em seguida, com uma data específica. O pipeline é um canal usado entre vários cmdlets para transmitir dados. Para ver todos os grupos modificados de alguma forma em 13 de janeiro de 2012:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
Para saber mais sobre mais operações do Windows PowerShell com pipelines, consulte [Piping e o Pipeline no Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Como alternativa, para descobrir cada grupo que possui Tony Wang como um membro e o grupo foi modificado pela última vez:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
Como alternativa localizar todos os objetos com autoridade restauradas usando um backup do estado do sistema no domínio, com base na sua versão artificialmente alto:  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
Como alternativa, envie todos os metadados de usuário para um arquivo CSV para análise posterior no Microsoft Excel:  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata  
Este cmdlet retorna informações sobre a configuração e o estado de replicação para um controlador de domínio, permitindo que você monitore, inventariar ou solucionar problemas. Ao contrário de Repadmin.exe, usando o Windows PowerShell significa que você vê apenas os dados que são importantes para você, no formato desejado.  
  
Por exemplo, o estado de replicação legível de um controlador de domínio único:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
Como alternativa, a última vez em que um controlador de domínio replicado entrada e formatar seus parceiros, em uma tabela:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
Como alternativa, entre em contato com todos os controladores de domínio na floresta e exibir qualquer cuja última replicação tentativa falhou por qualquer motivo:  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>Get-ADReplicationFailure  
Este cmdlet pode ser usado para retorna informações sobre erros recentes na replicação. Ele é análogo ao **Repadmin.exe /showreplsum**, mas novamente, com muito mais controlam graças ao Windows PowerShell.  
  
Por exemplo, você pode retornar falhas mais recentes do controlador de domínio e os parceiros que ele falha ao entrar em contato com:  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
Como alternativa, retorne um modo de exibição de tabela para todos os servidores em um anúncio lógico site específico, ordenado para exibir mais fácil e contendo apenas os dados mais importantes:  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>Get-ADReplicationQueueOperation e Get-ADReplicationUpToDatenessVectorTable  
Esses dois cmdlets retorna mais aspectos do controlador de domínio "até dateness", que inclui pendentes replication e informações de vetor de versão.  
  
### <a name="BKMK_Sync"></a>Sincronizar ADObject  
Este cmdlet é análoga a execução **Repadmin.exe /replsingleobject**. É muito útil quando você faz alterações que exigem de replicação de banda, especialmente para corrigir um problema.  
  
Por exemplo, se alguém excluído conta de usuário do CEO e restaurados com o Active Directory Lixeira, provavelmente você desejará que ele replicada para todos os controladores de domínio imediatamente. Você provavelmente também queira fazer isso sem forçar replicação de todas as outras alterações de objeto feita; Afinal, é por isso que você tem um agendamento de replicação - para evitar a sobrecarga links WAN.  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>Topologia  
Enquanto Repadmin.exe é bom para retornar informações sobre a topologia de replicação como sites, links de site, pontes e conexões, ele não tem um conjunto abrangente de argumentos para fazer alterações. Na verdade, nunca houve programável, na caixa utilitário do Windows projetado especificamente para os administradores criar e modificar a topologia do AD DS. Como o Active Directory tem se converteu em milhões de ambientes de cliente, a necessidade de em massa modificar Active Directory informações lógicas ficam evidente.  
  
Por exemplo, após uma rápida expansão de filiais novo, combinada com a consolidação de terceiros, você pode ter centenas de mudanças de site para fazer com base em locais físicos, alterações de rede e novos requisitos de capacidade. Em vez de usar Dssites.msc e Adsiedit.msc para fazer alterações, você pode automatizar. Isso é especialmente atraente quando você começa com uma planilha de dados fornecidos por suas equipes de recursos e rede.  
  
O **obter-Adreplication\ *** cmdlets retornar informações sobre a topologia de replicação e são úteis para pipelining no **definir-Adreplication\ *** cmdlets em massa. **Obter** cmdlets não alteram dados, eles apenas mostram dados ou para criar o Windows PowerShell sessão objetos que pode ser o em pipeline para **definir-Adreplication\ *** cmdlets. O **nova** e **remover** cmdlets são úteis para criar ou removendo objetos de topologia do Active Directory.  
  
Por exemplo, você pode criar novos sites usando um arquivo CSV:  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
Como alternativa, crie um novo link de site entre dois locais existentes com um custo de intervalo e o site de replicação personalizado:  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
Como alternativa, localizar todos os sites na floresta e substitua seus **opções** atributos com o sinalizador para habilitar entre sites alterar notificação, para replicar com a velocidade máxima com compactação:  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> Defina **- bor 5** para desativar a compactação nesses links de site também.  
  
Como alternativa, localize todos os sites ausentes atribuições de sub-rede, para reconciliar a lista com as sub-redes reais desses locais:  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![gerenciamento avançado com o powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>Consulte também  
[Introdução à replicação do Active Directory e gerenciamento de topologia usando o Windows PowerShell & #40; Nível de 100 & #41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

