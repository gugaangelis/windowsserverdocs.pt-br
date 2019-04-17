---
title: Paridade acelerada por espelho
ms.prod: windows-server-threshold
ms.author: gawatu
ms.manager: masriniv
ms.technology: storage-file-systems
ms.topic: article
author: gawatu
ms.date: 8/9/2017
ms.assetid: 
ms.openlocfilehash: a94bc4f8251d0f1fb18c3dad95c2915d3346c69a
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="mirror-accelerated-parity"></a>Paridade acelerada por espelho
>Aplica-se ao Windows Server 2016

Espaços de armazenamento podem fornecer tolerância a falhas de dados usando duas técnicas fundamentais: espelhamento e paridade. Em [Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md), ReFS apresenta a paridade acelerada por espelho, que permite a criação de volumes que usam as resiliências de espelhamento e paridade. A paridade acelerada por espelho oferece armazenamento acessível com espaço eficiente sem sacrificar o desempenho. 

![Volume de paridade acelerada por espelho](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume.png)


## <a name="background"></a>Contexto
Esquemas de resiliência de espelhamento e paridade têm características de armazenamento e desempenho fundamentalmente diferentes:
- A resiliência por espelhamento permite que os usuários obtenham desempenho de gravação rápido, mas a replicação de dados para cada cópia não é eficiente com relação ao espaço. 
- A paridade, por outro lado, deve ser computada novamente para cada gravação, causando danos aleatórios ao desempenho de gravação. A paridade, no entanto, permite que os usuários armazenem seus dados com maior eficiência de espaço. (Saiba mais sobre a eficiência de espaço ![aqui](../storage-spaces/Storage-Spaces-Fault-Tolerance.md)). 

Sendo assim, o espelhamento está pré-disposto a fornecer armazenamento sensível ao desempenho enquanto a paridade oferece utilização de capacidade de armazenamento aprimorada. Em paridade acelerada por espelho, ReFS aproveita os benefícios de cada tipo de resiliência para fornecer armazenamento de alto desempenho e de capacidade eficiente combinando esquemas de resiliência com um único volume.


## <a name="data-rotation-on-mirror-accelerated-parity"></a>Rotação de dados em paridade acelerada por espelho

ReFS ativamente gira dados entre espelhamento e paridade, em tempo real. Isso permite que as gravações recebidas sejam rapidamente registradas ao espelhamento e depois rotacionadas a paridade para serem armazenadas de forma eficiente. Dessa forma, E/S de entrada é atendido rapidamente em espelhamento enquanto dados menos acessados são armazenados de forma eficiente na paridade, fornecendo desempenho ideal e armazenamento de custo perdido dentro do mesmo volume. 

Para girar dados entre espelhamento e paridade, ReFS logicamente divide o volume em regiões de 64 MiB, que são a unidade de rotação. A imagem a seguir mostra um volume de paridade com aceleração de espelho dividido em regiões. 

![Volume de paridade acelerada por espelho com contêiners de armazenamento](media/mirror-accelerated-parity/Mirror-Accelerated-Parity-Volume-with-Storage-Containers.png)

ReFS começa a rodar regiões completas de espelhamento a paridade uma vez que a camada do espelhamento tiver alcançado um nível de capacidade especificado. Em vez de imediatamente mover os dados de espelhamento a paridade, ReFS aguarda e mantém dados no espelhamento por quanto tempo for possível, permitindo que o ReFS continue entregando desempenho ideal de dados (consulte "desempenho de E/S" abaixo). 

Quando os dados são movidos de espelhamento para paridade, os dados são lidos, as codificações de paridade são computadas, e depois que os dados são escritos na paridade. A animação abaixo ilustra isso usando uma região espelhada tripla que é convertida em uma região codificada de eliminação durante rotação:

![Rotação de paridade acelerada por espelho](media/mirror-accelerated-parity/Container-Rotation.gif)

## <a name="io-on-mirror-accelerated-parity"></a>E/S de paridade acelerada por espelho
### <a name="io-behavior"></a>Comportamento de E/S
**Gravações:** ReFS oferece manutenção a gravações recebidas de três maneiras distintas:
1.  **Grava no espelho:** 
     - **a.** Se a gravação de entrada modificar os dados existentes no espelho, ReFS modificará os dados no local. 
     - **b.** Se a gravação de entrada for uma nova gravação e o ReFS encontrar com êxito espaço livre suficiente em espelho para suportar essa gravação, o ReFS escreve no espelho. 

![Gravar no espelho](media/mirror-accelerated-parity/Write-to-Mirror.png)

2. **Grava no espelho, Realocado da paridade:**
    - **a.** Se a gravação de entrada modificar os dados que estiverem na paridade e o ReFS encontrar com êxito espaço livre suficiente em espelho para atender a gravação de entrada, o ReFS primeiro invalidará os dados anteriores em paridade e, em seguida, registrará no espelho. Essa invalidação é uma operação de metadados rápida e barata que ajuda a aumentar significativamente o desempenho de gravação feito a paridade.

![Gravação realocada](media/mirror-accelerated-parity/Reallocated-Write.png)

3. **Grava na paridade:**
    - **a.** Se o ReFS não conseguir encontrar com sucesso espaço livre suficiente no espelhamento, o ReFS escreverá novos dados na paridade ou modificará dados existentes na paridade diretamente. A seção "Otimizações de desempenho" abaixo fornece orientações que ajudam a minimizar gravações na paridade.

![Grava na paridade](media/mirror-accelerated-parity/Write-to-Parity.png)

**Leituras:** ReFS lerá diretamente da camada que contém os dados relevantes. Se a paridade é construída com HDDs, o cache em espaços de armazenamento direto armazenará em cache esses dados para acelerar leituras futuras. 

> [!NOTE]
> Leituras nunca causam ReFS para girar dados de volta para a faixa de espelho. 

### <a name="io-performance"></a>Desempenho de E/S:

**Gravações:** cada tipo de gravação descrito acima tem suas próprias características de desempenho. Em termos gerais, gravações para o nível de espelho são muito mais rápidas que as gravações realocadas, e estas são significativamente mais rápidas que gravações feitas diretamente para a faixa de paridade. Essa relação é ilustrada com a desigualdade abaixo: 


- **Camada de espelho > Leituras realocadas >> Camada de paridade**


**Leituras:** não há nenhum impacto de desempenho significativo, negativo durante a leitura de paridade:
- Se o espelho e a paridade forem construídos com o mesmo tipo de mídia, o desempenho de leitura será equivalente. 
- Se o espelho e a paridade são construídos com diferentes tipos de mídia — espelhado SSDs, paridade HDDs, por exemplo —[o cache no Espaços de Armazenamento Diretos](../storage-spaces/understand-the-cache.md) ajudará cache de dados muito utilizados para acelerar quaisquer leituras de paridade.

## <a name="refs-compaction"></a>Compactação de ReFS
Na versão semestral deste segundo semestre, ReFS apresenta compactação, o que melhora significativamente o desempenho para volumes de paridade com aceleração de espelho que estão 90% completos. 

**Em segundo plano:** anteriormente, conforme os volumes de paridade com aceleração de espelho se tornam completos, o desempenho desses volumes pode ser degradado. O desempenho degrada porque dados altos e baixos se tornam mistos em todos os volumes extras. Isso significa que menos dados quentes podem ser armazenados em espelho porque os dados quentes ocupam espaço no espelho que pode ser, de outra forma, usado por dados quentes. Armazenando dados quentes em espelho é fundamental para manter o alto desempenho como gravações diretas para espelhar de forma muito mais rápida as leituras e gravações realocadas de magnitude mais rápido do que grava diretamente à paridade. Dessa forma, ter dados frios em espelho é ruim para o desempenho, pois reduz a probabilidade do ReFS poder gravar diretamente ao espelho. 

Compactação de ReFS corrige esses problemas de desempenho, liberando espaço no espelho para dados quentes. A compactação primeiro consolida todos os dados — de espelho e paridade — em paridade. Isso reduz a fragmentação dentro do volume e aumenta a quantidade de espaço endereçável no espelho. Mais importante, esse processo permite que o ReFS consolide dados quentes novamente em espelho:
-   Quando chegam novas gravações, elas serão mais usados no espelho. Assim, dados recém-criados ou quentes residem em espelho. 
-   Quando uma gravação de modificação é feita em dados em paridade, o ReFS torna uma gravação realocada, para essa gravação ser atendida no espelho também. Consequentemente, dados quentes que foram movidos para paridade durante compactação serão realocados novamente em espelho. 

## <a name="performance-optimizations"></a>Otimizações de desempenho

>[!IMPORTANT]
>Para cargas de trabalho do Hyper-V em paridade com aceleração de espelho, o ReFS oferece o melhor desempenho quando VHDs com uso intenso de gravação são distribuídos em vários diretórios. Assim, para obter o desempenho ideal, é recomendável não colocar muitos, com uso intenso de gravação VHDs no mesmo subdiretório.   

### <a name="performance-counters"></a>Contadores de desempenho: 

ReFS mantém contadores de desempenho para ajudar a avaliar o desempenho de paridade com aceleração de espelho. 
-   Conforme descrito anteriormente na gravação à seção de paridade, o ReFS gravará diretamente a paridade quando ela não conseguir localizar espaço livre no espelho. Em geral, isso ocorre quando a faixa de espelho preenche mais rápido do que o ReFS consegue girar dados na paridade. Em outras palavras, a rotação ReFS não é capaz de acompanhar a taxa de inclusão. Os contadores de desempenho abaixo identificam quando o ReFS grava diretamente na paridade:
```
ReFS\Data allocations slow tier/sec
ReFS\Metadata allocations slow tier/sec
```
-   Se esses contadores forem diferentes de zero, isso indica que o ReFS não está girando dados rápidos o suficiente de espelho. Para ajudar a minimizar esse problema, um pode mudar a agressividade de rotação ou aumentar o tamanho da camada espelhada.

### <a name="rotation-aggressiveness"></a>Agressividade de rotação:
O ReFS começa a girar dados depois do espelho atingir um limite especificado de capacidade.
-   Valores maiores desse limite de rotação fazem o ReFS reter dados na camada de espelho por mais tempo. Deixar dados quentes na camada de espelho é ideal para o desempenho, mas o ReFS não será capaz de suportar de forma eficaz grandes quantidades de E/S de entrada. 
-   Valores menores permitem que o ReFS transfira proativamente dados e receba da melhor forma os E/S de entrada. Isso é aplicável a cargas de trabalho pesados para processamento, como armazenamento de arquivamento. Valores menores, no entanto, podem degradar o desempenho para cargas de trabalho de finalidade geral. Girar desnecessariamente dados fora o nível de espelho carregará uma penalidade de desempenho. 

O ReFS apresenta um parâmetro ajustável para ajustar esse limite, que é configurável usando uma chave do registro. Essa chave do registro deve ser configurada em **cada nó em uma implantação direta de espaços de armazenamento**, e uma reinicialização é necessária para que todas as alterações entrem em vigor. 
-   **Key:** HKEY_LOCAL_MACHINE\System\CurrentControlSet\Policies
-   **ValueName (DWORD):** DataDestageSsdFillRatioThreshold
-   **ValueType:** Percentage

Se essa chave do registro não estiver definida, o ReFS usará um valor padrão de 85%.  Esse valor padrão é recomendado para a maioria das implantações e valores abaixo de 50% não são recomendados. O comando do PowerShell a seguir demonstra como definir essa chave do registro com um valor de 75%: 
```
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75
 ```
 Para configurar essa chave do registro entre cada nó em uma implantação Espaços de armazenamento diretos, você pode usar o comando do PowerShell abaixo:
 ```
 $Nodes = 'S2D-01', 'S2D-02', 'S2D-03', 'S2D-04'
 Invoke-Command $Nodes {Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Policies -Name DataDestageSsdFillRatioThreshold -Value 75}
 ```

### <a name="increasing-the-size-of-the-mirrored-tier"></a>Aumentando o tamanho da camada espelhada: 
Aumentar o tamanho da camada espelhada permite que o ReFS mantenha uma maior parte do conjunto de trabalho no espelho. Isso aumenta a probabilidade de que ReFS podem gravar diretamente no espelho, o que ajudará a obter um desempenho melhor. Os cmdlets do PowerShell abaixo demonstram como aumentar o tamanho da camada espelhada:
```
Resize-StorageTier -FriendlyName “Performance” -Size 20GB
Resize-StorageTier -InputObject (Get-StorageTier -FriendlyName “Performance”) -Size 20GB
```
>[!TIP]
>Certifique-se de redimensionar a **Partição** e o **Volume** depois de redimensionar o **StorageTier**. Para obter mais informações e exemplos, consulte [Redimensionar volumes](../storage-spaces/resize-volumes.md).

## <a name="creating-a-mirror-accelerated-parity-volume"></a>Criando um volume de paridade acelerada por espelho
O cmdlet do PowerShell abaixo cria um volume de aceleração de paridade de espelho com uma taxa de espelho: paridade de 20:80, que é a configuração recomendada para a maioria das cargas de trabalho. Para obter mais informações e exemplos, consulte [Criando volumes em Espaços de Armazenamento Diretos](../storage-spaces/Create-volumes.md).

```
New-Volume – FriendlyName “TestVolume” -FileSystem CSVFS_ReFS -StoragePoolFriendlyName “StoragePoolName” -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 200GB, 800GB
```

## <a name="see-also"></a>Veja também

-   [Visão geral do ReFS](refs-overview.md)
-   [Clonagem de blocos ReFS](block-cloning.md)
-   [Fluxos de integridade ReFS](integrity-streams.md)
-   [Visão geral de Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md)
