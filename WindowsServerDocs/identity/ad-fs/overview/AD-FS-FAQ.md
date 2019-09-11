---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: PERGUNTAS FREQUENTES AD FS
description: Perguntas frequentes sobre AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8444e417fe089c1a3cf2acc2648b222ec5c9774c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865464"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Perguntas frequentes sobre o AD FS (FAQ)


A documentação a seguir é uma página inicial para perguntas frequentes em relação a Serviços de Federação do Active Directory (AD FS).  O documento foi dividido em grupos com base no tipo de pergunta.

## <a name="deployment"></a>Implantação

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Como posso atualizar/migrar de versões anteriores do AD FS
Você pode atualizar AD FS usando um dos seguintes:


- O Windows Server 2012 R2 AD FS ao Windows Server 2016 AD FS ou superior. Observe que a metodologia é a mesma se você estiver atualizando do Windows Server 2016 AD FS para o Windows Server 2019 AD FS. 
    - [Como atualizar para o AD FS no Windows Server 2016 por meio de um banco de dados WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Como atualizar para o AD FS no Windows Server 2016 por meio de um banco de dados SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS para Windows Server 2012 R2 AD FS
    - [Migrar para o AD FS no Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2,0 para o Windows Server 2012 AD FS
    - [Migrar para o AD FS no Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1. x a AD FS 2,0
    - [Atualizar do AD FS 1. x para AD FS 2,0](https://technet.microsoft.com/library/ff678035.aspx)

Se precisar atualizar do AD FS 2,0 ou 2,1 (Windows Server 2008 R2 ou Windows Server 2012), você deverá usar os scripts na caixa (localizados em C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Por que AD FS instalação requer uma reinicialização do servidor?

O suporte a HTTP/2 foi adicionado no Windows Server 2016, mas HTTP/2 não pode ser usado para autenticação de certificado de cliente.  Como muitos cenários de AD FS usam a autenticação de certificado de cliente e um número significativo de clientes não dão suporte a solicitações de repetição usando HTTP/1.1, a configuração de farm de AD FS redefine as configurações de HTTP do servidor local como HTTP/1.1.  Isso requer uma reinicialização do servidor.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>O está usando servidores WAP do Windows 2016 para publicar o farm de AD FS na Internet sem a necessidade de atualizar o farm de AD FS de back-end com suporte?
Sim, essa configuração tem suporte, no entanto, não há suporte para novos recursos do AD FS 2016 nessa configuração.  Essa configuração deve ser temporária durante a fase de migração do AD FS 2012 R2 para AD FS 2016 e não deve ser implantada por longos períodos de tempo.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>É possível implantar AD FS para o Office 365 sem publicar um proxy no Office 365?
Sim, há suporte para isso. No entanto, como um efeito colateral

1. Você precisará gerenciar manualmente os certificados de assinatura de token de atualização porque o Azure AD não poderá acessar os metadados de Federação. Para obter mais informações sobre como atualizar manualmente o certificado de assinatura de token, leia [renovar certificados de Federação para o Office 365 e Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. Você não poderá aproveitar os fluxos de autenticação herdados (por exemplo, o fluxo de autenticação de proxy ExO)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quais são os requisitos de balanceamento de carga para servidores AD FS e WAP?

AD FS é um sistema sem estado. Portanto, o balanceamento de carga é bem simples para logons. Veja a seguir as principais recomendações para sistemas de balanceamento de carga.


- Os balanceadores de carga não devem ser configurados com afinidade de IP. Isso pode colocar a carga indevida em um subconjunto de seus servidores em determinados cenários do Exchange Online.
- Os balanceadores de carga não devem encerrar as conexões HTTPS e reiniciar uma nova conexão com o servidor ADFS.
- Os balanceadores de carga devem garantir que o endereço IP de conexão deve ser convertido como o IP de origem no pacote HTTP ao ser enviado ao ADFS. Caso um balanceador de carga não possa enviar o IP de origem no pacote HTTP, o balanceador de carga deve adicionar (ou acrescentar no caso de existente) o endereço IP ao cabeçalho x-forwardd-for. Isso é necessário para o tratamento correto de determinados recursos relacionados a IP (IP Banido, bloqueio inteligente de extranet,...) e pode levar à segurança reduzida se estiver configurado de forma inadequada.
- Os balanceadores de carga devem dar suporte a SNI. Caso contrário, verifique se AD FS está configurado para criar associações HTTPS para lidar com clientes não-compatíveis com SNI.
- Os balanceadores de carga devem usar o ponto de extremidade AD FS de investigação de integridade HTTP para detectar se os servidores AD FS ou WAP estão em execução e os excluim se um 200 OK não for retornado.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Quais configurações de várias florestas têm suporte pelo AD FS?

O AD FS dá suporte a várias configurações de várias florestas e se baseia na rede de confiança subjacente AD DS para autenticar usuários em vários territórios confiáveis. Recomendamos que florestas de 2 vias sejam altamente confiáveis, pois essa é uma configuração mais simples para garantir que o subsistema de confiança funcione corretamente sem problemas. Além disso

- No caso de uma relação de confiança de floresta unidirecional, como uma floresta DMZ contendo identidades de parceiros, é recomendável implantar o ADFS na floresta Corp e tratar a floresta DMZ como outra confiança do provedor de declarações local conectada via LDAP. Nesse caso, a autenticação integrada do Windows não funcionará para os usuários da floresta DMZ e será necessário executar a autenticação de senha, pois esse é o único mecanismo com suporte para LDAP. Caso não seja possível buscar essa opção, você precisaria configurar outro ADFS na floresta DMZ e adicioná-lo como confiança do provedor de declarações no ADFS na floresta Corp. Os usuários precisarão realizar a descoberta de realm inicial, mas a autenticação integrada do Windows e a autenticação de senha funcionarão. Faça as alterações apropriadas nas regras de emissão no ADFS na floresta DMZ, pois o ADFS na floresta Corp não poderá obter informações adicionais do usuário sobre o usuário da floresta DMZ.
- Enquanto as relações de confiança no nível do domínio têm suporte e podem funcionar, é altamente recomendável que você se mova para um modelo de confiança no nível da floresta. Além disso, você precisaria garantir que o roteamento UPN e a resolução de nomes NETBIOS tenham que funcionar com precisão.



## <a name="design"></a>Criar

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quais provedores de autenticação multifator de terceiros estão disponíveis para AD FS?
AD FS fornece um mecanismo extensível para provedores de MFA de terceiros a serem integrados. Não há nenhum programa de certificação definido para isso. Supõe-se que o fornecedor executou as validações necessárias antes do lançamento. 

A lista de fornecedores que notificaram a Microsoft são publicadas em [provedores de MFA para AD FS](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md).  Pode haver sempre provedores disponíveis que não conhecem e atualizaremos a lista à medida que aprendemos sobre eles.

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Os proxies de terceiros têm suporte com o AD FS?
Sim, os proxies de terceiros podem ser colocados na frente do proxy de aplicativo Web, mas qualquer proxy de terceiros deve dar suporte ao [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) para ser usado no lugar do proxy de aplicativo Web.

Veja abaixo uma lista de provedores de terceiros dos quais estamos cientes.  Pode haver sempre provedores disponíveis que não conhecem e atualizaremos a lista à medida que aprendemos sobre eles.

- [F5 Access Policy Manager](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Onde está a planilha de dimensionamento de planejamento de capacidade para AD FS 2016?
A versão AD FS 2016 da planilha pode ser baixada [aqui](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Isso também pode ser usado para AD FS no Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Como garantir que meus servidores AD FS e WAP ofereçam suporte aos requisitos de ATP da Apple?

A Apple lançou um conjunto de requisitos chamados de ATS (segurança de transporte de aplicativo) que podem afetar chamadas de aplicativos iOS que se autenticam no AD FS.  Você pode garantir que seus servidores AD FS e WAP estejam em conformidade, certificando-se de que eles dão suporte aos [requisitos de conexão usando o ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
Em particular, você deve verificar se os seus servidores AD FS e WAP suportam o TLS 1,2 e se o pacote de criptografia negociado da conexão TLS dará suporte ao sigilo perfeita para o encaminhamento.

Você pode habilitar e desabilitar SSL 2,0 e 3,0 e versões de TLS 1,0, 1,1 e 1,2 usando [Gerenciar protocolos SSL no AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Para garantir que seus servidores AD FS e WAP negociem somente conjuntos de criptografia TLS que dão suporte a ATP, você pode desabilitar todos os conjuntos de codificação que não estão na [lista de conjuntos de codificação compatíveis com ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Para fazer isso, use os [cmdlets do PowerShell do Windows TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Desenvolvedor

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>Ao gerar um id_token com o ADFS para um usuário autenticado no AD, como a declaração "sub" é gerada no id_token?
O valor da declaração "sub" é o hash da ID do cliente + valor da declaração de âncora.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Qual é o tempo de vida do token de atualização/token de acesso quando o usuário faz logon por meio de uma confiança do provedor de declarações remota por WS-enalimentate/SAML-P?
O tempo de vida do token de atualização será o tempo de vida do token que o ADFS obteve da confiança do provedor de declarações remota. O tempo de vida do token de acesso será o tempo de vida do token da terceira parte confiável para a qual o token de acesso está sendo emitido.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>Preciso retornar escopos de perfil e de email além do escopo de OpenId. Posso obter informações adicionais usando escopos? Como fazer isso no AD FS?
Você pode usar o id_token personalizado para adicionar informações relevantes no próprio id_token. Para obter mais informações, consulte o artigo [Personalizar declarações a serem emitidas no id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Como emitir BLOBs JSON dentro de tokens JWT?
Um ValueType especial ("<http://www.w3.org/2001/XMLSchema#json>") e um caractere de escape (\x22) para isso foi adicionado no AD FS 2016. Use o exemplo a seguir para a regra de emissão e também a saída final do token de acesso.

Regra de emissão de exemplo:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Declaração emitida no token de acesso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>Posso passar o valor do recurso como parte do valor do escopo como como as solicitações são feitas no Azure AD?
Com AD FS no servidor 2019, agora você pode passar o valor do recurso inserido no parâmetro de escopo. O parâmetro de escopo agora pode ser organizado como uma lista separada por espaços, em que cada entrada é estruturada como recurso/escopo. Por exemplo  
**< criar uma solicitação de amostra válida >**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS oferece suporte à extensão PKCE?
O AD FS no servidor 2019 dá suporte à chave de prova de PKCE (troca de código para fluxo de concessão de código de autorização OAuth

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Quais escopos permitidos têm suporte pelo AD FS?
- Aza-se estiver usando [extensões de protocolo OAuth 2,0 para clientes do Broker](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) e se o parâmetro de escopo contiver o escopo "aza", o servidor emitirá um novo token de atualização primário e o definirá no campo refresh_token da resposta, bem como Configurando o refresh_token_ o campo expires_in para o tempo de vida do novo token de atualização primário se um for imposto.
- OpenID – permite que o aplicativo solicite o uso do protocolo de autorização OpenID Connect.
- logon_cert-o escopo logon_cert permite que um aplicativo solicite certificados de logon, que podem ser usados para fazer logon interativamente usuários autenticados. O servidor de AD FS omite o parâmetro access_token da resposta e, em vez disso, fornece uma cadeia de certificados CMS codificada em base64 ou uma resposta de PKI completa de CMC. Mais detalhes estão disponíveis [aqui](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation-o escopo de user_impersonation é necessário para solicitar com êxito um token de acesso em nome de AD FS. Para obter detalhes sobre como usar esse escopo, consulte [criar um aplicativo de várias camadas usando obo (em nome de) usando OAuth com AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert-o escopo vpn_cert permite que um aplicativo solicite certificados VPN, que podem ser usados para estabelecer conexões VPN usando a autenticação EAP-TLS. Não há mais suporte para isso.
- email – permite que o aplicativo solicite a declaração de email para o usuário conectado. Não há mais suporte para isso. 
- Perfil – permite que o aplicativo solicite declarações relacionadas ao perfil para o usuário de conexão. Não há mais suporte para isso. 


## <a name="operations"></a>Operações

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Como fazer substituir o certificado SSL para AD FS?
O certificado SSL AD FS não é o mesmo que o certificado de comunicações do serviço de AD FS encontrado no snap-in de gerenciamento de AD FS.  Para alterar o AD FS certificado SSL, você precisará usar o PowerShell. Siga as orientações no artigo abaixo:

[Como gerenciar certificados SSL no AD FS e no WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Como habilitar ou desabilitar as configurações de TLS/SSL para AD FS
Para desabilitar ou habilitar protocolos SSL e conjuntos de codificação, use o seguinte:

[Gerenciar protocolos SSL no AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>O certificado SSL do proxy deve ser o mesmo que o certificado SSL do AD FS?  
Use as orientações a seguir com relação ao certificado SSL de proxy e ao AD FS certificado SSL:


- Se o proxy for usado para fazer proxy de AD FS solicitações que usam a autenticação integrada do Windows, o certificado SSL do proxy deverá ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Se a propriedade AD FS "ExtendedProtectionTokenCheck" estiver habilitada (a configuração padrão em AD FS), o certificado SSL do proxy deverá ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Caso contrário, o certificado SSL de proxy pode ter uma chave diferente da AD FS certificado SSL, mas deve atender aos mesmos [requisitos](../overview/AD-FS-2016-Requirements.md)

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>Por que só vejo um logon com senha no AD FS e não meus outros métodos de autenticação que configurei? 
AD FS mostra apenas um único método de autenticação na tela de logon quando o aplicativo requer explicitamente um URI de autenticação específico que mapeia para um método de autenticação configurado e habilitado. Isso é transmitido no parâmetro ' wauth ' para solicitações do WS-Federation e o parâmetro ' RequestedAuthnCtxRef ' em uma solicitação de protocolo SAML. Como resultado, apenas o método de autenticação solicitado é exibido (por exemplo, logon de senha).

Quando AD FS é usado com o Azure AD, é comum que os aplicativos enviem o parâmetro prompt = login para o Azure AD. Por padrão, o Azure AD converte isso para solicitar uma nova senha com base em logon para AD FS. Esse é o motivo mais comum para ver um logon de senha em AD FS dentro de sua rede ou não ver uma opção para fazer logon com seu certificado. Isso pode ser facilmente remediado fazendo uma alteração nas configurações de domínio federado no Azure AD. 

Para obter informações sobre como configurar isso, consulte [serviços de Federação do Active Directory (AD FS) prompt = suporte ao parâmetro de logon](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Como posso alterar a conta de serviço AD FS?
Para alterar a conta de serviço do AD FS, siga as instruções usando o [módulo do PowerShell da conta de serviço](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)da caixa de ferramentas AD FS.

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Como configurar navegadores para usar a WIA (autenticação integrada do Windows) com o AD FS?

Para obter informações sobre como configurar navegadores, consulte [Configurar navegadores para usar a autenticação integrada do Windows (WIA) com AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-turn-off-browserssoenabled"></a>Posso desativar o BrowserSsoEnabled?
Se você não tiver políticas de controle de acesso com base no dispositivo no ADFS ou no registro de certificado do Windows Hello para empresas usando o ADFS; Você pode desativar o BrowserSsoEnabled. O BrowserSsoEnabled permite que o ADFS colete um PRT (token de atualização principal) do cliente que contém informações sobre o dispositivo. Sem essa autenticação de dispositivo, o ADFS não funcionará em dispositivos Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Por quanto tempo os tokens AD FS são válidos?

Geralmente, essa pergunta significa ' por quanto tempo os usuários obtêm SSO (logon único) sem precisar inserir novas credenciais e como um controle de administrador? '  Esse comportamento e os parâmetros de configuração que o controlam, são descritos no artigo [AD FS configurações de logon único](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Os tempos de vida padrão dos vários cookies e tokens estão listados abaixo (bem como os parâmetros que regem os tempos de vida):

**Dispositivos registrados**

- Cookies de PRT e SSO: máximo de 90 dias, governado por PSSOLifeTimeMins. (O dispositivo fornecido é usado pelo menos a cada 14 dias, que é controlado por DeviceUsageWindow)

- Token de atualização: calculado com base no acima para fornecer comportamento consistente

- access_token: 1 hora por padrão, com base na terceira parte confiável

- id_token: igual ao token de acesso

**Dispositivos não registrados**

- Cookies de SSO: 8 horas por padrão, governadas pelo SSOLifetimeMins.  Quando manter-me conectado (KMSI) está habilitado, o padrão é 24 horas e configurável por meio de KMSILifetimeMins.


- Token de atualização: 8 horas por padrão. 24 horas com KMSI habilitado

- access_token: 1 hora por padrão, com base na terceira parte confiável

- id_token: igual ao token de acesso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS dá suporte à segurança de transporte HSTS (HTTP Strict Transport Security)?  

A HSTS (segurança de transporte estrita) HTTP é um mecanismo de diretiva de segurança da Web que ajuda a mitigar ataques de downgrade de protocolo e seqüestro de cookie para serviços que têm pontos de extremidade HTTP e HTTPS. Ele permite que os servidores Web declarem que os navegadores da Web (ou outros agentes de usuário de conformidade) só devem interagir com ele usando HTTPS e nunca por meio do protocolo HTTP.

Todos os pontos de extremidade AD FS para o tráfego de autenticação na Web são abertos exclusivamente por HTTPS.  Como resultado, AD FS atenua efetivamente as ameaças que o mecanismo de política de segurança de transporte estrito de HTTP fornece (por design, não há nenhum downgrade para HTTP, pois não há ouvintes em HTTP). Além disso, AD FS impede que os cookies sejam enviados a outro servidor com pontos de extremidade de protocolo HTTP marcando todos os cookies com o sinalizador de segurança.

Portanto, a implementação de HSTS em um servidor AD FS não é necessária porque ele nunca pode ser desatualizado.  Para fins de conformidade, os servidores de AD FS atendem a esses requisitos porque eles nunca podem usar HTTP e todos os cookies são marcados como seguros.

Além disso, AD FS 2016 (com os patches mais atualizados) e AD FS suporte a 2019 emitindo o cabeçalho HSTS. Para configurar isso, consulte [Personalizar cabeçalhos de resposta de segurança http com AD FS](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-MS-encaminhar-Client-IP não contém o IP do cliente, mas contém o IP do firewall na frente do proxy. Onde posso obter o IP correto do cliente?
Não é recomendável fazer terminação SSL antes de WAP. Caso o encerramento de SSL seja feito na frente do WAP, o X-MS-Forwarded-Client-IP conterá o IP do dispositivo de rede na frente do WAP. Veja abaixo uma breve descrição das várias declarações relacionadas a IP com suporte pelo AD FS:
 - x-MS-Client-IP: IP de rede do dispositivo que se conectou ao STS.  No caso de uma solicitação de extranet, isso sempre contém o IP do WAP.
 - x-MS-encaminhar-Client-IP: Declaração de valores múltiplos que conterá quaisquer valores encaminhados ao ADFS pelo Exchange Online, além do endereço IP do dispositivo que se conectou ao WAP.
 - Userip: Para solicitações de extranet, essa declaração conterá o valor de x-MS-encaminhar-Client-IP.  Para solicitações de intranet, essa declaração conterá o mesmo valor de x-MS-Client-IP.

 Além disso, no AD FS 2016 (com os patches mais atualizados) e versões posteriores também dão suporte à captura do cabeçalho x-forwardd-for. Qualquer balanceador de carga ou dispositivo de rede que não encaminhar na camada 3 (o IP é preservado) deve adicionar o IP do cliente de entrada ao cabeçalho x-forwardd-for padrão do setor. 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Estou tentando obter declarações adicionais no ponto de extremidade de informações do usuário, mas seu único retorno de assunto. Como posso obter declarações adicionais?
O ponto de extremidade AD FS UserInfo sempre retorna a declaração da entidade conforme especificado nos padrões de OpenID. AD FS não fornece declarações adicionais solicitadas por meio do ponto de extremidade de UserInfo. Se você precisar de declarações adicionais no token de ID, consulte [tokens de ID personalizados em AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Por que vejo muitos erros 1021 em meus servidores AD FS?
Esse evento é registrado normalmente para um acesso de recurso inválido em AD FS para o recurso 00000003-0000-0000-C000-000000000000. Esse erro é causado por um comportamento errado do cliente em que ele tenta obter um token de acesso para o serviço do Azure AD Graph. Como o recurso não está presente no AD FS, isso resulta na ID de evento 1021 nos servidores AD FS. É seguro ignorar quaisquer avisos ou erros para o recurso 00000003-0000-0000-C000-000000000000 no AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Por que estou vendo um aviso para falha ao adicionar a conta de serviço de AD FS ao grupo de administradores de chave corporativa?
Esse grupo só é criado quando um controlador de domínio do Windows 2016 com a função de PDC do FSMO existe no domínio. Para resolver o erro, você pode criar o grupo manualmente e seguir o abaixo para dar a permissão necessária depois de adicionar a conta de serviço como membro do grupo.
1.  Abra **Usuários e Computadores do Active Directory**.
2.  **Clique com o botão direito do mouse** no nome de domínio no painel de navegação e **clique em** Propriedades.
3.  **Clique em** Segurança (se a guia segurança estiver ausente, ative recursos avançados no menu Exibir).
4.  **Clique em** Avançadas. **Clique em** Agrega. **Clique em** Selecione uma entidade de segurança.
5.  A caixa de diálogo Selecionar usuário, computador, conta de serviço ou grupo é exibida.  Na caixa de texto Digite o nome do objeto a ser selecionado, digite grupo de administradores de chaves.  Clique em OK.
6.  Na caixa de listagem aplica-se a, selecione **objetos de usuário descendentes**.
7.  Usando a barra de rolagem, role até a parte inferior da página e **clique em** limpar tudo.
8.  Na seção **Propriedades** , selecione **ler msDS-KeyCredentialLink** e **gravar msDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Por que a autenticação moderna de dispositivos Android falhará se o servidor não enviar todos os certificados intermediários na cadeia com o certificado SSL?

Os usuários federados podem ter autenticação no Azure AD para aplicativos que usam a biblioteca ADAL do Android com falha. O aplicativo receberá uma **AuthenticationException** quando tentar mostrar a página de logon. No Chrome, a página de logon AD FS pode ser chamada de forma não segura.

Android – em todas as versões e todos os dispositivos-não oferece suporte ao download de certificados adicionais do campo **authorityInformationAccess** do certificado. Isso também é verdadeiro para o navegador Chrome. Qualquer certificado de autenticação de servidor que não contiver certificados intermediários resultará nesse erro se a cadeia de certificados inteira não for passada de AD FS.

Uma solução adequada para esse problema é configurar os servidores AD FS e WAP para enviar os certificados intermediários necessários juntamente com o certificado SSL.

Ao exportar o certificado SSL de um computador, para ser importado para o repositório pessoal do computador, dos servidores AD FS e WAP, certifique-se de exportar a chave privada e selecionar troca de **informações pessoais-#12 PKCS**.

É importante que a caixa de seleção **inclua todos os certificados no caminho do certificado, se possível** , esteja marcada, bem como **exportar todas as propriedades estendidas**.  

Execute certlm. msc nos servidores Windows e importe o *. PFX no repositório de certificados pessoal do computador. Isso fará com que o servidor passe toda a cadeia de certificados para a biblioteca ADAL.

>[!NOTE]
> O repositório de certificados de balanceadores de carga de rede também deve ser atualizado para incluir toda a cadeia de certificados, se presente

### <a name="does-ad-fs-support-head-requests"></a>AD FS solicitações de cabeçalho de suporte?
AD FS não dá suporte a solicitações HEAD.  Os aplicativos não devem estar usando solicitações HEAD em pontos de extremidade AD FS.  Isso pode causar respostas de erro HTTP inesperadas e/ou atrasadas.  Além disso, você pode ver eventos de erro inesperados no log de eventos do AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Por que não vejo um token de atualização quando estou fazendo logon com um IdP remoto?
Um token de atualização não será emitido se o token emitido pelo IdP tiver um Validty de menos de 1 hora. Para garantir que um token de atualização seja emitido, aumente a validade do token emitido pelo IdP para mais de uma hora.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>Há alguma maneira de alterar o algoritmo de criptografia do token RP?
Por padrão, a criptografia de token RP é definida como AES256 e não pode ser alterada para qualquer outro valor.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>Em um farm de modo misto, recebo um erro ao tentar definir o novo certificado SSL usando Set-AdfsSslCertificate-Thumbprint. Como posso atualizar o certificado SSL em um modo misto AD FS farm?
Os farms de AD FS de modo misto devem ser um estado de transição. É recomendável que, durante o planejamento, o certificado SSL seja revertido antes do processo de atualização ou conclua o processo e aumente o nível de comportamento do farm antes de atualizar o certificado SSL. Caso isso não tenha sido feito, as instruções abaixo fornecem a capacidade de atualizar o certificado SSL. 

Em servidores WAP, você ainda pode usar set-WebApplicationProxySslCertificate. Nos servidores ADFS, você precisa usar o netsh. Siga as etapas conforme indicado abaixo:

1. Selecione o subconjunto de servidores ADFS 2016 para manutenção (por exemplo, remover do balanceador de carga)
2. Nos servidores selecionados em #1, importe o novo certificado via MMC
3. Excluir os certificados existentes

    a. netsh http Delete sslcert hostnameport = FS. contoso. com: 443 b. netsh http Delete sslcert hostnameport = localhost: 443 c. netsh http Delete sslcert hostnameport = FS. contoso. com: 49443

4.  Adicionar o novo certificado

    a. netsh http add sslcert hostnameport = FS. contoso. com: 443 CERTHASH = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = meu sslctlstorename = AdfsTrustedDevices

    b. netsh http add sslcert hostnameport = localhost: 443 CERTHASH = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = meu sslctlstorename = AdfsTrustedDevices

    c. netsh http add sslcert hostnameport = FS. contoso. com: 49443 certhash = CERTTHUMBPRINT AppID = {5d89a20c-BEAB-4389-9447-324788eb944a} certstorename = meu sslctlstorename = AdfsTrustedDevices

5. Reiniciar o serviço ADFS no servidor selecionado
6. Remover o subconjunto de servidores WAP para manutenção
7. Nos servidores WAP selecionados, importe o novo certificado via MMC
8. Definir o novo certificado no WAP usando o cmdlet

    a. Set-WebApplicationProxySslCertificate-impressão digital "CERTTHUMBPRINT"

9. Reiniciar o serviço nos servidores WAP selecionados
10. Coloque os servidores WAP e AD FS selecionados de volta no ambiente de produção.

Execute a atualização no restante dos servidores AD FS e WAP de maneira semelhante.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>O ADFS tem suporte quando os servidores de proxy de aplicativo Web (WAP) estão atrás do firewall do aplicativo Web do Azure (WAF)?
Os servidores de aplicativos Web e ADFS oferecem suporte a qualquer firewall que não execute terminação SSL no ponto de extremidade. Além disso, os servidores ADFS/WAP têm mecanismos internos para evitar ataques comuns da Web, como scripts entre sites, proxy do ADFS e atender a todos os requisitos definidos pelo [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>Estou vendo um "evento 441: Foi encontrado um token com uma chave de associação de token inválido. " O que devo fazer para resolver isso?
No AD FS 2016, a associação de token é habilitada automaticamente e causa vários problemas conhecidos com cenários de proxy e Federação que resultam nesse erro. Para resolver isso, execute o seguinte comando do PowerShell e remova o suporte de associação de token.

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>Atualizei meu farm de AD FS no Windows Server 2016 para AD FS no Windows Server 2019. O nível de comportamento do farm para o farm de AD FS foi gerado com êxito para 2019, mas a configuração do proxy de aplicativo Web ainda é exibida como Windows Server 2016?
Após uma atualização para o Windows Server 2019, a versão de configuração do proxy de aplicativo Web continuará a ser exibida como Windows Server 2016. O proxy de aplicativo Web não tem novos recursos específicos de versão para o Windows Server 2019 e, se o nível de comportamento do farm tiver sido gerado com êxito no AD FS, o proxy de aplicativo Web continuará a ser exibido como Windows Server 2016 por design. 
