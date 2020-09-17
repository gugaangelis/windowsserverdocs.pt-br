---
title: Criar arquivos de conjunto de VHD do Hyper-V
description: Etapas para criar um arquivo VHDset no Hyper-v 2016
author: jiwool
ms.author: jiwool
ms.date: 01/26/2017
ms.topic: article
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
ms.openlocfilehash: 7e2c287666115b9a81c6ab5585187f26dad7a5e3
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746461"
---
# <a name="create-hyper-v-vhd-set-files"></a>Criar arquivos de conjunto de VHD do Hyper-V
Os arquivos do conjunto VHD são um novo modelo de disco virtual compartilhado para clusters convidados no Windows Server 2016. Os arquivos de conjunto de VHD dão suporte ao redimensionamento online de discos virtuais compartilhados, dão suporte à réplica do Hyper-V e podem ser incluídos em pontos de verificação consistentes com o aplicativo.

Os arquivos do conjunto VHD usam um novo tipo de arquivo VHD,. VHDS. Os arquivos do conjunto VHD armazenam informações de ponto de verificação sobre o disco virtual do grupo usado em clusters convidados, na forma de metadados.

O Hyper-V trata de todos os aspectos do gerenciamento das cadeias de pontos de verificação e da mesclagem do conjunto de VHD compartilhado. O software de gerenciamento pode executar operações de disco como redimensionamento online em arquivos de conjunto VHD da mesma maneira que no. Arquivos VHDX. Isso significa que o software de gerenciamento não precisa saber sobre o formato de arquivo do conjunto VHD.

> [!NOTE]
> É importante avaliar o impacto dos arquivos de conjunto VHD antes da implantação na produção. Certifique-se de que não haja degradação de desempenho ou funcional em seu ambiente, como a latência de disco.

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>Criar um arquivo de conjunto VHD do Gerenciador do Hyper-V

1.  Abra o Gerenciador do Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.
2.  No painel Ação, clique em **Novo** e em **Disco Rígido**.
3.  Na página **escolher formato de disco** , selecione **VHD definido** como o formato do disco rígido virtual.
4.  Continue nas páginas do assistente para personalizar o disco rígido virtual. Você pode clicar em **Próximo** para ir para as outras páginas do assistente ou pode clicar no nome de uma página no painel do lado esquerdo para ir diretamente para essa página.
5.  Após terminar de configurar o disco rígido virtual, clique em **Concluir**.

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>Criar um arquivo de conjunto de VHD do Windows PowerShell

Use o cmdlet [New-VHD](/powershell/module/hyper-v/new-vhd?view=win10-ps) , com o tipo de arquivo. VHDS no caminho do arquivo. Este exemplo cria um arquivo de conjunto VHD chamado base. VHDs com 10 gigabytes de tamanho.

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>Migrar um arquivo VHDX compartilhado para um arquivo de conjunto VHD

Migrar um VHDX compartilhado existente para um VHDS requer colocar a VM offline. Este é o processo recomendado usando o Windows PowerShell:

1. Remova o VHDX da VM. Por exemplo, execute:
   ``` PowerShell
   PS c:\>Remove-VMHardDiskDrive existing.vhdx
   ```

2. Converta o VHDX em um VHDS. Por exemplo, execute:
   ``` PowerShell
   PS c:\>Convert-VHD existing.vhdx new.vhds
   ```

3. Adicione os VHDS à VM. Por exemplo, execute:
   ``` PowerShell
   PS c:\>Add-VMHardDiskDrive new.vhds
   ```