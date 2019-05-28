---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: Perguntas frequentes sobre o AD FS 2016
description: Perguntas frequentes para o AD FS 2016
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdd31a8b7c2c6ef87d1d22d901b5c6ca69b5c70d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188722"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>Perguntas frequentes (FAQ) do AD FS


A documentação a seguir é uma casa para perguntas frequentes em relação a serviços de Federação do Active Directory.  O documento foi dividido em grupos com base no tipo de pergunta.

## <a name="deployment"></a>Implantação

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Como posso atualização/migrar de versões anteriores do AD FS
Você pode atualizar o AD FS usando um dos seguintes:


- Windows Server 2012 R2 AD FS para Windows Server 2016 AD FS
    - [Como atualizar para o AD FS no Windows Server 2016 por meio de um banco de dados WID](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Como atualizar para o AD FS no Windows Server 2016 por meio de um banco de dados SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 do AD FS para Windows Server 2012 R2 AD FS
    - [Migrar para o AD FS no Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- O AD FS 2.0 para Windows Server 2012 do AD FS
    - [Migrar para o AD FS no Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- O AD FS 1.x para o AD FS 2.0
    - [Atualização do AD FS 1.x para o AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Se você precisar atualizar do AD FS 2.0 ou 2.1 (Windows Server 2008 R2 ou Windows Server 2012), você deve usar os scripts de caixa de entrada (localizados em C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Por que a instalação do AD FS requer uma reinicialização do servidor?

Suporte ao HTTP/2 foi adicionado no Windows Server 2016, mas o HTTP/2 não pode ser usado para autenticação de certificado do cliente.  Porque muitos cenários do AD FS fazem uso da autenticação de certificado de cliente e um número significativo de clientes suporte não tentar novamente solicitações usando HTTP/1.1, o AD FS farm configuration reconfigura as configurações de HTTP do servidor local para HTTP/1.1.  Isso exige uma reinicialização do servidor.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Está usando o Windows 2016 os servidores do WAP para publicar o farm do AD FS na internet sem atualizar o farm do AD FS de back-end com suporte?
Sim, essa configuração tem suporte, mas nenhum recurso novo do AD FS 2016 teria suporte nesta configuração.  Essa configuração deve ser temporário durante a fase de migração do AD FS 2012 R2 para o AD FS 2016 e não deve ser implantada por longos períodos de tempo.

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>É possível implantar o AD FS para o Office 365 sem um proxy de publicação para o Office 365?
Sim, isso é suportado. No entanto, como um efeito colateral

1. Você precisará gerenciar manualmente a atualização de certificados de autenticação de token porque o Azure AD não poderão acessar os metadados de Federação. Para obter mais informações sobre como atualizar manualmente o certificado de autenticação de token, leia [renovar certificados de federação para o Office 365 e Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. Você não poderá aproveitar a autenticação herdada fluxos (por exemplo, ExO proxy fluxo de autenticação)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>Quais são os requisitos de balanceamento de carga para servidores do AD FS e WAP?

O AD FS é um sistema sem monitoração de estado. Portanto, o balanceamento de carga é bastante simple para logons. A seguir estão as principais recomendações para sistemas de balanceamento de carga.


- Balanceadores de carga não devem ser configurados com a afinidade de IP. Isso pode colocar uma carga indevida em um subconjunto de seus servidores em determinados cenários do Exchange Online.
- Balanceadores de carga não devem encerrar as conexões HTTPS e reiniciar uma nova conexão para o servidor ADFS.
- Balanceadores de carga devem garantir que o endereço IP da conexão deve ser traduzido como o IP de origem no pacote HTTP quando sendo enviados para o AD FS. No caso de um balanceador de carga não é possível enviar o IP de origem no pacote HTTP, o balanceador de carga deve adicionar (ou acrescentar em caso de existente) o endereço IP para o cabeçalho x-forwarded-for. Isso é necessário para a manipulação correta de determinados IP (IP proibidos, inteligente o bloqueio de Extranet...) de recursos relacionados ao e pode levar à segurança reduzida se for configurada incorretamente.
- Balanceadores de carga devem dar suporte a SNI. No evento não existir, certifique-se de que o AD FS está configurado para criar associações de HTTPS para lidar com clientes SNI sem suporte.
- Balanceadores de carga devem usar o ponto de extremidade de investigação de integridade do AD FS HTTP para detectar se os servidores do AD FS ou WAP estão em execução e excluí-los, se uma resposta 200 não Okey é retornado.

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>Quais configurações de várias florestas têm suporte pelo AD FS?

O AD FS dá suporte à configuração de várias floresta vários e se baseia em rede subjacente de confiança do AD DS para autenticar usuários em vários realms confiáveis. É altamente recomendável relações de confiança de florestas de 2 vias, pois essa é uma configuração mais simples para garantir que o subsistema de confiança funciona corretamente sem problemas. Além disso,

- No caso de uma relação de confiança de floresta unidirecional, como uma floresta de DMZ que contém identidades de parceiros, é recomendável implantar o AD FS na floresta corp e tratar a floresta da rede de Perímetro como outro confiança do provedor de declarações local conectada por meio do LDAP. Nesse caso, a autenticação integrada do Windows não funcionará para os usuários da floresta de rede de Perímetro e elas serão necessárias para executar autenticação de senha, pois esse é o único mecanismo com suporte para LDAP. No caso de você não é possível adotar essa opção, você precisaria configurar o ADFS outro na floresta de rede de Perímetro e o adicione como provedor confiável de declarações no AD FS na floresta corp. Os usuários precisarão de descoberta de realm de início, mas a autenticação integrada do Windows e autenticação de senha funcionará. Verifique as alterações apropriadas nas regras de emissão no AD FS na floresta da rede de Perímetro, como o AD FS na floresta corp não poderão obter informações de usuário adicionais sobre o usuário da floresta de rede de Perímetro.
- Enquanto o nível relações de confiança de domínio têm suporte e podem funcionar, é altamente recomendável mudar para um modelo de nível de confiança de floresta. Além disso, você precisa garantir que o roteamento de UPN e a resolução de nomes NETBIOS de nomes de precisem trabalhar com precisão.



## <a name="design"></a>Criar

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quais provedores de autenticação multifator de terceiros estão disponíveis para o AD FS?
Abaixo está uma lista de provedores de terceiros que estamos cientes de.  Pode haver sempre provedores disponíveis que não sabemos sobre e atualizaremos a lista à medida que aprendemos sobre eles.

- [Serviços de segurança e identidade Gemalto](http://www.gemalto.com/identity)
- [serviço inWebo Enterprise Authentication](http://www.inwebo.com/)
- [Conector de API de MFA de pessoas de logon](https://www.loginpeople.com)
- [RSA SecurID Authentication Agent para serviços de Federação do Microsoft Active Directory](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [SafeNet Authentication Service (SAS) Agent para AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Serviço de autenticação de ID móvel Swisscom](http://swisscom.ch/mid)
- [Symantec Validation e ID Protection Service (VIP)](http://www.symantec.com/vip-authentication-service)

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Há suporte para proxies de terceiros com o AD FS?
Sim, os proxies de terceiros podem ser colocados na frente do Proxy de aplicativo Web, mas qualquer proxy de terceiros deve oferecer suporte a [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) a ser usado no lugar de Proxy de aplicativo Web.

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>Quais proxies de terceiros estão disponíveis para o AD FS que dão suporte ao MS-ADFSPIP?

Abaixo está uma lista de provedores de terceiros que estamos cientes de.  Pode haver sempre provedores disponíveis que não sabemos sobre e atualizaremos a lista à medida que aprendemos sobre eles.

- [F5 Access Policy Manager](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Onde está a planejamento de capacidade planilha de dimensionamento para AD FS 2016?
A versão do AD FS 2016 da planilha pode ser baixada [aqui](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Isso também pode ser usado para o AD FS no Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Como garantir requisitos do ATP da Apple de Meu suporte a servidores do AD FS e WAP?

Apple lançou um conjunto de requisitos, chamado de segurança de transporte de aplicativo (ATS) que podem afetar a chamadas de aplicativos do iOS que se autenticar no AD FS.  Você pode garantir que o AD FS e estar de acordo com os servidores WAP, certificando-se de que eles dão suporte a [requisitos para se conectar usando o ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
Em particular, você deve verificar seus servidores AD FS e WAP dar suporte a TLS 1.2 e que o conjunto de codificação negociado da conexão TLS suportará sigilo.

Você pode habilitar e desabilitar o SSL 2.0 e 3.0 e TLS versões 1.0, 1.1 e 1.2 usando [Gerenciar protocolos de SSL no AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Para garantir que o AD FS e WAP servidores negociem apenas conjuntos de codificação TLS que dão suporte a ATP, você pode desabilitar todos os conjuntos de codificação que não estão na [lista de conjuntos de codificação compatíveis de ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Para fazer isso, use o [cmdlets do Windows PowerShell de TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index).

## <a name="developer"></a>Desenvolvedor

### <a name="when-generating-an-idtoken-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-idtoken"></a>Ao gerar um id_token com o ADFS para um usuário autenticado no AD, como a declaração "sub" é gerada no id_token?
O valor da declaração de "sub" é o hash da ID do cliente + valor de declaração de ancoragem.

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>Quando o usuário fizer logon por meio de uma relação de confiança do provedor de declarações remoto por WS-Fed/SAML-P, o que é o tempo de vida do token de acesso/token de atualização?
O tempo de vida de token de atualização será o tempo de vida do token que o ADFS foi obtido do provedor de declarações remoto. O tempo de vida do token de acesso será o tempo de vida de token da terceira parte confiável para o qual o token de acesso está sendo emitido.

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>É necessário retornar o perfil e email escopos bem além do escopo da OpenId. Posso obter informações adicionais usando escopos? Como fazer isso no AD FS?
Você pode usar o id_token personalizado para adicionar informações relevantes no id_token em si. Para obter mais informações, consulte o artigo [personalizar declarações a serem emitidos no id_token](../development/Custom-Id-Tokens-in-AD-FS.md).

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>Como emitir blobs json dentro de tokens JWT?
Um ValueType especial ("http://www.w3.org/2001/XMLSchema#json") e de escape character(\x22) para isso foi adicionado no AD FS 2016. Consulte o exemplo abaixo para a regra de emissão e também a saída final do token de acesso.

Exemplo de regra de emissão:

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

Declaração emitida no token de acesso:

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>Pode passar o valor do recurso como parte do valor de escopo, semelhante a como as solicitações são executadas no Azure AD?
Com o AD FS no servidor de 2019, você agora pode passar o valor do recurso inserido no parâmetro de escopo. O parâmetro de escopo agora pode ser organizado como uma lista separada por espaço em que cada entrada é a estrutura como recurso/escopo. Por exemplo  
**< criar uma solicitação de exemplo válido >**

### <a name="does-ad-fs-support-pkce-extension"></a>O AD FS dá suporte à extensão PKCE?
AD FS no servidor de 2019 dá suporte a chave de prova para código de câmbio (PKCE) para o fluxo de concessão de código de autorização do OAuth

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>Quais escopos permitidos são suportados pelo AD FS?
- aza - se usando [extensões do protocolo OAuth 2.0 para clientes de Broker](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706) e se o parâmetro de escopo contém o escopo "aza", o servidor emite um novo token de atualização primários e define-o no campo refresh_token da resposta, bem como a configuração o campo refresh_token_expires_in ao tempo de vida do token de atualização primários novos se um é imposto.
- OpenID - permite que o aplicativo solicitar o uso do protocolo de autorização do OpenID Connect.
- logon_cert - logon_cert escopo permite que um aplicativo para solicitar certificados de logon, que pode ser usado ao fazer logon interativamente a usuários autenticados. Servidor do AD FS omite o parâmetro access_token da resposta e em vez disso, fornece uma cadeia de certificados CMS codificada em base64 ou uma resposta PKI completa CMC. Mais detalhes disponíveis [aqui](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e). 
- user_impersonation - o escopo user_impersonation é necessário para solicitar com êxito um token de acesso em nome do AD FS. Para obter detalhes sobre como usar este escopo, consulte [compilar um aplicativo de várias camadas usando em nome (OBO) usando o OAuth com o AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md).
- vpn_cert - vpn_cert escopo permite que um aplicativo para solicitar certificados de VPN, o que pode ser usado para estabelecer conexões de VPN usando a autenticação EAP-TLS. Isso não é tem mais suporte.
- email – permite que o aplicativo solicitar a declaração de email para o usuário conectado. Isso não é tem mais suporte. 
- perfil - permite que o aplicativo solicitar um perfil relacionadas a declarações para o usuário entrar. Isso não é tem mais suporte. 


## <a name="operations"></a>Operações

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Como faço para substituir o certificado SSL do AD FS?
O certificado SSL do AD FS não é o mesmo que o certificado de comunicações de serviço do AD FS encontrado no snap-in de gerenciamento do AD FS.  Para alterar o certificado SSL do AD FS, você precisará usar o PowerShell. Siga as diretrizes no artigo abaixo:

[Como gerenciar certificados SSL no AD FS e no WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Como habilitar ou desabilitar as configurações de TLS/SSL para o AD FS
Para habilitar ou desabilitar protocolos SSL e conjuntos de criptografia, use o seguinte:

[Gerenciar protocolos SSL no AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>O certificado SSL de proxy precisa ser o mesmo que o certificado SSL do AD FS?  
Use as diretrizes a seguir em relação ao certificado SSL de proxy e o certificado SSL do AD FS:


- Se o proxy é usado para solicitações de proxy do AD FS que usam autenticação integrada do Windows, o certificado SSL de proxy devem ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Se a propriedade do AD FS "ExtendedProtectionTokenCheck" é habilitado (a configuração padrão no AD FS), o certificado SSL de proxy deve ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Caso contrário, o certificado SSL de proxy pode ter uma chave diferente do certificado SSL do AD FS, mas deve atender os mesmos [requisitos](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>Como configurar prompt = o comportamento de logon para o AD FS?
Para obter informações sobre como configurar o prompt = login, consulte [solicitar que os serviços de Federação do Active Directory = suporte ao parâmetro de logon](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-change-the-ad-fs-service-account"></a>Como alterar a conta de serviço do AD FS?
Para alterar a conta de serviço do AD FS, siga as instruções usando a caixa de ferramentas do AD FS [módulo do Powershell de conta de serviço](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule). 

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Como configurar a navegadores para usar a autenticação integrada do Windows (WIA) com o AD FS?

Para obter informações sobre como configurar os navegadores, consulte [configurar navegadores para usar a autenticação integrada do Windows (WIA) com o AD FS](../operations/Configure-AD-FS-Browser-WIA.md).

### <a name="can-i-trun-off-browserssoenabled"></a>É possível Desligue BrowserSsoEnabled?
Se você não tiver políticas de controle de acesso com base no dispositivo no AD FS ou o Windows Hello para registro de certificado de negócios usando o ADFS; Você pode desativar BrowserSsoEnabled. BrowserSsoEnabled permite que o ADFS coletar um PRT (Token de atualização principal) do cliente que contém informações de dispositivo. Sem esse dispositivo de autenticação do ADFS não funcionará em dispositivos Windows 10.

### <a name="how-long-are-ad-fs-tokens-valid"></a>Quanto tempo são tokens do AD FS válidos?

Geralmente, essa pergunta significa 'quanto tempo os usuários fazer logon único (SSO) sem a necessidade de inserir novas credenciais e como eu como um administrador pode controlar isso?'  Esse comportamento e as definições de configuração que controlam, são descritas no artigo [configurações do AD FS único logon](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Os tempos de vida padrão dos vários cookies e tokens estão listados abaixo (bem como os parâmetros que controlam os tempos de vida):

**Dispositivos registrados**

- Cookies PRT e SSO: máximo, 90 dias é regido por PSSOLifeTimeMins. (Dispositivo fornecido é usado pelo menos a cada 14 dias, que é controlada pela DeviceUsageWindow)

- Token de atualização: calculada com base nos itens acima para fornecer um comportamento consistente

- access_token: 1 hora por padrão, com base na terceira

- id_token: mesmo que o token de acesso

**Dispositivos não registrados**

- Cookies de SSO: 8 horas por padrão, são regidos por SSOLifetimeMins.  Ao manter-Me conectado no (KMSI) estiver habilitado, o padrão é 24 horas e podem ser configurados por meio de KMSILifetimeMins.


- Token de atualização: 8 horas por padrão. 24 horas com KMSI habilitado

- access_token: 1 hora por padrão, com base na terceira

- id_token: mesmo que o token de acesso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>O AD FS dá suporte a segurança de transporte estrito HTTP (HSTS)?  

Segurança de transporte estrito HTTP (HSTS) é um mecanismo de política de segurança da web que ajuda a atenuar ataques de downgrade de protocolo e sequestro de cookie para serviços que têm pontos de extremidade HTTP e HTTPS. Ele permite que os servidores web para declarar que navegadores da web (ou outros agentes de usuário complying) só devem interagir com ela usando HTTPS e nunca por meio do protocolo HTTP.

Todos os pontos de extremidade do AD FS para o tráfego de autenticação da web são abertos exclusivamente via HTTPS.  Como resultado, o AD FS efetivamente reduz as ameaças que fornece o mecanismo de diretiva de segurança de transporte estrito HTTP (por design não há nenhum downgrade para HTTP, pois não há nenhum ouvinte HTTP). Além disso, o AD FS impede que os cookies sendo enviados para outro servidor com pontos de extremidade de protocolo HTTP, marcando a todos os cookies com o sinalizador seguro.

Portanto, a implementar a HSTS em um servidor do AD FS não é necessário porque nunca pode ser desatualizado.  Para fins de conformidade, servidores do AD FS atender a esses requisitos porque eles nunca podem usar HTTP e todos os cookies são marcados como seguros.

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-forwarded--ip do cliente não contém o IP do cliente, mas contém o IP do firewall na frente do proxy. Onde posso obter o IP correto do cliente?
Não é recomendável fazer a terminação SSL antes de WAP. No caso de terminação SSL é feita na frente do WAP, o X-ms-forwarded--ip do cliente irá conter o IP do dispositivo de rede na frente do WAP. Abaixo está que uma breve descrição do IP diversos relacionados declarações que são suportadas pelo AD FS:
 - x-ms-client-ip : IP do dispositivo que conectado para o STS de rede.  No caso de uma solicitação de extranet, isso sempre contém o IP do WAP.
 - x-ms-forwarded-client-ip : Declaração de valores múltiplos que irá conter quaisquer valores encaminhados ao ADFS pelo Exchange Online mais o endereço IP do dispositivo que é conectado a WAP.
 - Userip: Para solicitações de extranet essa declaração conterá o valor de x-ms-forwarded--ip do cliente.  Para solicitações de intranet, essa declaração conterá o mesmo valor de x-ms-client-ip.

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Estou tentando obter declarações adicionais sobre o ponto de extremidade de informações do usuário, mas seu retornando somente entidade. Como posso obter declarações adicionais?
O ponto de extremidade do ADFS userinfo sempre retorna a declaração do assunto conforme especificado nos padrões do OpenID. O AD FS não fornece declarações adicionais solicitadas por meio do ponto de extremidade UserInfo. Se você precisar de declarações adicionais no token de ID, consulte [Tokens de ID personalizados no AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>Por que vejo vários erros 1021 em meus servidores do AD FS?
Esse evento é registrado, geralmente, para um acesso de recurso inválido no AD FS para o recurso 00000003-0000-0000-c000-000000000000. Esse erro é causado por um comportamento de erro do cliente em que ele tentará obter um token de acesso para o serviço do Azure AD Graph. Uma vez que o recurso não está presente no AD FS, isso resulta no evento ID 1021 nos servidores do AD FS. É seguro ignorar os avisos ou erros para o recurso 00000003-0000-0000-c000-000000000000 no AD FS.

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Por que estou vendo um aviso para Falha ao adicionar a conta de serviço do AD FS ao grupo de administradores de chave da empresa?
Esse grupo é criado somente quando um controlador de domínio do Windows 2016 com a função de FSMO PDC existe no domínio. Para resolver o erro, você pode criar o grupo manualmente e execute a seguir para conceder a permissão necessária depois de adicionar a conta de serviço como membro do grupo.
1.  Abra **Usuários e Computadores do Active Directory**.
2.  **Clique com botão direito** seu nome de domínio no painel de navegação e **clique em** propriedades.
3.  **Clique em** segurança (se a guia Segurança está ausente, ativar recursos avançados no menu Exibir).
4.  **Clique em** avançadas. **Clique em** adicionar. **Clique em** selecionar uma entidade.
5.  A caixa de diálogo Selecionar usuário, computador, conta de serviço ou grupo é exibida.  Em Digite o nome do objeto a caixa de seleção de texto, digite o grupo de administradores de chave.  Clique em OK.
6.  Em aplica-se a caixa de listagem, selecione **objetos de usuário descendentes**.
7.  Usando a barra de rolagem, role até a parte inferior da página e **clique** Limpar tudo.
8.  No **propriedades** seção, selecione **ler msDS-KeyCredentialLink** e **gravar msDS-KeyCredentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Por que a autenticação moderna de dispositivos Android falha se o servidor não enviar todos os certificados intermediários na cadeia com o certificado SSL?

Os usuários federados podem enfrentar a autenticação do AD do Azure para aplicativos que usam a falha de biblioteca do ADAL do Android. O aplicativo obterá um **AuthenticationException** quando ele tenta exibir a página de logon. No chrome, o AD FS página de logon pode ser destacada como não segura.

Android – em todas as versões e todos os dispositivos – não oferece suporte a baixar os certificados adicionais do **authorityInformationAccess** campo do certificado. Isso é verdadeiro do navegador Chrome. Qualquer certificado de autenticação de servidor que não tem certificados intermediários resultará nesse erro se a cadeia de certificados inteira não é passada do AD FS.

Uma solução adequada para esse problema é configurar os servidores do AD FS e WAP para enviar os certificados intermediários necessários junto com o certificado SSL.

Quando exportar o certificado SSL, de um computador, para ser importado para o computador armazenamento pessoal, do AD FS e servidores WAP, certifique-se de exportar a chave privada e selecione **troca de informações pessoais - PKCS n º 12**.

É importante que a caixa de seleção para **incluir todos os certificados no caminho do certificado, se possível** estiver marcada, bem como **exportar todas as propriedades estendidas**.  

Execute certlm nos servidores Windows e importe o *. PFX no repositório de certificados pessoal do computador. Isso fará com que o servidor passar a cadeia de certificados inteira para a biblioteca ADAL.

>[!NOTE]
> Certificado de armazenamento de rede balanceadores de carga também deve ser atualizados para incluir a cadeia de certificados inteira se estiver presente

### <a name="does-ad-fs-support-head-requests"></a>O AD FS dá suporte a solicitações HEAD?
O AD FS não dá suporte a solicitações HEAD.  Aplicativos não devem usar solicitações HEAD nos pontos de extremidade do AD FS.  Isso pode causar respostas de erro HTTP serão inesperado e/ou atrasada.  Além disso, você poderá ver eventos de erro inesperado no log de eventos do AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Por que eu não estou vendo um token de atualização quando eu estou fazendo logon com um IdP remoto?
Um token de atualização não será emitido se o token emitido pelo IdP tem um validty de menos de 1 hora. Para garantir que um token de atualização é emitido, aumente a validade do token emitido pelo IdP para mais de 1 hora.

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>Temos alguma maneira de alterar o algoritmo de criptografia de token da RP?
Por padrão, a criptografia de token da RP é definida como AES256 e ele não pode ser alterado para qualquer outro valor.

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>Em um farm de modo misto, recebo erros ao tentar definir o novo certificado SSL usando Set-AdfsSslCertificate-impressão digital. Como é possível atualizar o certificado SSL em um farm do AD FS de modo misto?
Nos servidores WAP, você ainda pode usar o conjunto WebApplicationProxySslCertificate. Nos servidores do AD FS, você precisa usar o netsh. Siga as etapas conforme indicado abaixo:

1. Selecionar um subconjunto dos servidores ADFS 2016 para manutenção (por exemplo, remover do balanceador de carga)
2. Nos servidores selecionados na #1, importar o novo certificado por meio do MMC
3. Excluir os certificados existentes

    a. Netsh http delete sslcert hostnameport=fs.contoso.com:443 b. Netsh http delete sslcert hostnameport = localhost:443 c. Netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  Adicionar o novo certificado

    a. Netsh http adicionar sslcert hostnameport=fs.contoso.com:443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    c. Netsh http adicionar sslcert hostnameport=fs.contoso.com:49443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

5. Reinicie o serviço do ADFS no servidor selecionado
6. Remover o subconjunto dos servidores do WAP para manutenção
7. Nos servidores WAP selecionados, importar o novo certificado por meio do MMC
8. Definir o novo certificado usando o cmdlet de WAP

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. Reinicie o serviço nos servidores WAP selecionados
10. Coloque os servidores WAP e AD FS selecionados novamente no ambiente de produção.

Execute a atualização no restante do AD FS e servidores do WAP de maneira semelhante.

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>AD FS tem suporte quando os servidores de Proxy de aplicativo da Web (WAP) estão por trás de Firewall(WAF) de aplicativo Web do Azure?
ADFS e servidores de aplicativos Web dão suporte a qualquer firewall que não faz a terminação SSL no ponto de extremidade. Além disso, os servidores do AD FS/WAP tem mecanismos internos para evitar ataques da web comuns, como o proxy do ADFS, scripts entre sites e atender a todos os requisitos definidos pela [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx).
