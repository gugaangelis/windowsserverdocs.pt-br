---
title: driverquery
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 993c05a930a7702880af23fcfa7c19a43aa8b22b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720828"
---
# <a name="driverquery"></a>driverquery



Permite que um administrador exiba uma lista de drivers de dispositivo instalados e suas propriedades. Se usado sem parâmetros, **DRIVERQUERY** é executado no computador local.



## <a name="syntax"></a>Sintaxe

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

### <a name="parameters"></a>Parâmetros

|         Parâmetro         |                                                                                                                                         Descrição                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<sistema>        |                                                                                      Especifica o nome ou o endereço IP de um computador remoto. Não use barras invertidas. O padrão é o computador local.                                                                                       |
| /u [\<domínio>\]<Username> | Executa o comando com as credenciais da conta de usuário, conforme especificado pelo *usuário* ou *domínio*\*usuário<em>. Por padrão, \* \*/s</em> \* usa as credenciais do usuário que está conectado no momento no computador que está emitindo o comando. **/u** não pode ser usado a menos que **/s** seja especificado. |
|      /p \<senha>       |                                                                           Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . **/p** não pode ser usado a menos que **/u** seja especificado.                                                                            |
|        /FO {tabela         |                                                                                                                                             list                                                                                                                                             |
|            /NH            |                                                                                      Omite a linha de cabeçalho das informações de driver exibidas. Não é válido se o parâmetro **/fo** estiver definido como **list**.                                                                                      |
|            /v             |                                                                                                               Exibe a saída detalhada. **/v** não é válido para drivers assinados.                                                                                                               |
|            /si            |                                                                                                                          Fornece informações sobre drivers assinados.                                                                                                                          |
|            /?             |                                                                                                                             Exibe a ajuda no prompt de comando.                                                                                                                             |

## <a name="examples"></a>Exemplos

Para exibir uma lista de drivers de dispositivo instalados no computador local, digite:
```
driverquery 
```
Para exibir a saída em um formato CSV (valores separados por vírgula), digite:
```
driverquery /fo csv 
```
Para ocultar a linha de cabeçalho na saída, digite:
```
driverquery /nh 
```
Para usar o comando **DRIVERQUERY** em um servidor remoto chamado **Server1** usando suas credenciais atuais no computador local, digite:
```
driverquery /s server1
```
Para usar o comando **DRIVERQUERY** em um servidor remoto chamado **Server1** usando as credenciais para **user1** no domínio **maindom**, digite:
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)