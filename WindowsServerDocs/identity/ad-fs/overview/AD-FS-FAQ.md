---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: Perguntas frequentes sobre o AD FS
description: Perguntas frequentes sobre o AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a1041bdc189238c7da32896e6f867f730e392d24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814426"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Perguntas frequentes do AD FS


A documentação a seguir é uma home page de perguntas frequentes sobre os Serviços de Federação do Active Directory (AD FS).  O documento foi dividido em grupos, de acordo com o tipo de pergunta.

## <a name="deployment"></a>Implantação

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Como posso fazer a atualização/migração de versões anteriores do AD FS?
Atualize o AD FS usando um dos seguintes procedimentos:


- AD FS do Windows Server 2012 R2 para o AD FS do Windows Server 2016 ou posterior. Observe que a metodologia é a mesma se você está atualizando o AD FS do Windows Server 2016 para o AD FS do Windows Server 2019. 
    - [Como atualizar para o AD FS no Windows Server 2016 por meio de um banco de dados WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Como atualizar para o AD FS no Windows Server 2016 por meio de um banco de dados SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- AD FS do Windows Server 2012 para o AD FS do Windows Server 2012 R2
    - [Migração para o AD FS no Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 para o AD FS do Windows Server 2012
    - [Migração para o AD FS no Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x para o AD FS 2.0
    - [Atualização do AD FS 1.x para o AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Caso precise atualizar o AD FS 2.0 ou 2.1 (Windows Server 2008 R2 ou Windows Server 2012), use os scripts nativos (localizados em C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Por que a instalação do AD FS exige uma reinicialização do servidor?

O suporte ao HTTP/2 foi adicionado no Windows Server 2016, mas o HTTP/2 não pode ser usado para autenticação de certificado de cliente.  Como muitos cenários do AD FS usam a autenticação de certificado de cliente e um número significativo de clientes não dá suporte a solicitações de repetição usando o HTTP/1.1, a configuração de farm do AD FS redefine as configurações de HTTP do servidor local como HTTP/1.1.  Isso exige uma reinicialização do servidor.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Há suporte para o uso de Servidores WAP do Windows 2016 para publicação do farm do AD FS na Internet sem a atualização do farm do AD FS de back-end?
Sim, há suporte para essa configuração; no entanto, não há suporte para os novos recursos do AD FS 2016 nessa configuração.  Essa configuração destina-se a ser temporária durante a fase de migração do AD FS 2012 R2 para o AD FS 2016 e não deve ser implantada por um longo tempo.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>É possível implantar o AD FS para o Office 365 sem publicar um proxy no Office 365?
Sim, há suporte para isso. No entanto, como efeito colateral

1. Você precisará gerenciar manualmente a atualização dos certificados de autenticação de tokens, porque o Azure AD não poderá acessar os metadados de federação. Para obter mais informações sobre como atualizar manualmente o certificado de autenticação de tokens, leia [Renovar certificados de federação para o Office 365 e o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. Você não poderá aproveitar os fluxos de autenticação herdados (por exemplo, fluxo de autenticação de proxy ExO)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quais são os requisitos de balanceamento de carga para servidores AD FS e WAP?

O AD FS é um sistema sem estado. Portanto, o balanceamento de carga é bem simples para logons. Veja a seguir as principais recomendações para sistemas de balanceamento de carga.


- Os balanceadores de carga não DEVEM ser configurados com a afinidade de IP. Isso pode colocar uma carga indevida em um subconjunto dos servidores em alguns cenários do Exchange Online.
- Os balanceadores de carga não DEVEM encerrar as conexões HTTPS e reiniciar uma nova conexão com o servidor ADFS.
- Os balanceadores de carga DEVEM garantir que o endereço IP de conexão seja convertido como o IP de origem no pacote HTTP ao ser enviado ao ADFS. Caso um balanceador de carga não possa enviar o IP de origem no pacote HTTP, o balanceador de carga DEVERÁ adicionar (ou acrescentar, no caso de um existente) o endereço IP ao cabeçalho x-forwarded-for. Isso é necessário para o tratamento correto de determinados recursos relacionados ao IP (IP banido, bloqueio inteligente de extranet etc.) e poderá levar à uma segurança reduzida se for configurado incorretamente.
- Os balanceadores de carga DEVEM dar suporte ao SNI. Caso contrário, verifique se o AD FS está configurado para criar associações HTTPS a fim de lidar com clientes não compatíveis com o SNI.
- Os balanceadores de carga DEVEM usar o ponto de extremidade de investigação de integridade HTTP do AD FS para detectar se os servidores AD FS ou WAP estão em funcionamento e os excluir se uma mensagem 200 OK não é retornada.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Há suporte para quais configurações de várias florestas no AD FS?

O AD FS dá suporte a diversas configurações de várias florestas e se baseia na rede de confiança subjacente do AD DS para autenticar os usuários em vários realms confiáveis. Recomendamos expressamente as relações de confiança de florestas bidirecionais, pois essa é uma configuração mais simples, a fim de garantir que o subsistema da relação de confiança funcione corretamente sem problemas. Além disso:

- No caso de uma relação de confiança de floresta unidirecional, como uma floresta DMZ que contém identidades de parceiros, recomendamos implantar o ADFS na floresta CORP e tratar a floresta DMZ como outra relação de confiança do provedor de declarações local conectada via LDAP. Nesse caso, a autenticação integrada do Windows não funcionará para os usuários da floresta DMZ. Eles precisarão realizar a autenticação de senha, pois esse é o único mecanismo compatível com o LDAP. Caso não possa escolher essa opção, você precisará configurar outro ADFS na floresta DMZ e adicioná-lo como uma relação de confiança do provedor de declarações ao ADFS na floresta CORP. Os usuários precisarão realizar a descoberta de realm inicial, mas a autenticação integrada do Windows e a autenticação de senha funcionarão. Faça as alterações apropriadas nas regras de emissão no ADFS da floresta DMZ, pois o ADFS na floresta CORP não poderá obter informações extras sobre o usuário da floresta DMZ.
- Embora as relações de confiança no nível do domínio tenham suporte e possam funcionar, recomendamos expressamente que você migre para um modelo de relação de confiança no nível da floresta. Além disso, você precisará garantir que os roteiros UPN e a resolução de nomes NetBIOS funcionem com precisão.

>[!NOTE]  
>Se a autenticação eletiva for usada com uma configuração de relação de confiança bidirecional, garanta que o usuário chamador receba a permissão "permitir autenticação" na conta de serviço de destino. 


## <a name="design"></a>Design

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quais provedores de autenticação multifator de terceiros estão disponíveis para o AD FS?
O AD FS fornece um mecanismo extensível para provedores de MFA de terceiros a serem integrados. Não há nenhum programa de certificação definido para isso. Supõe-se que o fornecedor executou as validações necessárias antes do lançamento. 

A lista de fornecedores que notificaram a Microsoft foi publicada em [Provedores de MFA para o AD FS](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).  Pode sempre haver provedores disponíveis que desconhecemos, por isso, atualizaremos a lista à medida que ficarmos cientes deles.

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Há suporte para proxies de terceiros no AD FS?
Sim, os proxies de terceiros podem ser colocados na frente do Proxy de aplicativo Web, mas qualquer proxy de terceiros precisa dar suporte ao [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) a ser usado no lugar do Proxy de aplicativo Web.

Veja abaixo uma lista de provedores de terceiros dos quais estamos cientes.  Pode sempre haver provedores disponíveis que desconhecemos, por isso, atualizaremos a lista à medida que ficarmos cientes deles.

- [F5 Access Policy Manager](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Em que local se encontra a planilha de dimensionamento do planejamento da capacidade do AD FS 2016?
A versão do AD FS 2016 da planilha pode ser baixada [aqui](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Isso também pode ser usado para o AD FS no Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Como garantir que meus servidores AD FS e WAP darão suporte aos requisitos de ATP da Apple?

A Apple lançou um conjunto de requisitos chamados ATS (App Transport Security) que possam afetar chamadas em aplicativos iOS que se autenticam no AD FS.  Você pode garantir que os seus servidores AD FS e WAP estejam em conformidade verificando se eles dão suporte aos [requisitos de conexão com o ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
Em particular, você deve verificar se os seus servidores AD FS e WAP dão suporte ao TLS 1.2 e se o pacote de criptografia negociado da conexão TLS dará suporte ao PFS.

Você pode habilitar e desabilitar o SSL 2.0 e 3.0 e as versões do TLS 1.0, 1.1 e 1.2 usando [Gerenciar protocolos SSL no AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Para garantir que os servidores AD FS e WAP negociem somente conjuntos de criptografia TLS que dão suporte a ATP, desabilite todos os conjuntos de criptografia que não estejam na [lista de conjuntos de criptografia em conformidade com ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Para fazer isso, use os [cmdlets do PowerShell do TLS no Windows](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Desenvolvedor

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>Ao gerar um id_token com o ADFS para um usuário autenticado no AD, como a "subdeclaração" é gerada no id_token?
O valor da "subdeclaração" é o hash da ID do cliente + o valor da declaração de âncora.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Qual é o tempo de vida do token de atualização/token de acesso quando o usuário faz logon por meio de uma relação de confiança remota do provedor de declarações por meio do WS-Fed/SAML-P?
O tempo de vida do token de atualização será o tempo de vida do token que o ADFS obteve da relação de confiança remota do provedor de declarações. O tempo de vida do token de acesso será o tempo de vida do token da terceira parte confiável para o qual o token de acesso está sendo emitido.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>Preciso retornar escopos de perfil e email além do escopo do OpenId. Posso obter informações adicionais usando escopos? Como fazer isso no AD FS?
Use id_token personalizado para adicionar informações relevantes no próprio id_token. Para obter mais informações, confira o artigo [Personalizar declarações a serem emitidas no id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Como emitir blobs JSON dentro de tokens JWT?
Um ValueType especial ("<http://www.w3.org/2001/XMLSchema#json>") e um caractere de escape (\x22) para isso foram adicionados no AD FS 2016. Use a amostra abaixo para a regra de emissão e também a saída final do token de acesso.

Regra de emissão da amostra:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Declaração emitida no token de acesso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>Posso transmitir o valor do recurso como parte do valor do escopo assim como as solicitações são feitas no Azure AD?
Com o AD FS no Server 2019, agora você pode transmitir o valor do recurso inserido no parâmetro de escopo. O parâmetro de escopo agora pode ser organizado como uma lista separada por espaço, em que cada entrada é estruturada como recurso/escopo. Por exemplo  
**< criar uma solicitação de exemplo válida>**

### <a name="does-ad-fs-support-pkce-extension"></a>O AD FS dá suporte à extensão da PKCE?
O AD FS no Server 2019 dá suporte à PKCE (Chave de Prova para Troca de Código) para o fluxo de concessão de código de autorização OAuth

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Há suporte para quais escopos permitidos no AD FS?
- aza: se você estiver usando [Extensões de Protocolo OAuth 2.0 para Clientes de Agente](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) e se o parâmetro de escopo contiver o escopo "aza", o servidor emitirá um novo token de atualização primário e o definirá no campo refresh_token da resposta, além de configurar o campo refresh_token_expires_in para o tempo de vida do novo token de atualização primário, caso um seja imposto.
- openid: permite que o aplicativo solicite o uso do protocolo de autorização OpenID Connect.
- logon_cert: o escopo logon_cert permite que um aplicativo solicite certificados de logon, que podem ser usados para fazer logon de maneira interativa dos usuários autenticados. O servidor do AD FS omite o parâmetro access_token da resposta e, em vez disso, fornece uma cadeia de certificados CMS codificada em Base64 ou uma resposta de PKI completa de CMC. Mais detalhes disponíveis [aqui](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation: o escopo user_impersonation é necessário para solicitar com êxito um token de acesso em nome do AD FS. Para obter detalhes sobre como usar esse escopo, veja [Criar um aplicativo de várias camadas usando o OBO (On-Behalf-Of) no OAuth com o AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert: o escopo vpn_cert permite que um aplicativo solicite certificados VPN, que podem ser usados para estabelecer conexões VPN usando a autenticação EAP-TLS. Não há mais suporte para isso.
- email: permite que o aplicativo solicite a declaração de email para o usuário conectado. Não há mais suporte para isso. 
- profile: permite que o aplicativo solicite declarações relacionadas ao perfil para o usuário de conexão. Não há mais suporte para isso. 


## <a name="operations"></a>Operações

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Como fazer para substituir o certificado SSL para o AD FS?
O certificado SSL do AD FS não é o mesmo que o certificado de comunicações do Serviço do AD FS encontrado no snap-in Gerenciamento do AD FS.  Para alterar o certificado SSL do AD FS, você precisará usar o PowerShell. Siga as diretrizes do artigo abaixo:

[Como gerenciar certificados SSL no AD FS e no WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Como fazer para habilitar ou desabilitar as configurações de TLS/SSL no AD FS?
Para desabilitar ou habilitar protocolos SSL e conjuntos de criptografia, use o seguinte:

[Gerenciar protocolos SSL no AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>O certificado SSL de proxy deve ser o mesmo que o certificado SSL do AD FS?  
Use as seguintes diretrizes com relação ao certificado SSL de proxy e ao certificado SSL do AD FS:


- Se o proxy for usado para usar um proxy de solicitações do AD FS que usam a Autenticação Integrada do Windows, o certificado SSL do proxy precisará ser igual (usar a mesma chave) o certificado SSL do servidor de federação
- Se a propriedade “ExtendedProtectionTokenCheck” do AD FS estiver habilitada (a configuração padrão no AD FS), o certificado SSL de proxy precisará ser o mesmo (usar a mesma chave) que o certificado SSL do servidor de federação
- Caso contrário, o certificado SSL de proxy poderá ter uma chave diferente daquela do certificado SSL do AD FS, mas precisará atender aos mesmos [requisitos](../overview/AD-FS-2016-Requirements.md)

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>Por que só vejo um logon com senha no AD FS e não meus outros métodos de autenticação que configurei? 
O AD FS mostra apenas um só método de autenticação na tela de logon quando o aplicativo exige explicitamente um URI de autenticação específico que é mapeado para um método de autenticação configurado e habilitado. Isso é transmitido no parâmetro 'wauth' de solicitações do Web Services Federation e o parâmetro 'RequestedAuthnCtxRef' em uma solicitação do protocolo SAML. Como resultado, apenas o método de autenticação solicitado é exibido (por exemplo, logon com senha).

Quando AD FS é usado com o Azure AD, é comum que os aplicativos enviem o parâmetro prompt=login ao Azure AD. Por padrão, o Azure AD converte isso para solicitar um novo logon baseado em senha ao AD FS. Esse é o motivo mais comum para ver um logon com senha no AD FS dentro da rede ou não ver uma opção para fazer logon com o certificado. Isso pode ser corrigido com facilidade com uma alteração nas configurações de domínio federado no Azure AD. 

Para obter informações sobre como configurar isso, confira [Suporte do parâmetro prompt=login do AD FS (Serviços de Federação do Active Directory)](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Como alterar a conta de serviço do AD FS?
Para alterar a conta de serviço do AD FS, siga as instruções usando a caixa de ferramentas [Módulo do PowerShell da Conta de Serviço](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule) do AD FS.

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Como configurar navegadores para usar a WIA (autenticação integrada do Windows) com o AD FS?

Para obter informações sobre como configurar navegadores, confira [Configurar navegadores para usar a WIA (autenticação integrada do Windows) com o AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-turn-off-browserssoenabled"></a>Posso desligar o BrowserSsoEnabled?
Se você não tiver políticas de controle de acesso baseadas no dispositivo no ADFS ou no registro de certificado do Windows Hello para Empresas que usem o ADFS, você poderá desligar o BrowserSsoEnabled. O BrowserSsoEnabled permite que o ADFS colete um PRT (token de atualização primário) do cliente que contém informações sobre o dispositivo. Sem essa autenticação de dispositivo, o ADFS não funcionará em dispositivos Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Por quanto tempo os tokens do AD FS são válidos?

Geralmente, essa pergunta significa 'por quanto tempo os usuários obtêm o SSO (logon único) sem precisar inserir novas credenciais e como eu, como administrador, controlo isso?'  Esse comportamento e as definições de configuração que o controlam são descritos no artigo [Configurações de logon único do AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Os tempos de vida padrão dos vários cookies e tokens são listados abaixo (bem como os parâmetros que controlam os tempos de vida):

**Dispositivos registrados**

- Cookies de PRT e SSO: máximo de 90 dias, controlado por PSSOLifeTimeMins. (O dispositivo fornecido é usado, pelo menos, a cada 14 dias, que é controlado por DeviceUsageWindow)

- Token de atualização: calculado com base nas informações acima para fornecer um comportamento consistente

- access_token: 1 hora por padrão, com base na terceira parte confiável

- id_token: igual ao token de acesso

**Dispositivos não registrados**

- Cookies de SSO: 8 horas por padrão, controladas por SSOLifetimeMins.  Quando o KMSI (Manter-me conectado) está habilitado, o padrão é 24 horas e é configurável por meio de KMSILifetimeMins.


- Token de atualização: 8 horas por padrão. 24 horas com o KMSI habilitado

- access_token: 1 hora por padrão, com base na terceira parte confiável

- id_token: igual ao token de acesso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>O AD FS dá suporte ao HSTS (HTTP Strict Transport Security)?  

O HSTS (HTTP Strict Transport Security) é um mecanismo de política de segurança da Web que ajuda a atenuar ataques de downgrade de protocolo e sequestro de cookie nos serviços que têm pontos de extremidade HTTP e HTTPS. Ele permite que os servidores Web declarem que os navegadores da Web (ou outros agentes de usuário em conformidade) só devem interagir com ele usando HTTPS e nunca por meio do protocolo HTTP.

Todos os pontos de extremidade do AD FS para o tráfego de autenticação na Web são abertos exclusivamente via HTTPS.  Como resultado, o AD FS atenua efetivamente as ameaças fornecidas pelo mecanismo de política do HTTP Strict Transport Security (por design, não há nenhum downgrade para HTTP, pois não há ouvintes em HTTP). Além disso, o AD FS impede que os cookies sejam enviados a outro servidor com pontos de extremidade de protocolo HTTP marcando todos os cookies com o sinalizador de segurança.

Portanto, a implementação do HSTS em um servidor AD FS não é necessária porque nunca é possível fazer downgrade dele.  Para fins de conformidade, os servidores AD FS atendem a esses requisitos, porque eles nunca podem usar HTTP e todos os cookies são marcados como seguros.

Além disso, o AD FS 2016 (com os patches mais atualizados) e o AD FS 2019 dão suporte à emissão do cabeçalho HSTS. Para configurar isso, confira [Personalizar cabeçalhos de resposta de segurança HTTP com o AD FS](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>O x-ms-forwarded-client-ip não contém o IP do cliente, mas contém o IP do firewall na frente do proxy. Em que local posso obter o IP correto do cliente?
Não é recomendável fazer uma terminação SSL antes do WAP. Caso a terminação SSL seja feita na frente do WAP, o x-ms-forwarded-client-ip conterá o IP do dispositivo de rede na frente do WAP. Veja abaixo uma breve descrição das várias declarações relacionadas a IP compatíveis com o AD FS:
 - x-ms-client-ip: IP de rede do dispositivo que se conectou ao STS.  No caso de uma solicitação da extranet, isso sempre conterá o IP do WAP.
 - x-ms-forwarded-client-ip: declaração de vários valores que conterá os valores encaminhados ao ADFS pelo Exchange Online, além do endereço IP do dispositivo que se conectou ao WAP.
 - Userip: para solicitações da extranet, essa declaração conterá o valor de x-ms-forwarded-client-ip.  Para solicitações da intranet, essa declaração conterá o mesmo valor de x-ms-client-ip.

 Além disso, o AD FS 2016 (com os patches mais atualizados) e as versões posteriores também dão suporte à captura do cabeçalho x-forwarded-for. Qualquer balanceador de carga ou dispositivo de rede que não faça o encaminhamento na camada 3 (o IP é preservado) deverá adicionar o IP do cliente de entrada ao cabeçalho x-forwarded-for padrão do setor. 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Estou tentando obter declarações adicionais no ponto de extremidade de informações do usuário, mas ele só retorna a entidade. Como posso obter declarações adicionais?
O ponto de extremidade UserInfo do AD FS sempre retorna a declaração da entidade, conforme especificado nos padrões do OpenID. O AD FS não fornece declarações adicionais solicitadas por meio do ponto de extremidade UserInfo. Caso você precise de declarações adicionais no token de ID, confira [Tokens de ID personalizados no AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Por que vejo muitos erros 1021 em meus servidores AD FS?
Esse evento é registrado em log normalmente para um acesso de recurso inválido no AD FS para o recurso 00000003-0000-0000-c000-000000000000. Esse erro é causado por um comportamento incorreto do cliente, em que ele tenta obter um token de acesso para o serviço do Azure AD Graph. Como o recurso não está presente no AD FS, isso resulta na ID de evento 1021 nos servidores AD FS. É seguro ignorar os avisos ou os erros para o recurso 00000003-0000-0000-c000-000000000000 no AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Por que estou vendo um aviso de falha ao adicionar a conta de serviço do AD FS ao grupo de Administradores de Chaves Empresariais?
Esse grupo só é criado quando um controlador de domínio do Windows 2016 com a função de controlador de domínio primário do FSMO existe no domínio. Para resolver o erro, crie o grupo manualmente e siga as instruções abaixo para dar a permissão necessária depois de adicionar a conta de serviço como membro do grupo.
1.    Abra **Usuários e Computadores do Active Directory**.
2.    **Clique com o botão direito do mouse** no nome de domínio no painel de navegação e **clique** em Propriedades.
3.    **Clique** em Segurança (se a guia Segurança estiver ausente, ative Recursos Avançados no menu Exibir).
4.    **Clique** em Avançado. **Clique** em Adicionar. **Clique** em Selecionar uma entidade.
5.    A caixa de diálogo Selecionar Usuário, Computador, Conta de Serviço ou Grupo será exibida.  Na caixa de texto Insira o nome do objeto a ser selecionado, digite Grupo de Administradores de Chaves.  Clique em OK.
6.    Na caixa de listagem Aplicável a, selecione **objetos de Usuário descendente**.
7.    Usando a barra de rolagem, role a página até a parte inferior da página e **clique** em Desmarcar tudo.
8.    Na seção **Propriedades**, selecione **Ler msDS-KeyCredentialLink** e **Gravar msDS-KeyCrendentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Por que a autenticação moderna em dispositivos Android falha se o servidor não envia todos os certificados intermediários na cadeia com o certificado SSL?

Os usuários federados podem observar uma falha na autenticação no Azure AD para os aplicativos que usam a biblioteca ADAL do Android. O aplicativo receberá uma **AuthenticationException** quando tentar mostrar a página de logon. No Chrome, a página de logon do AD FS poderá ser chamada de não segura.

O Android, em todas as versões e todos os dispositivos, não dá suporte ao download de certificados adicionais por meio do campo **authorityInformationAccess** do certificado. Isso também é verdadeiro para o navegador Chrome. Qualquer certificado de autenticação de servidor que não contiver certificados intermediários resultará nesse erro se a cadeia de certificados inteira não for transmitida pelo AD FS.

Uma solução adequada para esse problema é configurar os servidores AD FS e WAP para enviar os certificados intermediários necessários juntamente com o certificado SSL.

Ao exportar o certificado SSL, de um computador, para ser importado para o repositório pessoal do computador, do servidor AD FS e WAP, lembre-se de exportar a Chave privada e selecionar **Troca de Informações Pessoais –PKCS nº 12**.

É importante que a caixa de seleção **Incluir todos os certificados no caminho do certificado, se possível** esteja marcada, bem como **Exportar todas as propriedades estendidas**.  

Execute certlm.msc nos servidores Windows e importe o *.PFX no repositório de certificados pessoal do computador. Isso fará com que o servidor transmita toda a cadeia de certificados para a biblioteca ADAL.

>[!NOTE]
> O repositório de certificados de balanceadores de carga de rede também deve ser atualizado para incluir toda a cadeia de certificados, se presente

### <a name="does-ad-fs-support-head-requests"></a>O AD FS dá suporte a solicitações HEAD?
O AD FS não dá suporte a solicitações HEAD.  Os aplicativos não devem usar solicitações HEAD em pontos de extremidade do AD FS.  Isso poderá causar respostas de erro HTTP inesperadas e/ou atrasadas.  Além disso, você poderá receber eventos de erro inesperados no log de eventos do AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Por que não vejo um token de atualização quando faço logon com um IdP remoto?
Um token de atualização não é emitido se o token emitido pelo IdP tem uma validade inferior a 1 hora. Para garantir que um token de atualização seja emitido, aumente a validade do token emitido pelo IdP para acima de 1 hora.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>Há alguma maneira de alterar o algoritmo de criptografia de token RP?
Por padrão, a criptografia de token RP é definida como AES256 e não pode ser alterada para nenhum outro valor.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>Em um farm de modo misto, recebo um erro ao tentar definir o novo certificado SSL usando Set-AdfsSslCertificate -Thumbprint. Como posso atualizar o certificado SSL em um farm do AD FS de modo misto?
Os farms do AD FS de modo misto devem ser um estado de transição. É recomendável que, durante o planejamento, o certificado SSL seja substituído antes do processo de atualização ou conclua o processo e aumente o nível de comportamento do farm antes de atualizar o certificado SSL. Caso isso não tenha sido feito, as instruções abaixo fornecerão a capacidade de atualizar o certificado SSL. 

Em servidores WAP, você ainda poderá usar Set-WebApplicationProxySslCertificate. Nos servidores ADFS, você precisará usar o netsh. Siga as etapas conforme indicado abaixo:

1. Selecione o subconjunto de servidores ADFS 2016 para manutenção (por exemplo, remoção do balanceador de carga)
2. Nos servidores selecionados na etapa 1, importe o novo certificado via MMC
3. Excluir os certificados existentes

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443 b. netsh http delete sslcert hostnameport=localhost:443 c. netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  Adicionar o novo certificado

    a. netsh http add sslcert hostnameport=fs.contoso.com:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    c. netsh http add sslcert hostnameport=fs.contoso.com:49443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

5. Reiniciar o serviço ADFS no servidor selecionado
6. Remover o subconjunto de servidores WAP para manutenção
7. Nos servidores WAP selecionados, importe o novo certificado via MMC
8. Definir o novo certificado no WAP usando o cmdlet

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. Reiniciar o serviço nos servidores WAP selecionados
10. Coloque os servidores WAP e AD FS selecionados novamente no ambiente de produção.

Execute a atualização no restante dos servidores AD FS e WAP de maneira semelhante.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>Há suporte para o ADFS quando os servidores WAP (Proxy de aplicativo Web) estão atrás do WAF (firewall do aplicativo Web do Azure)?
Os servidores de aplicativos Web e do ADFS dão suporte a qualquer firewall que não execute a terminação SSL no ponto de extremidade. Além disso, os servidores ADFS/WAP têm mecanismos internos para impedir ataques comuns da Web, como scripts intersite e proxy do ADFS, além de atender a todos os requisitos definidos pelo [protocolo MS-ADFSPIPFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>Estou recebendo uma mensagem "Evento 441: Foi encontrado um token com uma chave de associação de token inválida." O que devo fazer para resolver isso?
No AD FS 2016, a associação de token é habilitada automaticamente e causa vários problemas conhecidos com cenários de proxy e federação que resultam nesse erro. Para resolver isso, execute o comando do PowerShell a seguir e remova o suporte de associação de token.

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>Atualizei meu farm do AD FS no Windows Server 2016 para o AD FS no Windows Server 2019. O nível de comportamento do farm do AD FS foi aumentado com êxito no 2019, mas a configuração do Proxy de aplicativo Web ainda é exibida como Windows Server 2016?
Após uma atualização para o Windows Server 2019, a versão de configuração do Proxy de aplicativo Web continuará sendo exibida como Windows Server 2016. O Proxy de aplicativo Web não tem novos recursos específicos da versão para o Windows Server 2019 e, se o nível de comportamento do farm for aumentado com êxito no AD FS, o Proxy de aplicativo Web continuará sendo exibido como o Windows Server 2016 por design.

### <a name="can-i-estimate-the-size-of-the-adfsartifactstore-before-enabling-esl"></a>Posso estimar o tamanho do ADFSArtifactStore antes de habilitar o ESL?
Com o ESL habilitado, o AD FS acompanha a atividade da conta e as localizações conhecidas para os usuários no banco de dados ADFSArtifactStore. Esse banco de dados é dimensionado em tamanho em relação ao número de usuários e localizações conhecidas acompanhados. Ao planejar a habilitação do ESL, você pode estimar o aumento do tamanho do banco de dados ADFSArtifactStore a uma taxa de até 1 GB a cada 100.000 usuários. Se o farm do AD FS estiver usando o WID (Banco de Dados Interno do Windows), a localização padrão dos arquivos de banco de dados será C:\Windows\WID\Data. Para evitar o preenchimento desta unidade, verifique se você tem um mínimo de 5 GB de armazenamento livre antes de habilitar o ESL. Além do armazenamento em disco, planeje um aumento da memória total do processo depois de habilitar o ESL em até um 1 GB de RAM adicional para populações de usuário de 500.000 ou menos.
