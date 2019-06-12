---
title: Atribua um caminho de pasta do ponto de montagem de uma unidade.
description: Este artigo descreve como atribuir um caminho de pasta do ponto de montagem (em vez de uma letra de unidade) a uma unidade.
keywords: virtualização, segurança, malware
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d09024b3c7f7a1e55c9e9c2ece56e037fe7e16f2
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812497"
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>Atribuir um caminho de pasta do ponto de montagem a uma unidade

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar o Gerenciamento de Disco para atribuir um caminho de pasta do ponto de montagem (em vez de uma letra de unidade) a uma unidade. Os caminhos de pasta do ponto de montagem estão disponíveis somente em pastas vazias nos volumes NTFS básicos ou dinâmicos.

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>Atribuição de caminho de pasta do ponto de montagem a uma unidade

> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-by-using-the-windows-interface"></a>Para atribuir um caminho de pasta do ponto de montagem para uma unidade usando a interface do Windows

1.  No Gerenciador de Disco, clique com botão direito do mouse na partição ou no volume no qual você deseja atribuir o caminho da pasta do ponto de montagem. 
2. Clique em **Alterar a letra e os caminhos da unidade** e, em seguida, clique em **Adicionar**. 
3. Clique em **Montar na seguinte pasta NTFS vazia**.
4. Digite o caminho para uma pasta vazia em um volume NTFS ou clique em **Procurar** para localizá-lo.

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>Para atribuir um caminho de pasta do ponto de montagem a uma unidade usando uma linha de comando

1.  Abra um prompt de comando e digite `diskpart`.

2.  No prompt de comando **DISKPART**, digite `list volume`, anotando o número do volume que você deseja atribuir ao caminho.

3.  No prompt de comando **DISKPART**, digite `select volume <volumenumber>`. 

4. Selecione o volume simples *volumenumber* que você deseja atribuir ao caminho.

5.  No prompt de comando **DISKPART**, digite `assign [mount=<path>]`.

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>Para remover um caminho de pasta do ponto de montagem a uma unidade

-   Para remover um caminho de pasta do ponto de montagem, clique nele e, em seguida, clique em **Remover**.

| Valor | Descrição |
| --- | --- |
| **list volume** | Exibe uma lista dos volumes básicos e dinâmicos em todos os discos. |
| **Selecione o volume**        | Seleciona o volume especificado, onde <em>volumenumber</em> é o número do volume e concede foco a ele. Se nenhum volume for especificado, o comando **select** lista o volume atual com foco. Você pode especificar o volume por número, letra da unidade ou caminho de pasta do ponto de montagem. Em um disco básico, a seleção de um volume também oferece o foco de partição correspondente.|
| **assign** | <ul><li> Atribui uma letra da unidade ou caminho de pasta do ponto de montagem para o volume com foco. Se nenhuma letra de unidade ou caminho de pasta do ponto de montagem for especificado, a próxima letra de unidade disponível é atribuída. Um erro é gerado se a letra da unidade ou o caminho da pastada do ponto de montagem já estiver em uso.</li>  <li>Usando o comando **assign**, é possível alterar a letra da unidade associada a uma unidade removível.</li> <li> Você não pode atribuir letras de unidade aos volumes de inicialização ou volumes que contêm o arquivo de paginação. Além disso, você não pode atribuir uma letra de unidade a uma partição OEM, a uma partição de sistema EFI ou a qualquer partição GPT que não seja de dados básicos.</li></ul> |
| **mount=** <em>path</em> | Especifica uma pasta NTFS vazia existente onde ficará a unidade montada.  |

## <a name="additional-considerations"></a>Considerações adicionais

-   Se estiver administrando um computador local ou remoto, você poderá procurar pastas NTFS nesse computador.
-   Os caminhos de pasta do ponto de montagem estão disponíveis somente em pastas vazias nos volumes NTFS básicos ou dinâmicos.
-   Para modificar um caminho de pasta do ponto de montagem, remova-o e, em seguida, crie um novo caminho de pasta usando o novo local. Não é possível modificar o caminho da pasta do ponto de montagem diretamente.
-   Quando você atribuir um caminho de pasta do ponto de montagem a uma unidade, use o **Visualizador de Eventos** para verificar no log do sistema se há erros de serviço do Cluster ou avisos indicando falhas de caminho da pasta do ponto de montagem. Esses erros seriam listados como **ClusSvc** na coluna **Origem** e **Recurso de disco físico** na coluna **Categoria**.
-   Você também pode criar uma unidade montada usando o comando [mountvol](https://go.microsoft.com/fwlink/?linkid=64111).

## <a name="see-also"></a>Consulte também
-   [Notação de sintaxe de linha de comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


