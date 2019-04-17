---
title: Área de trabalho remota - permitir o acesso ao PC de fora da rede
description: Saiba mais sobre as opções para acessar remotamente um PC de fora da rede do PC
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: d77c362d9d06b70ad0747002ed8853a39e05b7ff
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708654"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Área de trabalho remota - permitir o acesso ao PC de fora da rede do seu PC

>Aplica-se a: Windows 10, Windows Server 2016

Quando você se conecta ao PC usando um cliente de área de trabalho remota, você está criando uma conexão de ponto a ponto. Isso significa que você precisa ter acesso direto ao PC (às vezes chamado de "host"). Se você precisar se conectar ao seu PC de fora da rede que PC está sendo executado, você precisará habilitar esse acesso. Você tem duas opções: usar o encaminhamento de porta ou configurar um VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Habilitar encaminhamento de porta no roteador

Encaminhamento de porta simplesmente mapeia a porta no endereço IP do seu roteador (seu IP público) para a porta e endereço IP do PC você deseja acessar. 

As etapas específicas para habilitar o encaminhamento de porta dependem do roteador que você está usando, portanto você precisará pesquisar online para obter instruções do seu roteador. Para obter uma discussão geral das etapas, confira [wikiHow para definir backup porta encaminhamento em um roteador](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Antes de você mapear a porta será necessário o seguinte:

- Endereço IP interno de PC: procure no **Configurações > Internet & rede > Status > exibir as propriedades da rede**. Encontre a configuração de rede com um status de "Operacional" e, em seguida, obter o **endereço IPv4**.

   ![Configuração de rede operacionais](../media/rdclient-operational-network.png)

- Seu endereço IP público (IP do roteador). Há muitas maneiras de se localizar este - você pode procurar "Meu IP" (em Bing ou o Google) ou exibir as [Propriedades de rede Wi-Fi](https://binged.it/2Gwob34) (para Windows 10).
- Número da porta que está sendo mapeado. Na maioria dos casos, isso é 3389 - que é a porta padrão usada por conexões de área de trabalho remota.
- Acesso de administrador para o seu roteador.  

   >[!WARNING]
   > Você está abrindo seu PC até internet - Verifique se que você tem uma senha forte definido para o seu PC.

Depois de mapear a porta, você poderá se conectar ao seu PC host de fora da rede local conectando-se ao endereço IP público do seu roteador (o segundo marcador acima).

Endereço IP do roteador pode alterar - seu provedor de serviços de internet (ISP) pode atribuir a você um novo IP a qualquer momento. Para evitar a execução para esse problema, considere o uso de DNS dinâmico - isso lhe permite conectar-se ao PC usando uma fácil de lembrar de nome de domínio, em vez do endereço IP. Seu roteador atualiza automaticamente o serviço DDNS com seu novo endereço IP, deve alterar.

Com a maioria dos roteadores, é possível definir quais IP de origem ou rede de origem pode usar o mapeamento de porta. Assim, se você souber que você vai apenas se conectam de trabalho, você pode adicionar o endereço IP para a rede de trabalho - permite que você evite abrir a porta em toda a internet pública. Se o host que você está usando para conectar usa o endereço IP dinâmico, defina a restrição de origem para permitir o acesso de todo o intervalo de um provedor específico.

Você também pode considerar configurar um [endereço IP estático](/windows-hardware/customize/mobile/mcsf/enable-static-ip) no seu PC, portanto não se altera o endereço IP interno. Se você fizer isso que, em seguida, o roteador encaminhamento de porta sempre apontará para o endereço IP correto.


## <a name="use-a-vpn"></a>Usar uma VPN

Se você se conectar à sua rede local usando uma rede virtual privada (VPN), você não precisa abrir o seu PC à internet pública. Em vez disso, quando você se conectar à VPN, seu cliente de área de trabalho remota atua como ele é parte da mesma rede e poderão acessar seu PC. Há um número de serviços VPN - você pode encontrar e usar aquele que funciona melhor para você.