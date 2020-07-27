---
title: 'Notas sobre a versão: problemas importantes no Windows Server, versão 1803'
description: Saiba mais sobre problemas conhecidos, limitações ou outras informações necessárias antes de instalar o Windows Server, versão 1803
ms.prod: windows-server
ms.date: 05/07/2018
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 7dc63afba827e2a58ba28d2c4398f1ba80d7e7b5
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962488"
---
# <a name="release-notes-important-issues-in-windows-server-version-1803"></a>Notas sobre a versão: Problemas importantes no Windows Server, versão 1803

>Aplica-se a: Windows Server Canal Semestral

Essas notas sobre a versão resumem os problemas mais críticos no sistema operacional do Windows Server, inclusive formas de evitar ou solucionar problemas conhecidos. Para saber sobre os novos recursos nesta versão, confira [Novidades no Windows Server versão 1803](whats-new-in-windows-server-1803.md). Confira [Sobre contêineres do Windows](/virtualization/windowscontainers/about/) se você estiver interessado em executar um contêiner com o Windows Server, versão 1803. 

Salvo indicação em contrário, cada problema reportado aplica-se a todas as edições e opções de instalação do Windows Server, versão 1803.  

Atualizaremos continuamente este artigo. Se algum problema conhecido for identificado, vamos documentá-lo aqui. 


## <a name="software-defined-datacenter"></a>Data center definido por software

Recursos de datacenter definido por software, como Espaços de Armazenamento Diretos, rede definida por software e máquinas virtuais blindadas não estão incluídos no Windows Server, versão 1803. Conforme descrito na [atualização do Canal Semestral do Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), o Canal Semestral do Windows Server se concentra em contêineres e cenários de aplicativo, que se beneficiam da inovação mais rápida. 

Se você precisar de funções de infraestrutura, use uma versão do Canal de Manutenção em Longo Prazo: Windows Server 2016 (agora disponível) ou [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (a ser lançado posteriormente neste ano).

Estamos empenhados em criar a melhor plataforma para infraestrutura hiperconvergente e continuaremos a desenvolver novos recursos e melhorar os existentes com base em seus comentários. Obrigado por nos ajudar a obter [mais de 10.000 clusters de Espaços de Armazenamento Diretos](https://techcommunity.microsoft.com/t5/storage-at-microsoft/storage-spaces-direct-10-000-clusters-and-counting/ba-p/428185). Estamos ansiosos para compartilhar mais posteriormente neste ano.
