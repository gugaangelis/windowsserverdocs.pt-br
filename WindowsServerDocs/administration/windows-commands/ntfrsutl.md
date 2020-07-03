---
title: ntfrsutl
description: Artigo de referência para o comando NTFRSUTL, que despeja as tabelas internas, thread e informações de memória para o serviço de replicação de arquivo NT (NTFRS).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7721a19-5a87-4ab6-b816-65d2da2c811f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 56aefc1277f67dc6a06ba4686c26f81592afc2f3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925332"
---
# <a name="ntfrsutl"></a>ntfrsutl

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Despeja as tabelas internas, thread e informações de memória para o serviço de replicação de arquivo NT (NTFRS) dos servidores locais e remotos. A configuração de recuperação para o NTFRS no Gerenciador de controle de serviço (SCM) pode ser essencial para localizar e manter eventos de log importantes no computador. Essa ferramenta fornece um método conveniente para revisar essas configurações.

## <a name="syntax"></a>Sintaxe

```
ntfrsutl[idtable|configtable|inlog|outlog][<computer>]
ntfrsutl[memory|threads|stage][<computer>]
ntfrsutl ds[<computer>]
ntfrsutl [sets][<computer>]
ntfrsutl [version][<computer>]
ntfrsutl poll[/quickly[=[<n>]]][/slowly[=[<n>]]][/now][<computer>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| idtable | Especifica a tabela de ID. |
| ConfigTable | Especifica a tabela de configuração do FRS. |
| inlog | Especifica o log de entrada. |
| outlog | Especifica o log de saída. |
| `<computer>` | Especifica o computador. |
| memória | Especifica o uso de memória. |
| threads | Especifica o uso de memória. |
| preparar | Especifica o uso de memória. |
| AD | Lista o modo de exibição do serviço NTFRS do DS. |
| conjuntos | Especifica os conjuntos de réplica ativas. |
| version | Especifica a API e as versões do serviço NTFRS. |
| sondagem | Especifica os intervalos de sondagem atuais.<ul><li>`/quickly`-Sonda rapidamente até recuperar uma configuração estável.</li><li>`/quickly=`-Sonda rapidamente a cada número padrão de minutos.</li><li>`/quickly=<n>`-Sonda rapidamente a cada *n* minutos.</li><li>`/slowly`-Sonda lentamente até recuperar uma configuração estável.</li><li>`/slowly=`-Sonda lentamente cada número padrão de minutos.</li><li>`/slowly=<n>`-Pesquisas lentamente a cada *n* minutos.</li><li>`/now`-Sondas agora.</li></ul>|
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para determinar o intervalo de sondagem para a replicação de arquivo, digite:

```
C:\Program Files\SupportTools>ntfrsutl poll wrkstn-1
```

Para determinar a versão atual da API (interface de programa de aplicativo) do NTFRS, digite:

```
C:\Program Files\SupportTools>ntfrsutl version
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
