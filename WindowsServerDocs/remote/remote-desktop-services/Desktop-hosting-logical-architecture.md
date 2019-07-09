---
title: Arquitetura de Serviços de Área de Trabalho Remota
description: Diagramas de arquitetura para RDS
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
ms.openlocfilehash: 14c32831445b12b314c71e538320c2ce5a9b7ee4
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63749440"
---
# <a name="remote-desktop-services-architecture"></a>Arquitetura de Serviços de Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Abaixo estão as várias configurações para a implantação de Serviços de Área de Trabalho Remota para hospedar áreas de trabalho e aplicativos do Windows para usuários finais.

>[!NOTE]
> Os diagramas de arquitetura abaixo mostram o uso de RDS no Azure. No entanto, é possível implantar Serviços de Área de Trabalho Remota no local e em outras nuvens. Esses diagramas destinam-se principalmente a ilustrar como as funções RDS são colocadas e usam outros serviços.

## <a name="standard-rds-deployment-architectures"></a>Arquiteturas de implantação de RDS padrão

Serviços de Área de Trabalho Remota tem duas arquiteturas padrão:
-   Implantação básica – Contém o número mínimo de servidores para criar um ambiente de RDS totalmente efetivo
-   Implantação altamente disponível – Contém todos os componentes necessários para ter o maior tempo de atividade garantido para o ambiente de RDS

### <a name="basic-deployment"></a>Implantação básica

![Implantação de RDS básica](./media/basic-rds.png)

### <a name="highly-available-deployment"></a>Implantação altamente disponível

![Implantação de RDS altamente disponível](./media/ha-rds.png)

## <a name="rds-architectures-with-unique-azure-paas-roles"></a>Arquiteturas de RDS com funções PaaS exclusivas do Azure

Embora as arquiteturas de implantação de RDS padrão se ajustem a maioria dos cenários, o Azure continua a investir em soluções de PaaS internas que geram valor ao cliente. Abaixo estão algumas arquiteturas mostrando como elas incorporam com RDS.

### <a name="rds-deployment-with-azure-ad-domain-services"></a>Implantação de RDS com o Azure AD Domain Services

Os dois diagramas de arquitetura padrão acima se baseiam em um AD (Active Directory) tradicional implantado em uma VM do Windows Server. No entanto, se você não tem um AD tradicional e tem apenas um locatário do Azure AD, por meio de serviços como o Office 365, mas ainda quer aproveitar o RDS, você pode usar o [Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) para criar um domínio totalmente gerenciado no seu ambiente de IaaS do Azure ambiente que usa os mesmos usuários existentes no locatário do Azure AD. Isso elimina a complexidade de sincronizar usuários manualmente e gerenciar mais máquinas virtuais. O Azure AD Domain Services pode trabalhar em qualquer implantação: básica ou altamente disponível.

![Implantação de RDS e Azure AD](./media/aadds-rds.png)

### <a name="rds-deployment-with-azure-ad-application-proxy"></a>Implantação de RDS com o Proxy de Aplicativo do Azure AD

Os dois diagramas de arquitetura padrão acima usam os servidores de Web/Gateway de Área de Trabalho Remota como o ponto de entrada para a Internet no sistema RDS. Em alguns ambientes, os administradores optariam por remover seus próprios servidores do perímetro e, em vez disso, usar tecnologias que também fornecem segurança adicional por meio de tecnologias de proxy reverso. A função PaaS do [Proxy de Aplicativo do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-get-started) se encaixa perfeitamente a esse cenário.

Para saber as configurações com suporte e como criar essa configuração, consulte como [publicar a Área de Trabalho Remota com o Proxy de Aplicativo do Azure AD](/azure/active-directory/application-proxy-publish-remote-desktop).

![RDS com Proxy de Aplicativo do Azure AD](./media/aadappproxy-rds.png)
