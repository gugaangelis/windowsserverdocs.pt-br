---
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: Planejamento e Design do AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 24fc96aae268d9847bd3b32460ada33d2c6e0d65
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848687"
---
# <a name="ad-ds-design-and-planning"></a>Planejamento e Design do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Implantando o Windows Server Active Directory domínio Services (AD DS) em seu ambiente, você pode tirar proveito do modelo administrativo centralizado e delegado e único (SSO) recurso de logon que o AD DS fornece. Depois de identificar as tarefas de implantação e o ambiente atual para sua organização, você pode criar a estratégia de implantação do AD DS que atenda às necessidades da sua organização.  
  
## <a name="about-this-guide"></a>Sobre este guia

Este guia fornece recomendações para ajudá-lo a desenvolver uma estratégia de implantação do AD DS com base nos requisitos da sua organização e no design específico que você deseja criar. Este guia destina-se ao uso por especialistas em infraestrutura ou arquitetos de sistema. Antes de ler este guia, você deve ter uma boa compreensão do funcionamento do AD DS em um nível funcional. Você também deve ter uma boa compreensão dos requisitos organizacionais que serão refletidas em sua estratégia de implantação do AD DS.  
  
Este guia descreve os conjuntos de tarefas para vários pontos de partida possíveis de uma implantação do AD DS do Windows Server 2008. O guia ajuda você a determinar a estratégia de implantação mais adequada para seu ambiente.  
  
Embora as estratégias que são apresentadas neste guia são apropriadas para quase todas as implantações de sistema operacional de servidor, elas foram testadas e validadas especificamente para ambientes que contêm menos de 100.000 usuários e menos de 1.000 sites, com conexões de rede de um mínimo de 28,8 Kbps por segundo (Kbps). Se seu ambiente não atender a esses critérios, considere usar uma firma de consultoria com experiência na implantação do AD DS em ambientes mais complexos.  
  
Para obter mais informações sobre como testar o processo de implantação do AD DS, consulte o artigo [Testando e verificando o processo de implantação](https://go.microsoft.com/fwlink/?LinkId=100206).  
  
## <a name="in-this-guide"></a>Neste guia

[Noções básicas sobre o Design do AD DS](Understanding-AD-DS-Design.md)  
  
[Identificar seus requisitos de implantação e Design do AD DS](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)  
  
[Mapear seus requisitos para uma estratégia de implantação do AD DS](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)  
  
[Criando a estrutura lógica para o Windows Server 2008 AD DS](Designing-the-Logical-Structure.md)  
  
[Projetando a topologia de Site para o Windows Server 2008 AD DS](Designing-the-Site-Topology.md)  
  
[Habilitar recursos avançados para AD DS](Enabling-Advanced-Features-for-AD-DS.md)  
  
[Avaliando exemplos de estratégia de implantação do AD DS](Evaluating-AD-DS-Deployment-Strategy-Examples.md)  
  
[Apêndice a: Revisar os termos do chave do AD DS](Appendix-A--Reviewing-Key-AD-DS-Terms.md)  
