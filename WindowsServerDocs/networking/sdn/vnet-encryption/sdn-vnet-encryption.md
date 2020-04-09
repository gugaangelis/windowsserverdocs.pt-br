---
title: Criptografia de rede virtual
description: A criptografia de rede virtual permite a criptografia do tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como ' criptografia habilitada '.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: 63daea02ec00593504383ce071d3f9454a37956b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853569"
---
# <a name="virtual-network-encryption"></a>Criptografia de rede virtual

>Aplica-se a: Windows Server

A criptografia de rede virtual permite a criptografia do tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como ' criptografia habilitada '. Ela também utiliza o DTLS (Datagrama do protocolo TLS) na sub-rede virtual para criptografar os pacotes. O DTLS protege contra interceptações, falsificação e adulteração por qualquer pessoa com acesso à rede física.

A criptografia de rede virtual requer:
- Certificados de criptografia instalados em cada um dos hosts Hyper-V habilitados para SDN.
- Um objeto de credencial no controlador de rede que faz referência à impressão digital desse certificado.
- A configuração em cada uma das redes virtuais contém sub-redes que exigem Criptografia.

Depois de habilitar a criptografia em uma sub-rede, todo o tráfego de rede dentro dessa sub-rede é criptografado automaticamente, além de qualquer criptografia no nível do aplicativo que também possa ocorrer.  O tráfego que cruza entre sub-redes, mesmo se marcado como criptografado, é enviado sem criptografia automaticamente. Qualquer tráfego que cruzar o limite de rede virtual também é enviado sem criptografia.

>[!TIP]
>Se você precisar restringir os aplicativos para se comunicar apenas na sub-rede criptografada, poderá usar ACLs (listas de controle de acesso) somente para permitir a comunicação dentro da sub-rede atual. Para obter mais informações, consulte [usar ACLs (listas de controle de acesso) para gerenciar o fluxo de tráfego de rede do datacenter](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

### <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

[Configurar a criptografia para uma rede virtual](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

