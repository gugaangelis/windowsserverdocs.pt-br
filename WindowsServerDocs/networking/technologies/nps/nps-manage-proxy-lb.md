---
title: Balanceamento de carga do servidor de Proxy NPS
description: Você pode usar este tópico para saber mais sobre a funcionalidade e os recursos do Windows Server 2016 e VPN do Windows 10.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 528280e6-b47e-489f-b310-b257d434aa0d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9f00eb6575284a69358c58b36d08672cdd69dc18
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="nps-proxy-server-load-balancing"></a>Balanceamento de carga do servidor de Proxy NPS

Aplica-se a: Windows Server 2016

Clientes remotos Authentication Dial-In User Service (RADIUS), que são servidores de acesso à rede, como servidores de rede virtual privada (VPN) e pontos de acesso sem fio, criar solicitações de conexão e enviá-las para servidores RADIUS, como NPS. Em alguns casos, um servidor NPS pode receber muitas solicitações de conexão ao mesmo tempo, resultando em degradação do desempenho ou uma sobrecarga. Quando um servidor NPS é sobrecarregado, é uma boa ideia adicionar mais servidores NPS para sua rede e configurar balanceamento de carga. Quando você distribuir uniformemente solicitações de conexão recebidas entre vários servidores NPS para evitar a sobrecarga de um ou mais servidores NPS, ele é chamado de balanceamento de carga.

Balanceamento de carga é particularmente útil para:

- As organizações que usam \(EAP-TLS\) Extensible Authentication Protocol-Transport Layer Security ou Protected Extensible Authentication Protocol \ (PEAP\)-TLS para autenticação. Como esses métodos de autenticação usam certificados para autenticação de servidor e para o usuário ou autenticação do computador cliente, a carga em servidores e proxies RADIUS é mais pesada que quando os métodos de autenticação baseada em senha são usados.
- Organizações que precisam manter a disponibilidade de serviço contínuo.
- Internet serviço provedores \(ISPs\) que terceirizar o acesso VPN para outras empresas. Os serviços VPN terceirizados podem gerar um grande volume de tráfego de autenticação.

Há dois métodos que você pode usar para equilibrar a carga de solicitações de conexão enviadas aos servidores NPS:

- Configure os servidores de acesso de rede para enviar solicitações de conexão para vários servidores RADIUS. Por exemplo, se você tiver 20 pontos de acesso sem fio e dois servidores RADIUS, configure cada ponto de acesso para enviar solicitações de conexão para ambos os servidores RADIUS. Você pode carregar saldo e fornecer failover em cada servidor de acesso de rede ao configurar o servidor de acesso para enviar solicitações de conexão com vários servidores RADIUS em uma ordem especificada de prioridade. Esse método de balanceamento de carga é normalmente melhor para pequenas empresas que não implantou um grande número de clientes RADIUS.
- Use configurado como um proxy RADIUS de NPS para carregar as solicitações de conexão de equilíbrio entre vários servidores NPS ou outros servidores RADIUS. Por exemplo, se você tiver três servidores RADIUS, um proxy NPS e 100 pontos de acesso sem fio, você pode configurar os pontos de acesso para enviar todo o tráfego para o proxy NPS. No proxy NPS, configure balanceamento de carga de forma que o proxy uniformemente distribui as solicitações de conexão entre os servidores RADIUS três. Esse método de balanceamento de carga é melhor para médias e grandes organizações que possuem muitos clientes RADIUS e servidores.

Em muitos casos, a melhor abordagem ao balanceamento de carga é configurar clientes RADIUS para enviar solicitações de conexão para os servidores proxy NPS dois e, em seguida, configure os proxies NPS para carregar o equilíbrio entre servidores RADIUS. Essa abordagem apresenta failover e balanceamento de carga para NPS proxies e servidores RADIUS.

## <a name="radius-server-priority-and-weight"></a>Peso e prioridade de servidor RADIUS

Durante o processo de configuração de proxy NPS, você pode criar grupos de servidores RADIUS remotos e, em seguida, adicionar servidores RADIUS para cada grupo. Para configurar o balanceamento de carga, você deve ter mais de um servidor RADIUS por grupo de servidores remotos RADIUS. Ao adicionar membros do grupo, ou após a criação de um servidor RADIUS como um membro do grupo, você pode acessar a caixa de diálogo Adicionar RADIUS servidor para configurar os seguintes itens na guia balanceamento de carga:

- **Prioridade**. Prioridade Especifica a ordem de importância do servidor RADIUS no servidor proxy NPS. Nível de prioridade deve ser atribuído a um valor que é um número inteiro, como 1, 2 ou 3. Quanto menor o número, a prioridade mais alta o proxy NPS dá ao servidor RADIUS. Por exemplo, se o servidor RADIUS é atribuído a prioridade mais alta de 1, o proxy NPS envia solicitações de conexão para o servidor RADIUS pela primeira vez; Se a servidores com prioridade 1 não estiverem disponíveis, o NPS, em seguida, envia solicitações de conexão para servidores RADIUS com prioridade 2 e assim por diante. Você pode atribuir a mesma prioridade para vários servidores RADIUS e, em seguida, use a configuração de peso para carregar o equilíbrio entre eles.

- **Peso**. Usos NPS solicita essa configuração de espessura para determinar quantos conexão para enviar para cada grupo membro quando os membros do grupo têm o mesmo nível de prioridade. Peso configuração deve ser atribuída a um valor entre 1 e 100 e o valor representa uma porcentagem de 100%. Por exemplo, se o grupo de servidores remotos RADIUS contém dois membros que tenham um nível de prioridade 1 e uma classificação de espessura de 50, o proxy NPS encaminha 50% das solicitações de conexão para cada servidor RADIUS.

- **Configurações avançadas**. Essas configurações de failover fornecem uma maneira de NPS determinar se o servidor RADIUS remoto está indisponível. Se NPS determina que um servidor RADIUS está disponível, ele pode começar a enviar solicitações de conexão para outros membros do grupo. Com essas configurações, você pode configurar o número de segundos que o proxy NPS aguarda uma resposta do servidor RADIUS antes que ele considera a solicitação foi ignorada; o número máximo de solicitações ignoradas antes que o proxy NPS identifica o servidor RADIUS como indisponível; e o número de segundos que pode passar entre solicitações antes do proxy NPS identifica o servidor RADIUS como indisponível.

## <a name="configure-nps-proxy-load-balancing"></a>Configurar balanceamento de carga de proxy NPS

Antes de configurar o balanceamento de carga, crie um plano de implantação que inclui quantas RADIUS grupos de servidores remotos exige que você, quais servidores são membros de cada grupo em particular e a configuração de prioridade e peso para cada servidor.

>[!NOTE]
>As etapas a seguir pressupõem que você já tenha implantado e configurado servidores RADIUS.

Para configurar o NPS para atuar como um servidor proxy e solicitações de conexão direta dos clientes RADIUS para servidores RADIUS remotos, você deve executar as seguintes ações:

1. Implantar seus clientes RADIUS \ (servidores VPN, servidores dial-up, servidores de Gateway de serviços de Terminal, comutadores de autenticação 802.1 X e 802.1 X sem fio points\ de acesso) e configurá-los para enviar solicitações de conexão para seus servidores de proxy NPS.

2. No proxy NPS, configure os servidores de acesso de rede como clientes RADIUS. Para obter mais informações, consulte [configurar clientes RADIUS](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-radius-clients-configure).

3. No proxy NPS, crie um ou mais grupos de servidores RADIUS remotos. Durante esse processo, adicione servidores RADIUS para os grupos de servidores remotos RADIUS. Para obter mais informações, consulte [definir grupos de servidores RADIUS remotos](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-rrsg-configure).

4. No proxy NPS, para cada servidor RADIUS que você adicionar a um grupo de servidores remotos RADIUS, clique no servidor RADIUS **balanceamento de carga** guia e, em seguida, configure **prioridade**, **espessura**, e **configurações avançadas**.

5. No proxy NPS, configure políticas de solicitação de conexão para encaminhar solicitações de autenticação e estatísticas para grupos de servidores remotos RADIUS. Você deve criar uma política de solicitação de conexão por grupo de servidores remotos RADIUS. Para obter mais informações, consulte [configurar políticas de solicitação de Conexão](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-crp-configure).


