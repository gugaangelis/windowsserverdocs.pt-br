---
title: nlbmgr
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 757b218ad3a88cc10c4d1bcfed15a83bfd34cc74
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437029"
---
# <a name="nlbmgr"></a>nlbmgr

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Usando o Gerenciador de balanceamento de carga de rede, você pode configurar e gerenciar seus clusters de balanceamento de carga de rede e todos os hosts de cluster de um único computador, e você também pode replicar a configuração do cluster para outros hosts. Você pode iniciar o Gerenciador de balanceamento de carga de rede da linha de comando usando o comando **nlbmgr.exe**, que é instalado no **sistema\System32** pasta.
## <a name="syntax"></a>Sintaxe
```
nlbmgr [/help] [/noping] [/hostlist <filename>] [/autorefresh <interval>]
```
### <a name="parameters"></a>Parâmetros

|        Parâmetro        |                                                                                                                                                                                                Descrição                                                                                                                                                                                                |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /help          |                                                                                                                                                                                   Exibe a ajuda no prompt de comando.                                                                                                                                                                                    |
|         /noping         | Impede que o Gerenciador de balanceamento de carga de rede executar o ping para os hosts antes de tentar contatá-lo por meio do Windows Management Instrumentation (WMI). Use esta opção se você tiver desabilitado a mensagem de protocolo ICMP (Internet Control) em todos os adaptadores de rede disponíveis. Se o Gerenciador de balanceamento de carga de rede tenta contatar um host que não está disponível, você terá um atraso ao usar essa opção. |
|  /hostlist <filename>   |                                                                                                                                                                Carrega os hosts especificados no nome do arquivo no Gerenciador de balanceamento de carga de rede.                                                                                                                                                                 |
| /autorefresh <interval> |                                                                                                          Faz com que o Gerenciador de balanceamento de carga de rede atualizar suas informações de host e cluster cada <interval> segundos. Se nenhum intervalo for especificado, as informações são atualizadas a cada 60 segundos.                                                                                                          |
|           /?            |                                                                                                                                                                                   Exibe a ajuda no prompt de comando.                                                                                                                                                                                    |

## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

