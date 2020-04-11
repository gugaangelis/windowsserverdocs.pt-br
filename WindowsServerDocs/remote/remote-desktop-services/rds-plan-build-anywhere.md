---
title: Serviços de Área de Trabalho Remota – Compilar em qualquer lugar
description: Informações de planejamento para ajudá-lo a determinar onde hospedar sua implantação do RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: c803a383-0eea-4e11-bca5-d204ab758048
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 9b56614e347b36fb86e6e4680f1b179accaef058
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857369"
---
# <a name="remote-desktop-services---build-anywhere"></a>Serviços de Área de Trabalho Remota – Compilar em qualquer lugar

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Implante localmente, na nuvem ou uma combinação de ambos. Modifique sua implantação conforme suas necessidades empresariais mudam.

Onde quer que você esteja, a [arquitetura](desktop-hosting-logical-architecture.md) subjacente do ambiente dos Serviços de Área de Trabalho Remota permanece o mesmo:
- Você ainda precisa ter um servidor voltado para a Internet para utilizar o Acesso via Web da Área de Trabalho Remota e o Gateway de Área de Trabalho Remota para usuários externos
- Você ainda precisa ter um Active Directory e, para ambientes altamente disponíveis, um Banco de Dados SQL para conter propriedades de Área de Trabalho Remota
- Você ainda precisa ter acesso de comunicação entre as funções de infraestrutura de RD (Agente de Conexão de Área de Trabalho Remota, Gateway de Área de Trabalho Remota, Licenciamento de Área de Trabalho Remota e Acesso via Web da Área de Trabalho Remota) e os hosts finais de RDSH ou RDVH para conseguir conectar os usuários finais a suas áreas de trabalho ou aplicativos.

Essa flexibilidade permite que você obtenha o melhor dos dois mundos:
- A simplicidade os métodos pagos conforme o uso associados à nuvem e ao mundo online.
- A familiaridade e a forma descomplicada de aproveitar recursos pesados que já existem localmente.

Para obter mais informações, confira [Crie e implante sua implantação de Serviços de Área de Trabalho Remota](rds-build-and-deploy.md).