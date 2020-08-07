---
title: diskpart
description: Artigo de referência do interpretador de comandos do DiskPart, que ajuda você a gerenciar as unidades do computador.
author: jasongerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 627f9254606b1ed70b198f6dd0096ccbff424c45
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890924"
---
# <a name="diskpart"></a>diskpart

> Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

O interpretador de comandos do DiskPart ajuda você a gerenciar as unidades de seu computador (discos, partições, volumes ou discos rígidos virtuais).

Antes de usar os comandos do **DiskPart** , primeiro você deve listar e, em seguida, selecionar um objeto para dar o foco. Depois que um objeto tiver foco, todos os comandos do DiskPart que você digitar agirão nesse objeto.

## <a name="list-available-objects"></a>Listar objetos disponíveis

Você pode listar os objetos disponíveis e determinar o número ou a letra da unidade de um objeto usando:

- `list disk`-Exibe todos os discos no computador.

- `list volume`-Exibe todos os volumes no computador.

- `list partition`-Exibe as partições no disco que tem o foco no computador.

- `list vdisk`-Exibe todos os discos virtuais no computador.

Depois de executar os comandos da **lista** , um asterisco (*) é exibido ao lado do objeto com foco.

## <a name="determine-focus"></a>Determinar o foco

Quando você seleciona um objeto, o foco permanece nesse objeto até que você selecione um objeto diferente. Por exemplo, se o foco estiver definido no disco 0 e você selecionar volume 8 no disco 2, o foco mudará do disco 0 para o disco 2, volume 8.

Alguns comandos alteram o foco automaticamente. Por exemplo, quando você cria uma nova partição, o foco alterna automaticamente para a nova partição.

Você só pode dar enfoque a uma partição no disco selecionado. Depois que uma partição tiver foco, o volume relacionado (se houver) também terá o foco. Após o foco de um volume, o disco e a partição relacionados também terão foco se o volume for mapeado para uma única partição específica. Se esse não for o caso, o foco no disco e na partição será perdido.

## <a name="syntax"></a>Sintaxe

Para iniciar o interpretador de comandos do DiskPart, no prompt de comando, digite:

```
diskpart <parameter>
```

> [!IMPORTANT]
> Você deve estar no grupo **Administradores** local ou em um grupo com permissões semelhantes para executar o DiskPart.

### <a name="parameters"></a>Parâmetros

Você pode executar os seguintes comandos no interpretador de comandos do DiskPart:

| Comando | Descrição |
| ------- | ----------- |
| [active](active.md) | Marca a partição do disco com foco, como ativa. |
| [add](add.md) | Espelha o volume simples com foco para o disco especificado. |
| [assign](assign.md) | Atribui uma letra de unidade ou ponto de montagem ao volume com foco. |
| [attach vdisk](attach-vdisk.md) | Anexa (às vezes chamadas de montagens ou superfícies) um disco rígido virtual (VHD) para que ele apareça no computador host como uma unidade de disco rígido local. |
| [attributes](attributes.md) | Exibe, define ou limpa os atributos de um disco ou volume. |
| [automount](automount.md) | Habilita ou desabilita o recurso de montagem automática. |
| [break](break.md) | Quebra o volume espelhado com foco em dois volumes simples. |
| [clean](clean.md) | Remove toda e qualquer formatação de partição ou volume do disco com foco. |
| [compact vdisk](compact-vdisk.md) | Reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica. |
| [convert](convert.md) | Converte os volumes FAT (tabela de alocação de arquivo) e FAT32 no sistema de arquivos NTFS, deixando os arquivos e diretórios existentes intactos. |
| [create](create.md) | Cria uma partição em um disco, um volume em um ou mais discos ou um VHD (disco rígido virtual). |
| [delete](delete.md) | Exclui uma partição ou um volume. |
| [detach vdisk](detach-vdisk.md) | Interrompe a exibição do VHD (disco rígido virtual) selecionado como uma unidade de disco rígido local no computador host. |
| [detail](detail.md) | Exibe informações sobre o disco, a partição, o volume ou o VHD (disco rígido virtual) selecionado. |
| [exit](exit.md) | Sai do interpretador de comandos do DiskPart. |
| [expand vdisk](expand-vdisk.md) | Expande um VHD (disco rígido virtual) para o tamanho que você especificar. |
| [extend](extend.md) | Estende o volume ou a partição com foco, juntamente com seu sistema de arquivos, para espaço livre (não alocado) em um disco. |
| [filesystems](filesystems.md) | Exibe informações sobre o sistema de arquivos atual do volume com foco e lista os sistemas de arquivos com suporte para formatar o volume. |
| [format](format.md) | Formata um disco para aceitar arquivos do Windows. |
| [gpt](gpt.md) | Atribui os atributos GPT à partição com foco em discos básicos da tabela de partição GUID (GPT). |
| [help](help.md) | Exibe uma lista de comandos disponíveis ou informações de ajuda detalhadas sobre um comando especificado. |
| [import](import.md) | Importa um grupo de discos externos para o grupo de discos do computador local. |
| [inactive](inactive.md) | Marca a partição do sistema ou a partição de inicialização com foco como inativa em discos básicos de MBR (registro mestre de inicialização). |
| [list](list.md) | Exibe uma lista de discos, de partições em um disco, de volumes em um disco ou de VHDs (discos rígidos virtuais). |
| [merge vdisk](merge-vdisk.md) | Mescla um VHD (disco rígido virtual) diferencial com seu VHD pai correspondente. |
| [offline](offline.md) | Coloca um disco ou volume online no estado offline. |
| [online](online.md) | Coloca um disco ou volume offline para o estado online. |
| [recover](recover.md) | Atualiza o estado de todos os discos em um grupo de discos, tenta recuperar discos em um grupo de discos inválido e sincroniza novamente os volumes espelhados e os volumes RAID-5 que têm dados obsoletos. |
| [rem](rem.md) | Fornece uma maneira de adicionar comentários a um script. |
| [remove](remove.md) | Remove uma letra de unidade ou ponto de montagem de um volume. |
| [repair](repair.md) | Repara o volume RAID-5 com foco, substituindo a região do disco com falha pelo disco dinâmico especificado. |
| [rescan](rescan.md) | Localiza novos discos que podem ter sido adicionados ao computador. |
| [retain](retain.md) | Prepara um volume simples dinâmico existente para ser usado como um volume do sistema ou de inicialização. |
| [san](san.md) | Exibe ou define a política de San (rede de área de armazenamento) para o sistema operacional. |
| [select](select.md) | Desloca o foco para um disco, partição, volume ou VHD (disco rígido virtual). |
| [set id](set-id.md) | Altera o campo de tipo de partição para a partição com foco. |
| [shrink](shrink.md) | Reduz o tamanho do volume selecionado pelo valor especificado. |
| [uniqueid](uniqueid.md) | Exibe ou define o identificador GPT (tabela de partição GUID) ou a assinatura MBR (registro mestre de inicialização) para o disco com foco. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Visão geral do gerenciamento de disco](../../storage/disk-management/overview-of-disk-management.md)

- [Cmdlets de armazenamento no Windows PowerShell](/powershell/module/storage/)
