---
title: Exportar e importar máquinas virtuais
description: Mostra como exportar e importar máquinas virtuais usando o Gerenciador do Hyper-V ou o Windows PowerShell.
ms.prod: windows-server-threshold
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: b326fe8d7ff05ba73fc94225fa38921b42eb3fc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844317"
---
>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>Exportar e Importar máquinas virtuais

Este artigo mostra como exportar e importar uma máquina virtual, que é uma maneira rápida de mover ou copiá-los. Este artigo também aborda algumas das escolhas fazer ao fazer uma exportação ou importação.

## <a name="export-a-virtual-machine"></a>Exportar uma Máquina Virtual

Uma exportação reúne todos os arquivos necessários em uma unidade – arquivos de disco rígido virtual, os arquivos de configuração de máquina virtual e quaisquer arquivos de ponto de verificação. Você pode fazer isso em uma máquina virtual que está em um estado iniciado ou parado.

### <a name="using-hyper-v-manager"></a>Usando o Gerenciador do Hyper-V

Para criar uma exportação de máquina virtual:

1. No Gerenciador do Hyper-V, a máquina virtual com o botão direito e selecione **exportar**.

2. Escolha um local para armazenar os arquivos exportados e clique em **exportar**.

Quando a exportação for concluída, você pode ver todos os arquivos exportados no local de exportação.

### <a name="using-powershell"></a>Usando o PowerShell

Abra uma sessão como administrador e execute um comando semelhante ao seguinte, depois de substituir \<nome da vm\> e \<caminho\>:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

Para obter detalhes, consulte [Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm).

## <a name="import-a-virtual-machine"></a>Importar uma Máquina Virtual 

Importar uma máquina virtual a registra com o host do Hyper-V. Você pode importar novamente para o host ou o novo host. Se você estiver importando para o mesmo host, você não precisa exportar a máquina virtual em primeiro lugar, como o Hyper-V tenta recriar a máquina virtual a partir de arquivos disponíveis. Importar uma máquina virtual registra-o para que possa ser usado no host Hyper-V.

O Assistente Importar Máquina Virtual também ajuda você a corrigir incompatibilidades que podem existir ao mover de um host para outro. Esse geralmente é as diferenças de hardware físico, como memória, comutadores virtuais e processadores virtuais.

### <a name="import-using-hyper-v-manager"></a>Importar usando o Gerenciador do Hyper-V

Para importar uma máquina virtual:

1. Dos **ações** menu no Gerenciador do Hyper-V, clique em **importar Máquina Virtual**.

2. Clique em **Avançar**.

3. Selecione a pasta que contém os arquivos exportados e clique em **próxima**.

4. Selecione a máquina virtual para importar.

5. Escolha o tipo de importação e, em seguida, clique em **próxima**. (Para obter descrições, consulte [importar tipos](#import-types), abaixo.)

6. Clique em **concluir**.

### <a name="import-using-powershell"></a>Importar usando o PowerShell

Use o **Import-VM** cmdlet, seguindo o exemplo para o tipo de importação que você deseja. Para obter descrições dos tipos, consulte [importar tipos](#import-types), abaixo. 

#### <a name="register-in-place"></a>Registrar in-loco

Esse tipo de importação usa os arquivos de onde eles estão armazenados no momento da importação e retém o ID. da máquina virtual O comando a seguir mostra um exemplo de um arquivo de importação. Execute um comando semelhante com seus próprios valores.

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>Restaurar

Para importar a máquina virtual especificando seu próprio caminho para os arquivos de máquina virtual, execute um comando como este, substituindo os exemplos por seus valores:

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>Importar como uma cópia

Para concluir uma importação de cópia e mover os arquivos de máquina virtual para o local padrão do Hyper-V, execute um comando como este, substituindo os exemplos por seus valores:

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

Para obter detalhes, consulte [Import-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm).

### <a name="import-types"></a>Tipos de importação

Hyper-V oferece três tipos de importação:

- **Registrar in-loco** – esse tipo pressupõe que os arquivos de exportação estão no local onde você vai armazenar e executar a máquina virtual. A máquina virtual importada tem a mesma ID, como fazia no momento da exportação. Por isso, se a máquina virtual já está registrada com o Hyper-V, ele precisa ser excluído antes da importação funciona. Quando a importação for concluída, os arquivos de exportação tornam-se a execução arquivos de estado e não pode ser removido.

- **Restaurar a máquina virtual** – restaurar a máquina virtual para um local escolhido, ou usar o padrão para o Hyper-V. Esse tipo de importação cria uma cópia dos arquivos exportados e os move para o local selecionado. Quando importado, a máquina virtual tem a mesma ID que no momento da exportação. Por isso, se a máquina virtual já está em execução no Hyper-V, ele precisa ser excluído antes que a importação ser concluída. Quando a importação for concluída, os arquivos exportados permanecem intactos e podem ser removidos ou importados novamente.

- **Copiar a máquina virtual** – isso é semelhante ao tipo restaurar, em que você selecione um local para os arquivos. A diferença é que a máquina virtual importada tem uma nova ID exclusiva, o que significa que você pode importar a máquina virtual no mesmo host várias vezes.

