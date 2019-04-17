---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: "Coletando informações de rede"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 174f11ca85a659f9d0c52e220d5ce37b1804f39b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="collecting-network-information"></a>Coletando informações de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa na criação de uma topologia de sites eficaz nos serviços de domínio do Active Directory (AD DS) é consultar o grupo de rede da sua organização para coletar informações e se comunicar com eles regularmente sobre a topologia de rede física.  
  
## <a name="creating-a-location-map"></a>Criar um mapa de localização  
Crie um mapa de localização que representa a infraestrutura de rede física da sua organização. No mapa de localização, identifica as localizações geográficas que contêm grupos de computadores com conectividade interna de 10 megabits por segundo (Mbps) ou superior (velocidade de rede local (LAN) ou melhor).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Links de comunicação de listagem e largura de banda disponível  
Depois de criar um mapa de localização, documente o tipo de link de comunicação, sua velocidade do link e a largura de banda disponível entre cada local. Obter uma topologia de rede (WAN) de longa distância do seu grupo de rede. Para obter uma lista dos tipos de circuito WAN comuns e suas larguras de banda, consulte a seção "Determinando o custo" [criando um projeto de Link Site](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Você precisará dessas informações para criar links de site mais tarde no processo de design de topologia de site.  
  
Largura de banda se refere à quantidade de dados que você pode transmitir em um canal de comunicação em uma determinada quantidade de tempo. Largura de banda disponível se refere à quantidade de largura de banda, na verdade, disponível para uso pelo AD DS. Você pode obter informações de largura de banda disponível de seu grupo de rede, ou você pode analisar o tráfego em cada link usando um analisador de protocolo, como o Monitor de rede, que é um componente fornecido com o Windows Server 2008. Para obter informações sobre como instalar o Monitor de rede, consulte monitorar o tráfego de rede ([https://go.microsoft.com/fwlink/?LinkId=107058](https://go.microsoft.com/fwlink/?LinkId=107058)).  
  
Documente cada local e os outros locais que são vinculados a ele. Além disso, registre o tipo de link de comunicação e sua largura de banda disponível. Para uma planilha para ajudá-lo a listagem de largura de banda disponível e links de comunicação, veja trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e Abra (DSSTOPO_1.doc) "Localizações geográficas e comunicação Links".  
  
## <a name="listing-ip-subnets-within-each-location"></a>Listagem sub-redes IP dentro de cada local  
Depois que você documentar os links de comunicação e a largura de banda disponível entre cada local, registre as sub-redes IP dentro de cada local. Se você não conheça o endereço IP em cada local e máscara de sub-rede, consulte seu grupo de rede.  
  
AD DS associa uma estação de trabalho com um site, comparando o endereço IP da estação de trabalho com as sub-redes são associados a cada site. Conforme você adiciona controladores de domínio em um domínio, o AD DS também examina seus endereços IP e coloca no site mais apropriado.  
  
Para uma planilha para ajudá-lo na listagem as sub-redes IP dentro de cada local, consulte trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abrir "locais e subredes" (DSSTOPO_2.doc).  
  
> [!NOTE]  
> Além de IP versão 4 (IPv4) endereços, Windows Server 2008 também dá suporte a IP versão 6 (IPv6) prefixos de sub-rede. Para uma planilha para ajudá-lo em detalhes os prefixos de sub-rede IPv6, consulte [apêndice a: locais e prefixos de sub-rede](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
## <a name="listing-domains-and-number-of-users-for-each-location"></a>Listando domínios e o número de usuários para cada local  
O número de usuários para cada domínio regional que é representado em um local é um dos fatores que determinam o posicionamento dos controladores de domínio regional e servidores de catálogo global, que é a próxima etapa no processo de design de topologia de site. Por exemplo, planeje colocar um controlador de domínio regionais em um local que contém mais de 100 usuários de domínio regional, portanto, eles ainda podem fazer logon no domínio se o link WAN falhar.  
  
Registre a localização, os domínios que são representados em cada local e o número de usuários para cada domínio que é representado em cada local. Para uma planilha para ajudá-lo na lista os domínios e o número de usuários que são representados em cada local, consulte trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), baixe < DICT__Job_Aids_Designing_and_Deploying_ Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip > e abrir "Domínios e os usuários em cada local" (DSSTOPO_3.doc).  
  


