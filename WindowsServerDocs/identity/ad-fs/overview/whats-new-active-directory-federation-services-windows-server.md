---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novidades nos Serviços de Federação do Active Directory (AD FS) para Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 02/26/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: faa0590dc38921a56952aa54bf38243b6ff84d82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867707"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Novidades nos Serviços de Federação do Active Directory (AD FS)


>Aplica-se a: Windows Server 2019, Windows Server 2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>O que há de novo no Active Directory Directory Federation Services para Windows Server 2019

### <a name="protected-logins"></a>Logons protegidos
Este é um breve resumo de atualizações a serem protegidos logons disponíveis no AD FS 2019:
- **Provedores de autenticação externos como primário** -os clientes podem agora usar 3ª produtos de autenticação de terceiros como o primeiro fator e não expõem as senhas como o primeiro fator. Nos casos em que um provedor de autenticação externa pode provar 2 fatores, ele poderá solicitar MFA. 
- **Autenticação de senha como autenticação adicional** -os clientes têm uma opção de caixa de entrada com suporte completo para usar a senha somente para os fatores adicionais depois de uma senha menor opção é usada como o primeiro fator. Isso melhora a experiência do cliente do ADFS 2016 em que os clientes tinham para baixar um adaptador do github que tem suporte como está. 
- **Framework de módulo de ameaça conectável** -os clientes agora podem criar seu próprios plug-in módulos bloquear determinados tipos de solicitações durante a fase de pré-autenticação. Isso torna mais fácil para os clientes usem inteligência da nuvem, como proteção de identidade para bloquear os logons para usuários arriscados ou transações arriscadas.
- **Aprimoramentos de ESL** -aprimora o QFE ESL em 2016, adicionando os seguintes recursos
    - Permite que os clientes estejam no modo de auditoria ao que está sendo protegido pela funcionalidade de bloqueio de extranet 'clássico' disponível desde o ADFS 2012R2. Atualmente, os clientes de 2016 teria sem proteção no modo de auditoria. 
    - Permite que o limite de bloqueio independentes para locais familiares. Isso torna possível para várias instâncias de aplicativos em execução com uma conta de serviço comum sobrepor senhas com o mínimo de impacto. 

### <a name="additional-security-improvements"></a>Aprimoramentos de segurança adicionais
As seguintes melhorias de segurança adicionais estão disponíveis no AD FS 2019:
- **PSH remota usando o logon de cartão inteligente** - clientes pode agora uso cartões quando inteligentes para remoto se conectar ao AD FS por meio do PSH e uso que gerenciar PSH todas as funções incluem cmdlets PSH com vários nós.
- **Personalização do cabeçalho HTTP** -os clientes agora podem personalizar cabeçalhos HTTP emitidos durante as respostas do ADFS. Isso inclui os seguintes cabeçalhos
     - HSTS: Isso transmite que pontos de extremidade do AD FS podem ser usados apenas em pontos de extremidade HTTPS para um navegador em conformidade para impor
     - x-frame-options: Permite aos administradores do AD FS permitir que terceiras partes confiáveis específicas incorporar o iFrames para páginas de logon interativo do ADFS. Isso deve ser usado com cuidado e somente em hosts HTTPS. 
     - Cabeçalho futuras: Cabeçalhos adicionais de futuros podem ser configurados também. 

Para obter mais informações, consulte [cabeçalhos de resposta HTTP personalizar segurança com o AD FS de 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>Recursos/política de autenticação
São os seguintes recursos/política de autenticação do AD FS 2019:
- **Especifique o método de autenticação para autenticação adicional por RP** -os clientes podem agora usar declarações de regras para decidir qual provedor de autenticação adicional para invocar para o provedor de autenticação adicional. Isso é útil para casos de uso 2
    - Os clientes estão fazendo a transição do provedor de autenticação adicional de um para outro. Dessa maneira como eles usuários integrados a um provedor de autenticação mais recente eles podem usar grupos para controlar quais autenticação adicional que o provedor é chamado.
    - Os clientes têm necessidades de um provedor de autenticação adicional específica (por exemplo, o certificado) para determinados aplicativos. 
- **Restringir a autenticação de dispositivo TLS com base somente para aplicativos que precisam dele** -os clientes agora podem restringir o TLS é baseada no autenticações de dispositivo para somente os aplicativos que executam o dispositivo de acesso condicional baseado em cliente. Isso impede que solicitações indesejadas de autenticação de dispositivo (ou falhas se o aplicativo cliente não puder manipular) para aplicativos que não exigem a autenticação do TLS com base em dispositivo.
- **Suporte à atualização MFA** -AD FS agora oferece suporte à capacidade para fazer novamente o 2º credencial fator com base na atualização da credencial 2º fator. Isso permite que os clientes façam uma transação inicial com 2 fatores e somente o prompt para o 2º fator em intervalos periódicos. Isso só está disponível para aplicativos que podem fornecer um parâmetro adicional na solicitação e não não uma definição configurável no ADFS. Esse parâmetro tem suporte pelo Azure AD quando "Lembrar minha MFA para X dias" está configurado e o sinalizador 'supportsMFA' está definido como true nas configurações de confiança de domínio federado no Azure AD. 

### <a name="sign-in-sso-improvements"></a>Melhorias na conexão SSO
Foram feitas as seguintes melhorias de SSO entrar no AD FS 2019:

- [Paginado UX com tema centralizado](../operations/AD-FS-paginated-sign-in.md) -ADFS agora foi movido para um fluxo UX paginado que permite que o ADFS validar e fornecer uma experiência de entrada mais mais suave. Agora, o ADFS usa uma interface do usuário centralizada (em vez do lado direito da tela). Você pode exigir mais recentes imagens de logotipo e o plano de fundo para alinhar com essa experiência. Isso também espelha a funcionalidade oferecida no Azure AD.
- **Correção de bug: Estado persistente do SSO para dispositivos do Win10 ao fazer a autenticação PRT** isso soluciona um problema em que o estado MFA não ter persistido ao usar a autenticação de PRT para dispositivos Windows 10. O resultado do problema era que os usuários finais seria solicitado a fornecer 2º credencial de multifator (MFA) com frequência. A correção também torna a experiência consistente quando a autenticação de dispositivo é executada com êxito por meio do cliente TLS e por meio do mecanismo PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Suporte para a criação de aplicativos de linha de negócios modernos
Foi adicionado o suporte a seguir para criar aplicativos LOB modernos para o AD FS 2019:

 - **Fluxo de OAuth dispositivo/perfil** -o perfil de fluxo OAuth do dispositivo para executar logons em dispositivos que não têm uma área de superfície de interface do usuário para dar suporte a experiências de logon avançadas do AD FS agora oferece suporte. Isso permite que o usuário concluir a experiência de logon em um dispositivo diferente. Essa funcionalidade é necessária para a experiência da CLI do Azure no Azure Stack e pode ser usada em outros casos. 
 - **Remoção do parâmetro 'Resource'** -AD FS agora removeu o requisito para especificar um parâmetro de recurso que está de acordo com as especificações de Oauth atuais. Os clientes agora podem fornecer o identificador da relação de confiança de terceira parte confiável como o parâmetro de escopo além para permissões solicitadas. 
 - **Cabeçalhos CORS nas respostas do AD FS** -os clientes agora podem criar aplicativos de página única que permitem que o cliente bibliotecas do lado JS validar a assinatura do id_token, consultando as chaves de assinatura do documento de descoberta OIDC no AD FS. 
 - **Suporte a PKCE** -AD FS adiciona suporte a PKCE para fornecer um fluxo de código de autenticação seguro dentro do OAuth. Isso adiciona uma camada adicional de segurança para este fluxo para evitar o sequestro do código e substituí-lo a partir de um cliente diferente. 
 - **Correção de bug: Declaração de envio de x5t e kid** -essa é uma correção de bug secundária. O AD FS agora envia adicionalmente a declaração 'kid' para denotar a dica de id da chave para verificar a assinatura. O AD FS apenas enviada anteriormente isso como a declaração 'x5t'.

### <a name="supportability-improvements"></a>Aprimoramentos de capacidade de suporte
Os seguintes aprimoramentos de capacidade de suporte não fazem parte do AD FS 2019:
- **Enviar detalhes do erro para os administradores do AD FS** -permite aos administradores configurar usuários finais para enviar os logs de depuração relacionadas a uma falha na autenticação do usuário final a ser armazenado como um compactado arquivado para fácil consumo. Os administradores também podem configurar uma conexão SMTP para automail o arquivo compactado para uma conta de email de triagem ou automaticamente criar um tíquete com base no email. 

### <a name="deployment-updates"></a>Atualizações da implantação
As seguintes atualizações de implantação agora estão incluídas no AD FS 2019:
- **Comportamento de 2019 de nível de farm** – como com o AD FS 2016, há uma nova versão de nível de comportamento de Farm que é necessário para habilitar a nova funcionalidade discutida acima. Isso permite passar da:
    - 2012 R2-> 2019
    - 2016 -> 2019   

### <a name="saml-updates"></a>Atualizações SAML
É a seguinte atualização SAML do AD FS 2019:
- **Correção de bug: Corrigir bugs em federação agregada** -houve diversas correções de bugs em torno do suporte à Federação agregados (por exemplo, InCommon). As correções foram em torno do seguinte: 
  - Aprimorada a colocação em escala para grandes n º de entidades no documento de metadados de Federação agregados. Anteriormente, isso falhará com o erro "ADMIN0017". 
  - Consulta usando o parâmetro 'ScopeGroupID' por meio do cmdlet Get-AdfsRelyingPartyTrustsGroup PSH. 
  - Tratando condições de erro em torno de entityID duplicado


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Especificação do recurso de estilo de AD do Azure no parâmetro de escopo 
Anteriormente, o AD FS exigia o recurso desejado e o escopo em um parâmetro separado em qualquer solicitação de autenticação. Por exemplo, uma solicitação oauth típica seria abaixo: 7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize?</br> response_type = código de & client_id = claimsxrayclient & resource = urn: microsoft:</br>adfs:claimsxray & escopo = oauth & redirect_uri = https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray / TokenResponse & prompt = login**
 
Com o AD FS no servidor de 2019, você agora pode passar o valor do recurso inserido no parâmetro de escopo. Isso é consistente com como uma pessoa pode fazer autenticação no Azure AD também. 

O parâmetro de escopo agora pode ser organizado como uma lista separada por espaço em que cada entrada é a estrutura como recurso/escopo. Por exemplo  

**< criar uma solicitação de exemplo válido >**
> [!NOTE]
> Apenas um recurso pode ser especificado na solicitação de autenticação. Se mais de um recurso está incluído na solicitação, o AD FS retornará que um erro e a autenticação não terá êxito. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Chave de prova para o suporte do código de câmbio (PKCE) para oAuth 
Clientes públicos OAuth, usando a concessão de código de autorização são suscetíveis ao ataque de interceptação de código de autorização.  O ataque é bem descrito no RFC 7636. Para atenuar esse ataque, AD FS no servidor de 2019 dá suporte a chave de prova para código de câmbio (PKCE) para o fluxo de concessão de código de autorização do OAuth. 
 
Para aproveitar o suporte a PKCE, essa especificação adiciona parâmetros adicionais para as solicitações de Token de acesso e autorização do OAuth 2.0.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. O cliente cria e registra um segredo chamado "code_verifier" e é derivada de uma versão transformada "t(code_verifier)" (conhecido como "ao code_challenge"), que é enviada no OAuth 2.0 solicitação de autorização, juntamente com o método de transformação "t_m". 

B. O ponto de extremidade de autorização responde como de costume, mas t(code_verifier)"registros" e o método de transformação. 

C. O cliente, em seguida, envia o código de autorização no Token solicitar acesso como de costume, mas inclui o segredo "code_verifier" gerado no (A). 

D. O AD FS transforma "code_verifier" e o compara com "t(code_verifier)" de (B).  Acesso negado se não forem iguais. 

#### <a name="faq"></a>Perguntas frequentes 
**P.** Pode passar o valor do recurso como parte do valor de escopo, semelhante a como as solicitações são executadas no Azure AD? 
</br>**A.** Com o AD FS no servidor de 2019, você agora pode passar o valor do recurso inserido no parâmetro de escopo. O parâmetro de escopo agora pode ser organizado como uma lista separada por espaço em que cada entrada é a estrutura como recurso/escopo. Por exemplo  
**< criar uma solicitação de exemplo válido >**

**P.** O AD FS dá suporte à extensão PKCE?
</br>**A.** AD FS no servidor de 2019 dá suporte a chave de prova para código de câmbio (PKCE) para o fluxo de concessão de código de autorização do OAuth 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novidades nos Serviços de Federação do Active Directory (AD FS) para Windows Server 2016   
Se você estiver procurando informações sobre versões anteriores do AD FS, consulte os seguintes artigos:  
 [ADFS no Windows Server 2012 ou 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) e [do AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Serviços de Federação do Active Directory fornece controle de acesso e de logon único em uma ampla variedade de aplicativos, incluindo o Office 365, a nuvem com base em aplicativos SaaS e aplicativos na rede corporativa.  
* Para a organização de TI, ele permite que você fornecer o logon no e controle de acesso a aplicativos herdados e modernos, locais e na nuvem, com base no mesmo conjunto de credenciais e as políticas.    
* Para o usuário, ele fornece logon contínuo usando as credenciais de conta do mesmo, familiar.  
* Para o desenvolvedor, ele fornece uma maneira fácil de autenticar os usuários cujas identidades estão no diretório organizacional para que você possa concentrar seus esforços em seu aplicativo, não a autenticação ou identidade.  

Este artigo descreve o que há de novo no AD FS no Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Eliminar senhas da Extranet  
Comprometem o AD FS 2016 permite três novas opções para o logon sem senhas, permitindo que as organizações evitar o risco de rede de captura, as senhas perdidas ou roubadas. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Entrar com autenticação multifator do Azure
AD FS 2016 tem como base a autenticação multifator recursos (MFA) do AD FS no Windows Server 2012 R2, permitindo que o logon usando apenas um código de MFA do Azure, sem primeiro se digitar um nome de usuário e senha.

* Com o Azure MFA como método de autenticação primária, o usuário é solicitado para o nome de usuário e o código OTP do aplicativo Azure Authenticator.  
* Com o Azure MFA como método de autenticação adicional ou secundário, o usuário fornece autenticação primária credenciais (usando a autenticação integrada do Windows, nome de usuário e senha, cartão inteligente ou certificado de usuário ou dispositivo), em seguida, vê uma solicitação para texto, voz ou OTP com base em logon de MFA do Azure.  
* Com o novo adaptador de MFA do Azure interno, instalação e configuração para o Azure MFA com AD FS nunca foi tão simples.
* As organizações podem tirar proveito da MFA do Azure sem a necessidade de um servidor Azure MFA local.
* O MFA do Azure pode ser configurado para a intranet ou extranet ou como parte de qualquer política de controle de acesso.

Para obter mais informações sobre o Azure MFA com AD FS
*  [Configurar o AD FS 2016 e MFA do Azure](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Acesso sem senha em dispositivos compatíveis
AD FS 2016 se baseia nos recursos de registro de dispositivo anterior para habilitar o logon e controle de acesso com base em status de conformidade do dispositivo. Os usuários podem entrar no usando a credencial de dispositivo e a conformidade é avaliada novamente quando os atributos de dispositivo são alterados, para que você sempre pode garantir que políticas estão sendo aplicadas.  Isso permite que as políticas, como

* Habilitar o acesso somente de dispositivos que são gerenciados e/ou em conformidade  
* Habilitar o acesso à Extranet somente de dispositivos que são gerenciados e/ou em conformidade  
* Exigir a autenticação multifator para os computadores que não são gerenciados ou não está em conformidade  

O AD FS fornece o componente local diante das políticas de acesso condicional em um cenário híbrido. Ao registrar dispositivos com o Azure AD para acesso condicional aos recursos de nuvem, a identidade do dispositivo pode ser usada para políticas de AD FS, bem.

![o que há de novo](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Para obter mais informações sobre como usar o dispositivo com base em acesso condicional na nuvem   
 *  [Acesso condicional do Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Para obter mais informações sobre como usar o dispositivo com base em acesso condicional com o AD FS
*  [Planejamento para o dispositivo com base no acesso condicional com o AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Políticas de controle de acesso no AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Entrar com o Windows Hello para empresas   
Dispositivos Windows 10 introduzem o Windows Hello e o Windows Hello para empresas, substituindo as senhas de usuário com credenciais de usuário de associação de dispositivo forte protegidas por gesto do usuário (um PIN, um gesto biométrico, como reconhecimento facial ou impressão digital). AD FS 2016 dá suporte a esses novos recursos do Windows 10 para que os usuários podem entrar aplicativos do AD FS da intranet ou extranet sem a necessidade de fornecer uma senha.

Para obter mais informações sobre como usar o Microsoft Windows Hello para empresas em sua organização
*  [Habilitar Windows Hello para empresas em sua organização](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Acesso seguro a aplicativos

### <a name="modern-authentication"></a>Autenticação moderna
AD FS 2016 dá suporte a protocolos modernos mais recentes que fornecem uma melhor experiência de usuário para Windows 10, bem como o iOS mais recente e dispositivos Android e aplicativos.  

Para obter mais informações, consulte [cenários do AD FS para desenvolvedores](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurar políticas de controle de acesso sem precisar saber a linguagem de regras de declaração  
Anteriormente, os administradores do AD FS precisava configurar políticas usando a linguagem de regra de declaração do AD FS, tornando difícil de configurar e manter políticas. Com as políticas de controle de acesso, os administradores podem usar modelos incorporados para aplicar políticas comuns, como
* Permitir o acesso somente à intranet
* Permitir todos e exigir MFA de Extranet
* Permitir todos e exigir MFA de um grupo específico

Os modelos são fáceis de personalizar usando um assistente controlado por processo para adicionar exceções ou regras de políticas adicionais e podem ser aplicados a um ou muitos aplicativos para a imposição de política consistente.

Para obter mais informações, consulte [políticas de controle de acesso no AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Habilitar o logon com diretórios LDAP não AD  
Muitas organizações têm uma combinação de Active Directory e diretórios de terceiros. Com a adição de suporte do AD FS para autenticar usuários armazenados em diretórios LDAP v3 compatível, o AD FS agora pode ser usado para:
* Usuários no terceiro, diretórios do LDAP v3 em conformidade
* Usuários em florestas do Active Directory ao qual uma relação de confiança bidirecional do Active Directory não está configurada
* Usuários no Active Directory Lightweight Directory Services (AD LDS)

Para obter mais informações, consulte [Configure AD FS para autenticar usuários armazenados em diretórios LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Melhor experiência de entrada
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalizar a experiência de aplicativos do AD FS de entrada  
Ouvimos você que a capacidade de personalizar a experiência de logon para cada aplicativo seria uma melhoria de usabilidade grande, especialmente para organizações que fornecem entrada sobre para aplicativos que representam várias empresas diferentes ou as marcas.  

Anteriormente, o AD FS no Windows Server 2012 R2 fornecidas uma entrada comum na experiência para todos os aplicativos de terceira parte confiável, com a capacidade de personalizar um subconjunto de texto com base em conteúdo por aplicativo. Com o Windows Server 2016, você pode personalizar não só as mensagens, mas imagens, como web e o logotipo de tema por aplicativo. Além disso, você pode criar novos, temas da web personalizado e aplicá-las por terceira parte confiável terceiros.  

Para obter mais informações, consulte [personalização de entrada do usuário de AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Capacidade de gerenciamento e operacionais aprimoramentos  
A seção a seguir descreve os cenários de operacionais aprimorados que são apresentados com os serviços de Federação do Active Directory no Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Simplificada de auditoria para facilitar o gerenciamento administrativo  
No AD FS para o Windows Server 2012 R2 lá foram vários eventos de auditoria gerados por uma única solicitação e as informações relevantes sobre um log-in ou atividade de emissão de token está ausente (em algumas versões do AD FS) ou espalhada em vários eventos de auditoria. Por padrão, o AD FS eventos de auditoria são desativados devido à sua natureza detalhada.  
Com o lançamento do AD FS 2016, o auditoria tornou mais simples e menos detalhada.  

Para obter mais informações, consulte [aprimoramentos de auditoria para o AD FS no Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Melhor interoperabilidade com SAML 2.0 para participação em confederações  
AD FS 2016 contém suporte a protocolo SAML adicional, incluindo suporte para importação de relações de confiança com base nos metadados que contém várias entidades. Isso permite que você configurar o AD FS para participar confederações como Federation InCommon e outras implementações em conformidade com o eGov 2.0 padrão.  

Para obter mais informações, consulte [melhor interoperabilidade com SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gerenciamento simplificado de senha para federado os usuários do O365  
Você pode configurar os serviços de Federação do Active Directory (AD FS) para enviar solicitações de expiração de senha para a terceira parte (aplicativos) que é protegidas pelo AD FS. Como essas declarações são usadas depende do aplicativo. Por exemplo, com o Office 365, como a terceira, as atualizações foram implementadas ao Exchange e Outlook para notificar os usuários federados das suas senhas estar expirado em breve.  

Para obter mais informações, consulte [Configure AD FS para enviar solicitações de expiração de senha.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Mover do AD FS no Windows Server 2012 R2 para o AD FS no Windows Server 2016 é mais fácil  
Anteriormente, migrando para uma nova versão do AD FS exigia exportando a configuração do farm antigo e importar para um farm totalmente novo, paralelo.  

Agora, mudando do AD FS no Windows Server 2012 R2 para o AD FS no Windows Server 2016 tornou-se muito mais fácil. Basta adicionar um novo servidor do Windows Server 2016 para um farm do Windows Server 2012 R2 e o farm atuará no nível de comportamento de farm do Windows Server 2012 R2, para que ele se parece e se comporta exatamente como um farm do Windows Server 2012 R2.  

Em seguida, adicionar novos servidores do Windows Server 2016 ao farm, verifique se a funcionalidade e remova os servidores mais antigos do balanceador de carga. Depois que todos os nós do farm estão executando o Windows Server 2016, você está pronto para atualizar o nível de comportamento de farm para 2016 e começar a usar os novos recursos.  

Para obter mais informações, consulte [atualização para o AD FS no Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
