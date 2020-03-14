---
ms.assetid: 342173ca-4e10-44f4-b2c9-02a6c26f7a4a
title: Planejamento de volumes nos Espaços de Armazenamento Diretos
ms.prod: windows-server
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 06/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: 52c600068d5dd447ff9faa7c40788664e222a83a
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322778"
---
# <a name="planning-volumes-in-storage-spaces-direct"></a>Planejamento de volumes nos Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico fornece diretrizes de como planejar volumes em Espaços de Armazenamento Diretos para atender às necessidades de desempenho e capacidade de cargas de trabalho, incluindo escolher o sistema de arquivos, tipo de resiliência e tamanho.

## <a name="review-what-are-volumes"></a>Revisão: O que são volumes

Os volumes são onde você coloca os arquivos de que suas cargas de trabalho precisam, como arquivos VHD ou VHDX para máquinas virtuais do Hyper-V. Os volumes combinam as unidades no pool de armazenamento para apresentar a tolerância, escalabilidade e benefícios de desempenho de Espaços de Armazenamento Diretos.

   >[!NOTE]
   > Em toda a documentação de Espaços de Armazenamento Direto, usamos o termo "volume" para nos referir conjuntamente ao volume e ao disco virtual sob ele, incluindo a funcionalidade fornecida por outros recursos internos do Windows, como Volumes Compartilhados do Cluster (CSV) e ReFS. Não é necessário entender essas distinções no nível de implementação para planejar e implantar com êxito Espaços de Armazenamento Diretos.

![o que são volumes](media/plan-volumes/what-are-volumes.png)

Todos os volumes são acessíveis por todos os servidores do cluster ao mesmo tempo. Depois de criadas, elas aparecem em **C:\ClusterStorage\\** em todos os servidores.

![pasta de captura de tela csv](media/plan-volumes/csv-folder-screenshot.png)

## <a name="choosing-how-many-volumes-to-create"></a>Escolhendo quantos volumes criar

Recomendamos criar um número de volumes múltiplo do número de servidores no cluster. Por exemplo, se você tiver quatro servidores, ocorrerá um desempenho mais consistente com 4 volumes totais do que com 3 ou 5. Isso permite que o cluster distribua a "propriedade" do volume (um servidor manipula coordenação de metadados para cada volume) uniformemente entre servidores.

É recomendável limitar o número total de volumes para:

| Windows Server 2016          | Windows Server 2019          |
|------------------------------|------------------------------|
| Até 32 volumes por cluster | Até 64 volumes por cluster |

## <a name="choosing-the-filesystem"></a>Escolhendo o sistema de arquivos

Recomendamos usar o novo [Sistema de Arquivos Resiliente (ReFS)](../refs/refs-overview.md) para Espaços de Armazenamento Diretos. ReFS é o sistema de arquivos premier voltado para virtualização e oferece muitas vantagens, incluindo acelerações de desempenho espetaculares e proteção interna contra corrupção de dados. Ele dá suporte a quase todos os principais recursos do NTFS, incluindo eliminação de duplicação de dados no Windows Server, versão 1709 e posterior. Consulte a [tabela de comparação de recursos](../refs/refs-overview.md#feature-comparison) ReFS para obter detalhes.

Se sua carga de trabalho requerer um recurso a que o ReFS ainda não dá suporte, você pode usar o NTFS.

   > [!TIP]
   > Volumes com sistemas de arquivos diferentes podem coexistir no mesmo cluster.

## <a name="choosing-the-resiliency-type"></a>Escolhendo o tipo de resiliência

Os volumes em Espaços de Armazenamento Diretos fornecem resiliência para se proteger contra os problemas de hardware, como falhas na unidade ou no servidor, e para possibilitar a disponibilidade contínua durante a manutenção do servidor, como atualizações de software.

   > [!NOTE]
   > Os tipos de resiliência entre os quais você pode escolher independem dos tipos de unidades que você tem.

### <a name="with-two-servers"></a>Com dois servidores

Com dois servidores no cluster, você pode usar o espelhamento bidirecional. Se você estiver executando o Windows Server 2019, também poderá usar resiliência aninhada.

O espelhamento bidirecional mantém duas cópias de todos os dados, uma cópia nas unidades de cada servidor. Sua eficiência de armazenamento é de 50% – para gravar 1 TB de dados, você precisa de pelo menos 2 TB de capacidade de armazenamento físico no pool de armazenamento. O espelhamento bidirecional pode tolerar com segurança uma falha de hardware por vez (um servidor ou unidade).

![espelhamento bidirecional](media/plan-volumes/two-way-mirror.png)

A resiliência aninhada (disponível somente no Windows Server 2019) fornece resiliência de dados entre servidores com espelhamento bidirecional e, em seguida, adiciona resiliência em um servidor com espelhamento bidirecional ou paridade acelerada por espelho. O aninhamento fornece resiliência de dados mesmo quando um servidor está reiniciando ou indisponível. Sua eficiência de armazenamento é de 25% com espelhamento bidirecional aninhado e cerca de 35-40% para paridade com aceleração de espelho aninhado. A resiliência aninhada pode tolerar com segurança duas falhas de hardware por vez (duas unidades ou um servidor e uma unidade no servidor restante). Por causa dessa resiliência de dados adicionada, recomendamos o uso de resiliência aninhada em implantações de produção de clusters de dois servidores, se você estiver executando o Windows Server 2019. Para obter mais informações, consulte [resiliência aninhada](nested-resiliency.md).

![Espelhamento aninhado-paridade acelerada](media/nested-resiliency/nested-mirror-accelerated-parity.png)

### <a name="with-three-servers"></a>Com três servidores

Com três servidores, você deve usar o espelhamento de três voas para melhor tolerância de falhas e melhor desempenho. O espelhamento de três vias mantém três cópias de todos os dados, uma cópia nas unidades em cada servidor. A eficiência de armazenamento é de 33,3% – para gravar 1 TB de dados, é necessário ter pelo menos 3 TB de capacidade de armazenamento físico no pool de armazenamento. O espelhamento de três vias pode tolerar com segurança [pelo menos dois problemas de hardware (unidade ou servidor) por vez](storage-spaces-fault-tolerance.md#examples). Se 2 nós ficarem indisponíveis, o pool de armazenamento perderá o quorum, uma vez que 2/3 dos discos não estão disponíveis e os discos virtuais ficarão inacessíveis. No entanto, um nó pode estar inoperante e um ou mais discos em outro nó podem falhar e os discos virtuais permanecerão online. Por exemplo, se você estiver reiniciando um servidor quando, de repente, outra unidade ou servidor falhar, todos os dados permanecem seguros e continuamente acessíveis.

![espelhamento triplo](media/plan-volumes/three-way-mirror.png)

### <a name="with-four-or-more-servers"></a>Com quatro ou mais servidores

Com quatro ou mais servidores, você pode escolher para cada volume se deseja usar o espelhamento de três vias, paridade dupla (geralmente chamada de "codificação de apagamento") ou misturar os dois com paridade acelerada por espelho.

A paridade dupla fornece a mesma tolerância a falhas que o espelhamento de três vias, mas com melhor eficiência de armazenamento. Com quatro servidores, sua eficiência de armazenamento é 50.0% – para armazenar 2 TB de dados, você precisa de 4 TB de capacidade de armazenamento físico no pool de armazenamento. Isso aumenta a eficiência de armazenamento para 66,7% com sete servidores e continua a aumentar até 80,0% de eficiência de armazenamento. A desvantagem é que codificação de paridade tem um processamento mais intensivo, o que pode limitar seu desempenho.

![paridade dupla](media/plan-volumes/dual-parity.png)

O tipo de resiliência a ser usado depende das necessidades de sua carga de trabalho. Aqui está uma tabela que resume quais cargas de trabalho são uma boa opção para cada tipo de resiliência, bem como a eficiência de desempenho e armazenamento de cada tipo de resiliência.

| Tipo de resiliência | Eficiência da capacidade | Velocidade | Cargas de trabalho |
| ------------------- | ----------------------  | --------- | ------------- |
| **Espelho**         | ![Eficiência de armazenamento mostrando 33%](media/plan-volumes/3-way-mirror-storage-efficiency.png)<br>Espelho de três vias: 33% <br>Espelho bidirecional: 50%     |![Desempenho mostrando 100%](media/plan-volumes/three-way-mirror-perf.png)<br> Desempenho mais alto  | Cargas de trabalho virtualizadas<br> Bancos de dados<br>Outras cargas de trabalho de alto desempenho |
| **Paridade acelerada por espelho** |![Eficiência de armazenamento mostrando cerca de 50%](media/plan-volumes/mirror-accelerated-parity-storage-efficiency.png)<br> Depende da proporção de espelho e paridade | ![Desempenho mostrando cerca de 20%](media/plan-volumes/mirror-accelerated-parity-perf.png)<br>Muito mais lento do que o espelho, mas até duas vezes mais rápido que a dupla paridade<br> Melhor para gravações e leituras sequenciais grandes | Arquivamento e backup<br> Infraestrutura de área de trabalho virtualizada     |
| **Paridade dupla**               | ![Eficiência de armazenamento mostrando cerca de 80%](media/plan-volumes/dual-parity-storage-efficiency.png)<br>4 servidores: 50% <br>16 servidores: até 80% | ![Desempenho mostrando cerca de 10%](media/plan-volumes/dual-parity-perf.png)<br>Latência de e/s mais alta & uso da CPU em gravações<br> Melhor para gravações e leituras sequenciais grandes | Arquivamento e backup<br> Infraestrutura de área de trabalho virtualizada  |

#### <a name="when-performance-matters-most"></a>Quando desempenho é o mais importante

As cargas de trabalho que tenham requisitos estritos de latência ou que precisem de muitos IOPS mistos aleatórios, como bancos de dados do SQL Server ou máquinas virtuais Hyper-V dependentes de desempenho, devem ser executadas em volumes que usem espelhamento para maximizar o desempenho.

   >[!TIP]
   > O espelhamento é mais rápido do que qualquer outro tipo de resiliência. Usamos o espelhamento para quase todos os exemplos de desempenho.

#### <a name="when-capacity-matters-most"></a>Quando a capacidade é o mais importante

Cargas de trabalho que gravam com pouca frequência, como data warehouses ou armazenamento "frio", devem ser executadas em volumes que usem a paridade dupla para aumentar a eficiência de armazenamento. Algumas outras cargas de trabalho, como servidores de arquivos tradicionais, Virtual Desktop Infrastructure (VDI) ou outros que não criem muito tráfego de E/S aleatório e errático e/ou não exijam o melhor desempenho também podem usar a paridade dupla, a seu critério. A paridade inevitavelmente aumenta utilização da CPU e a latência de E/S, principalmente nas gravações, em comparação ao espelhamento.

#### <a name="when-data-is-written-in-bulk"></a>Quando os dados são gravados em massa

Cargas de trabalho que gravam em passos grandes e sequenciais, como destinos de arquivamento ou backup, têm outra opção que é nova no Windows Server 2016: um volume pode misturar espelhamento e paridade dupla. As gravações são feitas primeiro na parte espelhada e, depois, são gradualmente movidas para a parte de paridade. Isso acelera a recepção e reduz a utilização de recursos quando chegam grandes gravações, permitindo que a codificação de paridade de processamento intensivo aconteça durante um período mais longo. Ao dimensionar as partes, considere que a quantidade de gravações que ocorrem ao mesmo tempo (por exemplo, um backup diário) deve caber confortavelmente na parte de espelho. Por exemplo, se receber 100 GB uma vez por dia, considere usar o espelhamento para 150 GB a 200 GB e a paridade dupla para o restante.

A eficiência de armazenamento resultante depende das proporções que você escolher. Consulte [esta demonstração](https://www.youtube.com/watch?v=-LK2ViRGbWs&t=36m55s) para obter alguns exemplos.

   > [!TIP]
   > Se você observar uma diminuição abrupta no desempenho de gravação MSRC durante por meio da ingestão de dados, isso pode indicar que a parte espelho não é grande o suficiente ou que a paridade acelerada por espelhamento não é adequada para seu caso de uso. Por exemplo, se o desempenho de gravação diminuir de 400 MB/s para 40 MB/s, considere expandir a parte do espelho ou alternar para o espelho de três vias.

### <a name="about-deployments-with-nvme-ssd-and-hdd"></a>Sobre as implantações com o NVMe, SSD e HDD

Em implantações com dois tipos de unidades, as unidades mais rápidas fornecem cache enquanto as unidades mais lentas fornecem a capacidade. Isso acontece automaticamente – para obter mais informações, consulte [Noções básicas sobre o cache em Espaços de Armazenamento Diretos](understand-the-cache.md). Em tais implantações, todos os volumes, em última análise, residem no mesmo tipo de unidades – as unidades de capacidade.

Em implantações com todos os três tipos de unidades, somente as unidades mais rápidas (NVMe) fornecem cache, deixando dois tipos de unidades (SSD e HDD) para fornecer capacidade. Para cada volume, você pode escolher se ele reside inteiramente na camada SSD, inteiramente na camada HDD, ou se abrange as duas.

   > [!IMPORTANT]
   > Recomendamos usar a camada SSD para colocar as cargas de trabalho mais dependentes de desempenho em all-flash.

## <a name="choosing-the-size-of-volumes"></a>Escolhendo o tamanho dos volumes

É recomendável limitar o tamanho de cada volume para:

| Windows Server 2016 | Windows Server 2019 |
| ------------------- | ------------------- |
| Até 32 TB         | Até 64 TB         |

   > [!TIP]
   > Se você usar uma solução de backup que dependa do VSS (serviço de cópias de sombra de volume) e do provedor de software VolSnap, como é comum com cargas de trabalho de servidor de arquivos, limitar o tamanho do volume a 10 TB melhorará o desempenho e a confiabilidade. As soluções de backup que usam a mais recente API RCT Hyper-V e/ou a clonagem de blocos ReFS e/ou as APIs de backup SQL nativas executam bem até 32 TB ou mais.

### <a name="footprint"></a>Superfície

O tamanho de um volume se refere à sua capacidade utilizável, a quantidade de dados que ele pode armazenar. Isso é fornecido pelo parâmetro **-Tamanho** do cmdlet **Novo Volume** e, em seguida, aparece na propriedade **Tamanho** quando você executa o cmdlet **Get-Volume**.

Tamanho é diferente da *superfície* do volume, a capacidade total de armazenamento físico que ocupa no pool de armazenamento. A superfície depende do tipo de resiliência. Por exemplo, os volumes que usam o espelhamento de três vias têm uma superfície de três vezes o seu tamanho.

As superfícies de seus volumes precisam caber no pool de armazenamento.

![tamanho versus superfície](media/plan-volumes/size-versus-footprint.png)

### <a name="reserve-capacity"></a>Capacidade de reserva

Deixar alguma capacidade não alocada no pool de armazenamento oferece espaço aos volumes para reparar "in-loco" após falhas de unidade, melhorando o desempenho e a segurança dos dados. Se houver capacidade suficiente, um reparo imediato, in-loco e paralelo pode restaurar os volumes a uma resiliência completa antes mesmo de as unidades com falha serem substituídas. Isso acontece automaticamente.

É recomendável reservar o equivalente a uma unidade de capacidade por servidor, até 4 unidades. Você pode reservar mais a seu critério, mas essa recomendação mínima garante que um reparo imediato, in-loco e paralelo pode ter sucesso após a falha de qualquer unidade.

![reserve](media/plan-volumes/reserve.png)

Por exemplo, se você tiver 2 servidores e estiver usando unidades de 1 TB de capacidade, separe 2 x 1 = 2 TB do pool como reserva. Se você tiver 3 servidores e unidades de 1 TB de capacidade, separe 3 x 1 = 3 TB como reserva. Se você tiver 4 ou mais servidores e unidades de 1 TB de capacidade, separe 4 x 1 = 4 TB como reserva.

   >[!NOTE]
   > Nos clusters com unidades de todos os três tipos (NVMe + SSD + HDD), é recomendável reservar o equivalente a uma SSD mais uma HDD por servidor, até 4 unidades de cada.

## <a name="example-capacity-planning"></a>Exemplo: planejamento da capacidade

Considere um cluster de quatro servidores. Cada servidor tem algumas unidades de cache além de dezesseis unidades de 2 TB de capacidade.

```
4 servers x 16 drives each x 2 TB each = 128 TB
```

Desses 128 TB no pool de armazenamento, separamos quatro unidades, ou 8 TB, para que os reparos in-loco possam acontecer sem qualquer pressa para substituir as unidades depois de falharem. Isso deixa 120 TB de capacidade de armazenamento físico no pool com o qual podemos criar volumes.

```
128 TB – (4 x 2 TB) = 120 TB
```

Suponha que precisamos que nossa implantação hospede algumas máquinas virtuais Hyper-V extremamente ativas, mas também há muito espaço de armazenamento frio – arquivos antigos e backups que precisamos manter. Como temos quatro servidores, vamos criar quatro volumes.

Vamos colocar as máquinas virtuais nos dois primeiros volumes: *Volume1* e *Volume2*. Escolhemos ReFS como o sistema de arquivos (para uma criação mais rápida e pontos de verificação) e o espelhamento de três vias para resiliência a fim de maximizar o desempenho. Vamos colocar o armazenamento frio em outros dois volumes: *Volume 3* e *Volume 4*. Escolhemos NTFS como o sistema de arquivos (para Eliminação de Duplicação de Dados) e a paridade dupla para resiliência a fim de maximizar a capacidade.

Não precisamos criar todos os volumes do mesmo tamanho, mas para simplificar, vamos fazer isso. Por exemplo, podemos criar todos com 12 TB.

Os *Volume1* e *Volume2* ocuparão cada um 12 TB x 33,3% de eficiência = 36 TB de capacidade de armazenamento físico.

Os *Volume3* e *Volume4* ocuparão cada um 12 TB x 50,0% de eficiência = 24 TB de capacidade de armazenamento físico.

```
36 TB + 36 TB + 24 TB + 24 TB = 120 TB
```

Os quatro volumes se encaixam exatamente na capacidade de armazenamento físico disponível no nosso pool. Perfeito!

![exemplo](media/plan-volumes/example.png)

   >[!TIP]
   > Você não precisa criar todos os volumes imediatamente. Sempre é possível estender volumes ou criar novos volumes mais tarde.

Para simplificar, este exemplo usa unidades decimais (base 10), ou seja, 1 TB = 1.000.000.000.000 bytes. No entanto, as quantidades de armazenamento no Windows aparecem em unidades binários (base 2). Por exemplo, cada unidade de 2 TB apareceria como 1,82 TiB no Windows. Da mesma forma, o pool de armazenamento de 128 TB apareceria como 116,41 TiB. Isso é esperado.

## <a name="usage"></a>Uso

Consulte [Criando volumes em Espaços de Armazenamento Diretos](create-volumes.md).

### <a name="see-also"></a>Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Escolhendo unidades para Espaços de Armazenamento Diretos](choosing-drives.md)
- [Tolerância a falhas e eficiência de armazenamento](storage-spaces-fault-tolerance.md)
