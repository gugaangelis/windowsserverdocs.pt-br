---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novidades nos Serviços de Federação do Active Directory (AD FS) para Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 01/22/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: adce37d8d06399d3a00221a12f3449244720ade7
ms.sourcegitcommit: 840d1d8851f68936db3934c80796fb8722d3c64a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519478"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Novidades nos Serviços de Federação do Active Directory (AD FS)


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>O que há de novo no Serviços de Federação do Active Directory (AD FS) para o Windows Server 2019

### <a name="protected-logins"></a>Logons protegidos
Veja a seguir um breve resumo das atualizações para logons protegidos disponíveis no AD FS 2019:
- **Provedores de autenticação externa como primário** -os clientes agora podem usar produtos de autenticação de terceiros como o primeiro fator e não expor senhas como o primeiro fator. Nos casos em que um provedor de autenticação externa pode provar 2 fatores, ele pode declarar MFA. 
- **Autenticação de senha como autenticação adicional** – os clientes têm uma opção de caixa de entrada totalmente suportada para usar a senha somente para o fator adicional depois que uma opção de menos senha é usada como o primeiro fator. Isso melhora a experiência do cliente do ADFS 2016, em que os clientes tinham que baixar um adaptador do GitHub que tem suporte como está. 
- **Módulo de avaliação de risco conectável** – os clientes agora podem criar seus próprios módulos de plug-in para bloquear determinados tipos de solicitações durante o estágio de pré-autenticação. Isso torna mais fácil para os clientes usarem a inteligência de nuvem, como a proteção de identidade para bloquear logons para usuários arriscados ou transações arriscadas.  Para obter mais informações, consulte [criar plug-ins com o modelo de avaliação de risco AD FS 2019](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **Aprimoramentos do ESL** – melhora o QFE ESL em 2016 adicionando os seguintes recursos
    - Permite que os clientes estejam no modo de auditoria enquanto estão protegidos pela funcionalidade de bloqueio de extranet ' clássica ' disponível desde o 2012R2 do ADFS. Atualmente, os clientes 2016 não teriam proteção no modo de auditoria. 
    - Habilita o limite de bloqueio independente para locais familiares. Isso possibilita que várias instâncias de aplicativos em execução com uma conta de serviço comum sobreponham senhas com a menor quantidade de impacto. 

### <a name="additional-security-improvements"></a>Aprimoramentos de segurança adicionais
Os seguintes aprimoramentos de segurança adicionais estão disponíveis no AD FS 2019:
- **PSH remoto usando logon de cartão inteligente** -os clientes agora podem usar cartões inteligentes para se conectar remotamente ao ADFS por meio do PSH e usá-los para gerenciar todas as funções do PSH incluem cmdlets de PSH de vários nós.
- **Personalização de cabeçalho http** -os clientes agora podem personalizar os cabeçalhos HTTP emitidos durante as respostas do ADFS. Isso inclui os seguintes cabeçalhos
     - HSTS: isso transmite que os pontos de extremidade do ADFS só podem ser usados em pontos de extremidade HTTPS para que um navegador compatível se aplique
     - x-frame – opções: permite que os administradores do ADFS permitam que partes confiáveis específicas insiram iFrames para páginas de logon interativo do ADFS. Isso deve ser usado com cuidado e somente em hosts HTTPS. 
     - Cabeçalho futuro: outros cabeçalhos futuros também podem ser configurados. 

Para obter mais informações, consulte [Personalizar cabeçalhos de resposta de segurança http com o AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>Recursos de autenticação/política
Os seguintes recursos de autenticação/política estão em AD FS 2019:
- **Especificar o método de autenticação para autenticação adicional por RP** -os clientes agora podem usar regras de declarações para decidir qual provedor de autenticação adicional invocar para o provedor de autenticação adicional. Isso é útil para dois casos de uso
    - Os clientes estão fazendo a transição de um provedor de autenticação adicional para outro. Dessa forma, à medida que eles integram usuários a um provedor de autenticação mais novo, eles podem usar grupos para controlar qual provedor de autenticação adicional é chamado.
    - Os clientes precisam de um provedor de autenticação adicional específico (por exemplo, certificado) para determinados aplicativos. 
- **Restringir a autenticação de dispositivo baseada em TLS somente a aplicativos que exigem ti** -os clientes agora podem restringir autenticações de dispositivo baseadas em TLS de cliente a apenas aplicativos que executam acesso condicional com base no dispositivo. Isso impede qualquer prompt indesejado para a autenticação do dispositivo (ou falhas se o aplicativo cliente não puder tratar) para aplicativos que não exigem a autenticação de dispositivo baseada em TLS.
- **Suporte à atualização de MFA** -AD FS agora dá suporte à capacidade de refazer a credencial da 2ª fator com base na atualização da credencial do 2º fator. Isso permite que os clientes façam uma transação inicial com 2 fatores e solicitem apenas o segundo fator em uma base periódica. Isso só está disponível para aplicativos que podem fornecer um parâmetro adicional na solicitação e não é uma configuração configurável no ADFS. Esse parâmetro tem suporte do Azure AD quando "lembrar minha MFA por X dias" está configurado e o sinalizador "supportsMFA" é definido como true nas configurações de confiança do domínio federado no Azure AD. 

### <a name="sign-in-sso-improvements"></a>Aprimoramentos de SSO de entrada
Os seguintes aprimoramentos de SSO de entrada foram feitos no AD FS 2019:

- [UX paginado com tema centralizado](../operations/AD-FS-paginated-sign-in.md) – o ADFS agora foi movido para um fluxo de UX paginado que permite ao ADFS validar e fornecer uma experiência de entrada mais suave. O ADFS agora usa uma interface do usuário centralizada (em vez do lado direito da tela). Você pode exigir que as imagens mais recentes do logotipo e do plano de fundo se alinhem com essa experiência. Isso também espelha a funcionalidade oferecida no Azure AD.
- **Correção de bug: estado de SSO persistente para dispositivos Win10 ao fazer a autenticação de PRT**   Isso resolve um problema em que o estado da MFA não foi persistido ao usar a autenticação de PRT para dispositivos Windows 10. O resultado do problema era que os usuários finais seriam solicitados a receber a credencial de 2ª fator (MFA) com frequência. A correção também torna a experiência consistente quando a autenticação do dispositivo é executada com êxito via TLS do cliente e por meio do mecanismo de PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Suporte para criar aplicativos de linha de negócios modernos
O seguinte suporte para a criação de aplicativos LOB modernos foi adicionado ao AD FS 2019:

 - O **fluxo/perfil do dispositivo OAuth** -AD FS agora dá suporte ao perfil de fluxo do dispositivo OAuth para executar logons em dispositivos que não têm uma área de superfície da interface do usuário para dar suporte a experiências de logon avançadas. Isso permite que o usuário conclua a experiência de logon em um dispositivo diferente. Essa funcionalidade é necessária para CLI do Azure experiência no Azure Stack e pode ser usada em outros casos. 
 - A **remoção do parâmetro ' Resource '** -AD FS agora removeu o requisito para especificar um parâmetro de recurso que está em linha com especificações OAuth atuais. Agora, os clientes podem fornecer o identificador de confiança de terceira parte confiável como o parâmetro de escopo, além das permissões solicitadas. 
 - **Cabeçalhos CORS em respostas AD FS** -os clientes agora podem criar aplicativos de página única que permitem que bibliotecas do lado do cliente validem a assinatura do id_token consultando as chaves de assinatura do documento de descoberta OIDC em AD FS. 
 - **Suporte a PKCE** – o AD FS adiciona suporte a PKCE para fornecer um fluxo de código de autenticação seguro dentro do OAuth. Isso adiciona uma camada adicional de segurança a esse fluxo para evitar seqüestrar o código e reproduzi-lo a partir de um cliente diferente. 
 - **Correção de bug: enviar x5t e reivindicação de criança** -essa é uma correção de bug secundária. AD FS agora, além disso, envia a declaração ' Kid ' para indicar a dica de ID de chave para verificar a assinatura. Anteriormente AD FS enviada apenas como declaração ' x5t '.

### <a name="supportability-improvements"></a>Aprimoramentos de suporte
Os aprimoramentos de suporte a seguir não fazem parte do AD FS 2019:
- **Enviar detalhes do erro para administradores de AD FS** – permite que os administradores configurem os usuários finais para enviar logs de depuração relacionados a uma falha na autenticação do usuário final a ser armazenada como um arquivamento compactado para facilitar o consumo. Os administradores também podem configurar uma conexão SMTP para enviar por email o arquivo compactado para uma conta de email de triagem ou para criar automaticamente um tíquete com base no email. 

### <a name="deployment-updates"></a>Atualizações de implantação
As atualizações de implantação a seguir agora estão incluídas no AD FS 2019:
- **Nível de comportamento de farm 2019** -como com AD FS 2016, há uma nova versão de nível de comportamento de farm necessária para habilitar a nova funcionalidade discutida acima. Isso permite a saída de:
    - 2012 R2-> 2019
    - 2016-> 2019   

### <a name="saml-updates"></a>Atualizações SAML
A seguinte atualização SAML está em AD FS 2019:
- **Correção de bug: corrigir bugs na Federação agregada** – houve inúmeras correções de bugs em relação ao suporte agregado à Federação (por exemplo, incomuns). As correções estão relacionadas ao seguinte: 
  - Dimensionamento aprimorado para grandes n º de entidades no documento de metadados de Federação agregado. anteriormente, isso falharia com o erro "ADMIN0017". 
  - Consulta usando o parâmetro ' ScopeGroupID ' por meio do cmdlet Get-AdfsRelyingPartyTrustsGroup PSH. 
  - Manipulando condições de erro em relação a EntityId duplicada


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Especificação de recurso de estilo do Azure AD no parâmetro de escopo 
Anteriormente, AD FS exigia que o recurso e o escopo desejados estejam em um parâmetro separado em qualquer solicitação de autenticação. Por exemplo, uma solicitação OAuth típica teria a seguinte aparência: 7 **https:&#47;&#47;FS.contoso.com/ADFS/oauth2/Authorize?</br>response_type = Code & client_id = claimsxrayclient & Resource = urn: Microsoft:</br>ADFS: claimsxray & Scope = OAuth & redirect_uri = https:&#47;&#47;adfshelp.Microsoft.com/</br> claimsxray/TokenResponse & prompt = login**
 
Com AD FS no servidor 2019, agora você pode passar o valor do recurso inserido no parâmetro de escopo. Isso é consistente com a maneira como uma pode fazer a autenticação no Azure AD também. 

O parâmetro de escopo agora pode ser organizado como uma lista separada por espaços, em que cada entrada é estruturada como recurso/escopo. 

> [!NOTE]
> Somente um recurso pode ser especificado na solicitação de autenticação. Se mais de um recurso for incluído na solicitação, AD FS retornará um erro e a autenticação não terá sucesso. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Chave de prova para o suporte do PKCE (Code Exchange) para oAuth 
Clientes públicos OAuth que usam a concessão de código de autorização são suscetíveis ao ataque de interceptação de código de autorização.  O ataque é bem descrito na RFC 7636. Para atenuar esse ataque, AD FS no Server 2019 dá suporte à chave de prova de PKCE (troca de código para o fluxo de concessão de código de autorização OAuth). 
 
Para aproveitar o suporte do PKCE, essa especificação adiciona parâmetros adicionais à autorização do OAuth 2,0 e às solicitações de token de acesso.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. O cliente cria e registra um segredo chamado "code_verifier" e deriva uma versão transformada "t (code_verifier)" (conhecida como "code_challenge"), que é enviada na solicitação de autorização OAuth 2,0 juntamente com o método de transformação "t_m". 

B. O ponto de extremidade de autorização responde normalmente, mas registra "t (code_verifier)" e o método de transformação. 

C. Em seguida, o cliente envia o código de autorização na solicitação de token de acesso como de costume, mas inclui o segredo "code_verifier" gerado em (A). 

D. O AD FS transforma "code_verifier" e compara-o com "t (code_verifier)" de (B).  O acesso será negado se eles não forem iguais. 

#### <a name="faq"></a>Perguntas frequentes 
**P.** Posso passar o valor do recurso como parte do valor do escopo como como as solicitações são feitas no Azure AD? 
</br>**A.** Com AD FS no servidor 2019, agora você pode passar o valor do recurso inserido no parâmetro de escopo. O parâmetro de escopo agora pode ser organizado como uma lista separada por espaços, em que cada entrada é estruturada como recurso/escopo. Por exemplo,  
**< criar uma solicitação de amostra válida >**

**P.** AD FS oferece suporte à extensão PKCE?
</br>**A.** O AD FS no servidor 2019 dá suporte à chave de prova de PKCE (troca de código para fluxo de concessão de código de autorização OAuth 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novidades nos Serviços de Federação do Active Directory (AD FS) para Windows Server 2016   
Se você estiver procurando informações sobre versões anteriores do AD FS, consulte os seguintes artigos:  
 [ADFS no Windows Server 2012 ou 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) e [AD FS 2,0](https://technet.microsoft.com/library/adfs2.aspx)  

 O Serviços de Federação do Active Directory (AD FS) fornece controle de acesso e logon único em uma ampla variedade de aplicativos, incluindo o Office 365, aplicativos SaaS baseados em nuvem e aplicativos na rede corporativa.  
* Para a organização de ti, ele permite que você forneça logon e controle de acesso para os aplicativos modernos e herdados, no local e na nuvem, com base no mesmo conjunto de credenciais e políticas.    
* Para o usuário, ele fornece logon contínuo usando as mesmas credenciais de conta conhecidas.  
* Para o desenvolvedor, ele fornece uma maneira fácil de autenticar usuários cujas identidades residem no diretório organizacional para que você possa concentrar seus esforços em seu aplicativo, não autenticação ou identidade.  

Este artigo descreve o que há de novo no AD FS no Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Eliminar senhas da extranet  
O AD FS 2016 permite três novas opções de logon sem senhas, permitindo que as organizações evitem o risco de comprometimento da rede contra senhas roubadas, vazadas ou roubados. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Entrar com a autenticação multifator do Azure
O AD FS 2016 baseia-se nos recursos de MFA (autenticação multifator) do AD FS no Windows Server 2012 R2, permitindo o logon usando apenas um código do Azure MFA, sem primeiro inserir um nome de usuário e uma senha.

* Com o Azure MFA como o método de autenticação principal, o usuário é solicitado a fornecer seu nome de usuário e o código de OTP do aplicativo Azure Authenticator.  
* Com o Azure MFA como o método de autenticação secundário ou adicional, o usuário fornece as credenciais de autenticação primária (usando a autenticação integrada do Windows, o nome de usuário e a senha, o cartão inteligente ou o certificado de dispositivo ou de usuários) e, em seguida, vê um prompt de texto, logon do Azure MFA baseado em voz ou OTP.  
* Com o novo adaptador do Azure MFA interno, a instalação e a configuração do Azure MFA com AD FS nunca foram mais simples.
* As organizações podem aproveitar o Azure MFA sem a necessidade de um servidor Azure MFA local.
* O Azure MFA pode ser configurado para intranet ou extranet, ou como parte de qualquer política de controle de acesso.

Para obter mais informações sobre o Azure MFA com AD FS
*  [Configurar o AD FS 2016 e o MFA do Azure](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Acesso sem senha de dispositivos em conformidade
AD FS 2016 baseia-se nos recursos anteriores de registro do dispositivo para habilitar o logon e o controle de acesso com base no status de conformidade do dispositivo. Os usuários podem fazer logon usando a credencial do dispositivo, e a conformidade é reavaliada quando os atributos do dispositivo são alterados, para que você sempre possa garantir que as políticas estejam sendo impostas.  Isso permite políticas como

* Habilitar o acesso somente de dispositivos gerenciados e/ou em conformidade  
* Habilitar o acesso de extranet somente de dispositivos gerenciados e/ou em conformidade  
* Exigir autenticação multifator para computadores que não são gerenciados ou não são compatíveis  

AD FS fornece o componente local das políticas de acesso condicional em um cenário híbrido. Quando você registra dispositivos com o Azure AD para acesso condicional a recursos de nuvem, a identidade do dispositivo também pode ser usada para políticas de AD FS.

![novidades](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Para obter mais informações sobre como usar o acesso condicional baseado em dispositivo na nuvem   
 *  [Acesso condicional ao Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Para obter mais informações sobre como usar o acesso condicional baseado em dispositivo com o AD FS
*  [Planejando o acesso condicional baseado em dispositivo com o AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Políticas de controle de acesso no AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Entre com o Windows Hello para empresas  

> [!NOTE]
> Atualmente, o Google Chrome e o [novo Microsoft Edge criados em](https://www.microsoft.com/edge?form=MB110A&OCID=MB110A) navegadores de projeto de código-fonte aberto Chromium não têm suporte para logon único baseado em navegador (SSO) com o Microsoft Windows Hello para empresas. Use o Internet Explorer ou uma versão mais antiga do Microsoft Edge.  

Dispositivos Windows 10 introduzem o Windows Hello e o Windows Hello para empresas, substituindo senhas de usuário com credenciais de usuário vinculadas a dispositivos fortes protegidas pelo gesto de um usuário (um PIN, um gesto biométrico como impressão digital ou reconhecimento facial). O AD FS 2016 dá suporte a esses novos recursos do Windows 10 para que os usuários possam entrar no AD FS aplicativos da intranet ou da extranet sem a necessidade de fornecer uma senha.

Para obter mais informações sobre como usar o Microsoft Windows Hello para empresas em sua organização
*  [Habilitar o Windows Hello para empresas em sua organização](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Proteger o acesso aos aplicativos

### <a name="modern-authentication"></a>Autenticação moderna
O AD FS 2016 dá suporte aos mais recentes protocolos modernos que fornecem uma melhor experiência de usuário para o Windows 10, bem como os dispositivos e aplicativos iOS e Android mais recentes.  

Para obter mais informações, consulte [cenários de AD FS para desenvolvedores](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurar políticas de controle de acesso sem a necessidade de conhecer o idioma das regras de declaração  
Anteriormente, AD FS administradores tinham que configurar políticas usando a linguagem de regra de declaração de AD FS, dificultando a configuração e a manutenção de políticas. Com as políticas de controle de acesso, os administradores podem usar modelos internos para aplicar políticas comuns, como
* Permitir somente acesso à intranet
* Permitir todos e exigir MFA da extranet
* Permitir todos e exigir MFA de um grupo específico

Os modelos são fáceis de personalizar usando um processo controlado por assistente para adicionar exceções ou regras de política adicionais e podem ser aplicadas a um ou vários aplicativos para imposição de política consistente.

Para obter mais informações, consulte [políticas de controle de acesso em AD FS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Habilitar logon com diretórios LDAP não AD  
Muitas organizações têm uma combinação de Active Directory e diretórios de terceiros. Com a adição de suporte de AD FS para autenticar usuários armazenados em diretórios compatíveis com LDAP v3, AD FS agora pode ser usado para:
* Usuários em diretórios de terceiros em conformidade com LDAP v3
* Usuários em Active Directory florestas às quais uma Active Directory relação de confiança bidirecional não está configurada
* Usuários em Active Directory Lightweight Directory Services (AD LDS)

Para obter mais informações, consulte [configurar AD FS para autenticar usuários armazenados em diretórios LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Melhor experiência de entrada
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalizar a experiência de entrada para aplicativos AD FS  
Ouvimos que a capacidade de personalizar a experiência de logon de cada aplicativo seria uma excelente melhoria de usabilidade, especialmente para organizações que fornecem logon para aplicativos que representam várias empresas ou marcas diferentes.  

Anteriormente, AD FS no Windows Server 2012 R2 fornecia uma experiência comum de logon para todos os aplicativos de terceira parte confiável, com a capacidade de personalizar um subconjunto de conteúdo baseado em texto por aplicativo. Com o Windows Server 2016, você pode personalizar não apenas as mensagens, mas as imagens, o logotipo e o tema da Web por aplicativo. Além disso, você pode criar temas da Web novos e personalizados e aplicá-los por terceira parte confiável.  

Para obter mais informações, consulte [AD FS personalização de entrada do usuário.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Capacidade de gerenciamento e aprimoramentos operacionais  
A seção a seguir descreve os cenários operacionais aprimorados que são introduzidos com o Serviços de Federação do Active Directory (AD FS) no Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Auditoria simplificada para facilitar o gerenciamento administrativo  
No AD FS do Windows Server 2012 R2, havia vários eventos de auditoria gerados para uma única solicitação e as informações relevantes sobre uma atividade de emissão de log ou de entrada estão ausentes (em algumas versões do AD FS) ou espalhadas por vários eventos de auditoria. Por padrão, os eventos de auditoria AD FS são desativados devido à sua natureza detalhada.  
Com o lançamento do AD FS 2016, a auditoria tornou-se mais simplificada e menos detalhada.  

Para obter mais informações, consulte [aprimoramentos de auditoria para AD FS no Windows Server 2016.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Melhor interoperabilidade com SAML 2,0 para participação no confederations  
O AD FS 2016 contém suporte a protocolo SAML adicional, incluindo suporte para importar relações de confiança com base em metadados que contêm várias entidades. Isso permite que você configure o AD FS para participar de confederations, como Federação incomum e outras implementações em conformidade com o padrão eGov 2,0.  

Para obter mais informações, consulte [interoperabilidade aprimorada com SAML 2,0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gerenciamento simplificado de senhas para usuários federados do O365  
Você pode configurar Serviços de Federação do Active Directory (AD FS) (AD FS) para enviar declarações de expiração de senha para os aplicativos de confiança de terceira parte confiável que são protegidos pelo AD FS. A forma como essas declarações são usadas depende do aplicativo. Por exemplo, com o Office 365 como sua terceira parte confiável, as atualizações foram implementadas para o Exchange e o Outlook para notificar os usuários federados sobre suas senhas em breve.  

Para obter mais informações, consulte [configurar AD FS para enviar declarações de expiração de senha.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>É mais fácil mover de AD FS no Windows Server 2012 R2 para AD FS no Windows Server 2016  
Anteriormente, a migração para uma nova versão do AD FS exigia a exportação da configuração do antigo farm e da importação para um farm novo e paralelo.  

Agora, a mudança de AD FS no Windows Server 2012 R2 para AD FS no Windows Server 2016 tornou-se muito mais fácil. Basta adicionar um novo servidor do Windows Server 2016 a um farm do Windows Server 2012 R2, e o farm atuará no nível de comportamento do farm do Windows Server 2012 R2 e, portanto, ele terá a aparência e se comportará exatamente como um farm do Windows Server 2012 R2.  

Em seguida, adicione novos servidores do Windows Server 2016 ao farm, verifique a funcionalidade e remova os servidores mais antigos do balanceador de carga. Depois que todos os nós do farm estiverem executando o Windows Server 2016, você estará pronto para atualizar o nível de comportamento do farm para 2016 e começar a usar os novos recursos.  

Para obter mais informações, consulte [Atualizando para o AD FS no Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
