---
title: Protegendo o acesso privilegiado
description: Abordagem em fases para proteger o acesso privilegiado
ms.prod: windows-server-threshold
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 0d54a94d51a4d1e0a1d28f78ec39bf16bc3d9100
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822007"
---
# <a name="securing-privileged-access"></a>Protegendo o acesso privilegiado

>Aplica-se a: Windows Server

Proteger o acesso privilegiado é a primeira etapa crítica para o estabelecimento de garantias de segurança para ativos de negócios em uma empresa moderna. A segurança da maioria ou todos os ativos de negócios em uma organização de TI depende a integridade das contas privilegiadas, usadas para administrar, gerenciar e desenvolver. Invasores costumam atacar essas contas e outros elementos de acesso privilegiado para obter acesso a dados e sistemas usando ataques de roubo de credencial, como [Pass-the-Hash e Pass-the-Ticket](https://www.microsoft.com/pth).

Proteger o acesso privilegiado contra determinados adversários exige que você adotar uma abordagem completa e elaborada para isolar esses sistemas contra riscos.

## <a name="what-are-privileged-accounts"></a>O que são contas com privilégios?

Antes de falarmos sobre como protegê-los permite definir contas privilegiadas.

Contas privilegiadas, como os administradores do Active Directory Domain Services têm acesso direto ou indireto, a maioria ou todos os ativos em uma organização de TI, fazendo um risco significativo de negócios de um comprometimento dessas contas.

## <a name="why-securing-privileged-access-is-important"></a>Por que a proteção de acesso privilegiado é importante?

Foco de invasores em acesso privilegiado aos sistemas, como o Active Directory (AD) para obter rapidamente acesso a todos de uma organização de dados de destino. Abordagens de segurança tradicionais se concentraram em rede e firewalls como o perímetro de segurança principal, mas a eficácia de segurança de rede foi significativamente reduzida por duas tendências:

* As organizações estão hospedando dados e recursos fora do limite tradicional da rede em computadores, dispositivos como celulares e tablets, corporativos móveis e serviços de nuvem, tragam seus próprios dispositivos (BYOD)
* Os adversários demonstraram a capacidade consistente e contínua de obter acesso a estações de trabalho dentro do limite da rede por meio de phishing e outros ataques pela Web e por email.

Esses fatores exigem a criação de controles de identidade, além da estratégia de perímetro de rede tradicional de um perímetro de segurança modernas sem autenticação e autorização. Um perímetro de segurança é definido como um conjunto consistente de controles entre ativos e as ameaças a eles. Contas privilegiadas são efetivamente no controle desse novo perímetro de segurança para que ele é essencial para proteger o acesso privilegiado.

![Diagrama que mostra a camada de identidade da organização](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Um invasor que assume o controle de uma conta administrativa pode usar esses privilégios para aumentar seu impacto na organização de destino, conforme descrito abaixo:

![Diagrama que mostra como um adversário que assume o controle de uma conta administrativa pode usar esses privilégios para benefício próprio, às custas da organização de destino](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

A ilustração a seguir mostra dois caminhos:

* Um caminho de "blue" em que uma conta de usuário padrão é usada para acesso sem privilégios a recursos como email e navegação na web e trabalho diário são concluídas.

   > [!NOTE]
   > Os itens de caminho azul descritos posteriormente indicam amplas proteções ambientais que ampliam as contas administrativas.

* Um caminho "vermelho", em que o acesso privilegiado ocorre em um dispositivo protegido para reduzir o risco de phishing e outros ataques da web e email.

![Diagrama mostrando o "caminho" separado para administração que estabelece o roteiro para isolar as tarefas de acesso privilegiado das tarefas de usuário padrão de alto risco, como navegar na web e acessar o email](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Protegendo o roteiro de acesso privilegiado

O roteiro foi projetado para maximizar o uso de tecnologias da Microsoft que você já tiver implantado, tirar proveito das tecnologias de nuvem para aumentar a segurança e integrar qualquer 3ª ferramentas de segurança de terceiros que talvez você já tenha implantado.

O roteiro de recomendações da Microsoft é dividido em 3 fases:

* [Fase 1: Primeiros 30 dias]()
   * Wins rápidas com um impacto positivo significativo.
* [Fase 2: 90 dias]()
   * Melhorias incrementais significativas.
* [Fase 3: Em andamento]()
   * Melhoria de segurança e sustainment.

O roteiro é priorizado para agendar primeiro as implementações mais eficientes e mais rápidas, com base em nossas experiências com esses ataques e na implementação da solução. 

A Microsoft recomenda que você siga este roteiro para proteger o acesso privilegiado contra os adversários determinados. Você pode ajustar este roteiro para acomodar seus recursos existentes e requisitos específicos nas organizações.

> [!NOTE]
> Proteger o acesso privilegiado requer uma ampla variedade de elementos, incluindo componentes técnicos (defesas de host, proteções de conta, gerenciamento de identidade, etc.), bem como alterações em processos e práticas administrativas e conhecimento. As linhas do tempo para o roteiro são aproximadas e são baseadas em nossa experiência com implementações de clientes. A duração variará em sua organização, dependendo da complexidade do ambiente e dos processos de gerenciamento de alterações.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Fase 1: Wins rápidas com mínima complexidade operacional

Fase 1 do roteiro se concentra em reduzir rapidamente as técnicas de ataque mais usadas de roubo de credenciais e abuso. Fase 1 foi projetada para ser implementado em aproximadamente 30 dias e é ilustrada neste diagrama:

![Diagrama da fase 1: 1. Administrador e usuário conta separada, 2. Apenas em senhas de administrador local do tempo, 3. Estágio de estação de trabalho de administração 1, 4. Detecção de ataque de identidade](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Contas separadas

Para ajudar a separar os riscos da internet (ataques de phishing, navegação na web) de privilégios acessar contas, criar uma conta dedicada para todos os funcionários com acesso privilegiado. Os administradores não devem navegar na web, verificando seu email e realizar tarefas do dia a dia produtividade com contas altamente privilegiadas. Para obter mais informações sobre isso podem ser encontradas na seção [separar contas administrativas](securing-privileged-access-reference-material.md#separate-administrative-accounts) do documento de referência.

Siga as orientações neste artigo [gerenciar contas de acesso de emergência no Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access) para criar acesso de emergência de pelo menos duas contas, com direitos de administrador permanentemente atribuídos, em ambos os locais AD e o Azure AD ambientes . Essas contas são apenas para uso quando contas de administrador tradicionais não conseguem executar uma tarefa obrigatória como em um caso de um desastre.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Apenas em senhas de administrador local do tempo

Para atenuar o risco de um adversário roube um hash de senha de conta de administrador local do banco de dados SAM local e utilize-a para atacar outros computadores, as organizações devem assegurar que cada máquina tem uma senha de administrador local exclusivo. A ferramenta de solução de senha de Administrador Local (LAPS) pode configurar senhas aleatórias exclusivas em cada estação de trabalho e servidor armazená-las no Active Directory (AD) protegido por uma ACL. Somente usuários autorizados elegíveis podem ler ou solicitar a redefinição de senhas dessas contas de administrador local. Você pode obter as LAPS para uso em estações de trabalho e servidores do [Microsoft Download Center](http://Aka.ms/LAPS).

Orientações adicionais para operar um ambiente com LAPS e PAWs podem ser encontradas na seção [padrões operacionais com base no princípio da origem limpa](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Estações de trabalho administrativas

Como medida de segurança iniciais para os usuários com privilégios administrativos do Active Directory local tradicional e o Azure Active Directory, certifique-se de que estão usando dispositivos Windows 10 configurados com o [padrões para um Windows altamente seguro dispositivo 10](/windows-hardware/design/device-experiences/oem-highly-secure). 

### <a name="4-identity-attack-detection"></a>4. Detecção de ataque de identidade

[Proteção avançada de ameaça do Azure (ATP)](/azure-advanced-threat-protection/what-is-atp) é uma solução de segurança baseado em nuvem que identifica, detecta e ajuda você a investigar ameaças avançadas, identidades comprometidas e ações internas mal-intencionadas direcionadas a seu ativo no local Ambiente de diretório.

## <a name="phase-2-significant-incremental-improvements"></a>Fase 2: Melhorias significativas de incrementais

Fase 2 amplia o trabalho feito na fase 1 e foi projetado para ser concluída em aproximadamente 90 dias. As etapas desse estágio são ilustradas no diagrama:

![Diagrama da fase 2: 1. Windows Hello para empresas / MFA, 2. Distribuição PAW, 3. Apenas no tempo de privilégios, 4. Credential Guard, 5. Credenciais vazadas, 6. Detecção de vulnerabilidade de movimento lateral](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Exigir Windows Hello for Business e MFA

Os administradores podem aproveitar a facilidade de uso associado com o Windows Hello para empresas. Os administradores podem substituir suas senhas complexas com autenticação de dois fatores forte em seus computadores. Um invasor precisa ter tanto o dispositivo e a informações biometria ou PIN, é muito mais difícil de obter acesso sem o conhecimento do funcionário. Para obter mais detalhes sobre o Windows Hello for Business e o caminho para distribuir podem ser encontradas no artigo [Windows Hello para visão geral de negócios](/windows/security/identity-protection/hello-for-business/hello-overview)

Habilite a autenticação multifator (MFA) para suas contas de administrador no Azure AD usando o Azure MFA. No mínimo enable a [política de acesso condicional de proteção de linha de base](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins) para obter mais informações sobre a autenticação multifator do Azure podem ser encontradas no artigo [implantar autenticação de multifator do Azure baseado em nuvem](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Implantar a PAW a todos os proprietários de conta de acesso de identidade privilegiada

Continuando o processo de separar contas com privilégios de ameaças encontradas no email, navegação na web e outras tarefas não administrativas, você deve implementar dedicado estações de trabalho (PAW com acesso privilegiado) para todos os funcionários com acesso privilegiado ao seu sistemas de informações da organização. Orientações adicionais para a implantação da PAW podem ser encontradas no artigo [estações de trabalho de acesso privilegiado](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Just-in privilégios de tempo

Para reduzir o tempo de exposição de privilégios e aumentar a visibilidade sobre seu uso, forneça privilégios just-in-time (JIT) usando uma solução apropriada, como aqueles abaixo ou outras soluções de terceiros:

* Para o AD DS (Active Directory Domain Services), use o recurso [PAM (Privileged Access Manager)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) do MIM (Microsoft Identity Manager).
* Para o Azure Active Directory, use o recurso [PIM (Privileged Identity Management) do Azure AD](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Habilitar o Windows Defender Credential Guard

Habilitar o Credential Guard ajuda a proteger os hashes de senha NTLM, Kerberos Ticket Granting Tickets e as credenciais armazenadas por aplicativos como credenciais de domínio. Esse recurso ajuda a impedir ataques de roubo de credenciais como Pass-the-Hash ou Pass-The-Ticket, aumentando a dificuldade de dinamização no ambiente usando credenciais roubadas. Informações sobre como funciona o Credential Guard e como implantar podem ser encontradas no artigo [derivado de proteger as credenciais de domínio com o Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Credenciais vazadas reporting

"Todos os dias, o Microsoft analisa os sinais de mais de trilhões de 6.5 para identificar ameaças emergentes e proteger os clientes" - [Microsoft por números](https://news.microsoft.com/bythenumbers/cyber-attacks)

Habilite o Microsoft Azure AD Identity Protection informar os usuários com credenciais vazadas, de modo que você pode corrigi-los. [O Azure AD Identity Protection](/azure/active-directory/identity-protection/index) podem ser aproveitados para ajudar sua organização proteger os ambientes de nuvem e híbridas de ameaças.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Caminhos de movimento Lateral de ATP do Azure

Certifique-se com privilégios acessar conta detentores estão usando seu PAW para administração apenas para que um contas sem privilégios comprometidas não é possível obter acesso a uma conta com privilégios por meio de ataques de roubo de credenciais como Pass-the-Hash ou Pass-The-Ticket. [Azure ATP Lateral caminhos de movimento (LMPs)](/azure-advanced-threat-protection/use-case-lateral-movement-path) fornece fácil entender a emissão de relatórios para identificar onde podem ser abertas para comprometer contas privilegiadas.

## <a name="phase-3-security-improvement-and-sustainment"></a>Fase 3: Sustainment e melhoria de segurança

Fase 3 do roteiro do amplia as etapas executadas nas fases 1 e 2 para fortalecer sua postura de segurança. Fase 3 é descrito visualmente neste diagrama:

![Fase 3: 1. Examine o RBAC, 2. Reduza superfícies de ataque, 3. Integre logs SEIM, 4. Automação de credenciais vazadas](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Esses recursos serão compilam as etapas das fases anteriores e moverão suas defesas para uma postura mais proativa. Essa fase não tem nenhuma linha do tempo específica e pode levar mais tempo para implementar com base em sua organização individual.

### <a name="1-review-role-based-access-control"></a>1. Examine o controle de acesso baseado em função

Usando o modelo em camadas três descritas no artigo [modelo de camadas administrativas do Active Directory](securing-privileged-access-reference-material.md), revise e verifique se os administradores de camada inferiores não têm acesso administrativo a recursos da camada superior (associações de grupo, as ACLs em contas de usuário, etc...).

### <a name="2-reduce-attack-surfaces"></a>2. Reduzir superfícies de ataque

Proteger suas cargas de trabalho de identidade incluindo domínios, controladores de domínio, AD FS e Azure AD Connect como comprometer um desses sistemas poderá resultar no comprometimento de outros sistemas em sua organização. Os artigos [reduzindo a superfície de ataque do Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) e [cinco etapas para proteger sua infraestrutura de identidade](/azure/security/azure-ad-secure-steps) fornecem diretrizes para proteger seus locais e híbridos ambientes de identidade.

### <a name="3-integrate-logs-with-siem"></a>3. Integrar os logs de SIEM

A integração do registro em log em uma ferramenta SIEM centralizada pode ajudar sua organização para analisar, detectar e responder a eventos de segurança. Os artigos [monitoramento do Active Directory para sinais de comprometimento](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) e [l Apêndice: Eventos a serem monitorados](../ad-ds/plan/appendix-l--events-to-monitor.md) fornecem orientação sobre eventos que devem ser monitorados no seu ambiente.

Isso faz parte do beyond planejar porque agregar, criando e ajuste de alertas em um evento e informações de gerenciamento de segurança (SIEM) requer a analistas qualificados (ao contrário do Azure ATP no plano de 30 dias, que inclui fora a caixa de alerta)

### <a name="4-leaked-credentials---force-password-reset"></a>4. Credenciais vazadas - redefinição de senha de força

Continue a melhorar sua postura de segurança, permitindo que o Azure AD Identity Protection forçar a redefinições de senha automaticamente quando as senhas são suspeita de comprometimento. As diretrizes encontradas no artigo [usar eventos de risco para a autenticação multifator de disparador e alterações de senha](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) explica como habilitar isso usando uma política de acesso condicional.

## <a name="am-i-done"></a>Isso é tudo?

A resposta curta é não.

As pessoas mal-intencionadas nunca interrompa, portanto, nenhum dos dois pode você. Este mapa pode ajudar sua organização a proteger contra conhecida no momento, as ameaças conforme os invasores serão constantemente evoluem- and -shift. É recomendável que considerar a segurança como um processo contínuo voltada para aumentar o custo e reduzir a taxa de sucesso de adversários direcionando o seu ambiente.

Embora não seja a única parte do programa de segurança da sua organização protegendo o acesso privilegiado é um componente essencial de sua estratégia de segurança.
