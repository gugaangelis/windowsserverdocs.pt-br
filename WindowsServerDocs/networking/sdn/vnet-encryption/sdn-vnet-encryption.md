---
title: Criptografia de rede virtual
description: Este tópico fornece informações sobre criptografia de rede Virtual para Software definido de rede no Windows Server
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 425cc1cff8c7241aeec7764b60c89b4586fd323b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-network-encryption"></a>Criptografia de rede virtual

>Aplica-se a: Windows Server

Criptografia de rede virtual oferece a possibilidade do tráfego de rede virtual a ser criptografado entre máquinas virtuais que se comunicam entre si dentro de sub-redes são marcados como "Criptografia ativada".

Esse recurso utiliza datagrama Transport Layer Security (DTLS) na sub-rede virtual para criptografar os pacotes.  DTLS oferece proteção contra interceptações, falsificação e falsificado por qualquer pessoa com acesso à rede física.

Requries de criptografia de rede virtual um certificado de criptografia a ser instalado em cada um do SDN habilitado hosts Hyper-V, um objeto de credenciais no controlador de rede fazendo referência a impressão digital do certificado e configuração em cada uma das redes virtuais que contêm sub-redes exigir criptografia.

Depois que a criptografia está habilitada em uma sub-rede, todo o tráfego de rede dentro daquela sub-rede é criptografado automaticamente.  Isso será além de qualquer criptografia no nível do aplicativo que também pode ocupar o lugar.  O tráfego cruza entre sub-redes, mesmo se ambas as sub-redes são marcadas como criptografada será automaticamente enviado não criptografado.  Todo o tráfego que atravessa o limite de rede virtual também será enviado descriptografado.

Para obter informações sobre como configurar a criptografia de rede Virtual, consulte [configurar criptografia para uma rede Virtual](sdn-config-vnet-encryption.md).

Se você deve restringir aplicativos se comuniquem somente na sub-rede criptografada.  Você pode usar listas de controle de acesso (ACLs) para permitir apenas a comunicação dentro da sub-rede atual.  

Para obter informações sobre como configurar a lista de controle de acesso, consulte [uso controle listas de acesso (ACLs) para gerenciar Datacenter rede tráfego fluir](../manage/use-acls-for-traffic-flow.md).