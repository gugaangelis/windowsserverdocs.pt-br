---
ms.assetid: 98c5ef45-2bcb-4f87-86c8-5ac6c16a6097
title: Certificados de autenticação de tokens
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9db69cfb2eb42af90b392433a6e05eaab9978160
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190812"
---
# <a name="token-signing-certificates"></a>Certificados de autenticação de tokens

Servidores de Federação exigem token\-certificados de assinatura para impedir que invasores alterem ou falsifiquem tokens de segurança em uma tentativa de obter acesso não autorizado a recursos federados. Particular\/chave pública de emparelhamento que é usada com o token\-certificados de assinatura é o mecanismo de validação mais importante de qualquer parceria federada porque essas chaves verificar se um token de segurança foi emitido por um parceiro válido servidor de Federação e que o token não foi modificado durante o trânsito.  
  
## <a name="token-signing-certificate-requirements"></a>Token\-requisitos de certificado de assinatura  
Um token\-certificado de autenticação deve atender aos requisitos a seguir para trabalhar com o AD FS:  
  
-   Para obter um token\-certificado para assinar com êxito um token de segurança, o token de autenticação\-certificado de assinatura deve conter uma chave privada.  
  
-   A conta de serviço do AD FS deve ter acesso ao token\-assinatura de chave privada do certificado no repositório pessoal do computador local. Isso é resolvido pela instalação. Você também pode usar o snap de gerenciamento do AD FS\-para garantir que esse acesso, se você alterar posteriormente o token\-certificado de assinatura.  
  
> [!NOTE]  
> É uma infraestrutura de chave pública \(PKI\) melhor prática para não compartilhar a chave privada para vários fins. Portanto, não use o certificado de comunicação de serviço que você instalou o servidor de federação que o token\-certificado de assinatura.  
  
## <a name="how-token-signing-certificates-are-used-across-partners"></a>Como token\-certificados de autenticação são usados por parceiros  
Todos os tokens\-certificado de assinatura contém chaves privadas de criptografia e chaves públicas que são usadas para assinar digitalmente \(por meio da chave privada\) um token de segurança. Posteriormente, depois que são recebidas por um servidor de Federação do parceiro, essas chaves validam a autenticidade \(por meio da chave pública\) do token de segurança criptografado.  
  
Como cada token de segurança é assinado digitalmente pelo parceiro de conta, o parceiro de recurso pode verificar se o token de segurança foi de fato emitido pelo parceiro de conta e não foi modificado. Assinaturas digitais serão verificadas pela parte de chave pública do token de um parceiro\-certificado de assinatura. Depois que a assinatura é verificada, o servidor de Federação do recurso gera seu próprio token de segurança para sua organização e assina o token de segurança com seu próprio token\-certificado de assinatura.  
  
Para ambientes de parceiro de federação, quando o token\-certificado de autenticação foi emitido por uma autoridade de certificação, certifique-se de que:  
  
1.  As listas de certificados revogados \(CRLs\) do certificado são acessíveis para terceiras partes confiáveis e servidores Web que confiam no servidor de Federação.  
  
2.  O certificado de autoridade de certificação raiz é confiável pelo terceiras partes confiáveis e servidores Web que confiam no servidor de Federação.  
  
O servidor Web no parceiro de recurso usa a chave pública do token\-certificado de autenticação para verificar se o token de segurança está assinado pelo servidor de federação de recurso. O servidor Web, em seguida, permite o acesso apropriado para o cliente.  
  
## <a name="deployment-considerations-for-token-signing-certificates"></a>Considerações de implantação para o token\-certificados de assinatura  
Quando você implanta o primeiro servidor de Federação em uma nova instalação do AD FS, você deve obter um token\-de assinatura de certificado e instalá-lo no repositório de certificados pessoal do computador local nesse servidor de Federação. Você pode obter um token\-o certificado de assinatura por um solicitante de uma empresa autoridade de certificação ou uma autoridade de certificação pública ou criando um self\-certificado autoassinado.  
  
Quando você implanta um farm do AD FS, token\-certificados de autenticação são instalados de forma diferente, dependendo de como você cria o farm de servidores.  
  
Há duas opções de farm de servidor que você pode considerar quando você obter token\-certificados para sua implantação de autenticação:  
  
-   Uma chave privada de um token\-certificado de assinatura é compartilhado entre todos os servidores de Federação em um farm.  
  
    Em um ambiente de farm de servidores de federação, é recomendável que todos os servidores de Federação compartilham \(ou reutilizar\) o mesmo token\-certificado de assinatura. Você pode instalar um único token\-de assinatura de certificado de uma autoridade de certificação em um servidor de Federação e, em seguida, exportar a chave privada, desde que o certificado emitido é marcado como exportável.  
  
    Conforme mostrado na ilustração a seguir, a chave privada de um único token\-certificado de autenticação pode ser compartilhado para todos os servidores de Federação em um farm. Essa opção — em comparação com o seguinte "token exclusivo\-certificado de assinatura" opção — reduz os custos se você planeja obter um token\-certificado de uma CA pública de assinatura.  
  
![assinatura de token](media/adfs2_fedserver_certstory_3.gif)  
  
-   Há um token exclusivo\-certificado de assinatura para cada servidor de Federação em um farm.  
  
    Quando você usa vários, certificados exclusivos em todo seu farm, cada servidor no farm assina tokens com sua própria chave privada exclusivo.  
  
    Conforme mostrado na ilustração a seguir, você pode obter um token separado\-certificado de assinatura para cada servidor de Federação único no farm. Essa opção é mais cara, se você planeja obter seu token\-certificados de uma CA pública de assinatura.  
  
![assinatura de token](media/adfs2_fedserver_certstory_4.gif)  
  
Para obter informações sobre como instalar um certificado ao usar serviços de certificados Microsoft como sua autoridade de certificação corporativa, consulte [IIS 7.0: Criar um certificado de servidor de domínio no IIS 7.0](https://go.microsoft.com/fwlink/?LinkId=108548).  
  
Para obter informações sobre como instalar um certificado de uma CA pública, consulte [IIS 7.0: Solicitar um certificado de servidor de Internet](https://go.microsoft.com/fwlink/?LinkId=108549).  
  
Para obter informações sobre como instalar um self\-assinada do certificado, consulte [IIS 7.0: Criar um Self\-assinou o certificado do servidor no IIS 7.0](https://go.microsoft.com/fwlink/?LinkID=108271).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
