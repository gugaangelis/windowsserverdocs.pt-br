---
title: Introdução aos MultiPoint Services
description: Fornece uma visão geral dos serviços do MultiPoint, uma maneira de permitir que vários usuários compartilhem um sistema
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: c1f317909b05c421058041bcd2546c4cc0e704bb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970633"
---
# <a name="introducing-multipoint-services"></a>Introdução aos MultiPoint Services
A função de serviços do MultiPoint no Windows Server 2016 permite que vários usuários, cada um com sua própria experiência independente e familiar do Windows, compartilhem simultaneamente um computador. Há várias maneiras pelas quais os usuários podem acessar suas sessões. Uma maneira é a comunicação remota no servidor usando os [aplicativos de área de trabalho remota](../remote-desktop-services/clients/remote-desktop-clients.md) com qualquer dispositivo. Outra maneira é por meio das estações físicas que as estações conectadas ao servidor MultiPoint:

-   Diretamente para portas de vídeo no computador

-   Por meio de clientes USB especializados (também chamados de hubs USB multifuncionais), bem como por meio de dispositivos USB-over-Ethernet semelhantes.

-   Sobre a rede local (LAN)

Cada um desses métodos é descrito mais detalhadamente em [estações de serviços do MultiPoint](MultiPoint-services-Stations.md) , mais adiante neste documento.

Este documento aborda os seguintes fatores a serem considerados ao planejar a implantação dos serviços do MultiPoint:

-   Que tipo de desktops usar com o sistema MultiPoint Services: serão necessárias sessões, máquinas virtuais ou computadores Windows?

-   [Selecionando o hardware para o seu sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md): quais decisões de hardware você deve fazer?

-   [Requisitos de hardware e recomendações de desempenho](Hardware-Requirements-and-Performance-Recommendations.md): o hardware é necessário para os serviços do MultiPoint?

-   [Planejamento de site dos serviços do MultiPoint](MultiPoint-services-Site-Planning.md): onde os computadores que estão executando os serviços do MultiPoint e suas estações estão localizados e como eles serão configurados?

-   [Considerações de rede e contas de usuário](Network-Considerations-and-User-Accounts.md): o ambiente de rede no qual o sistema de serviços do MultiPoint está implantado pode afetar como as contas de usuário são gerenciadas. Qual é seu ambiente de rede? Como as contas de usuário serão gerenciadas?

-   [Armazenando arquivos com os serviços do MultiPoint](Storing-Files-with-MultiPoint-services.md): onde os arquivos do usuário serão armazenados e como eles serão acessados?

-   [Lista de verificação pré-implantação](Predeployment-Checklist.md)