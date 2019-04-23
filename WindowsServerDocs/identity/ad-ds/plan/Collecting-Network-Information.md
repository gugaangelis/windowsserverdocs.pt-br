---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Coletar informações de rede
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d88cc2fafd9aa4fc221efc901c48fa41ef007a21
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867037"
---
# <a name="collecting-network-information"></a>Coletar informações de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa na criação de uma topologia de site em vigor nos serviços de domínio Active Directory (AD DS) é consultar o grupo de rede da sua organização para coletar informações e se comunicar com eles regularmente sobre a topologia de rede física.  
  
## <a name="creating-a-location-map"></a>Criando um mapa do local

Crie um mapa do local que representa a infraestrutura de rede física de sua organização. No mapa de localização, identifique os locais geográficos que contêm grupos de computadores com conectividade interna de 10 megabits por segundo (Mbps) ou superior (velocidade de rede local (LAN) ou melhor).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Listar links de comunicação e a largura de banda disponível

Depois de criar um mapa do local, documente o tipo de link de comunicação, a velocidade do link e a largura de banda disponível entre cada local. Obtenha uma topologia de rede de (longa distância WAN) de longa distância de seu grupo de rede. Para obter uma lista de tipos comuns de circuito WAN e suas larguras de banda, consulte a seção "Determinando o custo" no [criar um Design de Link de Site](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Você precisará dessas informações para criar links de site posteriormente no processo de design de topologia de site.  
  
Largura de banda refere-se à quantidade de dados que você pode transmitir por um canal de comunicação em uma determinada quantidade de tempo. Largura de banda disponível refere-se à quantidade de largura de banda, na verdade, está disponível para uso pelo AD DS. Você pode obter informações de largura de banda disponível de seu grupo de rede, ou você pode analisar o tráfego em cada link usando um analisador de protocolo como o Monitor de rede. Para obter informações sobre como instalar o Monitor de rede, consulte o artigo [monitoramento de tráfego de rede](https://go.microsoft.com/fwlink/?LinkId=107058).  
  
Documente cada local e outros locais que estão vinculados a ele. Além disso, registre o tipo de link de comunicação e sua largura de banda disponível. Para uma planilha para ajudá-lo na listagem links de comunicação e a largura de banda disponível, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, e Abra "Geográficas locais e Links de comunicação" (DSSTOPO_1.doc).  
  
## <a name="listing-ip-subnets-within-each-location"></a>Listando as sub-redes IP em cada local

Depois que você documente os links de comunicação e a largura de banda disponível entre cada local, registre as sub-redes IP em cada local. Se você já souber o endereço IP em cada local e uma máscara de sub-rede, consulte seu grupo de rede.  
  
AD DS associa uma estação de trabalho com um site, comparando o endereço IP da estação de trabalho com as sub-redes que estão associados a cada site. Conforme você adiciona controladores de domínio em um domínio, AD DS também examina seus endereços IP e as coloca no local mais apropriado.  
  
Para uma planilha para ajudá-lo listando as sub-redes IP em cada local, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de baixar e abrir " Locais e sub-redes"(DSSTOPO_2.doc).  
  
> [!NOTE]  
> Além de IP versão 4 (IPv4) endereços, o Windows Server também oferece suporte a IP versão 6 (IPv6) prefixos de sub-rede. Para uma planilha para ajudá-lo listando os prefixos de sub-rede IPv6, consulte [apêndice a: Locais e prefixos de sub-rede](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>A listagem de domínios e o número de usuários para cada local

O número de usuários para cada domínio regional que é representado em um local é um dos fatores que determinam o posicionamento dos controladores de domínio regional e servidores de catálogo global, que é a próxima etapa no processo de design de topologia de site. Por exemplo, se planeje colocar um controlador de domínio regional em um local que contém mais de 100 usuários de domínio regional, portanto, eles ainda podem fazer logon no domínio se o link WAN falhar.  
  
Registre os locais, os domínios que são representados em cada local e o número de usuários para cada domínio que é representado em cada local. Para uma planilha para ajudá-lo listando os domínios e o número de usuários que são representados em cada local, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixe Job_Aids_Designing_and_Deploying_Directory_and_ Security_Services.zip e abrir "domínios e usuários em cada local" (DSSTOPO_3.doc).  
