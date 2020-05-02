---
title: assign
description: Tópico de referência para o comando assign, que atribui uma letra de unidade ou ponto de montagem ao volume com foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f17c22a0052ade6f16e7842813a04c95e76b57ab
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718982"
---
# <a name="assign"></a>assign

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atribui uma letra de unidade ou ponto de montagem ao volume com foco. Você também pode usar esse comando para alterar a letra da unidade associada a uma unidade removível. Se nenhuma letra de unidade ou ponto de montagem for especificado, a próxima letra de unidade disponível será atribuída. Se a letra da unidade ou o ponto de montagem já estiver em uso, um erro será gerado.

Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.

> [!IMPORTANT]
> Você não pode atribuir letras de unidade a volumes do sistema, volumes de inicialização ou volumes que contêm o arquivo de paginação. Além disso, não é possível atribuir uma letra da unidade a uma partição OEM (fabricante original do equipamento) ou a qualquer partição GPT (tabela de partição GUID) que não seja uma partição de dados básica.

## <a name="syntax"></a>Sintaxe

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `letter=<d>` | A letra da unidade que você deseja atribuir ao volume. |
| `mount=<path>` | O caminho do ponto de montagem que você deseja atribuir ao volume. Para obter instruções sobre como usar esse comando, consulte [atribuir um caminho de pasta de ponto de montagem a uma unidade](https://docs.microsoft.com/windows-server/storage/disk-management/assign-a-mount-point-folder-path-to-a-drive). |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para atribuir a letra E ao volume em foco, digite:

```
assign letter=e
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando de volume](select-volume.md)