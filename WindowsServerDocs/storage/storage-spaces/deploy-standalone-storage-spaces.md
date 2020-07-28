---
title: Implantar espaços de armazenamento em um servidor autônomo
description: Descreve como implantar espaços de armazenamento em um servidor autônomo baseado no Windows Server 2012.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: b5f1ccab7e4c0ca2bbd478509a76a4a37559c345
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181862"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>Implantar espaços de armazenamento em um servidor autônomo

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como implantar espaços de armazenamento em um servidor autônomo. Para obter informações sobre como criar um espaço de armazenamento em cluster, consulte [implantar um cluster de espaços de armazenamento no Windows Server 2012 R2](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Para criar um espaço de armazenamento, você deve primeiramente criar um ou mais pools de armazenamento. Um pool de armazenamento é uma coleção de discos físicos. Um pool de armazenamento permite a agregação de armazenamento, expansão da capacidade elástica e administração delegada.

Em um pool de armazenamento, você pode criar um ou mais discos virtuais. Esses discos virtuais também são chamados de *espaços de armazenamento*. Um espaço de armazenamento aparece no sistema operacional do Windows como um disco regular, a partir do qual é possível criar volumes formatados. Ao criar um disco virtual por meio da interface de usuário de Serviços de Arquivo e Armazenamento, você pode configurar o tipo de resiliência (simples, espelho ou paridade), o tipo de provisionamento (dinâmico ou fixo) e o tamanho. Com o Windows PowerShell, você pode definir parâmetros adicionais, como o número de colunas, o valor de intercalação e quais discos físicos devem ser usados no pool. Para obter informações sobre esses parâmetros adicionais, consulte [New-VirtualDisk](/powershell/module/storage/new-virtualdisk?view=win10-ps) e o [Windows Server Storage forum](https://docs.microsoft.com/answers/topics/windows-server-storage.html).

>[!NOTE]
>Você não pode usar um espaço de armazenamento para hospedar o sistema operacional Windows.

Em um disco virtual, você pode criar um ou mais volumes. Ao criar um volume, você pode configurar o tamanho, a letra da unidade ou a pasta, sistema de arquivos (sistema de arquivos NTFS ou ReFS), o tamanho da unidade de alocação e um rótulo de volume opcional.

A figura a seguir ilustra o fluxo de trabalho dos Espaços de Armazenamento.

![Fluxo de trabalho de Espaços de Armazenamento](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figura 1: fluxo de trabalho de espaços de armazenamento**

>[!NOTE]
>Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [PowerShell](/powershell/scripting/powershell-scripting?view=powershell-6).

## <a name="prerequisites"></a>Pré-requisitos

Para usar espaços de armazenamento em um servidor autônomo baseado no Windows Server 2012 −, verifique se os discos físicos que você deseja usar atendem aos seguintes pré-requisitos.

> [!IMPORTANT]
> Se você quiser saber como implantar espaços de armazenamento em um cluster de failover, consulte [implantar um cluster de espaços de armazenamento no Windows Server 2012 R2](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Uma implantação de cluster de failover tem pré-requisitos diferentes, como tipos de barramento de disco com suporte, tipos de resiliência com suporte e o número mínimo necessário de discos.

|Área|Requisito|Observações|
|---|---|---|
|Tipos de barramento de disco|-SAS (Serial Attached SCSI)<br>-SATA (Serial Advanced Technology Attachment)<br>-Controladores de Fibre Channel e iSCSI. |Você também pode usar unidades USB. No entanto, não é ideal usar unidades USB em um ambiente de servidor.<br>Os espaços de armazenamento têm suporte em controladores iSCSI e Fibre Channel (FC), contanto que os discos virtuais criados sobre eles não sejam resilientes (simples com qualquer número de colunas).<br>|
|Configuração de disco|-Discos físicos devem ter pelo menos 4 GB<br>-Os discos devem estar em branco e não formatados. Não crie volumes.||
|Considerações de HBA|-Adaptadores de barramento de host simples (HBAs) que não dão suporte à funcionalidade de RAID são recomendados<br>-Se for compatível com RAID, os HBAs devem estar no modo não RAID com toda a funcionalidade RAID desabilitada<br>-Os adaptadores não devem abstrair os discos físicos, armazenar dados em cache ou ocultar todos os dispositivos anexados. Isso inclui serviços de compartimentos fornecidos pelos dispositivos JBOD conectados. |Os Espaços de Armazenamento são compatíveis somente com os HBAs nos quais é possível desabilitar totalmente a funcionalidade RAID.|
|Compartimentos JBOD|-Compartimentos JBOD são opcionais<br>-Recomendado para usar os compartimentos certificados com espaços de armazenamento listados no catálogo do Windows Server<br>-Se você estiver usando um compartimento JBOD, verifique com seu fornecedor de armazenamento que o compartimento dá suporte a espaços de armazenamento para garantir a funcionalidade completa<br>-Para determinar se o compartimento JBOD dá suporte à identificação de compartimento e de slot, execute o seguinte cmdlet do Windows PowerShell:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Se os campos **EnclosureNumber** e **SlotNumber** contiverem valores, o compartimento dará suporte a esses recursos.||

Para planejar o número de discos físicos e o tipo de resiliência desejado para uma implantação de servidor autônoma, use as orientações a seguir.

|Tipo de resiliência|Requisitos de disco|Quando usar|
|---|---|---|
|**Simples**<br><br>-Distribui dados em discos físicos<br>-Maximiza a capacidade do disco e aumenta a taxa de transferência<br>-Sem resiliência (não protege contra falha de disco)<br><br><br><br><br><br><br>|Requer ao menos um disco físico.|Não use para hospedar dados insubstituíveis. Espaços simples não protegem contra falhas de disco.<br><br>Use para hospedar dados temporários ou recriá-los com baixo custo.<br><br>Adequado para cargas de trabalho de alto desempenho em que a resiliência não é necessária ou já é fornecida pelo aplicativo.|
|**Espelho**<br><br>-Armazena duas ou três cópias dos dados no conjunto de discos físicos<br>-Aumenta a confiabilidade, mas reduz a capacidade. A duplicação ocorre a cada gravação. Um espaço espelhado também distribui os dados entre diversas unidades físicas.<br>-Maior taxa de transferência de dados e menor latência de acesso que a paridade<br>– Usa o controle de região sujo (DRT) para controlar as modificações nos discos no pool. Quando o sistema volta de um desligamento inesperado e os espaços ficam novamente online, o DRT torna os discos do pool consistentes entre si.|Ele requer ao menos dois discos para oferecer proteção contra falha de disco único.<br><br>Ele requer ao menos cinco discos para oferecer proteção contra duas falhas de disco simultâneas.|Use para a maioria das implantações. Por exemplo, espaços de espelho são indicados para o compartilhamento de arquivos geral ou para uma biblioteca de VHD (disco rígido virtual).|
|**Parity**<br><br>-Distribui dados e informações de paridade em discos físicos<br>-Aumenta a confiabilidade quando comparada a um espaço simples, mas, de certa forma, reduz a capacidade<br>-Aumenta a resiliência por meio do registro em diário. Isso ajuda a prevenir dados corrompidos em caso de desligamento inesperado.|Ele requer ao menos três discos para oferecer proteção contra falha de disco único.|Use para cargas de trabalho que são altamente sequenciais, como arquivos ou backups.|

## <a name="step-1-create-a-storage-pool"></a>Etapa 1: criar um pool de armazenamento

Primeiramente, você deve agrupar os discos físicos disponíveis em um ou mais pools de armazenamento.

1. No painel de navegação Gerenciador do Servidor, selecione **serviços de arquivo e armazenamento**.

2. No painel de navegação, selecione a página **pools de armazenamento** .

    Por padrão, os discos disponíveis são incluídos em um pool chamado pool *primordial*. Se nenhum pool primordial estiver listado nos **POOLS DE ARMAZENAMENTO**, isso indica que o armazenamento não cumpre os requisitos dos Espaços de Armazenamento. Verifique se os discos estão de acordo com os requisitos descritos na seção Pré-requisitos.

    >[!TIP]
    >Se você selecionar o pool de armazenamento **Primordial**, os discos físicos disponíveis estarão listados em **DISCOS FÍSICOS**.

3. Em **pools de armazenamento**, selecione a lista **tarefas** e, em seguida, selecione **novo pool de armazenamento**. O assistente de novo pool de armazenamento será aberto.

4. Na página **antes de começar** , selecione **Avançar**.

5. Na página **especificar o nome e o subsistema do pool de armazenamento** , insira um nome e uma descrição opcional para o pool de armazenamento, selecione o grupo de discos físicos disponíveis que você deseja usar e, em seguida, selecione **Avançar**.

6. Na página **Selecionar discos físicos para o pool de armazenamento** , faça o seguinte e, em seguida, selecione **Avançar**:

    1. Marque a caixa de seleção ao lado de cada disco físico que você deseja incluir no pool de armazenamento.

    2. Se você quiser designar um ou mais discos como hot spares, em **alocação**, selecione a seta suspensa e, em seguida, selecione **hot spare**.

7. Na página **confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

8. Na página **exibir resultados** , verifique se todas as tarefas foram concluídas e, em seguida, selecione **fechar**.

    >[!NOTE]
    >Como opção, para continuar diretamente na próxima etapa, você pode marcar a caixa de seleção **Criar um disco virtual quando esse assistente fecha**.

9. Em **POOLS DE ARMAZENAMENTO**, verifique se o novo pool de armazenamento está listado.

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>Comandos equivalentes do Windows PowerShell para criar pools de armazenamento

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

O exemplo a seguir mostra quais discos físicos estão disponíveis no pool primordial.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

O exemplo a seguir cria um novo pool de armazenamento chamado *StoragePool1* que usa todos os discos disponíveis.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

O exemplo a seguir cria um novo pool de armazenamento, *StoragePool1*, que usa quatro dos discos disponíveis.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

A sequência de cmdlets de exemplo a seguir mostra como adicionar um disco físico disponível *PhysicalDisk5* como espera ativa do pool de armazenamento *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## <a name="step-2-create-a-virtual-disk"></a>Etapa 2: criar um disco virtual

Em seguida, você deverá criar um ou mais discos virtuais no pool de armazenamento. Ao criar um disco virtual, você pode selecionar como os dados são distribuídos entre os discos físicos. Isso afeta a confiabilidade e o desempenho. Você também pode selecionar se deseja criar discos provisionados dinâmicos ou fixos.

1. Se o Assistente de Novo Disco Virtual não estiver aberto, na página **Pools de Armazenamento** no Gerenciador do Servidor, em **POOLS DE ARMAZENAMENTO**, verifique se o pool de armazenamento desejado foi selecionado.

2. Em **discos virtuais**, selecione a lista **tarefas** e, em seguida, selecione **novo disco virtual**. O assistente de novo disco virtual será aberto.

3. Na página **antes de começar** , selecione **Avançar**.

4. Na página **selecionar o pool de armazenamento** , selecione o pool de armazenamento desejado e, em seguida, selecione **Avançar**.

5. Na página **especificar o nome do disco virtual** , insira um nome e uma descrição opcional e, em seguida, selecione **Avançar**.

6. Na página **selecionar o layout de armazenamento** , selecione o layout desejado e, em seguida, selecione **Avançar**.

    >[!NOTE]
    >Se você selecionar um layout onde você não tem discos físicos suficientes, receberá uma mensagem de erro quando selecionar **Avançar**. Para obter informações sobre qual layout usar e os requisitos de disco, consulte [Prerequisites](#prerequisites)).

7. Se você selecionou **espelhamento** como o layout de armazenamento e tiver cinco ou mais discos no pool, a página **configurar as configurações de resiliência** será exibida. Selecione uma das seguintes opções:

      - **Espelho de duas vias**
      - **Espelho de três vias**

8. Na página **especificar o tipo de provisionamento** , selecione uma das opções a seguir e, em seguida, selecione **Avançar**.

   - **Dinâmico**

     Com o provisionamento dinâmico, o espaço é alocado conforme a necessidade. Isso otimiza o uso do armazenamento disponível. Contudo, como isso permite a alocação excessiva do armazenamento, é necessário monitorar atentamente quanto espaço em disco está disponível.

   - **Fixo**

     Com o provisionamento fixo, a capacidade de armazenamento é alocada imediatamente, no momento da criação do disco virtual. Portanto o provisionamento fixo usa espaço do pool de armazenamento igual ao tamanho do disco virtual.

     >[!TIP]
     >Com os Espaços de Armazenamento, você pode criar discos virtuais com ambos os provisionamentos dinâmico e fixo no mesmo pool de armazenamento. Por exemplo, você poderia usar um disco virtual com provisionamento dinâmico para hospedar um banco de dados e outro com provisionamento fixo para hospedar os arquivos de log associados.

9. Na página **Especificar o tamanho do disco virtual**, execute o seguinte procedimento:

    Se você selecionou provisionamento dinâmico na etapa anterior, na caixa Tamanho do **disco virtual** , insira um tamanho de disco virtual, selecione as unidades (**MB**, **GB**ou **TB**) e, em seguida, selecione **Avançar**.

    Se você tiver selecionado provisionamento fixo na etapa anterior, selecione uma das seguintes opções:

      - **Especificar tamanho**

        Para especificar um tamanho, insira um valor na caixa **tamanho do disco virtual** e, em seguida, selecione as unidades (**MB**, **GB**ou **TB**).

        Se você usar outro layout de armazenamento que não o simples, o disco virtual usará mais espaço livre do que o tamanho especificado. Para evitar um potencial erro no qual o tamanho do volume excede o espaço livre do pool de armazenamento, marque a caixa de seleção **Criar o maior disco virtual possível, até o tamanho especificado**.

      - **Tamanho máximo**

        Selecione esta opção para criar um disco virtual que usa a capacidade máxima do pool de armazenamento.

10. Na página **confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

11. Na página **exibir resultados** , verifique se todas as tarefas foram concluídas e, em seguida, selecione **fechar**.

    >[!TIP]
    >A caixa de seleção **Criar um volume ao fechar o assistente** é marcada por padrão. Isso levará você até a próxima etapa.

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Comandos equivalentes do Windows PowerShell para criar discos virtuais

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

O exemplo a seguir cria um disco virtual de 50 GB chamado *VirtualDisk1* em um pool de armazenamento chamado *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

O exemplo a seguir cria um disco virtual espelhado chamado *VirtualDisk1* em um pool de armazenamento chamado *StoragePool1*. O disco usa a capacidade máxima de armazenamento do pool de armazenamento.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

O exemplo a seguir cria um disco virtual de 50 GB chamado *VirtualDisk1* em um pool de armazenamento chamado *StoragePool1*. O disco usa o tipo de provisionamento dinâmico.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

O exemplo a seguir cria um disco virtual chamado *VirtualDisk1* em um pool de armazenamento chamado *StoragePool1*. O disco virtual usa espelhamento de três vias e possui tamanho fixo de 20 GB.

>[!NOTE]
>Você deve ter pelo menos cinco discos físicos no pool de armazenamento para que este cmdlet funcione. (Isso não inclui discos alocados como espera ativa.)

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>Etapa 3: criar um volume

Em seguida, você deve criar um volume para o disco virtual. Você pode atribuir uma letra de unidade ou pasta opcional e formatar o volume com um sistema de arquivos.

1. Se o assistente de novo volume ainda não estiver aberto, na página **pools de armazenamento** em Gerenciador do servidor, em **discos virtuais**, clique com o botão direito do mouse no disco virtual desejado e selecione **novo volume**.

    O Assistente de Novo Volume é aberto.

2. Na página **antes de começar** , selecione **Avançar**.

3. Na página **selecionar o servidor e o disco** , faça o seguinte e, em seguida, selecione **Avançar**.

    1. Na área **servidor** , selecione o servidor no qual você deseja provisionar o volume.

    2. Na área **disco** , selecione o disco virtual no qual você deseja criar o volume.

4. Na página **especificar o tamanho do volume** , insira um tamanho de volume, especifique as unidades (**MB**, **GB**ou **TB**) e, em seguida, selecione **Avançar**.

5. Na página **atribuir a uma letra da unidade ou pasta** , configure a opção desejada e, em seguida, selecione **Avançar**.

6. Na página **selecionar configurações do sistema de arquivos** , faça o seguinte e, em seguida, selecione **Avançar**.

    1. Na lista **sistema de arquivos** , selecione **NTFS** ou **ReFS**.

    2. Na lista **Tamanho da unidade de alocação**, deixe a configuração **Padrão** ou defina o tamanho da unidade de alocação.

        >[!NOTE]
        >Para obter mais informações sobre o tamanho da unidade de alocação, consulte [Tamanho de cluster padrão para NTFS, FAT e exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).


    3. Opcionalmente, na caixa **Rótulo do volume**, digite um nome de rótulo para o volume, por exemplo, **Dados de RH**.

7. Na página **confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

8. Na página **exibir resultados** , verifique se todas as tarefas foram concluídas e, em seguida, selecione **fechar**.

9. Para verificar se o volume foi criado, em Gerenciador do Servidor, selecione a página **volumes** . O volume estará listado no servidor onde foi criado. Você também pode verificar se o volume está no Windows Explorer.

### <a name="windows-powershell-equivalent-commands-for-creating-volumes"></a>Comandos equivalentes do Windows PowerShell para criar volumes

O cmdlet do Windows PowerShell a seguir executa a mesma função do procedimento anterior. Digite o comando como uma linha única.

O exemplo a seguir inicializa os discos para o disco virtual *VirtualDisk1*, cria uma partição com uma letra da unidade atribuída e formata o volume com o sistema de arquivos NTFS.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## <a name="additional-information"></a>Informações adicionais

- [Espaços de armazenamento](overview.md)
- [Cmdlets de armazenamento no Windows PowerShell](/powershell/module/storage/index?view=win10-ps)
- [Implantar Espaços de Armazenamento clusterizados](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Fórum de armazenamento do Windows Server](https://docs.microsoft.com/answers/topics/windows-server-storage.html)
