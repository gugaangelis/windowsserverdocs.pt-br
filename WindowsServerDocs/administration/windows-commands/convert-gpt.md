---
title: convert gpt
description: Artigo de referência do comando Convert GPT, que converte um disco básico vazio com o estilo de partição MBR (registro mestre de inicialização) em um disco básico com o estilo de partição GPT (tabela de partição GUID).
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec1066e0ac50536db915eed9df7a6076ba5f3879
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892575"
---
# <a name="convert-gpt"></a>convert gpt

Converte um disco básico vazio com o estilo de partição MBR (registro mestre de inicialização) em um disco básico com o estilo de partição GPT (tabela de partição GUID). Um disco MBR básico deve ser selecionado para que essa operação tenha sucesso. Use o [comando selecionar disco](select-disk.md) para selecionar um disco básico e deslocar o foco para ele.

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco básico. Faça backup dos dados e exclua todas as partições ou volumes antes de converter o disco. O tamanho mínimo de disco necessário para conversão em GPT é de 128 megabytes.

> [!NOTE]
> Para obter instruções sobre como usar esse comando, consulte [alterar um disco de registro mestre de inicialização para um disco de tabela de partição GUID](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc725671(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
convert gpt [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para converter um disco básico do estilo de partição MBR para o estilo de partição GPT, digite:

```
convert gpt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Convert](convert.md)
