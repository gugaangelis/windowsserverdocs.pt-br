---
title: assign
description: Tópico de comandos do Windows para **atribuir**, que atribui uma letra de unidade ou ponto de montagem ao volume com foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57912b73-622e-489b-b053-a369021ba8e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4745b0472e2c8ee7a4034d9a06d395d6089db6f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851299"
---
# <a name="assign"></a>assign

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atribui uma letra de unidade ou ponto de montagem ao volume com foco.

## <a name="syntax"></a>Sintaxe

```
assign [{letter=<d> | mount=<path>}] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `letter=<d>` | A letra da unidade que você deseja atribuir ao volume. |
| `mount=<path>` | O caminho do ponto de montagem que você deseja atribuir ao volume. Para obter instruções sobre como usar esse comando, consulte [atribuir um caminho de pasta de ponto de montagem a uma unidade](https://go.microsoft.com/fwlink/?LinkId=207059). |
| NOERR | somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="remarks"></a>Comentários

- Se nenhuma letra de unidade ou ponto de montagem for especificado, a próxima letra de unidade disponível será atribuída. Se a letra da unidade ou o ponto de montagem já estiver em uso, um erro será gerado.

- Usando o comando atribuir, você pode alterar a letra da unidade associada a uma unidade removível.

- Não é possível atribuir letras de unidade a volumes do sistema, volumes de inicialização ou volumes que contêm o arquivo de paginação. Além disso, não é possível atribuir uma letra da unidade a uma partição OEM (fabricante original do equipamento) ou a qualquer partição GPT (tabela de partição GUID) que não seja uma partição de dados básica.

- Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.

## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para atribuir a letra E ao volume em foco, digite:
```
assign letter=e
```

## <a name="additional-references"></a>Referências adicionais

- [Chave de sintaxe de linha de comando] (command-line-syntax-key.md

