---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: "Requisitos de resolução de nome para servidores de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisitos de resolução de nome para servidores de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando computadores cliente na rede corporativa tentam acessar um aplicativo ou serviço da Web que está protegido pelo \(AD FS\) serviços de Federação do Active Directory, o usuário deve primeiro autenticar em um servidor de Federação. É uma maneira de se autenticar com os clientes de rede corporativa acessar um servidor de federação local por meio de autenticação integrada do Windows.  
  
## <a name="configure-corporate-dns"></a>Configurar o DNS corporativa  
Para que a resolução de nomes bem-sucedida por meio de autenticação integrada do Windows em servidores de federação local pode ocorrer, Domain Name System \(DNS\) na rede da empresa do parceiro de conta devem ser definidas para um novo registro de recurso \(A\) do host que resolverá o nome do host \(FQDN\) do nome de domínio totalmente qualificado do servidor de federação para o endereço IP do cluster do servidor de Federação.  
  
Na ilustração a seguir, você pode ver como essa tarefa é realizada para um determinado cenário. Nesse cenário, \(NLB\) balanceamento de carga de rede Microsoft fornece um nome FQDN do cluster único e endereço IP do cluster único para um farm de servidores de Federação existente.  
  
![Requisitos de nome](media/adfs2_deploy_single_fs.gif)  
  
Para obter informações sobre como configurar um endereço IP do cluster ou FQDN usando NLB do cluster, consulte [especificando os parâmetros de Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Para obter informações sobre como configurar o DNS corporativo para um servidor de federação, consulte [adicionar um Host & #40; A & #41; Registro de recurso DNS corporativo para um servidor de Federação](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Para obter informações sobre como configurar proxies de servidor de federação na rede de perímetro, consulte [requisitos de resolução de nome de Proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
