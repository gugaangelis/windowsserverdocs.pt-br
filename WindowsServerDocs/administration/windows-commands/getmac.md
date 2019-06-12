---
title: getmac
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1266b7368f1b073e00735a8d3362c75305d7c0f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438273"
---
# <a name="getmac"></a>getmac

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Retorna a mídia de acessar o endereço MAC (controle) e a lista de protocolos de rede associados a cada endereço para todas as placas de rede em cada computador, localmente ou em uma rede. 
## <a name="syntax"></a>Sintaxe
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
### <a name="parameters"></a>Parâmetros

|             Parâmetro              |                                                                                          Descrição                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s <computer>            |                                      Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                       |
|        /u <Domain>\\<User>         | Executa o comando com as permissões de conta de usuário especificada pelo usuário ou domínio \ usuário. O padrão é que as permissões do usuário no computador que está emitindo o comando de logon atual. |
|           /p <Password>            |                                                     Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.                                                     |
| /FO {tabela &#124; lista&#124; CSV} |                       Especifica o formato a ser usado para a saída da consulta. Os valores válidos são **tabela**, **lista**, e **CSV**. O formato padrão para a saída é **tabela**.                        |
|                /nh                 |                                             Suprime o cabeçalho de coluna na saída. Válido quando o **/fo** parâmetro for definido como **tabela** ou **CSV**.                                              |
|                 /v                 |                                                                    Especifica que a saída exiba informações detalhadas.                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>Comentários
**GETMAC** pode ser útil quando você deseja inserir o endereço MAC em um analisador de rede ou quando você precisa saber quais protocolos estão atualmente em uso em cada adaptador de rede em um computador.
## <a name="BKMK_Examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o **getmac** comando:
```
getmac /fo table /nh /v
```
```
getmac /s srvmain
```
```
getmac /s srvmain /u maindom\hiropln
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```
```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
