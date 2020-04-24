---
title: Arquitetura de referência de hospedagem de área de trabalho
description: Diretrizes de arquitetura para criação de uma solução de hospedagem de área de trabalho com o RDS e o Azure.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: c2bc0c2ba3d12ea1caf8737369ba882f69b111e4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80818449"
---
# <a name="desktop-hosting-reference-architecture"></a>Arquitetura de referência de hospedagem de área de trabalho

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Este artigo define um conjunto de blocos arquitetônicos para usar RDS (Serviços de Área de Trabalho Remota) e máquinas virtuais do Microsoft Azure para criar serviços de aplicativos e áreas de trabalho hospedadas de multilocatário do Windows, o que chamamos de "hospedagem de área de trabalho". É possível usar essa referência de arquitetura para criar soluções de hospedagem de área de trabalho altamente seguras, escalonáveis e confiáveis para empresas de pequeno e médio porte com 5 a 5000 usuários.    
  
O público-alvo primário dessa arquitetura de referência são provedores de hospedagem que desejam aproveitar os Serviços de Infraestrutura do Microsoft Azure para fornecer serviços de hospedagem de área de trabalho e SALs (Licenças de Acesso ao Assinante) para vários locatários por meio do programa [SPLA (Contrato de Licenciamento do Provedor de Serviços) da Microsoft](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx). Um público secundário para essa arquitetura de referência são clientes finais que desejam criar e gerenciar soluções de hospedagem da área de trabalho nos Serviços de Infraestrutura do Microsoft Azure para seus próprios funcionários usando [direitos estendidos de CALs de Usuário RDS por meio do SA (Software Assurance)](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).   
  
Para fornecer soluções de hospedagem, os parceiros de hospedagem e os clientes SA aproveitam o Windows Server para oferecer aos usuários do Windows uma experiência de aplicativo que seja familiar aos usuários comerciais e consumidores. Criado com base no Windows 10, o Windows Server 2016 oferece suporte a aplicativos e experiência de usuário familiares.    
  
O escopo deste documento é limitado a:   
  
* Diretrizes de design de arquitetura para um serviço de hospedagem de área de trabalho. Informações detalhadas, como procedimentos de implantação, desempenho e planejamento da capacidade são explicadas em documentos separados. Para obter mais informações sobre Serviços de Infraestrutura do Azure, consulte [Máquinas virtuais do Microsoft Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).   
  
* Áreas de trabalho baseadas em sessão, aplicativos RemoteApp e áreas de trabalho pessoais baseadas em servidor que usam Host da Sessão RD (Host da Sessão de Área de Trabalho Remota) do Windows Server 2016. Infraestruturas de área de trabalho virtual baseadas no cliente Windows não são cobertos porque não há nenhum SPLA (Contrato de Licença do Provedor de Serviço) para sistemas operacionais de cliente Windows. Infraestruturas de área de trabalho virtual baseadas no Windows Server são permitidas sob o SPLA e infraestruturas de área de trabalho virtual baseadas no cliente Windows são permitidas em hardware dedicado com licenças de cliente final em determinados cenários. No entanto, infraestruturas de área de trabalho virtual baseadas em cliente estão fora do escopo deste documento.   
  
* Produtos e recursos da Microsoft, principalmente o Windows Server 2016 e os Serviços de Infraestrutura do Microsoft Azure.   
  
* Serviços de hospedagem de área de trabalho para locatários com tamanho variando de 5 até 5.000 usuários.   Para locatários maiores, talvez seja necessário modificar essa arquitetura para fornecer um desempenho adequado. A GUI (interface gráfica do usuário) do RDS do Gerenciador de Servidores não é recomendada para implantações com mais de 500 usuários. PowerShell é recomendado para gerenciar implantações de RDS entre 500 e 5000 usuários.   
  
* O conjunto mínimo de componentes e serviços exigidos para um serviço de hospedagem de área de trabalho. Há vários componentes e serviços opcionais que podem ser adicionados para melhorar um serviço de hospedagem de área de trabalho, mas eles estão fora do escopo deste documento.    
  
Depois de ler este documento, o leitor deve compreender:   
- Os blocos de construção que são necessários para fornecer uma solução de hospedagem de área de trabalho segura, confiável e de multilocatário baseada nos Serviços do Microsoft Azure.  
- A finalidade de cada bloco de construção e como eles se encaixam.  
  
Há várias maneiras de criar uma solução de hospedagem de área de trabalho com base nessa arquitetura. Essa arquitetura descreve a integração e as melhorias no Azure com o Windows Server 2016. Outras opções de implantação estão disponíveis com o [Guia de arquitetura de referência de hospedagem de área de trabalho](https://go.microsoft.com/fwlink/p/?LinkId=517389) para Windows Server 2012 R2.    
  
Os seguintes tópicos são abordados:  
- [Arquitetura lógica de hospedagem de área de trabalho](Desktop-hosting-logical-architecture.md)  
- [Noções básicas sobre as funções RDS](Understanding-RDS-roles.md)
- [Noções básicas sobre o ambiente de hospedagem de área de trabalho](Understanding-the-desktop-hosting-environment.md)  
- [Serviços do Azure e considerações para a hospedagem de área de trabalho](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


