---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Requisitos de resolução de nome para servidores de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2776cc29b8c9ede884a6b304cd541f700f516ca
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191263"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisitos de resolução de nome para servidores de federação

Quando os computadores cliente na rede corporativa tentam acessar um aplicativo ou serviço da Web que é protegido pelos serviços de Federação do Active Directory \(do AD FS\), eles deverão primeiro autenticar em um servidor de Federação. Uma maneira de autenticar é fazer com que os clientes de rede corporativa acessem um servidor de federação local por meio da autenticação integrada do Windows.  
  
## <a name="configure-corporate-dns"></a>Configurar DNS corporativo  
Para que a resolução de nome bem-sucedida por meio da autenticação integrada do Windows nos servidores de federação local possa ocorrer, o sistema de nomes de domínio \(DNS\) na rede corporativa da conta de parceiro deve ser configurado para um novo host \(Um\) registro de recurso que resolve o nome de domínio totalmente qualificado \(FQDN\) nome do host do servidor de federação para o endereço IP do cluster do servidor de Federação.  
  
Na ilustração a seguir, você pode ver como essa tarefa é realizada para determinado cenário. Nesse cenário, o balanceamento de carga de rede da Microsoft \(NLB\) fornece um nome de FQDN de cluster único e um único endereço IP para um farm de servidor de Federação existente.  
  
![requisitos de nome](media/adfs2_deploy_single_fs.gif)  
  
Para obter informações sobre como configurar um endereço IP ou FQDN de cluster usando NLB, consulte [especificando os parâmetros de Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Para obter informações sobre como configurar DNS corporativo para um servidor de federação, consulte [adicionar um Host &#40;um&#41; registro de recurso ao DNS corporativo para um servidor de Federação](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Para obter informações sobre como configurar proxies de servidor de federação na rede de perímetro, consulte [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
