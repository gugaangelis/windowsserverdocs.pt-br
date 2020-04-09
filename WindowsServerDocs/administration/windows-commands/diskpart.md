---
title: DiskPart
description: O tópico comandos do Windows para o DiskPart, que ajuda você a gerenciar as unidades do computador.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 45fe66e4843b96db8e4593c0e963e4a80dbd22c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845469"
---
# <a name="diskpart"></a>DiskPart

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

Os comandos do DiskPart ajudam você a gerenciar as unidades de seu computador (discos, partições, volumes ou discos rígidos virtuais). 

Antes de usar os comandos do DiskPart, primeiro você deve listar e, em seguida, selecionar um objeto para dar o foco. Quando um objeto tiver foco, todos os comandos do DiskPart que você digitar agirão nesse objeto.

## <a name="list-the-available-objects"></a>Listar os objetos disponíveis

Você pode listar os objetos disponíveis e determinar o número ou a letra da unidade de um objeto usando:

- `list disk`-exibe todos os discos no computador.

- `list volume`-exibe todos os volumes no computador.

- `list partition`-exibe as partições no disco que tem o foco no computador.

- `list vdisk`-exibe todos os discos virtuais no computador.

Quando você usa os comandos da **lista** , um asterisco (\*) é exibido ao lado do objeto com foco.

## <a name="determine-focus"></a>Determinar o foco

Quando você seleciona um objeto, o foco permanece nesse objeto até que você selecione um objeto diferente. Por exemplo, se o foco estiver definido no disco 0 e você selecionar volume 8 no disco 2, o foco mudará do disco 0 para o disco 2, volume 8.

Alguns comandos alteram o foco automaticamente. Por exemplo, quando você cria uma nova partição, o foco alterna automaticamente para a nova partição.

Você só pode dar enfoque a uma partição no disco selecionado. Quando uma partição tem foco, o volume relacionado (se houver) também tem foco. Quando um volume tiver foco, o disco e a partição relacionados também terão foco se o volume for mapeado para uma única partição específica. Se esse não for o caso, o foco no disco e na partição será perdido.

## <a name="diskpart-commands"></a>Comandos do DiskPart

Para iniciar o interpretador de comandos do DiskPart, no prompt de comando, digite:

```
diskpart
```

> [!IMPORTANT]
> Você deve estar no grupo **Administradores** local ou em um grupo com permissões semelhantes para executar o DiskPart. 

Você pode executar os seguintes comandos no interpretador de comandos do DiskPart:

| {1&gt;Comando&lt;1} | Descrição |
| ------- | ----------- |
| [Activo](active.md) | Marca a partição do disco com foco, como ativa. |
| [Adicionar](add.md) | Espelha o volume simples com foco para o disco especificado. |
| [Cancele](assign.md) | Atribui uma letra de unidade ou ponto de montagem ao volume com foco. |
| [Anexar vdisk](attach-vdisk.md) | Anexa (às vezes chamadas de montagens ou superfícies) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local. |
| [Atributos](attributes.md) | Exibe, define ou limpa os atributos de um disco ou volume. |
| [Montagem automática](automount.md) | Habilita ou desabilita o recurso de montagem automática. | 
| [Interrupção](break.md) | Quebra o volume espelhado com foco em dois volumes simples. |
| [Limpar](clean.md) | Remove toda e qualquer formatação de partição ou volume do disco com foco. |
| [Compact vdisk](compact-vdisk.md) | Reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica. |
| [Vertê](convert.md) | Converte os volumes FAT (tabela de alocação de arquivo) e FAT32 no sistema de arquivos NTFS, deixando os arquivos e diretórios existentes intactos. |
| [Criada](create.md) | Cria uma partição em um disco, um volume em um ou mais discos ou um VHD (disco rígido virtual). |
| [Excluir](delete.md) | Exclui uma partição ou um volume. |
| [Desanexar vdisk](detach-vdisk.md) | Interrompe a exibição do VHD (disco rígido virtual) selecionado como uma unidade de disco rígido local no computador host. |
| [Detalhes](detail.md) | Exibe informações sobre o disco, a partição, o volume ou o VHD (disco rígido virtual) selecionado. |
| [Sair](exit.md) | Sai do interpretador de comandos do DiskPart. |
| [Expandir vdisk](expand-vdisk.md) | 
| [Estender](extend.md) | 
| [Sistemas](filesystems.md) | 
| [Ao](format.md) | 
| [GPT](gpt.md) | 
| [Ajuda](help.md) | 
| [Importar](import.md) | 
| [Inativo](inactive.md) | 
| [Lista](list.md) | 
| [Mesclar vdisk](merge-vdisk.md) | 
| [Está](offline.md) | 
| [Conectar](online.md) | 
| [Recupera](recover.md) | 
| [Trab](rem.md) | 
| [Remover](remove.md) | 
| [Corrige](repair.md) | 
| [Examinar novamente](rescan.md) | 
| [Manteve](retain.md) | 
| [Solução](san.md) | 
| [Não](select.md) | 
| [ID do conjunto](set-id.md) | 
| [PodeReduzir](shrink.md) | 
| [UniqueId](uniqueid.md) | 

## <a name="additional-references"></a>Referências adicionais

- [Chave de sintaxe de linha de comando] (command-line-syntax-key.md

- [Visão geral do gerenciamento de disco](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Cmdlets de armazenamento no Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
