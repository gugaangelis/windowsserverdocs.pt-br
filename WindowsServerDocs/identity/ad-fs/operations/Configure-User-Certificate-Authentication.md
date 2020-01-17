---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurar suporte de AD FS para autenticação de certificado de usuário
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c36555a8bca7882125451b2c86a0707e3de9b2db
ms.sourcegitcommit: 8771a9f5b37b685e49e2dd03c107a975bf174683
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76145922"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configurando AD FS para autenticação de certificado de usuário

A autenticação de certificado de usuário é usada principalmente em dois casos de uso
* Os usuários estão usando cartões inteligentes para entrar em seu sistema de AD FS
* Os usuários estão usando certificados provisionados para dispositivos móveis


## <a name="prerequisites"></a>Pré-requisitos
1) Determine o modo de autenticação de certificado de usuário AD FS que você deseja habilitar usando um dos modos descritos neste [artigo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
2) Verifique se a cadeia confiável de certificado de usuário está instalada & confiável por todos os AD FS e servidores WAP, incluindo quaisquer autoridades de certificação intermediárias. Geralmente isso é feito por meio de GPO em servidores AD FS/WAP
3)  Verifique se o certificado raiz da cadeia de confiança para os certificados de usuário está no repositório NTAuth no Active Directory
4) Se estiver usando AD FS no modo de autenticação de certificado alternativo, certifique-se de que os servidores AD FS e WAP tenham certificados SSL que contenham o nome de host AD FS prefixado com "certauth", por exemplo, "certauth.fs.contoso.com", e que o tráfego para esse nome de host é permitido por meio do firewall
5) Se estiver usando a autenticação de certificado da extranet, verifique se pelo menos um AIA e pelo menos um local de CDP ou OCSP da lista especificada em seus certificados estão acessíveis pela Internet.
6) Além disso, para a autenticação de certificado do Azure AD, para clientes do Exchange ActiveSync, o certificado do cliente deve ter o endereço de email roteável dos usuários no Exchange Online no nome da entidade de segurança ou no valor do nome do RFC822 do campo nome alternativo da entidade. (Azure Active Directory mapeia o valor de RFC822 para o atributo de endereço de proxy no diretório.)


## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurar o AD FS para autenticação de certificado do usuário  

Habilite a autenticação de certificado de usuário como um método de autenticação de intranet ou Extranet no AD FS, usando o console de gerenciamento AD FS ou o cmdlet do PowerShell `Set-AdfsGlobalAuthenticationPolicy`.

Se estiver configurando AD FS para autenticação de certificado do Azure AD, verifique se você definiu as [configurações do Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) e as [regras de declaração de AD FS necessárias](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) para o emissor do certificado e o número de série

Além disso, há alguns aspectos opcionais.
- Se você quiser usar declarações baseadas em campos de certificado e extensões, além do EKU (tipo de declaração https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku), configure as regras adicionais de passagem de declaração na relação de confiança do provedor de declarações Active Directory.  Veja abaixo uma lista completa de declarações de certificado disponíveis.  
- Se você precisar restringir o acesso com base no tipo de certificado, poderá usar as propriedades adicionais no certificado em AD FS regras de autorização de emissão para o aplicativo. Os cenários comuns são "permitir somente certificados provisionados por um provedor de MDM" ou "permitir somente certificados de cartão inteligente"
- Configure as autoridades de certificação de emissão permitidas para certificados de cliente usando as diretrizes em "gerenciamento de emissores confiáveis para autenticação de cliente" neste [artigo](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx).
- Convém considerar a modificação das páginas de entrada para atender às necessidades de seus usuários finais ao fazer a autenticação do certificado. Os casos comuns são para (a) alterar "entrar com seu certificado X509" para algo mais amigável para o usuário final

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurar a autenticação de certificado contínuo para o navegador Chrome em áreas de trabalho do Windows
Quando vários certificados de usuário (como certificados Wi-Fi) estiverem presentes no computador que atendem às finalidades da autenticação de cliente, o navegador Chrome no Windows Desktop solicitará que o usuário selecione o certificado correto. Isso pode ser confuso para o usuário final. Para otimizar essa experiência, você pode definir uma política para Chrome para selecionar automaticamente o certificado certo para uma melhor experiência do usuário. Essa política pode ser definida manualmente, fazendo com que um registro seja alterado ou configurado automaticamente por meio de GPO (para definir as chaves do registro). Isso exige que os certificados de cliente do usuário para autenticação no AD FS tenham emissores distintos de outros casos de uso. 

Para obter mais informações sobre como configurar isso para o Chrome, consulte este [link](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshoot-certificate-authentication"></a>Solucionar problemas de autenticação de certificado
Este documento se concentra em problemas comuns de solução de problema quando AD FS está configurado para autenticação de certificado para usuários. 

### <a name="check-if-certificate-trusted-issuers-is-configured-properly-in-all-the-ad-fswap-servers"></a>Verificar se os emissores confiáveis do certificado estão configurados corretamente em todos os servidores AD FS/WAP
*Sintoma comum: HTTP 204 "nenhum conteúdo do HTTPS\://certauth.adfs.contoso.com"*

AD FS usa o sistema operacional Windows subjacente para provar a posse do certificado de usuário e garantir que ele corresponda a um emissor confiável fazendo a validação da cadeia de certificados confiáveis. Para corresponder ao emissor confiável, você precisará garantir que todas as autoridades raiz e intermediárias sejam configuradas como emissores confiáveis no repositório de autoridades de certificação do computador local. Para validar isso automaticamente, use a [ferramenta AD FS Diagnostic Analyzer](https://adfshelp.microsoft.com/DiagnosticsAnalyzer/Analyze). A ferramenta consulta todos os servidores e garante que os certificados corretos sejam provisionados corretamente. 
1)  Baixe e execute a ferramenta de acordo com as instruções fornecidas no link acima
2)  Carregar os resultados e examinar as falhas

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>Verifique se a autenticação de certificado está habilitada na política de autenticação de AD FS
O AD FS não habilita a autenticação de certificado por padrão. Consulte o início deste documento sobre como habilitar a autenticação de certificado. 

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>Verifique se a autenticação de certificado está habilitada na política de autenticação de AD FS
O AD FS faz a autenticação de certificado de usuário por padrão na porta 49443 com o mesmo nome de host que AD FS (por exemplo, `adfs.contoso.com`). Você também pode configurar AD FS para usar a porta 443 (porta HTTPS padrão) usando a associação SSL alternativa. No entanto, a URL usada nessa configuração é `certauth.<adfs-farm-name>` (por exemplo, `certauth.contoso.com`). Consulte [este link](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md) para obter mais informações. O caso mais comum de conectividade de rede é que um firewall foi configurado incorretamente e bloqueia ou interfere o tráfego de autenticação de certificado do usuário. Normalmente, você verá uma tela em branco ou um erro de servidor 500 quando esse problema ocorrer. 
1)  Observe o nome do host e a porta que você configurou no AD FS
2)  Verifique se qualquer firewall na frente do AD FS ou do WAP (proxy de aplicativo Web) está configurado para permitir a combinação de `hostname:port` para o farm de AD FS. Você precisará consultar seu engenheiro de rede para executar esta etapa. 

### <a name="check-certificate-revocation-list-connectivity"></a>Verificar conectividade da lista de certificados revogados
As listas de certificados revogados (CRL) são pontos de extremidade codificados no certificado do usuário para executar verificações de revogação de tempo de execução. Por exemplo, se um dispositivo que continha um certificado for roubado, um administrador poderá adicionar o certificado à lista de certificados revogados. Qualquer ponto de extremidade que aceitou esse certificado anteriormente agora falharia na autenticação.

Cada AD FS e servidor WAP precisarão acessar o ponto de extremidade da CRL para validar se o certificado apresentado a ele ainda for válido e não tiver sido revogado. A validação de CRL pode ocorrer via HTTPS, HTTP, LDAP ou via OCSP (protocolo de status de certificado online). Se os servidores AD FS/WAP não puderem alcançar o ponto de extremidade, a autenticação falhará. Siga as etapas abaixo para solucionar o problema. 
1) Consulte seu engenheiro PKI para determinar os pontos de extremidade da CRL usados para revogar certificados de usuário do seu sistema PKI. 
2)  Em cada servidor AD FS/WAP, verifique se os pontos de extremidade da CRL estão acessíveis por meio do protocolo usado (normalmente HTTPS ou HTTP)
3)  Para validação avançada, [habilite o log de eventos CAPI2](https://blogs.msdn.microsoft.com/benjaminperkins/2013/09/30/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues/) em cada servidor AD FS/WAP
4) Verificar a ID do evento 41 (verificar revogação) nos logs operacionais do CAPI2
5) Verificar `‘\<Result value="80092013"\>The revocation function was unable to check revocation because the revocation server was offline.\</Result\>'`

***Dica***: você pode direcionar um único servidor de AD FS ou WAP para facilitar a solução de problemas Configurando a resolução de DNS (arquivo de hosts no Windows) para apontar para um servidor específico. Isso permite que você habilite o rastreamento direcionado a um servidor. 

### <a name="check-if-this-is-a-server-name-indication-sni-issue"></a>Verificar se este é um problema de Indicação de Nome de Servidor (SNI)
AD FS requer que o dispositivo cliente (ou navegadores) e os balanceadores de carga ofereçam suporte ao SNI. Alguns dispositivos cliente (geralmente versões mais antigas do Android) podem não dar suporte a SNI. Além disso, os balanceadores de carga podem não dar suporte ao SNI ou não foram configurados para SNI. Nessas instâncias, é provável que você veja as falhas de certificação do usuário. 
1)  Trabalhe com seu engenheiro de rede para garantir que o Load Balancer para AD FS/WAP dê suporte ao SNI
2)  Caso o SNI não tenha suporte AD FS tenha uma solução alternativa seguindo as etapas abaixo
    *   Abrir uma janela de prompt de comandos com privilégios elevados no servidor de AD FS primário
    *   Digite ```Netsh http show sslcert```
    *   Copiar o ' GUID do aplicativo ' e o ' hash de certificado ' do serviço de Federação
    *   Digite `netsh http add sslcert ipport=0.0.0.0:{your_certauth_port} certhash={your_certhash} appid={your_applicaitonGUID}`

### <a name="check-if-the-client-device-has-been-provisioned-with-the-certificate-correctly"></a>Verificar se o dispositivo cliente foi provisionado corretamente com o certificado
Você pode observar que alguns dispositivos estão funcionando corretamente, mas outros dispositivos não. Nesse caso, geralmente é um resultado do certificado do usuário não ser provisionado corretamente no dispositivo cliente. Siga as etapas abaixo. 
1)  Se o problema for específico de um dispositivo Android, o problema mais comum é que a cadeia de certificados não é totalmente confiável no dispositivo Android.  Consulte seu fornecedor de MDM para verificar se o certificado foi provisionado corretamente e se toda a cadeia é totalmente confiável no dispositivo Android. 
2)  Se o problema for específico para um dispositivo Windows, verifique se o certificado foi provisionado corretamente verificando o repositório de certificados do Windows para o usuário conectado (não sistema/computador).
3)  Exporte o certificado de usuário do cliente para o arquivo. cer e execute o comando ' certutil-f-urlfetch-Verify certificatefilename. cer '


### <a name="check-if-the-tls-version-is-compatible-between-ad-fswap-servers-and-the-client-device"></a>Verifique se a versão do TLS é compatível entre os servidores AD FS/WAP e o dispositivo cliente
Em casos raros, um dispositivo cliente (normalmente dispositivos móveis) é atualizado para dar suporte apenas a uma versão mais recente do TLS (digamos, 1,3) ou você pode ter o problema inverso em que os servidores AD FS/WAP foram atualizados para usar apenas uma versão de TLS mais alta e o dispositivo cliente não dá suporte a ele. Você pode usar as ferramentas SSL online para verificar seus servidores AD FS/WAP e ver se ele é compatível com o dispositivo. Para obter mais informações sobre como controlar as versões de TLS, consulte [este link](manage-ssl-protocols-in-ad-fs.md).

### <a name="check-if-azure-ad-promptloginbehavior-is-configured-correctly-on-your-federated-domain-settings"></a>Verifique se o Azure AD PromptLoginBehavior está configurado corretamente em suas configurações de domínio federado
Muitos aplicativos do Office 365 enviam prompt = logon ao Azure AD. O AD do Azure, por padrão, converte-o em um logon atualizado de senha para AD FS. Como resultado, mesmo que você tenha configurado a autenticação de certificado no AD FS, os usuários finais verão apenas um logon de senha. 
1)  Obter as configurações de domínio federado usando o comando "Get-MsolDomainFederationSettings" Let
2)  Verifique se o parâmetro PromptLoginBehavior está definido como um ' disabled ' ou ' NativeSupport '

Para obter mais informações, consulte [este link](ad-fs-prompt-login.md). 

### <a name="additional-troubleshooting"></a>Solução de problemas adicionais
Essas ocorrências são raras
1)  Se suas listas de CRL forem muito longas, ela poderá atingir o tempo limite ao tentar fazer o download. Nesse caso, você precisa atualizar o ' MaxFieldLength ' e o ' MaxRequestByte ' de acordo com https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows




## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Referência: lista completa de tipos de declaração de certificado de usuário e valores de exemplo

|                                         Tipo de declaração                                         |                              Exemplo de valor                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN = EntCA, DC = Domain, DC = contoso, DC = com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN = EntCA, DC = Domain, DC = contoso, DC = com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E =user@contoso.com, CN = user, CN = Users, DC = Domain, DC = contoso, DC = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E =user@contoso.com, CN = user, CN = Users, DC = Domain, DC = contoso, DC = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {Dados de certificado digital codificados na base64}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    KeyID = D6 13 E3 6B BC E5 D8 15 52 0A FD 36 6a D5 0b 51 F3 0B 25 7F     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   Usuário                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | Outro nome: nome da entidade =user@contoso.com, nome do RFC822 =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

## <a name="related-links"></a>Links relacionados
* [Configurar a associação de nome de host alternativo para autenticação de certificado AD FS](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [Configurar autoridades de certificação no Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)
