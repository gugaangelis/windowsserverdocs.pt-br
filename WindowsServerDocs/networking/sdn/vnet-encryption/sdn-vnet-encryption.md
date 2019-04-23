---
title: Criptografia de rede virtual
description: Criptografia de rede virtual permite a criptografia de tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como 'A criptografia habilitada.'
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: f2f50ae3146854e2ef6081b0c400a474b53dcf66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851077"
---
# <a name="virtual-network-encryption"></a>Criptografia de rede virtual

>Aplica-se a: Windows Server

Criptografia de rede virtual permite a criptografia de tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como 'A criptografia habilitada.' Ele também utiliza datagrama Transport Layer Security (DTLS) na sub-rede virtual para criptografar pacotes. O DTLS oferece proteção contra interceptações, falsificação e falsificação por qualquer pessoa com acesso à rede física.

Requer criptografia de rede virtual:
- Certificados de criptografia instalados em cada um dos hosts Hyper-V SDN habilitado.
- Um objeto de credencial no controlador de rede, fazendo referência a impressão digital do certificado.
- A configuração em cada uma das redes virtuais contém sub-redes que exigem criptografia.

Depois que você habilitar a criptografia em uma sub-rede, todo o tráfego de rede dentro dessa sub-rede é criptografado automaticamente, além de qualquer criptografia no nível do aplicativo que pode ocorrer também.  Tráfego que atravessa entre sub-redes, mesmo se marcado como criptografadas, é automaticamente enviado descriptografado. Qualquer tráfego que cruza o limite de rede virtual também obtém enviado descriptografado.

>[!TIP]
>Se você deve restringir os aplicativos se comuniquem apenas na sub-rede criptografada, você pode usar listas de controle de acesso (ACLs) apenas para permitir a comunicação dentro da sub-rede atual. Para obter mais informações, consulte [uso Access Control Lists (ACLs) para gerenciar o data center rede tráfego fluir](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

### <a name="next-steps"></a>Próximas etapas

[Configurar a criptografia para uma rede virtual](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

