---
Title: Comandos de DiskPart
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3cc0667b54dba75d892795f6520664ce7a7a62a5
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192652"
---
# <a name="diskpart-commands"></a>Comandos de DiskPart

Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

Comandos de DiskPart ajudam você a gerenciar as unidades do seu PC (discos, partições, volumes ou discos rígidos virtuais). Antes de usar comandos de DiskPart, você deve primeiro Liste e, em seguida, selecione um objeto para destacá-la. Quando um objeto tem o foco, quaisquer comandos de DiskPart que você digite atuará naquele objeto.

Você pode listar os objetos disponíveis e determinar a letra de unidade ou o número de um objeto usando o **disco de lista, o volume de lista e a partição da lista**, e **listar vdisk** comandos. O **disco de lista, a lista vdisk** e **listar volume** comandos exibem todos os discos e volumes no computador. No entanto, o **lista a partição** comando somente exibe as partições no disco que tem o foco. Quando você usa o **lista** comandos, um asterisco (\*) é exibido ao lado do objeto com o foco.

Quando você seleciona um objeto, o foco permanece nesse objeto até que você selecione um objeto diferente. Por exemplo, se o foco é definido no disco 0 e você seleciona o volume 8 no disco 2, o foco muda do disco 0 para o disco 2, volume 8. Alguns comandos mudam automaticamente o foco. Quando você cria uma nova partição, por exemplo, o foco muda automaticamente para a nova partição.

Você só pode oferecer o foco para uma partição do disco selecionado. Quando uma partição tem o foco, o volume relacionado (se houver) também tem o foco. Quando um volume tiver foco, o disco relacionado e a partição também terá foco se o volume é mapeado para uma única partição específica. Se isso não for o caso, o foco no disco e partição serão perdida.

## <a name="diskpart-commands"></a>Comandos de DiskPart

Para iniciar o interpretador de comandos de DiskPart, no prompt de comando digite:

`diskpart`

> [!IMPORTANT]
> Associação local **administradores** grupo, ou equivalente, é o mínimo necessário para executar o DiskPart. 

Você pode executar os comandos a seguir no interpretador de comandos de Diskpart:

  - [Active](active.md)  
      
  - [Adicionar](add.md)  
      
  - [Atribuir](assign.md)  
      
  - [Anexar vdisk](attach-vdisk.md)  
      
  - [Atributos](attributes.md)  
      
  - [Montagem automática](automount.md)  
      
  - [quebra](break.md)  
      
  - [Limpar](clean.md)  
      
  - [Compactar vdisk](compact-vdisk.md)  
      
  - [Convert](convert.md)  
      
  - [criar](create.md)  
      
  - [Excluir](delete.md)  
      
  - [Desanexar vdisk](detach-vdisk.md)  
      
  - [Detalhes](detail.md)  
      
  - [Sair](exit.md)  
      
  - [Expandir vdisk](expand-vdisk.md)  
      
  - [Extend](extend.md)  
      
  - [sistemas de arquivos](filesystems.md)  
      
  - [Formato](format.md)  
      
  - [GPT](gpt.md)  
      
  - [Ajuda](help.md)  
      
  - [Importar](import.md)  
      
  - [inativo](inactive.md)  
      
  - [Lista](list.md)  
      
  - [Mesclar vdisk](merge-vdisk.md)  
      
  - [Offline](offline.md)  
      
  - [Online](online.md)  
      
  - [recuperar](recover.md)  
      
  - [Rem](rem.md)  
      
  - [Remover](remove.md)  
      
  - [Repair](repair.md)  
      
  - [Rescan](rescan.md)  
      
  - [Reter](retain.md)  
      
  - [San](san.md)  
      
  - [Select](select.md)  
      
  - [Id do conjunto](set-id.md)  
      
  - [reduzir](shrink.md)  
      
  - [Uniqueid](uniqueid.md)  
      

## <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Cmdlets de armazenamento no Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/storage/)
