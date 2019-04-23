---
title: Serviços do Azure e considerações para a hospedagem de área de trabalho
description: Saiba mais sobre as considerações de exclusivas para o Azure com uma área de trabalho remota, solução de hospedagem.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: dougkim
ms.openlocfilehash: 37210a5d75399309c53364f5b8ee9e06d26d6f32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849797"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Serviços do Azure e considerações para a hospedagem de área de trabalho

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

As seções a seguir descrevem os serviços de infraestrutura do Azure.
  
## <a name="azure-portal"></a>Portal do Azure

Depois que o provedor cria uma assinatura do Azure, o portal do Azure pode ser usado para criar manualmente o ambiente de cada locatário. Esse processo também pode ser automatizado usando scripts do PowerShell.  

Para obter mais informações, visite o [Microsoft Azure](https://www.azure.microsoft.com) site.
  
## <a name="azure-load-balancer"></a>Azure Load Balancer

Componentes do locatário executados em máquinas virtuais que se comunicam entre si em uma rede isolada. Durante o processo de implantação, você pode acessar externamente essas máquinas virtuais por meio do balanceador de carga do Azure usando pontos de extremidade do protocolo de área de trabalho remota ou um ponto de extremidade remoto do PowerShell. Quando uma implantação for concluída, esses pontos de extremidade normalmente serão excluídos para reduzir a área de superfície de ataque. Os pontos de extremidade somente serão os pontos de extremidade HTTPS e UDP criados para a máquina virtual executando a Web da área de trabalho remota e os componentes do Gateway de área de trabalho remota. Isso permite que os clientes na internet para se conectar a sessões em execução no serviço de hospedagem da área de trabalho do locatário. Se um usuário abre um aplicativo que se conecta à internet, como um navegador da web, as conexões serão passadas por meio do balanceador de carga do Azure.  
  
Para obter mais informações, consulte [o que é o balanceador de carga do Azure?](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>Considerações sobre segurança

Este guia do Azure da área de trabalho de hospedagem referência arquitetura foi projetado para fornecer um ambiente altamente seguro e isolado para cada locatário. Segurança do sistema também depende de garantias executadas pelo provedor durante a implantação e operação do serviço hospedado. A lista a seguir descreve algumas considerações sobre que o provedor deve tomar para manter sua solução de hospedagem da área de trabalho com base nessa arquitetura de referência segura.

- Todas as senhas administrativas devem ser fortes, idealmente aleatoriamente geradas, alterado com frequência e salvo em um local central seguro somente acessível aos poucos administradores de provedor select.  
- Ao replicar o ambiente de locatário para novos locatários, evite usar senhas administrativas mesmas ou fracas.
- A URL do site de acesso via Web RD, nome e certificados devem ser reconhecível para cada locatário para evitar ataques de falsificação e exclusivos.  
- Durante a operação normal do serviço de hospedagem da área de trabalho, todos os endereços IP públicos devem ser excluídos para todas as máquinas virtuais, exceto a máquina virtual Web da área de trabalho remota e Gateway de área de trabalho remota que permite aos usuários a se conectar com segurança para o serviço nuvem de hospedagem da área de trabalho do locatário. Endereços IP públicos podem ser adicionados temporariamente quando necessário para tarefas de gerenciamento, mas sempre devem ser excluídos posteriormente.  
  
Para obter mais informações, consulte os seguintes artigos:

- [Segurança e proteção](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [Práticas recomendadas de segurança para o IIS 8](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Proteger o Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>Considerações de design

É importante levar em consideração as restrições dos serviços de infraestrutura do Microsoft Azure durante a criação de uma área de trabalho multilocatária, serviço de hospedagem. A lista a seguir descreve as considerações sobre que o provedor deve tomar para realizar uma funcional e econômica da área de trabalho solução de hospedagem com base nessa arquitetura de referência.  
  
- Uma assinatura do Azure tem um número máximo de redes virtuais, núcleos de VMs e serviços de nuvem que pode ser usado. Se um provedor precisa de mais recursos do que isso, precisará criar várias assinaturas.
- Um serviço de nuvem do Azure tem um número máximo de máquinas virtuais que podem ser usados. O provedor talvez precise criar vários serviços de nuvem para locatários maiores que excedem o máximo.  
- Os custos de implantação do Azure baseiam-se parcialmente o número e tamanho das máquinas virtuais. O provedor deve otimizar o número e tamanho das máquinas virtuais para cada locatário para fornecer um funcional e altamente seguro o ambiente de hospedagem de área de trabalho com o menor custo.  
- Os recursos de computador físico no data center do Azure são virtualizados usando o Hyper-V. Hosts Hyper-V não estão configuradas em clusters de host, portanto, a disponibilidade das máquinas virtuais é dependente da disponibilidade dos servidores individuais usados na infraestrutura do Azure. Para fornecer maior disponibilidade, várias instâncias de cada máquina de virtual do serviço de função podem ser criadas em um conjunto de disponibilidade, em seguida, o clustering de convidado pode ser implementado dentro das máquinas virtuais.  
- Em uma configuração típica de armazenamento, um provedor de serviços terá uma única conta de armazenamento com vários contêineres (por exemplo, um para cada Locatário) e vários discos de dentro de cada contêiner. No entanto, há um limite para o armazenamento total e o desempenho pode ser obtido para uma única conta de armazenamento. Para provedores de serviços que dão suporte a grandes números de locatários ou locatários com requisitos de armazenamento de alta capacidade ou desempenho, o provedor de serviços talvez precise criar várias contas de armazenamento.  
  
Para obter mais informações, consulte os seguintes artigos:

- [Tamanhos para serviços de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Máquina de virtual do Microsoft Azure os detalhes de preços](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Visão geral do Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Destinos de escalabilidade e desempenho de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Proxy de aplicativo do Azure Active Directory

Proxy de aplicativo do Azure Active Directory (AD) é um serviço fornecido em SKUs do Azure AD pagos que permitem que os usuários se conectem a aplicativos internos por meio do serviço de proxy reverso do Azure. Isso permite que os pontos de extremidade da Web da área de trabalho remota e Gateway de área de trabalho remota a ser escondido dentro da rede virtual, eliminando a necessidade de ser exposta à internet por um endereço IP público. Hosters podem usar o Proxy de aplicativo do Azure AD para condensar o número de máquinas virtuais no ambiente do locatário e ainda manter uma implantação completa. O Proxy de aplicativo do Azure AD também permite que muitos dos benefícios que o AD do Azure fornece, como acesso condicional e autenticação multifator.

Para obter mais informações, consulte [Introdução ao Proxy de aplicativo e instalar o conector](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable).