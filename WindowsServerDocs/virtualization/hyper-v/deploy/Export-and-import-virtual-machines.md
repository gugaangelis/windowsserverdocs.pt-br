---
title: Exportar e importar máquinas virtuais
description: Mostra como exportar e importar máquinas virtuais usando o Gerenciador do Hyper-V ou o Windows PowerShell.
ms.prod: windows-server
author: kbdazure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: 1e9cd8710a53c1e5d9d97e464c32dbf7f17d29a7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860889"
---
>Aplica-se a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>Exportar e Importar máquinas virtuais

Este artigo mostra como exportar e importar uma máquina virtual, que é uma maneira rápida de movê-las ou copiá-las. Este artigo também aborda algumas das opções a serem feitas ao fazer uma exportação ou importação.

## <a name="export-a-virtual-machine"></a>Exportar uma Máquina Virtual

Uma exportação reúne todos os arquivos necessários em uma unidade--arquivos de disco rígido virtual, arquivos de configuração de máquina virtual e qualquer arquivo de ponto de verificação. Você pode fazer isso em uma máquina virtual que esteja no estado iniciado ou parado.

### <a name="using-hyper-v-manager"></a>Uso do Gerenciador Hyper-V

Para criar uma exportação de máquina virtual:

1. No Gerenciador do Hyper-V, clique com o botão direito do mouse na máquina virtual e selecione **Exportar**.

2. Escolha onde armazenar os arquivos exportados e clique em **Exportar**.

Quando a exportação for concluída, você poderá ver todos os arquivos exportados no local de exportação.

### <a name="using-powershell"></a>Uso do PowerShell

Abra uma sessão como administrador e execute um comando como o seguinte, depois de substituir \<nome da VM\> e \<\>de caminho:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

Para obter detalhes, consulte [Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm).

## <a name="import-a-virtual-machine"></a>Importar uma Máquina Virtual 

Importar uma máquina virtual a registra com o host do Hyper-V. Você pode importar de volta para o host ou para o novo host. Se estiver importando para o mesmo host, você não precisará exportar a máquina virtual primeiro, porque o Hyper-V tenta recriar a máquina virtual de arquivos disponíveis. A importação de uma máquina virtual a registra para que possa ser usada no host do Hyper-V.

O assistente de importação de máquina virtual também ajuda a corrigir incompatibilidades que podem existir ao mudar de um host para outro. Normalmente, isso é uma diferença no hardware físico, como memória, comutadores virtuais e processadores virtuais.

### <a name="import-using-hyper-v-manager"></a>Importar usando o Gerenciador do Hyper-V

Para importar uma máquina virtual:

1. No menu **ações** no Gerenciador do Hyper-V, clique em **importar máquina virtual**.

2. Clique em **Avançar**.

3. Selecione a pasta que contém os arquivos exportados e clique em **Avançar**.

4. Selecione a máquina virtual a ser importada.

5. Escolha o tipo de importação e clique em **Avançar**. (Para obter descrições, consulte [tipos de importação](#import-types), abaixo.)

6. Clique em **Concluir**.

### <a name="import-using-powershell"></a>Importar usando o PowerShell

Use o cmdlet **Import-VM** , seguindo o exemplo para o tipo de importação desejado. Para obter descrições dos tipos, consulte [tipos de importação](#import-types), abaixo. 

#### <a name="register-in-place"></a>Registrar em vigor

Esse tipo de importação usa os arquivos onde eles são armazenados no momento da importação e retém a ID da máquina virtual. O comando a seguir mostra um exemplo de um arquivo de importação. Execute um comando semelhante com seus próprios valores.

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>Restauração

Para importar a máquina virtual especificando seu próprio caminho para os arquivos de máquina virtual, execute um comando como este, substituindo os exemplos pelos seus valores:

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>Importar como uma cópia

Para concluir uma importação de cópia e mover os arquivos da máquina virtual para o local do Hyper-V padrão, execute um comando como este, substituindo os exemplos pelos seus valores:

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

Para obter detalhes, consulte [Import-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm).

### <a name="import-types"></a>Tipos de importação

O Hyper-V oferece três tipos de importação:

- **Registro in-loco** – esse tipo assume que os arquivos de exportação estão no local onde você armazenará e executará a máquina virtual. A máquina virtual importada tem a mesma ID que foi no momento da exportação. Por isso, se a máquina virtual já estiver registrada com o Hyper-V, ela precisará ser excluída antes que a importação funcione. Quando a importação for concluída, os arquivos de exportação se tornarão os arquivos de estado em execução e não poderão ser removidos.

- **Restaurar a máquina virtual** – restaure a máquina virtual em um local que você escolher ou use o padrão para o Hyper-V. Esse tipo de importação cria uma cópia dos arquivos exportados e os move para o local selecionado. Quando importado, a máquina virtual tem a mesma ID que no momento da exportação. Por isso, se a máquina virtual já estiver em execução no Hyper-V, ela precisará ser excluída para que a importação possa ser concluída. Quando a importação for concluída, os arquivos exportados permanecerão intactos e poderão ser removidos ou importados novamente.

- **Copie a máquina virtual** – isso é semelhante ao tipo de restauração no qual você seleciona um local para os arquivos. A diferença é que a máquina virtual importada tem uma nova ID exclusiva, o que significa que você pode importar a máquina virtual para o mesmo host várias vezes.

