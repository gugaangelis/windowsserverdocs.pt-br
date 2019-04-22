---
title: Proteção do AD FS ataque de senha
description: Este documento descreve como proteger os usuários do AD FS contra ataques de senha
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: de1af9712b54c977c591953c68eec506c80d3cdd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821997"
---
# <a name="ad-fs-password-attack-protection"></a>Proteção do AD FS ataque de senha

## <a name="what-is-a-password-attack"></a>O que é um ataque de senha?

Um requisito para o logon único federado é a disponibilidade de pontos de extremidade para autenticar na Internet. A disponibilidade de pontos de extremidade de autenticação na internet permite que os usuários acessem os aplicativos, mesmo quando eles não estiverem em uma rede corporativa. 

No entanto, isso também significa que alguns atores ruins podem aproveitar os pontos de extremidade federados disponíveis na internet e usar esses pontos de extremidade para tentar determinar senhas ou para criar ataques negação de serviço. Um ataque desse tipo que está se tornando mais comum é chamado de um **ataques de senha**. 

Há 2 tipos de ataques comuns à senha. Ataque de senha de força de senha spray ataque & bruta. 

### <a name="password-spray-attack"></a>Ataque de Spray de senha
Em um ataque de spray de senha, esses atores ruins tentará as senhas mais comuns em muitas contas diferentes e serviços para obter acesso a todos eles podem encontrar os ativos protegido por senha. Geralmente eles abrangem muitos diferentes organizações e provedores de identidade. Por exemplo, um invasor usará um kit de ferramentas geralmente disponível para enumerar todos os usuários em várias organizações e, em seguida, tente "P@$$w0rd" e "Password1" em relação a todas essas contas. Para dar a ideia, um ataque pode parecer com:

|Usuário de destino|Senha de destino|
|-----|-----|-----|
|User1@org1.com|Password1|
|User2@org1.com|Password1|
|User1@org2.com|Password1|
|User2@org2.com|Password1|
|…|…|
|User1@org1.com|P@$$w0rd|
|User2@org1.com|P@$$w0rd|
|User1@org2.com|P@$$w0rd|
|User2@org2.com|P@$$w0rd|

Esse padrão de ataque escapa a maioria das técnicas de detecção, porque a partir do ponto de posições de vantagem de um usuário individual ou da empresa, o ataque apenas se parece com um logon com falha isolado.

Para os invasores, é um jogo de números: eles sabem que há algumas senhas por aí que são muito comuns.  O invasor obterá alguns êxitos para todas as contas de milhar atacadas e isso é o suficiente para ser eficaz. Eles usam as contas para obter dados de emails, colete informações de contato e enviar links de phishing ou apenas, expanda o grupo de destino de spray de senha. Os invasores não se preocupar muito com quem são os destinos iniciais — apenas que eles têm algum sucesso que podem utilizar.

Eles usam as contas para obter dados de emails, colete informações de contato e enviar links de phishing ou apenas, expanda o grupo de destino de spray de senha. Os invasores não se preocupar muito com quem são os destinos iniciais — apenas que eles têm algum sucesso que podem utilizar.

Mas seguindo algumas etapas para configurar o AD FS e de rede corretamente, os pontos de extremidade do AD FS podem ser protegidos contra esses tipos de ataques. Este artigo aborda 3 áreas que precisam ser configurados corretamente para ajudar a proteger contra esses ataques.

### <a name="brute-force-password-attack"></a>Ataque de força bruta 
Essa forma de ataque, o invasor tenta várias tentativas de senha em relação a um conjunto direcionado de contas. Em muitos casos, essas contas serão direcionadas em relação a usuários que têm um nível mais alto de acesso dentro da organização. Eles podem ser executivos dentro da organização ou os administradores que gerenciam infraestrutura crítica.  

Esse tipo de ataque também pode resultar em padrões DOS. Isso pode ocorrer no nível de serviço em que o ADFS é capaz de processar um grande número de solicitações devido ao # insuficiente de servidores ou poderia estar em um nível de usuário em que um usuário estiver bloqueado do sua conta.  

## <a name="securing-ad-fs-against-password-attacks"></a>Protegendo o AD FS contra ataques de senha 
 
Mas seguindo algumas etapas para configurar o AD FS e de rede corretamente, os pontos de extremidade do AD FS podem ser protegidos contra esses tipos de ataques. Este artigo aborda 3 áreas que precisam ser configurados corretamente para ajudar a proteger contra esses ataques. 


- Nível 1, linha de base: Essas são as configurações básicas que devem ser configuradas em um servidor do AD FS para garantir que os atores ruins não os usuários federado do ataque de força bruta. 
- Nível 2, protegendo a extranet: Essas são as configurações que devem ser configuradas para garantir que o acesso à extranet esteja configurado para usar protocolos seguros, as políticas de autenticação e aplicativos apropriados. 
- Nível 3, a mudança para sem senha para acesso à extranet: Esses são avançados, as configurações e diretrizes para habilitar o acesso aos recursos federados com as credenciais mais segura, em vez de senhas que são propensas a ataques. 

## <a name="level-1-baseline"></a>Nível 1: Linha de base

1. Se o AD FS 2016, implementar [extranet bloqueio inteligente](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md) Extranet bloqueio inteligente controla locais familiares e permitirá que um usuário válido cheguem por meio de se eles já fez logon anteriormente com êxito nesse local. Usando o bloqueio inteligente de extranet, você pode garantir que os atores ruins não ser capazes de força bruta atacar os usuários e ao mesmo tempo permitirá que usuários legítimos sejam produtivos.
    - Se você não estiver no AD FS 2016, é altamente recomendável [atualizar](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) para AD FS 2016. É um caminho simples de atualização do AD FS 2012 R2. Se você estiver usando o AD FS 2012 R2, implemente [bloqueio de extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md). Uma desvantagem dessa abordagem é que os usuários válidos podem ser impedidos de acesso à extranet, se você estiver em um padrão de força bruta. O AD FS no Server 2016 não tem esta desvantagem.

2. Monitor & Bloco de endereços IP suspeitos 
    - Se você tiver o Azure AD Premium, implementar o Connect Health para AD FS e usar o [relatório IP arriscado](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview) notificações que ele oferece.
        
        a. Licenciamento não é para todos os usuários e requer o servidor de licenças/ADFS/WAP 25 que pode ser mais fácil para um cliente.
    
        b. Agora você pode investigar o IP que está gerando um grande número de logons com falha
    
        c. Isso exigirá que você habilitar a auditoria nos servidores ADFS.

3.  Bloquear o IP suspeito.  Potencialmente, isso bloqueia ataques DOS.

    a. Se em 2016, use o [ *endereços IP Extranet proibidos* ](../../ad-fs/operations/configure-ad-fs-banned-ip.md) sinalizado o recurso para bloquear todas as solicitações do IP por #3 (ou análise manual).

    b. Se você estiver usando o AD FS 2012 R2 ou inferior, bloquear o endereço IP diretamente no Exchange Online e, opcionalmente, no firewall.

4. Se você tiver o Azure AD Premium, use [proteção por senha do Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) para impedir que as senhas podem ser adivinhadas Introdução ao Azure AD  

    a. Observe que, se você tiver que adivinhar senhas, você pode quebrá-las com apenas 1 a 3 tentativas. Esse recurso impede que eles do conjunto de Introdução. 

    b. Do nosso estatísticas de visualização, quase 20 a 50% de novas senhas obter bloqueado de ser definida. Isso implica que % dos usuários estão vulneráveis à fáceis de adivinhar senhas.

## <a name="level-2-protect-your-extranet"></a>Nível 2: Proteger seu extranet

5. Mover para a autenticação moderna para todos os clientes acessando da extranet. Os clientes de email são uma boa parte disso. 

    a. Você precisará usar o Outlook Mobile para dispositivos móveis. O novo aplicativo de email nativo do iOS dá suporte à autenticação moderna também. 

    b. Você precisará usar o Outlook 2013 (com os patches CU mais recente) ou o Outlook 2016.

6.  Habilite a MFA para todos os acessos extranet. Assim você adicionou uma proteção para qualquer acesso à extranet.

    a.  Se você tiver o Azure AD premium, use [políticas de acesso condicional do Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) controlar isso.  Isso é melhor do que implementar as regras no AD FS.  Isso ocorre porque os aplicativos cliente modernos são aplicados com mais frequência.  Isso ocorre, no Azure AD, ao solicitar um novo token de acesso (normalmente a cada hora) usando um token de atualização.  

    b.  Se você não tiver o Azure AD premium ou se tiveram aplicativos adicionais no AD FS que permitem a internet acesso com base em, implementar o MFA (também pode ser Azure MFA no AD FS 2016) e fazer uma [política MFA global](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally) para todo o acesso à extranet.
 
## <a name="level-3-move-to-password-less-for-extranet-access"></a>Nível 3: Mover para a senha, menos para acesso à extranet

7. Mover a janela de 10 e usar [Hello For Business](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

8. Para outros dispositivos, se no AD FS 2016, você pode usar [OTP de MFA do Azure](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md) como o primeiro e a senha como o 2º fator. 

9.  Para dispositivos móveis, se você permitir que somente dispositivos gerenciados MDM, você pode usar [certificados](../../ad-fs/operations/configure-user-certificate-authentication.md) para o usuário. 
 
## <a name="urgent-handling"></a>Tratamento de urgente

Se o ambiente do AD FS estiver sob ataque ativo, as etapas a seguir devem ser implementadas o mais rápido possível:

 - Desabilitar ponto de extremidade de U/P no ADFS e exigir que todas as pessoas a VPN para obter acesso ou ser dentro de sua rede. Isso exige que você tem a etapa **nível 2 #1a** concluída. Caso contrário, todas as solicitações de Outlook internas ainda serão roteadas por meio da nuvem por meio de autenticação de proxy. EXO
 - Se o ataque só estará disponível por meio de EXO, você pode desabilitar a autenticação básica para protocolos de troca (POP, IMAP, SMTP, EWS, etc.) usando as políticas de autenticação, esses protocolos e métodos de autenticação estão sendo usados na grande maioria desses ataques. Além disso, as regras de acesso do cliente na habilitação de protocolo EXO e por caixa de correio são avaliada pós-autenticação e não ajudará a reduzir os ataques. 
 - Seletivamente oferecem acesso à extranet usando o nível 3 #1 a 3.

## <a name="next-steps"></a>Próximas etapas

- [Atualizar para o servidor do AD FS 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Bloqueio inteligente de extranet do AD FS 2016](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurar políticas de acesso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Proteção por senha do AD do Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
