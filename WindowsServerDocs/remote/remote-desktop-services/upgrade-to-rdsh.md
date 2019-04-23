---
title: Atualizando seu Host de sessão da área de trabalho remota para o Windows Server 2016
description: Este artigo descreve como atualizar as implantações existentes do serviços de área de trabalho remota para Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: 0cf5af29d610ba64d045e10241fd39b01d3f7024
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856057"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>Atualizando seu Host de sessão da área de trabalho remota para o Windows Server 2016

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

> [!IMPORTANT]
> Todos os aplicativos devem ser desinstalados antes do upgrade e reinstalados após a atualização para evitar problemas de compatibilidade de aplicativo que podem aumentar devido à atualização.

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Suporte para atualizações de sistema operacional com a função RDS instalada
Atualizações para o Windows Server 2016 têm suporte apenas no Windows Server 2012 R2 e Windows Server 2016 TP5.

## <a name="upgrading-a-rds-session-based-collection"></a>Atualização de uma coleção com base em sessão RDS
Para impedir que o tempo de inatividade mínimo, é melhor seguir as etapas abaixo ao atualizar uma coleção com base em sessão RDS:

1. Identificar os servidores a serem atualizados, digamos, metade os servidores na coleção.
2. Impedir que novas conexões para esses servidores definindo **permitir que novas conexões** como false.
3. Fazer logoff de todas as sessões nesses servidores. 
4. Remova esses servidores da coleção.
5. Atualize os servidores para o Windows Server 2016.
6. Definir **permitir que novas conexões** como "false" nos servidores restantes na coleção.
7. Adicione os servidores atualizados para suas coleções correspondentes.
8. Remova o conjunto restante dos servidores a serem atualizados da coleção.
9. Definir **permitir que novas conexões** como "true" nos servidores atualizados na coleção.
10. Agora, atualize os servidores restantes na implantação seguindo as etapas de 3 a 9 acima.

## <a name="upgrading-a-standalone-rd-session-host-server"></a>Atualizando um servidor de Host de sessão de área de trabalho remota autônomos
Um servidor de Host de sessão de área de trabalho remota autônomos pode ser atualizado a qualquer momento.