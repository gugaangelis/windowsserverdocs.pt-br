---
title: Implantar espaços de armazenamento em um servidor autônomo
description: Descreve como implantar espaços de armazenamento em um servidor autônomo baseado no Windows Server 2012.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 40265e767ac9aca05386c0893def259aca3a5633
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992529"
---
# Implantar espaços de armazenamento em um servidor autônomo

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como implantar espaços de armazenamento em um servidor autônomo. Para obter informações sobre como criar um espaço de armazenamento de cluster, consulte [implantar um cluster de espaços de armazenamento no Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Para criar um espaço de armazenamento, você deve primeiro criar um ou mais pools de armazenamento. Um pool de armazenamento é uma coleção de discos físicos. Um pool de armazenamento permite a agregação de armazenamento, expansão de capacidade elástica e administração delegada.

De um pool de armazenamento, você pode criar um ou mais discos virtuais. Esses discos virtuais também são chamados de *espaços de armazenamento*. Um espaço de armazenamento é exibido para o sistema operacional Windows como um disco normal do qual você pode criar volumes formatados. Quando você cria um disco virtual por meio da interface do usuário de serviços de arquivo e armazenamento, você pode configurar o tipo de resiliência (simples, espelhamento ou paridade), o tipo de provisionamento (fino ou fixo) e o tamanho. Por meio do Windows PowerShell, você pode definir parâmetros adicionais, como o número de colunas, o valor interleave e discos quais físicos no pool de usar. Para obter informações sobre esses parâmetros adicionais, consulte [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) e [quais são as colunas e como espaços de armazenamento decidir quantos usar?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) no armazenamento espaços com frequência perguntas Frequentes.

>[!NOTE]
>Você não pode usar um espaço de armazenamento para hospedar o sistema operacional Windows.

De um disco virtual, você pode criar um ou mais volumes. Quando você cria um volume, você pode configurar o tamanho, letra da unidade ou pasta, sistema de arquivos (sistema de arquivos NTFS ou sistema de arquivos resiliente (ReFS)), tamanho da unidade de alocação e um rótulo de volume opcional.

A figura a seguir ilustra o fluxo de trabalho de espaços de armazenamento.

![Fluxo de trabalho de espaços de armazenamento](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figura 1: Fluxo de trabalho de espaços de armazenamento**

>[!NOTE]
>Este tópico inclui cmdlets do Windows PowerShell de exemplo que você pode usar para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte o [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## Pré-requisitos

Para usar espaços de armazenamento em um servidor autônomo de 2012−based do Windows Server, certifique-se de que os discos físicos que você deseja usar atende aos seguintes pré-requisitos.

> [!IMPORTANT]
> Se você quiser saber como implantar espaços de armazenamento em um cluster de failover, consulte [implantar um cluster de espaços de armazenamento no Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Uma implantação de cluster de failover tem pré-requisitos diferentes, como o número mínimo necessário de discos, tipos de resiliência com suporte e tipos de barramento do disco com suporte.

|Área|Requisito|Observações|
|---|---|---|
|Tipos de barramento de disco|-Série anexada SCSI (SAS)<br>-Série avançada tecnologia anexo (SATA)<br>-iSCSI e Fibre Channel controladores. |Você também pode usar unidades USB. No entanto, não é ideal para usar unidades USB em um ambiente de servidor.<br>Espaços de armazenamento é compatível com iSCSI e FC (Fibre Channel) controladores, desde que os discos virtuais criados sobre eles são não resiliente (simples com qualquer número de colunas).<br>|
|Configuração de disco|-Discos físicos devem ter pelo menos 4 GB<br>-Discos devem estar em branco e não formatado. Não crie volumes.||
|Considerações de HBA|-Simples adaptadores de barramento (HBAs) que não dão suporte a funcionalidade RAID são recomendados<br>-Se compatíveis com RAID, HBAs devem estar no modo de não-RAID com todas as funcionalidades de RAID desabilitada<br>-Adaptadores não devem abstrair os discos físicos, dados de cache, ou oculte os dispositivos conectados. Isso inclui serviços compartimento que são fornecidos por dispositivos de (JBOD) just-um-monte-de discos conectados. |Espaços de armazenamento é compatível somente com HBAs onde você pode desativar completamente todas as funcionalidades de RAID.|
|Compartimentos JBOD|-Compartimentos JBOD são opcionais<br>-Recomendado usar espaços de armazenamento certificados compartimentos listados no catálogo do Windows Server<br>-Se você estiver usando um compartimento JBOD, verifique com seu fornecedor de armazenamento o compartimento dá suporte a espaços de armazenamento para garantir a funcionalidade completa<br>-Para determinar se o compartimento JBOD dá suporte a identificação do compartimento e slot, execute o seguinte cmdlet do Windows PowerShell:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Se os campos **EnclosureNumber** e **SlotNumber** contiverem valores, o compartimento dá suporte a esses recursos.||

Para planejar o número de discos físicos e o tipo de resiliência desejado para uma implantação de servidor autônomo, use as diretrizes a seguir.

|Tipo de resiliência|Requisitos de disco|Quando usar|
|---|---|---|
|**Simples**<br><br>-Distribui os dados em todos os discos físicos<br>-Maximiza a capacidade do disco e aumenta a taxa de transferência<br>-Nenhuma resiliência (não protege contra falha de disco)<br><br><br><br><br><br><br>|Requer pelo menos um disco físico.|Não use para hospedar dados insubstituível. Espaços simples não protege contra falhas de disco.<br><br>Use para hospedar dados temporários ou facilmente recriado a um custo reduzido.<br><br>Adequado para cargas de trabalho de alto desempenho em que a resiliência não é necessário ou já é fornecida pelo aplicativo.|
|**Mirror**<br><br>-Armazena duas ou três cópias dos dados em um conjunto de discos físicos<br>-Aumenta a confiabilidade, mas reduz a capacidade. Eliminação de duplicação ocorre com cada gravação. Um espaço de espelhamento também distribui os dados em várias unidades físicas.<br>-Taxa de transferência de dados maior e menor latência de acesso de paridade<br>-Usa anormal região (DRT) para controlar modificações para os discos no pool de rastreamento. Quando o sistema retoma de um desligamento não planejado e os espaços são colocados on-line, DRT torna discos no pool de consistente entre si.|Requer pelo menos dois discos físicos para proteger contra falhas de disco único.<br><br>Requer pelo menos cinco discos físicos para proteger contra falhas de dois discos simultâneas.|Use para a maioria das implantações. Por exemplo, os espaços de espelho são adequados para um compartilhamento de arquivos de uso geral ou uma biblioteca de disco rígido virtual (VHD).|
|**Paridade**<br><br>-Informações de dados e a paridade faixas em todos os discos físicos<br>-Aumenta a confiabilidade quando ele é comparada com um espaço simple, mas um pouco reduz a capacidade<br>-Aumenta a resiliência por meio do registro em log. Isso ajuda a evitar corrupção de dados se ocorrer um desligamento não planejado.|Requer pelo menos três discos físicos para proteger contra falhas de disco único.|Use para cargas de trabalho que são altamente sequenciais, como arquivamento ou backup.|

## Etapa 1: Criar um pool de armazenamento

Você deve primeiros discos físicos de disponível de grupo em um ou mais pools de armazenamento.

1. No painel de navegação do Gerenciador do servidor, selecione o **arquivo e serviços de armazenamento**.

2. No painel de navegação, selecione a página de **Pools de armazenamento** .
    
    Por padrão, discos disponíveis estão incluídos em um pool chamado o pool *primordial* . Se nenhum pool primordial está listado em **POOLS de armazenamento**, isso indica que o armazenamento não atende aos requisitos de espaços de armazenamento. Certifique-se de que os discos de atender aos requisitos descritos na seção de pré-requisitos.
    
    >[!TIP]
    >Se você selecionar o pool de armazenamento **Primordial** , os discos físicos disponíveis são listados em **Discos físicos**.

3. Em **POOLS de armazenamento**, selecione a lista de **tarefas** e, em seguida, selecione o **Novo Pool de armazenamento**. O novo Assistente de Pool de armazenamento será aberta.

4. Na página **antes de começar** , selecione **Avançar**.

5. Na página **Especifique um nome de pool de armazenamento e subsistema** , insira um nome e uma descrição opcional para o pool de armazenamento, selecione o grupo de discos físicos disponíveis que você deseja usar e, em seguida, selecione **Avançar**.

6. Na página **Selecionar discos físicos no pool de armazenamento** , faça o seguinte e, em seguida, selecione **em seguida**:
    
    1. Marque a caixa de seleção ao lado de cada disco físico que você deseja incluir no pool de armazenamento.
    
    2. Se você quiser designar um ou mais discos como sobressalentes em **alocação**, selecione a seta suspensa, selecione **Hot reposição**.

7. Na página **Confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

8. Na página de **resultados de modo de exibição** , verifique se que todas as tarefas concluídas e, em seguida, selecione **Fechar**.
    
    >[!NOTE]
    >Opcionalmente, para continuar diretamente para a próxima etapa, você pode selecionar a caixa de seleção de **criar um disco virtual quando este assistente for fechado** .

9. Em **POOLS de armazenamento**, verifique se o novo pool de armazenamento é listado.

### Comandos equivalentes do Windows PowerShell para criar pools de armazenamento

O seguinte cmdlet do Windows PowerShell ou cmdlets executar a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles podem aparecer quebra de várias linhas aqui devido a restrições de formatação.

O exemplo a seguir mostra quais discos físicos estão disponíveis no pool de primordial.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

O exemplo a seguir cria um novo pool de armazenamento denominado *StoragePool1* que usa todos os discos disponíveis.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

O exemplo a seguir cria um novo pool de armazenamento, *StoragePool1*, que usa quatro dos discos disponíveis.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

A seguinte sequência de exemplo de cmdlets mostra como adicionar um disco físico disponível *PhysicalDisk5* como reposição hot ao pool de armazenamento *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## Etapa 2: Criar um disco virtual

Em seguida, você deve criar um ou mais discos virtuais do pool de armazenamento. Quando você cria um disco virtual, você pode selecionar como os dados estão dispostos em todos os discos físicos. Isso afeta o desempenho e confiabilidade. Você também pode selecionar se deseja criar discos ou fixo-provisionamento dinâmico.

1. Se o Assistente de novo disco Virtual já não estiver aberto, na página **Pools de armazenamento** no Gerenciador do servidor, em **POOLS de armazenamento**, certifique-se de que o pool de armazenamento desejado está selecionado.

2. Em **Discos virtuais**, selecione a lista de **tarefas** e, em seguida, selecione o **Novo disco Virtual**. O Assistente de novo disco Virtual será aberto.

3. Na página **antes de começar** , selecione **Avançar**.

4. Na página **Selecionar pool de armazenamento** , selecione o pool de armazenamento desejada e selecione **Avançar**.

5. Na página **Especifique o nome do disco virtual** , insira um nome e uma descrição opcional e selecione **Avançar**.

6. Na página **Selecionar o layout de armazenamento** , selecione o layout desejado e selecione **Avançar**.
    
    >[!NOTE]
    >Se você selecionar um layout de onde você não tem suficiente discos físicos, você receberá uma mensagem de erro quando você selecionar **Avançar**. Para obter informações sobre qual layout uso e os requisitos de disco, consulte [os pré-requisitos](#prerequisites)).

7. Se você selecionou o **espelho** como o layout de armazenamento e você tiver cinco ou mais discos no pool, aparecerá a página **Configurar as configurações de resiliência** . Selecione uma das seguintes opções:
    
      - **Espelhamento de duas vias**
      - **Espelhamento de três vias**

8. Na página **especificar o tipo de provisionamento** , selecione uma das opções a seguir, e **Avançar**.
    
      - **Fina**
        
        Com o provisionamento dinâmico, o espaço é alocado em uma base conforme o necessário. Isso otimiza o uso do armazenamento disponível. No entanto, pois isso permite que você alocar excesso de armazenamento, você deve monitorar cuidadosamente quanto espaço em disco.
    
      - **Corrigido**
        
        Com o provisionamento fixo, a capacidade de armazenamento é alocada imediatamente, no momento em que um disco virtual é criado. Portanto, fixo provisionamento usa espaço de pool de armazenamento é igual ao tamanho do disco virtual.
    
    >[!TIP]
    >Com espaços de armazenamento, você pode criar dois discos virtuais e fixo-provisionamento dinâmico no mesmo pool de armazenamento. Por exemplo, você poderia usar um disco virtual provisionamento dinâmico para hospedar um banco de dados e um disco virtual fixo provisionado para hospedar os arquivos de log associado.

9. Na página **Especifique o tamanho do disco virtual** , faça o seguinte:
    
    Se você selecionou o provisionamento dinâmico na etapa anterior, na caixa de **tamanho de disco Virtual** , insira um tamanho de disco virtual, selecione as unidades (**MB**, **GB**ou **TB**) e selecione **Avançar**.
    
    Se você selecionou o provisionamento fixo na etapa anterior, selecione um dos seguintes:
    
      - **Especifique o tamanho**
        
        Para especificar um tamanho, insira um valor na caixa de **tamanho de disco Virtual** , selecione as unidades (**MB**, **GB**ou **TB**).
        
        Se você usar um layout de armazenamento que não seja simples, o disco virtual usa mais espaço livre que o tamanho que você especificar. Para evitar um erro em potencial, onde o tamanho do volume excede o espaço livre do pool de armazenamento, você pode marcar a caixa de seleção **criar o disco virtual maior possível, até o tamanho especificado** .
    
      - **Tamanho máximo**
        
        Selecione esta opção para criar um disco virtual que usa a capacidade máxima do pool de armazenamento.

10. Na página **Confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

11. Na página de **resultados de modo de exibição** , verifique se que todas as tarefas concluídas e, em seguida, selecione **Fechar**.
    
    >[!TIP]
    >Por padrão, a caixa de seleção de **criar um volume quando este assistente se fechar** é selecionada. Isso levará você diretamente para a próxima etapa.

### Comandos equivalentes do Windows PowerShell para a criação de discos virtuais

O seguinte cmdlet do Windows PowerShell ou cmdlets executar a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que eles podem aparecer quebra de várias linhas aqui devido a restrições de formatação.

O exemplo a seguir cria um disco virtual de 50 GB denominado *VirtualDisk1* em um pool de armazenamento chamado *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

O exemplo a seguir cria um disco virtual espelhado denominado *VirtualDisk1* em um pool de armazenamento chamado *StoragePool1*. O disco usa a capacidade de armazenamento máximo do pool de armazenamento.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

O exemplo a seguir cria um disco virtual de 50 GB denominado *VirtualDisk1* em um pool de armazenamento é denominado *StoragePool1*. O disco usa o tipo de provisionamento dinâmico.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

O exemplo a seguir cria um disco virtual chamado *VirtualDisk1* em um pool de armazenamento chamado *StoragePool1*. O disco virtual usa três vias e é um tamanho fixo de 20 GB.

>[!NOTE]
>Você deve ter pelo menos cinco discos físicos no pool de armazenamento para esse cmdlet trabalhar. (Isso não inclui todos os discos que são alocados como sobressalentes).

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## Etapa 3: Criar um volume

Em seguida, você deve criar um volume de disco virtual. Você pode atribuir uma letra de unidade opcional ou uma pasta e formatar o volume com um sistema de arquivos.

1. Se o Assistente de novo Volume já não estiver aberto, na página **Pools de armazenamento** no Gerenciador do servidor, em **Discos virtuais**, clique com botão direito no disco virtual desejado e, em seguida, selecione o **Novo Volume**.
    
    Abre o Assistente de novo Volume.

2. Na página **antes de começar** , selecione **Avançar**.

3. Na página **Selecionar o servidor e o disco** , faça o seguinte e, em seguida, selecione **em seguida**.
    
    1. Na área do **servidor** , selecione o servidor no qual você deseja provisionar o volume.
    
    2. Na área do **disco** , selecione o disco virtual no qual você deseja criar o volume.

4. Na página **Especifique o tamanho do volume** , insira um tamanho de volume, especifique as unidades (**MB**, **GB**ou **TB**) e selecione **Avançar**.

5. Na página **atribuir uma letra de unidade ou pasta** , configurar a opção desejada e selecione **Avançar**.

6. Na página **Selecionar configurações do sistema de arquivo** , faça o seguinte e, em seguida, selecione **em seguida**.
    
    1. Na lista de **sistema de arquivos** , selecione **NTFS** ou **ReFS**.
    
    2. Na lista de **tamanho de unidade de alocação** , deixe a configuração **padrão** ou definir o tamanho da unidade de alocação.
        
        >[!NOTE]
        >Para obter mais informações sobre o tamanho da unidade de alocação, consulte o [tamanho de cluster para NTFS, FAT e exFAT padrão](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Opcionalmente, na caixa de **rótulo de Volume** , insira um nome de rótulo de volume, por exemplo **HR dados**.

7. Na página **Confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

8. Na página de **resultados de modo de exibição** , verifique se que todas as tarefas concluídas e, em seguida, selecione **Fechar**.

9. Para verificar se o volume foi criado, no Gerenciador do servidor, selecione a página de **Volumes** . O volume está listado sob o servidor de onde ele foi criado. Você também pode verificar que o volume é no Windows Explorer.

### Comandos equivalentes do Windows PowerShell para a criação de volumes

O seguinte cmdlet do Windows PowerShell executa a mesma função que o procedimento anterior. Insira o comando em uma única linha.

O exemplo a seguir inicializa os discos para o disco virtual *VirtualDisk1*, cria uma partição com uma letra de unidade atribuída e, em seguida, formata o volume com o sistema de arquivos NTFS padrão.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## Informações adicionais

- [Espaços de Armazenamento](overview.md)
- [Cmdlets de armazenamento no Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Implantar espaços de armazenamento clusterizados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Espaços de armazenamento perguntas frequentes (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
