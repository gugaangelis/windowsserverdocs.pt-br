---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: PERGUNTAS FREQUENTES SOBRE O AD FS 2016
description: Perguntas frequentes para o AD FS 2016
author: jenfieldmsft
ms.author: billmath
manager: femila
ms.date: 03/06/2018
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 313447d2c92c15505434ec5c39898ca84aef46db
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS perguntas frequentes (FAQ)

>Aplica-se a: Windows Server 2016

A documentação a seguir é um lar para perguntas frequentes em relação a serviços de Federação do Active Directory.  O documento foi dividido em grupos com base no tipo de pergunta.

## <a name="deployment"></a>Implantação 

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>Como posso atualizar/migrar de versões anteriores do AD FS
Você pode atualizar o AD FS usando um destes procedimentos:


- Windows Server 2012 R2 AD FS para Windows Server 2016 AD FS
    - [Atualizar para o AD FS no Windows Server 2016 usando um banco de dados de trabalho](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [Atualizar para o AD FS no Windows Server 2016 usando um banco de dados SQL](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS do Windows Server 2012 R2 AD FS
    - [Migrar para o AD FS no Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 para o Windows Server 2012 AD FS
    - [Migrar para o AD FS no Windows Server 2012](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1. x para AD FS 2.0 
    - [Atualize do AD FS 1. x para AD FS 2.0](https://technet.microsoft.com/library/ff678035.aspx)

Se você precisar atualizar a partir do AD FS 2.0 ou 2.1 (Windows Server 2008 R2 ou Windows Server 2012), você deve usar os scripts de caixa de entrada (localizados na C:\Windows\ADFS).

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>Por que a instalação do AD FS requer uma reinicialização do servidor?

Suporte a HTTP/2 foi adicionado no Windows Server 2016, mas o HTTP/2 não pode ser usado para autenticação de certificado cliente.  Porque muitos cenários de AD FS fazem uso de autenticação de certificado de cliente e um número significativo de clientes não suporte à repetição solicita usando HTTP/1.1, AD FS farm configuração novamente configura as configurações de HTTP do servidor local ao HTTP/1.1.  Isso requer uma reinicialização do servidor.  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Está usando o Windows 2016 WAP servidores para publicar sítio AD FS à internet sem sítio AD FS back-end com suporte a atualização?
Sim, essa configuração é suportada, mas nenhum novo recurso do AD FS 2016 com suporte nesta configuração.  Essa configuração deve ser temporário durante a fase de migração do AD FS 2012 R2 AD FS 2016 e não deve ser implantada por longos períodos de tempo.

## <a name="design"></a>Design

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>Quais provedores de autenticação multifator de terceiros estão disponíveis para o AD FS? 
Abaixo há uma lista de provedores de terceiros que estamos cientes de.  Sempre pode haver provedores disponíveis que não conhecemos e atualizamos a lista como nós conhecê-los.

- [Serviços de segurança e identidade Gemalto](http://www.gemalto.com/identity)
- [inWebo serviço de autenticação empresarial](http://www.inwebo.com/)
- [Conector de pessoas MFA API de logon](https://www.loginpeople.com)
- [Agente de autenticação RSA SecurID para serviços de Federação do Microsoft Active Directory](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [Agente de serviço (SAS) de autenticação SafeNet do AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Serviço de autenticação de ID de Swisscom Mobile](http://swisscom.ch/mid)
- [Validação da Symantec e serviço de proteção de ID (VIP)](http://www.symantec.com/vip-authentication-service) 

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>Proxies de terceiros são compatíveis com o AD FS?
Sim, proxies de terceiros podem ser colocados na frente do Proxy de aplicativo Web, mas qualquer proxy de terceiros deve dar suporte a [protocolo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) a ser usado no lugar do Proxy de aplicativo Web.

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>Quais proxies de terceiros estão disponíveis para o AD FS que dão suporte a MS-ADFSPIP?

Abaixo há uma lista de provedores de terceiros que estamos cientes de.  Sempre pode haver provedores disponíveis que não conhecemos e atualizamos a lista como nós conhecê-los.

- [F5 Gerenciador de política de acesso](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>Onde está a planejamento da capacidade planilha de dimensionamento para AD FS 2016?
A versão do AD FS 2016 da planilha pode ser baixada [aqui](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx).
Isso também pode ser usado para o AD FS no Windows Server 2012 R2.

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>Como garantir requisitos de ATP da Apple de Meu suporte de servidores do AD FS e WAP?

Apple lançou um conjunto de requisitos chamado segurança de transporte de aplicativo (ATS) que podem ter impacto em chamadas de aplicativos do iOS que se autenticar em AD FS.  Você pode garantir que o AD FS e servidores WAP estar em conformidade, certificando-se de que eles dão suporte a [requisitos para se conectar usando ATS](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  
Em particular, você deve verificar que os servidores do AD FS e WAP dão suporte a TLS 1.2 e que o pacote de codificação negociado da conexão TLS dará suporte sigilo.

Você pode habilitar e desabilitar versões SSL 2.0 e 3.0 e TLS 1.0, 1.1 e 1.2 usando [Gerenciar protocolos SSL no AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md).

Para garantir o AD FS e WAP servidores negociam apenas pacotes de codificação TLS que dão suporte a ATP, você pode desabilitar todos os pacotes de codificação que não estão a [lista de pacotes de codificação em conformidade de ATP](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57).  Para fazer isso, use o [cmdlets do Windows PowerShell de TLS](https://technet.microsoft.com/itpro/powershell/windows/tls/index). 


## <a name="operations"></a>Operações

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>Como substituir o certificado SSL do AD FS? 
O certificado SSL do AD FS não é o mesmo que o certificado de comunicações de serviço do AD FS encontrado no snap-in de gerenciamento do AD FS.  Para alterar o certificado SSL do AD FS, você precisará usar o PowerShell. Siga as orientações no artigo abaixo:

[Gerenciamento de certificados SSL no AD FS e WAP 2016](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>Como habilitar ou desabilitar configurações de TLS/SSL do AD FS
Para desativar ou ativar protocolos SSL e pacotes de codificação, use o seguinte:

[Gerenciar protocolos SSL no AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>O certificado SSL proxy precisa ser o mesmo que o certificado SSL do AD FS?  
Use as seguintes diretrizes em relação ao certificado SSL de proxy e o certificado SSL do AD FS:


- Se o proxy é usado para solicitações de proxy do AD FS que usam a autenticação integrada do Windows, o certificado SSL proxy devem ser os mesmos (use a mesma chave) como o certificado SSL do servidor de Federação
- Se a propriedade AD FS "ExtendedProtectionTokenCheck" está habilitado (a configuração padrão no AD FS), o certificado SSL proxy deve ser o mesmo (use a mesma chave) que o certificado SSL do servidor de Federação
- Caso contrário, o certificado SSL proxy pode ter uma chave diferente do certificado AD FS SSL, mas devem atender a mesma [requisitos](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>Como configurar prompt = comportamento de logon do AD FS?
Para obter informações sobre como configurar o prompt = logon, veja [solicitar que os serviços de Federação do Active Directory = suporte de parâmetro de logon](../operations/AD-FS-Prompt-Login.md).

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Como configurar o navegadores para usar autenticação integrada do Windows (WIA) com o AD FS?

Para obter informações sobre como configurar navegadores consulte [configurar navegadores para usar autenticação integrada do Windows (WIA) com o AD FS](../operations/Configure-AD-FS-Browser-WIA.md).


### <a name="how-long-are-ad-fs-tokens-valid"></a>Quanto tempo são tokens do AD FS válidos?

Muitas vezes essa pergunta significa 'quanto tempo os usuários fazer logon único (SSO) sem precisar inserir novas credenciais, e como posso como um administrador pode controlar que?'  Esse comportamento e as configurações que controlam, são descritas no artigo [aqui](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings).

Os tempos de vida padrão dos vários cookies e tokens são listados abaixo (bem como os parâmetros que regem os tempos de vida):

**Dispositivos registrados**

- Cookies PRT e SSO: máximo, 90 dias regido PSSOLifeTimeMins. (Dispositivo fornecido é usado pelo menos a cada 14 dias, que é controlada pelo DeviceUsageWindow)

- Atualizar o token: calculado com base em acima para fornecer um comportamento consistente

- access_token: 1 hora por padrão, com base no terceiro

- id_token: mesmo que o token de acesso

**Dispositivos cancelados**

- Cookies SSO: 8 horas por padrão, regido SSOLifetimeMins.  Quando o Keep Me conectado (KMSI) está habilitado, o valor padrão é 24 horas e configurável por meio de KMSILifetimeMins.


- Atualizar o token: 8 horas por padrão. 24 horas com KMSI habilitada

- access_token: 1 hora por padrão, com base no terceiro

- id_token: mesmo que o token de acesso

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS dá suporte a HTTP Strict Transport Security (HSTS)?  

HTTP Strict Transport Security (HSTS) é um mecanismo de política de segurança da web que ajuda a atenuar ataques de downgrade de protocolo e sequestro de cookie de serviços que têm pontos de extremidade HTTP e HTTPS. Ele permite que os servidores web para declarar que os navegadores da web (ou outros complying agentes do usuário) devem interagir apenas com ele usando HTTPS e nunca por meio do protocolo HTTP.

Todos os pontos de extremidade do AD FS para o tráfego de autenticação da web são abertos exclusivamente por HTTPS.  Como resultado, o AD FS reduz efetivamente as ameaças que fornece mecanismo de política de HTTP Strict Transport Security (por design não há nenhuma downgrade para HTTP como não há nenhum ouvintes em HTTP). Além disso, o AD FS impede que cookies sejam enviados para outro servidor com pontos de extremidade do protocolo HTTP marcando todos os cookies com o sinalizador seguro.

Portanto, a implementação HSTS em um servidor do AD FS não é necessária porque ele nunca pode ser downgrade.  Para fins de conformidade, servidores do AD FS cumprir estes requisitos porque eles nunca podem usar HTTP e todos os cookies são marcados seguros.

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-encaminhado-client-ip não contém o IP do cliente, mas contém IP do firewall na frente do proxy. Onde posso obter o direito IP do cliente?
Não é recomendável fazer encerramento SSL antes WAP. No caso de encerramento do SSL é feito na frente do WAP, o X-ms-encaminhado-client-ip conterá o IP do dispositivo de rede na frente WAP. A seguir está que uma breve descrição de IP diversos relacionados declarações com suporte pelo AD FS:
 - x-ms-client-ip: rede IP do dispositivo que conectado ao STS.  No caso de uma solicitação extranet sempre contém o IP do WAP.
 - x-ms-encaminhado-client-ip: declaração múltiplos valores que irá conter quaisquer valores encaminhados para ADFS pelo Exchange Online e o endereço IP do dispositivo que conectado ao WAP.
 - Userip: Para solicitações de extranet dessa declaração conterá o valor de x-ms-encaminhado-client-ip.  Para solicitações de intranet, essa declaração conterá o mesmo valor x-ms-client-ip.

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>Estou tentando obter declarações adicionais no ponto de extremidade de informações do usuário, mas ele só está retornando o assunto. Como obter declarações adicionais?
O ponto de extremidade do AD FS userinfo sempre retorna a declaração de assunto conforme especificado nos padrões OpenID. AD FS não fornece declarações adicionais solicitadas por meio do ponto de extremidade UserInfo. Se você precisar declarações adicionais no token de ID, consulte [Tokens de ID personalizada no AD FS](../development/custom-id-tokens-in-ad-fs.md).

### <a name="why-do-i-see-alot-of-1021-errors-on-my-ad-fs-servers"></a>Por que vejo muita 1021 erros em servidores meu AD FS?
Esse evento é registrado geralmente para um acesso de recurso inválidas no AD FS para recurso 00000003-0000-0000-c000-000000000000. Esse erro é causado por um comportamento do cliente em que ele tenta obter um token de acesso para o serviço do Azure AD Graph incorretos. Desde que o recurso não está presente no AD FS, isso resulta em evento ID 1021 nos servidores do AD FS. É seguro ignorar os avisos ou erros de recurso 00000003-0000-0000-c000-000000000000 no AD FS. 

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>Por que estou vendo um aviso de falha adicionar a conta de serviço do AD FS a chave grupo Administradores de empresa?
Esse grupo só é criado quando um controlador de domínio do Windows 2016 com a função FSMO PDC existe no domínio. Para resolver o erro, você pode criar o grupo manualmente e siga o abaixo para conceder a permissão necessária depois de adicionar a conta de serviço como membro do grupo.
1.  Abrir **usuários e computadores Active Directory**. 
2.  **Clique com botão direito** seu nome de domínio no painel de navegação e **clique** propriedades.
3.  **Clique em** segurança (se a guia segurança estiver ausente, ativar recursos avançados no menu Exibir).
4.  **Clique em** avançadas. **Clique em** adicionar. **Clique em** selecionar uma entidade de segurança.
5.  A caixa de diálogo Selecionar usuários, computador, conta de serviço ou grupo é exibida.  Em Digite o nome do objeto para caixa de seleção de texto, digite grupo de administração de chave.  Clique em Okey.
6.  No aplica-se a caixa de listagem, selecione **objetos de usuário descendente**.
7.  Usando a barra de rolagem, role até a parte inferior da página e **clique** Limpar tudo.
8.  No **propriedades** seção, selecione **ler msDS-KeyCredentialLink** e **escrever msDS-KeyCrendentialLink**.

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>Por que a autenticação moderna de dispositivos Android falhar se o servidor não enviar todos os certificados intermediários na cadeia com o certificado SSL?

Os usuários federados podem enfrentar autenticação no Azure AD para aplicativos que usam a Android ADAL ocorrer uma falha na biblioteca. O aplicativo terá um **AuthenticationException** quando tentar mostrar a página de logon. No chrome o AD FS página de logon pode ser explicada como inseguras.

Android – em todas as versões e todos os dispositivos - não oferece suporte a download certificados adicionais do **authorityInformationAccess** campo do certificado. Isso também é verdadeiro do navegador Chrome. Qualquer certificado de autenticação de servidor que não tem os certificados intermediários resultará nesse erro se a cadeia de certificados inteira não forem repassada do AD FS. 

Uma solução adequada para esse problema é configurar os servidores do AD FS e WAP para enviar os certificados intermediários necessários juntamente com o certificado SSL. 

Quando estiver exportando o certificado SSL, de um computador, a ser importado para o armazenamento pessoal do computador, dos servidores AD FS e WAP, certifique-se de exportar o particular chave e selecione **troca de informações pessoais - PKCS #12**. 

É importante que a caixa de seleção para **incluir todos os certificados no caminho de certificado, se possível** estiver marcada, bem como **exportar todas as propriedades estendidas**. 

Execute certlm.msc nos servidores do Windows e importar o *. PFX no repositório de certificados pessoais do computador. Isso fará com que o servidor passar a cadeia de certificados inteira para a biblioteca ADAL. 

>[!NOTE]
> Rede Load balanceadores certificado loja também devem ser atualizados para incluir a cadeia de certificados inteira se presente

### <a name="does-ad-fs-support-head-requests"></a>AD FS dá suporte a solicitações HEAD?
AD FS não dá suporte a solicitações HEAD.  Aplicativos não devem estar usando solicitações HEAD contra pontos de extremidade do AD FS.  Isso pode causar respostas de erro HTTP que estão inesperado e/ou posterior.  Além disso, você pode ver eventos de erro inesperado no log de eventos do AD FS.

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>Por que não vejo um token de atualização quando estou registro em log com um IdP remoto?
Um token de atualização não foi emitido se o token emitido por IdP tem um validty de menos de 1 hora. Para garantir que um token de atualização é emitido, aumente a validade do token emitido pelo IdP para mais de 1 hora.
