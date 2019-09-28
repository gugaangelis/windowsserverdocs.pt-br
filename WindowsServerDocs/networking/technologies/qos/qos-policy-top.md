---
title: Política de Qualidade de Serviço (QoS)
description: Este tópico fornece uma visão geral da política de QoS (qualidade de serviço), que permite que você use Política de Grupo para priorizar a largura de banda de tráfego de rede de aplicativos e serviços específicos no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 16918506-102c-482e-89d3-004ad8d6aabe
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 429c38d93c2c5c0053153d538304767c8261229c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395862"
---
# <a name="quality-of-service-qos-policy"></a>Qualidade da política \(de\) QoS de serviço

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar a Política de QoS como um ponto central de gerenciamento de largura de banda de rede em toda sua infraestrutura do Active Directory por meio da criação de perfis de QoS, cujas configurações são distribuídas com a Política de Grupo.

>[!NOTE]
>  Além deste tópico, a documentação de política de QoS a seguir está disponível.  
>   
>  - [Introdução com a política de QoS](qos-policy-get-started.md)
>  - [Gerenciar política de QoS](qos-policy-manage.md)
>  - [Perguntas frequentes sobre a política de QoS](qos-policy-faq.md)

As políticas de QoS são aplicadas a uma sessão de logon de usuário ou a um computador como \(parte\) de um GPO de política de grupo objeto que você vinculou a um contêiner de Active Directory, como um domínio \(,\)site ou UO da unidade organizacional.

O gerenciamento de tráfego de QoS ocorre abaixo da camada de aplicativo, o que significa que os aplicativos existentes não precisam ser modificados para se beneficiarem das vantagens fornecidas pelas políticas de QoS.

## <a name="operating-systems-that-support-qos-policy"></a>Sistemas operacionais que dão suporte à política de QoS

Você pode usar a política de QoS para gerenciar a largura de banda de computadores ou usuários com os seguintes sistemas operacionais da Microsoft.

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

### <a name="location-of-qos-policy-in-group-policy"></a>Local da política de QoS no Política de Grupo

No Windows Server 2016 Editor de Gerenciamento de Política de Grupo, o caminho para a política de QoS para a configuração do computador é o seguinte.

**Política de domínio padrão | Configuração do computador | Políticas | Configurações do Windows | QoS\-baseada em política**

Esse caminho é ilustrado na imagem a seguir.

![Local da política de QoS no Política de Grupo](../../media/QoS/QoS-Gp.jpg)

No Windows Server 2016 Editor de Gerenciamento de Política de Grupo, o caminho para a política de QoS para a configuração do usuário é o seguinte.

**Política de domínio padrão | Configuração do usuário | Políticas | Configurações do Windows | QoS\-baseada em política**

Por padrão, nenhuma política de QoS é configurada.

## <a name="why-use-qos-policy"></a>Por que usar a política de QoS?
  
À medida que o tráfego aumenta em sua rede, é cada vez mais importante balancear o desempenho da rede com o custo de serviço, mas o tráfego de rede normalmente não é fácil de priorizar e gerenciar.

Em sua rede, os\-aplicativos críticos de\-missão crítica e de latência devem competir pela largura de banda da rede em relação ao tráfego de prioridade mais baixa. Ao mesmo tempo, alguns usuários e computadores com requisitos de desempenho de rede específicos podem exigir níveis de serviço diferenciados.

Os desafios de fornecer níveis de desempenho de rede previsíveis e econômicos geralmente aparecem em conexões WAN \(\) de rede de longa distância ou com aplicativos sensíveis à latência, como \(VoIPdevozsobreIP\) e streaming de vídeo. No entanto, a meta final de fornecer níveis de serviço de rede previsíveis se \(aplica a qualquer ambiente de rede, por exemplo\), a uma rede de área local das empresas e a mais de aplicativos VoIP, como a linhapersonalizadadasuaempresa\-de\-aplicativos de negócios.
  
A QoS baseada em políticas é a ferramenta de gerenciamento de largura de banda de rede que fornece controle de rede baseado em aplicativos, usuários e computadores. 

Quando você usa a política de QoS, seus aplicativos não precisam ser escritos para APIs \(\)de interfaces de programação de aplicativo específicas. Isso lhe dá a capacidade de usar QoS com aplicativos existentes. Além disso, a QoS baseada em políticas aproveita sua infraestrutura de gerenciamento existente, pois a QoS baseada em políticas é incorporada ao Política de Grupo.

## <a name="define-qos-priority-through-a-differentiated-services-code-point-dscp"></a>Definir a prioridade de QoS por meio de \(um DSCP de ponto de código serviços diferenciados\)
  
Você pode criar políticas de QoS que definem a prioridade de tráfego de rede \(com\) um valor de DSCP de ponto de código serviços diferenciados que você atribui a diferentes tipos de tráfego de rede. 

O DSCP \(permite aplicar um valor 0 – 63\) dentro do tipo de campo Service \(tos\) no cabeçalho de um pacote IPv4 e dentro do campo classe de tráfego no IPv6. 

O valor de DSCP fornece a classificação de tráfego de rede \(no\) nível de IP do protocolo de Internet, que os roteadores usam para decidir o comportamento do enfileiramento de tráfego. 

Por exemplo, você pode configurar roteadores para colocar pacotes com valores DSCP específicos em uma das três filas: alta prioridade, melhor esforço ou menor esforço. 

O tráfego de rede de missão crítica, que está na fila de alta prioridade, tem preferência sobre outro tráfego.

### <a name="limit-network-bandwidth-use-per-application-with-throttle-rate"></a>Limitar o uso de largura de banda de rede por aplicativo com taxa de aceleração

Você também pode limitar o tráfego de rede de saída de um aplicativo especificando uma taxa de limitação na política de QoS.

Uma política de QoS que define os limites de limitação determina a taxa de tráfego de rede de saída. Por exemplo, para gerenciar os custos de WAN, um departamento de ti pode implementar um contrato de nível de serviço que especifica que um servidor de arquivos nunca pode fornecer downloads além de uma taxa específica.  

### <a name="use-qos-policy-to-apply-dscp-values-and-throttle-rates"></a>Usar a política de QoS para aplicar valores de DSCP e taxas de limitação

Você também pode usar a política de QoS para aplicar valores de DSCP e taxas de limitação para o tráfego de rede de saída para o seguinte:

- Enviando o caminho do aplicativo e do diretório

- Endereços IPv4 ou IPv6 de origem e de destino ou prefixos de endereço

- Protocolo-protocolo \(TCP\) e UDP do protocolo \(de datagrama do usuário\)

- Portas de origem e de destino e \(intervalos de porta TCP ou UDP\)

- Grupos específicos de usuários ou computadores por meio da implantação no Política de Grupo

Usando esses controles, você pode especificar uma política de QoS com um valor DSCP de 46 para um aplicativo VoIP, permitindo que roteadores coloquem pacotes VoIP em uma fila de baixa latência, ou você pode usar uma política de QoS para limitar um conjunto de tráfego de saída de servidores a 512 quilobytes por segundo <C1/>Kbps\) ao enviar da porta TCP 443.

Você também pode aplicar a política de QoS a um aplicativo específico que tenha requisitos especiais de largura de banda. Para obter mais informações, consulte [cenários de política de QoS](qos-policy-scenarios.md).
  
## <a name="advantages-of-qos-policy"></a>Vantagens da política de QoS

Com a política de QoS, você pode configurar e impor políticas de QoS que não podem ser configuradas em roteadores e comutadores. A política de QoS oferece as seguintes vantagens.
  
1. **Nível de detalhe:** É difícil criar políticas de QoS no nível do usuário em roteadores ou comutadores, especialmente se o computador do usuário estiver configurado usando a atribuição de endereço IP dinâmico ou se o computador não estiver conectado a comutador ou portas de roteador fixos, como é frequentemente o caso com computadores portáteis. Por outro lado, a política de QoS torna mais fácil configurar\-uma política de QoS de nível de usuário em um controlador de domínio e propagar a política para o computador do usuário.
2. **Flexibilidade**. Independentemente de onde ou como um computador se conecta à rede, a política de QoS é aplicada-o computador pode se conectar usando WiFi ou Ethernet de qualquer local. Para as\-políticas de QoS de nível de usuário, a política de QoS é aplicada em qualquer dispositivo compatível em qualquer local em que o usuário fizer logon.
3. **Segurança** Se o seu departamento de ti criptografar o tráfego dos usuários de ponta a ponta usando IPsec \(\)de segurança de protocolo Internet, você não poderá classificar o tráfego em roteadores com base em qualquer informação acima \(da camada IP no pacote, por exemplo, um Porta\)TCP. No entanto, usando a política de QoS, você pode classificar os pacotes no dispositivo final para indicar a prioridade dos pacotes no cabeçalho IP antes que as cargas de IP sejam criptografadas e que os pacotes sejam enviados.
4. **Desempenho** Algumas funções de QoS, como a limitação, são mais bem executadas quando estão mais perto da origem. A política de QoS move essas funções de QoS mais próximas da origem.
5. **Capacidade** A política de QoS aprimora a capacidade de gerenciamento de rede de duas maneiras:

    **a**. Como ele se baseia em Política de Grupo, você pode usar a política de QoS para configurar e gerenciar um conjunto de políticas de QoS de usuário/computador sempre que necessário e em um computador do controlador de domínio central.

    **b**. A política de QoS facilita a configuração do usuário/computador fornecendo um mecanismo para especificar políticas por URL \(\) do localizador de recursos uniforme em vez de especificar políticas com base nos endereços IP de cada um dos servidores em que as políticas de QoS precisam a ser aplicado. Por exemplo, suponha que sua rede tenha um cluster de servidores que compartilham uma URL comum. Usando a política de QoS, você pode criar uma política com base na URL comum, em vez de criar uma política para cada servidor no cluster, com cada política baseada no endereço IP de cada servidor.

Para o próximo tópico deste guia, consulte [introdução com a política de QoS](qos-policy-get-started.md).

