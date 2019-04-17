---
ms.assetid: 56fc7f80-9558-467e-a6e9-a04c9abbee33
title: "Reconhecimento de domínio falha"
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-failover-clustering
ms.topic: article
author: cosmosdarwin
ms.date: 09/16/2016
ms.openlocfilehash: f5c64bb8f8b7d4b8d13c76c4e94cfcf52ee32c30
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="fault-domain-awareness-in-windows-server-2016"></a>Reconhecimento de domínio falha em Windows Server 2016

> Aplica-se a: Windows Server 2016

Cluster de failover permite que vários servidores trabalham juntos para fornecer alta disponibilidade – ou em outras palavras, para fornecer a tolerância do nó. Mas as empresas de hoje exigem disponibilidade cada vez maior de sua infraestrutura. Para obter a atividade semelhantes a nuvem, até mesmo bastante improvável ocorrências como falhas de chassi, rack interrupções ou desastres naturais devem ser protegidas contra. É por isso que cluster de Failover no Windows Server 2016 apresenta chassi, rack e tolerância site também.

Domínios de falha e tolerância são conceitos estreitamente relacionados. Um domínio de falha é um conjunto de componentes de hardware que compartilham um único ponto de falha. Para ser tolerantes para um determinado nível, você precisa de vários domínios de falha nesse nível. Por exemplo, para ser rack tolerantes, seus servidores e os dados devem ser distribuídos em vários racks.

Este breve vídeo apresenta uma visão geral dos domínios de falha no Windows Server 2016:  
[![Clique nessa imagem para assistir a uma visão geral dos domínios de falha no Windows Server 2016](media/Fault-Domains-in-Windows-Server-2016/Part-1-Fault-Domains-Overview.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-1-Overview)

## <a name="benefits"></a>Benefícios
- **Espaços de armazenamento, incluindo direta de espaços de armazenamento, usa domínios de falha para maximizar a segurança de dados.**  
    Resiliência em espaços de armazenamento é conceitualmente semelhante a RAID distribuído, definido pelo software. Várias cópias de todos os dados são mantidas em sincronia, e se a falha de hardware e uma cópia é perdida, outros são copiados novamente para restaurar resiliência. Para obter a melhor resiliência possível, cópias devem ser mantidas em domínios de falha separado.

- **O [serviço de integridade](health-service-overview.md) usa falha domínios para fornecer alertas mais útil.**  
    Cada domínio falha pode ser associado com metadados de localização, que serão incluídos automaticamente em qualquer alertas subsequentes. Esses descritores podem ajudar a equipe de manutenção ou de operações e reduzir erros por disambiguating hardware.  

- **Ampliação clustering usa domínios de falha para afinidade de armazenamento.** Stretch clustering permite que os servidores longínqua ingressar em um cluster comuns. O melhor desempenho, aplicativos ou máquinas virtuais deve ser executadas em servidores que estão próximos aos fornecendo seu armazenamento. Reconhecimento de domínio falha permite esse afinidade de armazenamento.   

## <a name="levels-of-fault-domains"></a>Níveis de domínios de falha  
Há quatro níveis canônicos de domínios de falha - site, rack, chassis e nó. Nós são detectados automaticamente. cada nível adicional é opcional. Por exemplo, se sua implantação não usar servidores blade, o nível de chassi talvez não faça sentido para você.  

![Diagrama de diferentes níveis de domínios de falha](media/Fault-Domains-in-Windows-Server-2016/levels-of-fault-domains.png)

## <a name="usage"></a>Uso  
Você pode usar a marcação do PowerShell ou XML para especificar os domínios de falha. As duas abordagens são equivalentes e fornecem funcionalidade completa.

>[!IMPORTANT]
> Especifique os domínios de falha antes de habilitar direta de espaços de armazenamento, se possível. Isso permite que a configuração automática preparar o pool, faixas e configurações como resiliência e contagem de colunas para tolerância chassi ou rack. Depois que o pool e volumes tiverem sido criados, dados não forma retroativa moverá em resposta às alterações para a topologia de domínio falha. Para mover nós entre chassi ou racks após a habilitação da direta de espaços de armazenamento, você deve remover primeiro o nó e suas unidades do pool usando `Remove-ClusterNode -CleanUpDisks`.

### <a name="defining-fault-domains-with-powershell"></a>Definindo domínios de falha com o PowerShell
Windows Server 2016 apresenta os seguintes cmdlets para trabalhar com domínios de falha:
* `Get-ClusterFaultDomain`
* `Set-ClusterFaultDomain`
* `New-ClusterFaultDomain` 
* `Remove-ClusterFaultDomain`

Este breve vídeo demonstra o uso desses cmdlets.
[![Clique nessa imagem para assistir a um breve vídeo sobre o uso dos cmdlets do domínio de falha de Cluster](media/Fault-Domains-in-Windows-Server-2016/Part-2-Using-PowerShell.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-2-Using-PowerShell)

Use `Get-ClusterFaultDomain`para ver a topologia de domínio falha atual. Isso irá listar todos os nós de cluster, além de qualquer chassi, racks ou sites que você criou. Você pode filtrar usando parâmetros como **-tipo** ou **-nome**, mas eles não são necessários.

```PowerShell
Get-ClusterFaultDomain
Get-ClusterFaultDomain -Type Rack
Get-ClusterFaultDomain -Name "server01.contoso.com"
```

Use `New-ClusterFaultDomain`para criar novo chassi, racks ou sites. O `-Type`e `-Name`parâmetros são necessários. Os valores possíveis para `-Type`são `Chassis`, `Rack`, e `Site`. O `-Name`pode ser qualquer cadeia de caracteres. (Para `Node`domínios de falha de tipo, o nome devem ser o nome do nó real, como conjunto automaticamente).

```PowerShell
New-ClusterFaultDomain -Type Chassis -Name "Chassis 007"
New-ClusterFaultDomain -Type Rack -Name "Rack A"
New-ClusterFaultDomain -Type Site -Name "Shanghai"
```

> [!IMPORTANT]  
> Windows Server não pode e não verifica que qualquer domínios de falha que criar correspondem aos nada no mundo real, físico. (Isso pode parecer óbvio, mas é importante entender). Se, no mundo físico, os nós estão todos em um rack, em seguida, criar dois `-Type Rack`domínios de falha no software não fornece tolerância rack magicamente. Você é responsável por garantir a topologia que você cria com esses cmdlets corresponde a disposição real de seu hardware.

Use `Set-ClusterFaultDomain`para mover um domínio falha em outro. Os termos "pai" e "filho" são usadas para descrever essa relação de aninhamento. O `-Name`e `-Parent`parâmetros são necessários. Em `-Name`, forneça o nome do domínio falha que será movido; em `-Parent`, forneça o nome do destino. Para mover vários domínios de falha ao mesmo tempo, liste seus nomes.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent "Rack A"
Set-ClusterFaultDomain -Name "Rack A", "Rack B", "Rack C", "Rack D" -Parent "Shanghai"
```

> [!IMPORTANT]  
> Ao mover domínios de falha, seus filhos movem com eles. No exemplo acima, se um Rack é o pai de server01.contoso.com, o último não separadamente precisa ser movido para o site de Xangai – ele já estiver lá devido ao seu pai, sendo exatamente como no mundo físico.

Você pode ver as relações pai-filho na saída do `Get-ClusterFaultDomain`, além do `ParentName`e `ChildrenNames`colunas.

Você também pode usar `Set-ClusterFaultDomain`modificar determinadas outras propriedades de domínios de falha. Por exemplo, você pode fornecer opcional `-Location`ou `-Description`metadados para qualquer domínio de falha. Se fornecido, essas informações serão incluídas no hardware alertas do serviço de integridade. Você também pode renomear domínios de falha usando o `-NewName`parâmetro. Não renomeie `Node`digite domínios de falha.

```PowerShell
Set-ClusterFaultDomain -Name "Rack A" -Location "Building 34, Room 4010"
Set-ClusterFaultDomain -Type Node -Description "Contoso XYZ Server"
Set-ClusterFaultDomain -Name "Shanghai" -NewName "China Region"
```

Use `Remove-ClusterFaultDomain`para remover o chassi, racks ou sites que você criou. O `-Name`parâmetro será obrigatório. Você não pode remover um domínio de falhas que contém filhos – primeiro, remova os filhos ou movê-los fora usando `Set-ClusterFaultDomain`. Para mover um domínio de falha fora de todos os outros domínios de falha, defina sua `-Parent`na cadeia de caracteres vazia (""). Você não pode remover `Node`digite domínios de falha. Para remover vários domínios de falha ao mesmo tempo, liste seus nomes.

```PowerShell
Set-ClusterFaultDomain -Name "server01.contoso.com" -Parent ""
Remove-ClusterFaultDomain -Name "Rack A"
```

### <a name="defining-fault-domains-with-xml-markup"></a>Definindo domínios de falha com marcação XML
Domínios de falha podem ser especificados usando uma sintaxe XML e inspirados. Recomendamos usar o editor de texto favorito, como o código do Visual Studio (disponível gratuitamente *[aqui](https://code.visualstudio.com/)*) ou bloco de notas, crie um documento XML que você pode salvar e reutilizar.  

Este breve vídeo demonstra o uso da marcação XML para especificar os domínios de falha.

[![CClique essa imagem para assistir a um breve vídeo sobre como usar XML para especificar os domínios de falha](media/Fault-Domains-in-Windows-Server-2016/Part-3-Using-XML-Markup.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-3-Using-XML)

No PowerShell, execute o seguinte cmdlet:`Get-ClusterFaultDomainXML`. Isso retorna a especificação de domínio falha atual para o cluster, como XML. Isso reflete a cada descobertos `<Node>`encapsulada na abertura e fechamento `<Topology>`marcas.  

Execute o seguinte para salvar essa saída para um arquivo.  

```PowerShell
Get-ClusterFaultDomainXML | Out-File <Path>  
```

Abra o arquivo e adicione `<Site>`, `<Rack>`, e `<Chassis>`marcas para especificar como esses nós são distribuídos em sites, racks e chassi. Cada marca deve ser identificada por um único **nome **. Para nós, você deve manter o nome do nó conforme preenchidos por padrão.  

> [!IMPORTANT]  
> Embora todas as marcas adicionais são opcionais, eles devem cumprir o Site transitivo &gt;Rack &gt;chassi &gt;hierarquia de nós e deve ser fechado corretamente.  
Além do nome, de forma livre `Location="..."`e `Description="..."`descritores podem ser adicionados a qualquer marca.  

#### <a name="example-two-sites-one-rack-each"></a>Exemplo: Dois locais, um coletar cada  

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

Para definir a nova especificação de domínio falha, salve seu XML e execute o seguinte no PowerShell.  

```PowerShell
$xml = Get-Content <Path> | Out-String  
Set-ClusterFaultDomainXML -XML $xml
```

Este guia apresenta apenas dois exemplos, mas o `<Site>`, `<Rack>`, `<Chassis>`, e `<Node>`marcas podem ser misturadas e combinadas em muitas outras maneiras para refletir a topologia física da implantação, tudo o que pode estar. Esperamos que estes exemplos ilustram a flexibilidade dessas marcas e o valor de descritores de localização de forma livre para desambiguá-los.  

### <a name="optional-location-and-description-metadata"></a>Opcional: Metadados de local e descrição

Você pode fornecer opcional **local** ou **descrição** metadados para qualquer domínio de falha. Se fornecido, essas informações serão incluídas no hardware alertas do serviço de integridade. Este breve vídeo demonstra o valor da adição de tais descritores.

[![CClique para ver um breve vídeo demonstrando o valor da adição de descritores de localização para domínios de falha](media/Fault-Domains-in-Windows-Server-2016/part-4-location-description.jpg)](https://channel9.msdn.com/Blogs/windowsserver/Fault-Domain-Awareness-in-WS2016-Part-4-Location-Description)

## <a name="see-also"></a>Consulte também  
-   [Windows Server 2016](../get-started/windows-server-2016.md)  
-   [Espaços de armazenamento direcionam no Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md) 
