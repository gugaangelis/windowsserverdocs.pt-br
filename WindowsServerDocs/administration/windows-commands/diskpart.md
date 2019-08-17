---
title: Comandos do DiskPart
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7155dbf34f9986b3ebdd8b549b6a861cf7fcfe3a
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560435"
---
# <a name="diskpart-commands"></a>Comandos do DiskPart

Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008

Os comandos do DiskPart ajudam você a gerenciar as unidades de seu PC (discos, partições, volumes ou discos rígidos virtuais). Antes de usar os comandos do DiskPart, primeiro você deve listar e, em seguida, selecionar um objeto para dar o foco. Quando um objeto tiver foco, todos os comandos do DiskPart que você digitar agirão nesse objeto.

Você pode listar os objetos disponíveis e determinar o número ou a letra da unidade de um objeto usando os comandos **listar disco, listar volume, listar partição**e **listar VDISK** . Os comandos **listar disco, lista VDISK** e **listar volume** exibem todos os discos e volumes no computador. No entanto, o comando **listar partição** só exibe partições no disco que tem o foco. Quando você usa os comandos da **lista** , um asterisco\*() é exibido ao lado do objeto com foco.

Quando você seleciona um objeto, o foco permanece nesse objeto até que você selecione um objeto diferente. Por exemplo, se o foco estiver definido no disco 0 e você selecionar volume 8 no disco 2, o foco mudará do disco 0 para o disco 2, volume 8. Alguns comandos alteram o foco automaticamente. Por exemplo, quando você cria uma nova partição, o foco alterna automaticamente para a nova partição.

Você só pode dar enfoque a uma partição no disco selecionado. Quando uma partição tem foco, o volume relacionado (se houver) também tem foco. Quando um volume tiver foco, o disco e a partição relacionados também terão foco se o volume for mapeado para uma única partição específica. Se esse não for o caso, o foco no disco e na partição será perdido.

## <a name="diskpart-commands"></a>Comandos do DiskPart

Para iniciar o interpretador de comandos do DiskPart, no prompt de comando, digite:

`diskpart`

> [!IMPORTANT]
> A associação no grupo local de **Administradores** , ou equivalente, é o requisito mínimo necessário para executar o DiskPart. 

Você pode executar os seguintes comandos no interpretador de comandos do DiskPart:

  - [Activo](active.md)  
      
  - [Adicionar](add.md)  
      
  - [Atribuir](assign.md)  
      
  - [Anexar vdisk](attach-vdisk.md)  
      
  - [Atributos](attributes.md)  
      
  - [Montagem automática](automount.md)  
      
  - [Interrupção](break.md)  
      
  - [Limpar](clean.md)  
      
  - [Compact vdisk](compact-vdisk.md)  
      
  - [Vertê](convert.md)  
      
  - [Criada](create.md)  
      
  - [Excluir](delete.md)  
      
  - [Desanexar vdisk](detach-vdisk.md)  
      
  - [Detalhes](detail.md)  
      
  - [Sair](exit.md)  
      
  - [Expandir vdisk](expand-vdisk.md)  
      
  - [Estender](extend.md)  
      
  - [Sistemas](filesystems.md)  
      
  - [Formatar](format.md)  
      
  - [GPT](gpt.md)  
      
  - [Ajuda](help.md)  
      
  - [Importarar](import.md)  
      
  - [Inativo](inactive.md)  
      
  - [Lista](list.md)  
      
  - [Mesclar vdisk](merge-vdisk.md)  
      
  - [Está](offline.md)  
      
  - [Conectar](online.md)  
      
  - [Recupera](recover.md)  
      
  - [Trab](rem.md)  
      
  - [Removerr](remove.md)  
      
  - [Corrige](repair.md)  
      
  - [Examinar novamente](rescan.md)  
      
  - [Manteve](retain.md)  
      
  - [Solução](san.md)  
      
  - [Não](select.md)  
      
  - [ID do conjunto](set-id.md)  
      
  - [PodeReduzir](shrink.md)  
      
  - [UniqueId](uniqueid.md)  
      

## <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Cmdlets de armazenamento no Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/)
