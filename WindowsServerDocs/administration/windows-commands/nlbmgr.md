---
title: nlbmgr
description: Artigo de referência para o comando Nlbmgr, que ajuda a configurar e gerenciar seus clusters de balanceamento de carga de rede e todos os hosts de cluster de um único computador, usando o Gerenciador de balanceamento de carga de rede.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 882d3540897214db2f3fa9d04a6f11fc76a76502
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935529"
---
# <a name="nlbmgr"></a>nlbmgr

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure e gerencie seus clusters de balanceamento de carga de rede e todos os hosts de cluster de um único computador, usando o Gerenciador de balanceamento de carga de rede. Você também pode usar esse comando para replicar a configuração do cluster para outros hosts.

Você pode iniciar o Gerenciador de balanceamento de carga de rede na linha de comando usando o comando **nlbmgr.exe**, que é instalado na pasta **systemroot\System32**

## <a name="syntax"></a>Sintaxe

```
nlbmgr [/noping][/hostlist <filename>][/autorefresh <interval>][/help | /?]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /noping | Impede que o Gerenciador de balanceamento de carga de rede execute ping nos hosts antes de tentar contatá-los por meio de Instrumentação de Gerenciamento do Windows (WMI). Use esta opção se você desabilitou o protocolo ICMP em todos os adaptadores de rede disponíveis. Se o Gerenciador de balanceamento de carga de rede tentar contatar um host que não está disponível, você enfrentará um atraso ao usar essa opção. |
| /hostlist`<filename>` | Carrega os hosts especificados em filename no Gerenciador de balanceamento de carga de rede. |
| /autorefresh`<interval>` | Faz com que o Gerenciador de balanceamento de carga de rede Atualize suas informações de host e de cluster a cada `<interval>` segundos. Se nenhum intervalo for especificado, as informações serão atualizadas a cada 60 segundos. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência de cmdlets do NetworkLoadBalancingClusters](https://docs.microsoft.com/powershell/module/networkloadbalancingclusters)
