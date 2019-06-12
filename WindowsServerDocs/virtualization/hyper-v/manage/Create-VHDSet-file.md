---
title: Criar arquivos de conjunto de VHD do Hyper-V
description: Etapas para criar um arquivo VHDset em 2016 Hyper-v
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: a5a6f79d362b9058ca29d979457a1dcdfc0c9f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445695"
---
# <a name="create-hyper-v-vhd-set-files"></a>Criar arquivos de conjunto de VHD do Hyper-V
Arquivos VHD definido são um novo modelo de disco Virtual compartilhado para clusters de convidado no Windows Server 2016. Arquivos VHD definido dão suporte ao redimensionamento online de discos virtuais compartilhados, dão suporte a réplica do Hyper-V e podem ser incluídos em pontos de verificação consistente com o aplicativo. 

Arquivos VHD definida usam um novo tipo de arquivo VHD. VHDS. Arquivos VHD definido armazenam informações de ponto de verificação sobre o disco virtual de grupo usado em clusters de convidado, na forma de metadados.

Hyper-V trata todos os aspectos do gerenciamento de cadeias de ponto de verificação e mesclar o VHD compartilhado definido. Software de gerenciamento pode executar operações de disco como um redimensionamento online em arquivos VHD definido da mesma forma que para o. Arquivos VHDX. Isso significa que o software de gerenciamento não precisa saber sobre o formato de arquivo VHD definido.

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>Crie um arquivo de VHD definido usando o Gerenciador do Hyper-V

1.  Abra o Gerenciador Hyper-V. Clique em **Iniciar**, vá em **Ferramentas Administrativas** e clique em **Gerenciador do Hyper-V**.
2.  No painel Ação, clique em **Novo** e em **Disco Rígido**.
3.  Sobre o **Escolher formato de disco** página, selecione **VHD definir** como o formato do disco rígido virtual.
4.  Prossiga com as páginas do Assistente para personalizar o disco rígido virtual. Você pode clicar em **Próximo** para ir para as outras páginas do assistente ou pode clicar no nome de uma página no painel do lado esquerdo para ir diretamente para essa página.
5.  Após terminar de configurar o disco rígido virtual, clique em **Concluir**.

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>Crie um arquivo de conjunto de VHD do Windows PowerShell

Use o [New-VHD](https://technet.microsoft.com/library/hh848503.aspx) cmdlet com o tipo de arquivo. VHDS no caminho do arquivo. Este exemplo cria um arquivo VHD definido chamado base.vhds é de 10 Gigabytes de tamanho.

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>Migrar de um arquivo VHDX compartilhado para um arquivo VHD definido

Migrando um VHDX compartilhado existente para um VHDS requer colocar a VM offline. Isso é o processo recomendado usando o Windows PowerShell:

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
  



