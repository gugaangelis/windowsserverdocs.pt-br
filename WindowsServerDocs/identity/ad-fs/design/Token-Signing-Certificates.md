---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificados de autenticação de tokens
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 24b095827ad074dbe249a9d4decd8edeecbb3603
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858779"
---
# <a name="token-signing-certificates"></a>Certificados de autenticação de tokens

Os servidores de Federação exigem certificados de assinatura de\-de token para impedir que invasores alterem ou falsificam tokens de segurança em uma tentativa de obter acesso não autorizado a recursos federados. O emparelhamento de chave pública\/privada que é usado com certificados de assinatura de\-de token é o mecanismo de validação mais importante de qualquer parceria federada, pois essas chaves verificam se um token de segurança foi emitido por um servidor de Federação de parceiro válido e se o token não foi modificado durante o trânsito.  
  
## <a name="token-signing-certificate-requirements"></a>Requisitos de certificado de assinatura\-de token  
Um certificado de assinatura de\-de token deve atender aos seguintes requisitos para trabalhar com AD FS:  
  
-   Para um token\-certificado de autenticação para assinar com êxito um token de segurança, o token\-certificado de assinatura deve conter uma chave privada.  
  
-   A conta de serviço do AD FS deve ter acesso ao token\-chave privada do certificado de assinatura no repositório pessoal do computador local. Isso é resolvido pela instalação. Você também pode usar o\-snap de gerenciamento de AD FS no para garantir esse acesso se você alterar posteriormente o token\-certificado de autenticação.  
  
> [!NOTE]  
> É uma infraestrutura de chave pública \(PKI\) prática recomendada para não compartilhar a chave privada para várias finalidades. Portanto, não use o certificado de comunicação do serviço que você instalou no servidor de Federação como o token\-certificado de autenticação.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Como o token\-certificados de assinatura são usados entre parceiros  
Cada token\-certificado de autenticação contém chaves privadas criptográficas e chaves públicas que são usadas para assinar digitalmente \(por meio da chave privada\) um token de segurança. Posteriormente, depois de serem recebidos por um servidor de Federação de parceiro, essas chaves validam o \(de autenticidade por meio da\) de chave pública do token de segurança criptografado.  
  
Como cada token de segurança é assinado digitalmente pelo parceiro de conta, o parceiro de recurso pode verificar se o token de segurança foi, na verdade, emitido pelo parceiro de conta e se ele não foi modificado. As assinaturas digitais são verificadas pela parte da chave pública do token do parceiro\-certificado de assinatura. Depois que a assinatura é verificada, o servidor de Federação de recursos gera seu próprio token de segurança para sua organização e assina o token de segurança com seu próprio token\-certificado de assinatura.  
  
Para ambientes de parceiros de Federação, quando o token\-certificado de assinatura tiver sido emitido por uma autoridade de certificação, verifique se:  
  
1.  A revogação de certificado lista \(as CRLs\) do certificado são acessíveis a partes confiáveis e servidores Web que confiam no servidor de Federação.  
  
2.  O certificado de autoridade de certificação raiz é confiável para as partes confiantes e os servidores Web que confiam no servidor de Federação.  
  
O servidor Web no parceiro de recurso usa a chave pública do token\-certificado de autenticação para verificar se o token de segurança está assinado pelo servidor de Federação de recursos. O servidor Web permite o acesso apropriado ao cliente.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considerações de implantação para certificados de assinatura de\-de token  
Ao implantar o primeiro servidor de Federação em uma nova instalação do AD FS, você deve obter um token\-certificado de autenticação e instalá-lo no repositório de certificados pessoal do computador local nesse servidor de Federação. Você pode obter um token\-certificado de assinatura solicitando um de uma autoridade de certificação corporativa ou uma AC pública ou criando um certificado auto\-assinado.  
  
Quando você implanta um farm de AD FS, os certificados de assinatura de\-de token são instalados de forma diferente, dependendo de como você cria o farm de servidores.  
  
Há duas opções de farm de servidores que você pode considerar ao obter tokens\-certificados de assinatura para sua implantação:  
  
-   Uma chave privada de um token\-certificado de assinatura é compartilhada entre todos os servidores de Federação em um farm.  
  
    Em um ambiente de farm de servidores de Federação, é recomendável que todos os servidores de Federação compartilhem \(ou reutilizem\) mesmo token\-certificado de autenticação. Você pode instalar um único token\-certificado de assinatura de uma AC em um servidor de Federação e, em seguida, exportar a chave privada, desde que o certificado emitido seja marcado como exportável.  
  
    Conforme mostrado na ilustração a seguir, a chave privada de um único token\-certificado de assinatura pode ser compartilhada para todos os servidores de Federação em um farm. Essa opção — em comparação com a seguinte opção "certificado de assinatura de\-de token único" — reduz os custos se você planeja obter um token\-certificado de assinatura de uma AC pública.  
  
![assinatura de token](media/adfs2_fedserver_certstory_3.gif)  
  
-   Há um token exclusivo\-certificado de assinatura para cada servidor de Federação em um farm.  
  
    Quando você usa vários certificados exclusivos em todo o farm, cada servidor desse farm assina tokens com sua própria chave privada exclusiva.  
  
    Conforme mostrado na ilustração a seguir, você pode obter um token separado\-certificado de assinatura para cada servidor de Federação único no farm. Essa opção é mais cara se você planeja obter o token\-certificados de assinatura de uma CA pública.  
  
![assinatura de token](media/adfs2_fedserver_certstory_4.gif)  
  
Para obter informações sobre como instalar um certificado ao usar os serviços de certificados da Microsoft como sua AC corporativa, consulte [IIS 7,0: criar um certificado do servidor de domínio no IIS 7,0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Para obter informações sobre como instalar um certificado de uma AC pública, consulte [IIS 7,0: solicitar um certificado de servidor de Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Para obter informações sobre como instalar um certificado auto\-assinado, consulte [iis 7,0: criar um certificado de servidor assinado por si mesmo\-no IIS 7,0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
