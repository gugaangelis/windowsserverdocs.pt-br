---
title: Integração de rede Virtual do Azure
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 673cb5a2292bab113aefb1de37f80bf4d880b467
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433898"
---
# <a name="azure-virtual-network-integration"></a>Integração de rede Virtual do Azure

>Aplica-se a: Windows Server 2016 Essentials

Conforme as organizações cheguem à computação em nuvem, raramente serão eles mover todos os seus recursos 100% ao mesmo tempo, mas em vez disso, adotar uma abordagem em que alguns recursos estão na nuvem e alguns ainda estão no local. Essa abordagem híbrida torna mais fácil para que as organizações não apenas para mover alguns recursos de computação na nuvem, mas também permite que eles aumentem sua infra-estrutura de TI sem precisar adquirir novo hardware.

Ao implementar essa abordagem híbrida à computação, uma maneira perfeita para os recursos em ambos os locais para se comunicar uns com os outros é necessária. A rede Virtual do Azure é um serviço do Azure que permite às organizações criar uma ponto a ponto (P2P) ou o site a site (S2S) rede virtual privada que faz com que os recursos que estão executando a aparência do Azure (por exemplo, máquinas virtuais e armazenamento), como se fossem na rede local para acesso contínuo de aplicativos e recursos.

Configuração de uma rede Virtual do Azure pode ser complexa. Com o Windows Server Essentials 2016, você pode configurar facilmente sua rede Virtual do Azure por meio de um assistente simples que ajuda você a escolher os padrões mais apropriados para seu ambiente de rede. Conforme mostrado na captura de tela abaixo, uma nova tarefa de integração de rede Virtual do Azure foi adicionada à seção do painel do Windows Essentials para introduzir a rede Virtual do Azure, bem como fornecer um link rápido para iniciar a integração de serviços de nuvem da Microsoft .

![Uma captura de tela mostrando a guia de Introdução na Home page do painel do Windows Server Essentials. Na guia de Introdução, a seção de serviços foi selecionada e o painel indica na integração de serviços de nuvem do Microsoft rede Virtual do Azure está desabilitada no momento.](media/azure-virtual-network-1.PNG)

Quando você clica o **integrar agora** vincular para a rede Virtual do Azure na captura de tela acima, uma caixa de diálogo será exibida solicitando que você faça logon sua conta do Microsoft Azure. Se você não tiver uma conta do Microsoft Azure, você terá a opção para inscrever-se para uma nessa tela, que redirecionará você para o portal de inscrição de conta do Azure:

![Uma captura de tela mostrando a página entrar no Microsoft Azure do Assistente de rede a integração com o Virtual do Azure.](media/azure-virtual-network-2.PNG)

Depois de entrar no Azure, você será apresentado com a opção de escolher qual assinatura que desejam associar ao Azure Virtual networking serviço:

![Uma captura de tela mostrando a página de minha assinatura do Microsoft Azure da integração com o Assistente de rede Virtual do Azure.](media/azure-virtual-network-3.PNG)

Depois de escolher qual assinatura do Azure que você deseja usar para a rede Virtual do Azure, você verá a opção de criar uma nova rede Virtual do Azure, ou se um já foi configurada sob essa assinatura, a caixa de lista suspensa mostrará está disponível. Você também escolherá um nome para a rede Local que a rede Virtual do Azure usará para identificar recursos em sua rede local. Por fim, você deverá escolher qual região do Azure no qual você deseja que sua rede Virtual do Azure para ser hospedado. Escolher um local que está fisicamente mais próximo de sua rede local geralmente é melhor para otimizar a velocidade de largura de banda para se comunicar com os recursos que você pode hospedar em seus serviços do Azure:

![Uma captura de tela mostrando a página de rede definir backup Virtual do Azure do Assistente de rede a integração com o Virtual do Azure.](media/azure-virtual-network-4.PNG)

A última etapa do processo de integração é configurar o dispositivo VPN que será usado para a conexão VPN S2S. Como a maioria das pequenas empresas têm somente alguns servidores em seu ambiente e não têm a equipe de TI para configurar corretamente o roteador VPN para se conectar ao Microsoft Azure, a seleção padrão será configurar o servidor Windows Server Essentials como um servidor VPN que recursos sua rede local conectará a fim de acessar recursos na rede Virtual do Azure. No entanto, se você preferir usar outro servidor em seu ambiente como um servidor VPN, ou você preferir usar um roteador VPN, você pode selecionar essas opções.

Devido à variação nos modelos e tipos de roteador, o Windows Server Essentials não tenta configurar automaticamente o roteador VPN. Apenas selecionar o roteador VPN neste assistente de integração notifica o sistema de rede Virtual do Azure do tipo de dispositivo para configurações de roteamento apropriados necessários no Azure para conectividade.

Após concluir o Assistente de integração, uma nova guia será visível no painel do Windows Server Essentials para a rede Virtual do Azure:

![Uma captura de tela mostrando a página de rede virtual do Azure do painel do Windows Server Essentials. Guia de rede Virtual do Azure está selecionada e mostra o status como configurando.](media/azure-virtual-network-5.PNG)

>! Observação para concluir a configuração de uma rede Virtual do Azure na nuvem pode levar muito tempo, para cima para 30 minutos. Durante esse tempo, o status de configuração será presente na página de status de rede Virtual do Azure do painel.

Depois que a configuração da rede Virtual do Azure for concluída, o status será alterado para conectado e mostre os detalhes de sua rede Virtual do Azure, como dados de entrada/saída, o endereço IP do gateway, o local endereço IP e conta de detalhes:

![Uma captura de tela mostrando a página de rede virtual do Azure do painel do Windows Server Essentials. Guia de rede Virtual do Azure está selecionada e mostra o status como conectado e sob essas informações de status os detalhes da rede virtual são exibidos.](media/azure-virtual-network-6.PNG)

No painel de tarefas no lado direito do painel são as várias tarefas que você pode realizar com sua rede Virtual do Azure.

-   **Desconexão da VNET do Azure** Configurando uma rede Virtual do Azure é gratuito, mas há um encargo para o gateway VPN que se conecta a fontes locais e outras redes virtuais no Azure. Desconectando a rede virtual do Azure interrompe toda a cobrança.

-   **Alterne sobre o dispositivo de VPN** que você deseja alterar de um servidor VPN para o roteador VPN, esta tarefa permitirá a você fazer a transição e notificar a rede virtual do Azure.

-   **Configurar a rede virtual do Azure** essa tarefa permite que você altere as opções de configuração avançada de rede virtual do Azure, redirecionando-los para a página de configuração do portal do Azure para rede virtual do Azure.

-   **Atualizar Status** atualiza a página de status, atualizar o status de conexão de rede virtual do Azure, incluindo dados de entrada/saída.

-   **Desabilitar a integração de rede virtual do Azure** desconecta da rede virtual do Azure e remove a integração do painel do Windows Server Essentials. Observe que isso não exclui a rede virtual do Azure, as configurações ainda são preservadas no Azure, se você quiser mais tarde reintegrar rede virtual do Azure com o painel.

-   **Saiba mais sobre rede virtual do Azure** [ https://azure.microsoft.com/services/virtual-network/ ](https://azure.microsoft.com/services/virtual-network/).

<a name="see-also"></a>Consulte também
--------
[Introdução ao Windows Server Essentials](get-started.md)
