---
title: Qualidade da política de serviço (QoS)
description: Este tópico fornece uma visão geral da política de qualidade de serviço (QoS), que permite que você use a política de grupo para priorizar a largura de banda de tráfego de rede de aplicativos específicos e serviços no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 76405c677fe7eb4d51f9c190499b909ac2fe4a0c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="quality-of-service-qos-policy"></a>Qualidade de serviço \(QoS\) política

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar política de QoS como um ponto central de gerenciamento de largura de banda de rede em sua infraestrutura do Active Directory inteira através da criação de perfis de QoS, cujas configurações são distribuídas com política de grupo.

>[!NOTE]
>  Além de neste tópico, a seguinte documentação QoS política está disponível.  
>   
>  - [Introdução à política de QoS](qos-policy-get-started.md)
>  - [Gerenciar diretiva de QoS](qos-policy-manage.md)
>  - [Perguntas frequentes sobre a política de QoS](qos-policy-faq.md)

Políticas de QoS são aplicadas a uma sessão de logon do usuário ou um computador como parte de um objeto de política de grupo que você tiver vinculado a um contêiner do Active Directory, como um domínio, site ou unidade organizacional \(GPO\) \(OU\).

Gerenciamento de tráfego de QoS ocorre abaixo da camada de aplicativo, o que significa que seus aplicativos existentes não precisa ser modificado para aproveitar as vantagens que são fornecidas pelas políticas de QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemas operacionais com suporte a política de QoS

Você pode usar a política de QoS para gerenciar largura de banda para computadores ou usuários com os seguintes sistemas operacionais Microsoft.

- Windows Server 2016
- Windows 10
- Windows Server 2012 R2
- Windows 8.1
- Windows Server 2012
- Windows 8
- Windows Server 2008 R2
- Windows 7
- Windows Server 2008
- Windows Vista

### <a name="location-of-qos-policy-in-group-policy"></a>Local da diretiva de QoS na política de grupo

No Editor de gerenciamento do política de grupo do Windows Server 2016, o caminho para a diretiva de QoS para configuração do computador é a seguinte:

**Diretiva de domínio padrão | Configuração do computador | Políticas | Configurações do Windows | QoS baseada em política de senha \**

Esse caminho é ilustrado na imagem a seguir.

![Local da diretiva de QoS na política de grupo](../../media/QoS/QoS-Gp.jpg)

No Editor de gerenciamento do política de grupo do Windows Server 2016, o caminho para a diretiva de QoS para configuração do usuário é a seguinte:

**Diretiva de domínio padrão | Configuração do usuário | Políticas | Configurações do Windows | QoS baseada em política de senha \**

Por padrão, não há políticas de QoS são configuradas.

## <a name="why-use-qos-policy"></a>Por que usar a política de QoS?
  
Quando o tráfego aumenta em sua rede, é cada vez mais importante para você equilíbrio entre desempenho de rede e o custo de serviço - mas o tráfego de rede não é normalmente fácil priorizar e gerenciar.

Em sua rede, os aplicativos críticos de mission\ e latency\ confidenciais competem por largura de banda de rede contra o tráfego de prioridade mais baixa. Ao mesmo tempo, alguns usuários e computadores com o desempenho de rede específica requisitos podem exigir diferenciados níveis de serviço.

Os desafios de fornecer níveis de desempenho de rede econômica e previsíveis aparecem com frequência pela primeira vez em conexões de \(WAN\) de rede de longa distância ou com aplicativos sensíveis à latência, como VoIP \(VoIP\) IP e streaming de vídeo. No entanto, o objetivo de fim de fornecer os níveis de serviço de rede previsível se aplica a qualquer ambiente de rede \ (por exemplo, um empresas rede local rede \) e a mais de aplicativos de VoIP, como aplicativos de negócios de of\ line\ personalizados da sua empresa.
  
QoS baseada em política é a ferramenta de gerenciamento de largura de banda de rede que fornece controle de rede - com base em aplicativos, os usuários e computadores. 

Quando você usa a política de QoS, seus aplicativos não precisa ser escrito para o aplicativo específico programação interfaces \(APIs\). Isso lhe dá a capacidade de usar QoS com aplicativos existentes. Além disso, com base em política QoS tira proveito das sua infraestrutura de gerenciamento existentes, porque QoS baseada em política é incorporado a política de grupo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definir a prioridade de QoS por meio de um ponto de código de serviços diferenciados \(DSCP\)
  
Você pode criar políticas de QoS que definem a prioridade de tráfego de rede com um valor \(DSCP\) diferenciadas ponto de código de serviços que você atribuir a diferentes tipos de tráfego de rede. 

O DSCP permite que você aplique \(0–63\) um valor no campo tipo de serviço \(TOS\) no cabeçalho de um pacote IPv4 e dentro do campo de classe de tráfego no IPv6. 

O valor DSCP fornece a classificação do tráfego de rede no nível do protocolo de Internet \(IP\) roteadores usam para decidir o tráfego de comportamento de enfileiramento de mensagens. 

Por exemplo, você pode configurar roteadores para colocar os pacotes com valores DSCP específicos em uma das três filas: alta prioridade, melhor ou inferiores a melhor esforço. 

O tráfego de rede essenciais, que é na fila de alta prioridade, tem preferência sobre outro tráfego.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Uso de largura de banda de rede de limite por aplicativo com taxa de aceleração

Você também pode limitar o tráfego de rede de saída de um aplicativo, especificando uma taxa de aceleração na política de QoS.

Uma política de QoS que define os limites de otimização determina a taxa de saída do tráfego de rede. Por exemplo, para gerenciar os custos da WAN, um departamento de TI pode implementar um contrato de nível de serviço que especifica que um servidor de arquivos pode oferecer nunca downloads além de uma taxa específica.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Usar a política de QoS para aplicar DSCP valores e as taxas de aceleração

Você também pode usar a política de QoS para aplicar valores DSCP e limitar as taxas para tráfego de rede de saída para o seguinte:

- Aplicativo de envio e o caminho de diretório

- Endereços IPv4 ou IPv6 de origem e de destino ou prefixos

- Protocolo - controle de transmissão de protocolo \(TCP\) e usuário datagrama protocolo \(UDP\)

- Portas de origem e de destino e intervalos de portas \(TCP or UDP\)

- Grupos específicos de usuários ou computadores por meio da implantação na política de grupo

Usando esses controles, você pode especificar uma política de QoS com um valor DSCP de 46 para um aplicativo de VoIP, permitindo roteadores colocar os pacotes de VoIP em uma fila de baixa latência, ou você pode usar uma política de QoS para limitar a um conjunto de tráfego de saída dos servidores 512 KB por segundo \(KBps\) durante o envio da porta TCP 443.

Você também pode aplicar a política de QoS para um aplicativo específico que tem requisitos de largura de banda especial. Para obter mais informações, consulte [cenários de política de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Vantagens da política de QoS

Com a política de QoS, você pode configurar e impor políticas de QoS que não podem ser configuradas em roteadores e switches. Política de QoS fornece as seguintes vantagens.
  
1. **Nível de detalhe:** é difícil criar políticas de QoS de nível de usuário em roteadores ou switches, especialmente se o computador do usuário é um configurado usando IP dinâmico resolver atribuição ou se o computador não estiver conectado a switch fixo ou portas do roteador, como ocorre com frequência com computadores portáteis. Em contraste, política de QoS torna mais fácil configurar uma política de QoS user\ nível em um controlador de domínio e propagar a política para o computador do usuário.
2. **Flexibilidade**. Independentemente de onde ou como um computador se conecta à rede, QoS política é aplicada - o computador pode se conectar usando Wi-Fi ou Ethernet de qualquer local. Para políticas de QoS user\ nível, a política de QoS é aplicada em qualquer dispositivo compatível em qualquer local onde o usuário faz logon.
3. **Segurança:** se seu departamento de TI criptografar o tráfego dos usuários de ponta a ponta, usando o protocolo de Internet security \(IPsec\), você não pode classificar o tráfego em roteadores com base em qualquer informação acima da camada IP no pacote \ (por exemplo, um port\ TCP). No entanto, usando a política de QoS, você pode classificar pacotes no dispositivo final para indicar a prioridade dos pacotes no cabeçalho IP antes que as cargas IP são criptografadas e os pacotes são enviados.
4. **Desempenho:** algumas funções de QoS, como otimização, melhor são executadas quando eles são mais próximos à origem. Política de QoS move tais funções de QoS mais próximos à origem.
5. **Capacidade de gerenciamento:** política QoS melhora a capacidade de gerenciamento de rede de duas maneiras:

    **a**. Como ele é baseado na política de grupo, você pode usar política de QoS para configurar e gerenciar um conjunto de políticas de QoS computador/usuário sempre que necessário e no computador de um controlador de domínio central.

    **b**. Política de QoS facilita a configuração do computador/usuário, fornecendo um mecanismo para especificar políticas pela URL \(URL\) em vez de especificar políticas com base nos endereços IP de cada um dos servidores onde as políticas de QoS precisam ser aplicada. Por exemplo, suponha que sua rede tem um cluster de servidores que compartilham uma URL comuns. Usando a política de QoS, você pode criar uma política com base no URL comum, em vez de criar uma política para cada servidor no cluster, com cada política com base no endereço IP de cada servidor.

Para o próximo tópico neste guia, consulte [Introdução à política de QoS](qos-policy-get-started.md).

