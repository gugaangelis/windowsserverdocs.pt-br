---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Requisitos de resolução de nome para servidores de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 88ec418bd72a6389856deb1abd85641d8782bc30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408045"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisitos de resolução de nome para servidores de federação

Quando os computadores cliente na rede corporativa tentam acessar um aplicativo ou serviço Web protegido por Serviços de Federação do Active Directory (AD FS) \(AD FS\), eles devem primeiro se autenticar em um servidor de Federação. Uma maneira de se autenticar é fazer com que os clientes de rede corporativa acessem um servidor de federação local por meio da autenticação integrada do Windows.  
  
## <a name="configure-corporate-dns"></a>Configurar DNS corporativo  
Para que a resolução de nomes bem-sucedida por meio da autenticação integrada do Windows em servidores de federação local possa ocorrer, o sistema de nomes de domínio \(DNS\) na rede corporativa do parceiro de conta deve ser configurado para um novo host \(um registro de recurso de\) que resolverá o nome de domínio totalmente qualificado \(FQDN\) nome de host do servidor de Federação para o endereço IP do cluster do servidor de  
  
Na ilustração a seguir, você pode ver como essa tarefa é realizada para determinado cenário. Nesse cenário, o balanceamento de carga de rede da Microsoft \(o NLB\) fornece um único nome FQDN de cluster e um único endereço IP de cluster para um farm de servidores de Federação existente.  
  
![requisitos de nome](media/adfs2_deploy_single_fs.gif)  
  
Para obter informações sobre como configurar um endereço IP de cluster ou FQDN de cluster usando o NLB, consulte [especificando os parâmetros de cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Para obter informações sobre como configurar o DNS corporativo para um servidor de Federação, consulte [Adicionar &#40;um&#41; host um registro de recurso ao DNS corporativo para um servidor de Federação](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Para obter informações sobre como configurar proxies de servidor de Federação na rede de perímetro, consulte [requisitos de resolução de nomes para proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
