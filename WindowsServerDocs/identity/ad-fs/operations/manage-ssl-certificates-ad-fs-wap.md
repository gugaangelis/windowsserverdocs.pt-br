---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Gerenciamento de certificados SSL no AD FS e no Windows Server 2016
description: Gerenciamento de certificados SSL no AD FS e no Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.openlocfilehash: cc48e3efc783665921519272443e86620dcd4d4a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962470"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Gerenciamento de certificados SSL no AD FS e no Windows Server 2016



Este artigo descreve como implantar um novo certificado SSL em seus servidores AD FS e WAP.

>[!NOTE]
>A maneira recomendada para substituir o certificado SSL que está sendo encaminhado para um farm de AD FS é usar Azure AD Connect.  Para obter mais informações, consulte [atualizar o certificado SSL para um farm de serviços de Federação do Active Directory (AD FS) (AD FS)](/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Obtendo seus certificados SSL
Para os farms de AD FS de produção, é recomendado um certificado SSL confiável publicamente. Normalmente, isso é obtido com o envio de um CSR (solicitação de assinatura de certificado) para um provedor de certificados público de terceiros. Há várias maneiras de gerar o CSR, incluindo de um PC com Windows 7 ou superior. Seu fornecedor deve ter a documentação para isso.

- Verifique se o certificado atende aos [requisitos de certificado SSL do AD FS e do proxy de aplicativo Web](../overview/ad-fs-requirements.md#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Quantos certificados são necessários
É recomendável que você use um certificado SSL comum em todos os AD FS e servidores de proxy de aplicativo Web. Para obter os requisitos detalhados, consulte o documento [AD FS e requisitos de certificado SSL do proxy de aplicativo Web](../overview/ad-fs-requirements.md#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisitos de Certificado SSL
Para requisitos, incluindo nomenclatura, raiz de confiança e extensões, consulte o documento [AD FS e requisitos de certificado SSL de proxy de aplicativo Web](../overview/ad-fs-requirements.md#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Substituindo o certificado SSL para AD FS
> [!NOTE]
> O certificado SSL do AD FS não é o mesmo que o certificado de comunicações do Serviço do AD FS encontrado no snap-in Gerenciamento do AD FS. Para alterar o AD FS certificado SSL, você precisará usar o PowerShell.

Primeiro, determine qual modo de associação de certificado seus servidores de AD FS estão executando: Associação de autenticação de certificado padrão ou modo de associação de TLS de cliente alternativo.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Substituindo o certificado SSL para AD FS em execução no modo de associação de autenticação de certificado padrão
O AD FS, por padrão, executa a autenticação de certificado de dispositivo na porta 443 e autenticação de certificado de usuário na porta 49443 (ou uma porta configurável que não seja 443).
Nesse modo, use o cmdlet do PowerShell Set-AdfsSslCertificate para gerenciar o certificado SSL.

Siga as etapas abaixo:

1. Primeiro, será necessário obter o novo certificado. Isso geralmente é feito enviando uma solicitação de assinatura de certificado (CSR) para um provedor de certificados público de terceiros. Há várias maneiras de gerar o CSR, incluindo de um PC com Windows 7 ou superior. Seu fornecedor deve ter a documentação para isso.

    * Verifique se o certificado atende aos [requisitos de certificado SSL do AD FS e do proxy de aplicativo Web](../overview/ad-fs-requirements.md#BKMK_1)

1. Depois de obter a resposta do seu provedor de certificado, importe-a para o repositório do computador local em cada AD FS e servidor de proxy de aplicativo Web.

1. No servidor de AD FS **primário** , use o seguinte cmdlet para instalar o novo certificado SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

A impressão digital do certificado pode ser encontrada executando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Observações adicionais

* O cmdlet Set-AdfsSslCertificate é um cmdlet com vários nós; Isso significa que ele só precisa ser executado a partir do primário e todos os nós no farm serão atualizados. Isso é novo no servidor 2016. No Server 2012 R2, era necessário executar Set-AdfsSslCertificate em cada servidor.
* O cmdlet Set-AdfsSslCertificate deve ser executado somente no servidor primário. O servidor primário precisa estar executando o servidor 2016 e o nível de comportamento do farm deve ser aumentado para 2016.
* O cmdlet Set-AdfsSslCertificate usará a comunicação remota do PowerShell para configurar os outros servidores de AD FS, verifique se a porta 5985 (TCP) está aberta nos outros nós.
* O cmdlet Set-AdfsSslCertificate concederá as permissões de leitura principal do adfssrv para as chaves privadas do certificado SSL. Essa entidade representa o serviço AD FS. Não é necessário conceder ao AD FS acesso de leitura à conta de serviço para as chaves privadas do certificado SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Substituindo o certificado SSL para AD FS em execução no modo de associação TLS alternativo
Quando configurado em modo de ligação TLS de cliente alternativo, AD FS executa a autenticação de certificado de dispositivo na porta 443 e autenticação de certificado de usuário na porta 443 também, em um nome de host diferente. O nome do host do certificado de usuário é o AD FS nome de host autônomo com "certauth", por exemplo, "certauth.fs.contoso.com".
Nesse modo, use o cmdlet do PowerShell Set-AdfsAlternateTlsClientBinding para gerenciar o certificado SSL. Isso gerenciará não apenas a associação de TLS de cliente alternativa, mas todas as outras associações nas quais AD FS também define o certificado SSL.

Siga as etapas abaixo:

1. Primeiro, será necessário obter o novo certificado. Isso geralmente é feito enviando uma solicitação de assinatura de certificado (CSR) para um provedor de certificados público de terceiros. Há várias maneiras de gerar o CSR, incluindo de um PC com Windows 7 ou superior. Seu fornecedor deve ter a documentação para isso.

    * Verifique se o certificado atende aos [requisitos de certificado SSL do AD FS e do proxy de aplicativo Web](../overview/ad-fs-requirements.md#BKMK_1)

1. Depois de obter a resposta do seu provedor de certificado, importe-a para o repositório do computador local em cada AD FS e servidor de proxy de aplicativo Web.

1. No servidor de AD FS **primário** , use o seguinte cmdlet para instalar o novo certificado SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

A impressão digital do certificado pode ser encontrada executando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Observações adicionais

* O cmdlet Set-AdfsAlternateTlsClientBinding é um cmdlet com vários nós; Isso significa que ele só precisa ser executado a partir do primário e todos os nós no farm serão atualizados.
* O cmdlet Set-AdfsAlternateTlsClientBinding deve ser executado somente no servidor primário. O servidor primário precisa estar executando o servidor 2016 e o nível de comportamento do farm deve ser aumentado para 2016.
* O cmdlet Set-AdfsAlternateTlsClientBinding usará a comunicação remota do PowerShell para configurar os outros servidores de AD FS, verifique se a porta 5985 (TCP) está aberta nos outros nós.
* O cmdlet Set-AdfsAlternateTlsClientBinding concederá as permissões de leitura principal do adfssrv para as chaves privadas do certificado SSL. Essa entidade representa o serviço AD FS. Não é necessário conceder ao AD FS acesso de leitura à conta de serviço para as chaves privadas do certificado SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Substituindo o certificado SSL para o proxy de aplicativo Web
Para configurar a associação de autenticação de certificado padrão ou o modo de associação TLS de cliente alternativo no WAP, podemos usar o cmdlet Set-WebApplicationProxySslCertificate.
Para substituir o certificado SSL do proxy de aplicativo Web, em **cada** servidor proxy de aplicativo Web, use o seguinte cmdlet para instalar o novo certificado SSL:

```powershell
Set-WebApplicationProxySslCertificate -Thumbprint '<thumbprint of new cert>'
```

Se o cmdlet acima falhar porque o certificado antigo já expirou, reconfigure o proxy usando os seguintes cmdlets:

```powershell
$cred = Get-Credential
```

Insira as credenciais de um usuário de domínio que seja administrador local no servidor de AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Referências adicionais
* [Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [Informações da propriedade KeySpec AD FS e do certificado](../technical-reference/AD-FS-and-KeySpec-Property.md)
