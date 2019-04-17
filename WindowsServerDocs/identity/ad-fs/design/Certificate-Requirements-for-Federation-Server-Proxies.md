---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: "Requisitos de certificado para Proxies de servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7fb8e71afed1c0eb6b55857835d95f2dd0ec9d5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisitos de certificado para Proxies de servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores que executam na função de proxy do servidor de Federação em serviços de Federação do Active Directory \(AD FS\) são necessárias para usar certificados de autenticação de servidor Secure Sockets Layer \(SSL\). Proxies de servidor de federação usam certificados de autenticação de servidor SSL para proteger a comunicação de tráfego do servidor Web com clientes da Web.  
  
Proxies de servidor de Federação geralmente são expostos para computadores na Internet que não estão incluídos em sua infraestrutura de chave pública do enterprise \(PKI\). Portanto, use um certificado de autenticação de servidor é emitido por uma autoridade de certificação pública \(third\-party\) \(CA\), por exemplo, VeriSign.  
  
Quando você tiver um farm de proxy do servidor de federação, todos os computadores de proxy de servidor de Federação devem usar o mesmo certificado de autenticação de servidor. Para obter mais informações, consulte [quando criar um Farm de Proxy do servidor de Federação](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
É importante verificar se o nome do requerente do certificado de autenticação de servidor corresponde o valor de nome de serviço de Federação é especificado no AD FS gerenciamento snap\-in. Para localizar esse valor, abra o snap\-in, right\-clique **serviço**, clique em **editar propriedades do serviço de Federação**e, em seguida, localizar o valor em **nome do serviço de Federação** caixa de texto.  
  
Para obter informações gerais sobre o uso de certificados SSL, consulte Configurando Secure Sockets Layer no IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/? LinkID\ = 108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) e Configurando certificados de servidor no IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/? LinkID\ = 108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Certificados de autenticação de cliente não são necessários para proxies de servidor de Federação do AD FS.  
  
Se qualquer certificado que você usa tiver listas de certificados revogados \(CRLs\), o servidor com o certificado configurado deve ser capaz de entrar em contato com o servidor que distribui as CRLs. O tipo de CRL determina quais portas são usadas.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
