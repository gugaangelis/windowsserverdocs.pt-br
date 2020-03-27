---
title: Balanceamento de carga do servidor proxy NPS
description: Você pode usar este tópico para aprender sobre recursos e funcionalidades de VPN do Windows Server 2016 e do Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 10a33494365f5a10923dd9ce46c3575675099b27
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315969"
---
# <a name="nps-proxy-server-load-balancing"></a>Balanceamento de carga do servidor proxy NPS

Aplica-se a: Windows Server 2016

Clientes do serviço RADIUS (RADIUS), que são servidores de acesso à rede, como servidores de rede virtual privada (VPN) e pontos de acesso sem fio, criam solicitações de conexão e as enviam para servidores RADIUS, como o NPS. Em alguns casos, um NPS pode receber muitas solicitações de conexão ao mesmo tempo, resultando em um desempenho degradado ou em uma sobrecarga. Quando um NPS está sobrecarregado, é uma boa ideia adicionar mais NPSs à sua rede e configurar o balanceamento de carga. Quando você distribui uniformemente as solicitações de conexão de entrada entre vários NPSs para evitar o sobrecarregamento de um ou mais NPSs, ele é chamado de balanceamento de carga.

O balanceamento de carga é particularmente útil para:

- As organizações que usam o protocolo de autenticação extensível – segurança de camada de transporte \(EAP-TLS\) ou protocolo de autenticação extensível protegida \(PEAP\)-TLS para autenticação. Como esses métodos de autenticação usam certificados para autenticação de servidor e para autenticação de computador cliente ou de usuário, a carga em proxies e servidores RADIUS é mais pesada do que quando métodos de autenticação baseados em senha são usados.
- Organizações que precisam manter a disponibilidade contínua do serviço.
- Provedores de serviços de Internet \(ISPs\) que terceirizam o acesso VPN para outras organizações. Os serviços de VPN terceirizados podem gerar um grande volume de tráfego de autenticação.

Há dois métodos que você pode usar para balancear a carga de solicitações de conexão enviadas para seu NPSs:

- Configure seus servidores de acesso à rede para enviar solicitações de conexão para vários servidores RADIUS. Por exemplo, se você tiver 20 pontos de acesso sem fio e dois servidores RADIUS, configure cada ponto de acesso para enviar solicitações de conexão para ambos os servidores RADIUS. Você pode balancear a carga e fornecer failover em cada servidor de acesso à rede configurando o servidor de acesso para enviar solicitações de conexão a vários servidores RADIUS em uma ordem de prioridade especificada. Esse método de balanceamento de carga geralmente é melhor para pequenas organizações que não implantam um grande número de clientes RADIUS.
- Use o NPS configurado como um proxy RADIUS para balancear a carga de solicitações de conexão entre vários NPSs ou outros servidores RADIUS. Por exemplo, se você tiver 100 pontos de acesso sem fio, um proxy NPS e três servidores RADIUS, você poderá configurar os pontos de acesso para enviar todo o tráfego para o proxy NPS. No proxy NPS, configure o balanceamento de carga para que o proxy distribua uniformemente as solicitações de conexão entre os três servidores RADIUS. Esse método de balanceamento de carga é melhor para organizações de médio e grande porte que têm muitos clientes e servidores RADIUS.

Em muitos casos, a melhor abordagem para o balanceamento de carga é configurar clientes RADIUS para enviar solicitações de conexão para dois servidores proxy NPS e, em seguida, configurar os proxies NPS para balancear a carga entre os servidores RADIUS. Essa abordagem fornece failover e balanceamento de carga para proxies NPS e servidores RADIUS.

## <a name="radius-server-priority-and-weight"></a>Prioridade e peso do servidor RADIUS

Durante o processo de configuração do proxy NPS, você pode criar grupos de servidores RADIUS remotos e, em seguida, adicionar servidores RADIUS a cada grupo. Para configurar o balanceamento de carga, você deve ter mais de um servidor RADIUS por grupo de servidores RADIUS remotos. Ao adicionar membros do grupo ou depois de criar um servidor RADIUS como um membro do grupo, você pode acessar a caixa de diálogo Adicionar servidor RADIUS para configurar os seguintes itens na guia balanceamento de carga:

- **Prioridade**. Prioridade especifica a ordem de importância do servidor RADIUS para o servidor proxy NPS. O nível de prioridade deve ser atribuído um valor que seja um inteiro, como 1, 2 ou 3. Quanto menor o número, maior a prioridade que o proxy NPS fornece ao servidor RADIUS. Por exemplo, se o servidor RADIUS receber a prioridade mais alta de 1, o proxy NPS enviará solicitações de conexão para o servidor RADIUS primeiro; Se os servidores com prioridade 1 não estiverem disponíveis, o NPS enviará solicitações de conexão para servidores RADIUS com prioridade 2 e assim por diante. Você pode atribuir a mesma prioridade a vários servidores RADIUS e, em seguida, usar a configuração de peso para balancear a carga entre eles.

- **Peso**. O NPS usa essa configuração de peso para determinar quantas solicitações de conexão devem ser enviadas a cada membro do grupo quando os membros do grupo têm o mesmo nível de prioridade. A configuração de peso deve ser atribuída a um valor entre 1 e 100, e o valor representa um percentual de 100%. Por exemplo, se o grupo de servidores remotos RADIUS contiver dois membros que tenham um nível de prioridade 1 e uma classificação de peso de 50, o proxy NPS encaminhará 50% das solicitações de conexão para cada servidor RADIUS.

- **Configurações avançadas**. Essas configurações de failover fornecem uma maneira para o NPS determinar se o servidor RADIUS remoto está indisponível. Se o NPS determinar que um servidor RADIUS está indisponível, ele poderá começar a enviar solicitações de conexão para outros membros do grupo. Com essas configurações, você pode configurar o número de segundos que o proxy NPS espera por uma resposta do servidor RADIUS antes de considerar a solicitação descartada; o número máximo de solicitações removidas antes que o proxy NPS identifique o servidor RADIUS como indisponível; e o número de segundos que podem decorrer entre solicitações antes que o proxy NPS identifique o servidor RADIUS como indisponível.

## <a name="configure-nps-proxy-load-balancing"></a>Configurar balanceamento de carga de proxy do NPS

Antes de configurar o balanceamento de carga, crie um plano de implantação que inclua quantos grupos de servidores RADIUS remotos você precisa, quais servidores são membros de cada grupo específico e a configuração de prioridade e peso de cada servidor.

>[!NOTE]
>As etapas a seguir pressupõem que você já implantou e configurou servidores RADIUS.

Para configurar o NPS para atuar como um servidor proxy e encaminhar solicitações de conexão de clientes RADIUS para servidores RADIUS remotos, você deve executar as seguintes ações:

1. Implante seus clientes RADIUS \(servidores VPN, servidores dial-up, servidores de gateway de serviços de terminal, comutadores de autenticação 802.1 X e pontos de acesso sem fio 802.1 X\) e configure-os para enviar solicitações de conexão para seus servidores proxy NPS.

2. No proxy NPS, configure os servidores de acesso à rede como clientes RADIUS. Para obter mais informações, consulte [configurar clientes RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. No proxy NPS, crie um ou mais grupos de servidores RADIUS remotos. Durante esse processo, adicione servidores RADIUS aos grupos de servidores RADIUS remotos. Para obter mais informações, consulte [configurar grupos de servidores remotos RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. No proxy NPS, para cada servidor RADIUS que você adiciona a um grupo de servidores remotos RADIUS, clique na guia **balanceamento de carga** do servidor RADIUS e **Configure prioridade**, **peso**e **Configurações avançadas**.

5. No proxy NPS, configure as políticas de solicitação de conexão para encaminhar solicitações de autenticação e contabilização para grupos de servidores RADIUS remotos. Você deve criar uma política de solicitação de conexão por grupo de servidores RADIUS remotos. Para obter mais informações, consulte [Configurar políticas de solicitação de conexão](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


