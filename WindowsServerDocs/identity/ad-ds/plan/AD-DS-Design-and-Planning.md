---
ms.assetid: a91339ef-6ad4-445f-8ecc-a95fbcc04296
title: Planejamento e Design do AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: b735852f8e2428c3d6375a3168f204e403a461f5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941452"
---
# <a name="ad-ds-design-and-planning"></a>Planejamento e Design do AD DS

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ao implantar o Windows Server Active Directory Domain Services (AD DS) em seu ambiente, você pode tirar proveito do recurso de logon único e delegado centralizado e de SSO que o AD DS fornece. Depois de identificar as tarefas de implantação e o ambiente atual da sua organização, você pode criar a estratégia de implantação AD DS que atende às necessidades da sua organização.

## <a name="about-this-guide"></a>Sobre este guia

Este guia fornece recomendações para ajudá-lo a desenvolver uma estratégia de implantação AD DS com base nos requisitos de sua organização e no design específico que você deseja criar. Este guia destina-se a especialistas em infraestrutura ou arquitetos de sistema. Antes de ler este guia, você deve ter uma boa compreensão de como AD DS funciona em um nível funcional. Você também deve ter uma boa compreensão dos requisitos organizacionais que serão refletidos em sua estratégia de implantação AD DS.

Este guia descreve conjuntos de tarefas para vários pontos de partida possíveis de uma implantação do Windows Server 2008 AD DS. O guia o ajuda a determinar a estratégia de implantação mais adequada para seu ambiente.

Embora as estratégias apresentadas neste guia sejam apropriadas para quase todas as implantações de sistema operacional do servidor, elas foram testadas e validadas especificamente para ambientes que contêm menos de 100.000 usuários e menos de 1.000 sites, com conexões de rede de um mínimo de 28,8 kilobits por segundo (Kbps). Se o seu ambiente não atender a esses critérios, considere o uso de uma empresa de consultoria que tenha experiência com a implantação de AD DS em ambientes mais complexos.

Para obter mais informações sobre como testar o processo de implantação de AD DS, consulte o artigo [testando e verificando o processo de implantação](/previous-versions/windows/it-pro/windows-server-2003/cc772722(v=ws.10)).

## <a name="in-this-guide"></a>Neste guia

[Noções básicas do design do AD DS](Understanding-AD-DS-Design.md)

[Como identificar seus requisitos de design e implantação do AD DS](Identifying-Your-AD-DS-Design-and-Deployment-Requirements.md)

[Como mapear seus requisitos para uma estratégia de implantação do AD DS](Mapping-Your-Requirements-to-an-AD-DS-Deployment-Strategy.md)

[Criando a estrutura lógica para o Windows Server 2008 AD DS](Designing-the-Logical-Structure.md)

[Criando a topologia do site para o Windows Server 2008 AD DS](Designing-the-Site-Topology.md)

[Como habilitar recursos avançados para AD DS](Enabling-Advanced-Features-for-AD-DS.md)

[Como avaliar exemplos de estratégia de implantação do AD DS](Evaluating-AD-DS-Deployment-Strategy-Examples.md)

[Apêndice A: como examinar os principais termos do AD DS](Appendix-A--Reviewing-Key-AD-DS-Terms.md)
