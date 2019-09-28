---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificados de autenticação de tokens
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 02c126fe0792872298a5512abf850af41e1db826
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407928"
---
# <a name="token-signing-certificates"></a>Certificados de autenticação de tokens

Os servidores de Federação\-exigem certificados de autenticação de token para impedir que invasores alterem ou falsificam tokens de segurança em uma tentativa de obter acesso não autorizado a recursos federados. O emparelhamento\/de chave pública privada que é usado com certificados\-de autenticação de token é o mecanismo de validação mais importante de qualquer parceria federada, pois essas chaves verificam se um token de segurança foi emitido por um parceiro válido servidor de Federação e que o token não foi modificado durante o trânsito.  
  
## <a name="token-signing-certificate-requirements"></a>Requisitos\-de certificado de autenticação de token  
Um certificado\-de autenticação de token deve atender aos seguintes requisitos para trabalhar com AD FS:  
  
-   Para que um\-certificado de autenticação de token assine com êxito um token de segurança\-, o certificado de autenticação de token deve conter uma chave privada.  
  
-   A conta de serviço de AD FS deve ter acesso à\-chave privada do certificado de autenticação de token no repositório pessoal do computador local. Isso é resolvido pela instalação. Você também pode usar o snap\-AD FS Management para garantir esse acesso se você alterar posteriormente o certificado de autenticação de tokens.\-  
  
> [!NOTE]  
> É uma prática recomendada de PKI \(\) de infraestrutura de chave pública não compartilhar a chave privada para várias finalidades. Portanto, não use o certificado de comunicação do serviço que você instalou no servidor de Federação como o\-certificado de autenticação de tokens.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Como os\-certificados de assinatura de token são usados entre parceiros  
Cada certificado\-de autenticação de token contém chaves privadas criptográficas e chaves públicas que são usadas para \(assinar digitalmente por meio da\) chave privada um token de segurança. Posteriormente, depois de serem recebidos por um servidor de Federação de parceiro, essas chaves validam \(a autenticidade por meio da chave\) pública do token de segurança criptografado.  
  
Como cada token de segurança é assinado digitalmente pelo parceiro de conta, o parceiro de recurso pode verificar se o token de segurança foi, na verdade, emitido pelo parceiro de conta e se ele não foi modificado. As assinaturas digitais são verificadas pela parte da chave pública do certificado de\-autenticação de token de um parceiro. Depois que a assinatura é verificada, o servidor de Federação de recursos gera seu próprio token de segurança para sua organização e assina o token de\-segurança com seu próprio certificado de autenticação de token.  
  
Para ambientes de parceiros de Federação, quando\-o certificado de autenticação de tokens tiver sido emitido por uma autoridade de certificação, verifique se:  
  
1.  A revogação de certificados \(lista\) as CRLs do certificado podem ser acessadas por partes confiáveis e servidores Web que confiam no servidor de Federação.  
  
2.  O certificado de autoridade de certificação raiz é confiável para as partes confiantes e os servidores Web que confiam no servidor de Federação.  
  
O servidor Web no parceiro de recurso usa a chave pública do certificado de\-autenticação de tokens para verificar se o token de segurança está assinado pelo servidor de Federação de recurso. O servidor Web permite o acesso apropriado ao cliente.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considerações de implantação para\-certificados de assinatura de token  
Ao implantar o primeiro servidor de Federação em uma nova instalação do AD FS, você deve obter um\-certificado de autenticação de token e instalá-lo no repositório de certificados pessoal do computador local nesse servidor de Federação. Você pode obter um certificado\-de autenticação de token solicitando um de uma autoridade de certificação corporativa ou uma AC pública ou\-criando um certificado autoassinado.  
  
Quando você implanta um farm de AD FS,\-os certificados de assinatura de token são instalados de forma diferente, dependendo de como você cria o farm de servidores.  
  
Há duas opções de farm de servidores que você pode considerar ao obter certificados\-de assinatura de token para sua implantação:  
  
-   Uma chave privada de um certificado\-de autenticação de token é compartilhada entre todos os servidores de Federação em um farm.  
  
    Em um ambiente de farm de servidores de Federação, é recomendável que \(todos os\) servidores de Federação\-compartilhem ou reutilizem o mesmo certificado de autenticação de token. Você pode instalar um único certificado\-de autenticação de token de uma AC em um servidor de Federação e, em seguida, exportar a chave privada, desde que o certificado emitido seja marcado como exportável.  
  
    Conforme mostrado na ilustração a seguir, a chave privada de um único certificado\-de autenticação de token pode ser compartilhada para todos os servidores de Federação em um farm. Essa opção — em comparação com a opção "certificado\-de assinatura de token exclusivo" a seguir — reduz os custos se você\-planeja obter um certificado de autenticação de token de uma AC pública.  
  
![assinatura de token](media/adfs2_fedserver_certstory_3.gif)  
  
-   Há um certificado de autenticação\-de token exclusivo para cada servidor de Federação em um farm.  
  
    Quando você usa vários certificados exclusivos em todo o farm, cada servidor desse farm assina tokens com sua própria chave privada exclusiva.  
  
    Conforme mostrado na ilustração a seguir, você pode obter um certificado de\-autenticação de tokens separado para cada servidor de Federação único no farm. Essa opção será mais cara se você planeja obter seus certificados de\-assinatura de token de uma AC pública.  
  
![assinatura de token](media/adfs2_fedserver_certstory_4.gif)  
  
Para obter informações sobre como instalar um certificado ao usar os serviços de certificados da Microsoft como sua [AC corporativa, consulte IIS 7,0: Crie um certificado do servidor de domínio no](https://go.microsoft.com/fwlink/?LinkId=108548)IIS 7,0.  
  
Para obter informações sobre como instalar um certificado de uma AC pública [, consulte IIS 7,0: Solicitar um certificado de servidor de Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Para obter informações sobre como instalar\-um certificado autoassinado [, consulte IIS 7,0: Crie um\-certificado de servidor autoassinado no IIS](https://go.microsoft.com/fwlink/?LinkID=108271)7,0.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
