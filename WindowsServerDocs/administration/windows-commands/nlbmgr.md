---
title: nlbmgr
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2843e303b296beca24132b62073b6776a343544b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373163"
---
# <a name="nlbmgr"></a>nlbmgr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usando o Gerenciador de balanceamento de carga de rede, você pode configurar e gerenciar seus clusters de balanceamento de carga de rede e todos os hosts de cluster de um único computador, e também pode replicar a configuração de cluster para outros hosts. Você pode iniciar o Gerenciador de balanceamento de carga de rede na linha de comando usando o comando **Nlbmgr. exe**, que é instalado na pasta **systemroot\System32**
## <a name="syntax"></a>Sintaxe
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                                                                                                                                                Descrição                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   Exibe a ajuda no prompt de comando.                                                                                                                                                                                    |
|         /noping         | Impede que o Gerenciador de balanceamento de carga de rede execute ping nos hosts antes de tentar contatá-los por meio de Instrumentação de Gerenciamento do Windows (WMI). Use esta opção se você desabilitou o protocolo ICMP em todos os adaptadores de rede disponíveis. Se o Gerenciador de balanceamento de carga de rede tentar contatar um host que não está disponível, você passará por um atraso ao usar essa opção. |
|  /hostlist <filename>   |                                                                                                                                                                Carrega os hosts especificados em filename no Gerenciador de balanceamento de carga de rede.                                                                                                                                                                 |
| /AutoRefresh <interval> |                                                                                                          Faz com que o Gerenciador de balanceamento de carga de rede Atualize suas informações de host e de cluster a cada <interval> segundos. Se nenhum intervalo for especificado, as informações serão atualizadas a cada 60 segundos.                                                                                                          |
|           /?            |                                                                                                                                                                                   Exibe a ajuda no prompt de comando.                                                                                                                                                                                    |

## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

