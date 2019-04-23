---
title: Arquitetura de referência de hospedagem de área de trabalho
description: Diretrizes de arquitetura para a criação de uma solução com o RDS e o Azure de hospedagem de área de trabalho.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: 6f235fd89c34c00601c802f4ea71e440af630169
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890237"
---
# <a name="desktop-hosting-reference-architecture"></a>Arquitetura de referência de hospedagem de área de trabalho

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este artigo define um conjunto de blocos arquitetônicos para usar os serviços de área de trabalho remota (RDS) e máquinas virtuais do Microsoft Azure para criar multilocatário, hospedados área de trabalho do Windows e o aplicativo serviços, o que chamamos de "hospedagem da área de trabalho". Você pode usar essa referência de arquitetura para criar soluções para empresas de pequeno e médio porte com usuários de 5 a 5000 de hospedagem de desktop altamente seguro, escalonável e confiável.    
  
O principal público-alvo para essa arquitetura de referência é provedores de hospedagem que desejam aproveitar os serviços de infraestrutura do Microsoft Azure para fornecer serviços de hospedagem da área de trabalho e licenças de acesso de assinante (SALs) para vários locatários por meio de [ O contrato de licença de provedor do Microsoft Service](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx) programa (SPLA). Um segundo público para essa arquitetura de referência são clientes finais que desejam criar e gerenciar soluções de hospedagem da área de trabalho nos serviços de infraestrutura do Microsoft Azure para seus próprios funcionários usando [RDS CALs de usuário estendido direitos por meio do Software Garantia](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf) (SA).   
  
Para fornecer uma hospedagem de soluções, hospedagem de área de trabalho parceiros e clientes do SA aproveitam o Windows Server para oferecer uma experiência de aplicativo que é familiar aos usuários comerciais e consumidores de usuários do Windows. Criado nos fundamentos do Windows 10, Windows Server 2016 fornece suporte a aplicativos familiares e o usuário enfrentar.    
  
O escopo deste documento é limitado a:   
  
* Diretrizes de design de arquitetura para uma serviço de hospedagem de área de trabalho. Informações detalhadas, como planejamento de capacidade, desempenho e procedimentos de implantação são explicadas em documentos separados. Para obter mais informações sobre serviços de infraestrutura do Azure, consulte [máquinas virtuais do Microsoft Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).   
  
* Áreas de trabalho baseadas em sessão, os aplicativos do RemoteApp e desktops pessoais baseada em servidor que usam o Windows Server 2016 Desktop Host da sessão remota (Host de sessão de área de trabalho remota). Windows com base no cliente da área de trabalho infraestruturas virtuais não são cobertos porque não há nenhum contrato de licença de provedor (SPLA) da serviço para sistemas operacionais Windows. Infraestruturas de área de trabalho virtual baseada em servidor do Windows é permitido sob o SPLA e Windows baseados em cliente da área de trabalho infraestruturas virtuais são permitidas em hardware dedicado com licenças de cliente final em determinados cenários. No entanto, infra-estruturas de desktop virtuais baseada em cliente estão fora do escopo deste documento.   
  
* Produtos e recursos, principalmente o Windows Server 2016 e os serviços de infraestrutura do Microsoft Azure.   
  
* Hospedagem de serviços para locatários que variam em tamanho de 5 até 5.000 usuários de área de trabalho.   Para locatários maiores, talvez você precise modificar essa arquitetura para fornecer um desempenho adequado. A interface gráfica do usuário do Gerenciador do servidor RDS (GUI) não é recomendada para implantações mais de 500 usuários. PowerShell é recomendado para gerenciar implantações de RDS entre 500 e 5000 usuários.   
  
* O conjunto mínimo de componentes e serviços necessários para uma serviço de hospedagem de área de trabalho. Há vários componentes opcionais e serviços que podem ser adicionados para melhorar a uma serviço de hospedagem de área de trabalho, mas eles estão fora do escopo deste documento.    
  
Depois de ler este documento, o leitor deve compreender:   
- Os blocos de construção que são necessários para fornecer uma área de trabalho segura, confiável e multilocatária solução de hospedagem baseada em serviços do Microsoft Azure.  
- A finalidade de cada bloco de construção e como elas se encaixam.  
  
Há várias maneiras de criar uma solução baseada nessa arquitetura de hospedagem de área de trabalho. Essa arquitetura descreve a integração e melhorias no Azure com o Windows Server 2016. Outras opções de implantação estão disponíveis com o [guia de arquitetura de referência de hospedagem de área de trabalho](https://go.microsoft.com/fwlink/p/?LinkId=517389) para Windows Server 2012 R2.    
  
Os seguintes tópicos são abordados:  
- [Arquitetura lógica de hospedagem da área de trabalho](Desktop-hosting-logical-architecture.md)  
- [Entender as funções RDS](Understanding-RDS-roles.md)
- [Compreender o ambiente de hospedagem da área de trabalho](Understanding-the-desktop-hosting-environment.md)  
- [Considerações sobre a hospedagem de área de trabalho e serviços do Azure](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


