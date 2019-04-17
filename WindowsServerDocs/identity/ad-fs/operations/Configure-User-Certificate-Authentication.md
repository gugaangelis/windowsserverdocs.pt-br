---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: "Configurar o AD FS suporte para autenticação de certificado de usuário"
description: 
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.service: active-directory
ms.technology: identity-adfs
ms.openlocfilehash: 9941d4dd997e857874aceddc920ec7f9d8944a81
ms.sourcegitcommit: d351cdbb0bf2533d6db35626ebbc4924b3834309
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/23/2018
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configurando o AD FS para autenticação de certificado de usuário

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

AD FS pode ser configurado para x509 usando um dos modos de autenticação de certificado de usuário descrita em [neste artigo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md). Essa funcionalidade pode ser usada [com o Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/) ou por conta própria permitir que os clientes e dispositivos provisionada com certificados de usuário para acessar o AD FS recursos da intranet ou extranet.

## <a name="prerequisites"></a>Pré-requisitos
- Certifique-se de que os certificados de usuário são confiável pelo AD FS e WAP todos os servidores
- Certifique-se de que o certificado raiz da cadeia de confiança para os certificados de usuário está no arquivo NTAuth no Active Directory
- Se usando o AD FS no modo de autenticação de certificado alternativo, certifique-se de que os servidores do AD FS e WAP certificados SSL contêm o nome do host do AD FS prefixado com "certauth", por exemplo "certauth.fs.contoso.com", e o que o tráfego para esse nome de host é permitido através do firewall
- Se usar a autenticação de certificado de extranet, certifique-se de que pelo menos um AIA e CDP ou OCSP pelo menos uma localização na lista especificada nos seus certificados sejam acessíveis pela internet.
- Se você estiver configurando o AD FS para autenticação de certificado do Azure AD, certifique-se de que você tenha configurado o [configurações do Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) e o [AD FS reivindicar regras necessárias](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) para o emissor de certificado e o número de série
- Também para a autenticação de certificado do Azure AD, para clientes do Exchange ActiveSync, o certificado cliente deve ter o endereço de email pode ser roteado usuários no Exchange online no nome da entidade de segurança ou o valor de nome RFC822 do campo de nome alternativo. (Active Directory do azure mapeia o valor RFC822 para o atributo de endereço do Proxy no diretório.)

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurar o AD FS para autenticação de certificado de usuário  
- Habilitar a autenticação de certificado de usuário como uma intranet ou método de autenticação extranet no AD FS, usando o console de gerenciamento do AD FS ou o cmdlet do PowerShell Set-AdfsGlobalAuthenticationPolicy
- Certifique-se de que toda a cadeia de confiança, incluindo quaisquer certificados intermediários, é instalada em todos os servidores do AD FS e WAP. Os certificados intermediários devem ser instalados ao repositório de autoridades de certificação intermediários de computador local no AD FS e WAP todos os servidores.
- Se você quiser usar declarações com base no certificado campos e extensões além EKU (tipo de declaração https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configure pass reivindicação adicionais por meio de regras na relação de confiança do provedor de declarações do Active Directory.  Veja a seguir uma lista completa de declarações de certificado disponível.  
- [Opcional] Configurar permitidos emissora autoridades de certificação para obter certificados de cliente usando a diretriz em "Gerenciamento de emissores confiáveis para autenticação do cliente" no [neste artigo](https://technet.microsoft.com/en-us/library/dn786429(v=ws.11).aspx).

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurar a autenticação de certificado perfeita para o navegador Chrome em áreas de trabalho do Windows
Quando vários certificados de usuário (como os certificados de Wi-Fi) estão presentes na máquina que satisfazem os fins de autenticação de cliente, o navegador Chrome na área de trabalho do Windows solicitará que o usuário selecione o certificado correto. Isso pode ser confuso para o usuário final. Para otimizar essa experiência, você pode definir uma política para Chrome para autoselecione o certificado correto para uma melhor experiência de usuário. Esta configuração de política pode ser definida manualmente, fazendo uma alteração no registro ou configurada automaticamente por meio do GPO (para definir as chaves do registro). Isso requer os certificados de cliente do usuário para autenticação contra AD FS ter emissores distintas de outros casos de uso. 

Para obter mais informações sobre como configurar isso para Chrome, consulte [link](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshooting"></a>Solução de problemas
- Se as solicitações de autenticação de certificado falhar com uma resposta de HTTP 204 "Não conteúdo de https://certauth.fs.contoso.com", verifique se a raiz e quaisquer certificados de CA intermediários são instalados, respectivamente, para a raiz confiável CA e CA intermediária repositórios em todos os servidores de federação de certificados.
- Se as solicitações de autenticação de certificado falharem por motivos desconhecidos, exportar o certificado de cliente para um arquivo. cer e execute o comando 

`certutil -f -urlfetch -verify certificatefilename.cer`

Certifique-se de qualquer CRL e resolver de locais de CRL delta.  Observe que os locais de CRL delta são encontrados com base no conteúdo da CRL Base.

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Referência: A lista completa de certificado de usuário reivindicar tipos e valores de exemplo

|Tipo de declaração|Valor de exemplo
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN = entca, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN = entca, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E =user@contoso.com, CN = usuário, CN = usuários, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E =user@contoso.com, CN = usuário, CN = usuários, DC = domain, DC = contoso, DC = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Dados de certificado digital codificado em Base64}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | KeyID = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | Usuário
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | Outro nome de entidade de segurança: nome =user@contoso.com, nome RFC822 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


