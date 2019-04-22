---
title: Política de Qualidade de Serviço (QoS)
description: Este tópico fornece uma visão geral da política de qualidade de serviço (QoS), que permite que você use a diretiva de grupo para priorizar a largura de banda de tráfego de rede de aplicativos e serviços no Windows Server 2016 específicos.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a3ef81825ef6544bc96506e37bc027e0ac2a78db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820277"
---
# <a name="quality-of-service-qos-policy"></a>Qualidade de serviço \(QoS\) política

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar a Política de QoS como um ponto central de gerenciamento de largura de banda de rede em toda sua infraestrutura do Active Directory por meio da criação de perfis de QoS, cujas configurações são distribuídas com a Política de Grupo.

>[!NOTE]
>  Além deste tópico, a seguinte documentação de política de QoS está disponível.  
>   
>  - [Introdução à política de QoS](qos-policy-get-started.md)
>  - [Gerenciar a política de QoS](qos-policy-manage.md)
>  - [Perguntas frequentes sobre a política de QoS](qos-policy-faq.md)

As políticas de QoS são aplicadas em uma sessão de logon do usuário ou um computador como parte de um objeto de diretiva de grupo \(GPO\) que você vinculou a um contêiner do Active Directory, como um domínio, site ou unidade organizacional \(UO\).

O gerenciamento de QoS tráfego ocorre abaixo da camada de aplicativo, que significa que seus aplicativos existentes não precisam ser modificados para aproveitar as vantagens que são fornecidas pelas políticas de QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemas operacionais que dão suporte à política de QoS

Você pode usar a política de QoS para gerenciar a largura de banda para computadores ou usuários com os seguintes sistemas operacionais Microsoft.

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

No Editor de gerenciamento do diretiva de grupo do Windows Server 2016, o caminho para a política de QoS para configuração do computador é o seguinte.

**Política de domínio padrão | Configuração do computador | Políticas | Configurações do Windows | Diretiva\-com base em QoS**

Esse caminho é ilustrado na imagem a seguir.

![Local da diretiva de QoS na política de grupo](../../media/QoS/QoS-Gp.jpg)

No Editor de gerenciamento do diretiva de grupo do Windows Server 2016, o caminho para a política de QoS para configuração do usuário é o seguinte.

**Política de domínio padrão | Configuração do usuário | Políticas | Configurações do Windows | Diretiva\-com base em QoS**

Por padrão, nenhuma política de QoS é configurada.

## <a name="why-use-qos-policy"></a>Por que usar a política de QoS?
  
À medida que aumenta o tráfego na rede, é cada vez mais importante para você equilibrar o desempenho de rede com o custo de serviço - mas o tráfego de rede não é normalmente mais fácil priorizar e gerenciar.

Em sua rede, de missão\-críticos e latência\-aplicativos confidenciais precisam competir por largura de banda de rede contra tráfego de prioridade inferior. Ao mesmo tempo, alguns usuários e computadores com o desempenho de rede específico requisitos podem exigir diferenciados níveis de serviço.

Os desafios do fornecimento de níveis de desempenho de rede previsível e econômica aparecem frequentemente pela primeira vez pela rede de longa distância \(WAN\) conexões ou com aplicativos sensíveis à latência, como voz sobre IP \(VoIP\) e o streaming de vídeo. No entanto, o objetivo final de fornecer níveis de serviço de rede previsível se aplica a qualquer ambiente de rede \(por exemplo, das empresas rede local\)e a mais do que os aplicativos de VoIP, como linha personalizada de sua empresa\-de\-aplicativos de negócios.
  
QoS baseada em diretivas é a ferramenta de gerenciamento de largura de banda de rede que fornece controle de rede - com base em aplicativos, usuários e computadores. 

Quando você usa a política de QoS, os aplicativos não precisarão ser escrito para interfaces de programação de aplicativo específico \(APIs\). Isso lhe dá a capacidade de usar a QoS com aplicativos existentes. Além disso, QoS baseada em diretivas se beneficia de sua infraestrutura existente de gerenciamento, como QoS baseada em política é integrada na política de grupo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definir a prioridade de QoS por meio de um ponto de código de serviços diferenciados \(DSCP\)
  
Você pode criar políticas de QoS que definem a prioridade do tráfego de rede com um ponto de código de serviços Differentiated \(DSCP\) valor que você atribui a diferentes tipos de tráfego de rede. 

O DSCP permite que você aplique um valor \(0 – 63\) dentro do tipo de serviço \(TOS\) campo no cabeçalho de um pacote IPv4 e dentro do campo Traffic Class no IPv6. 

O valor DSCP fornece classificação de tráfego de rede do protocolo da Internet \(IP\) nível, quais roteadores usam para decidir o comportamento de tráfego de enfileiramento de mensagens. 

Por exemplo, você pode configurar roteadores para colocar os pacotes com valores DSCP específicos em um dos três filas: alta prioridade, melhor esforço ou menor que a melhor esforço. 

O tráfego de rede críticos, que está na fila de prioridade alta, tem preferência sobre outros tráfegos.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limite o uso de largura de banda de rede por aplicativo, com taxa de aceleração

Você também pode limitar o tráfego de rede de saída de um aplicativo, especificando uma taxa de aceleração na política de QoS.

Uma política de QoS que define os limites de limitação determina a taxa de tráfego de rede de saída. Por exemplo, para gerenciar os custos de WAN, um departamento de TI pode implementar um contrato de nível de serviço que especifica que um servidor de arquivos nunca pode fornecer downloads, além de uma taxa específica.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Usar a política de QoS para aplicar o DSCP valores e as taxas de limitação

Você também pode usar a política de QoS para aplicar os valores de DSCP e limitar as taxas para o tráfego de rede de saída para o seguinte:

- Aplicativo de envio e o caminho do diretório

- Endereços IPv4 ou IPv6 de origem e destino ou prefixos de endereço

- Protocol - protocolo de controle de transmissão \(TCP\) e o protocolo de datagrama de usuário \(UDP\)

- Portas de origem e de destino e intervalos de porta \(TCP ou UDP\)

- Grupos de usuários ou computadores por meio da implantação na diretiva de grupo específicos

Usando esses controles, você pode especificar uma política de QoS com um valor DSCP de 46 para um aplicativo de VoIP, permitindo que os roteadores para colocar os pacotes de VoIP em uma fila de baixa latência, ou você pode usar uma política de QoS para restringir um conjunto de tráfego de saída de servidores para 512 quilobytes por segundo <c1/>KBps\) durante o envio da porta TCP 443.

Você também pode aplicar a política de QoS para um aplicativo específico que tem requisitos especiais de largura de banda. Para obter mais informações, consulte [cenários de política de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Vantagens da diretiva de QoS

Com a política de QoS, você pode configurar e impor políticas de QoS que não podem ser configuradas nos roteadores e comutadores. Política de QoS fornece as seguintes vantagens.
  
1. **Nível de detalhe:** É difícil criar políticas de QoS de nível de usuário em roteadores ou comutadores, especialmente se o computador do usuário for configuradas por meio da atribuição de endereço IP dinâmica ou se o computador não estiver conectado switch fixo ou portas do roteador, como acontece frequentemente com computadores portáteis. Em contraste, política de QoS torna mais fácil configurar um usuário\-política de QoS em um controlador de domínio de nível e propagar a política para o computador do usuário.
2. **Flexibilidade**. Independentemente de onde ou como um computador se conecta à rede, política de QoS é aplicada – o computador pode se conectar usando o Wi-Fi ou Ethernet de qualquer local. Para o usuário\-nível políticas de QoS, o QoS política é aplicada em qualquer dispositivo compatível em qualquer local em que o usuário fizer logon.
3. **Segurança:** Se seu departamento de TI criptografa o tráfego dos usuários de ponta a ponta usando o Internet Protocol security \(IPsec\), você não pode classificar o tráfego em roteadores com base em todas as informações acima da camada IP no pacote \(por exemplo, um A porta TCP\). No entanto, por meio da política de QoS, você pode classificar pacotes no dispositivo de final para indicar a prioridade dos pacotes no cabeçalho IP antes que as cargas IP são criptografadas e os pacotes são enviados.
4. **Desempenho:** Algumas funções de QoS, como as limitações, são melhor executadas quando eles são mais próximos da fonte. Política de QoS move essas funções de QoS mais próximas da origem.
5. **Capacidade de gerenciamento:** Política de QoS aumenta a capacidade de gerenciamento de rede de duas maneiras:

    **a**. Como ele se baseia na diretiva de grupo, você pode usar a política de QoS para configurar e gerenciar um conjunto de políticas de QoS de usuário/computador sempre que necessário e no computador de um controlador de domínio central.

    **b**. Política de QoS facilita a configuração do usuário/computador, fornecendo um mecanismo para especificar as políticas por Uniform Resource Locator \(URL\) em vez de especificar políticas com base nos endereços IP de cada um dos servidores em que precisam de políticas de QoS a ser aplicado. Por exemplo, suponha que sua rede tiver um cluster de servidores que compartilham um URL comum. Usando a política de QoS, você pode criar uma política com base na URL comum, em vez de criar uma política para cada servidor no cluster, com cada política com base no endereço IP de cada servidor.

Para o próximo tópico neste guia, consulte [Introdução à política de QoS](qos-policy-get-started.md).

