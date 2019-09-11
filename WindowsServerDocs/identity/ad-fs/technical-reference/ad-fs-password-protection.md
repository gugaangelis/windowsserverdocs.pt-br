---
title: Proteção contra ataques de senha AD FS
description: Este documento descreve como proteger AD FS usuários contra ataques de senha
author: billmath
manager: mtillman
ms.reviewer: andandyMSFT
ms.date: 11/15/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c446ec86d426148836267e29610f86691cf0798a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865459"
---
# <a name="ad-fs-password-attack-protection"></a>Proteção contra ataques de senha AD FS

## <a name="what-is-a-password-attack"></a>O que é um ataque de senha?

Um requisito para o logon único federado é a disponibilidade de pontos de extremidade para autenticar pela Internet. A disponibilidade de pontos de extremidade de autenticação na Internet permite que os usuários acessem os aplicativos mesmo quando não estão em uma rede corporativa. 

No entanto, isso também significa que alguns atores ruins podem aproveitar os pontos de extremidade federados disponíveis na Internet e usar esses pontos de extremidade para tentar determinar senhas ou para criar ataques de negação de serviço. Um desses ataques que está se tornando mais comum é chamado de **ataque de senha**. 

Há dois tipos de ataques de senha comuns. Ataque de irrigação de senha & ataque de senha de força bruta. 

### <a name="password-spray-attack"></a>Ataque de irrigação de senha
Em um ataque de irrigação de senha, esses atores inválidos tentarão as senhas mais comuns em várias contas e serviços diferentes para obter acesso a quaisquer ativos protegidos por senha que eles possam encontrar. Normalmente, eles abrangem muitas organizações e provedores de identidade diferentes. Por exemplo, um invasor usará um kit de ferramentas comumente disponível para enumerar todos os usuários em várias organizações e, em seguida, tentar "P @ $ $w 0rd" e "password1" em relação a todas essas contas. Para lhe dar a ideia, um ataque pode ser semelhante a:


|  Usuário de destino   | Senha de destino |
|----------------|-----------------|
| User1@org1.com |    Password1    |
| User2@org1.com |    Password1    |
| User1@org2.com |    Password1    |
| User2@org2.com |    Password1    |
|       …        |        …        |
| User1@org1.com |    P @ $ $w 0rd     |
| User2@org1.com |    P @ $ $w 0rd     |
| User1@org2.com |    P @ $ $w 0rd     |
| User2@org2.com |    P @ $ $w 0rd     |

Esse padrão de ataque evita a maioria das técnicas de detecção porque, do ponto privilegiando de um usuário individual ou empresa, o ataque parece ser um logon com falha isolado.

Para os invasores, é um jogo de números: eles sabem que há algumas senhas que são muito comuns.  O invasor terá alguns sucessos para cada mil contas atacadas, e isso é suficiente para ser eficaz. Eles usam as contas para obter dados de emails, coletar informações de contato e enviar links de phishing ou simplesmente expandir o grupo de destino de irrigação de senha. Os invasores não se preocupam muito com quem são os destinos iniciais — apenas que têm um sucesso que podem ser aproveitados.

Mas, ao fazer algumas etapas para configurar o AD FS e a rede corretamente, AD FS pontos de extremidade podem ser protegidos contra esses tipos de ataques. Este artigo aborda três áreas que precisam ser configuradas corretamente para ajudar a proteger contra esses ataques.

### <a name="brute-force-password-attack"></a>Ataque de senha de força bruta 
Nessa forma de ataque, um invasor tentará várias tentativas de senha em relação a um conjunto de contas direcionado. Em muitos casos, essas contas serão destinadas a usuários que têm um nível mais alto de acesso dentro da organização. Eles podem ser executivos na organização ou administradores que gerenciam a infraestrutura crítica.  

Esse tipo de ataque também poderia resultar em padrões de DOS. Isso pode estar no nível de serviço em que o ADFS não consegue processar um grande número de solicitações devido a um número insuficiente de servidores ou pode estar em um nível de usuário em que um usuário está bloqueado de sua conta.  

## <a name="securing-ad-fs-against-password-attacks"></a>Protegendo AD FS contra ataques de senha 

Mas, ao fazer algumas etapas para configurar o AD FS e a rede corretamente, AD FS pontos de extremidade podem ser protegidos contra esses tipos de ataques. Este artigo aborda três áreas que precisam ser configuradas corretamente para ajudar a proteger contra esses ataques. 


- Nível 1, linha de base: Essas são as configurações básicas que devem ser configuradas em um servidor AD FS para garantir que os atores inválidos não possam atacar os usuários federados do ataque de força bruta. 
- Nível 2, protegendo a extranet: Essas são as configurações que devem ser configuradas para garantir que o acesso à extranet esteja configurado para usar protocolos seguros, políticas de autenticação e aplicativos apropriados. 
- Nível 3, mova para a menos senha para acesso à extranet: Essas são configurações avançadas e diretrizes para habilitar o acesso a recursos federados com credenciais mais seguras, em vez de senhas que estão sujeitas a ataques. 

## <a name="level-1-baseline"></a>Nível 1: base

1. Se o ADFS 2016, implementar o bloqueio inteligente da extranet de [bloqueio inteligente de extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md) rastreia locais familiares e permitirá que um usuário válido chegue se tiver feito logon com êxito nesse local. Usando o bloqueio inteligente de extranet, você pode garantir que os atores ruins não sejam capazes de atacar a força bruta dos usuários e, ao mesmo tempo, permitir que o usuário legítimo seja produtivo.
    - Se você não estiver no AD FS 2016, é altamente recomendável [Atualizar](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) para AD FS 2016. É um caminho de atualização simples do AD FS 2012 R2. Se você estiver no AD FS 2012 R2, implemente o [bloqueio de extranet](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md). Uma desvantagem dessa abordagem é que os usuários válidos podem ser impedidos de acesso à extranet se você estiver em um padrão de força bruta. O AD FS no servidor 2016 não tem essa desvantagem.

2. Monitorar & bloquear endereços IP suspeitos 
    - Se você tiver Azure AD Premium, implemente o Connect Health para ADFS e use as notificações de [relatório de IP arriscadas](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-adfs#risky-ip-report-public-preview) que ele fornece.

        a. O licenciamento não é para todos os usuários e requer 25 licenças/servidor ADFS/WAP, o que pode ser fácil para um cliente.

        b. Agora você pode investigar os IPs que estão gerando um grande número de logons com falha

        c. Isso exigirá que você habilite a auditoria em seus servidores ADFS.

3.  Bloquear IP suspeito.  Isso potencialmente bloqueia ataques de DOS.

    a. Se em 2016, use o recurso de [*endereços IP proibidos pela extranet*](../../ad-fs/operations/configure-ad-fs-banned-ip.md) para bloquear as solicitações de IP sinalizadas por #3 (ou análise manual).

    b. Se você estiver no AD FS 2012 R2 ou inferior, bloqueie o endereço IP diretamente no Exchange Online e, opcionalmente, no firewall.

4. Se você tiver Azure AD Premium, use a [proteção de senha do Azure ad](https://docs.microsoft.com/azure/active-directory/authentication/concept-password-ban-bad-on-premises) para impedir que senhas que sejam adivinhadas entrem no Azure AD  

    a. Observe que, se você tiver senhas que possam ser adivinhadas, poderá decifra-las com apenas 1-3 tentativas. Esse recurso impede que eles sejam definidos. 

    b. De nossas estatísticas de visualização, quase 20-50% das novas senhas são impedidas de serem definidas. Isso implica que% dos usuários estão vulneráveis a senhas facilmente adivinhadas.

## <a name="level-2-protect-your-extranet"></a>Nível 2: Proteja sua extranet

5. Vá para a autenticação moderna para todos os clientes que acessam a partir da extranet. Os clientes de email são uma grande parte disso. 

    a. Você precisará usar o Outlook Mobile para dispositivos móveis. O novo aplicativo de correio nativo do iOS também dá suporte à autenticação moderna. 

    b. Você precisará usar o Outlook 2013 (com os patches mais recentes do CU) ou o Outlook 2016.

6. Habilite a MFA para todos os acessos à extranet. Isso permite que você adicione proteção para qualquer acesso à extranet.

   a.  Se você tiver o Azure AD Premium, use [as políticas de acesso condicional do Azure ad](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) para controlar isso.  Isso é melhor do que implementar as regras em AD FS.  Isso ocorre porque os aplicativos cliente modernos são impostos com frequência.  Isso ocorre, no Azure AD, ao solicitar um novo token de acesso (normalmente a cada hora) usando um token de atualização.  

   b.  Se você não tiver o Azure AD Premium ou tiver aplicativos adicionais em AD FS que permita o acesso baseado na Internet, implemente o MFA (pode ser o Azure MFA, bem como o AD FS 2016) e faça uma [política de MFA global](../../ad-fs/operations/configure-authentication-policies.md#to-configure-multi-factor-authentication-globally) para todos os acessos à extranet.

## <a name="level-3-move-to-password-less-for-extranet-access"></a>Nível 3: Mover para senha menos para acesso à extranet

7. Vá para a janela 10 e use [Olá para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).

8. Para outros dispositivos, se estiver no AD FS 2016, você poderá usar a [OTP do Azure MFA](../../ad-fs/operations/configure-ad-fs-and-azure-mfa.md) como o primeiro fator e a senha como o 2º fator. 

9. Para dispositivos móveis, se você só permitir dispositivos gerenciados por MDM, poderá usar [certificados](../../ad-fs/operations/configure-user-certificate-authentication.md) para fazer logon no usuário. 

## <a name="urgent-handling"></a>Tratamento urgente

Se o ambiente de AD FS estiver sob ataque ativo, as etapas a seguir devem ser implementadas no mais cedo:

 - Desabilite o ponto de extremidade U/P no ADFS e exija que todos para VPN obtenham acesso ou estejam dentro de sua rede. Isso exige que você tenha o **nível de etapa 2 #1a** concluído. Caso contrário, todas as solicitações internas do Outlook ainda serão roteadas por meio da nuvem por meio da autenticação de proxy EXO.
 - Se o ataque só for fornecido via EXO, você poderá desabilitar a autenticação básica para protocolos do Exchange (POP, IMAP, SMTP, EWS, etc.) usando políticas de autenticação, esses protocolos e métodos de autenticação estão sendo usados na grande maioria desses ataques. Além disso, as regras de acesso para cliente no EXO e a habilitação do protocolo por caixa de correio são avaliadas após a autenticação e não ajudarão a mitigar os ataques. 
 - Ofereça acesso de extranet seletivamente usando o nível 3 #1-3.

## <a name="next-steps"></a>Próximas etapas

- [Atualizar para o AD FS Server 2016](../../ad-fs/deployment/upgrading-to-ad-fs-in-windows-server.md) 
- [Bloqueio inteligente de extranet no AD FS 2016](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurar políticas de acesso condicional](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [Proteção de senha do Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises)
