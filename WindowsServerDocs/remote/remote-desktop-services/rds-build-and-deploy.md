---
title: RDS – criar e implantar
description: Etapas para criar uma implantação de Área de Trabalho Remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 04/18/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 176ae424-96e9-4c78-88f5-da418e76c3d7
author: lizap
manager: dongill
ms.openlocfilehash: 309ea068488d005eabfe22f8ea055f85dd098452
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66453076"
---
# <a name="build-and-deploy-your-remote-desktop-services-deployment"></a>Crie e implante sua implantação de Serviços de Área de Trabalho Remota

Uma implantação de Serviços de Área de Trabalho Remota é a infraestrutura usada para compartilhar aplicativos e recursos com seus usuários. Dependendo da experiência que desejar oferecer, você poderá tornar essa infraestrutura tão pequena ou complexa quanto for necessário. Implantações de Área de Trabalho Remota são dimensionadas com facilidade. Você poderá aumentar e diminuir o Acesso via Web à Área de Trabalho Remota, o Gateway, o Agente de Conexão e os servidores Host da Sessão como quiser. Você pode usar o Agente de Conexão de Área de Trabalho Remota para distribuir cargas de trabalho. A autenticação com base no Active Directory fornece um ambiente altamente seguro. 

[Os clientes da Área de Trabalho Remota](clients/remote-desktop-clients.md) habilitam o acesso de qualquer computador, tablet ou telefone Windows, Apple ou Android.

Confira [Arquitetura de Serviços de Área de Trabalho Remota](desktop-hosting-logical-architecture.md) para uma discussão detalhada sobre as diferentes partes que funcionam em conjunto para compor sua implantação de Serviços de Área de Trabalho Remota.

Você tem uma implantação de Área de Trabalho Remota existente criada em uma versão anterior do Windows Server? Confira as opções para migrar para o Windows Server 2016, em que você pode tirar proveito de funcionalidades novas e melhores para desempenho e escala:

- [Migrar sua implantação do RDS para o Windows Server 2016](migrate-rds-role-services.md)
- [Atualizar sua implantação do RDS para o Windows Server 2016](upgrade-to-rds-2016.md)

Deseja criar uma nova implantação de Área de Trabalho Remota? Use as informações a seguir para implantar a Área de Trabalho Remota no Windows Server 2016:

- [Implantar a infraestrutura de Serviços de Área de Trabalho Remota](rds-deploy-infrastructure.md)
- [Criar uma coleção de sessão para conter os aplicativos e recursos que você deseja compartilhar](rds-create-collection.md)
- [Licenciar sua implantação do RDS](rds-client-access-license.md)
- Faça com que os usuários instalem um [cliente de Área de Trabalho Remota](clients/remote-desktop-clients.md) para que possam acessar os aplicativos e recursos. 
- Habilite a alta disponibilidade adicionando Hosts da Sessão e Agentes de Conexão adicionais:
   - [Dimensionar uma coleção de RDS existente com um farm de host de sessão de Área de Trabalho Remota](rds-scale-rdsh-farm.md)
   - [Adicionar alta disponibilidade à infraestrutura do Agente de Conexão de Área de Trabalho Remota](rds-connection-broker-cluster.md)
   - [Adicionar alta disponibilidade à Web de Área de Trabalho Remota e ao Web Front do Gateway de Área de Trabalho Remota](rds-rdweb-gateway-ha.md)
   - [Implantar um sistema de arquivos de Espaços de Armazenamento Diretos com dois nós para armazenamento UPD](rds-storage-spaces-direct-deployment.md)


Se você é um parceiro de hospedagem interessado em usar a Área de Trabalho Remota para fornecer aplicativos e recursos aos clientes ou é um cliente procurando alguém para hospedar seus aplicativos, confira [Parceiros de hospedagem de Serviços de Área de Trabalho Remota](rds-hosting-partners.md) para obter informações sobre uma avaliação pela qual você pode passar sobre como usar o RDS no Azure como um ambiente de hospedagem, bem como uma lista de parceiros que foram aprovados nela.
