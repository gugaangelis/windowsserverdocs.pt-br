---
title: Criando volumes em Espaços de Armazenamento Diretos
description: Como criar volumes no Espaços de Armazenamento Diretos usando o centro de administração do Windows e o PowerShell.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 06/06/2019
ms.openlocfilehash: 8c17671f2f15d1373973dcf2fbafc753f0a163a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402886"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Criando volumes em Espaços de Armazenamento Diretos

> Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico descreve como criar volumes em um cluster Espaços de Armazenamento Diretos usando o centro de administração do Windows, o PowerShell ou o Gerenciador de Cluster de Failover.

> [!TIP]
> Se você ainda não fez isto, consulte primeiro [Planejando volumes em Espaços de Armazenamento Diretos](plan-volumes.md).

## <a name="create-a-three-way-mirror-volume"></a>Criar um volume de espelho de três vias

Para criar um volume de espelho de três vias no centro de administração do Windows: 

1. No centro de administração do Windows, conecte-se a um cluster Espaços de Armazenamento Diretos e, em seguida, selecione **volumes** no painel **ferramentas** .
2. Na página volumes, selecione a guia **inventário** e, em seguida, selecione **criar volume**.
3. No painel **criar volume** , insira um nome para o volume e deixe a **resiliência** como um **espelho de três vias**.
4. Em **tamanho no HDD**, especifique o tamanho do volume. Por exemplo, 5 TB (terabytes).
5. Selecione **Criar**.

Dependendo do tamanho, a criação do volume pode levar alguns minutos. As notificações no canto superior direito permitirão que você saiba quando o volume é criado. O novo volume aparece na lista de inventário.

Assista a um vídeo rápido sobre como criar um volume de espelho de três vias.

> [!VIDEO https://www.youtube-nocookie.com/embed/o66etKq70N8]

## <a name="create-a-mirror-accelerated-parity-volume"></a>Criar um volume de paridade acelerado por espelhamento

A paridade acelerada por espelhamento reduz a superfície do volume no HDD. Por exemplo, um volume de espelho de três vias significaria que, para cada 10 terabytes de tamanho, você precisará de 30 terabytes como superfície. Para reduzir a sobrecarga no espaço, crie um volume com paridade acelerada por espelho. Isso reduz a superfície de 30 terabytes a apenas 22 terabytes, mesmo com apenas 4 servidores, espelhando os 20% mais ativos de dados e usando a paridade, que é mais eficiente em termos de espaço para armazenar o restante. Você pode ajustar essa taxa de paridade e espelhamento para tornar o desempenho versus o equilíbrio de capacidade ideal para sua carga de trabalho. Por exemplo, 90% de paridade e 10% de espelhamento gera menos desempenho, mas simplifica ainda mais a superfície.

Para criar um volume com paridade acelerada por espelhamento no centro de administração do Windows:

1. No centro de administração do Windows, conecte-se a um cluster Espaços de Armazenamento Diretos e, em seguida, selecione **volumes** no painel **ferramentas** .
2. Na página volumes, selecione a guia **inventário** e, em seguida, selecione **criar volume**.
3. No painel **criar volume** , insira um nome para o volume.
4. Em **resiliência**, selecione **paridade acelerada por espelhamento**.
5. Em **porcentagem de paridade**, selecione a porcentagem de paridade.
6. Selecione **Criar**.

Assista a um vídeo rápido sobre como criar um volume de paridade acelerado por espelhamento.

> [!VIDEO https://www.youtube-nocookie.com/embed/R72QHudqWpE]

## <a name="open-volume-and-add-files"></a>Abrir volume e adicionar arquivos

Para abrir um volume e adicionar arquivos ao volume no centro de administração do Windows:

1. No centro de administração do Windows, conecte-se a um cluster Espaços de Armazenamento Diretos e, em seguida, selecione **volumes** no painel **ferramentas** .
2. Na página volumes, selecione a guia **inventário** .
2. Na lista de volumes, selecione o nome do volume que você deseja abrir.

    Na página detalhes do volume, você pode ver o caminho para o volume.

4. Na parte superior da página, selecione **abrir**. Isso inicia a ferramenta arquivos no centro de administração do Windows.
5. Navegue até o caminho do volume. Aqui você pode procurar os arquivos no volume.
6. Selecione **carregar**e, em seguida, selecione um arquivo para carregar.
7. Use o botão **voltar** do navegador para voltar ao painel de ferramentas no centro de administração do Windows.

Assista a um vídeo rápido sobre como abrir um volume e adicionar arquivos.

> [!VIDEO https://www.youtube-nocookie.com/embed/j59z7ulohs4]

## <a name="turn-on-deduplication-and-compression"></a>Ativar a eliminação de duplicação e a compactação

A eliminação de duplicação e a compactação são gerenciadas por volume. A eliminação de duplicação e a compactação usam um modelo de pós-processamento, o que significa que você não verá economias até que ele seja executado. Quando isso ocorrer, ele trabalhará em todos os arquivos, mesmo aqueles que estavam lá antes.

1. No centro de administração do Windows, conecte-se a um cluster Espaços de Armazenamento Diretos e, em seguida, selecione **volumes** no painel **ferramentas** .
2. Na página volumes, selecione a guia **inventário** .
3. Na lista de volumes, selecione o nome do volume que deseja gerenciar.
4. Na página detalhes do volume, clique na opção rotulada **eliminação de duplicação e compactação**.
5. No painel habilitar eliminação de duplicação, selecione o modo de eliminação de duplicação.

    Em vez de configurações complicadas, o centro de administração do Windows permite que você escolha entre perfis prontos para cargas de trabalho diferentes. Se você não tiver certeza, use a configuração padrão.

6. Selecione **Habilitar**.

Assista a um vídeo rápido sobre como ativar a eliminação de duplicação e a compactação.

> [!VIDEO https://www.youtube-nocookie.com/embed/PRibTacyKko]

## <a name="create-volumes-using-powershell"></a>Criar volumes usando o PowerShell

Recomendamos usar o cmdlet **New-Volume** para criar volumes para Espaços de Armazenamento Diretos. Isso fornece a experiência mais rápida e mais direta. Este cmdlet único cria automaticamente o disco virtual, partições, formata-o, cria o volume com o nome correspondente e o adiciona a volumes de cluster compartilhados – tudo em uma etapa simples.

O cmdlet **New-Volume** tem quatro parâmetros que você sempre precisará fornecer:

- **FriendlyName** Qualquer cadeia de caracteres que você desejar, por exemplo, *"Volume1"*
- **WPD** **CSVFS_ReFS** (recomendado) ou **CSVFS_NTFS**
- **StoragePoolFriendlyName:** O nome do seu pool de armazenamento, por exemplo, *"S2D em ClusterName"*
- **Tamanho:** O tamanho do volume, por exemplo, *"10 TB"*

   > [!NOTE]
   > O Windows, inclusive o PowerShell, conta usando números binários (base-2), embora as unidades geralmente sejam rotuladas usando números decimais de (base-10). Isso explica por que uma unidade de "um terabyte", definida como 1.000.000.000.000 bytes, aparece no Windows como de "909 GB". Isso é esperado. Ao criar volumes usando **New-Volume**, você deve especificar o parâmetro **Tamanho** em números binários (base-2). Por exemplo, especificar "909GB" ou "0,909495 TB" criará um volume de aproximadamente 1.000.000.000.000 bytes.

### <a name="example-with-2-or-3-servers"></a>Exemplo: Com 2 ou três servidores

Para facilitar as coisas, se sua implantação tem apenas dois servidores, Espaços de Armazenamento Diretos usará automaticamente o espelhamento de duas vias para resiliência. Se sua implantação tiver apenas três servidores, o espelhamento de três vias será usado automaticamente.

```PowerShell
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>Exemplo: Com mais de 4 servidores

Se tiver quatro ou mais servidores, você pode usar o parâmetro opcional **ResiliencySettingName** para escolher o tipo de resiliência.

-   **ResiliencySettingName** **Espelho** ou **paridade**.

No exemplo a seguir, o *"Volume2"* usa espelhamento de três vias e o *"Volume3"* usa paridade dupla (geralmente chamada de "codificação de eliminação").

```PowerShell
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>Exemplo: Usando camadas de armazenamento

Em implantações com três tipos de unidades, um volume pode abranger as camadas SSD e HDD para residir parcialmente em cada uma delas. Da mesma forma, em implantações com quatro ou mais servidores, um volume pode misturar espelhamento e paridade dual para residir parcialmente em cada um.

Para ajudá-lo a criar esses volumes, Espaços de Armazenamento Diretos fornece modelos de camada padrão chamados *Desempenho* e *Capacidade*. Eles encapsulam definições para espelhamento de três vias nas unidades mais rápidas de capacidade (se aplicável) e paridade dual nas unidades mais lentas de capacidade (se aplicável).

Você pode vê-las, executando o cmdlet **Get-StorageTier**.

```PowerShell
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![Captura de tela de PowerShell de camadas de armazenamento](media/creating-volumes/storage-tiers-screenshot.png)

Para criar volumes em camadas, referencie esses modelos de camadas usando os parâmetros **StorageTierFriendlyNames** e **StorageTierSizes** do cmdlet **New-Volume**. Por exemplo, o seguinte cmdlet cria um volume que combina espelhamento de três vias e paridade dual em proporções 30:70.

```PowerShell
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

## <a name="create-volumes-using-failover-cluster-manager"></a>Criar volumes usando o Gerenciador de Cluster de Failover

Você também pode criar volumes usando o *Assistente de Novo Disco Virtual (Espaços de Armazenamento Diretos)* , seguido pelo *Assistente de Novo Volume* do Gerenciador de Cluster de Failover, embora esse fluxo de trabalho tenha muitas etapas manuais e não seja recomendado.

Há três etapas principais:

### <a name="step-1-create-virtual-disk"></a>Etapa 1: Criar disco virtual

![Novo disco virtual](media/creating-volumes/GUI-Step-1.png)

1. No Gerenciador de Cluster de Failover, navegue até **Armazenamento** -> **Pools**.
2. Selecione **Novo Disco Virtual** no painel Ações à direita, ou clique com o botão direito no pool e selecione **Novo Disco Virtual**.
3. Selecione o pool de armazenamento clique em **OK**. O *Assistente de Novo disco Virtual (Espaços de Armazenamento Diretos)* será aberto.
4. Use o Assistente para nomear o disco virtual e especificar seu tamanho.
5. Revise suas seleções e clique em **Criar**.
6. Não se esqueça de marcar a caixa de seleção **Criar um volume quando este assistente se fechar** antes de fechar.

### <a name="step-2-create-volume"></a>Etapa 2: Criar volume

O *Assistente de Novo Volume* é aberto.

7. Selecione o disco virtual recém-criado e clique em **Próximo**.
8. Especifique o tamanho do volume (padrão: do mesmo tamanho que o disco virtual) e clique em **Próximo**. 
9. Atribua o volume a uma letra de unidade ou escolha **Não atribuir uma letra de unidade** e clique em **Próximo**.
10. Especifique o filesystem a ser usado, deixe o tamanho da unidade de alocação como *Padrão*, nomeie o volume e clique em **Próximo**.
11. Revise suas seleções e clique em **Criar** e, em seguida, em **Fechar**.

### <a name="step-3-add-to-cluster-shared-volumes"></a>Etapa 3: Adicionar a volumes compartilhados do cluster

![Adicionar a Volumes Compartilhados Clusterizados](media/creating-volumes/GUI-Step-2.png)

12. No Gerenciador de Cluster de Failover, navegue até **Armazenamento** -> **Discos**.
13. Selecione o disco virtual que você acabou de criar e selecione **Adicionar a Volumes Compartilhados do Cluster** no painel Ações à direita, ou clique com o botão direito do mouse no disco virtual e selecione **Adicionar a Volumes Compartilhados do Cluster**.

Concluído! Repita conforme necessário para criar mais de um volume.

## <a name="see-also"></a>Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Planejando volumes no Espaços de Armazenamento Diretos](plan-volumes.md)
- [Estendendo volumes no Espaços de Armazenamento Diretos](resize-volumes.md)
- [Excluindo volumes no Espaços de Armazenamento Diretos](delete-volumes.md)
