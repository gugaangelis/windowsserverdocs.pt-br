---
title: prompt
description: Saiba como personalizar o prompt de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 4b3aedc341dfcee094e20f43e39f34dc6fd08109
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722790"
---
# <a name="prompt"></a>prompt



Altera o prompt de comando cmd. exe. Se usado sem parâmetros, **prompt** redefine o prompt de comando para a configuração padrão, que é a letra da unidade e o diretório atuais seguidos pelo símbolo**>** maior que ().



## <a name="syntax"></a>Sintaxe

```
prompt [<Text>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> de texto|Especifica o texto e as informações que você deseja incluir no prompt de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Você pode personalizar o prompt de comando para exibir qualquer texto desejado, incluindo informações como o nome do diretório atual, a hora e a data e o número de versão do Microsoft Windows.

A tabela a seguir lista as combinações de caracteres que você pode incluir em vez de, ou além, uma ou mais cadeias de caracteres no parâmetro de *texto* . A lista inclui uma breve descrição do texto ou das informações que cada combinação de caracteres adiciona ao seu prompt de comando.  

| Caractere |                                 Descrição                                 |
|-----------|-----------------------------------------------------------------------------|
|    $q     |                               = (sinal de igual)                                |
|    $$     |                               $ (cifrão)                               |
|    $t     |                                Hora atual                                 |
|    $d     |                                Data atual                                 |
|    $p     |                           Unidade e caminho atuais                            |
|    $v     |                           Número de versão do Windows                            |
|    $n     |                                Unidade atual                                |
|    $g     |                            > (sinal de maior que)                            |
|    $l     |                             < (sinal de menor que)                              |
|    $b     |                              \|(símbolo de pipe)                               |
|    $_     |                               INSERIR ALIMENTAÇÃO DE DISCAGEM                                |
|    $e     |                         Código de escape ANSI (código 27)                          |
|    $h     | Backspace (para excluir um caractere que foi gravado na linha de comando) |
|    $a     |                                & (e comercial)                                |
|    $c     |                            (parêntese esquerdo)                             |
|    $f     |                            ) (parêntese direito)                            |
|    $s     |                                    espaço                                    |

Quando as extensões de comando são habilitadas (ou seja, o padrão), o comando de **prompt** dá suporte aos seguintes caracteres de formatação:  

|Caractere|Descrição|
|---------|-----------|
|$+|Zero ou mais caracteres de sinal**+** de adição (), dependendo da profundidade da pilha de diretórios **PUSHD** (um caractere para cada nível enviado).|
|$m|O nome remoto associado à letra da unidade atual ou à cadeia de caracteres vazia se a unidade atual não for uma unidade de rede.|

Se você incluir o caractere de **$p** no parâmetro de texto, o disco será lido depois que você inserir cada comando (para determinar a unidade e o caminho atuais). Isso pode levar mais tempo, especialmente para unidades de disquete.

## <a name="examples"></a><a name="BKMK_examples"></a>Disso

Para definir um prompt de comando de duas linhas com a hora e a data atuais na primeira linha e o sinal de maior que na próxima linha, digite:
```
prompt $d$s$s$t$_$g 
```
O prompt é alterado da seguinte maneira, em que a data e a hora são atuais:
```
Fri 06/01/2007  13:53:28.91
>
```
Para definir o prompt de comando para exibir como uma seta`-->`(), digite:
```
prompt --$g
```
Para alterar manualmente o prompt de comando para a configuração padrão (a unidade e o caminho atuais seguidos pelo sinal de maior que), digite:
```
prompt $p$g
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
