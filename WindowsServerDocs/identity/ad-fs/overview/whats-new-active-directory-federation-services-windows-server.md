---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novidades nos Serviços de Federação do Active Directory (AD FS) para Windows Server 2016
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/22/2020
ms.topic: article
ms.openlocfilehash: a0407639f6c3efcbdc8f0353b0313101272351ac
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938138"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Novidades nos Serviços de Federação do Active Directory (AD FS)


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Novidades nos Serviços de Federação do Active Directory (AD FS) para Windows Server 2019

### <a name="protected-logins"></a>Logons protegidos

Veja abaixo um breve resumo das atualizações para logons protegidos disponíveis no AD FS 2019:
- **Provedores de autenticação externa como primários** – os clientes agora podem usar produtos de autenticação de terceiros como o primeiro fator e não expor senhas como o primeiro fator. Nos casos em que um provedor de autenticação externa pode provar dois fatores, ele pode declarar a MFA.
- **Autenticação de senha como autenticação adicional** – os clientes têm uma opção de caixa de entrada totalmente compatível para usar a senha somente para o fator adicional após uma opção sem senha ser usada como o primeiro fator. Isso aprimora a experiência dos clientes do ADFS 2016, visto que eles tinham que baixar um adaptador do GitHub compatível no estado em que se encontra.
- **Módulo de avaliação de risco conectável** – os clientes agora podem criar módulos de plug-in próprios para bloquear determinados tipos de solicitações durante a fase de pré-autenticação. Para os clientes, isso facilita o uso de inteligência de nuvem, como a proteção de identidade para bloquear logons de usuários ou transações suspeitas.  Para mais informações, confira [ Criar plug-ins com o Modelo de Avaliação de Risco do AD FS 2019](../../ad-fs/development/ad-fs-risk-assessment-model.md)
- **Aprimoramentos de ESL** – aprimora o QFE ESL em 2016 adicionando as seguintes funcionalidades
    - Permite que os clientes estejam no modo de auditoria enquanto estão protegidos pela funcionalidade de bloqueio de extranet “clássica” disponível desde o 2012R2 do ADFS. Atualmente, os clientes 2016 não têm proteção no modo de auditoria.
    - Habilita o limite de bloqueio independente para locais familiares. Isso possibilita que várias instâncias de aplicativos em execução com uma conta de serviço comum sobreponham senhas com menos impacto.


### <a name="additional-security-improvements"></a>Aprimoramentos de segurança adicionais

Os seguintes aprimoramentos de segurança adicionais estão disponíveis no AD FS 2019:
- **PSH (PowerShell) remoto usando o logon com Cartão Inteligente** – os clientes agora podem usar cartões inteligentes para se conectar remotamente ao ADFS por meio do PSH e usá-lo para gerenciar todas as funções do PSH, incluindo os cmdlets do PSH de vários nós.
- **Personalização de cabeçalho HTTP** – os clientes agora podem personalizar os cabeçalhos HTTP emitidos durante as respostas do ADFS. Isso inclui os seguintes cabeçalhos
     - HSTS: ele faz com que os pontos de extremidade do ADFS só possam ser usados em pontos de extremidade HTTPS para que um navegador em conformidade se aplique
     - x-frame-options: possibilita que os administradores do ADFS permitam que partes confiáveis específicas insiram iFrames em páginas de logon interativo do ADFS. Ele deve ser usado com cuidado e somente em hosts HTTPS.
     - Cabeçalho futuro: outros cabeçalhos futuros também podem ser configurados.

Para obter mais informações, confira [Personalizar cabeçalhos de resposta de segurança HTTP com o AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md)


### <a name="authenticationpolicy-capabilities"></a>Funcionalidades de autenticação/política

As seguintes funcionalidades de autenticação/política estão inclusas no AD FS 2019:
- **Especificar o método de autenticação para autenticação adicional por RP** – os clientes agora podem usar regras de declarações para decidir qual provedor de autenticação adicional invocar para o provedor de autenticação adicional. Isso é útil para dois casos de uso
    - Os clientes estão fazendo a transição de um provedor de autenticação adicional para outro. Dessa forma, à medida que eles integram usuários a um provedor de autenticação mais recente, eles podem usar grupos para controlar qual provedor de autenticação adicional é chamado.
    - Os clientes precisam de um provedor de autenticação adicional específico (por exemplo, certificado) para determinados aplicativos.
- **Restringir a autenticação de dispositivo baseada em TLS somente a aplicativos que fazem essa exigência** – os clientes agora podem restringir autenticações de dispositivo baseadas em TLS de cliente somente a aplicativos que executam acesso condicional com base no dispositivo. Isso impede qualquer prompt indesejado na autenticação do dispositivo (ou falhas se o aplicativo cliente não puder tratar) para aplicativos que não exigem a autenticação de dispositivo baseada em TLS.
- **Suporte à atualização de MFA** – o AD FS agora é compatível com a capacidade de refazer a credencial de segundo fator com base na atualização da credencial de segundo fator. Isso permite que os clientes façam uma transação inicial com dois fatores e solicitem apenas o segundo fator em uma base periódica. Isso só está disponível para aplicativos que podem fornecer um parâmetro adicional na solicitação e não é uma definição configurável no ADFS. Esse parâmetro é compatível com o Azure AD quando "Lembrar minha MFA por X dias" está configurado e o sinalizador "supportsMFA" é definido como true nas configurações de confiança do domínio federado no Azure AD.


### <a name="sign-in-sso-improvements"></a>Aprimoramentos de SSO de entrada

Os seguintes aprimoramentos de SSO de entrada foram feitos no AD FS 2019:

- [Experiência do usuário paginada com tema centralizado](../operations/AD-FS-paginated-sign-in.md) – o ADFS agora foi movido para um fluxo de experiência do usuário paginada que permite ao ADFS validar e fornecer uma experiência de entrada mais suave. O ADFS agora usa uma interface do usuário centralizada (em vez do lado direito da tela). Você pode exigir que as imagens mais recentes de logotipo e de tela de fundo se alinhem com essa experiência. Isso também espelha a funcionalidade oferecida no Azure AD.
- **Correção de bug: estado de SSO persistente para dispositivos Win10 ao fazer a autenticação de PRT** isso resolve um problema em que o estado de MFA não era persistente ao usar a autenticação de PRT para dispositivos Windows 10. O resultado do problema era que os usuários finais eram solicitados a inserir a credencial de segundo fator (MFA) com frequência. A correção também torna a experiência consistente quando a autenticação do dispositivo é executada com êxito via TLS do cliente e por meio do mecanismo de PRT.


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Suporte para criar aplicativos de linha de negócios modernos

O seguinte suporte para a criação de aplicativos LOB modernos foi adicionado ao AD FS 2019:

- **Fluxo/perfil do dispositivo OAuth** – o AD FS agora é compatível com o perfil de fluxo do dispositivo OAuth para executar logons em dispositivos que não têm uma área de superfície da interface do usuário para dar suporte a experiências de logon avançadas. Isso permite que o usuário conclua a experiência de logon em um dispositivo diferente. Essa funcionalidade é necessária para a experiência de CLI do Azure no Azure Stack e pode ser usada em outros casos.
- **Remoção do parâmetro “Resource”** – o AD FS removeu o requisito para especificar um parâmetro de recurso que está em linha com as especificações OAuth atuais. Agora, os clientes podem fornecer o identificador do objeto de confiança de terceira parte confiável como o parâmetro de escopo, além das permissões solicitadas.
- **Cabeçalhos CORS em respostas do AD FS** – os clientes agora podem criar aplicativos de página única que permitem que bibliotecas JS do lado do cliente validem a assinatura do id_token consultando as chaves de assinatura do documento de descoberta OIDC no AD FS.
- **Suporte à PKCE** – o AD FS adiciona suporte à PKCE para fornecer um fluxo de código de autenticação seguro dentro do OAuth. Isso adiciona uma camada extra de segurança a esse fluxo para evitar sequestrar o código e reproduzi-lo de um cliente diferente.
- **Correção de bug: enviar declaração x5t e “kid”** – essa é uma pequena correção de bug. Agora, o AD FS também envia a declaração “kid” para indicar a dica de ID de chave para verificar a assinatura. Anteriormente, o AD FS fazia o envio apenas como declaração “x5t”.


### <a name="supportability-improvements"></a>Aprimoramentos de capacidade de suporte

Os seguintes aprimoramentos de suporte não fazem parte do AD FS 2019:
- **Enviar detalhes do erro para os administradores do AD FS** – permite que os administradores configurem os usuários finais para enviar logs de depuração relacionados a uma falha na autenticação do usuário final a ser armazenada como um arquivo compactado para facilitar o consumo. Os administradores também podem configurar uma conexão SMTP para enviar por email automaticamente o arquivo compactado para uma conta de email de triagem ou para criar automaticamente um tíquete com base no email.


### <a name="deployment-updates"></a>Atualizações de implantação

As seguintes atualizações de implantação foram incluídas no AD FS 2019:
- **Nível de comportamento de farm 2019** – como com o AD FS 2016, há uma nova versão de nível de comportamento de farm necessária para habilitar a nova funcionalidade discutida acima. Isso permite fazer as seguintes atualizações:
    - 2012 R2-> 2019
    - 2016 -> 2019


### <a name="saml-updates"></a>Atualizações SAML

A seguinte atualização SAML está inclusa no AD FS 2019:
- **Correção de bug: correção de bugs em federação agregada** – foram feitas inúmeras correções de bugs em relação ao suporte de federação agregada (por exemplo, InCommon). As correções estão relacionadas ao seguinte:
  - Dimensionamento aprimorado para grandes quantidades de entidades no documento de metadados de federação agregada. Anteriormente, isso retornaria uma falha com o erro "ADMIN0017".
  - Consulta usando o parâmetro 'ScopeGroupID' por meio do cmdlet do PSH (PowerShell) Get-AdfsRelyingPartyTrustsGroup.
  - Manipulação de condições de erro em relação à entityID duplicada


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Especificação de recurso de estilo do Azure AD no parâmetro de escopo

Anteriormente, o AD FS exigia que o recurso e o escopo desejados estivessem em um parâmetro separado em qualquer solicitação de autenticação. Por exemplo, uma solicitação OAuth típica teria a seguinte aparência: 7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize?</br>response_type=code&client_id=claimsxrayclient&resource=urn:microsoft:</br>adfs:claimsxray&scope=oauth&redirect_uri=https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray/TokenResponse&prompt=login**

Com o AD FS no Server 2019, agora você pode transmitir o valor do recurso inserido no parâmetro de escopo. Isso também é consistente com a maneira como uma pessoa pode fazer a autenticação no Azure AD.

O parâmetro de escopo agora pode ser organizado como uma lista separada por espaço, em que cada entrada é estruturada como recurso/escopo.

> [!NOTE]
> Somente um recurso pode ser especificado na solicitação de autenticação. Se mais de um recurso for incluído na solicitação, o AD FS retornará um erro e a autenticação não terá êxito.


### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Suporte de PKCE (Chave de Prova para Troca de Código) para OAuth

Clientes públicos OAuth que usam a concessão de código de autorização são suscetíveis a ataques de interceptação de código de autorização.  O ataque é bem descrito na RFC 7636. Para mitigar esse risco, o AD FS no Server 2019 é compatível com a PKCE (Chave de Prova para Troca de Código) para o fluxo de concessão de código de autorização OAuth.

Para aproveitar o suporte da PKCE, essa especificação adiciona parâmetros extras à autorização do OAuth 2.0 e às solicitações de token de acesso.

![Chave de prova](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. O cliente cria e registra um segredo chamado "code_verifier" e deriva uma versão transformada "t (code_verifier)" (conhecida como "code_challenge"), que é enviada na solicitação de autorização do OAuth 2.0 juntamente com o método de transformação "t_m".

B. O ponto de extremidade da autorização responde normalmente, mas registra "t(code_verifier)" e o método de transformação.

C. Em seguida, o cliente envia o código de autorização na solicitação de token de acesso como de costume, mas inclui o segredo "code_verifier" gerado em (A).

D. O AD FS transforma o "code_verifier" e compara-o com "t (code_verifier)" de (B).  O acesso será negado se eles não forem iguais.


#### <a name="faq"></a>Perguntas frequentes

> [!NOTE]
> Você poderá receber esse erro nos logs de eventos de administrador do ADFS: Solicitação OAuth inválida recebida. O 'NAME' do cliente é proibido de acessar o recurso com o escopo 'ugs'.
> Para corrigir esse erro:
> 1. Inicie o console de gerenciamento do AD FS. Navegue até "Serviços > Descrições de Escopo"
> 2. Clique com o botão direito do mouse em "Descrições de Escopo" e selecione "Adicionar Descrição de Escopo"
> 3. No nome, digite "ugs" e clique em Aplicar > OK
> 4. Inicie o PowerShell como Administrador
> 5. Execute o comando "Get-AdfsApplicationPermission". Procure os ScopeNames: {openid, aza} que tenham o ClientRoleIdentifier. Anote o ObjectIdentifier.
> 6. Execute o comando "Set-AdfsApplicationPermission -TargetIdentifier <ObjectIdentifier da etapa 5> -AddScope 'ugs'
> 7. Reinicie o serviço ADFS.
> 8. No cliente: Reinicie o cliente. O usuário deverá ser solicitado a provisionar o WHFB.
> 9. Se a janela de provisionamento não for exibida, será necessário coletar logs de rastreamento do NGC e solucionar problemas adicionais.

**P.:** Posso transmitir o valor do recurso como parte do valor do escopo assim como as solicitações são feitas no Azure AD?</br>
**R.:** Com o AD FS no Server 2019, agora você pode transmitir o valor do recurso inserido no parâmetro de escopo. O parâmetro de escopo agora pode ser organizado como uma lista separada por espaço, em que cada entrada é estruturada como recurso/escopo. Por exemplo, **<criar uma solicitação de exemplo válida>**

**P.:** O AD FS dá suporte à extensão da PKCE?</br>
**R.:** O AD FS no Server 2019 dá suporte à PKCE (Chave de Prova para Troca de Código) para o fluxo de concessão de código de autorização OAuth


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novidades nos Serviços de Federação do Active Directory (AD FS) para Windows Server 2016

Se você estiver procurando informações sobre versões anteriores do AD FS, confira os seguintes artigos: [ADFS no Windows Server 2012 ou 2012 R2](../../active-directory-federation-services.md) e [AD FS 2.0](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd727958(v=ws.10))

 O AD FS (Serviços de Federação do Active Directory) fornece controle de acesso e logon único em uma ampla variedade de aplicativos, incluindo Office 365, aplicativos SaaS baseados em nuvem e aplicativos na rede corporativa.
* Para a organização de TI, ele permite que você forneça logon e controle de acesso para os aplicativos modernos e herdados, no local e na nuvem, com base no mesmo conjunto de credenciais e políticas.
* Para o usuário, ele fornece logon contínuo usando as mesmas credenciais de conta conhecidas.
* Para o desenvolvedor, ele fornece uma maneira fácil de autenticar usuários cujas identidades residem no diretório organizacional para que você possa concentrar seus esforços em seu aplicativo, não na autenticação ou na identidade.

Este artigo descreve o que há de novo no AD FS no Windows Server 2016 (AD FS 2016).


## <a name="eliminate-passwords-from-the-extranet"></a>Eliminação de senhas da extranet

O AD FS 2016 permite três novas opções de logon sem senhas, permitindo que as organizações evitem o risco de comprometimento da rede contra senhas roubadas, vazadas ou que sofreram phishing.


### <a name="sign-in-with-azure-multi-factor-authentication"></a>Entrada com a Autenticação Multifator do Azure

O AD FS 2016 baseia-se nos recursos de MFA (autenticação multifator) do AD FS no Windows Server 2012 R2, permitindo o logon usando apenas um código de MFA do Azure, sem primeiro inserir um nome de usuário e uma senha.

* Com a MFA do Azure como o método de autenticação primário, o usuário é solicitado a fornecer seu nome de usuário e o código OTP do aplicativo Microsoft Authenticator.
* Com a MFA do Azure como o método de autenticação secundário ou adicional, o usuário fornece as credenciais de autenticação primária (usando a autenticação integrada do Windows, o nome de usuário e a senha, o cartão inteligente ou o certificado de dispositivo ou de usuários) e, em seguida, vê um prompt de texto, voz ou logon da MFA do Azure baseado em OTP.
* Com o novo adaptador da MFA do Azure interno, a instalação e a configuração da MFA do Azure com o AD FS nunca estiveram mais simples.
* As organizações podem aproveitar a MFA do Azure sem a necessidade de um Servidor da MFA do Azure local.
* A MFA do Azure pode ser configurada para intranet, extranet ou como parte de uma política de controle de acesso.

Para obter mais informações sobre a MFA do Azure com o AD FS
*  [Configurar o AD FS 2016 e o MFA do Azure](../operations/configure-ad-fs-and-azure-mfa.md)


### <a name="password-less-access-from-compliant-devices"></a>Acesso sem senha de dispositivos em conformidade

O AD FS 2016 baseia-se nas funcionalidades anteriores de registro do dispositivo para habilitar o logon e o controle de acesso com base no status de conformidade do dispositivo. Os usuários podem fazer logon usando a credencial do dispositivo e a conformidade é reavaliada quando os atributos do dispositivo são alterados, para que você sempre possa garantir que as políticas estejam sendo impostas.  Isso permite políticas como

* Habilitar o acesso somente de dispositivos gerenciados e/ou em conformidade
* Habilitar o acesso de extranet somente de dispositivos gerenciados e/ou em conformidade
* Exigir autenticação multifator para computadores que não são gerenciados ou não estão em conformidade

O AD FS fornece o componente local das políticas de acesso condicional em um cenário híbrido. Quando você registra dispositivos com o Azure AD para acesso condicional a recursos de nuvem, a identidade do dispositivo também pode ser usada para políticas do AD FS.

![Novidades](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)

 Para obter mais informações sobre como usar o acesso condicional baseado em dispositivo na nuvem
 * [Acesso condicional do Azure Active Directory](/azure/active-directory/conditional-access/overview)

Para obter mais informações sobre como usar o acesso condicional baseado em dispositivo com o AD FS
* [Como planejar o acesso condicional baseado em dispositivo com o AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)
* [Políticas de controle de acesso no AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)


### <a name="sign-in-with-windows-hello-for-business"></a>Entrada com o Windows Hello para Empresas

> [!NOTE]
> Atualmente, navegadores de projeto de software livre, como o Google Chrome e o [novo Microsoft Edge criado no Chromium](https://www.microsoft.com/edge?form=MB110A&OCID=MB110A), não são compatíveis com SSO (logon único) baseado em navegador com o Microsoft Windows Hello para Empresas. Use o Internet Explorer ou uma versão mais antiga do Microsoft Edge.

Os dispositivos Windows 10 usam o Windows Hello e o Windows Hello para Empresas, substituindo senhas de usuário por credenciais de usuário vinculadas a dispositivos fortes protegidas pelo gesto de um usuário (um PIN, um gesto biométrico como impressão digital ou reconhecimento do rosto). O AD FS 2016 é compatível com essas novas funcionalidades do Windows 10 para que os usuários possam entrar nos aplicativos do AD FS da intranet ou da extranet sem a necessidade de fornecer uma senha.

Para obter mais informações sobre como usar o Microsoft Windows Hello para Empresas na sua organização
* [Habilitar o Windows Hello para Empresas na organização](/windows/security/identity-protection/hello-for-business/hello-identity-verification)


## <a name="secure-access-to-applications"></a>Acesso seguro aos aplicativos

### <a name="modern-authentication"></a>Autenticação moderna

O AD FS 2016 é compatível com os mais recentes protocolos modernos que fornecem uma experiência aprimorada de usuário para o Windows 10, bem como os dispositivos e aplicativos iOS e Android mais recentes.

Para obter mais informações, confira [Cenários de AD FS para desenvolvedores](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurar políticas de controle de acesso sem a necessidade de conhecer a linguagem das regras de declaração

Anteriormente, os administradores do AD FS tinham que configurar políticas usando a linguagem de regra de declaração do AD FS, dificultando a configuração e a manutenção de políticas. Com as políticas de controle de acesso, os administradores podem usar modelos internos para aplicar políticas comuns, como
* Permitir somente acesso à intranet
* Permitir todos os usuários e exigir MFA da extranet
* Permitir todos os usuários e exigir MFA de um grupo específico

Os modelos são fáceis de personalizar usando um processo controlado por assistente para adicionar exceções ou regras de política adicionais e podem ser aplicados a um ou vários aplicativos para imposição de política consistente.

Para obter mais informações, confira [Políticas de controle de acesso no AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)


### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Habilitar logon com diretórios LDAP não AD

Muitas organizações têm uma combinação de Active Directory e diretórios de terceiros. Com a adição de suporte do AD FS para autenticar usuários armazenados em diretórios em conformidade com LDAP v3, o AD FS agora pode ser usado por:
* Usuários em diretórios de terceiros em conformidade com LDAP v3
* Usuários em florestas do Active Directory às quais uma relação de confiança bidirecional do Active Directory não está configurada
* Usuários no AD LDS (Active Directory Lightweight Directory Services)

Para obter mais informações, confira [Configurar o AD FS para autenticar usuários armazenados em diretórios LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)


## <a name="better-sign-in-experience"></a>Experiência de entrada aprimorada

### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalizar a experiência de entrada para aplicativos AD FS

Ouvimos que a capacidade de personalizar a experiência de logon de cada aplicativo seria um excelente aprimoramento de usabilidade, especialmente para organizações que fornecem logon para aplicativos que representam várias empresas ou marcas diferentes.

Anteriormente, o AD FS no Windows Server 2012 R2 fornecia uma experiência comum de logon para todos os aplicativos de terceira parte confiável, com a capacidade de personalizar um subconjunto de conteúdo baseado em texto por aplicativo. Com o Windows Server 2016, você pode personalizar não apenas as mensagens, mas as imagens, o logotipo e o tema da Web por aplicativo. Além disso, você pode criar temas da Web novos e personalizados e aplicá-los de acordo com a terceira parte confiável.

Para obter mais informações, confira [Personalização de entrada do usuário do AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)


## <a name="manageability-and-operational-enhancements"></a>Aprimoramentos operacionais e de capacidade de gerenciamento

A seção a seguir descreve os cenários operacionais aprimorados que são introduzidos com o AD FS (Serviços de Federação do Active Directory) no Windows Server 2016.


### <a name="streamlined-auditing-for-easier-administrative-management"></a>Auditoria simplificada para facilitar o gerenciamento administrativo

No AD FS do Windows Server 2012 R2, havia vários eventos de auditoria gerados para uma solicitação e as informações relevantes sobre uma atividade de emissão de logon ou de token estão ausentes (em algumas versões do AD FS) ou espalhadas por vários eventos de auditoria. Por padrão, os eventos de auditoria do AD FS são desativados devido à sua natureza detalhada.
Com o lançamento do AD FS 2016, a auditoria tornou-se mais simplificada e menos detalhada.

Para mais informações, confira [Aprimoramentos de auditoria para o AD FS no Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)


### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Interoperabilidade aprimorada com SAML 2.0 para participação em confederações

O AD FS 2016 contém suporte a protocolo SAML adicional, incluindo suporte para importar relações de confiança com base em metadados que contêm várias entidades. Isso permite que você configure o AD FS para participar de confederações, como Federação InCommon, bem como de outras implementações em conformidade com o padrão eGov 2.0.

Para obter mais informações, confira [Interoperabilidade aprimorada com o SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)


### <a name="simplified-password-management-for-federated-o365-users"></a>Gerenciamento simplificado de senhas para usuários federados do O365

Você pode configurar o AD FS (Serviços de Federação do Active Directory) para enviar declarações de expiração de senha para os aplicativos de confiança de terceira parte confiável que são protegidos pelo AD FS. A forma como essas declarações são usadas depende do aplicativo. Por exemplo, com o Office 365 como sua terceira parte confiável, as atualizações foram implementadas no Exchange e no Outlook para notificar os usuários federados sobre suas senhas que expirarão em breve.

Para mais informações, confira [Configurar o AD FS para enviar declarações de expiração de senha.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)


### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>É mais fácil mover do AD FS no Windows Server 2012 R2 para o AD FS no Windows Server 2016

Anteriormente, a migração para uma nova versão do AD FS exigia a exportação da configuração do antigo farm e a importação para um farm novo e paralelo.

Agora, a mudança do AD FS no Windows Server 2012 R2 para o AD FS no Windows Server 2016 tornou-se muito mais fácil. Basta adicionar um novo servidor do Windows Server 2016 a um farm do Windows Server 2012 R2 e o farm atuará no nível de comportamento do farm do Windows Server 2012 R2 e, portanto, ele terá a aparência e se comportará exatamente como um farm do Windows Server 2012 R2.

Em seguida, adicione novos servidores do Windows Server 2016 ao farm, verifique a funcionalidade e remova os servidores mais antigos do balanceador de carga. Depois que todos os nós do farm estiverem executando o Windows Server 2016, você estará pronto para atualizar o nível de comportamento do farm para 2016 e começar a usar os novos recursos.

Para obter mais informações, confira [Como atualizar para o AD FS no Windows Server 2016.](../deployment/upgrading-to-ad-fs-in-windows-server.md)
