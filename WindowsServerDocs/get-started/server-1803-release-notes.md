---
title: 'Notas de versão: problemas importantes no Windows Server, versão 1803'
description: Saiba mais sobre problemas conhecidos, limitações ou outras informações que necessárias antes de instalar o Windows Server, versão 1803
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 05/07/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: e9bd860769ec375a6d89ac452e3430b791fff3ad
ms.sourcegitcommit: 9ed4c9fe04ebf3ef488170503c9a354c992b6fde
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4339314"
---
# Notas de versão: Problemas importantes no Windows Server, versão 1803

>Aplicável a: Canal semestral do Windows Server

Essas notas de versão resumem os problemas mais importantes no sistema operacional Windows Server, inclusive formas de evitar ou solucionar problemas conhecidos. Para saber mais sobre novos recursos nesta versão, consulte [as novidades no Windows Server versão 1803](whats-new-in-windows-server-1803.md). Confira [os contêineres do Windows sobre](https://docs.microsoft.com/virtualization/windowscontainers/about/) se você estiver interessado em um Windows Server, versão 1803, o contêiner de execução. 

Salvo indicação em contrário, cada problema reportado aplica-se a todas as edições e opções de instalação do Windows Server, versão 1803.  

Atualizamos continuamente neste artigo. Se os problemas conhecidos são identificados, nós vai documentá-las aqui. 


## Datacenter definido pelo software

Recursos de datacenter definido pelo software como espaços de armazenamento diretos, definido pelo software, rede e blindado máquinas virtuais não estão incluídas no Windows Server, versão 1803. Conforme descrito no [canal semestral do Windows Server update](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/), o canal de canal semestral do Windows Server se concentra em contêineres e cenários de aplicativos que se beneficiam da inovação mais rápida. 

Se você precisar funções de infraestrutura, use uma versão de canal de manutenção a longo prazo: Windows Server 2016 (disponível agora) ou [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (provenientes posteriormente neste ano).

Estamos comprometidos para criar a melhor plataforma de infraestrutura hiperconvergente e continuamos a desenvolver novos recursos e melhorar os existentes com base em seus comentários. Obrigado por nos ajudar a obter a [10.000 clusters de espaços de armazenamento diretos](https://blogs.technet.microsoft.com/filecab/2018/03/27/storage-spaces-direct-momentum)! Estamos ansiosos para compartilhar mais posteriormente neste ano.