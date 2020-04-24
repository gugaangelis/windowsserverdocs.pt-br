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
ms.openlocfilehash: a511c9888f27fe97fdeaf65cde022e5c2aa999d4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80826079"
---
# <a name="release-notes-important-issues-in-windows-server-version-1803"></a>Notas sobre a versão: Problemas importantes no Windows Server, versão 1803

>Aplica-se a: Windows Server Canal Semestral

Essas notas sobre a versão resumem os problemas mais críticos no sistema operacional do Windows Server, inclusive formas de evitar ou solucionar problemas conhecidos. Para saber sobre os novos recursos nesta versão, confira [Novidades no Windows Server versão 1803](whats-new-in-windows-server-1803.md). Confira [Sobre contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/about/) se você estiver interessado em executar um contêiner com o Windows Server, versão 1803. 

Salvo indicação em contrário, cada problema reportado aplica-se a todas as edições e opções de instalação do Windows Server, versão 1803.  

Atualizaremos continuamente este artigo. Se algum problema conhecido for identificado, vamos documentá-lo aqui. 


## <a name="software-defined-datacenter"></a>Data center definido por software

Recursos de datacenter definido por software, como Espaços de Armazenamento Diretos, rede definida por software e máquinas virtuais blindadas não estão incluídos no Windows Server, versão 1803. Conforme descrito na [atualização do Canal Semestral do Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), o Canal Semestral do Windows Server se concentra em contêineres e cenários de aplicativo, que se beneficiam da inovação mais rápida. 

Se você precisar de funções de infraestrutura, use uma versão do Canal de Manutenção em Longo Prazo: Windows Server 2016 (agora disponível) ou [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (a ser lançado posteriormente neste ano).

Estamos empenhados em criar a melhor plataforma para infraestrutura hiperconvergente e continuaremos a desenvolver novos recursos e melhorar os existentes com base em seus comentários. Obrigado por nos ajudar a obter [mais de 10.000 clusters de Espaços de Armazenamento Diretos](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum). Estamos ansiosos para compartilhar mais posteriormente neste ano.