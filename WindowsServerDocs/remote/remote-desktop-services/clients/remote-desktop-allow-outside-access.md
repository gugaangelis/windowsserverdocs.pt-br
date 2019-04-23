---
title: Área de trabalho remota - permitir o acesso ao seu PC de fora da sua rede
description: Saiba mais sobre as opções para acessar remotamente seu PC de fora da rede do computador
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888147"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Área de trabalho remota - permitir o acesso ao seu PC de fora da rede do seu PC

>Aplica-se a: Windows 10,  Windows Server 2016

Quando você se conecta a seu computador usando um cliente de área de trabalho remota, você está criando uma conexão ponto a ponto. Isso significa que você precisa ter acesso direto ao PC (às vezes chamado de "host"). Se você precisar se conectar ao seu PC de fora da rede do que seu computador está em execução no, você precisa habilitar o acesso. Você tem duas opções: usar o encaminhamento de porta ou configurar uma VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Habilitar o encaminhamento de porta no roteador

Simplesmente, o encaminhamento de porta mapeia a porta no endereço IP do seu roteador (o IP público) para a porta e endereço IP do PC no qual você deseja acessar. 

As etapas específicas para habilitar o encaminhamento de porta dependem de roteador que você está usando, portanto, você precisará pesquisar online para obter instruções do seu roteador. Para obter uma discussão geral sobre as etapas, confira [wikiHow para definir o encaminhamento de porta em um roteador](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Antes de mapear a porta, você precisará do seguinte:

- Endereço IP interno do PC: Examinar **Configurações > rede e Internet > Status > exibir as propriedades da rede**. Localizar a configuração de rede com um status "Operational" e, em seguida, obter o **endereço IPv4**.

   ![Configuração de rede operacional](../media/rdclient-operational-network.png)

- Seu endereço IP público (IP do roteador). Há várias maneiras para encontrá-los – você pode pesquisar (no Bing ou o Google) "Meu IP" ou exibir o [propriedades da rede Wi-Fi](https://binged.it/2Gwob34) (para Windows 10).
- Número da porta que está sendo mapeado. Na maioria dos casos, isso é 3389 - que é a porta padrão usada por conexões de área de trabalho remota.
- Acesso de administrador para seu roteador.  

   >[!WARNING]
   > Você está abrindo seu PC até a internet - Verifique se que você tem uma senha forte definida para o seu PC.

Depois de mapear a porta, você poderá se conectar ao seu PC host de fora da rede local ao se conectar ao endereço IP do roteador (o segundo marcador acima).

Endereço IP do roteador pode alterar - se seu provedor de serviços de internet (ISP) pode atribuir a você um novo IP a qualquer momento. Para evitar esse problema, considere o uso de DNS dinâmico – Isso permite que você se conectar ao computador usando uma forma fácil de lembrar o nome de domínio, em vez do endereço IP. Seu roteador atualiza automaticamente o serviço DDNS com seu novo endereço IP, deve alterar.

Com a maioria dos roteadores, você pode definir qual IP de origem ou a rede de origem pode usar o mapeamento de porta. Assim, se você souber que só você vai conectar-se de trabalho, você pode adicionar o endereço IP para a rede de trabalho - que permite que você evite abrindo a porta em toda a internet pública. Se o host que você está usando para conectar-se usar endereço IP dinâmico, defina a restrição de origem para permitir o acesso de todo o intervalo de provedor específico.

Você também pode considerar como configurar uma [endereço IP estático](/windows-hardware/customize/mobile/mcsf/enable-static-ip) em seu computador para que o endereço IP interno não é alterado. Se você fizer isso que, em seguida, o roteador encaminhamento de porta será sempre apontar para o endereço IP correto.


## <a name="use-a-vpn"></a>Usar uma VPN

Se você se conectar à sua rede local usando uma rede virtual privada (VPN), você não precisa abrir seu PC para a internet pública. Em vez disso, quando você se conectar à VPN, seu cliente de área de trabalho remota age como ele faz parte da mesma rede e ser capaz de acessar seu PC. Um número de serviços VPN estão disponíveis – você pode localizar e usar o que funciona melhor para você.