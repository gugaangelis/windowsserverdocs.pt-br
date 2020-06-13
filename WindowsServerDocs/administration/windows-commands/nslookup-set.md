---
title: nslookup set
description: Tópico de referência para o comando set do nslookup, que altera as definições de configuração que afetam a forma como as pesquisas se comportam.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1fe5b36d-e93e-468b-abca-43b0204b32d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 579334b3b6b0cd5e9373876144f46fa21d57c745
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721179"
---
# <a name="nslookup-set"></a>nslookup set

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera as definições de configuração que afetam o funcionamento das pesquisas.

## <a name="syntax"></a>Sintaxe

```
set all [class | d2 | debug | domain | port | querytype | recurse | retry | root | search | srchlist | timeout | type | vc] [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [nslookup set all](nslookup-set-all.md) | Lista todas as configurações atuais. |
| [nslookup set class](nslookup-set-class.md) | Altera a classe de consulta, que especifica o grupo de protocolos das informações. |
| [nslookup set d2](nslookup-set-d2.md) | Ativa ou desativa o modo de depuração detalhado. |
| [nslookup set debug](nslookup-set-debug.md) | Desativa completamente o modo de depuração. |
| [nslookup set domain](nslookup-set-domain.md) | Altera o nome de domínio padrão do DNS (sistema de nomes de domínio) para o nome especificado. |
| [nslookup set port](nslookup-set-port.md) | Altera a porta do servidor de nome DNS (sistema de nomes de domínio) TCP/UDP padrão para o valor especificado.
| [nslookup set querytype](nslookup-set-querytype.md) | Altera o tipo de registro de recurso para a consulta. |
| [nslookup set recurse](nslookup-set-recurse.md) | Informa ao servidor de nomes DNS (sistema de nomes de domínio) para consultar outros servidores se ele não encontrar nenhuma informação. |
| [nslookup set retry](nslookup-set-retry.md) | Define o número de repetições. |
| [nslookup set root](nslookup-set-root.md) | Altera o nome do servidor raiz usado para consultas. |
| [nslookup set search](nslookup-set-search.md) | Acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida. |
| [nslookup set srchlist](nslookup-set-srchlist.md) | Altera o nome de domínio padrão do DNS (sistema de nomes de domínio) e a lista de pesquisa. |
| [nslookup set timeout](nslookup-set-timeout.md) | Altera o número inicial de segundos para aguardar uma resposta a uma solicitação de pesquisa. |
| [nslookup set type](nslookup-set-type.md) | Altera o tipo de registro de recurso para a consulta. |
| [nslookup set vc](nslookup-set-vc.md) | Especifica se um circuito virtual deve ser usado ao enviar solicitações ao servidor. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
