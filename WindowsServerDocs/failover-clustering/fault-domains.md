---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: Reconhecimento de domínio de falha
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: 18b7a932cc8a22c356fde89baa316c0532ebc374
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475998"
---
# <a name="fault-domain-awareness"></a>Reconhecimento de domínio de falha

> Aplica-se a: Windows Server 2019 e Windows Server 2016

Clustering de failover permite que vários servidores funcionem juntos para fornecer alta disponibilidade ou, em outras palavras, para fornecer tolerância a falhas de nó. Mas as empresas de hoje exigem disponibilidade cada vez maior de suas infraestrutura. Para conseguir um tempo de atividade parecido com o da nuvem, é necessário se proteger até mesmo de ocorrências altamente improváveis como desastres naturais, interrupções de rack ou falhas de chassi. É por isso que o Clustering de Failover no Windows Server 2016 introduziu o chassi, rack e site a tolerância a falhas também.

## <a name="fault-domain-awareness"></a>Reconhecimento de domínio de falha

Domínios de falha e tolerância a falhas são conceitos bem próximos. Um domínio de falha é um conjunto de componentes de hardware que compartilham um único ponto de falha. Para ser tolerante a falhas em um determinado nível, você precisa de vários domínios de falha nesse nível. Por exemplo, para ser rack tolerante a falhas do rack, seus servidores e seus dados devem ser distribuídos entre vários racks.

Este breve vídeo apresenta uma visão geral de domínios de falha no Windows Server 2016:  
[![Clique nessa imagem para assistir a uma visão geral de domínios de falha no Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

### <a name="fault-domain-awareness-in-windows-server-2019"></a>Reconhecimento de domínio de falha no Windows Server 2019

Reconhecimento de domínio de falha está disponível no Windows Server 2019, mas é desabilitado por padrão e deve ser habilitado por meio do registro do Windows.

Para habilitar o reconhecimento de domínio de falha no Windows Server 2019, vá para o registro do Windows e definir o (Get-Cluster). Chave do registro AutoAssignNodeSite como 1.

```Registry
    (Get-Cluster).AutoAssignNodeSite=1
```

Para desabilitar o reconhecimento de domínio de falha no Windows de 2019, vá para o registro do Windows e defina o (Get-Cluster). Chave do registro AutoAssignNodeSite como 0.

```Registry
    (Get-Cluster).AutoAssignNodeSite=0
```

## <a name="benefits"></a>Benefícios
- **Espaços de armazenamento, incluindo espaços de armazenamento diretos, usam domínios de falha para maximizar a segurança dos dados.**  
    A resiliência nos Espaços de armazenamento é, conceitualmente, parecida com RAID distribuída e definida pelo software. Várias cópias de todos os dados são mantidas em sincronia, e se o hardware falhar e uma cópia for perdida, outros são copiados novamente para restaurar a resiliência. Para obter a máxima resiliência possível, as cópias devem ser mantidas em domínios de falha separados.

- **O [serviço de integridade](health-service-overview.md) usa domínios para fornecer alertas mais úteis de falha.**  
    Cada domínio de falha pode ser associado aos metadados de local, que serão incluído automaticamente em todos os alertas subsequentes. Esses descritores podem ajudar a equipe de manutenção ou de operações e reduzir os erros por desambiguação de hardware.  

- **O cluster estendido usa domínios de falha para afinidade de armazenamento.** O cluster estendido permite que servidores distantes ingressem em um cluster comum. Para obter o melhor desempenho, os aplicativos ou máquinas virtuais devem ser executadas em servidores que estão próximos àqueles que fornecem seu armazenamento. Reconhecimento de domínio de falha permite que a afinidade de armazenamento.   

## <a name="levels-of-fault-domains"></a>Níveis de domínios de falha  
Há quatro níveis canônicos de domínios de falha - site, rack, chassi e nó. Os nós são descobertos automaticamente; cada nível adicional é opcional. Por exemplo, se sua implantação não usar servidores blade, talvez o nível de chassi não faça sentido para você.  

![Diagrama dos diferentes níveis de domínios de falha](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Uso  
Você pode usar a marcação XML ou do PowerShell para especificar os domínios de falha. Ambas as abordagens são equivalentes e fornecem a funcionalidade completa.

>[!IMPORTANT]
> Especifique os domínios de falha antes de habilitar Espaços de Armazenamento Diretos, se for possível. Isso permite que a configuração automática prepare o pool, as camadas e as configurações para resiliência e número de colunas, para tolerância a falhas de chassi ou rack. Após a criação do pool e dos volumes, os dados não se movimentarão de retroativa em resposta às alterações na topologia do domínio de falha. Para mover nós entre chassis ou racks depois de habilitar os Espaços de Armazenamento Diretos, você deve remover primeiro o nó e suas unidades do pool usando `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Definir domínios de falha com o PowerShell
Windows Server 2016 apresenta os seguintes cmdlets para trabalhar com domínios de falha:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Este breve vídeo demonstra o uso desses cmdlets.
[![Clique nessa imagem para assistir a um vídeo curto sobre o uso dos cmdlets do domínio de falha do Cluster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Use `Get-ClusterFaultDomain` para ver a topologia de domínio de falha atual. Isso listará todos os nós no cluster, além de qualquer chassi, rack ou site que você criou. Você pode filtrar usando parâmetros como **-Type** ou **-Name**, mas isso não é obrigatório.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Use `New-ClusterFaultDomain` para criar novos chassis, racks ou sites. O `-Type` e `-Name` parâmetros são obrigatórios. Os valores possíveis para `-Type` estão `Chassis`, `Rack`, e `Site`. O `-Name` pode ser qualquer cadeia de caracteres. (Para `Node` domínios de falha de tipo, o nome devem ser o nome real do nó, conforme definido automaticamente).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server não pode e não verifica que os domínios de falha, que você cria correspondem a qualquer coisa do mundo real, físico. (Isso pode parecer óbvio, mas é importante entender). Se, no mundo físico, seus nós estão todos em um rack, a criação de dois domínios de falha `-Type Rack` no software não fornece tolerância a falhas de rack como por mágica. Você é responsável por garantir a topologia criada por você usando esses cmdlets corresponda à organização real de seu hardware.

Use `Set-ClusterFaultDomain` para mover um domínio de falha para outro. Os termos "parent" (pai) e "child" (filho) são usados normalmente para descrever essa relação de aninhamento. O `-Name` e `-Parent` parâmetros são obrigatórios. Na `-Name`, forneça o nome do domínio de falha que está sendo movimentado; em `-Parent`, forneça o nome do destino. Para mover vários domínios de falha ao mesmo tempo, liste seus nomes.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Ao mover domínios de falha, seus filhos os acompanham. No exemplo acima, se um Rack A é o pai do server01.contoso.com, o último não precisa ser movido separadamente para o site Xangai – ele já existirá em virtude de seu pai, assim como no mundo físico.

Você pode ver as relações pai-filho na saída do `Get-ClusterFaultDomain`, no `ParentName` e `ChildrenNames` colunas.

Você também pode usar `Set-ClusterFaultDomain` para modificar outras propriedades de domínios de falha. Por exemplo, você pode fornecer opcional `-Location` ou `-Description` metadados para qualquer domínio de falha. Se forem fornecidas, essas informações serão incluídas no alerta de hardware do Serviço de Integridade. Você também pode renomear domínios de falha usando o `-NewName` parâmetro. Não renomeie `Node` digite domínios de falha.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Use `Remove-ClusterFaultDomain` para remover o chassi, rack ou site que você criou. O parâmetro `-Name` é necessário. Você não pode remover um domínio de falha que contém filhos – primeiro, remova os filhos ou mova-os para fora usando `Set-ClusterFaultDomain`. Para mover um domínio de falha fora de todos os outros domínios de falha, defina seu `-Parent` na cadeia de caracteres vazia (""). Não é possível remover `Node` digite domínios de falha. Para remover vários domínios de falha ao mesmo tempo, liste seus nomes.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Definir domínios de falha com marcação XML
Os domínios de falha podem ser especificados usando uma sintaxe inspirada em XML. Recomendamos o uso de seu editor de texto favorito, como o Visual Studio Code (disponível gratuitamente *[aqui](https://code.visualstudio.com/)* ) ou o Bloco de Notas, para criar um documento XML que você pode salvar e reutilizar.  

Este breve vídeo demonstra o uso de Marcação XML para especificar os domínios de falha.

[![Clique nessa imagem para assistir a um breve vídeo sobre como usar XML para especificar os domínios de falha](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

No PowerShell, execute o seguinte cmdlet: `Get-ClusterFaultDomainXML`. Isso retorna a especificação de domínio de falha atual para o cluster, como XML. Isso reflete todo descoberto `<Node>`encapsulada de abertura e fechamento `<Topology>` marcas.  

Execute o seguinte para salvar essa saída em um arquivo.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Abra o arquivo e adicione `<Site>`, `<Rack>`, e `<Chassis>` marcas para especificar como esses nós são distribuídos entre sites, racks e chassi. Cada marca deve ser identificada por um único **Nome**. Para os nós, você deve manter o nome padrão do nó.  

> [!IMPORTANT]  
> Embora todas as marcas adicionais sejam opcionais, elas devem aderir à hierarquia temporária de Site &gt; Rack &gt; Chassi &gt; Nó, e devem ser fechadas corretamente.  
Além do nome, de forma livre `Location="..."` e `Description="..."` descritores podem ser adicionados a qualquer marca.  

#### <a name="example-two-sites-one-rack-each"></a>Exemplo: Dois sites, um rack  

```XML
<Topology>  
  <Site Name="SEA" Location="Contoso HQ, 123 Example St, Room 4010, Seattle">  
    <Rack Name="A01" Location="Aisle A, Rack 01">  
      <Node Name="Server01" Location="Rack Unit 33" />  
      <Node Name="Server02" Location="Rack Unit 35" />  
      <Node Name="Server03" Location="Rack Unit 37" />  
    </Rack>  
  </Site>  
  <Site Name="NYC" Location="Regional Datacenter, 456 Example Ave, New York City">  
    <Rack Name="B07" Location="Aisle B, Rack 07">  
      <Node Name="Server04" Location="Rack Unit 20" />  
      <Node Name="Server05" Location="Rack Unit 22" />  
      <Node Name="Server06" Location="Rack Unit 24" />  
    </Rack>  
  </Site>  
</Topology> 
``` 

#### <a name="example-two-chassis-blade-servers"></a>Exemplo: dois chassis, servidores blade  
```XML
<Topology>  
  <Rack Name="A01" Location="Contoso HQ, Room 4010, Aisle A, Rack 01">  
    <Chassis Name="Chassis01" Location="Rack Unit 2 (Upper)" >  
      <Node Name="Server01" Location="Left" />  
      <Node Name="Server02" Location="Right" />  
    </Chassis>  
    <Chassis Name="Chassis02" Location="Rack Unit 6 (Lower)" >  
      <Node Name="Server03" Location="Left" />  
      <Node Name="Server04" Location="Right" />  
    </Chassis>  
  </Rack>  
</Topology>  
```

Para definir a nova especificação de domínio de falha, salve o XML e execute o seguinte no PowerShell.  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

Este guia apresenta apenas dois exemplos, mas o `<Site>`, `<Rack>`, `<Chassis>`, e `<Node>` marcas podem ser misturadas e combinadas de várias maneiras para refletir a topologia física de sua implantação, seja ele qual for. Esperamos que esses exemplos ilustrem a flexibilidade dessas marcas e o valor de descritores de local de forma livre para resolver a ambiguidade entre elas.  

### <a name="optional-location-and-description-metadata"></a>Opcional: Local e uma descrição de metadados

Você pode fornecer opcional **local** ou **descrição** metadados para qualquer domínio de falha. Se forem fornecidas, essas informações serão incluídas no alerta de hardware do Serviço de Integridade. Este breve vídeo demonstra a importância de adicionar esses descritores.

[![Clique para ver um breve vídeo que demonstra a importância de adicionar descritores de local em domínios de falha](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Consulte também  
- [Introdução ao Windows Server 2019](https://docs.microsoft.com/windows-server/get-started-19/get-started-19)  
- [Introdução ao Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics)  
-   [Visão geral do espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-overview.md) 
