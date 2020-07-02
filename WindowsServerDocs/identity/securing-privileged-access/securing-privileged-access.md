---
title: Proteger o acesso privilegiado
description: Abordagem em fases para proteger o acesso privilegiado
ms.prod: windows-server
ms.topic: conceptual
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
ms.date: 02/25/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 806a2aced95421bd469ba885d4a81c219ae1b651
ms.sourcegitcommit: 457e88e5aa6be13a2bffdb8e434a8efc3698678f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85548830"
---
# <a name="securing-privileged-access"></a>Proteger o acesso privilegiado

>Aplica-se a: Windows Server

Proteger o acesso privilegiado é a primeira etapa crítica para o estabelecimento de garantias de segurança para ativos de negócios em uma empresa moderna. A segurança da maioria dos ativos de negócios ou de todos eles em uma organização de TI depende da integridade das contas privilegiadas usadas para administrar, gerenciar e desenvolver. Invasores virtuais costumam visar essas contas e outros elementos de acesso privilegiado para obter acesso aos dados e sistemas usando ataques de roubo de credenciais, como [Pass-the-Hash e Pass-the-Ticket](https://www.microsoft.com/pth).

Proteger o acesso privilegiado em relação a determinados adversários requer uma abordagem completa e elaborada para isolar esses sistemas contra riscos.

## <a name="what-are-privileged-accounts"></a>O que são contas privilegiadas?

Antes de falarmos sobre como protegê-las, vamos definir contas privilegiadas.

Contas privilegiadas, como administradores do Active Directory Domain Services, têm acesso direto ou indireto à maioria ou a todos os ativos de uma organização de TI, o que torna o comprometimento dessas contas um risco comercial significativo.

## <a name="why-securing-privileged-access-is-important"></a>Por que proteger o acesso privilegiado é importante?

Os invasores virtuais estão se concentrando no acesso privilegiado a sistemas como o AD (Active Directory) para obter rapidamente acesso a todos os dados de destino de uma organização. As abordagens tradicionais de segurança têm se concentrado em rede e firewalls como o perímetro de segurança principal, mas a eficácia da segurança da rede foi significativamente reduzida por duas tendências:

* As organizações estão hospedando dados e recursos fora do limite tradicional da rede em computadores corporativos móveis, dispositivos como celulares e tablets, serviços de nuvem e dispositivos BYOD (traga seus próprios dispositivos)
* Os adversários demonstraram a capacidade consistente e contínua de obter acesso a estações de trabalho dentro do limite da rede por meio de phishing e outros ataques pela Web e por email.

Esses fatores exigem a criação de um perímetro de segurança moderno fora dos controles de identidade de autenticação e autorização, além da estratégia de perímetro de rede tradicional. Um perímetro de segurança aqui é definido como um conjunto consistente de controles entre os ativos e as ameaças a eles. Contas com privilégios estão efetivamente no controle desse novo perímetro de segurança. Portanto, é essencial proteger o acesso privilegiado.

![Diagrama que mostra a camada de identidade da organização](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Um invasor que assume o controle de uma conta administrativa pode usar esses privilégios para aumentar seu impacto à organização de destino, conforme descrito abaixo:

![Diagrama que mostra como um adversário que assume o controle de uma conta administrativa pode usar esses privilégios para benefício próprio, às custas da organização de destino](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

A ilustração a seguir descreve dois caminhos:

* Um caminho "azul" em que uma conta de usuário padrão é usada para acesso não privilegiado a recursos como email e navegação na Web e trabalho diário é concluído.

   > [!NOTE]
   > Itens no caminho azul descritos posteriormente em indicam proteções ambientais amplas que se estendem além das contas administrativas.

* Um caminho "vermelho" em que o acesso privilegiado ocorre em um dispositivo protegido para reduzir o risco de phishing e outros ataques por email e pela Web.

![Diagrama que mostra o "caminho" separado para administração que o roteiro estabelece para isolar as tarefas de acesso privilegiado das tarefas de usuário padrão de alto risco, como navegar na Web e acessar emails](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

## <a name="securing-privileged-access-roadmap"></a>Como proteger o roteiro de acesso privilegiado

O roteiro foi projetado para maximizar o uso de tecnologias da Microsoft que você já implantou, aproveitar as tecnologias de nuvem para aumentar a segurança e integrar ferramentas de segurança de terceiros que talvez você já tenha implantado.

O roteiro de recomendações da Microsoft é dividido em três fases:

* [Fase 1: Primeiros 30 dias](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#phase-1-quick-wins-with-minimal-operational-complexity)
   * WINS rápido com impacto positivo significativo.
* [Fase 2: 90 dias](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#phase-2-significant-incremental-improvements)
   * Melhorias incrementais significativas.
* [Fase 3: Em andamento](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#phase-3-security-improvement-and-sustainment)
   * Melhoria e manutenção da segurança.

O roteiro é priorizado para agendar primeiro as implementações mais eficientes e mais rápidas, com base em nossas experiências com esses ataques e na implementação da solução. 

A Microsoft recomenda que você siga este roteiro para proteger o acesso privilegiado contra os adversários determinados. Você pode ajustar este roteiro para acomodar seus recursos existentes e requisitos específicos nas organizações.

> [!NOTE]
> Proteger o acesso privilegiado requer uma ampla variedade de elementos, incluindo componentes técnicos (defesas de host, proteções de conta, gerenciamento de identidade, etc.), bem como alterações em processos e práticas administrativas e conhecimento. As linhas do tempo para o roteiro são aproximadas e são baseadas em nossa experiência com implementações de clientes. A duração variará em sua organização, dependendo da complexidade do ambiente e dos processos de gerenciamento de alterações.

## <a name="phase-1-quick-wins-with-minimal-operational-complexity"></a>Fase 1: Rápido ganhos com complexidade operacional mínima

A fase 1 do roteiro se concentra em reduzir rapidamente as técnicas de ataque mais usadas para roubo e abuso de credenciais. A fase 1 foi projetada para ser implementado em aproximadamente 30 dias e é ilustrado neste diagrama:

![Diagrama da fase 1: 1. Administrador separado e conta de usuário, 2. Senhas de administrador local just-in-time, 3. Estação de trabalho do administrador fase 1, 4. Detecção de ataque de identidade](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

### <a name="1-separate-accounts"></a>1. Separar contas

Para ajudar a separar os riscos da Internet (ataques de phishing, navegação na Web) de contas com acesso privilegiado, crie uma conta dedicada para todos os funcionários com acesso privilegiado. Os administradores não devem navegar na Web, verificar emails e realizar tarefas de produtividade cotidianas com contas altamente privilegiadas. Mais informações sobre isso podem ser encontradas na seção [Separar contas administrativas](securing-privileged-access-reference-material.md#separate-administrative-accounts) do documento de referência.

Siga as orientações no artigo [Gerenciar contas de acesso de emergência no Azure AD](/azure/active-directory/users-groups-roles/directory-emergency-access) para criar pelo menos duas contas de acesso de emergência, com direitos de administrador atribuídos permanentemente, em seus ambientes do AD local e do Azure AD. Essas contas deverão ser usadas apenas quando as contas de administrador tradicionais não puderem executar uma tarefa necessária, como no caso de um desastre.

### <a name="2-just-in-time-local-admin-passwords"></a>2. Senhas de administrador local just-in-time

Para reduzir o risco de um adversário roubar um hash de senha de conta de administrador local do banco de dados SAM local e usá-lo indevidamente para atacar outros computadores, as organizações devem garantir que cada computador tenha uma senha de administrador local exclusiva. A ferramenta LAPS (Solução de Senha de Administrador Local) pode configurar senhas aleatórias exclusivas em cada estação de trabalho e servidor e armazená-las no AD (Active Directory) protegidas por uma ACL. Somente usuários autorizados qualificados podem ler ou solicitar a redefinição dessas senhas de conta de administrador local. Você pode obter a LAPS para uso em estações de trabalho e servidores do [Centro de Download da Microsoft](https://aka.ms/LAPS).

Diretrizes adicionais para operar um ambiente com LAPS e PAWs podem ser encontradas na seção [Padrões operacionais com base no princípio de código-fonte limpo](securing-privileged-access-reference-material.md#operational-standards-based-on-clean-source-principle).

### <a name="3-administrative-workstations"></a>3. Estações de trabalho administrativas

Como medida de segurança inicial para os usuários com privilégios administrativos do Azure Active Directory e do Active Directory local tradicional, verifique se eles estão usando dispositivos Windows 10 configurados com os [Padrões para um dispositivo Windows 10 altamente seguro](/windows-hardware/design/device-experiences/oem-highly-secure). Contas de administrador privilegiado não devem ser membros do grupo de administradores locais das estações de trabalho administrativas.  A elevação de privilégios por meio do UAC (Controle de Acesso do Usuário) pode ser utilizada quando são necessárias alterações de configuração nas estações de trabalho.  Além disso, a Linha de Base de Segurança do Windows 10 deve ser aplicada às estações de trabalho para proteger ainda mais o dispositivo.

### <a name="4-identity-attack-detection"></a>4. Detecção de ataque de identidade

A [ATP (Proteção Avançada contra Ameaças) do Azure](/azure-advanced-threat-protection/what-is-atp) é uma solução de segurança baseada em nuvem que identifica, detecta e ajuda você a investigar ameaças avançadas, identidades comprometidas e ações de um funcionário mal-intencionado direcionadas ao ambiente do Active Directory local.

## <a name="phase-2-significant-incremental-improvements"></a>Fase 2: Melhorias incrementais significativas

A fase 2 desenvolve o trabalho feito na fase 1 e foi projetada para ser concluída em aproximadamente 90 dias. As etapas desse estágio são ilustradas no diagrama:

![Diagrama da fase 2: 1. Modo Windows Hello para Empresas / MFA, 2. Distribuição de PAW, 3. Privilégios just-in-time, 4. Credential Guard, 5. Credenciais vazadas, 6. Detecção de vulnerabilidades de movimento lateral](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

### <a name="1-require-windows-hello-for-business-and-mfa"></a>1. Exigir Windows Hello para Empresas e MFA

Os administradores podem se beneficiar da facilidade de uso associada ao Windows Hello para Empresas. Os administradores podem substituir suas senhas complexas por uma forte autenticação de dois fatores em seus computadores. Um invasor precisa ter o dispositivo e as informações biométricas ou o PIN, e é muito mais difícil conseguir acesso sem o conhecimento do funcionário. Mais detalhes sobre o Windows Hello para Empresas e o caminho para distribuição podem ser encontrados no artigo [Visão geral do Windows Hello para Empresas](/windows/security/identity-protection/hello-for-business/hello-overview)

Habilite a MFA (autenticação multifator) para suas contas de administrador no Azure AD usando a MFA do Azure. No mínimo, habilite a [política de acesso condicional da proteção de linha de base](/azure/active-directory/conditional-access/baseline-protection#require-mfa-for-admins). Mais informações sobre a autenticação multifator do Azure podem ser encontradas no artigo [Implantar a Autenticação Multifator do Azure baseada em nuvem](/azure/active-directory/authentication/howto-mfa-getstarted)

### <a name="2-deploy-paw-to-all-privileged-identity-access-account-holders"></a>2. Implantar PAW para todos os titulares de conta de acesso de identidade privilegiada

Continuando o processo de separar contas privilegiadas de ameaças encontradas em email, navegação na Web e outras tarefas não administrativas, você deve implementar PAWs (estações de trabalho de acesso privilegiado) dedicadas para todos os funcionários com acesso privilegiado ao seu sistemas de informações da organização. Diretrizes adicionais para a implantação de PAWs podem ser encontradas no artigo [Estações de trabalho com acesso privilegiado](privileged-access-workstations.md#paw-phased-implementation).

### <a name="3-just-in-time-privileges"></a>3. Privilégios just-in-time

Para reduzir o tempo de exposição de privilégios e aumentar a visibilidade de seu uso, forneça privilégios JIT (just in time) usando uma solução apropriada, como as descritas abaixo ou outras soluções de terceiros:

* Para o AD DS (Active Directory Domain Services), use o recurso [PAM (Privileged Access Manager)](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services) do MIM (Microsoft Identity Manager).
* Para o Azure Active Directory, use o recurso [PIM (Privileged Identity Management) do Azure AD](/azure/active-directory/privileged-identity-management/pim-deployment-plan).

### <a name="4-enable-windows-defender-credential-guard"></a>4. Habilitar o Windows Defender Credential Guard

Habilitar o Credential Guard ajuda a proteger hashes de senha NTLM, tíquetes de concessão de tíquete Kerberos e credenciais armazenadas por aplicativos como credenciais de domínio. Essa funcionalidade ajuda a evitar ataques de roubo de credenciais, como Pass-the-Hash ou Pass-the-Ticket, aumentando a dificuldade de dinamizar o ambiente usando credenciais roubadas. Informações sobre como o Credential Guard funciona e como implantá-lo podem ser encontradas no artigo [Proteger credenciais de domínio derivadas com o Windows Defender Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard).

### <a name="5-leaked-credentials-reporting"></a>5. Relatórios de credenciais vazadas

"Todos os dias, a Microsoft analisa mais de 6,5 trilhões de sinais para identificar ameaças emergentes e proteger os clientes" – [Microsoft By the Numbers](https://news.microsoft.com/bythenumbers/cyber-attacks)

Habilite a Proteção de Identidade do Microsoft Azure AD para relatar os usuários com credenciais vazadas para que você possa corrigi-las. O [Azure AD Identity Protection](/azure/active-directory/identity-protection/index) pode ser utilizado para ajudar sua organização a proteger ambientes de nuvem e híbridos contra ameaças.

### <a name="6-azure-atp-lateral-movement-paths"></a>6. Caminhos de movimento lateral do ATP do Azure

Verifique se os detentores de conta de acesso privilegiado estão usando o PAW apenas para administração de modo que contas não privilegiadas comprometidas não possam obter acesso a uma conta privilegiada por meio de ataques de roubo de credenciais, como Pass-the-Hash ou Pass-The-Ticket. [Os LMPs (caminhos de movimento lateral](/azure-advanced-threat-protection/use-case-lateral-movement-path)) do ATP do Azure fornecem relatórios fáceis de entender para identificar em que locais contas privilegiadas podem estar suscetíveis a serem comprometidas.

## <a name="phase-3-security-improvement-and-sustainment"></a>Fase 3: Melhoria e manutenção da segurança

A fase 3 do roteiro se baseia nas etapas seguidas nas Fases 1 e 2 para reforçar sua postura de segurança. A Fase 3 é descrita visualmente neste diagrama:

![Fase 3: 1. Examine o RBAC, 2. Reduzir superfícies de ataque, 3. Integrar logs com SEIM, 4. Automação de credenciais vazadas](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Essas funcionalidades se basearão nas etapas das fases anteriores e moverão suas defesas para uma postura mais proativa. Essa fase não tem uma linha do tempo específica e pode demorar mais para ser implementada dependendo de sua organização individual.

### <a name="1-review-role-based-access-control"></a>1. Examinar o controle de acesso baseado em função

Usando o modelo de três camadas descrito no artigo [Modelo de nível administrativo do Active Directory](securing-privileged-access-reference-material.md), examine e garanta que os administradores de nível inferior não tenham acesso administrativo a recursos de nível superior (associações de grupo, ACLs em contas de usuário etc.).

### <a name="2-reduce-attack-surfaces"></a>2. Reduzir superfícies de ataque

Proteja suas cargas de trabalho de identidade incluindo Domínios, Controladores de Domínio, ADFS e Azure AD Connect, uma vez que comprometer um desses sistemas pode resultar no comprometimento de outros sistemas em sua organização. Os artigos [Como reduzir a superfície de ataque do Active Directory](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md) e [Cinco etapas para proteger sua infraestrutura de identidade](/azure/security/azure-ad-secure-steps) apresentam diretrizes para proteger seus ambientes de identidades locais e híbridos.

### <a name="3-integrate-logs-with-siem"></a>3. Integrar logs com o SIEM

A integração do registro em log em uma ferramenta SIEM centralizada pode ajudar sua organização a analisar, detectar e responder a eventos de segurança. Os artigos [Como monitorar o Active Directory quanto a sinais de comprometimento](../ad-ds/plan/security-best-practices/monitoring-active-directory-for-signs-of-compromise.md) e [Apêndice L: eventos a serem monitorados](../ad-ds/plan/appendix-l--events-to-monitor.md) fornecem orientação sobre eventos que devem ser monitorados em seu ambiente.

Isso faz parte do plano acima, pois, para agregar, criar e ajustar alertas em um SIEM (gerenciamento de evento e informações de segurança), são necessários analistas capacitados (ao contrário do ATP do Azure no plano de 30 dias, que inclui alertas prontos para uso)

### <a name="4-leaked-credentials---force-password-reset"></a>4. Credenciais vazadas – forçar redefinição de senha

Continue a aprimorar sua postura de segurança habilitando o Azure AD Identity Protection para forçar automaticamente redefinições de senha quando houver suspeita de comprometimento das senhas. As diretrizes encontradas no artigo [Usar eventos de risco para disparar a Autenticação Multifator e as alterações de senha](/azure/active-directory/authentication/tutorial-risk-based-sspr-mfa) explica como habilitar isso usando uma política de acesso condicional.

## <a name="am-i-done"></a>Isso é tudo?

A resposta curta é não.

Pessoas mal-intencionadas nunca param, assim, você também não pode parar. Este roteiro pode ajudar sua organização a proteger-se contra ameaças atualmente conhecidas conforme os invasores evoluem e mudam constantemente. Recomendamos que você veja a segurança como um processo contínuo com foco no aumento do custo e na redução da taxa de sucesso de adversários que visam o seu ambiente.

Embora não seja a única parte do programa de segurança de sua organização, proteger o acesso privilegiado é um componente crítico de sua estratégia de segurança.
