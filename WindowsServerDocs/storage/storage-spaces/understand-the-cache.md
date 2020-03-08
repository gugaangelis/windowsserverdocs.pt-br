---
title: Noções básicas sobre o cache nos Espaços de Armazenamento Diretos
ms.assetid: 69b1adc0-ee64-4eed-9732-0fb216777992
ms.prod: windows-server
ms.author: cosdar
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: f2c2e0435d06c18dbacab4e85db770ba86e654b3
ms.sourcegitcommit: b5c12007b4c8fdad56076d4827790a79686596af
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78865415"
---
# <a name="understanding-the-cache-in-storage-spaces-direct"></a>Noções básicas sobre o cache nos Espaços de Armazenamento Diretos

>Aplica-se a: Windows Server 2019, Windows Server 2016

[Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md) contam com um cache do servidor interno para maximizar o desempenho de armazenamento. Trata-se de um cache de leitura *e* gravação grande, persistente e em tempo real. O cache é configurado automaticamente quando os Espaços de Armazenamento Diretos estão habilitados. Na maioria dos casos, nenhum tipo de gerenciamento manual é necessário.
A maneira como o cache funciona depende dos tipos de unidades presentes.

O vídeo a seguir entra em detalhes sobre como o cache funciona para Espaços de Armazenamento Diretos, assim como outras considerações de design.

<strong>Considerações de design de Espaços de Armazenamento Diretos</strong><br>(20 minutos)<br>
<iframe src="https://channel9.msdn.com/Blogs/windowsserver/Design-Considerations-for-Storage-Spaces-Direct/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="drive-types-and-deployment-options"></a>Tipos de unidade e opções de implantação

Os Espaços de Armazenamento Diretos atualmente funcionam com três tipos de dispositivos de armazenamento:

<table>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/NVMe-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            NVMe (Non-Volatile Memory Express)
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/SSD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            SATA/SAS SSD (unidade de estado sólido)
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 10px; border: 0; width:70px">
            <img src="media/understand-the-cache/HDD-100px.png">
        </td>
        <td style="padding: 10px; border: 0;" valign="middle">
            HDD (unidade de disco rígido)
        </td>
    </tr>
</table>

Eles podem ser combinados de seis maneiras, que podemos agrupar em duas categorias: "tudo flash" e "híbridos".

### <a name="all-flash-deployment-possibilities"></a>Possibilidades de implantação tudo flash

As implantações tudo flash pretendem maximizar o desempenho de armazenamento e não incluem unidades de disco rígido (HDD) rotacionais.

![All-Flash-Deployment-Possibilities](media/understand-the-cache/All-Flash-Deployment-Possibilities.png)

### <a name="hybrid-deployment-possibilities"></a>Possibilidades de implantação híbrida

As implantações híbridas pretendem equilibrar desempenho e capacidade ou maximizar a capacidade e incluem unidades de disco rígido (HDD) rotacionais.

![Hybrid-Deployment-Possibilities](media/understand-the-cache/Hybrid-Deployment-Possibilities.png)

## <a name="cache-drives-are-selected-automatically"></a>Unidades de cache são selecionadas automaticamente

Em implantações com vários tipos de unidades, os Espaços de Armazenamento Diretos usam automaticamente todas as unidades do tipo "mais rápida" para armazenar em cache. As unidades restantes são usadas para capacidade.

O tipo "mais rápido" é determinado de acordo com a hierarquia a seguir.

![Drive-Type-Hierarchy](media/understand-the-cache/Drive-Type-Hierarchy.png)

Por exemplo, se você tiver NVMe e SSDs, o NVMe armazenará em cache para as SSDs.

Se você tiver SSDs e HDDs, as SSDs armazenarão em cache para as HDDs.

   >[!NOTE]
   > As unidades de cache não contribuem para a capacidade de armazenamento utilizável. Todos os dados armazenados no cache também são armazenados em outro lugar, ou serão assim que forem despreparados. Isso significa que a capacidade de armazenamento bruta total da implantação é apenas a soma das unidades de capacidade.

Quando todas as unidades são do mesmo tipo, nenhum cache é configurado automaticamente. Você tem a opção de configurar manualmente unidades de mais resistência para armazenar em cache para unidades de menos resistência do mesmo tipo – consulte a seção [Configuração manual](#manual-configuration) para aprender como.

   >[!TIP]
   > Em implantações com tudo NVMe ou SSD, especialmente em escala muito pequena, não "gastar" unidades em cache pode aumentar consideravelmente a eficiência de armazenamento.

## <a name="cache-behavior-is-set-automatically"></a>Comportamento do cache é definido automaticamente

O comportamento do cache é determinado automaticamente com base nos tipos de unidades que estão sendo usadas no armazenamento em cache. Durante o armazenamento em cache para unidades de estado sólido (como armazenamento em cache NVMe para SSDS), apenas as gravações são armazenadas em cache. Durante o armazenamento em cache para unidades de disco rígido (por exemplo, armazenamento em cache SSDs para HDDs), as leituras e as gravações são armazenadas em cache.

![Cache-Read-Write-Behavior](media/understand-the-cache/Cache-Read-Write-Behavior.png)

### <a name="write-only-caching-for-all-flash-deployments"></a>Armazenamento em cache somente leitura para implantações tudo em flash

Durante o armazenamento em cache para unidades de estado sólido (NVMe ou SSDs), apenas as gravações são armazenadas em cache. Isso reduz o desgaste da capacidade das unidades porque muitas gravações e regravações podem se unir no cache e só serem despreparadas conforme necessário, o que reduz o tráfego cumulativo para as unidades de capacidade e prolonga a vida útil. Por esse motivo, recomendamos selecionar unidades [de mais resistência, otimizadas para gravação](http://whatis.techtarget.com/definition/DWPD-device-drive-writes-per-day) para o cache. Naturalmente, as unidades de capacidade podem ter menos resistência à gravação.

Isso porque as leituras não afetam significativamente o tempo de vida do flash e, como unidades de estado sólido universalmente oferecem baixa latência de leitura, as leituras não são armazenada em cache: elas são fornecidas diretamente pelas unidades de capacidade (exceto quando os dados foram gravados tão recentemente a ponto de ainda não terem sido despreparados). Isso permite que o cache seja inteiramente dedicado a gravações, o que maximiza a eficácia.

Isso resulta nas características de gravação, como latência de gravação, sendo ditadas pelas unidades de cache, e nas características de leitura sendo determinadas pelas unidades de capacidade. Ambas são consistentes, previsíveis e uniformes.

### <a name="readwrite-caching-for-hybrid-deployments"></a>Armazenamento em cache de leitura/gravação para implantações híbridas

Durante o armazenamento em cache para unidades de disco rígido (HDDs), leituras *e* gravações são armazenadas em cache, para oferecer latência semelhante a flash (normalmente, cerca de 10 vezes melhor) para ambas. O cache de leitura armazena dados de leitura recentes e frequentes para acesso rápido e minimização de tráfego aleatório para as HDDs. (Por causa de atrasos rotacionais e de busca, a latência e o tempo perdido incorridos pelo acesso aleatório a uma HDD são significativos.) As gravações são armazenadas em cache para absorver intermitências e, como antes, unir gravações e regravações, além de minimizar o tráfego cumulativo para as unidades de capacidade.

Espaços de Armazenamento Diretos implementam um algoritmo que cancela a reprodução aleatória de gravações antes da preparação delas a fim de emular um padrão de E/S para disco aparentemente sequencial, mesmo quando a E/S real proveniente da carga de trabalho (como máquinas virtuais) é aleatória. Isso maximiza o IOPS e a taxa de transferência para as HDDs.

### <a name="caching-in-deployments-with-drives-of-all-three-types"></a>Armazenando em cache implantações com unidades de todos os três tipos

Quando unidades de todos os três tipos estão presentes, as unidades NVMe oferecem armazenamento em cache para as SSDs e as HDDs. O comportamento é o descrito acima: somente gravações são armazenadas em cache para as SSDs, e leituras e gravações são armazenadas em cache para as HDDs. O custo indireto do armazenamento em cache para as HDDs é distribuído por igual entre as unidades de cache. 

## <a name="summary"></a>Resumo

Esta tabela resume quais unidades são usadas no armazenamento em cache, quais são usadas na capacidade e qual é o comportamento de armazenamento em cache para cada possibilidade de implantação.

| Implantação     | Unidades de cache                        | Unidades de capacidade | Comportamento de cache (padrão)  |
| -------------- | ----------------------------------- | --------------- | ------------------------- |
| Tudo NVMe         | Nenhum (opcional: configurar manualmente) | NVMe            | Somente gravação (se configurada)  |
| Todas as SSDs          | Nenhum (opcional: configurar manualmente) | SSD             | Somente gravação (se configurada)  |
| NVMe + SSD       | NVMe                                | SSD             | Somente gravação                  |
| NVMe + HDD       | NVMe                                | HDD             | Leitura + gravação                |
| SSD + HDD        | SSD                                 | HDD             | Leitura + gravação                |
| NVMe + SSD + HDD | NVMe                                | SSD + HDD       | Leitura + gravação para HDD, somente gravação para SSD  |

## <a name="server-side-architecture"></a>Arquitetura do servidor

O cache é implementado no nível da unidade: unidades de cache individuais dentro de um servidor são associadas a uma ou várias unidades de capacidade dentro do mesmo servidor.

Como está abaixo do restante da pilha de armazenamento definido pelo software Windows, o cache não tem nem precisa ter percepção de conceitos como espaços de armazenamento ou tolerância a falhas. Você pode ver isso como criar unidades "híbridas" (meio flash, meio disco) que acabam sendo apresentadas para o Windows. Assim como acontece com uma unidade híbrida real, a movimentação em tempo real de dados quentes e frios entre as partes mais rápidas e mais lentas da mídia física é praticamente invisível por fora.

Uma vez que a resiliência em Espaços de Armazenamento Diretos está pelo menos no nível do servidor (o que significa que as cópias dos dados são sempre gravadas em servidores diferentes; no máximo, uma cópia por servidor), os dados no cache aproveitam a mesma resiliência de dados que não estão no cache.

![Cache-Server-Side-Architecture](media/understand-the-cache/Cache-Server-Side-Architecture.png)

Por exemplo, durante o uso do espelhamento triplo, três cópias de todos os dados são gravadas em servidores diferentes, onde eles chegam ao cache. Independentemente de serem despreparados ou não depois, sempre haverá três cópias.

## <a name="drive-bindings-are-dynamic"></a>Associações de unidade são dinâmicas

A associação entre as unidades de cache e capacidade podem ter qualquer proporção, de 1:1 a 1:12 e além. Ela é ajustada dinamicamente sempre que as unidades são adicionadas ou removidas, como acontece durante escalas verticais ou depois de falhas. Isso significa que você pode adicionar unidades de cache ou de capacidade de maneira independente, sempre que quiser.

![Dynamic-Binding](media/understand-the-cache/Dynamic-Binding.gif)

Por simetria, recomendamos que o número de unidades de capacidade seja um múltiplo do número de unidades de cache. Por exemplo, se você tiver 4 unidades de cache, terá um desempenho mais uniforme com 8 unidades de capacidade (proporção 1:2) do que com 7 ou 9.

## <a name="handling-cache-drive-failures"></a>Identificando falhas na unidade de cache

Quando ocorre uma falha em uma unidade de cache, todas as gravações que ainda não foram despreparadas são perdidas *para o servidor local*, o que significa que existem apenas nas outras cópias (em outros servidores). Assim como acontece após qualquer outra falha na unidade, os Espaços de Armazenamento podem e se recuperam automaticamente consultando as cópias sobreviventes.

Por um breve período, as unidades de capacidade que estavam associadas à unidade de cache perdido surgirão não íntegras. Depois que a reassociação de cache tiver ocorrido (automática) e o reparo de dados tiver sido concluído (automático), elas voltarão a ser mostradas como íntegras.

É por causa desse cenário que pelo menos duas unidades de cache são necessárias por servidor para preservar o desempenho.

![Handling-Failure](media/understand-the-cache/Handling-Failure.gif)

É possível acabar substituindo a unidade de cache assim como qualquer outra troca de unidade.

   >[!NOTE]
   > Você precisa desligar para substituir com segurança o NVMe, um fator forma de cartão suplementar (AIC) ou M.2.

## <a name="relationship-to-other-caches"></a>Relação com outros caches

Existem diversos outros caches não relacionados na pilha de armazenamento definido pelo software do Windows. Entre os exemplos estão o cache com write-back de Espaços de Armazenamento e o cache de leitura na memória Volume Compartilhado Clusterizado (CSV).

Com Espaços de Armazenamento Diretos, o cache com write-back de Espaços de Armazenamento não deve ter o comportamento padrão modificado. Por exemplo, parâmetros como **- WriteCacheSize** no cmdlet **New-Volume** não devem ser usados.

Você pode optar por usar o cache CSV, ou não – cabe a você. Ele permanece desativado por padrão em Espaços de Armazenamento Diretos, mas não entra em conflito com o novo cache descrito neste tópico de maneira alguma. Em determinados cenários, ele pode proporcionar ganhos de desempenho importantes. Para obter mais informações, consulte [How to Enable CSV Cache](../../failover-clustering/failover-cluster-csvs.md#enable-the-csv-cache-for-read-intensive-workloads-optional).

## <a name="manual-configuration"></a>Configuração manual

Para a maioria das implantações, a configuração manual não é necessária. Caso você precise dele, consulte as seções a seguir. 

Se você precisar fazer alterações no modelo de dispositivo de cache após a instalação, edite o documento de componentes de suporte do Serviço de Integridade, conforme descrito em [serviço de integridade visão geral](../../failover-clustering/health-service-overview.md#supported-components-document).

### <a name="specify-cache-drive-model"></a>Especificar o modelo da unidade de cache

Em implantações nas quais todas as unidades sejam do mesmo tipo, como implantações tudo NVMe ou SSD, nenhum cache é configurado porque o Windows não consegue diferenciar automaticamente características como resistência a gravações entre unidades do mesmo tipo.

Para usar unidades de mais resistência no armazenamento em cache para unidades de menos resistência, é possível especificar qual modelo de unidade usar com o parâmetro **-CacheDeviceModel** do cmdlet **Enable-ClusterS2D**. Assim que Espaços de Armazenamento Diretos for habilitado, todas as unidades desse modelo serão usadas no armazenamento em cache.

   >[!TIP]
   > Certifique-se de comparar a cadeia de caracteres do modelo exatamente como ela é exibida na saída de **Get-PhysicalDisk**.

####  <a name="example"></a>{1&gt;Exemplo&lt;1}

Primeiro, obtenha uma lista de discos físicos:

```PowerShell
Get-PhysicalDisk | Group Model -NoElement
```

Veja a seguir um exemplo de saída:

```
Count Name
----- ----
    8 FABRIKAM NVME-1710
   16 CONTOSO NVME-1520
```

Em seguida, digite o seguinte comando, especificando o modelo de dispositivo de cache:

```PowerShell
Enable-ClusterS2D -CacheDeviceModel "FABRIKAM NVME-1710"
```

Você pode verificar se as unidades desejadas estão sendo usadas no armazenamento em cache executando **Get-PhysicalDisk** no PowerShell e verificando se a propriedade **Usage** diz **"Journal"** .

### <a name="manual-deployment-possibilities"></a>Possibilidades de implantação manual

A configuração manual permite as seguintes possibilidades de implantação:

![Exotic-Deployment-Possibilities](media/understand-the-cache/Exotic-Deployment-Possibilities.png)

### <a name="set-cache-behavior"></a>Definir comportamento do cache

É possível substituir o comportamento padrão do cache. Por exemplo, é possível defini-lo para leituras de cache, mesmo em uma implantação tudo flash. Não incentivamos a modificação do comportamento, a menos que você tenha a certeza de que o padrão não atende à carga de trabalho.

Para substituir o comportamento, use o cmdlet **set-ClusterStorageSpacesDirect** e seus parâmetros **-CacheModeSSD** e **-CacheModeHDD** . O parâmetro **CacheModeSSD** define o comportamento do cache durante o armazenar em cache para unidades de estado sólido. O parâmetro **CacheModeHDD** define o comportamento do cache durante o armazenamento em cache para unidades de disco rígido. Isso poderá ser feito a qualquer momento depois que Espaços de Armazenamento Diretos forem habilitados.

Você pode usar **Get-ClusterStorageSpacesDirect** para verificar se o comportamento está definido.

#### <a name="example"></a>{1&gt;Exemplo&lt;1}

Primeiro, obtenha as configurações de Espaços de Armazenamento Diretos:

```PowerShell
Get-ClusterStorageSpacesDirect
```

Veja a seguir um exemplo de saída:

```
CacheModeHDD : ReadWrite
CacheModeSSD : WriteOnly
```

Em seguida, faça o seguinte:

```PowerShell
Set-ClusterStorageSpacesDirect -CacheModeSSD ReadWrite

Get-ClusterS2D
```

Veja a seguir um exemplo de saída:

```
CacheModeHDD : ReadWrite
CacheModeSSD : ReadWrite
```

## <a name="sizing-the-cache"></a>Dimensionando o cache

O cache deve ser dimensionado para acomodar o conjunto de trabalho (os dados em leitura ou gravação ativa a qualquer momento) dos aplicativos e cargas de trabalho.

Isso é especialmente importante em implantações híbridas com unidades de disco rígido. Se o conjunto de trabalho ativo exceder o tamanho do cache, ou se o conjunto de trabalho ativo aumentar muito rapidamente, as perdas no cache de leitura aumentarão, e as gravações precisarão ser despreparadas de maneira mais agressiva, prejudicando o desempenho geral.

É possível usar o utilitário Monitor de Desempenho (PerfMon.exe) interno do Windows para inspecionar a taxa de perdas no cache. Mais especificamente, é possível comparar o **Cache Miss Reads/sec** do contador **Cluster Storage Hybrid Disk** definido como o IOPS de leitura geral da implantação. Cada "Disco Híbrido" corresponde a uma unidade de capacidade.

Por exemplo, 2 unidades de cache associadas a 4 unidades de capacidade resulta em 4 instâncias de objeto "Disco Híbrido" por servidor.

![Performance-Monitor](media/understand-the-cache/PerfMon.png)

Não há regra universal, mas se houver muitas perdas de leitura no cache, isso poderá ser subdimensionado e você deverá levar em consideração a adição de unidades de cache para expandir o cache. É possível adicionar unidades de cache ou de capacidade de maneira independente, sempre que você quiser.

## <a name="see-also"></a>Consulte também

- [Escolhendo unidades e tipos de resiliência](choosing-drives.md)
- [Tolerância a falhas e eficiência de armazenamento](storage-spaces-fault-tolerance.md)
- [Requisitos de hardware Espaços de Armazenamento Diretos](storage-spaces-direct-hardware-requirements.md)
