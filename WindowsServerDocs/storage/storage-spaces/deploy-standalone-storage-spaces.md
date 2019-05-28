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
ms.openlocfilehash: 7090657a0936aed0f4b2e79007f69d7b082b0b8f
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63750655"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>Implantar espaços de armazenamento em um servidor autônomo

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como implantar espaços de armazenamento em um servidor autônomo. Para obter informações sobre como criar um espaço de armazenamento em cluster, consulte [implantar um cluster de espaços de armazenamento no Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Para criar um espaço de armazenamento, você deve primeiramente criar um ou mais pools de armazenamento. Um pool de armazenamento é uma coleção de discos físicos. Um pool de armazenamento permite a agregação de armazenamento, expansão da capacidade elástica e administração delegada.

Em um pool de armazenamento, você pode criar um ou mais discos virtuais. Esses discos virtuais também são chamados de *espaços de armazenamento*. Um espaço de armazenamento aparece no sistema operacional do Windows como um disco regular, a partir do qual é possível criar volumes formatados. Ao criar um disco virtual por meio da interface de usuário de Serviços de Arquivo e Armazenamento, você pode configurar o tipo de resiliência (simples, espelho ou paridade), o tipo de provisionamento (dinâmico ou fixo) e o tamanho. Com o Windows PowerShell, você pode definir parâmetros adicionais, como o número de colunas, o valor de intercalação e quais discos físicos devem ser usados no pool. Para obter informações sobre esses parâmetros adicionais, consulte [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) e [o que são colunas e como os espaços de armazenamento decidem quantas usar?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) no armazenamento de espaços de perguntas frequentes (FAQ).

>[!NOTE]
>Você não pode usar um espaço de armazenamento para hospedar o sistema operacional Windows.

Em um disco virtual, você pode criar um ou mais volumes. Quando você cria um volume, você pode configurar o tamanho, letra da unidade ou pasta, o sistema de arquivos (sistema de arquivos NTFS ou sistema de arquivos resiliente (ReFS)), tamanho da unidade de alocação e um rótulo de volume opcional.

A figura a seguir ilustra o fluxo de trabalho dos Espaços de Armazenamento.

![Fluxo de trabalho de Espaços de Armazenamento](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figura 1: Fluxo de trabalho de espaços de armazenamento**

>[!NOTE]
>Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## <a name="prerequisites"></a>Pré-requisitos

Para usar espaços de armazenamento em um servidor autônomo de 2012−based do Windows Server, certifique-se de que os discos físicos que você deseja usar atendem aos pré-requisitos a seguir.

> [!IMPORTANT]
> Se você quiser saber como implantar espaços de armazenamento em um cluster de failover, consulte [implantar um cluster de espaços de armazenamento no Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Uma implantação de cluster de failover tem pré-requisitos diferentes, como o número mínimo necessário de discos, tipos de resiliência com suporte e tipos de barramento de disco com suporte.

|Área|Requisito|Observações|
|---|---|---|
|Tipos de barramento de disco|-Serial Attached SCSI (SAS)<br>-Serial Advanced Technology Attachment (SATA)<br>-iSCSI e controladores Fibre Channel. |Você também pode usar unidades USB. No entanto, não é ideal para usar unidades USB em um ambiente de servidor.<br>Espaços de armazenamento tem suporte no iSCSI e controladores Fibre Channel (FC), desde que os discos virtuais criados sobre eles estão sem proteção (simples com qualquer número de colunas).<br>|
|Configuração de disco|-Discos físicos devem ser pelo menos 4 GB<br>-Discos devem estar em branco e não formatados. Não crie volumes.||
|Considerações de HBA|-Adaptadores de barramento de host simples (HBAs) que não dão suporte a funcionalidade RAID são recomendados<br>-Se compatível com RAID, HBAs devem ser no modo não RAID com todas as funcionalidades RAID desabilitadas<br>-Adaptadores não devem abstrair os discos físicos, armazenar dados em cache ou ocultar dispositivos conectados. Isso inclui serviços de compartimentos fornecidos pelos dispositivos JBOD conectados. |Os Espaços de Armazenamento são compatíveis somente com os HBAs nos quais é possível desabilitar totalmente a funcionalidade RAID.|
|Compartimentos JBOD|-Compartimentos JBOD são opcionais<br>-Recomendado usar espaços de armazenamento certified compartimentos listados no catálogo do Windows Server<br>-Se você estiver usando um compartimento JBOD, verifique com seu fornecedor de armazenamento que o compartimento dá suporte a espaços de armazenamento para garantir a funcionalidade completa<br>-Para determinar se o compartimento JBOD dá suporte a compartimento e identificação de slot, execute o seguinte cmdlet do Windows PowerShell:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Se o **EnclosureNumber** e **SlotNumber** campos contêm valores e, em seguida, o compartimento dá suporte a esses recursos.||

Para planejar o número de discos físicos e o tipo de resiliência desejado para uma implantação de servidor autônoma, use as orientações a seguir.

|Tipo de resiliência|Requisitos de disco|Quando usar|
|---|---|---|
|**Simple**<br><br>-Distribui os dados entre os discos físicos<br>-Maximiza a capacidade do disco e aumenta a taxa de transferência<br>-Sem resiliência (não protege contra falha de disco)<br><br><br><br><br><br><br>|Requer ao menos um disco físico.|Não use para hospedar dados insubstituíveis. Espaços simples não protegem contra falhas de disco.<br><br>Use para hospedar dados temporários ou recriá-los com baixo custo.<br><br>Adequada para cargas de trabalho de alto desempenho em que a resiliência não é necessária ou já é fornecida pelo aplicativo.|
|**Espelho**<br><br>-Armazena duas ou três cópias dos dados em um conjunto de discos físicos<br>-Aumenta a confiabilidade, mas reduz a capacidade. A duplicação ocorre a cada gravação. Um espaço espelhado também distribui os dados entre diversas unidades físicas.<br>-Taxa de transferência de dados maior e menor latência de acesso que paridade<br>-Usa sujos região DRT (rastreamento) para rastrear as modificações feitas para os discos no pool. Quando o sistema volta de um desligamento inesperado e os espaços ficam novamente online, o DRT torna os discos do pool consistentes entre si.|Ele requer ao menos dois discos para oferecer proteção contra falha de disco único.<br><br>Ele requer ao menos cinco discos para oferecer proteção contra duas falhas de disco simultâneas.|Use para a maioria das implantações. Por exemplo, espaços de espelho são indicados para o compartilhamento de arquivos geral ou para uma biblioteca de VHD (disco rígido virtual).|
|**Paridade**<br><br>-Distribui os dados e informações de paridade entre os discos físicos<br>-Aumenta a confiabilidade quando ele é comparado com um espaço simples, mas reduz a capacidade<br>-Aumenta a resiliência por meio do registro no diário. Isso ajuda a prevenir dados corrompidos em caso de desligamento inesperado.|Ele requer ao menos três discos para oferecer proteção contra falha de disco único.|Use para cargas de trabalho que são altamente sequenciais, como arquivos ou backups.|

## <a name="step-1-create-a-storage-pool"></a>Etapa 1: Criar um pool de armazenamento

Primeiramente, você deve agrupar os discos físicos disponíveis em um ou mais pools de armazenamento.

1. No painel de navegação do Gerenciador do servidor, selecione **serviços de arquivo e armazenamento**.

2. No painel de navegação, selecione o **Pools de armazenamento** página.
    
    Por padrão, os discos disponíveis são incluídos em um pool chamado pool *primordial*. Se nenhum pool primordial estiver listado nos **POOLS DE ARMAZENAMENTO**, isso indica que o armazenamento não cumpre os requisitos dos Espaços de Armazenamento. Verifique se os discos estão de acordo com os requisitos descritos na seção Pré-requisitos.
    
    >[!TIP]
    >Se você selecionar o pool de armazenamento **Primordial**, os discos físicos disponíveis estarão listados em **DISCOS FÍSICOS**.

3. Sob **POOLS de armazenamento**, selecione o **tarefas** lista e, em seguida, selecione **novo Pool de armazenamento**. O Assistente de novo Pool de armazenamento será aberta.

4. Sobre o **antes de começar** página, selecione **próxima**.

5. No **especificar um nome de pool de armazenamento e o subsistema** , insira um nome e uma descrição opcional para o pool de armazenamento, selecione o grupo de discos físicos disponíveis que você deseja usar e, em seguida, selecione **próxima**.

6. Sobre o **selecionar discos físicos no pool de armazenamento** página, faça o seguinte e, em seguida, selecione **próxima**:
    
    1. Marque a caixa de seleção ao lado de cada disco físico que você deseja incluir no pool de armazenamento.
    
    2. Se você quiser designar um ou mais discos como sobressalentes, sob **alocação**, selecione a seta suspensa e selecione **sobressalente ativo**.

7. Sobre o **confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

8. Sobre o **exibir resultados** página, verifique se todas as tarefas concluídas e, em seguida, selecione **fechar**.
    
    >[!NOTE]
    >Como opção, para continuar diretamente na próxima etapa, você pode marcar a caixa de seleção **Criar um disco virtual quando esse assistente fecha**.

9. Em **POOLS DE ARMAZENAMENTO**, verifique se o novo pool de armazenamento está listado.

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>Comandos equivalentes do Windows PowerShell para a criação de pools de armazenamento

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

O exemplo a seguir mostra quais discos físicos estão disponíveis no pool primordial.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

O exemplo a seguir cria um novo pool de armazenamento denominado *StoragePool1* que usa todos os discos disponíveis.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

O exemplo a seguir cria um novo pool de armazenamento *StoragePool1*, que usa quatro dos discos disponíveis.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

A sequência de cmdlets de exemplo a seguir mostra como adicionar um disco físico disponível *PhysicalDisk5* como espera ativa do pool de armazenamento *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## <a name="step-2-create-a-virtual-disk"></a>Etapa 2: Criar um disco virtual

Em seguida, você deverá criar um ou mais discos virtuais no pool de armazenamento. Ao criar um disco virtual, você pode selecionar como os dados são distribuídos entre os discos físicos. Isso afeta a confiabilidade e o desempenho. Você também pode selecionar se deseja criar discos provisionados dinâmicos ou fixos.

1. Se o Assistente de Novo Disco Virtual não estiver aberto, na página **Pools de Armazenamento** no Gerenciador do Servidor, em **POOLS DE ARMAZENAMENTO**, verifique se o pool de armazenamento desejado foi selecionado.

2. Sob **discos virtuais**, selecione o **tarefas** lista e, em seguida, selecione **novo disco Virtual**. O Assistente de novo disco Virtual será aberta.

3. Sobre o **antes de começar** página, selecione **próxima**.

4. Sobre o **selecione o pool de armazenamento** página, selecione o pool de armazenamento desejada e, em seguida, selecione **próxima**.

5. Sobre o **especifique o nome do disco virtual** página, insira um nome e uma descrição opcional e selecione **próxima**.

6. Sobre o **selecionar o layout de armazenamento** página, selecione o layout desejado e selecione **próxima**.
    
    >[!NOTE]
    >Se você selecionar um layout em que você não tem discos físicos suficientes, você receberá uma mensagem de erro quando você seleciona **próxima**. Para obter informações sobre qual layout usar e os requisitos de disco, consulte [pré-requisitos](#prerequisites)).

7. Se você selecionou **espelho** como o layout de armazenamento e você tiver cinco ou mais discos no pool, o **definir configurações de resiliência** página será exibida. Selecione uma das seguintes opções:
    
      - **Espelho de duas vias**
      - **Espelho de três vias**

8. Sobre o **especificar o tipo de provisionamento** página, selecione uma das opções a seguir e selecione **próxima**.
    
      - **Dinâmico**
        
        Com o provisionamento dinâmico, o espaço é alocado conforme a necessidade. Isso otimiza o uso do armazenamento disponível. Contudo, como isso permite a alocação excessiva do armazenamento, é necessário monitorar atentamente quanto espaço em disco está disponível.
    
      - **corrigido**
        
        Com o provisionamento fixo, a capacidade de armazenamento é alocada imediatamente, no momento da criação do disco virtual. Portanto o provisionamento fixo usa espaço do pool de armazenamento igual ao tamanho do disco virtual.
    
    >[!TIP]
    >Com os Espaços de Armazenamento, você pode criar discos virtuais com ambos os provisionamentos dinâmico e fixo no mesmo pool de armazenamento. Por exemplo, você poderia usar um disco virtual com provisionamento dinâmico para hospedar um banco de dados e outro com provisionamento fixo para hospedar os arquivos de log associados.

9. Na página **Especificar o tamanho do disco virtual**, execute o seguinte procedimento:
    
    Se você selecionou provisionamento dinâmico na etapa anterior, na **tamanho do disco Virtual** , insira um tamanho de disco virtual, selecione as unidades (**MB**, **GB**, ou **TB** ), em seguida, selecione **próxima**.
    
    Se você selecionou provisionamento fixo na etapa anterior, selecione um dos seguintes:
    
      - **Especificar tamanho**
        
        Para especificar o tamanho, insira um valor de **tamanho do disco Virtual** caixa e, em seguida, selecione as unidades (**MB**, **GB**, ou **TB**).
        
        Se você usar outro layout de armazenamento que não o simples, o disco virtual usará mais espaço livre do que o tamanho especificado. Para evitar um potencial erro no qual o tamanho do volume excede o espaço livre do pool de armazenamento, marque a caixa de seleção **Criar o maior disco virtual possível, até o tamanho especificado**.
    
      - **Tamanho máximo**
        
        Selecione esta opção para criar um disco virtual que usa a capacidade máxima do pool de armazenamento.

10. Sobre o **confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

11. Sobre o **exibir resultados** página, verifique se todas as tarefas concluídas e, em seguida, selecione **fechar**.
    
    >[!TIP]
    >A caixa de seleção **Criar um volume ao fechar o assistente** é marcada por padrão. Isso levará você até a próxima etapa.

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Comandos equivalentes do Windows PowerShell para criar discos virtuais

O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.

O exemplo a seguir cria um disco virtual de 50 GB chamado *VirtualDisk1* em um pool de armazenamento denominada *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

O exemplo a seguir cria um disco virtual espelhado denominado *VirtualDisk1* em um pool de armazenamento denominada *StoragePool1*. O disco usa a capacidade de armazenamento máximo do pool de armazenamento.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

O exemplo a seguir cria um disco virtual de 50 GB chamado *VirtualDisk1* em um pool de armazenamento denominada *StoragePool1*. O disco usa o tipo de provisionamento dinâmico.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

O exemplo a seguir cria um disco virtual chamado *VirtualDisk1* em um pool de armazenamento denominada *StoragePool1*. O disco virtual usa espelhamento de três vias e possui tamanho fixo de 20 GB.

>[!NOTE]
>Você deve ter pelo menos cinco discos físicos no pool de armazenamento para que este cmdlet funcione. (Isso não inclui discos alocados como espera ativa.)

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>Etapa 3: Criar um volume

Em seguida, você deve criar um volume para o disco virtual. Você pode atribuir uma letra de unidade opcional ou pasta e formatar o volume com um sistema de arquivos.

1. Se o Assistente de novo Volume já não estiver aberto, nos **Pools de armazenamento** página no Gerenciador do servidor, em **discos virtuais**, clique com botão direito no disco virtual desejado e, em seguida, selecione **Novo Volume**.
    
    O Assistente de Novo Volume é aberto.

2. Sobre o **antes de começar** página, selecione **próxima**.

3. Sobre o **selecione o servidor e disco** página, faça o seguinte e, em seguida, selecione **próxima**.
    
    1. No **Server** área, selecione o servidor no qual você deseja provisionar o volume.
    
    2. No **disco** área, selecione o disco virtual no qual você deseja criar o volume.

4. Sobre o **especificar o tamanho do volume** , insira um tamanho de volume, especifique as unidades (**MB**, **GB**, ou **TB**) e, em seguida, selecione **Próxima**.

5. Sobre o **atribuir a uma letra da unidade ou pasta** página, configure a opção desejada e, em seguida, selecione **próxima**.

6. Sobre o **selecionar configurações do sistema de arquivos** página, faça o seguinte e, em seguida, selecione **próxima**.
    
    1. No **sistema de arquivos** lista, selecione **NTFS** ou **ReFS**.
    
    2. Na lista **Tamanho da unidade de alocação**, deixe a configuração **Padrão** ou defina o tamanho da unidade de alocação.
        
        >[!NOTE]
        >Para obter mais informações sobre o tamanho de unidade de alocação, consulte [tamanho de cluster padrão para NTFS, FAT e exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Opcionalmente, na caixa **Rótulo do volume**, digite um nome de rótulo para o volume, por exemplo, **Dados de RH**.

7. Sobre o **confirmar seleções** , verifique se as configurações estão corretas e, em seguida, selecione **criar**.

8. Sobre o **exibir resultados** página, verifique se todas as tarefas concluídas e, em seguida, selecione **fechar**.

9. Para verificar se o volume foi criado, no Gerenciador do servidor, selecione a **Volumes** página. O volume estará listado no servidor onde foi criado. Você também pode verificar se o volume está no Windows Explorer.

### <a name="windows-powershell-equivalent-commands-for-creating-volumes"></a>Comandos equivalentes do Windows PowerShell para a criação de volumes

O seguinte cmdlet do Windows PowerShell executa a mesma função que o procedimento anterior. Digite o comando como uma linha única.

O exemplo a seguir inicializa os discos para o disco virtual *VirtualDisk1*, cria uma partição com uma letra da unidade atribuída e formata o volume com o sistema de arquivos NTFS.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## <a name="additional-information"></a>Informações adicionais

- [Espaços de armazenamento](overview.md)
- [Cmdlets de armazenamento no Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Implantar espaços de armazenamento clusterizados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Espaços de armazenamento, perguntas frequentes (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)
