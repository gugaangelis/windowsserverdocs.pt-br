---
title: Área de Trabalho Remota – Conceder acesso ao seu computador de fora da rede
description: Saiba mais sobre as opções para acessar remotamente seu computador de fora da rede
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1b7568006ef9e32132d772702212c5fd78ec4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857419"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Área de Trabalho Remota – Conceder acesso ao seu computador de fora da rede

>Aplica-se a: Windows 10,  Windows Server 2016

Quando você se conecta a seu computador usando um cliente de Área de Trabalho Remota, você está criando uma conexão ponto a ponto. Ou seja, você precisa ter acesso direto ao computador (às vezes chamado de "host"). Se você precisar se conectar ao seu computador em uma rede externa à do seu computador, será necessário habilitar o acesso. Você tem duas opções: usar o encaminhamento de porta ou configurar uma VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Habilitar o encaminhamento de porta no roteador

O encaminhamento de porta simplesmente mapeia a porta no endereço IP do seu roteador (o IP público) para a porta e o endereço IP do computador que você deseja acessar. 

As etapas específicas para habilitar o encaminhamento de porta dependem de roteador em uso, portanto, você precisará pesquisar online para obter as instruções do seu roteador. ***Para acessar uma discussão geral sobre as etapas, confira o artigo [Como configurar o encaminhamento de porta em um roteador](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router) na wikiHow.

***Antes de mapear a porta, você precisará do seguinte:

- Endereço IP interno do computador: Procure em **Configurações > Rede e Internet > Status > Exibir as propriedades da rede**. Localize a configuração de rede com um status "Operacional" e obtenha o **endereço IPv4**.

   ![Configuração de rede adicional](../media/rdclient-operational-network.png)

- Seu endereço IP público (o IP do roteador). Há várias maneiras para encontrar essa informação – você pode pesquisar (no Bing ou o Google) por "meu IP" ou exibir as [propriedades da rede Wi-Fi](https://binged.it/2Gwob34) (para o Windows 10).
- Número da porta a mapear. Na maioria dos casos, a porta padrão usada por conexões de área de trabalho remota é a 3389.
- Acesso de administrador ao roteador.  

   >[!WARNING]
   > Você vai abrir seu computador para a internet – certifique-se de configurar uma senha forte para o seu computador.

Depois de mapear a porta, você poderá se conectar ao seu PC host de fora da rede local ao se conectar ao endereço IP público do seu roteador (o segundo marcador acima).

O endereço IP do roteador pode ser alterado, o seu provedor de serviços de internet (ISP) pode atribuir a você um novo IP a qualquer momento. Para evitar esse problema, considere o uso de DNS dinâmico, que permite que você se conecte ao computador usando um nome de domínio fácil de lembrar, em vez do endereço IP. Seu roteador atualiza automaticamente o serviço DDNS com seu novo endereço IP, caso seja alterado.

Na maioria dos roteadores, você pode definir qual IP de origem ou rede de origem pode usar o mapeamento de porta. Assim, se souber que só vai se conectar no trabalho, você pode adicionar o endereço IP da rede do seu trabalho, o que permite que você evite abrir a porta para toda a internet pública. Se o host dessa conexão usar um endereço IP dinâmico, defina a restrição de origem para permitir o acesso de todo o intervalo desse provedor específico.

Você também pode considerar a configuração de um [endereço IP estático](/windows-hardware/customize/mobile/mcsf/enable-static-ip) em seu computador para que o endereço IP interno não seja alterado. Se você fizer isso, então o encaminhamento de porta do roteador sempre indicará o endereço IP correto.


## <a name="use-a-vpn"></a>Usar uma VPN

Se você se conectar à sua rede local usando uma rede virtual privada (VPN), você não precisa abrir seu PC para a internet pública. Em vez disso, ao se conectar à VPN, seu cliente de área de trabalho remota age como se fosse parte da mesma rede e consegue acessar seu computador. Existem vários serviços disponíveis de VPN – você pode localizar e usar o que funcionar melhor para você.