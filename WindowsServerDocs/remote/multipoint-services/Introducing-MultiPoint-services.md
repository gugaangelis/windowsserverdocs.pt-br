---
title: Introdução aos MultiPoint Services
description: Fornece uma visão geral dos serviços do MultiPoint, uma maneira de permitir que vários usuários compartilhem um sistema
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 1cbef744-4661-4ba9-9e2b-0bbd8854fd5c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e547156bb46d7195baa64a0094f1d3ca432eb016
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859169"
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