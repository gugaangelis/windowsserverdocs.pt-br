---
title: Serviços da Área de Trabalho Remota – Acesso de qualquer lugar
description: Informações de planejamento para um Gateway de Área de Trabalho Remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: 0d3d8ed036b3befd81da6d5bbe8702ee866c6aa8
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63748828"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>Serviços da Área de Trabalho Remota – Acesso de qualquer lugar

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Os usuários finais podem conectar aos recursos da rede interna com segurança por fora do firewall corporativo através do Gateway de Área de Trabalho Remota.

Independentemente de como você configura as áreas de trabalho para os usuários finais, é possível conectar facilmente o Gateway de Área de Trabalho Remota no fluxo de conexão para uma conexão rápida e segura. Para usuários finais que conectam por meio de feeds publicados, é possível configurar a propriedade do Gateway de Área de Trabalho Remota ao configurar as propriedades gerais de implantação. Para usuários finais que conectam por meio de suas áreas de trabalho sem um feed, é possível adicionar facilmente o nome do Gateway de Área de Trabalho Remota da organização como uma propriedade de conexão, independentemente de qual aplicativo cliente da Área de Trabalho Remota os usuários usam.

As três principais finalidades do Gateway de Área de Trabalho Remota, na ordem da sequência de conexão, são:
1. **Estabelecer um túnel SSL criptografado entre o dispositivo do usuário final e o servidor de Gateway de Área de Trabalho Remota**: Para conectar por meio de qualquer servidor de Gateway de Área de Trabalho Remota, esse servidor deve ter um certificado instalado que o dispositivo do usuário final reconheça. Em testes e provas de conceitos, é possível usar certificados autoassinados, mas somente certificados publicamente confiáveis de uma autoridade de certificação devem ser usados em qualquer ambiente de produção.
2. **Autenticar o usuário no ambiente**: O Gateway de Área de Trabalho Remota usa o serviço IIS de caixa de entrada para realizar a autenticação e pode até mesmo utilizar o protocolo RADIUS para aproveitar as soluções de [autenticação multifator](rds-plan-mfa.md), como o Azure MFA. Além das políticas padrão criadas, é possível criar RD RAPs (Diretivas de Autorização de Recursos da Área de Trabalho Remota) adicionais e RD CAPs (Diretivas de Autorização de Conexão de Área de Trabalho Remota) adicionais para definir, mais especificamente, quais usuários devem ter acesso a quais recursos dentro do ambiente seguro.
3. **Passagem de tráfego entre o dispositivo do usuário final e o recurso especificado**: O Gateway de Área de Trabalho Remota continua a executar essa tarefa contanto que a conexão esteja estabelecida. É possível especificar diferentes propriedades de tempo limite nos servidores de Gateway de Área de Trabalho Remota para manter a segurança do ambiente, caso os usuários deixem o dispositivo inativo.

Encontre detalhes adicionais sobre a arquitetura geral de uma implantação dos Serviços de Área de Trabalho Remota [na arquitetura de referência da hospedagem de área de trabalho](desktop-hosting-reference-architecture.md).