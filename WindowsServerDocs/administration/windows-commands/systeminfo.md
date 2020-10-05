---
title: systeminfo
description: Artigo de referência para o comando systeminfo, que exibe informações detalhadas de configuração sobre um computador e seu sistema operacional, incluindo configuração do sistema operacional, informações de segurança, ID do produto e propriedades de hardware (como RAM, espaço em disco e placas de rede).
ms.topic: reference
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9c104d3114eae6170e3fbbd60ab718fa0013ea85
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718193"
---
# <a name="systeminfo"></a>systeminfo

Exibe informações de configuração detalhadas sobre um computador e seu sistema operacional, incluindo configuração do sistema operacional, informações de segurança, ID do produto e propriedades de hardware (como RAM, espaço em disco e placas de rede).

## <a name="syntax"></a>Sintaxe

```
systeminfo [/s <computer> [/u <domain>\<username> [/p <password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. |
| /u `<domain>\<username>` | Executa o comando com as permissões de conta da conta de usuário especificada. Se **/u** não for especificado, esse comando usará as permissões do usuário que está conectado no momento ao computador que está emitindo o comando. |
| /p `<password>` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| /Fo `<format>` | Especifica o formato de saída com um dos seguintes valores:<ul><li>**Tabela** -exibe a saída em uma tabela.</li><li>**Lista** -exibe a saída em uma lista.</li><li>**CSV** -exibe a saída em formato de valores separados por vírgulas (. csv).</li></ul> |
| /NH | Suprime cabeçalhos de coluna na saída. Válido quando o parâmetro **/fo** é definido como Table ou CSV. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para exibir as informações de configuração de um computador chamado *srvmain*, digite:

```
systeminfo /s srvmain
```

Para exibir remotamente as informações de configuração de um computador chamado *Srvmain2* que está localizado no domínio *Maindom* , digite:

```
systeminfo /s srvmain2 /u maindom\hiropln
```

Para exibir remotamente as informações de configuração (em formato de lista) para um computador chamado *Srvmain2* que está localizado no domínio *Maindom* , digite:

```
systeminfo /s srvmain2 /u maindom\hiropln /p p@ssW23 /fo list
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
