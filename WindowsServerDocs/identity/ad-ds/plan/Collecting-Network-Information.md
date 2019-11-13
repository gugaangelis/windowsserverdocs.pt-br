---
ms.assetid: 8be8c48d-790c-4199-b9d3-9f4a07d5ced2
title: Coletar informações de rede
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5642963caee47ac48f841a13b47852b6b7d9b241
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408981"
---
# <a name="collecting-network-information"></a>Coletar informações de rede

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa na criação de uma topologia de site em vigor no Active Directory Domain Services (AD DS) é consultar o grupo de rede da sua organização para coletar informações e se comunicar com elas regularmente sobre sua topologia de rede física.  
  
## <a name="creating-a-location-map"></a>Criando um mapa de local

Crie um mapa de local que represente a infraestrutura de rede física da sua organização. No mapa de localização, identifique as localizações geográficas que contêm grupos de computadores com conectividade interna de 10 megabits por segundo (Mbps) ou superior (velocidade de rede local (LAN) ou melhor).  
  
## <a name="listing-communication-links-and-available-bandwidth"></a>Listando links de comunicação e largura de banda disponível

Depois de criar um mapa de local, documente o tipo de link de comunicação, a velocidade do link e a largura de banda disponível entre cada local. Obtenha uma topologia de WAN (rede de longa distância) do seu grupo de rede. Para obter uma lista de tipos de circuito WAN comuns e suas larguras de banda, consulte a seção "determinando o custo" em [criando um design de link de site](../../ad-ds/plan/Creating-a-Site-Link-Design.md). Você precisará dessas informações para criar links de site posteriormente no processo de design da topologia do site.  
  
A largura de banda refere-se à quantidade de dados que você pode transmitir por um canal de comunicação em um determinado período de tempo. A largura de banda disponível refere-se à quantidade de largura de banda realmente disponível para uso por AD DS. Você pode obter as informações de largura de banda disponíveis do seu grupo de rede ou pode analisar o tráfego em cada link usando um analisador de protocolo, como Monitor de Rede. Para obter informações sobre como instalar Monitor de Rede, consulte o artigo [monitorando o tráfego de rede](https://go.microsoft.com/fwlink/?LinkId=107058).  
  
Documente cada local e os outros locais que estão vinculados a ele. Além disso, registre o tipo de link de comunicação e sua largura de banda disponível. Para uma planilha para ajudá-lo a listar os links de comunicação e a largura de banda disponível, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e abra "locais geográficos e links de comunicação" (DSSTOPO_1. doc).  
  
## <a name="listing-ip-subnets-within-each-location"></a>Listando sub-redes IP em cada local

Depois de documentar os links de comunicação e a largura de banda disponível entre cada local, registre as sub-redes IP em cada local. Se você ainda não souber a máscara de sub-rede e o endereço IP em cada local, consulte seu grupo de rede.  
  
O AD DS associa uma estação de trabalho a um site comparando o endereço IP da estação de trabalho com as sub-redes associadas a cada site. À medida que você adiciona controladores de domínio a um domínio, AD DS também examina seus endereços IP e os coloca no site mais apropriado.  
  
Para uma planilha para ajudá-lo a listar as sub-redes IP em cada local, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e abrir "locais e sub-redes" (DSSTOPO_2. doc).  
  
> [!NOTE]  
> Além dos endereços IP versão 4 (IPv4), o Windows Server também dá suporte a prefixos de sub-rede IP versão 6 (IPv6). Para uma planilha para ajudá-lo a listar os prefixos de sub-rede IPv6, consulte o [Apêndice a: locais e prefixos de sub-rede](../../ad-ds/plan/Appendix-A--Locations-and-Subnet-Prefixes.md).  

## <a name="listing-domains-and-number-of-users-for-each-location"></a>Listando domínios e número de usuários para cada local

O número de usuários para cada domínio regional representado em um local é um dos fatores que determinam o posicionamento de controladores de domínio regionais e servidores de catálogo global, que é a próxima etapa no processo de design da topologia do site. Por exemplo, planeje posicionar um controlador de domínio regional em um local que contenha mais de 100 usuários de domínio regional para que eles ainda possam fazer logon no domínio se o link de WAN falhar.  
  
Registre os locais, os domínios que são representados em cada local e o número de usuários para cada domínio que é representado em cada local. Para uma planilha para ajudá-lo a listar os domínios e o número de usuários que são representados em cada local, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e abrir "domínios e usuários em cada local" (DSSTOPO_3. doc).  
