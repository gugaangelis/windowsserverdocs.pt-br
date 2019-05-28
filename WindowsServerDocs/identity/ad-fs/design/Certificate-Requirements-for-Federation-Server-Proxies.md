---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisitos de Certificado para Proxies do Servidor de Federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca0b25480eedfc6471837ab8ae83b0d1d522e61e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191657"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisitos de Certificado para Proxies do Servidor de Federação

Servidores que executam na função de proxy de servidor de federação nos serviços de Federação do Active Directory \(do AD FS\) são necessários para usar Secure Sockets Layer \(SSL\) certificados de autenticação de servidor. Os proxies de servidor de federação usam certificados de autenticação de servidor SSL para proteger a comunicação de tráfego do servidor Web com clientes Web.  
  
Proxies de servidor de Federação geralmente são expostos a computadores na Internet que não estão incluídos na sua infraestrutura de chave pública do enterprise \(PKI\). Portanto, usar um certificado de autenticação de servidor emitido por um público \(terceiro\-terceiros\) autoridade de certificação \(autoridade de certificação\), por exemplo, VeriSign.  
  
Quando você tiver um farm de proxy do servidor de federação, todos os computadores de proxy de servidor de Federação devem usar o mesmo certificado de autenticação de servidor. Para obter mais informações, consulte [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
É importante verificar se o nome do assunto nas correspondências de certificado de autenticação de servidor o nome do serviço de federação que é o valor especificado no snap do gerenciamento do AD FS\-no. Para localizar esse valor, abra o snap\-, com o botão direito\-clique em **Service**, clique em **editar propriedades do serviço de Federação**e, em seguida, localize o valor na **federação Nome do serviço** caixa de texto.  
  
Para obter informações gerais sobre como usar certificados SSL, consulte Configurando Secure Sockets Layer no IIS 7.0 \( [http:\/\/go.microsoft.com\/fwlink\/? LinkID\=108544](https://go.microsoft.com/fwlink/?LinkID=108544) \) e Configuring Server Certificates in IIS 7.0 \( [http:\/\/go.microsoft.com\/fwlink\/? LinkID\=108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Certificados de autenticação de cliente não são necessários para proxies de servidor de Federação do AD FS.  
  
Se qualquer certificado que você use tem listas de certificados revogados \(CRLs\), o servidor com o certificado configurado deve ser capaz de contatar o servidor que distribui as CRLs. O tipo de CRL determina quais portas são usadas.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
