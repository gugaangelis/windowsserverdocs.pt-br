---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Gerenciamento de certificados SSL no AD FS e no Windows Server 2016
description: Gerenciamento de certificados SSL no AD FS e no Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: abcff8632bc8a3a75af4eee30c3aed046ca0ccc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877337"
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Gerenciamento de certificados SSL no AD FS e no Windows Server 2016

>Aplica-se a: Windows Server 2016

Este artigo descreve como implantar um novo certificado SSL em seus servidores AD FS e WAP.

>[!NOTE]
>A maneira recomendada para substituir o certificado SSL no futuro para um farm do AD FS é usar o Azure AD Connect.  Para obter mais informações, consulte [atualizar o certificado SSL para um farm de serviços de Federação do Active Directory (AD FS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Como obter os certificados SSL
Para farms de servidores de produção do AD FS, um certificado SSL confiável publicamente é recomendado. Isso geralmente é obtido, enviando uma solicitação assinatura de certificado (CSR) a um terceiro, o provedor de certificado público. Há várias maneiras para gerar o CSR, inclusive a partir de um PC superior ou o Windows 7. O fornecedor deve ter a documentação para este.

- Verifique se o certificado atende a [requisitos de certificado do AD FS e o SSL de Proxy de aplicativo Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Quantos certificados são necessários
É recomendável que você use um certificado SSL comuns entre os servidores do AD FS e o Proxy de aplicativo Web. Para obter requisitos detalhados, consulte o documento [requisitos de certificado do AD FS e o SSL de Proxy de aplicativo Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisitos de Certificado SSL
Para requisitos incluindo de nomenclatura, a raiz de confiança e extensões Consulte o documento [requisitos de certificado do AD FS e o SSL de Proxy de aplicativo Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Substituindo o certificado SSL do AD FS
> [!NOTE]
> O certificado SSL do AD FS não é o mesmo que o certificado de comunicações de serviço do AD FS encontrado no snap-in de gerenciamento do AD FS. Para alterar o certificado SSL do AD FS, você precisará usar o PowerShell.

Primeiro, determinar qual modo de associação de certificado servidores do AD FS estão em execução: associação de autenticação de certificado padrão ou modo de associação de TLS do cliente alternativo.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Substituindo o certificado SSL do AD FS, executando o modo de ligação de autenticação de certificado padrão
O AD FS por padrão executa a autenticação de certificado do dispositivo na porta 443 e autenticação de certificado de usuário na porta 49443 (ou uma porta configurável que não é 443).
Nesse modo, use o cmdlet Set-AdfsSslCertificate do powershell para gerenciar o certificado SSL.

Siga os passos abaixo:

1. Primeiro, você precisará obter o novo certificado. Geralmente, isso é feito enviando uma solicitação assinatura de certificado (CSR) a um terceiro, o provedor de certificado público. Há várias maneiras para gerar o CSR, inclusive a partir de um PC superior ou o Windows 7. O fornecedor deve ter a documentação para este.

    * Verifique se o certificado atende a [requisitos de certificado do AD FS e o SSL de Proxy de aplicativo Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Depois de obter a resposta de seu provedor de certificado, importá-lo para o repositório de computador Local em cada servidor do AD FS e Proxy de aplicativo Web.

1. Sobre o **primário** servidor AD FS, use o cmdlet a seguir para instalar o novo certificado SSL

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

A impressão digital do certificado pode ser encontrada executando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Observações adicionais

* O cmdlet Set-AdfsSslCertificate é um cmdlet de vários nó; Isso significa que ele só precisa ser executado do primário e todos os nós no farm serão atualizados. Isso é novo no Server 2016. No Server 2012 R2, você precisava executar Set-AdfsSslCertificate em cada servidor.
* O cmdlet Set-AdfsSslCertificate deve ser executado somente no servidor primário. O servidor primário deve estar executando o Server 2016 e o nível de comportamento de Farm deve ser gerado para o 2016.
* O cmdlet Set-AdfsSslCertificate usará a comunicação remota do PowerShell para configurar servidores do AD FS, verifique se a porta 5985 (TCP) está aberta nos outros nós.
* O cmdlet Set-AdfsSslCertificate concederá adfssrv principal permissões de leitura para as chaves privadas do certificado SSL. Essa entidade representa o serviço AD FS. Não é necessário conceder o acesso de leitura de conta de serviço do AD FS para as chaves privadas do certificado SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Substituindo o certificado SSL do AD FS em execução no modo alternativo de associação de TLS
Quando configurado no cliente alternativo modo de associação de TLS, o AD FS executa autenticação de certificado do dispositivo na porta 443 e a autenticação de certificado de usuário na porta 443, em um nome de host diferente. O nome de host do certificado de usuário é o AD FS hostname precede "certauth", por exemplo "certauth.fs.contoso.com".
Nesse modo, use o cmdlet do powershell Set-AdfsAlternateTlsClientBinding para gerenciar o certificado SSL. Isso irá gerenciar não apenas a associação de TLS do cliente alternativo, mas todas as outras associações na qual o AD FS define o certificado SSL.

Siga os passos abaixo:

1. Primeiro, você precisará obter o novo certificado. Geralmente, isso é feito enviando uma solicitação assinatura de certificado (CSR) a um terceiro, o provedor de certificado público. Há várias maneiras para gerar o CSR, inclusive a partir de um PC superior ou o Windows 7. O fornecedor deve ter a documentação para este.

    * Verifique se o certificado atende a [requisitos de certificado do AD FS e o SSL de Proxy de aplicativo Web](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Depois de obter a resposta de seu provedor de certificado, importá-lo para o repositório de computador Local em cada servidor do AD FS e Proxy de aplicativo Web.

1. Sobre o **primário** servidor AD FS, use o cmdlet a seguir para instalar o novo certificado SSL

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

A impressão digital do certificado pode ser encontrada executando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Observações adicionais

* O cmdlet Set-AdfsAlternateTlsClientBinding é um cmdlet de vários nó; Isso significa que ele só precisa ser executado do primário e todos os nós no farm serão atualizados.
* O cmdlet Set-AdfsAlternateTlsClientBinding deve ser executado somente no servidor primário. O servidor primário deve estar executando o Server 2016 e o nível de comportamento de Farm deve ser gerado para o 2016.
* O cmdlet Set-AdfsAlternateTlsClientBinding usará a comunicação remota do PowerShell para configurar servidores do AD FS, verifique se a porta 5985 (TCP) está aberta nos outros nós.
* O cmdlet Set-AdfsAlternateTlsClientBinding concederá adfssrv principal permissões de leitura para as chaves privadas do certificado SSL. Essa entidade representa o serviço AD FS. Não é necessário conceder o acesso de leitura de conta de serviço do AD FS para as chaves privadas do certificado SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Substituindo o certificado SSL para o Proxy de aplicativo Web
Para configurar a associação de autenticação de certificado padrão ou o modo de associação de TLS do cliente alternativo no WAP, podemos usar o cmdlet Set-WebApplicationProxySslCertificate.
Para substituir o certificado SSL de Proxy de aplicativo Web, em **cada** servidor Proxy de aplicativo Web use o seguinte cmdlet para instalar o novo certificado SSL:

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

Se o cmdlet acima falhar porque o antigo certificado já expirou, reconfigure o proxy usando os cmdlets a seguir:

```powershell
$cred = Get-Credential
```

Insira as credenciais de um usuário de domínio que seja administrador local no servidor do AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Referências adicionais  
* [Suporte do AD FS para associação de nome de host alternativo para autenticação de certificado](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [O AD FS e a propriedade KeySpec informações de certificado](../technical-reference/AD-FS-and-KeySpec-Property.md)
