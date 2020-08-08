---
title: Etapa 3 verificar a implantação de acesso remoto (VPN)
description: Este tópico faz parte do guia adicionar o DirectAccess a uma implantação de VPN (acesso remoto) existente para o Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9da14f076f177e7e819c1529f9de647b5dde0500
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969213"
---
# <a name="step-3-verify-the-remote-access-vpn-deployment"></a>Etapa 3 verificar a implantação de acesso remoto (VPN)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico descreve como verificar se você configurou corretamente sua implantação do DirectAccess.

### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Para verificar o acesso aos recursos internos por meio do DirectAccess

1.  Conecte um computador cliente do DirectAccess à rede corporativa e obtenha a política de grupo.

2.  Clique no ícone **Conexões de rede** na área de notificação para acessar o Gerenciador de Mídia do DA.

3.  Clique em **Conexão do DirectAccess** e verá o status **Conectado Localmente**.

4.  Conecte o computador cliente à rede externa e tentar acessar os recursos internos.

    Você deve conseguir acessar todos os recursos corporativos.



