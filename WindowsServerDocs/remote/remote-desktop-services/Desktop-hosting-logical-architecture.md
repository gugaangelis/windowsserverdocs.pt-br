---
title: Arquitetura de Serviços de Área de Trabalho Remota
description: Diagramas de arquitetura de RDS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 02/10/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f73bb0a-ce98-48a4-9d9f-cf7438936ca1
author: lizap
manager: dongill
ms.openlocfilehash: ba597318cdaf1d4659a72905eeb4e252c9020e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887867"
---
# <a name="remote-desktop-services-architecture"></a>Arquitetura de Serviços de Área de Trabalho Remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Abaixo estão as várias configurações para a implantação de serviços de área de trabalho remota para hospedar aplicativos do Windows e áreas de trabalho para os usuários finais.

>[!NOTE]
> Os diagramas de arquitetura abaixo mostram usando RDS no Azure. No entanto, você pode implantar serviços de área de trabalho remota local e em outras nuvens. Esses diagramas destinam-se principalmente para ilustrar como as funções RDS são colocadas e usam outros serviços.

## <a name="standard-rds-deployment-architectures"></a>Arquiteturas de implantação do RDS padrão

Serviços de área de trabalho remota tem duas arquiteturas padrão:
-   Implantação básica – isso contém o número mínimo de servidores para criar um ambiente totalmente em vigor do RDS
-   Implantação de alta disponibilidade – isso contém todos os componentes necessários para que o maior tempo de atividade garantido para o seu ambiente de RDS

### <a name="basic-deployment"></a>Implantação básica

![Implantação de RDS básica](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>Implantação altamente disponível

![Implantação de RDS altamente disponível](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>Arquiteturas RDS com funções exclusivas de PaaS do Azure

Embora as arquiteturas de implantação do RDS padrão se ajustarem a maioria dos cenários, o Azure continua a investir em soluções de PaaS de primeira parte desse valor ao cliente unidade. Abaixo estão algumas arquiteturas mostrando como eles incorporar com RDS.

### <a name="rds-deployment-with-azure-ad-domain-services"></a>Implantação de RDS com o Azure AD Domain Services

Os dois diagramas de arquitetura padrão acima se baseiam em um tradicional AD (Active Directory) implantados em uma VM do Windows Server. No entanto, se você não tem um AD tradicional e ter apenas um locatário Azure AD — por meio de serviços, como o Office 365 — mas ainda desejam aproveitar o RDS, você pode usar [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) para criar um domínio totalmente gerenciado no seu IaaS do Azure ambiente que usa os mesmos usuários que existem em seu locatário do Azure AD. Isso remove a complexidade de manualmente a sincronização de usuários e gerenciamento mais máquinas virtuais. Azure AD Domain Services pode trabalhar em qualquer implantação: básico ou altamente disponível.

![Azure AD e a implantação do RDS](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>Implantação do RDS com o Proxy de aplicativo do Azure AD

Os dois diagramas de arquitetura padrão acima usam os servidores de Gateway/da Web da área de trabalho remota como o ponto de entrada para a Internet no sistema de RDS. Em alguns ambientes, os administradores preferem que remover seus próprios servidores do perímetro e em vez disso, usar tecnologias que também fornecem segurança adicional por meio de tecnologias de proxy reverso. O [Proxy de aplicativo do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) função de PaaS se encaixa perfeitamente com esse cenário.

Para configurações com suporte e como criar essa configuração, consulte como [publicar a área de trabalho remota com o Proxy de aplicativo do Azure AD](/azure/active-directory/application-proxy-publish-remote-desktop).

![RDS com o Proxy de aplicativo do Azure AD](./media/aadappproxy-rds.png)
