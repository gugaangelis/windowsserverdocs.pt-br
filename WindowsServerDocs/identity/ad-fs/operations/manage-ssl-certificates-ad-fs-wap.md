---
ms.assetid: a3f50046-5d48-43d3-b0f8-ac2346b15285
title: Gerenciamento de certificados SSL no AD FS e WAP no Windows Server 2016
description: Gerenciamento de certificados SSL no AD FS e WAP no Windows Server 2016
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 10/02/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5156b3ad357ab8edb5e08a89a459beaf9b8c9b1a
ms.sourcegitcommit: ca7dc3d56a33924ae5fe0e9ffb1274da6dc4e54d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2017
---
# <a name="managing-ssl-certificates-in-ad-fs-and-wap-in-windows-server-2016"></a>Gerenciamento de certificados SSL no AD FS e WAP no Windows Server 2016

>Aplica-se a: Windows Server 2016

Este artigo descreve como implantar um novo certificado SSL em seus servidores AD FS e WAP.

>[!NOTE]
>A maneira recomendada para substituir o certificado SSL futuramente para um farm AD FS é usar o Azure AD Connect.  Para obter mais informações, consulte [atualizar o certificado SSL para um farm de serviços de Federação do Active Directory (AD FS)](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectfed-ssl-update)

## <a name="obtaining-your-ssl-certificates"></a>Obtendo os certificados SSL
Para farms de produção do AD FS um certificado SSL publicamente confiável é recomendado. Isso geralmente é obtido, enviando uma solicitação assinatura de certificado (CSR) a terceiros, o provedor de certificado pública. Há diversas maneiras de gerar o CSR, inclusive de um Windows 7 ou o computador mais alta. Seu fornecedor deve ter documentação para isso.

- Verifique se o certificado atende a [requisitos de certificados do AD FS e SSL de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="how-many-certificates-are-needed"></a>Certificados quantos forem necessários
É recomendável que você use um certificado SSL comum em servidores de todos os AD FS e Proxy de aplicativo Web. Para requisitos detalhados, consulte o documento [requisitos de certificados do AD FS e SSL de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

### <a name="ssl-certificate-requirements"></a>Requisitos de certificado SSL
Para requisitos incluindo de nomenclatura, raiz de confiança e extensões Consulte o documento [requisitos de certificados do AD FS e SSL de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

## <a name="replacing-the-ssl-certificate-for-ad-fs"></a>Substituindo o certificado SSL do AD FS
> [!NOTE]
> O certificado SSL do AD FS não é o mesmo que o certificado de comunicações de serviço do AD FS encontrado no snap-in de gerenciamento do AD FS. Para alterar o certificado SSL do AD FS, você precisará usar o PowerShell.

Primeiro, determine qual certificado associando o modo de seus servidores AD FS estão em execução: associação de autenticação de certificado padrão ou modo de vinculação alternativas de cliente TLS.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-default-certificate-authentication-binding-mode"></a>Substituindo o certificado SSL do AD FS em default executando o modo de vinculação de autenticação de certificado
AD FS por padrão executa a autenticação de certificado do dispositivo na porta 443 e autenticação de certificado de usuário na porta 49443 (ou uma porta configurável que não seja 443).
Nesse modo, use o cmdlet do powershell conjunto AdfsSslCertificate para gerenciar o certificado SSL.

Siga as etapas abaixo:

1. Primeiro, você precisará obter o novo certificado. Geralmente, isso é feito enviando uma solicitação assinatura de certificado (CSR) a terceiros, o provedor de certificado pública. Há diversas maneiras de gerar o CSR, inclusive de um Windows 7 ou o computador mais alta. Seu fornecedor deve ter documentação para isso.

    * Verifique se o certificado atende a [requisitos de certificados do AD FS e SSL de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Depois que você receber a resposta do seu provedor de certificado, importe-a para a loja de máquina Local em cada servidor AD FS e Proxy de aplicativo Web.

1. Sobre o **principal** servidor do AD FS, use o seguinte cmdlet para instalar o certificado SSL novo

```powershell
Set-AdfsSslCertificate -Thumbprint '<thumbprint of new cert>'
```

Impressão digital do certificado pode ser encontrado executando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Notas adicionais

* O cmdlet Set-AdfsSslCertificate é um cmdlet de vários nó; Isso significa que ele só precisa executar do principal e todos os nós no farm serão atualizados. Isso é novidade no servidor de 2016. No Server 2012 R2, você precisava executar conjunto AdfsSslCertificate em cada servidor.
* O cmdlet Set-AdfsSslCertificate deve ser executado somente no servidor primário. O servidor primário deve estar em execução Server 2016 e o nível de comportamento Farm deve ser gerado para 2016.
* O cmdlet Set-AdfsSslCertificate usará conexão remota do PowerShell para configurar os servidores do AD FS, certifique-se de porta 5985 (TCP) está aberta nos outros nós.
* O cmdlet Set-AdfsSslCertificate concederá adfssrv principal permissões de leitura para as chaves privadas do certificado SSL. Esse objeto representa o serviço do AD FS. Não é necessário conceder o acesso de leitura de conta de serviço AD FS para as chaves privadas do certificado SSL.

### <a name="replacing-the-ssl-certificate-for-ad-fs-running-in-alternate-tls-binding-mode"></a>Substituindo o certificado SSL do AD FS em execução no modo de associação TLS alternativo
Quando configurado no cliente alternativo modo de vinculação TLS, o AD FS realiza autenticação de certificado do dispositivo na porta 443 e autenticação de certificado de usuário na porta 443 também, em um nome de host diferente. O nome de host do certificado de usuário é o nome de host do AD FS pré-pendente com "certauth", por exemplo "certauth.fs.contoso.com".
Nesse modo, use o cmdlet do powershell conjunto AdfsAlternateTlsClientBinding para gerenciar o certificado SSL. Isso gerenciará não apenas a associação de TLS cliente alternativo, mas todas as outras vinculações no qual o AD FS define o certificado SSL também.

Siga as etapas abaixo:

1. Primeiro, você precisará obter o novo certificado. Geralmente, isso é feito enviando uma solicitação assinatura de certificado (CSR) a terceiros, o provedor de certificado pública. Há diversas maneiras de gerar o CSR, inclusive de um Windows 7 ou o computador mais alta. Seu fornecedor deve ter documentação para isso.

    * Verifique se o certificado atende a [requisitos de certificados do AD FS e SSL de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/AD-FS-2016-Requirements#BKMK_1)

1. Depois que você receber a resposta do seu provedor de certificado, importe-a para a loja de máquina Local em cada servidor AD FS e Proxy de aplicativo Web.

1. Sobre o **principal** servidor do AD FS, use o seguinte cmdlet para instalar o certificado SSL novo

```powershell
Set-AdfsAlternateTlsClientBinding -Thumbprint '<thumbprint of new cert>'
```

Impressão digital do certificado pode ser encontrado executando este comando:

```powershell
dir Cert:\LocalMachine\My\
```

#### <a name="additional-notes"></a>Notas adicionais

* O cmdlet Set-AdfsAlternateTlsClientBinding é um cmdlet de vários nó; Isso significa que ele só precisa executar do principal e todos os nós no farm serão atualizados.
* O cmdlet Set-AdfsAlternateTlsClientBinding deve ser executado somente no servidor primário. O servidor primário deve estar em execução Server 2016 e o nível de comportamento Farm deve ser gerado para 2016.
* O cmdlet Set-AdfsAlternateTlsClientBinding usará conexão remota do PowerShell para configurar os servidores do AD FS, certifique-se de porta 5985 (TCP) está aberta nos outros nós.
* O cmdlet Set-AdfsAlternateTlsClientBinding concederá adfssrv principal permissões de leitura para as chaves privadas do certificado SSL. Esse objeto representa o serviço do AD FS. Não é necessário conceder o acesso de leitura de conta de serviço AD FS para as chaves privadas do certificado SSL.

## <a name="replacing-the-ssl-certificate-for-the-web-application-proxy"></a>Substituindo o certificado SSL para o Proxy de aplicativo Web
Para configurar a associação de autenticação de certificado padrão ou o modo de vinculação do cliente alternativo TLS no WAP podemos usar o cmdlet Set-WebApplicationProxySslCertificate.
Para substituir o certificado SSL de Proxy de aplicativo Web, em **cada** servidor Proxy de aplicativo Web use o seguinte cmdlet para instalar o certificado SSL novos:

```powershell
Set-WebApplicationProxySslCertificate '<thumbprint of new cert>'
```

Se o cmdlet acima falhar porque o certificado antigo já tiver expirado, reconfigure o proxy usando os seguintes cmdlets:

```powershell
$cred = Get-Credential
```

Insira as credenciais de um usuário de domínio que é um administrador local no servidor do AD FS

```powershell
Install-WebApplicationProxy -FederationServiceTrustCredential $cred -CertificateThumbprint '<thumbprint of new cert>' -FederationServiceName 'fs.contoso.com'
```

## <a name="additional-references"></a>Referências adicionais  
* [AD FS dão suporte para associação de nome de host alternativo para autenticação de certificado](../operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [AD FS e certificado KeySpec propriedade informações](../technical-reference/AD-FS-and-KeySpec-Property.md)
