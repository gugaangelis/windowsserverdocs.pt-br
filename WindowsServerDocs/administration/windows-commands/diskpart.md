---
title: DiskPart
description: O tópico comandos do Windows para o **DiskPart**, que ajuda você a gerenciar as unidades do computador.
ms.prod: windows-server
ms.technology: storage
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: eb2921d8da4a4a29c4f700107ef5b6d7bfb41481
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122531"
---
# <a name="diskpart"></a>DiskPart

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

Os comandos do DiskPart ajudam você a gerenciar as unidades de seu computador (discos, partições, volumes ou discos rígidos virtuais).

Antes de usar os comandos do DiskPart, primeiro você deve listar e, em seguida, selecionar um objeto para dar o foco. Quando um objeto tiver foco, todos os comandos do DiskPart que você digitar agirão nesse objeto.

## <a name="list-available-objects"></a>Listar objetos disponíveis

Você pode listar os objetos disponíveis e determinar o número ou a letra da unidade de um objeto usando:

- `list disk`-exibe todos os discos no computador.

- `list volume`-exibe todos os volumes no computador.

- `list partition`-exibe as partições no disco que tem o foco no computador.

- `list vdisk`-exibe todos os discos virtuais no computador.

Quando você usa os comandos da **lista** , um asterisco (*) é exibido ao lado do objeto com foco.

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
| [Expandir vdisk](expand-vdisk.md) | expande um VHD (disco rígido virtual) para o tamanho que você especificar. |
| [Estender](extend.md) | Estende o volume ou a partição com foco, juntamente com seu sistema de arquivos, para espaço livre (não alocado) em um disco. |
| [Sistemas](filesystems.md) | Exibe informações sobre o sistema de arquivos atual do volume com foco e lista os sistemas de arquivos com suporte para formatar o volume. |
| [Ao](format.md) | Formata um disco para aceitar arquivos do Windows. |
| [GPT](gpt.md) | Atribui os atributos GPT à partição com foco em discos básicos da tabela de partição GUID (GPT). |
| [Ajuda](help.md) | Exibe uma lista de comandos disponíveis ou informações de ajuda detalhadas sobre um comando especificado. |
| [Importar](import.md) | Importa um grupo de discos externos para o grupo de discos do computador local. |
| [Inativo](inactive.md) | Marca a partição do sistema ou a partição de inicialização com foco como inativa em discos básicos de MBR (registro mestre de inicialização). |
| [Lista](list.md) | Exibe uma lista de discos, de partições em um disco, de volumes em um disco ou de VHDs (discos rígidos virtuais). |
| [Mesclar vdisk](merge-vdisk.md) | Mescla um VHD (disco rígido virtual) diferencial com seu VHD pai correspondente. |
| [Está](offline.md) | Coloca um disco ou volume online no estado offline. |
| [Conectar](online.md) | Coloca um disco ou volume offline para o estado online. |
| [Recupera](recover.md) | Atualiza o estado de todos os discos em um grupo de discos, tenta recuperar discos em um grupo de discos inválido e sincroniza novamente os volumes espelhados e os volumes RAID-5 que têm dados obsoletos. |
| [Trab](rem.md) | Fornece uma maneira de adicionar comentários a um script. |
| [Remover](remove.md) | Remove uma letra de unidade ou ponto de montagem de um volume. |
| [Corrige](repair.md) | Repara o volume RAID-5 com foco, substituindo a região do disco com falha pelo disco dinâmico especificado. |
| [Examinar novamente](rescan.md) | Localiza novos discos que podem ter sido adicionados ao computador. |
| [Manteve](retain.md) | Prepara um volume simples dinâmico existente para ser usado como um volume do sistema ou de inicialização. |
| [Solução](san.md) | Exibe ou define a política de San (rede de área de armazenamento) para o sistema operacional. |
| [Não](select.md) | Desloca o foco para um disco, partição, volume ou VHD (disco rígido virtual). |
| [ID do conjunto](set-id.md) | Altera o campo de tipo de partição para a partição com foco. |
| [PodeReduzir](shrink.md) | Reduz o tamanho do volume selecionado pelo valor especificado. |
| [UniqueId](uniqueid.md) | Exibe ou define o identificador GPT (tabela de partição GUID) ou a assinatura MBR (registro mestre de inicialização) para o disco com foco. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Visão geral do gerenciamento de disco](https://docs.microsoft.com/windows-server/storage/disk-management/overview-of-disk-management)

- [Cmdlets de armazenamento no Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
