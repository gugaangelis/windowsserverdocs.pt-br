---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisitos de Certificado para Proxies do Servidor de Federação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cc32288d01d7e1386f146716f45f0e49ced3d48e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858119"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisitos de Certificado para Proxies do Servidor de Federação

Os servidores que estão em execução na função de proxy do servidor de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS\) são necessários para usar os certificados de autenticação do servidor protocolo SSL \(SSL\). Os proxies de servidor de federação usam certificados de autenticação de servidor SSL para proteger a comunicação de tráfego do servidor Web com clientes Web.  
  
Os proxies de servidor de Federação geralmente são expostos a computadores na Internet que não estão incluídos em sua infraestrutura de chave pública corporativa \(\)PKI. Portanto, use um certificado de autenticação de servidor emitido por um \(público\-terceiros\) autoridade de certificação \(\)de certificação, por exemplo, VeriSign.  
  
Quando você tem um farm de proxy de servidor de Federação, todos os computadores proxy de servidor de Federação devem usar o mesmo certificado de autenticação de servidor. Para obter mais informações, consulte [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
É importante verificar se o nome da entidade no certificado de autenticação do servidor corresponde ao valor de Serviço de Federação nome especificado no\-snap de gerenciamento de AD FS no. Para localizar esse valor, abra o\-de snap no,\-clique em **serviço**, clique em **editar propriedades de serviço de Federação**e localize o valor na caixa de texto nome do **serviço de Federação** .  
  
Para obter informações gerais sobre como usar certificados SSL, consulte Configurando protocolo SSL no IIS 7,0 \([http:\/\/go.microsoft.com\/fwlink\/? LinkId\=108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) e configuração de certificados de servidor no IIS 7,0 \([http:\/\/go.Microsoft.com\/fwlink\/? LinkId\=\)108545](https://go.microsoft.com/fwlink/?LinkID=108545) .  
  
> [!NOTE]  
> Os certificados de autenticação de cliente não são necessários para AD FS proxies de servidor de Federação.  
  
Se qualquer certificado que você usar tiver listas de certificados revogados \(CRLs\), o servidor com o certificado configurado deverá ser capaz de contatar o servidor que distribui as CRLs. O tipo de CRL determina quais portas são usadas.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
