---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: "Certificados de autenticação de token"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 84e106ec87861960d7de651b4aaa0e88c952aa79
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="token-signing-certificates"></a>Certificados de autenticação de token

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores de Federação exigem certificados de assinatura de token\ para impedir que invasores alterem ou falsifiquem tokens de segurança em uma tentativa de obter acesso não autorizado a recursos federados. A chave pública/private\ emparelhamento ou seja usado com certificados de assinatura de token\ é o mecanismo de validação mais importante de qualquer parceria federado porque essas chaves verificar se um token de segurança foi emitido por um servidor de Federação do parceiro válido e se o token não foi modificado durante o trânsito.  
  
## <a name="token-signing-certificate-requirements"></a>Assinatura de Token\ requisitos de certificado  
Um certificado de assinatura de token\ deve atender aos seguintes requisitos para trabalhar com o AD FS:  
  
-   Para um certificado de assinatura de token\ entrar com êxito um token de segurança, o certificado de assinatura de token\ deve conter uma chave privada.  
  
-   A conta de serviço do AD FS deve ter acesso à chave privada do certificado de assinatura de token\ no armazenamento pessoal do computador local. Isso é controlado pela instalação. Você também pode usar o AD FS snap\-in Gerenciamento para garantir que esse acesso se você alterar o certificado de assinatura de token\ posteriormente.  
  
> [!NOTE]  
> É uma prática recomendada de \(PKI\) de infraestrutura de chave pública para não compartilhar a chave privada para várias finalidades. Portanto, não use o certificado de comunicação de serviço que você instalou no servidor de federação como o certificado de assinatura de token\.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Como os certificados de assinatura de token\ são usados entre parceiros  
Cada certificado de assinatura de token\ contém chaves privadas de criptografia e chaves públicas são usadas para assinar digitalmente \ (por meio do key\ privada) um token de segurança. Posteriormente, depois que eles são recebidos por um servidor de federação de parceiro, essas chaves validam a autenticidade \ (por meio do key\ pública) do token de segurança criptografado.  
  
Como cada token de segurança é assinado digitalmente pelo parceiro de conta, o parceiro de recurso pode verificar se o token de segurança na verdade foi emitido pelo parceiro de conta e se ele não foi modificado. Assinaturas digitais são verificadas por parte da chave pública do certificado de canto token\ do parceiro. Depois que a assinatura é verificada, o servidor de Federação do recurso gera seu próprio token de segurança para sua organização e assina o token de segurança com seu próprio certificado de assinatura de token\.  
  
Para ambientes de parceiros de federação, quando o certificado de assinatura de token\ foi emitido por uma autoridade de certificação, certifique-se de que:  
  
1.  O certificado revogação listas \(CRLs\) do certificado estão acessíveis a partes confiantes e servidores Web que confiar no servidor de Federação.  
  
2.  O certificado da CA raiz é confiável pelo partes confiantes e servidores Web que confiar no servidor de Federação.  
  
O servidor Web no parceiro de recurso usa a chave pública do certificado de assinatura de token\ para verificar se o token de segurança é assinado pelo servidor de federação de recurso. O servidor Web, em seguida, permite o acesso apropriado para o cliente.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considerações sobre a implantação de certificados de autenticação de token\  
Quando você implanta o primeiro servidor de Federação em uma nova instalação do AD FS, você deve obter um certificado de assinatura de token\ e instalá-lo no repositório de certificados pessoal do computador local nesse servidor de Federação. Você pode obter uma assinatura de token\ certificados por com uma empresa solicitante uma CA ou uma autoridade de certificação pública ou criando um certificado assinado self\.  
  
Quando você implanta um farm AD FS, certificados de autenticação de token\ são instalados de forma diferente, dependendo de como criar o farm de servidores.  
  
Há duas opções de farm de servidor que você pode considerar ao obter certificados de autenticação de token\ sua implantação:  
  
-   Uma chave privada de um certificado de assinatura de token\ é compartilhada entre todos os servidores de Federação em um farm.  
  
    Em um ambiente de farm de servidor de federação, recomendamos que todos os servidores de Federação compartilham o mesmo certificado de assinatura de token\ \(or reuse\). Você pode instalar um certificado de assinatura de token\ único de uma autoridade de certificação em um servidor de Federação e, em seguida, exportar a chave privada, desde que o certificado emitido é marcado como exportable.  
  
    Conforme mostrado na ilustração a seguir, a chave privada de um único certificado de assinatura de token\ pode ser compartilhada para todos os servidores de Federação em um farm. Essa opção — em comparação com o seguinte "exclusivo token\ certificado de autenticação" opção — reduz os custos se você pretende obter um certificado de assinatura de token\ de uma autoridade de certificação pública.  
  
![token de assinatura](media/adfs2_fedserver_certstory_3.gif)  
  
-   Há um certificado de assinatura de token\ exclusivo para cada servidor de Federação em um farm.  
  
    Quando você usa vários, certificados exclusivos em todo seu farm, cada servidor nessa farm assina tokens com sua própria chave privada exclusivo.  
  
    Conforme mostrado na ilustração a seguir, você pode obter um certificado de assinatura de token\ separado para cada servidor de Federação único no farm. Essa opção é mais cara, se você pretende obter os certificados de assinatura de token\ de uma autoridade de certificação pública.  
  
![token de assinatura](media/adfs2_fedserver_certstory_4.gif)  
  
Para obter informações sobre como instalar um certificado quando você usar os serviços de certificado Microsoft como sua autoridade de certificação, consulte [IIS 7.0: criar um certificado de servidor de domínio no IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Para obter informações sobre como instalar um certificado de uma autoridade de certificação pública, consulte [IIS 7.0: solicitar um certificado de servidor de Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Para obter informações sobre como instalar um certificado assinado self\, consulte [IIS 7.0: criar um certificado de servidor Self\-Signed no IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
