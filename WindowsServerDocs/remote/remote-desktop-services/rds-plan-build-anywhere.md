---
title: Serviços de área de trabalho remota - compilação em qualquer lugar
description: Informações de planejamento para ajudá-lo a determinar onde hospedar sua implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: cbb8e73d753b1fe4f0293cf4427c634020a23a42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869507"
---
# <a name="remote-desktop-services---build-anywhere"></a>Serviços de área de trabalho remota - compilação em qualquer lugar

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Implante no local, na nuvem ou uma mistura de ambos. Modificar sua implantação conforme suas necessidades comerciais mudarem.

Independentemente de onde estão, subjacente [arquitetura](desktop-hosting-logical-architecture.md) dos serviços de área de trabalho remota ambiente permanece o mesmo:
- Você deve ter um servidor de Internet para utilizar o acesso via Web RD e o Gateway de área de trabalho remota para usuários externos
- Você deve ter um Active Directory e --para ambientes altamente disponíveis, um banco de dados SQL para o usuário de casa e propriedades de área de trabalho remota
- Você ainda deve ter acesso de comunicação entre as funções de infraestrutura de área de trabalho remota (agente de Conexão de área de trabalho remota, Gateway de área de trabalho remota, licenciamento de área de trabalho remota e acesso via Web RD) e a extremidade RDSH ou hosts RDVH para ser capaz de conectar os usuários finais a suas áreas de trabalho ou aplicativos.

Essa flexibilidade permite que você obtenha o melhor dos dois mundos:
- Os métodos de simplicidade e pago pelo uso associados com a nuvem e o mundo online.
- A familiaridade e sem complicações forma de aproveitamento de recursos pesados que já existem no local.

Para obter mais informações, veja como [compilar e implantar sua implantação de serviços de área de trabalho remota](rds-build-and-deploy.md).