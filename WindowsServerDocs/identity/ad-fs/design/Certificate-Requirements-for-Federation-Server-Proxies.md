---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisitos de Certificado para Proxies do Servidor de Federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: dab77c3e3226e89eb3ac9b74e7db9b6df8f181bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408141"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisitos de Certificado para Proxies do Servidor de Federação

Os servidores que estão em execução na função proxy de servidor de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 são necessários para usar os certificados de autenticação de servidor protocolo SSL \(SSL @ no__t-3. Os proxies de servidor de federação usam certificados de autenticação de servidor SSL para proteger a comunicação de tráfego do servidor Web com clientes Web.  
  
Os proxies de servidor de Federação geralmente são expostos a computadores na Internet que não estão incluídos em sua infraestrutura de chave pública corporativa \(PKI @ no__t-1. Portanto, use um certificado de autenticação de servidor emitido por uma autoridade de certificação pública \(third @ no__t-1party @ no__t-2 \(CA @ no__t-4, por exemplo, VeriSign.  
  
Quando você tem um farm de proxy de servidor de Federação, todos os computadores proxy de servidor de Federação devem usar o mesmo certificado de autenticação de servidor. Para obter mais informações, consulte [Quando criar um Farm de Proxy do Servidor de Federação](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
É importante verificar se o nome da entidade no certificado de autenticação do servidor corresponde ao valor de Serviço de Federação nome especificado no snap-in de gerenciamento de AD FS. @ no__t-0in. Para localizar esse valor, abra o **serviço**snap @ no__t-0in, Right @ no__t-1CLICK, clique em **Editar serviço de Federação Propriedades**e localize o valor na caixa de texto **nome do serviço de Federação** .  
  
Para obter informações gerais sobre como usar certificados SSL, consulte Configurando protocolo SSL no IIS 7,0 \([http: @no__t -2\/go.microsoft.com @ no__t-4fwlink @ no__t-5? LinkId @ no__t-6108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) e configurando certificados de servidor no IIS 7,0 \([http: @no__t -101go.microsoft.com @ no__t-12fwlink @ no__t-13? LinkId @ no__t-14108545](https://go.microsoft.com/fwlink/?LinkID=108545)5.  
  
> [!NOTE]  
> Os certificados de autenticação de cliente não são necessários para AD FS proxies de servidor de Federação.  
  
Se qualquer certificado que você usar tiver listas de certificados revogados \(CRLs @ no__t-1, o servidor com o certificado configurado deverá ser capaz de contatar o servidor que distribui as CRLs. O tipo de CRL determina quais portas são usadas.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
