---
title: prompt
description: Saiba como personalizar o prompt de comando.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5ef487ce9799c1f09660cdfcd6fba71336fc4d9a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442142"
---
# <a name="prompt"></a>prompt



Altera o prompt de comando Cmd.exe. Se usado sem parâmetros, **prompt** redefine o prompt de comando para a configuração padrão, que é a letra da unidade atual e o diretório seguido pelo maior que o símbolo ( **>** ).

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
prompt [<Text>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Text>|Especifica o texto e as informações que você deseja incluir no prompt de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Você pode personalizar o prompt de comando para exibir qualquer texto que você deseja, incluindo informações como o nome da pasta atual, a hora e data e o número de versão do Microsoft Windows.

A tabela a seguir lista as combinações de caracteres que podem ser incluídas em vez de ou além dele, um ou mais cadeias de caracteres na *texto* parâmetro. A lista inclui uma breve descrição do texto ou informações que cada combinação de caracteres adiciona ao prompt de comando.  

| Caractere |                                 Descrição                                 |
|-----------|-----------------------------------------------------------------------------|
|    $q     |                               = (sinal de igual)                                |
|    $$     |                               $ (cifrão)                               |
|    $t     |                                Hora atual                                 |
|    $d     |                                Data atual                                 |
|    $p     |                           Caminho e a unidade atual                            |
|    $v     |                           Número de versão do Windows                            |
|    $n     |                                Unidade atual                                |
|    $g     |                            > (sinal de maior que)                            |
|    $l     |                             < (sinal de menor que)                              |
|    $b     |                                                                             |
|    $_     |                               INSIRA E AVANÇO DE LINHA                                |
|    $e     |                         Código de escape ANSI (código 27)                          |
|    $h     | Backspace (para excluir um caractere que foi escrito para a linha de comando) |
|    $a     |                                & (e comercial)                                |
|    $c     |                            ((parêntese esquerdo)                             |
|    $f     |                            ) (parêntese direito)                            |
|    $s     |                                    Espaço                                    |

Quando as extensões de comando estão habilitadas (ou seja, o padrão) a **prompt** comando dá suporte a caracteres de formatação a seguir:  

|Caractere|Descrição|
|---------|-----------|
|$+|Sinal de adição de zero ou mais ( **+** ) caracteres, dependendo da profundidade do **pushd** pilha de diretório (um caractere para cada nível enviados por push).|
|$m|O nome remoto associado com a letra da unidade atual ou a cadeia de caracteres vazia se a unidade atual não é uma unidade de rede.|

Se você incluir a **$p** caracteres no parâmetro de texto, o disco será lido depois que você insere cada comando (para determinar a unidade atual e o caminho). Isso pode levar mais tempo, especialmente para unidades de disquete.

## <a name="BKMK_examples"></a>Exemplos

Para definir um prompt de comando de duas linhas com a data e hora atual na primeira linha e o sinal maior que na próxima linha, digite:
```
prompt $d$s$s$t$_$g 
```
O prompt é alterado da seguinte maneira, em que a data e hora são atuais:
```
Fri 06/01/2007  13:53:28.91
>
```
Para definir o prompt de comando para exibir como uma seta (`-->`), digite:
```
prompt --$g
```
Para alterar manualmente o prompt de comando para a configuração padrão (a unidade atual e o caminho seguido o sinal maior que), digite:
```
prompt $p$g
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)