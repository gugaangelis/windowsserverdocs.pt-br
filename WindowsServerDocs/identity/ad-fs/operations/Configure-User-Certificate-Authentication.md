---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurar o suporte do AD FS para autenticação de certificado de usuário
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5c2d84c263517a4c81622ca02538796ccd9da71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817497"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configurar o AD FS para autenticação de certificado de usuário

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

AD FS pode ser configurado para x509 usando um dos modos de autenticação de certificado de usuário descrita na [deste artigo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md). Esse recurso pode ser usado [com o Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) ou por conta própria habilitar clientes e dispositivos provisionados com certificados de usuário para o acesso do AD FS recursos da intranet ou extranet.

## <a name="prerequisites"></a>Pré-requisitos
- Certifique-se de que seus certificados de usuário são confiáveis para servidores de todos os AD FS e WAP
- Verifique se o certificado raiz da cadeia de confiança para seus certificados de usuário está no repositório NTAuth no Active Directory
- Se usando o AD FS no modo de autenticação de certificado alternativo, verifique se seus servidores AD FS e WAP têm certificados SSL que contêm o nome do host do AD FS prefixada com "certauth", por exemplo, "certauth.fs.contoso.com", e esse tráfego para esse nome de host é permitido por meio do firewall
- Se usar a autenticação de certificado da extranet, certifique-se de que pelo menos um AIA e o local de pelo menos um CPD ou OCSP da lista especificada em seus certificados são acessíveis pela internet.
- Se você estiver configurando o AD FS para autenticação de certificado do Azure AD, certifique-se de que você tenha configurado o [configurações do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) e o [necessárias de regras de declaração do AD FS](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) para certificado do emissor e número de série
- Também para autenticação de certificado do AD do Azure, para clientes do Exchange ActiveSync, o certificado do cliente deve ter o endereço de email roteável de usuários no Exchange online do nome da entidade ou o valor de nome RFC822 do campo nome alternativo da entidade. (Do azure Active Directory mapeia o valor de RFC822 para o atributo de endereço de Proxy no diretório.)

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurar o AD FS para autenticação de certificado do usuário  
- Habilitar a autenticação de certificado de usuário como um método de autenticação da extranet do AD FS, usando o console de gerenciamento do AD FS ou o cmdlet do PowerShell Set-AdfsGlobalAuthenticationPolicy ou da intranet
- Certifique-se de que toda a cadeia de confiança, incluindo todos os certificados intermediários, é instalada em cada servidor do AD FS e WAP. Os certificados intermediários devem ser instalados no repositório de autoridades de certificação intermediárias computador local nos servidores de todos os AD FS e WAP.
- Se você quiser usar declarações com base nos campos de certificado e extensões, além de EKU (tipo de declaração https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configurar a passagem de declaração adicionais por meio de regras na relação de confiança de provedor de declarações do Active Directory.  Veja abaixo uma lista completa de declarações de certificado disponíveis.  
- [Opcional] Configurar autoridades de certificação emissora permitidos para certificados de cliente usando as diretrizes em "Gerenciamento de emissores confiáveis para autenticação de cliente" em [deste artigo](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx).

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurar a autenticação de certificado perfeita para o navegador Chrome em áreas de trabalho do Windows
Quando vários certificados de usuário (como certificados de Wi-Fi) estão presentes no computador que satisfazem as finalidades de autenticação de cliente, o navegador Chrome na área de trabalho do Windows solicitará ao usuário para selecionar o certificado correto. Isso pode ser confuso para o usuário final. Para otimizar essa experiência, você pode definir uma política para o Chrome para seleção automática de certificado correto para uma melhor experiência de usuário. Essa política pode ser definida manualmente, fazendo uma alteração no registro ou configurada automaticamente por meio do GPO (para definir as chaves do registro). Isso exige que seus certificados de cliente do usuário para autenticação com o AD FS para ter emissores distintos de outros casos de uso. 

Para obter mais informações sobre como configurar isso para o Chrome, consulte a este [link](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshooting"></a>Solução de problemas
- Se as solicitações de autenticação de certificado falharem com um HTTP 204 "sem conteúdo de https://certauth.fs.contoso.com" resposta, verificar se a raiz e todos os certificados da autoridade de certificação intermediários estão instalados, respectivamente, para a raiz confiável da autoridade de certificação e repositórios em todos os de certificado da autoridade de certificação intermediária servidores de Federação.
- Se as solicitações de autenticação de certificado estão falhando por motivos conhecidos, exporte o certificado de cliente para um arquivo. cer e execute o comando 

`certutil -f -urlfetch -verify certificatefilename.cer`

Garantir que qualquer delta resolver de locais de CRL e a CRL.  Observe que os locais de CRL delta são encontrados com base no conteúdo da CRL Base.

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Referência: Lista completa de certificado de usuário de declaração de tipos e valores de exemplo

|Tipo de declaração|Valor de exemplo
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN = entca, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN = entca, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E=user@contoso.com, CN=user, CN=Users, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E=user@contoso.com, CN=user, CN=Users, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Dados digital de certificado codificado em Base64}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | Bits DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | KeyID=d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | User
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | Outro nome de entidade: nome =user@contoso.com, nome RFC822 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


