---
title: Atribuir um caminho da pasta do ponto de montagem a uma unidade.
description: Este artigo descreve como atribuir um caminho da pasta do ponto de montagem (em vez de uma letra da unidade) a uma unidade.
ms.date: 06/07/2020
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 82b12edd9cb680eee567e5dc014615e3d042cd18
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966088"
---
# <a name="mount-a-drive-in-a-folder"></a>Montar uma unidade em uma pasta

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se desejar, use o Gerenciamento de Disco para montar (tornar uma unidade acessível) em uma pasta, em vez de uma letra da unidade. Isso faz com que a unidade apareça como apenas outra pasta. Você pode montar unidades somente em pastas vazias em volumes NTFS básicos ou dinâmicos.

## <a name="mounting-a-drive-in-an-empty-folder"></a>Montar uma unidade em uma pasta vazia

> [!NOTE]
> Para concluir estas etapas, no mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores**.

### <a name="to-mount-a-drive-in-an-empty-folder-by-using-the-windows-interface"></a>Para montar uma unidade em uma pasta vazia usando a interface do Windows

1.  No Gerenciador de Discos, clique com o botão direito do mouse na partição ou no volume que contém a pasta na qual você quer montar a unidade.
2. Clique em **Alterar letra da unidade e caminhos da unidade** e, em seguida, clique em **Adicionar**.
3. Clique em **Montar na seguinte pasta NTFS vazia**.
4. Digite o caminho para uma pasta vazia em um volume NTFS ou clique em **Procurar** para localizá-lo.

### <a name="to-mount-a-drive-in-an-empty-folder-using-a-command-line"></a>Para montar uma unidade em uma pasta vazia usando uma linha de comando

1.  Abra um prompt de comando e digite `diskpart`.

2.  No prompt **DISKPART**, digite `list volume`, anotando o número do volume ao qual você quer atribuir o caminho.

3.  No prompt **DISKPART**, digite `select volume <volumenumber>`, especificando o número do volume ao qual você quer atribuir o caminho.

5.  No prompt **DISKPART**, digite `assign [mount=<path>]`.

### <a name="to-remove-a-mount-point"></a>Para remover um ponto de montagem

Para remover o ponto de montagem para que a unidade não fique mais acessível por meio de uma pasta:

1. Selecione e mantenha o cursor (ou clique com o botão direito do mouse) na unidade montada em uma pasta e escolha **Alterar letra de unidade e caminho**.
2. Selecione a pasta na lista e escolha **Remover**.

| Valor | Descrição |
| --- | --- |
| **list volume** | Exibe uma lista dos volumes básicos e dinâmicos em todos os discos. |
| **select volume**        | Seleciona o volume especificado, onde <em>volumenumber</em> é o número do volume e concede foco a ele. Se nenhum volume for especificado, o comando **select** lista o volume atual com foco. É possível especificar o volume por número, letra da unidade ou caminho da pasta do ponto de montagem. Em um disco básico, a seleção de um volume também oferece o foco da partição correspondente.|
| **assign** | <ul><li> Atribui uma letra da unidade ou caminho da pasta do ponto de montagem ao volume com foco. Se nenhuma letra da unidade ou caminho da pasta do ponto de montagem for especificado, a próxima letra de unidade disponível será atribuída. Um erro será gerado se a letra da unidade ou o caminho da pasta do ponto de montagem já estiver em uso.</li>  <li>Usando o comando **assign**, é possível alterar a letra da unidade associada a uma unidade de disco removível.</li> <li> Não é possível atribuir letras de unidade aos volumes de inicialização ou volumes que contenham o arquivo de paginação. Além disso, não é possível atribuir uma letra da unidade a uma partição OEM (fabricante de equipamento original), a uma partição de sistema EFI ou a qualquer partição GPT que não seja uma partição de dados básicos.</li></ul> |
| **mount=** <em>path</em> | Especifica uma pasta NTFS vazia existente onde a unidade montada residirá.  |

## <a name="additional-considerations"></a>Considerações adicionais

-   Se estiver administrando um computador local ou remoto, você poderá procurar pastas NTFS nesse computador.
-   Os caminhos da pasta do ponto de montagem estão disponíveis somente em pastas vazias nos volumes NTFS básicos ou dinâmicos.
-   Para modificar um caminho da pasta do ponto de montagem, remova-o e, em seguida, crie um novo caminho da pasta usando o novo local. Não é possível modificar o caminho da pasta do ponto de montagem diretamente.
-   Ao atribuir um caminho da pasta do ponto de montagem a uma unidade, use o **Visualizador de Eventos** para verificar no log do sistema se há erros de serviço do Cluster ou avisos indicando falhas de caminho da pasta do ponto de montagem. Esses erros seriam listados como **ClusSvc** na coluna **Origem** e **Recurso de disco físico** na coluna **Categoria**.
-   Também é possível criar uma unidade montada usando o comando [mountvol](https://go.microsoft.com/fwlink/?linkid=64111).

## <a name="additional-references"></a>Referências adicionais
-   [Notação da sintaxe de linha de comando](/previous-versions/orphan-topics/ws.11/cc742449(v=ws.11))
