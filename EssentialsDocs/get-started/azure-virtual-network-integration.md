---
title: Integração de rede virtual do Azure
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 92c8241d861e72d5f9f409a334e6edbeed5eae4c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310571"
---
# <a name="azure-virtual-network-integration"></a>Integração de rede virtual do Azure

>Aplica-se a: Windows Server 2016 Essentials

À medida que as organizações passam para a computação em nuvem, raramente eles movem todos os seus recursos de 100% ao mesmo tempo, mas, em vez disso, assumem uma abordagem em que alguns recursos estão na nuvem e outros ainda estão no local. Essa abordagem híbrida torna mais fácil para as organizações mover alguns recursos de computação para a nuvem, mas também permite que eles aumentem sua infraestrutura de ti sem precisar adquirir novo hardware.

Ao implementar essa abordagem híbrida para a computação, é necessária uma maneira tranqüila de que os recursos em ambos os locais se comuniquem entre si. A rede virtual do Azure é um serviço do Azure que permite que as organizações criem uma rede virtual privada de ponto a ponto (P2P) ou site a site (S2S) que faz com que os recursos em execução no Azure (como máquinas virtuais e armazenamento) pareçam ser na rede local para acesso contínuo a aplicativos e recursos.

A configuração de uma rede virtual do Azure pode ser complexa. Com o Windows Server Essentials 2016, você pode configurar facilmente sua rede virtual do Azure por meio de um assistente simples que ajuda a escolher os padrões mais apropriados para seu ambiente de rede. Conforme mostrado na captura de tela abaixo, uma nova tarefa de integração de rede virtual do Azure foi adicionada à seção serviços de Microsoft Cloud do painel do Windows Essentials para introduzir a rede virtual do Azure, bem como fornecer um link rápido para iniciar a integração .

![Uma captura de tela mostrando a guia introdução na home page do painel do Windows Server Essentials. Na guia introdução, a seção serviços foi selecionada e o painel indica em integração de serviços de Microsoft Cloud que a rede virtual do Azure está desabilitada no momento.](media/azure-virtual-network-1.PNG)

Quando você clicar no link **integrar agora** para a rede virtual do Azure na captura de tela acima, uma caixa de diálogo será exibida solicitando que você faça logon na sua conta de Microsoft Azure. Se você não tiver uma conta de Microsoft Azure, terá a opção de se inscrever para uma nesta tela, que redirecionará a você para o portal de inscrição de conta do Azure:

![Uma captura de tela mostrando a página entrar no Microsoft Azure do assistente de integração com a rede virtual do Azure.](media/azure-virtual-network-2.PNG)

Depois de entrar no Azure, você verá a opção de escolher qual assinatura deseja associar ao serviço de rede virtual do Azure:

![Uma captura de tela mostrando a página minha assinatura de Microsoft Azure do assistente de integração com a rede virtual do Azure.](media/azure-virtual-network-3.PNG)

Depois de escolher a assinatura do Azure que você deseja usar para a rede virtual do Azure, você verá a opção de criar uma nova rede virtual do Azure ou, se uma já tiver sido configurada nessa assinatura, a caixa suspensa mostrará ela está disponível. Você também deve escolher um nome para a rede local que a rede virtual do Azure usará para identificar recursos em sua rede local. Por fim, você escolherá qual região do Azure na qual deseja que sua rede virtual do Azure seja hospedada. Escolher um local que esteja fisicamente mais próximo à sua rede local é geralmente melhor para otimizar a velocidade da largura de banda para se comunicar com os recursos que você pode hospedar em seus serviços do Azure:

![Uma captura de tela mostrando a página Configurar rede virtual do Azure do assistente de integração com a rede virtual do Azure.](media/azure-virtual-network-4.PNG)

A última etapa do processo de integração é configurar o dispositivo VPN que será usado para a conexão VPN S2S. Como a maioria das pequenas empresas tem apenas alguns servidores em seu ambiente e não tem a equipe de ti configurar corretamente um roteador VPN para se conectar ao Microsoft Azure, a seleção padrão será configurar o servidor do Windows Server Essentials como o servidor VPN que os recursos em sua rede local se conectará ao para acessar recursos na rede virtual do Azure. No entanto, se você preferir usar outro servidor em seu ambiente como o servidor VPN, ou preferir usar um roteador VPN, poderá selecionar essas opções.

Devido à variação em modelos e tipos de roteadores, o Windows Server Essentials não tenta configurar automaticamente o roteador VPN. Selecionar o roteador VPN neste assistente de integração notifica apenas a rede virtual do Azure do tipo de dispositivo para as configurações de roteamento apropriadas necessárias no Azure para conectividade.

Após a conclusão do assistente de integração, uma nova guia ficará visível no painel do Windows Server Essentials para a rede virtual do Azure:

![Uma captura de tela mostrando a página VNet do Azure do painel do Windows Server Essentials. A guia rede virtual do Azure está selecionada e mostra o status como configurando.](media/azure-virtual-network-5.PNG)

>! Observação a conclusão da configuração de uma rede virtual do Azure na nuvem pode levar muito tempo, até 30 minutos. Durante esse tempo, o status da configuração estará presente na página status da rede virtual do Azure do painel.

Depois que a configuração da rede virtual do Azure for concluída, o status será alterado para conectado e mostrará os detalhes da sua rede virtual do Azure, como entrada/saída de dados, endereço IP do gateway, endereço IP local e detalhes da conta:

![Uma captura de tela mostrando a página VNet do Azure do painel do Windows Server Essentials. A guia rede virtual do Azure é selecionada e mostra o status como conectado e, nessas informações de status, os detalhes da rede virtual são exibidos.](media/azure-virtual-network-6.PNG)

No painel tarefas no lado direito do painel estão as várias tarefas que você pode executar com sua rede virtual do Azure.

-   **Desconectar da VNET do Azure** A configuração de uma rede virtual do Azure é gratuita, mas há um encargo para o gateway de VPN que se conecta ao local e a outros VNETs no Azure. A desconexão da VNET do Azure interrompe toda a cobrança.

-   **Alternar por dispositivo VPN** Caso você queira alterar de um servidor VPN para um roteador VPN, essa tarefa permitirá que você faça a alternância e notifique a VNET do Azure.

-   **Configurar VNET do Azure** Essa tarefa permite que você altere as opções de configuração avançadas da VNET do Azure redirecionando-as para a página de configuração de portal do Azure para VNET do Azure.

-   **Atualizar status** Atualiza a página de status, atualizar o status da conexão da VNET do Azure, incluindo entrada/saída de dados.

-   **Desabilitar integração VNET do Azure** Desconecta a VNET do Azure e remove a integração do painel do Windows Server Essentials. Observe que isso não exclui a VNET do Azure, as configurações ainda são preservadas no Azure se você quiser novamente integrar a VNET do Azure ao painel.

-   **Saiba mais sobre o VNET do Azure** [https://azure.microsoft.com/services/virtual-network/](https://azure.microsoft.com/services/virtual-network/).

<a name="see-also"></a>Veja também
--------
[Introdução ao Windows Server Essentials](get-started.md)
