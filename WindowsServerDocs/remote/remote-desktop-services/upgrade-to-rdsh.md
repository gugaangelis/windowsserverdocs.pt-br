---
title: Atualização das suas implantações de Host de Sessão de Área de Trabalho Remota para o Windows Server 2016
description: Este artigo descreve como atualizar as implantações existentes de Serviços de Área de Trabalho Remota para o Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: e685c51a003a7121dab19c74d82796311ef0889a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857119"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>Atualização das suas implantações de Host de Sessão de Área de Trabalho Remota para o Windows Server 2016

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

> [!IMPORTANT]
> Todos os aplicativos devem ser desinstalados antes da atualização e reinstalados depois dela para evitar problemas de compatibilidade de aplicativos que podem aumentar devido à atualização.

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Atualizações de sistema operacional compatíveis com a função RDS instalada
Os upgrades para o Windows Server 2016 são compatíveis somente a partir do Windows Server 2012 R2 e Windows Server 2016 TP5.

## <a name="upgrading-a-rds-session-based-collection"></a>Atualização de uma coleção RDS baseada em sessão
Para manter o tempo de inatividade ao mínimo, siga as etapas abaixo durante a atualização de uma coleção RDS baseada em sessão:

1. Identificar os servidores a serem atualizados, digamos, metade dos servidores na coleção.
2. Impedir novas conexões para esses servidores definindo **Permitir Novas conexões** como false.
3. Fazer logoff de todas as sessões nesses servidores. 
4. Remova esses servidores da coleção.
5. Atualize os servidores para o Windows Server 2016.
6. Definir **Permitir Novas Conexões** como "false" nos servidores restantes na coleção.
7. Retorne os servidores atualizados para suas coleções correspondentes.
8. Remova o conjunto restante dos servidores a serem atualizados da coleção.
9. Definir **Permitir Novas Conexões** como "true" nos servidores atualizados na coleção.
10. Agora, atualize os servidores restantes na implantação seguindo as etapas de 3 a 9 acima.

## <a name="upgrading-a-standalone-rd-session-host-server"></a>Atualizando um servidor de Host de Sessão de Área de Trabalho Remota autônomo
Um servidor de Host de Sessão de Área de Trabalho Remota autônomo pode ser atualizado a qualquer momento.