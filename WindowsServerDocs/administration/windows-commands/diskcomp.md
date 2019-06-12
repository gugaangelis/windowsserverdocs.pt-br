---
title: diskcomp
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ccd1a347f9ac51fc98c963dedb1c0ab3fcd27d41
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439592"
---
# <a name="diskcomp"></a>diskcomp



Compara o conteúdo de dois disquetes. Se usado sem parâmetros, **diskcomp** usa a unidade atual para comparar os dois discos. Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
diskcomp [<Drive1>: [<Drive2>:]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive1>|Especifica a unidade que contém um dos disquetes.|
|\<Drive2>|Especifica a unidade que contém o outro disquete.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Usando discos

  O **diskcomp** comando funciona somente com disquetes. Não é possível usar **diskcomp** com um disco rígido. Se você especificar uma unidade de disco rígido para *unidade1* ou *unidade2*, **diskcomp** exibe a seguinte mensagem de erro:  
  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```  
- Comparar os discos

  Se todas as faixas nos dois discos que estão sendo comparados forem iguais, **diskcomp** exibe a seguinte mensagem:  
  ```
  Compare OK
  ```  
  Se as faixas não são os mesmos **diskcomp** exibe uma mensagem semelhante à seguinte:  
  ```
  Compare error on
  side 1, track 2
  ```  
  Quando **diskcomp** completar a comparação, ele exibe a seguinte mensagem:  
  ```
  Compare another diskette (Y/N)?
  ```  
  Se você pressionar S, **diskcomp** solicita que você insira o disco para a próxima comparação. Se você pressionar N, **diskcomp** interrompe a comparação.

  Quando **diskcomp** faz a comparação, ele ignora o número de volume do disco.
- A omissão de parâmetros de unidade

  Se você omitir a *unidade2* parâmetro, **diskcomp** usa a unidade atual para o *unidade2*. Se você omitir os dois parâmetros de unidade **diskcomp** usa a unidade atual para ambos. Se a unidade atual for igual a *unidade1*, **diskcomp** solicitará a troca de discos, conforme necessário.
- Usando uma unidade de disco

  Se você especificar a mesma unidade de disquete para *unidade1* e *unidade2*, **diskcomp** compara-as por meio de uma unidade e solicitará que você insira os discos conforme necessário. Você terá que trocar os discos de mais de uma vez, dependendo da capacidade dos discos e a quantidade de memória disponível.
- Comparando tipos diferentes de discos

  **Diskcomp** não é possível comparar um disco de lado único com um disco de dupla face, nem um disco de alta densidade com um disco de densidade dupla. Se o disco no *unidade1* não é do mesmo tipo que o disco no *unidade2*, **diskcomp** exibe a seguinte mensagem:  
  ```
  Drive types or diskette types not compatible
  ```  
- Usando o **diskcomp** com redes e unidades redirecionadas

  **Diskcomp** não funciona em uma unidade de rede ou em unidades criadas com o **subst** comando. Se você tentar usar **diskcomp** com uma unidade de qualquer um desses tipos **diskcomp** exibe a seguinte mensagem de erro:  
  ```
  Invalid drive specification
  ```  
- Comparando um disco com uma cópia do original

  Quando você usa **diskcomp** com um disco que você fez usando **cópia**, **diskcomp** pode exibir uma mensagem semelhante à seguinte:  
  ```
  Compare error on 
  side 0, track 0
  ```  
  Esse tipo de erro pode ocorrer mesmo que os arquivos nos discos são idênticos. Embora **cópia** duplica as informações, ele não necessariamente colocá-lo no mesmo local no disco de destino.
- Compreendendo **diskcomp** códigos de saída

  A tabela a seguir explica cada código de saída.  

  |Código de Saída|Descrição|
  |---------|-----------|
  |0|Discos são iguais|
  |1|Foram encontradas diferenças|
  |3|Ocorreu erro de hardware|
  |4|Erro de inicialização|

  Para códigos de saída do processo que são retornados pelo **diskcomp**, você pode usar a variável de ambiente ERRORLEVEL sobre o **se** linha de comando em um arquivo em lotes.

## <a name="BKMK_examples"></a>Exemplos

Se o computador tem apenas uma unidade de disquete (por exemplo, a unidade A), e você deseja comparar dois discos, digite:
```
diskcomp a: a:
```
**Diskcomp** solicitará a inserção de cada disco, conforme necessário.

O exemplo a seguir ilustra como processar um **diskcomp** sair do código em um arquivo em lotes que usa a variável de ambiente ERRORLEVEL na **se** linha de comando:
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
echo "You just pressed CTRL+C" to stop the comparison 
goto exit 
:no_compare 
echo Disks are not the same 
goto exit 
:compare_ok 
echo The comparison was successful; the disks are the same 
goto exit 
:exit
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
