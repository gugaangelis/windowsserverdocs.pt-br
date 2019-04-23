---
title: Balanceamento de carga do servidor de Proxy NPS
description: Você pode usar este tópico para saber mais sobre a funcionalidade e os recursos do Windows Server 2016 e de VPN do Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 04f466f7646fc5109af7379cab1240d8cd6829ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874397"
---
# <a name="nps-proxy-server-load-balancing"></a>Balanceamento de carga do servidor de Proxy NPS

Aplica-se a: Windows Server 2016

Clientes remotos autenticação Dial-In do serviço RADIUS (User), que são servidores de acesso de rede, como servidores de rede virtual privada (VPN) e pontos de acesso sem fio, criar solicitações de conexão e os enviam aos servidores RADIUS, como NPS. Em alguns casos, um NPS pode receber muitas solicitações de conexão de uma vez, resultando em desempenho degradado ou uma sobrecarga. Quando um NPS está sobrecarregado, é uma boa ideia adicionar NPSs mais a sua rede e configurar o balanceamento de carga. Quando você distribui uniformemente solicitações de conexão entrada entre vários NPSs para evitar a sobrecarga de um ou mais NPSs, ele é chamado de balanceamento de carga.

Balanceamento de carga é particularmente útil para:

- As organizações que usam Extensible Authentication Protocol-Transport Layer Security \(EAP-TLS\) ou Protected Extensible Authentication Protocol \(PEAP\)- TLS para autenticação. Como esses métodos de autenticação usam certificados para autenticação de servidor e para o usuário ou autenticação do computador cliente, a carga nos servidores e proxies RADIUS é mais pesada do que quando são usados os métodos de autenticação baseada em senha.
- Organizações que precisam para manter a disponibilidade contínua do serviço.
- Provedores de serviços de Internet \(ISPs\) que terceirizar o acesso VPN para outras organizações. Os serviços VPN terceirizados podem gerar um grande volume de tráfego de autenticação.

Há dois métodos que você pode usar para balancear a carga de solicitações de conexão enviadas ao seu NPSs:

- Configure seus servidores de acesso de rede para enviar solicitações de conexão para vários servidores RADIUS. Por exemplo, se você tiver 20 pontos de acesso sem fio e dois servidores RADIUS, configure cada ponto de acesso para enviar solicitações de conexão para ambos os servidores RADIUS. Você pode balancear a carga e fornecer um failover em cada servidor de acesso de rede ao configurar o servidor de acesso para enviar solicitações de conexão para vários servidores RADIUS na ordem de prioridade especificada. Esse método de balanceamento de carga é geralmente melhor para pequenas organizações que não implantam um grande número de clientes RADIUS.
- Use o NPS configurado como um proxy RADIUS para a carga de solicitações de conexão de equilíbrio entre vários NPSs ou outros servidores RADIUS. Por exemplo, se você tiver 100 pontos de acesso sem fio, um proxy NPS e três servidores RADIUS, você pode configurar os pontos de acesso para enviar todo o tráfego para o proxy do NPS. No proxy do NPS, configure o balanceamento de carga para que o proxy Distribui uniformemente as solicitações de conexão entre os três servidores RADIUS. Esse método de balanceamento de carga é melhor para as organizações de médio e grandes que têm muitos clientes e servidores RADIUS.

Em muitos casos, a melhor abordagem para balanceamento de carga é configurar clientes RADIUS para enviar solicitações de conexão para dois servidores de proxy NPS e, em seguida, configure os proxies do NPS para equilibrar entre os servidores RADIUS. Essa abordagem fornece failover e balanceamento de carga para proxies NPS e servidores RADIUS.

## <a name="radius-server-priority-and-weight"></a>Prioridade do servidor RADIUS e o peso

Durante o processo de configuração de proxy do NPS, você pode criar grupos de servidores remotos RADIUS e, em seguida, adicionar servidores RADIUS para cada grupo. Para configurar o balanceamento de carga, você deve ter mais de um servidor RADIUS por grupo de servidores RADIUS remotos. Durante a adição de membros do grupo, ou depois de criar um servidor RADIUS como um membro do grupo, você pode acessar a caixa de diálogo do servidor RADIUS Adicionar para configurar os seguintes itens na guia balanceamento de carga:

- **prioridade**. Prioridade Especifica a ordem de importância do servidor RADIUS para o servidor de proxy do NPS. Nível de prioridade deve ser atribuído um valor que é um inteiro, como 1, 2 ou 3. Quanto menor o número, maior a prioridade que o proxy NPS fornece para o servidor RADIUS. Por exemplo, se o servidor RADIUS for atribuído a prioridade mais alta de 1, o proxy NPS envia solicitações de conexão para o servidor RADIUS primeiro; Se não houver servidores com prioridade 1, o NPS, em seguida, envia solicitações de conexão para servidores RADIUS com prioridade 2 e assim por diante. Você pode atribuir a mesma prioridade para vários servidores RADIUS e, em seguida, use a configuração de peso para balancear a carga entre eles.

- **Peso**. Usos NPS essa configuração de peso para determinar quantos conexão solicitações para enviar para cada membro do grupo quando os membros do grupo têm o mesmo nível de prioridade. Configuração de peso deve ser atribuída um valor entre 1 e 100 e o valor representa um percentual de 100 por cento. Por exemplo, se o grupo de servidores remotos RADIUS contém dois membros que têm um nível de prioridade 1 e uma classificação de peso de 50, o proxy NPS encaminha 50 por cento das solicitações de conexão para cada servidor RADIUS.

- **Configurações avançadas**. Essas configurações de failover fornecem uma maneira para o NPS determinar se o servidor RADIUS remoto não está disponível. Se o NPS determina que um servidor RADIUS está indisponível, ela pode começar a enviar solicitações de conexão a outros membros do grupo. Com essas configurações, você pode configurar o número de segundos que o proxy NPS aguarda uma resposta do servidor RADIUS antes de considerar a solicitação foi ignorada; o número máximo de solicitações ignoradas antes que o proxy NPS identifica o servidor RADIUS como indisponível; e o número de segundos que pode decorrer entre solicitações antes que o proxy NPS identifica o servidor RADIUS como indisponível.

## <a name="configure-nps-proxy-load-balancing"></a>Configurar balanceamento de carga de proxy do NPS

Antes de configurar o balanceamento de carga, crie um plano de implantação que inclui quantas remotos grupos de servidores RADIUS você precisar, quais servidores são membros de cada grupo em particular e a configuração de prioridade e peso para cada servidor.

>[!NOTE]
>As etapas a seguir pressupõem que você já tiver implantado e configurou servidores RADIUS.

Para configurar o NPS para atuar como um servidor proxy e solicitações de conexão direta de clientes RADIUS para servidores RADIUS remotos, você deve executar as seguintes ações:

1. Implantar os clientes RADIUS \(802.1 X, servidores dial-up, servidores de Gateway de serviços de Terminal, comutadores de autenticação 802.1X e servidores VPN de pontos de acesso sem fio\) e configurá-los para enviar solicitações de conexão para o proxy NPS servidores.

2. No proxy do NPS, configure os servidores de acesso de rede, como clientes RADIUS. Para obter mais informações, consulte [configurar clientes de RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. No proxy do NPS, crie um ou mais grupos de servidores RADIUS remotos. Durante esse processo, adicione servidores RADIUS para os grupos de servidores RADIUS remotos. Para obter mais informações, consulte [configurar grupos de servidores remotos RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. No proxy do NPS, para cada servidor RADIUS que você adicionar a um grupo de servidores remotos RADIUS, clique no servidor RADIUS **balanceamento de carga** guia e, em seguida, configure **prioridade**, **peso** , e **configurações avançadas**.

5. No proxy do NPS, configure políticas de solicitação de conexão para encaminhar solicitações de autenticação e estatísticas para grupos de servidores RADIUS remotos. Você deve criar uma política de solicitação de conexão por grupo de servidores RADIUS remotos. Para obter mais informações, consulte [configurar políticas de solicitação de Conexão](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


