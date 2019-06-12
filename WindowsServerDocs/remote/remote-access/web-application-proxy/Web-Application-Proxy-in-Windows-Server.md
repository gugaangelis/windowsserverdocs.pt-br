---
ms.assetid: 0b3587b2-219f-43d8-88b4-1254eaa8b910
title: Proxy de aplicativo Web no Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: web-app-proxy
ms.tgt_pltfrm: na
ms.topic: article
author: kgremban
ms.openlocfilehash: 760b0fa11d8d0b77c2a44a8696d199bc378da947
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446795"
---
# <a name="web-application-proxy-in-windows-server"></a>Proxy de aplicativo Web no Windows Server

>Aplica-se a: Windows Server&reg; 2016

**Este conteúdo é relevante para a versão local do Proxy de aplicativo Web. Para habilitar o acesso seguro a aplicativos locais pela nuvem, consulte o [conteúdo de Proxy de aplicativo do Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/).**  
  
O conteúdo desta seção descreve as novidades e mudanças do Proxy de aplicativo de Web para o Windows Server 2016. Os novos recursos e alterações listados aqui são os mais probabilidade de ter um impacto maior enquanto você trabalha com a versão prévia.  
  
## <a name="web-application-proxy-new-features"></a>Novos recursos do Web Application Proxy  
  
- Pré-autenticação para publicação de aplicativo HTTP básico  
  
  HTTP básico é o protocolo de autorização usado por muitos protocolos, incluindo o ActiveSync, para conectar clientes avançados, incluindo smartphones, com sua caixa de correio do Exchange. Proxy de aplicativo Web tradicionalmente interage com o AD FS usando redirecionamentos que não há suporte para clientes do ActiveSync. Essa nova versão do Proxy de aplicativo Web fornece suporte para publicar um aplicativo usando HTTP básica, permitindo que o aplicativo HTTP receber uma não-declarações terceira parte confiável para o aplicativo para o serviço de Federação.  
  
  Para obter mais informações sobre a publicação de HTTP básica, consulte [publicar aplicativos usando pré-autenticação do AD FS](../web-application-proxy/../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
- Publicação de domínio de curinga de aplicativos  
  
  Para dar suporte a cenários como o SharePoint 2013, a URL externa para o aplicativo agora pode incluir um caractere curinga para que você possa publicar vários aplicativos a partir de um domínio específico, por exemplo, https://*.sp-apps.contoso.com. Isso simplificará a publicação de aplicativos do SharePoint.  
  
- HTTP para redirecionamento a HTTPS  
  
  Para certificar-se de que os usuários podem acessar seu aplicativo, mesmo se eles não digitar a URL HTTPS, Proxy de aplicativo Web agora dá suporte a HTTP para redirecionamento a HTTPS.  
  
- Publicação HTTP  
  
  Agora é possível publicar aplicativos HTTP usando a pré-autenticação de passagem  
  
- Publicação de aplicativos de Gateway de área de trabalho remota  
  
  Para obter mais informações sobre RDG no Proxy de aplicativo Web, consulte [publicando aplicativos com o SharePoint, Exchange e RDG](../web-application-proxy/Publishing-Applications-with-SharePoint,-Exchange-and-RDG.md)  
  
- Novo log de depuração para a melhor solução de problemas e log do serviço aprimorado de trilha de auditoria completa e tratamento de erros aprimorado  
  
  Para obter mais informações sobre como solucionar problemas, consulte [solução de problemas de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn770156.aspx)  
  
- Aprimoramentos de interface do usuário do Console do administrador  
  
- Propagação de endereço IP do cliente para aplicativos de back-end  
  
## <a name="see-also"></a>Consulte também  
  
-   [Novidades no Windows Server 2016](https://technet.microsoft.com/library/dn765472.aspx)  
  
-   [Como publicar aplicativos usando a pré-autenticação do AD FS](../web-application-proxy/Publishing-Applications-using-AD-FS-Preauthentication.md)  
  
-   [Como solucionar problemas de proxy de aplicativo Web](https://technet.microsoft.com/library/dn770156.aspx)  
  


