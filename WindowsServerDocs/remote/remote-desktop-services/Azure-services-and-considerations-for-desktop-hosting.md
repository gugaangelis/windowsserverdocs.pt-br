---
title: Serviços do Azure e considerações para a hospedagem de área de trabalho
description: Saiba mais sobre as considerações exclusivas para o Azure com uma solução de hospedagem de Área de Trabalho Remota.
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: lizross
ms.openlocfilehash: b6730b2294fec1c657c7990b1dd03bfedd1f5da8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966223"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Serviços do Azure e considerações para a hospedagem de área de trabalho

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

As seções a seguir descrevem os Serviços de Infraestrutura do Azure.

## <a name="azure-portal"></a>Portal do Azure

Depois que o provedor cria uma assinatura do Azure, é possível usar o portal do Azure para criar manualmente o ambiente de cada locatário. Esse processo também pode ser automatizado usando scripts do PowerShell.

Para obter mais informações, visite o site da Web [Microsoft Azure](https://www.azure.microsoft.com).

## <a name="azure-load-balancer"></a>Azure Load Balancer

Os componentes do locatário são executados em máquinas virtuais que se comunicam entre si em uma rede isolada. Durante o processo de implantação, é possível acessar externamente essas máquinas virtuais por meio do Azure Load Balancer usando pontos de extremidade do protocolo RDP ou um ponto de extremidade do PowerShell Remoto. Quando uma implantação for concluída, esses pontos de extremidade normalmente serão excluídos para reduzir a área da superfície de ataque. Os únicos pontos de extremidade serão os pontos de extremidade HTTPS e UDP criados para a máquina virtual executando os componentes da Web da Área de Trabalho Remota e do Gateway de Área de Trabalho Remota. Isso permite que os clientes na Internet conectem às sessões em execução no serviço de hospedagem de área de trabalho do locatário. Se um usuário abre um aplicativo que conecta à Internet, como um navegador da Web, as conexões serão passadas pelo Azure Load Balancer.

Para obter mais informações, consulte [O que é o Azure Load Balancer?](/azure/load-balancer/load-balancer-overview)

## <a name="security-considerations"></a>Considerações de segurança

Este Guia de Arquitetura de Referência de Hospedagem de Área de Trabalho do Azure serve para fornecer um ambiente altamente seguro e isolado para cada locatário. A segurança do sistema também depende das medidas de proteção tomadas pelo provedor durante a implantação e a operação do serviço hospedado. A lista a seguir descreve algumas considerações que o provedor deve levar em conta para manter segura a solução de hospedagem de área de trabalho com base nessa arquitetura de referência.

- Todas as senhas administrativas devem ser fortes, de preferência geradas aleatoriamente, alteradas com frequência e salvas em um local centralizado seguro que seja acessível a administradores de provedor especialmente selecionados.
- Ao replicar o ambiente de locatário para novos locatários, evite usar senhas administrativas iguais ou fracas.
- A URL do site de Acesso via Web à Área de Trabalho Remota, nome e certificados devem ser exclusivos e reconhecíveis para cada locatário de modo a prevenir ataques de falsificação.
- Durante a operação normal do serviço de hospedagem da área de trabalho, todos os endereços IP públicos devem ser excluídos de todas as máquinas virtuais, exceto a máquina virtual da Web da Área de Trabalho Remota e do Gateway de Área de Trabalho Remota, as quais permitem aos usuários conectar com segurança ao serviço de nuvem de hospedagem de área de trabalho do locatário. Endereços IP públicos podem ser adicionados temporariamente quando necessário para tarefas de gerenciamento, mas sempre devem ser excluídos posteriormente.

Para obter mais informações, confira os seguintes artigos:

- [Segurança e proteção](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831778(v=ws.11))
- [Práticas recomendadas de segurança para IIS 8](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj635855(v=ws.11))
- [Proteger o Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831360(v=ws.11))

## <a name="design-considerations"></a>Consideração sobre design

É importante levar em consideração as restrições dos Serviços de Infraestrutura do Microsoft Azure durante a criação de um serviço de hospedagem de área de trabalho de multilocatário. A lista a seguir descreve algumas considerações que o provedor deve levar em conta para obter uma solução de hospedagem de área de trabalho funcional e econômica com base nessa arquitetura de referência.

- Uma assinatura do Azure tem um número máximo de redes virtuais, núcleos de VM e Serviços de Nuvem que pode ser usado. Se um provedor necessitar de mais recursos do que isso, ele precisará criar várias assinaturas.
- Um Serviço de Nuvem do Azure tem um número máximo de máquinas virtuais que podem ser usadas. O provedor talvez precise criar vários Serviços de Nuvem para locatários maiores que excedam o máximo.
- Os custos de implantação do Azure baseiam-se parcialmente no número e tamanho das máquinas virtuais. O provedor deve otimizar o número e tamanho das máquinas virtuais para cada locatário, visando fornecer um ambiente de hospedagem de área de trabalho funcional e altamente seguro com o menor custo.
- Os recursos de computadores físicos no data center do Azure são virtualizados usando o Hyper-V. Os hosts Hyper-V não são configurados em clusters de hosts, portanto, a disponibilidade das máquinas virtuais é dependente da disponibilidade dos servidores individuais usados na infraestrutura do Azure. Para fornecer maior disponibilidade, várias instâncias de cada máquina virtual do serviço de função podem ser criadas em um conjunto de disponibilidade e, em seguida, o clustering de convidado pode ser implementado dentro das máquinas virtuais.
- Em uma configuração de armazenamento típica, um provedor de serviços terá uma única conta de armazenamento com vários contêineres (por exemplo, um para cada locatário) e vários discos dentro de cada contêiner. No entanto, há um limite para o armazenamento total e o desempenho que pode ser alcançado para uma única conta de armazenamento. Para provedores de serviços que dão suporte a grandes números de locatários ou locatários com requisitos de alta capacidade de armazenamento ou alto desempenho, o provedor de serviços talvez precise criar várias contas de armazenamento.

Para obter mais informações, confira os seguintes artigos:

- [Tamanhos para Serviços de Nuvem](/azure/cloud-services/cloud-services-sizes-specs)
- [Detalhes de preços de máquinas de virtuais do Microsoft Azure](https://azure.microsoft.com/pricing/details/virtual-machines/)
- [Visão geral do Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v=ws.11))
- [Metas de desempenho e escalabilidade do Armazenamento do Azure](/azure/storage/common/storage-scalability-targets)

## <a name="azure-active-directory-application-proxy"></a>Proxy de Aplicativo do Azure Active Directory

O Proxy de Aplicativo do Azure AD (Active Directory) é um serviço fornecido em SKUs pagas do Azure AD que permite aos usuários conectarem a aplicativos internos por meio do serviço de proxy reverso do próprio Azure. Isso permite que os pontos de extremidade da Web da Área de Trabalho Remota e do Gateway de Área de Trabalho Remota sejam ocultados dentro da rede virtual, eliminando a necessidade de serem expostos à internet por meio de um endereço IP público. Os hosters podem usar o Proxy de Aplicativo do Azure AD para condensar o número de máquinas virtuais no ambiente do locatário, enquanto continuam mantendo uma implantação completa. O Proxy de Aplicativo do Azure AD também oferece muitos dos benefícios que o AD do Azure fornece, como acesso condicional e autenticação multifator.

Para obter mais informações, consulte [Introdução ao Proxy de Aplicativo e instalação do conector](/azure/active-directory/manage-apps/application-proxy-enable).
