---
title: Introdução aos MultiPoint Services
description: Fornece uma visão geral do MultiPoint Services, uma maneira de permitir que vários usuários compartilham um sistema
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 86d240092282e7cc29eebe638e5a97312e22baff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844207"
---
# <a name="introducing-multipoint-services"></a>Introdução aos MultiPoint Services
Função do multiPoint Services no Windows Server 2016 permite que vários usuários, cada um com suas próprias independente e conhecida Windows experiência, para que compartilhem simultaneamente um computador. Há várias maneiras que os usuários podem acessar suas sessões. Uma delas é pela comunicação remota no servidor usando o [aplicativos de área de trabalho remotos](../remote-desktop-services/clients/remote-desktop-clients.md) com qualquer dispositivo. Outra maneira é por meio de estações físicas que estações anexadas ao MultiPoint server:  
  
-   Diretamente para portas de vídeo no computador  
  
-   Por meio de clientes USB zero especializados (também conhecidos como hubs multifuncionais de USB), bem como por meio de dispositivos semelhantes de USB over Ethernet.  
  
-   Sobre a rede local (LAN)  
  
Cada um desses métodos é descrita mais detalhadamente [estações do MultiPoint Services](MultiPoint-services-Stations.md) mais adiante neste documento.  
  
Este documento aborda os seguintes fatores devem ser consideradas quando você estiver planejando implantar o MultiPoint Services:  
  
-   Que tipo de áreas de trabalho para usar com seu sistema MultiPoint Services: Você precisará sessões, as máquinas virtuais ou computadores do Windows?  
  
-   [Selecionar o Hardware para o seu sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md): Quais decisões de hardware deve fazer?  
  
-   [Requisitos de hardware e recomendações de desempenho](Hardware-Requirements-and-Performance-Recommendations.md): Qual hardware é necessário para o MultiPoint Services?  
  
-   [Planejamento do Site do multiPoint Services](MultiPoint-services-Site-Planning.md): Onde os computadores que executam o MultiPoint Services e suas estações estará localizados e como serão eles configurados?  
  
-   [Contas de usuário e as considerações de rede](Network-Considerations-and-User-Accounts.md): O ambiente de rede na qual o sistema MultiPoint Services é implantado pode afetar como contas de usuário são gerenciadas. O que é o seu ambiente de rede? Como as contas de usuário ser gerenciadas?  
  
-   [Armazenar arquivos com MultiPoint Services](Storing-Files-with-MultiPoint-services.md): Onde os arquivos de usuário ser armazenados e como serão eles acessados?  
  
-   [Lista de verificação de pré-implantação](Predeployment-Checklist.md)  