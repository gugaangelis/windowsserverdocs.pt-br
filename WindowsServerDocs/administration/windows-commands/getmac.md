---
title: getmac
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fa37f558b454388b8451629ef96584ab8da0c3d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724977"
---
# <a name="getmac"></a>getmac

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Retorna o endereço MAC (controle de acesso à mídia) e a lista de protocolos de rede associados a cada endereço para todas as placas de rede em cada computador, seja localmente ou através de uma rede. 
## <a name="syntax"></a>Sintaxe
```
getmac[.exe][/s <computer> [/u <Domain\<User> [/p <Password>]]][/fo {TABLE | list | CSV}][/nh][/v]
```
#### <a name="parameters"></a>Parâmetros

|             Parâmetro              |                                                                                          Descrição                                                                                          |
|------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s<computer>            |                                      Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local.                                       |
|        /u<Domain>\\<User>         | Executa o comando com as permissões de conta do usuário especificado pelo usuário ou Domain\User. O padrão é as permissões do usuário conectado no momento no computador que emite o comando. |
|           /p<Password>            |                                                     Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                     |
| /FO {tabela &#124; lista&#124; CSV} |                       Especifica o formato a ser usado para a saída da consulta. Os valores válidos são **tabela**, **lista**e **CSV**. O formato padrão de saída é **tabela**.                        |
|                /NH                 |                                             Suprime o cabeçalho da coluna na saída. Válido quando o parâmetro **/fo** é definido como **Table** ou **CSV**.                                              |
|                 /v                 |                                                                    Especifica que a saída exibe informações detalhadas.                                                                     |
|                 /?                 |                                                                                                                                                                                               |

## <a name="remarks"></a>Comentários
o **GETMAC** pode ser útil quando você deseja inserir o endereço MAC em um analisador de rede ou quando você precisa saber quais protocolos estão atualmente em uso em cada adaptador de rede em um computador.
## <a name="examples"></a>Exemplos
Os exemplos a seguir mostram como você pode usar o comando **GETMAC** :
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
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
