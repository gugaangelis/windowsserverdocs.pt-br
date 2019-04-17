---
ms.assetid: a9f229eb-bef4-4231-97d0-0899e17cef32
title: Criando volumes em Espaços de Armazenamento Diretos
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/11/2017
ms.localizationpriority: medium
ms.openlocfilehash: 277a676d8e53a7847d54039aab6607be8e5a78c5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1833428"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Criando volumes em Espaços de Armazenamento Diretos

>Aplica-se a: Windows Server 2016

Este tópico descreve como criar volumes em Espaços de Armazenamento Diretos usando o PowerShell ou Gerenciador de Cluster de Failover.

   >[!TIP]
   >  Se você ainda não fez isto, consulte primeiro [Planejando volumes em Espaços de Armazenamento Diretos](plan-volumes.md).

## <a name="create-volumes-using-powershell"></a>Criar volumes usando o PowerShell

Recomendamos usar o cmdlet **New-Volume** para criar volumes para Espaços de Armazenamento Diretos. Isso fornece a experiência mais rápida e mais direta. Este cmdlet único cria automaticamente o disco virtual, partições, formata-o, cria o volume com o nome correspondente e o adiciona a volumes de cluster compartilhados – tudo em uma etapa simples.

O cmdlet **New-Volume** tem quatro parâmetros que você sempre precisará fornecer:

- **FriendlyName:** qualquer cadeia de caracteres que deseje, por exemplo *"Volume1"*
- **FileSystem:** ou **CSVFS_ReFS** (recomendado) ou **CSVFS_NTFS**
- **StoragePoolFriendlyName:** o nome do seu pool de armazenamento, por exemplo *"S2D em ClusterName"*
- **Tamanho:** o tamanho do volume, por exemplo *"10TB"*

   >[!NOTE]
   >  O Windows, inclusive o PowerShell, conta usando números binários (base-2), embora as unidades geralmente sejam rotuladas usando números decimais de (base-10). Isso explica por que uma unidade de "um terabyte", definida como 1.000.000.000.000 bytes, aparece no Windows como de "909 GB". Isso é esperado. Ao criar volumes usando **New-Volume**, você deve especificar o parâmetro **Tamanho** em números binários (base-2). Por exemplo, especificar "909GB" ou "0,909495 TB" criará um volume de aproximadamente 1.000.000.000.000 bytes.

### <a name="example-with-2-or-3-servers"></a>Exemplo: com 2 ou 3 servidores

Para facilitar as coisas, se sua implantação tem apenas dois servidores, Espaços de Armazenamento Diretos usará automaticamente o espelhamento de duas vias para resiliência. Se sua implantação tiver apenas três servidores, o espelhamento de três vias será usado automaticamente.

```
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>Exemplo: com 4 ou mais servidores

Se tiver quatro ou mais servidores, você pode usar o parâmetro opcional **ResiliencySettingName** para escolher o tipo de resiliência.

-   **ResiliencySettingName:** ou **Espelho** ou **Paridade**.

No exemplo a seguir, o *"Volume2"* usa espelhamento de três vias e o *"Volume3"* usa paridade dupla (geralmente chamada de "codificação de eliminação").

```
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>Exemplo: usando camadas de armazenamento

Em implantações com três tipos de unidades, um volume pode abranger as camadas SSD e HDD para residir parcialmente em cada uma delas. Da mesma forma, em implantações com quatro ou mais servidores, um volume pode misturar espelhamento e paridade dual para residir parcialmente em cada um.

Para ajudá-lo a criar esses volumes, Espaços de Armazenamento Diretos fornece modelos de camada padrão chamados *Desempenho* e *Capacidade*. Eles encapsulam definições para espelhamento de três vias nas unidades mais rápidas de capacidade (se aplicável) e paridade dual nas unidades mais lentas de capacidade (se aplicável).

Você pode vê-las, executando o cmdlet **Get-StorageTier**.

```
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![Captura de tela de PowerShell de camadas de armazenamento](media/creating-volumes/storage-tiers-screenshot.png)

Para criar volumes em camadas, referencie esses modelos de camadas usando os parâmetros **StorageTierFriendlyNames** e **StorageTierSizes** do cmdlet **New-Volume**. Por exemplo, o seguinte cmdlet cria um volume que combina espelhamento de três vias e paridade dual em proporções 30:70.

```
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

## <a name="create-volumes-using-failover-cluster-manager"></a>Criar volumes usando o Gerenciador de Cluster de Failover

Você também pode criar volumes usando o *Assistente de Novo Disco Virtual (Espaços de Armazenamento Diretos)*, seguido pelo *Assistente de Novo Volume* do Gerenciador de Cluster de Failover, embora esse fluxo de trabalho tenha muitas etapas manuais e não seja recomendado.

Há três etapas principais:

### <a name="step-1-create-virtual-disk"></a>Etapa 1: criar um disco virtual

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

### <a name="step-3-add-to-cluster-shared-volumes"></a>Etapa 3: Adicionar a Volumes Compartilhados do Cluster

![Adicionar a Volumes Compartilhados do Cluster](media/creating-volumes/GUI-Step-2.png)

12. No Gerenciador de Cluster de Failover, navegue até **Armazenamento** -> **Discos**.
13. Selecione o disco virtual que você acabou de criar e selecione **Adicionar a Volumes Compartilhados do Cluster** no painel Ações à direita, ou clique com o botão direito do mouse no disco virtual e selecione **Adicionar a Volumes Compartilhados do Cluster**.

Pronto! Repita conforme necessário para criar mais de um volume.

## <a name="see-also"></a>Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- [Planejamento de volumes nos Espaços de Armazenamento Diretos](plan-volumes.md)
