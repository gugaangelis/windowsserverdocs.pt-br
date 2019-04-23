---
title: Executar o modo de réplica do WSUS
description: 'Tópico do Windows Server Update Service (WSUS) - como configurar o modo de réplica '
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d218cd6b-3b6b-4429-913b-31d412ce3356
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b4139354a3f0f7b1f1a97107d2f6b28db2b02c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878737"
---
# <a name="running-wsus-replica-mode"></a>Executar o modo de réplica do WSUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um servidor do WSUS em execução no modo de réplica herda as aprovações de atualização e os grupos de computadores criados em um servidor de administração. Em um cenário que usa o modo de réplica, você normalmente tem um servidor de administração e um ou mais servidores do WSUS de réplica subordinada espalham por toda a organização, com base no site ou topografia organizacional. Você pode aprova atualizações e cria grupos de computadores no servidor de administração, os servidores de modo de réplica, em seguida, irá espelhar. Servidores de modo de réplica podem ser configurados somente durante a instalação do WSUS e, se você tiver implementado nesse cenário, provavelmente é porque é importante em sua organização que aprovações de atualizações e grupos de computadores são gerenciados centralmente.

Se o servidor do WSUS estiver em execução no modo de réplica, você poderá executar os recursos de administração limitada somente no servidor, que será principalmente consistem em:

-   Adicionar e remover computadores de grupos de computadores. Associação de grupo do computador não é distribuída para os servidores de réplica, apenas o computador de grupos em si. Portanto, em um servidor de modo de réplica, você herdará os grupos de computadores que você criou no servidor de administração. No entanto, os grupos de computadores estará vazios. Em seguida, você deve atribuir o cliente de computadores que se conectam ao servidor de réplica para os grupos de computadores.

-   Configuração de um cronograma de sincronização

-   Especificando as configurações do servidor proxy

-   Especificando a origem de atualização. Isso pode ser um servidor diferente do servidor de administração

-   Visualização de atualizações disponíveis

-   Atualização de monitoramento, sincronização, status do computador e as configurações do WSUS no servidor

-   Executando o WSUS padrão todos os relatórios disponíveis em servidores de modo de réplica



