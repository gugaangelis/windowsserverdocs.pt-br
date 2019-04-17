---
title: Proteger o acesso privilegiado
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: eb83903204b00ef6c1eb116554ec54bc2211a399
ms.sourcegitcommit: 7b01b54032ec56432116626e08fbd92508c3a7d5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2018
---
# <a name="securing-privileged-access"></a>Proteger o acesso privilegiado

>Aplica-se a: Windows Server 2016

Protegendo privilegiados acesso é a primeira etapa crítica para estabelecer garantias de segurança para ativos de negócios em uma organização moderna. A segurança da maioria dos ou todos os ativos de negócios em uma organização depende a integridade das contas privilegiadas que administrar e gerenciar os sistemas de TI. Invasores cibernéticos estão direcionando essas contas e outros elementos de acesso privilegiado acessem rapidamente dados direcionados e sistemas que usam os ataques de roubo de credenciais como [Pass-the-Hash e Pass-the-Ticket](https://www.microsoft.com/pth).

Proteger o acesso administrativo contra determinados adversários exigem uma abordagem completa e bem pensada para isolar esses sistemas contra riscos. Esta figura mostra os três estágios de recomendações para separar e proteger administração neste mapa:

![Diagrama dos três estágios de recomendações para separar e proteger este mapa de administração](../media/securing-privileged-access/PAW_LP_Fig1.JPG)

Objetivos de mapa:

-   **Planeje semana de 2 a 4**: rapidamente mitigar as técnicas de ataque mais usados

-   **Planeje mês de 1 a 3**: compilar visibilidade e controle de atividade de administrador

-   **plano de 6 + mês**: continuar a criação de defesas para uma postura de segurança mais proativa

A Microsoft recomenda que você siga este mapa para proteger o acesso privilegiado contra determinados adversários. Você pode ajustar essa estratégia para acomodar suas necessidades específicas em suas organizações e os recursos existentes.

> [!NOTE]
> Privilégios para proteger o acesso necessário uma ampla variedade de elementos, incluindo componentes técnicos (as defesas do host, proteções de conta, gerenciamento de identidade, etc.), bem como alterações para processar e práticas administrativas e dados de Conhecimento.

## <a name="why-is-securing-privileged-access-important"></a>Por que a proteção de acesso privilegiado é importante?
Na maioria das organizações, a segurança da maioria dos ou todos os ativos de negócios depende a integridade das contas privilegiadas que administrar e gerenciar os sistemas de TI. Invasores cibernéticos são enfocando privilegiado acessem sistemas como dados específicos do Active Directory para acessar rapidamente a todos de uma organização.

Abordagens de segurança tradicionais concentraram sobre como usar os pontos de entrada e saída de uma rede de organizações como o perímetro de segurança principal, mas a eficácia de segurança de rede tem diminuída significativamente por duas tendências:

-   As organizações são hospedar dados e recursos fora do limite de rede tradicional no mobile enterprise computadores, dispositivos como celulares e tablets, serviços de nuvem e dispositivos BYOD

-   Adversários demonstrou uma capacidade contínua e consistente para obter acesso em estações de trabalho dentro do limite de rede por meio de phishing e outros ataques na web e email.

A substituição natural para o perímetro da rede de segurança em uma empresa moderna complexa é os controles de autenticação e autorização na camada de identidade de uma organização. Contas com privilégios administrativas efetivamente estão no controle desse novo "perímetro da segurança" Portanto, é fundamental para proteger o acesso privilegiado:

![Diagrama mostrando a camada de identidade de uma organização](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

Um adversário que assuma o controle de uma conta administrativa pode usar esses privilégios para buscar seu ganho à custa da organização de destino, conforme mostrado abaixo:

![Diagrama mostrando como um adversário que assuma o controle de uma conta administrativa pode usar esses privilégios para buscar seu ganho à custa da organização de destino](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

Para obter mais informações sobre os tipos de ataques que normalmente levam para invasores no controle de contas de administrador, visite o [Pass The Hash web site](https://www.microsoft.com/pth) informativas white papers, vídeos e muito mais.

Esta figura mostra o "canal" separado para administração que estabelece o mapa para isolar tarefas com privilégios de acesso de tarefas de usuário padrão de alto risco como web navegação e acessar o email.

![Diagrama mostrando o "canal" separado para administração se o mapa estabelece para isolar tarefas com privilégios de acesso de tarefas de usuário padrão de alto risco como web de navegação e acessando email](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

Porque o adversário pode obter o controle de acesso privilegiado usando uma variedade de métodos, reduzindo esse risco requer uma abordagem de technical holística e detalhada, conforme descrito neste mapa. O mapa irá isolar e proteger os elementos em seu ambiente que permitem acesso privilegiado criando mitigações em cada área da coluna defesa nessa figura:

![Tabela mostrando colunas de ataque e defesa](../media/securing-privileged-access/PAW_LP_Fig5.JPG)

## <a name="security-privileged-access-roadmap"></a>Mapa de acesso de segurança privilegiado
O mapa foi projetado para maximizar o uso de tecnologias que você já pode ser implantado, tirar proveito das tecnologias de segurança atuais e futuras e integrar qualquer 3ª ferramentas de segurança de terceiros que você talvez já tenha implantado.

O mapa de recomendações da Microsoft é dividido em 3 etapas:

-   Planeje semana de 2 a 4 - rapidamente mitigar as técnicas de ataque mais usados

-   Planeje mês de 1 a 3 - compilação visibilidade e controle de atividade de administrador

-   6 + mês planejar - continuar a criação de defesas para uma postura de segurança mais proativa

Cada estágio do mapa foi projetado para aumentar o custo e a dificuldade para adversários privilegiado acesso para seu local de ataque e ativos de nuvem. O mapa é priorizado para agendar a mais eficiente e as implementações mais rápidas primeiro com base em nossas experiências com esses ataques e implementação da solução.

> [!NOTE]
> Os cronogramas para o mapa são aproximados e são baseados em nossa experiência com implementações do cliente. A duração variam em sua organização, dependendo da complexidade do seu ambiente e seus processos de gerenciamento de alteração.

### <a name="security-privileged-access-roadmap-stage-1"></a>Segurança privilegiados mapa de acesso: Estágio 1
Estágio 1 do mapa está voltado para reduzir rapidamente as técnicas de ataque mais usados de roubo de credenciais e o abuso. Estágio 1 foi projetado para ser implementada em aproximadamente 2 a 4 semanas e é mostrado neste diagrama:

![Figura mostrando esse estágio 1 foi projetada para ser implementada em aproximadamente 2 a 4 semanas](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

Estágio 1 do mapa de acesso de segurança privilegiado inclui estes componentes:

**1. separar a conta de administrador para tarefas de administração**

Para ajudar a separar os riscos de internet (ataques de phishing, navegação na web) de privilégios administrativos, criar uma conta dedicada para todas as pessoas com privilégios administrativos. Diretrizes sobre isso está incluído nas instruções PATA publicadas [aqui](http://Aka.ms/CyberPAW).

**2. privilegiados acesso estações de trabalho (patas) fase 1: Os administradores do Active Directory**

Para ajudar a separar os riscos de internet (ataques de phishing, navegação na web) de privilégios de administrador do domínio, criar estações de trabalho dedicado acesso privilegiado (patas) para a equipe com privilégios administrativos do AD. Isso é a primeira etapa de um programa PATA e é a fase 1 do guia publicado [aqui](http://Aka.ms/CyberPAW).

**3. senhas de Administrador Local exclusivo para estações de trabalho**

**4. senhas de Administrador Local exclusivo para servidores**

Para atenuar o risco de um adversário roubando um hash de senha de conta de administrador local do banco de dados SAM local e abuso-lo para atacar outros computadores, você deve usar a ferramenta VOLTAS para configurar senhas aleatórias exclusivas em cada estação de trabalho e servidor e registre essas senhas no Active Directory. Você pode obter a solução de senha de Administrador Local para uso em estações de trabalho e servidores [aqui](http://Aka.ms/LAPS).

Diretrizes adicionais para operar um ambiente com VOLTAS e patas podem ser encontrados [aqui](http://aka.ms/securitystandards).

### <a name="security-privileged-access-roadmap-stage-2"></a>Segurança privilegiados mapa de acesso: Estágio 2
Estágio 2 complementa as atenuações de estágio 1 e é projetado para ser implementadas em cerca de 1 a 3 meses. As etapas deste estágio são descritas neste diagrama:

![Diagrama mostrando as etapas da fase 2](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

**1. PATA fases 2 e 3: todos os administradores e proteção adicional**

Para separar os riscos de internet de todas as contas administrativas privilegiadas, continue com a PATA você começou no estágio 1 e implemente estações de trabalho dedicadas para todas as pessoas com acesso privilegiado. Este mapas a fase 2 e 3 do guia publicado [aqui](http://Aka.ms/CyberPAW).

**2. associadas ao tempo privilégios (não administradores permanentes)**

Para reduzir o tempo de exposição de privilégios e aumentar a visibilidade de seu uso, fornecem privilégios apenas in-time (JIT) usando uma solução apropriada, como aqueles abaixo:

-   Para serviços Active Directory domínio (AD DS), use o Microsoft Identity Manager (MIM) do [Manager com privilégios de acesso (PAM)](https://technet.microsoft.com/en-us/library/mt150258.aspx) funcionalidade.

-   Para o Active Directory do Azure, use [Azure AD privilegiados identidade PIM (gerenciamento)](http://aka.ms/AzurePIM) funcionalidade.

**3. multifator elevação associadas ao tempo**

Para aumentar o nível de garantia de autenticação de administrador, você deve exigir autenticação multifator antes de conceder privilégios.
Isso pode ser feito com MIM PAM e PIM do Azure AD usando Azure a autenticação multifator (MFA).

**4. suficientes administrador (JEA) para manutenção de DC**

Para reduzir a quantidade de contas com privilégios de administração de domínio e a exposição de risco associado, use o recurso apenas suficiente administração (JEA) no PowerShell para executar operações comuns de manutenção em controladores de domínio. A tecnologia JEA permite que os usuários específicos realizar tarefas administrativas específicas em servidores (como controladores de domínio), sem dar a eles direitos de administrador. Baixe este guia de [TechNet](http://aka.ms/JEA).

**5. menor superfície de ataque do domínio e controladores de domínio**

Para reduzir oportunidades para adversários assuma o controle de uma floresta, você deve reduzir os caminhos que um invasor pode levar para assumir o controle de controladores de domínio ou objetos no controle do domínio. Siga as orientações para reduzir esse risco publicado [aqui](http://aka.ms/HardenAD).

**6. detecção de ataque**

Para obter visibilidade no roubo de credenciais active e identidade ataques para que você possa responder rapidamente a eventos e conter danos, implantar em configurar [Microsoft Advanced ameaça análises (ATA)](https://www.microsoft.com/ata).

Antes de instalar ATA, você deve garantir que um processo para lidar com um incidente de segurança crítico que ATA pode detectar.

-   Para obter mais informações sobre como configurar um processo de resposta a incidentes, consulte [respondendo a incidentes de segurança de TI](https://aka.ms/irr) e o "responder às atividades suspeitas" e "Recuperar de uma violação" seções de [Mitigating Pass-the-Hash e outros roubo de credenciais](https://www.microsoft.com/pth), versão 2.

-   Para obter mais informações sobre envolvente serviços da Microsoft para ajudar na preparação de seu processo de Infravermelho ATA gerado eventos e implantando ATA, entre em contato com seu representante da Microsoft, acessando [nesta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

-   Acesso [nesta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) para obter mais informações sobre envolvente serviços da Microsoft para ajudar na investigação e recuperar um incidente

-   Para implementar ATA, siga o guia de implantação disponível [aqui](http://aka.ms/ata).

### <a name="security-privileged-access-roadmap-stage-3"></a>Segurança privilegiados mapa de acesso: Estágio 3
Estágio 3 do mapa complementa as atenuações de estágios 1 e 2 para fortalecer e adicione mitigações em toda a gama. Estágio 3 é mostrado visualmente neste diagrama:

![Diagrama mostrando estágio 3](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

Esses recursos serão basear as atenuações de fases anteriores e mover suas defesas em uma postura mais proativa.

**1. modernizar funções e modelo de delegação**

Para reduzir o risco de segurança, você deve recriar todos os aspectos de seu funções e o modelo de delegação para estar em conformidade com as regras do modelo de nível, acomodar funções administrativas do serviço de nuvem e incorpore a usabilidade de administrador como um princípio-chave. Este modelo deve aproveitar os recursos JIT e JEA implantados em estágios anteriores, bem como tecnologia de automação de tarefa para alcançar essas metas.

**2. Smartcard ou autenticação do Passport para todos os administradores**

Para aumentar o nível de garantia e a usabilidade de autenticação de administrador, você deve exigir autenticação forte para todas as contas administrativas hospedadas no Active Directory do Azure e no Windows Server Active Directory (incluindo contas federadas para um serviço de nuvem).

**3. Admin floresta para administradores do Active Directory**

Para fornecer a proteção mais forte para administradores do Active Directory, configure um ambiente que não tem segurança dependências em seu Active Directory de produção e é isolado do, mas ataques de todos os sistemas mais confiáveis em seu ambiente de produção. Para obter mais informações sobre a arquitetura ESAE visite [nesta página](http://aka.ms/esae).

**4. política de integridade de código para controladores de domínio (servidor de 2016)**

Para limitar o risco de programas não autorizados nos controladores de domínio de operações de ataque do adversário e inadvertidos erros administrativos, configure a integridade de código do Windows Server 2016 para kernel (drivers) e o modo de usuário (aplicativos) para permitir apenas executáveis autorizados sejam executados no computador.

**5. protegidas VMs para controladores de domínio virtuais (servidor 2016 Hyper-V malha)**

Para proteger controladores de domínio virtualizado de vetores de ataque que explorar perda inerente de uma máquina virtual de segurança física, use esse novo recurso de 2016 Hyper-V Server para ajudar a evitar o roubo de segredos do Active Directory de controladores de domínio Virtual. Usando essa solução, você pode criptografar geração 2 VMs para proteger os dados VM contra violação por administradores de rede e armazenamento, roubo e inspeção, bem como proteger o acesso à VM contra ataques de administradores de host do Hyper-V.

## <a name="am-i-done"></a>É só isso?
Concluir este mapa obterá você proteções de acesso privilegiado forte para os ataques que estão atualmente adversários conhecidos e disponíveis para hoje. Infelizmente, as ameaças à segurança serão constantemente evoluem e shift, portanto, recomendamos que você exibir segurança como um processo contínuo voltada para acionar o custo e reduzir a taxa de êxito de adversários direcionando seu ambiente.

Protegendo privilegiados acesso é a primeira etapa crítica para estabelecer garantias de segurança para ativos de negócios em uma organização moderna, mas não é a única parte de um programa de segurança completo que inclua elementos como política, operações, segurança das informações, servidores, aplicativos, computadores, dispositivos, estrutura de nuvem e outros componentes fornecem as garantias de segurança que exige que você.

Para obter mais informações sobre como criar uma estratégia de segurança completa, consulte a seção "responsabilidades do cliente e mapa" de nuvem do Microsoft Security para documento empresarial arquitetos disponível [aqui](http://aka.ms/securecustomer).

Para obter mais informações sobre envolvente serviços da Microsoft para ajudar com qualquer um destes tópicos, entre em contato com seu representante da Microsoft ou visite [nesta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="related-topics"></a>Tópicos relacionados
[Conheça das principais: atenuar Pass-the-Hash e outras formas de roubo de credenciais](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft Advanced Analytics ameaça](http://aka.ms/ata)

[Proteger credenciais de domínio derivadas com o Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx)

[Visão geral do Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)

[Proteger ativos de alto valor com estações de trabalho Administração segura](https://msdn.microsoft.com/en-us/library/mt186538.aspx)

[Modo de usuário isolado no Windows 10 com Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Isolado processos do modo de usuário e recursos no Windows 10 com Logan Gabriel (Channel 9)](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Mais sobre processos e recursos no modo de usuário isolado do Windows 10 com Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Minimizando o roubo de credenciais usando o Windows 10 modo de usuário isolado (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Habilitando a validação KDC rigorosa no Windows Kerberos](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Novidades na autenticação Kerberos para Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Garantia do mecanismo de autenticação para AD DS no guia passo a passo do Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Módulo de plataforma confiável](https://docs.microsoft.com/en-us/windows/device-security/tpm/trusted-platform-module-overview)
