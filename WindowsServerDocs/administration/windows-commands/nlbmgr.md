---
title: nlbmgr
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 89cb8590-b7cf-4a27-89fa-0fa62ea1a1ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59ec2ad6b4614f89f9c1c3cbda97d5283a2374bd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723747"
---
# <a name="nlbmgr"></a>nlbmgr

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usando o Gerenciador de balanceamento de carga de rede, você pode configurar e gerenciar seus clusters de balanceamento de carga de rede e todos os hosts de cluster de um único computador, e também pode replicar a configuração de cluster para outros hosts. Você pode iniciar o Gerenciador de balanceamento de carga de rede na linha de comando usando o comando **Nlbmgr. exe**, que é instalado na pasta **systemroot\System32**
## <a name="syntax"></a>Sintaxe
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
#### <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                                                                                                                                                Descrição                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   Exibe a ajuda no prompt de comando.                                                                                                                                                                                    |
|         /noping         | Impede que o Gerenciador de balanceamento de carga de rede execute ping nos hosts antes de tentar contatá-los por meio de Instrumentação de Gerenciamento do Windows (WMI). Use esta opção se você desabilitou o protocolo ICMP em todos os adaptadores de rede disponíveis. Se o Gerenciador de balanceamento de carga de rede tentar contatar um host que não está disponível, você passará por um atraso ao usar essa opção. |
|  /hostlist<filename>   |                                                                                                                                                                Carrega os hosts especificados em filename no Gerenciador de balanceamento de carga de rede.                                                                                                                                                                 |
| /autorefresh<interval> |                                                                                                          Faz com que o Gerenciador de balanceamento de carga de rede Atualize suas <interval> informações de host e de cluster a cada segundos. Se nenhum intervalo for especificado, as informações serão atualizadas a cada 60 segundos.                                                                                                          |
|           /?            |                                                                                                                                                                                   Exibe a ajuda no prompt de comando.                                                                                                                                                                                    |

## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

