---
title: alterar
description: Tópico de comandos do Windows para alteração, que altera Host da Sessão da Área de Trabalho Remota configurações do servidor para logons, mapeamentos de porta COM e modo de instalação.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90012116-0fb3-4f34-a819-cf4d4b4f8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5d91f8d0941fc96e776c761b9c7037e58588df8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847949"
---
# <a name="change"></a>alterar

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera Host da Sessão da Área de Trabalho Remota configurações do servidor para logons, mapeamentos de porta COM e modo de instalação.

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe

 ```
 change logon
 change port
 change user
 ```
 
 ### <a name="parameters"></a>Parâmetros
 
 |            Parâmetro            |                                                   Descrição                                                   |
 |---------------------------------|-----------------------------------------------------------------------------------------------------------------|
 | [change logon](change-logon.md) | Habilita ou desabilita os logons de sessões de cliente em um servidor de host de sessão de área de trabalho remota ou exibe o status de logon atual. |
 |  [change port](change-port.md)  |                Lista ou altera os mapeamentos de porta COM para que sejam compatíveis com os aplicativos do MS-DOS.                |
 |  [change user](change-user.md)  |                            altera o modo de instalação do servidor de host da sessão da área de trabalho remota.                             |
 
 ## <a name="additional-references"></a>Referências adicionais
 - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
 [referência de comando serviços de área de trabalho remota (serviços de terminal)](remote-desktop-services-terminal-services-command-reference.md)
