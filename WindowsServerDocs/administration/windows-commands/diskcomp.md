---
title: diskcomp
description: Artigo de referência do comando diskcomp, que compara o conteúdo de dois disquetes.
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 71401942f25d3f503639b2931f2f0ee49229e15b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890973"
---
# <a name="diskcomp"></a>diskcomp

Compara o conteúdo de dois disquetes. Se usado sem parâmetros, **diskcomp** usará a unidade atual para comparar ambos os discos.

## <a name="syntax"></a>Sintaxe

```
diskcomp [<drive1>: [<drive2>:]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive1>` | Especifica a unidade que contém um dos disquetes. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O comando **diskcomp** funciona apenas com disquetes. Não é possível usar **diskcomp** com um disco rígido. Se você especificar uma unidade de disco rígido para *unidade1* ou *unidade2*, **diskcomp** exibirá a seguinte mensagem de erro:

  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```

- Se todas as faixas nos dois discos em comparação forem as mesmas (ele ignorará o número de volume de um disco), **diskcomp** exibirá a seguinte mensagem:

  ```
  Compare OK
  ```

  Se as trilhas não forem as mesmas, **diskcomp** exibirá uma mensagem semelhante à seguinte:

  ```
  Compare error on
  side 1, track 2
  ```

  Quando **diskcomp** conclui a comparação, ele exibe a seguinte mensagem:

  ```
  Compare another diskette (Y/N)?
  ```

  Se você pressionar **s**, **diskcomp** solicitará que você insira o disco para a próxima comparação. Se você pressionar **N**, **diskcomp** interromperá a comparação.

- Se você omitir o parâmetro *unidade2* , **diskcomp** usará a unidade atual para *unidade2*. Se você omitir os dois parâmetros de unidade, **diskcomp** usará a unidade atual para ambos. Se a unidade atual for igual a *unidade1*, **diskcomp** solicitará que você troque os discos conforme necessário.

- Se você especificar a mesma unidade de disquete para *unidade1* e *unidade2*, **diskcomp** as compara usando uma unidade e solicita que você insira os discos conforme necessário. Talvez seja necessário trocar os discos mais de uma vez, dependendo da capacidade dos discos e da quantidade de memória disponível.

- **Diskcomp** não pode comparar um disco de um único lado com um disco duplo, nem um disco de alta densidade com um disco de densidade dupla. Se o disco em *unidade1* não for do mesmo tipo que o disco em *unidade2*, **diskcomp** exibirá a seguinte mensagem:

  ```
  Drive types or diskette types not compatible
  ```

- **Diskcomp** não funciona em uma unidade de rede ou em uma unidade criada pelo comando **subst** . Se você tentar usar **diskcomp** com uma unidade de qualquer um desses tipos, **diskcomp** exibirá a seguinte mensagem de erro:

  ```
  Invalid drive specification
  ```

- Se você usar **diskcomp** com um disco que você fez usando **Copy**, **diskcomp** poderá exibir uma mensagem semelhante à seguinte:

  ```
  Compare error on
  side 0, track 0
  ```

  Esse tipo de erro pode ocorrer mesmo que os arquivos nos discos sejam idênticos. Embora a **cópia** duplique as informações, elas não são necessariamente colocadas no mesmo local no disco de destino.

- códigos de saída de **diskcomp** :

  | Código de saída | Descrição |
  | --------- | ----------- |
  | 0 | Os discos são os mesmos |
  | 1 | Foram encontradas diferenças |
  | 3 | Ocorreu um erro de hardware |
  | 4 | Ocorreu um erro de inicialização |

  Para processar códigos de saída retornados por **diskcomp**, você pode usar a variável de ambiente *ERRORLEVEL* na linha de comando **If** em um programa em lotes.

## <a name="examples"></a>Exemplos

Se o computador tiver apenas uma unidade de disquete (por exemplo, unidade A) e você quiser comparar dois discos, digite:

```
diskcomp a: a:
```

**Diskcomp** solicita que você insira cada disco, conforme necessário.

Para ilustrar como processar um código de saída de **diskcomp** em um programa em lotes que usa a variável de ambiente *ERRORLEVEL* na linha de comando **If** :

```
rem Checkout.bat compares the disks in drive A and B
echo off
diskcomp a: b:
if errorlevel 4 goto ini_error
if errorlevel 3 goto hard_error
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok
:ini_error
echo ERROR: Insufficient memory or command invalid
goto exit
:hard_error
echo ERROR: An irrecoverable error occurred
goto exit
:break
echo You just pressed CTRL+C to stop the comparison
goto exit
:no_compare
echo Disks are not the same
goto exit
:compare_ok
echo The comparison was successful; the disks are the same
goto exit
:exit
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
