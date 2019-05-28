---
title: Criando volumes em Espaços de Armazenamento Diretos
description: Como criar volumes em espaços de armazenamento diretos usando o PowerShell e Windows Admin Center.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: cosmosdarwin
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.date: 05/09/2019
ms.openlocfilehash: d7c842a9b393f67c482dadeaa4090627887a67a3
ms.sourcegitcommit: 75f257d97d345da388cda972ccce0eb29e82d3bc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65613219"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Criando volumes em Espaços de Armazenamento Diretos

>Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico descreve como criar volumes em um cluster de espaços de armazenamento diretos usando o Windows Admin Center, o PowerShell ou Gerenciador de Cluster de Failover.

   >[!TIP]
   >  Se você ainda não fez isto, consulte primeiro [Planejando volumes em Espaços de Armazenamento Diretos](plan-volumes.md).

## <a name="create-a-three-way-mirror-volume"></a>Criar um volume de três vias

Para criar um volume de três vias no Windows Admin Center: 

1. No do Windows Admin Center, conectar a um cluster de espaços de armazenamento diretos e, em seguida, selecione **Volumes** da **ferramentas** painel.
2. Na página de Volumes, selecione a **estoque** guia e, em seguida, selecione **Criar volume**.
3. No **Criar volume** painel, insira um nome para o volume e deixe **resiliência** como **espelho triplo**.
4. Na **tamanho de unidade de disco rígido**, especifique o tamanho do volume. Por exemplo, 5 TB (terabytes).
5. Selecione **Criar**.

Dependendo do tamanho, criar o volume pode levar alguns minutos. As notificações no canto superior direito permitem que você sabe quando o volume é criado. O novo volume é exibida na lista de inventário.

Assista a um vídeo rápido sobre como criar um volume de três vias.

> [!VIDEO https://www.youtube-nocookie.com/embed/o66etKq70N8]

## <a name="create-a-mirror-accelerated-parity-volume"></a>Criar um volume de paridade de aceleração de espelho

Aceleração de espelho paridade reduz o volume do volume em que o disco rígido. Por exemplo, um volume de três vias significa que para cada 10 terabytes de tamanho, você precisará 30 terabytes como volume. Para reduzir a sobrecarga na superfície, crie um volume com a paridade de aceleração de espelho. Isso reduz a superfície de 30 terabytes para apenas 22 terabytes, mesmo com apenas 4 servidores, os 20% mais ativos de dados de espelhamento e, em seguida, usar a paridade, o que é mais eficiente para armazenar o resto de espaço. Você pode ajustar essa proporção de espelho para tornar o desempenho em relação ao equilíbrio de capacidade que é ideal para sua carga de trabalho e de paridade. Por exemplo, 90 por cento 10 por cento e paridade espelho gera um desempenho menor, mas simplifica ainda mais o volume.

Para criar um volume com aceleração de espelho paridade no Windows Admin Center:

1. No do Windows Admin Center, conectar a um cluster de espaços de armazenamento diretos e, em seguida, selecione **Volumes** da **ferramentas** painel.
2. Na página de Volumes, selecione a **estoque** guia e, em seguida, selecione **Criar volume**.
3. No **Criar volume** painel, insira um nome para o volume.
4. Na **resiliência**, selecione **acelerada de espelho paridade**.
5. Na **porcentagem de paridade**, selecione a porcentagem de paridade.
6. Selecione **Criar**.

Assista a um vídeo rápido sobre como criar um volume de paridade de aceleração de espelho.

> [!VIDEO https://www.youtube-nocookie.com/embed/R72QHudqWpE]

## <a name="open-volume-and-add-files"></a>Abra o volume e adicionar arquivos

Para abrir um volume e adicionar arquivos ao volume no Windows Admin Center:

1. No do Windows Admin Center, conectar a um cluster de espaços de armazenamento diretos e, em seguida, selecione **Volumes** da **ferramentas** painel.
2. Na página de Volumes, selecione a **inventário** guia.
2. Na lista de volumes, selecione o nome do volume que você deseja abrir.

    Na página de detalhes de volume, você pode ver o caminho para o volume.

4. Na parte superior da página, selecione **aberto**. Isso inicia a ferramenta arquivos do Windows Admin Center.
5. Navegue até o caminho do volume. Aqui você pode procurar os arquivos no volume.
6. Selecione **carregar**e, em seguida, selecione um arquivo para carregar.
7. Use o navegador **volta** botão para voltar ao painel de ferramentas no Windows Admin Center.

Assista a um vídeo rápido sobre como abrir um volume e adicionar arquivos.

> [!VIDEO https://www.youtube-nocookie.com/embed/j59z7ulohs4]

## <a name="turn-on-deduplication-and-compression"></a>Ativar a eliminação de duplicação e compactação

Eliminação de duplicação e compactação é gerenciado por volume. Eliminação de duplicação e compactação usa um modelo de pós-processamento, o que significa que você não verá a economia de até que ele seja executado. Quando isso acontecer, trabalhará em todos os arquivos, mesmo aqueles que não eram lá antes.

1. No do Windows Admin Center, conectar a um cluster de espaços de armazenamento diretos e, em seguida, selecione **Volumes** da **ferramentas** painel.
2. Na página de Volumes, selecione a **inventário** guia.
3. Na lista de volumes, selecione o nome do volume que deseja gerenciar.
4. Na página de detalhes de volume, clique na opção rotulada **eliminação de duplicação e compactação**.
5. No painel de eliminação de duplicação de ativar, selecione o modo de eliminação de duplicação.

    Em vez de configurações complicadas, o Windows Admin Center permite que você escolha entre os perfis prontos para diferentes cargas de trabalho. Se você não tiver certeza, use a configuração padrão.

6. Selecione **Habilitar**.

Assista a um vídeo rápido sobre como ativar a eliminação de duplicação e compactação.

> [!VIDEO https://www.youtube-nocookie.com/embed/PRibTacyKko]

## <a name="create-volumes-using-powershell"></a>Criar volumes usando o PowerShell

Recomendamos usar o cmdlet **New-Volume** para criar volumes para Espaços de Armazenamento Diretos. Isso fornece a experiência mais rápida e mais direta. Este cmdlet único cria automaticamente o disco virtual, partições, formata-o, cria o volume com o nome correspondente e o adiciona a volumes de cluster compartilhados – tudo em uma etapa simples.

O cmdlet **New-Volume** tem quatro parâmetros que você sempre precisará fornecer:

- **FriendlyName:** Qualquer cadeia de caracteres que você queira, por exemplo *"Volume1"*
- **FileSystem:** Qualquer um dos **CSVFS_ReFS** (recomendado) ou **CSVFS_NTFS**
- **StoragePoolFriendlyName:** O nome do seu armazenamento em pool, por exemplo *"S2D no ClusterName"*
- **Tamanho:** O tamanho do volume, por exemplo *"10 TB"*

   >[!NOTE]
   >  O Windows, inclusive o PowerShell, conta usando números binários (base-2), embora as unidades geralmente sejam rotuladas usando números decimais de (base-10). Isso explica por que uma unidade de "um terabyte", definida como 1.000.000.000.000 bytes, aparece no Windows como de "909 GB". Isso é esperado. Ao criar volumes usando **New-Volume**, você deve especificar o parâmetro **Tamanho** em números binários (base-2). Por exemplo, especificar "909GB" ou "0,909495 TB" criará um volume de aproximadamente 1.000.000.000.000 bytes.

### <a name="example-with-2-or-3-servers"></a>Exemplo: Com os servidores de 2 ou 3

Para facilitar as coisas, se sua implantação tem apenas dois servidores, Espaços de Armazenamento Diretos usará automaticamente o espelhamento de duas vias para resiliência. Se sua implantação tiver apenas três servidores, o espelhamento de três vias será usado automaticamente.

```
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>Exemplo: Com 4 + servidores

Se tiver quatro ou mais servidores, você pode usar o parâmetro opcional **ResiliencySettingName** para escolher o tipo de resiliência.

-   **ResiliencySettingName:** Qualquer um dos **espelho** ou **paridade**.

No exemplo a seguir, o *"Volume2"* usa espelhamento de três vias e o *"Volume3"* usa paridade dupla (geralmente chamada de "codificação de eliminação").

```
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>Exemplo: Usando as camadas de armazenamento

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

### <a name="step-3-add-to-cluster-shared-volumes"></a>Etapa 3: Adicionar aos volumes compartilhados do cluster

![Adicionar a Volumes Compartilhados Clusterizados](media/creating-volumes/GUI-Step-2.png)

12. No Gerenciador de Cluster de Failover, navegue até **Armazenamento** -> **Discos**.
13. Selecione o disco virtual que você acabou de criar e selecione **Adicionar a Volumes Compartilhados do Cluster** no painel Ações à direita, ou clique com o botão direito do mouse no disco virtual e selecione **Adicionar a Volumes Compartilhados do Cluster**.

Concluído! Repita conforme necessário para criar mais de um volume.

## <a name="see-also"></a>Consulte também

- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
- [Planejando volumes em espaços de armazenamento diretos](plan-volumes.md)
- [Extensão dos volumes em espaços de armazenamento diretos](resize-volumes.md)
- [Excluindo volumes em espaços de armazenamento diretos](delete-volumes.md)
