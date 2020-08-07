---
title: dfsdiag testreferral
description: Artigo de referência para o comando Dfsdiag testreferral, que verifica referências a Sistema de Arquivos Distribuído (DFS).
ms.topic: article
ms.assetid: 877c60dc-e993-4bd5-87dd-e892e3f98a1a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21ed7a6dd56fda0a6185f3f5aaa2a15d9d6fb565
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891126"
---
# <a name="dfsdiag-testreferral"></a>dfsdiag testreferral

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Verifica as referências de Sistema de Arquivos Distribuído (DFS) executando os seguintes testes:

- Se você usar o parâmetro **DFSpath*** sem argumentos, o comando validará que a lista de referências inclui todos os domínios confiáveis.

- Se você especificar um domínio, o comando executará uma verificação de integridade dos controladores de domínio ( `dfsdiag /testdcs` ) e testará as associações de site e o cache de domínio do host local.

- Se você especificar um domínio e \SYSvol ou \NETLOGON, o comando executará as mesmas verificações de integridade do controlador de domínio, juntamente com a verificação de que a **TTL (vida útil)** das referências de SYSVOL ou Netlogon corresponde ao valor padrão de 900 segundos.

- Se você especificar uma raiz de namespace, o comando executará as mesmas verificações de integridade do controlador de domínio, juntamente com a execução de uma verificação de configuração do DFS ( `dfsdiag /testdfsconfig` ) e uma verificação de integridade do namespace ( `dfsdiag /testdfsintegrity` ).

- Se você especificar uma pasta DFS (link), o comando executará as mesmas verificações de integridade de raiz de namespace, juntamente com a validação da configuração de site para destinos de pasta (Dfsdiag/testsites) e validará a associação do site do host local.

## <a name="syntax"></a>Sintaxe

```
dfsdiag /testreferral /DFSpath:<DFS path to get referrals> [/full]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| DFSpath`<path to get referrals>` | Um dos seguintes pode ser feito:<ul><li>**Em branco:** Testa somente domínios confiáveis.</li><li>`\\Domain:`Testa somente as referências do controlador de domínio.</li><li>`\\Domain\SYSvol:`Testa somente referências de SYSvol.</li><li>`\\Domain\NETLOGON:`Testa somente as referências de NETLOGON.</li><li>`\\<domain or server>\<namespace root>:`Testa apenas as referências de raiz de namespace.</li><li>`\\<domain or server>\<namespace root>\<DFS folder>:`Testa apenas as referências da pasta DFS (link).</li></ul> |
| /full | Aplica-se somente às referências de domínio e raiz. Verifica a consistência das informações de associação do site entre o registro e os serviços de domínio do Active Directory (AD DS). |

## <a name="examples"></a>Exemplos

Para verificar as referências do Sistema de Arquivos Distribuído (DFS) em *contoso. com\MyNamespace*, digite:

```
dfsdiag /testreferral /DFSpath:\\contoso.com\MyNamespace
```

Para verificar as referências do Sistema de Arquivos Distribuído (DFS) em todos os domínios confiáveis, digite:

```
dfsdiag /testreferral /DFSpath:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Dfsdiag](dfsdiag.md)
