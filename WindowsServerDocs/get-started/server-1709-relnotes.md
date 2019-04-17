---
title: 'Notas de versão: problemas importantes no Windows Server, versão 1709'
description: Resume os problemas críticos que exigem solução alternativa para evitar falhas, congelamento, falha de instalação e perda de dados.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 04/23/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 4eebc498289a81c7f27fcf4b84d81ae13bc38e4f
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133852"
---
# Notas de versão: problemas importantes no Windows Server, versão 1709

>Aplicável a: Canal semestral do Windows Server

Essas notas de versão resumem os problemas mais importantes no sistema operacional do Windows Server&reg;, inclusive formas de evitar ou solucionar problemas, se forem conhecidos. Para saber mais sobre alterações planejadas, novos recursos e correções desta versão, confira [Novidades no Windows Server versão 1709](whats-new-in-windows-server-1709.md) e os comunicados das equipes de recursos específicos. Salvo indicação em contrário, cada problema reportado aplica-se a todas as edições e opções de instalação do Windows Server 2016.  

Este documento é atualizado continuamente. À medida que são descobertas questões críticas que exijam uma solução alternativa, elas são adicionadas, assim como novas soluções e correções assim que estiverem disponíveis.  
  
## Espaços de armazenamento diretos
[comment]: # (ID: desconhecido; Emissor: stevenek; estado: aprovada)  
Os Espaços de Armazenamento Diretos não estão incluídos no Windows Server, versão 1709. Se você chamar *Enable-ClusterStorageSpacesDirect* ou o alias *Enable-ClusterS2D*, em um servidor executando o Windows Server, versão 1709, você receberá um erro com a mensagem "A operação solicitação não é suportada".

Também não há suporte para apresentar os servidores que executam o Windows Server, versão 1709, em uma implantação direta de Espaços de armazenamento diretos do Windows Server 2016.

O modelo de lançamento do Windows Server está oferecendo uma nova opção para se alinhar com versão semelhantes e manutenção de modelos de [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) e [Office 365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). As versões de Canal semestral oferecem novas funcionalidades para clientes que desejam mover em um ritmo rápido e terão novos lançamentos disponíveis duas vezes por ano, no primeiro e segundo semestres.

O canal de canal semestral do Windows Server se concentra em contêineres e cenários de aplicativos se beneficiando da inovação mais rápida, consulte este [blog](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update) para obter informações adicionais. Os clientes procurando por funções de infraestrutura, como espaços de armazenamento diretos, devem usar versões de canal de manutenção a longo prazo como Windows Server 2016 (agora disponível) e [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (provenientes posteriormente neste ano). Estamos comprometidos para criar a melhor plataforma de infraestrutura hiperconvergente e continuamos a desenvolver novos recursos e melhorar os existentes com base em seus comentários. 

Os Espaços de armazenamento diretos foram introduzidos no Windows Server 2016 e é a base para nossa plataforma com hiperconvergência. Estamos felizes pela adoção positiva da plataforma com hiperconvergência da Microsoft e estamos comprometidos com nossos clientes.

Estamos tiver sido escutando a seu feedback e estamos trabalhando para fornecer o [próximo conjunto de inovações](https://blogs.technet.microsoft.com/windowsserver/2017/09/07/sneak-peek-2-windows-server-version-1709-hyper-converged-infrastructure/) para nossa plataforma com hiperconvergência. Esses recursos estão disponíveis atualmente nas compilações do [Windows Insider](https://insider.windows.com/for-business/) , e nós gostaríamos para que você experimente-os e compartilhe seu feedback. Para clientes que buscam uma solução com hiperconvergência validada, recomendamos o programa [definido pelo software do Windows Server](http://microsoft.com/wssd).
