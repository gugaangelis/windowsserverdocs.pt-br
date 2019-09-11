---
title: Como iniciar sua jornada pelo Regulamento de Proteção Geral de Dados (GDPR) para Windows Server 2016
description: Use este artigo para entender o que é GDPR e sobre os produtos que a Microsoft fornece para ajudá-lo a seguir em direção a conformidade.
ms.technology: techgroup-security
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
author: nirb-ms
ms.openlocfilehash: 506cd5cb44d93c9d7d221917505f76a2c5625baa
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870552"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>Iniciando sua jornada de Regulamento Geral sobre a Proteção de Dados (GDPR) para o Windows Server 

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este artigo fornece informações sobre o GDPR, incluindo o que é, e os produtos que a Microsoft fornece para ajudar você a entrar em conformidade.

## <a name="introduction"></a>Introdução
Em 25 de maio de 2018, uma lei de privacidade Europeia entrará em vigor e estabelecerá um novo patamar global para direitos de privacidade, segurança e conformidade.

O Regulamento Geral sobre a Proteção de Dados, ou GDPR, é fundamentalmente sobre proteger e habilitar os direitos de privacidade dos indivíduos. O GDPR estabelece os requisitos de privacidade globais rígidos que regem como gerenciar e proteger dados pessoais enquanto respeita a escolha individual — não importa onde dados são enviados, processados ou armazenados.

A Microsoft e nossos clientes agora estão em uma viagem para alcançar as metas de privacidade do GDPR. Na Microsoft, acreditamos que a privacidade é um direito fundamental, e também acreditamos que o GDPR seja uma etapa importante para esclarecer e habilitar os direitos de privacidade individuais. Mas também reconhecemos que o GDPR irá exigir mudanças significativas das organizações em todo o mundo.

Nós destacamos nosso comprometimento ao GDPR e como estamos oferecendo suporte aos nossos clientes dentro de nossa postagem no blog [Ficar em conformidade com GDPR com Microsoft Cloud](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99) escrita por nosso Chefe de privacidade de escritório [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/) e a postagem de blog [Ganhando sua confiança com comprometimentos contratuais com o Regulamento de Proteção de Dados Geral](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99)” escrita por [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/) - Vice-Presidente da Microsoft Corporate e Conselheiro Geral do Deputado.

Embora sua jornada em direção à conformidade com GDPR possa parecer um desafio, estamos aqui para ajudá-lo. Para obter informações específicas sobre o GDPR, nossos compromissos e como começar sua jornada, visite a [seção GDPR do Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="gdpr-and-its-implications"></a>GDPR e suas implicações
O GDPR é um estatuto complexo que pode exigir alterações significativas na forma como você coleta, usa e gerencia dados pessoais. A Microsoft tem um longo histórico de ajudar nossos clientes a cumprirem as normas complexas e quando se trata de se preparar para o GDPR, somos seu parceiro nesta jornada.

O GDPR impõe regras em organizações que oferecem bens e serviços para pessoas na União Europeia (UE), ou que coletam e analisem dados vinculados a residentes da UE, não importa onde essas empresas estão localizadas. Entre os principais elementos do GDPR, estão os seguintes:

- **Direitos de privacidade pessoal aprimorados.** Proteção de dados reforçada para residentes da UE, garantindo que eles tenham o direito de acessar seus dados pessoais, para corrigir imprecisões nesses dados, para apagar dados, para contestar o processamento de seus dados pessoais e para movê-los.

- **Maior imposto para proteção de dados pessoais.** Responsabilidade reforçada de organizações que processam dados pessoais, fornecendo maior clareza sobre responsabilidade em garantir a conformidade.

- **Relatórios de violação de dados pessoais obrigatórios.** As organizações que controlam os dados pessoais são necessárias para relatar violações de dados pessoais que representam um risco para os direitos e a liberdade de indivíduos para suas autoridades de supervisão sem atrasos desnecessários e, onde possível, não depois de 72 horas após ficarem cientes da violação.

Como você pode prever, o GDPR pode ter um impacto significativo em seus negócios, potencialmente exigindo que você atualize as políticas de privacidade, implemente e fortaleça controles de proteção de dados e viole procedimentos de notificação, implante políticas altamente transparentes, e invista ainda mais em IT e treinamento. Microsoft Windows 10 pode ajudá-lo efetivamente e lidar com alguns desses requisitos com eficiência.

## <a name="personal-and-sensitive-data"></a>Dados pessoais e confidenciais
Como parte de seus esforços para estar em conformidade com a GDPR, você precisará entender como a norma define dados pessoais e confidenciais e como essas definições se relacionam aos dados mantidos por sua organização. Com base nessa compreensão, você poderá descobrir onde os dados são criados, processados, gerenciados e armazenados.

O GDPR considera dados pessoais como quaisquer informações relacionadas a uma pessoa identificável ou identificada naturalmente. Que podem incluir identificação direta (como seu nome legal) e identificação indireta (como informações específicas que tornam claro que é você as referências de dados). O GDPR também deixa claro que o conceito de dados pessoais inclui identificadores online (como, endereços IP, IDs de dispositivo móvel) e dados de localização.

O GDPR apresenta definições específicas para dados genéticos (como, a sequência de gene de um indivíduo) e dados biométricos. Dados genéticos e dados biométricos junto com outras subcategorias de dados pessoais (dados pessoais que revelam Racials ou étnicas, opiniões políticas, religiosas ou filosofia crenças ou associação de União comercial: dados relacionados à integridade; ou dados relacionados a um o sexo da vida útil da pessoa ou a orientação sexual) são tratados como dados pessoais confidenciais sob o GDPR. Os dados pessoais confidenciais são proteções aprimoradas e geralmente exigem o consentimento explícito de um indivíduo em que esses dados devem ser processados.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Exemplos de informações referentes a uma pessoa natural identificada ou identificável (dados sujeitos)
Esta lista fornece exemplos de vários tipos de informações que serão controladas por meio de GDPR. Esta não é uma lista cansativa.

-   Nome

-   Número de identificação (como SSN)

-   Dados de localização (por exemplo, endereço residencial)

-   Identificador online (como endereço de email, nomes de tela, endereço IP, IDs de dispositivo)

-   Dados pseudoanônimos (por exemplo, usando uma chave para identificar indivíduos)

-   Dados genéticos (como amostras biológicas de outra pessoa)

-   Dados biométricos (como impressões digitais, reconhecimento facial)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Guia de introdução sobre a jornada até a conformidade de GDPR
Dada a quantidade é complicado para se tornar GDPR compatível, é altamente recomendável que você não esperar para preparar até que começa de imposição. Você deve revisar sua privacidade e as práticas de gerenciamento de dados agora. Recomendamos que você comece sua jornada de conformidade GDPR, concentrando-se em quatro etapas principais:

-   **Descobri.** Identifique quais dados pessoais que você tem e onde estão. 

-   **Capacidade.** Gerencie como dados pessoais são usados e acessados.

-   **Protegendo.** Estabelece controles de segurança para evitar, detectar e responder a vulnerabilidades e violações de dados.  

-   **Relatar.** Age em solicitações de dados, violações de dados de relatório e mantém a documentação necessária.

    ![Diagrama sobre como as 4 etapas principais do GDPR funcionam juntas](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Para cada uma das etapas, criamos recursos em várias soluções da Microsoft, que podem ser usados para ajudá-lo a atender os requisitos dessa etapa, recursos e ferramentas de exemplo. Embora este artigo não seja um guia abrangente de "instruções", incluímos links para você encontrar mais detalhes e mais informações estão disponíveis na [seção GDPR da central de confiabilidade da Microsoft](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Segurança e privacidade do Windows Server
O GDPR exige que você implemente medidas de segurança técnicas e organizacionais apropriadas para proteger sistemas de processamento e dados pessoais. No contexto do GDPR, seus ambientes de servidor físico e virtual estão potencialmente processando dados pessoais e confidenciais. O processamento pode significar qualquer operação ou conjunto de operações, como a coleta de dados, o armazenamento e a recuperação.

Sua capacidade de atender a esse requisito e implementar medidas de segurança técnicas apropriadas deve refletir as ameaças que você enfrenta no ambiente de ti cada vez mais hostil de hoje. O panorama de ameaças de segurança de hoje é uma das ameaças agressivas e tenaciouss. Nos anos anteriores, invasores mal-intencionados se concentravam principalmente em receber o reconhecimento da comunidade por meio de seus ataques ou a emoção de colocar temporariamente um sistema offline. Desde então, as motivaçãos do invasor mudaram para a criação de dinheiro, incluindo dispositivos de retenção e data Hostage até que o proprietário pague o resgate exigido.

Ataques modernos cada vez mais se concentram no roubo de propriedade intelectual em grande escala; na degradação do sistema visado que pode resultar em perda financeira; e agora até mesmo o cyberterrorismo que ameaça a segurança de indivíduos, empresas e interesses nacionais em todo o mundo. Esses invasores são geralmente indivíduos altamente treinados e especialistas em segurança, alguns dos quais estão a serviço de nações que possuem grandes orçamentos e recursos humanos aparentemente ilimitados. Ameaças como essas exigem uma abordagem que pode superar esse desafio.

Essas ameaças não são apenas um risco à sua capacidade de manter o controle de quaisquer dados pessoais ou confidenciais, mas também são um risco material a sua empresa em um todo. Considere os dados recentes do McKinsey, Ponemon Institute, Verizon e Microsoft:

- O custo médio do tipo de violação de dados ao GDPR esperam que você relate US$ 3,5 milhões.

- 63% dessas violações envolvem senhas fracas ou roubadas que o GDPR espera que você resolva.

- Mais de 300.000 novas amostras de malware são criadas e espalhadas todos os dias, tornando sua tarefa de resolver a proteção de dados ainda mais desafiadora.

Como visto com os ataques de ransomware recentes, uma vez chamado de recompensa preta da Internet, os invasores estão indo além de destinos maiores que podem pagar mais, com consequências potencialmente catastróficas. O GDPR inclui penalidades que tornam seus sistemas, incluindo desktops e laptops, que contêm destinos avançados e de dados confidenciais de fato.

Dois princípios principais guiaram e continuarão a guiar o desenvolvimento do Windows:

- **Segurança.** Os dados que nossos softwares e serviços armazenam em nome de nossos clientes devem ser protegidos contra os danos e usados ou modificados apenas de maneiras apropriadas. Os modelos de segurança devem ser fáceis para os desenvolvedores entenderem e criarem em seus aplicativos.

- **Privacidade.** Os usuários devem estar no controle de como seus dados são usados. As políticas de uso de informações devem ser claras para o usuário. Os usuários devem estar no controle de quando e se receberem informações para fazer o melhor uso do seu tempo. Deve ser fácil para os usuários especificar o uso apropriado de suas informações, incluindo o controle do uso de emails que eles enviam.

A Microsoft permaneceu sólido em relação a esses princípios, conforme observado recentemente pelo CEO da Microsoft, Satya Nadella, 

> "À_medida que o mundo continua a mudar e os requisitos de negócios evoluem, algumas coisas são consistentes: a demanda de segurança e privacidade do cliente._ "

À medida que você trabalha para cumprir o GDPR, compreender a função dos seus servidores físicos e virtuais na criação, no acesso, no processamento, no armazenamento e no gerenciamento de dados que podem ser qualificados como dados pessoais e potencialmente confidenciais sob o GDPR é importante. O Windows Server fornece recursos que o ajudarão a cumprir os requisitos de GDPR para implementar medidas de segurança técnicas e organizacionais apropriadas para proteger dados pessoais.

A postura de segurança do Windows Server 2016 não é um complemento; é um princípio da arquitetura. E, pode ser melhor entendida em quatro principais:

- **Protegendo.** Foco contínuo e inovação em medidas preventivas; Bloqueie ataques conhecidos e malwares conhecidos.

- **Ocorre.** Ferramentas de monitoramento abrangentes para ajudá-lo a identificar anormalidades e responder a ataques com mais rapidez.

- **Responder.** Principais tecnologias de resposta e recuperação, além de ampla experiência em consultoria.

- **Isolar.** Isole os componentes do sistema operacional e os segredos de dados, limite os privilégios de administrador e meça rigorosamente a integridade do host.

Com o Windows Server, sua capacidade de proteger, detectar e defender contra os tipos de ataques que podem levar a violações de dados é significativamente aprimorada. Considerando requisitos rígidos que envolvem às notificações de violação dentro do GDPR, garantir que seus sistemas de desktop e laptop estão bem protegiam diminuirá os riscos decorrentes que poderiam resultar na notificação e análise de violação caras.

Na seção a seguir, você verá como o Windows Server fornece recursos que se ajustam à casa no estágio "proteger" de sua jornada de conformidade do GDPR. Esses recursos se enquadram em três cenários de proteção:

- **Proteja suas credenciais e limite os privilégios de administrador.** O Windows Server 2016 ajuda a implementar essas alterações, para ajudar a impedir que o sistema seja usado como um ponto de partida para outras invasões.

- **Proteja o sistema operacional para executar seus aplicativos e infraestrutura.** O Windows Server 2016 fornece camadas de proteção, o que ajuda a impedir que invasores externos executem software mal-intencionado ou explore vulnerabilidades.

- **Virtualização segura.** O Windows Server 2016 permite a virtualização segura, usando máquinas virtuais blindadas e malha protegida. Isso ajuda você a criptografar e executar suas máquinas virtuais em hosts confiáveis em sua malha, protegendo-os melhor contra ataques mal-intencionados.

Esses recursos, discutidos mais detalhadamente abaixo, com referências a requisitos específicos do GDPR, são criados com base na proteção avançada do dispositivo que ajuda a manter a integridade e a segurança do sistema operacional e dos dados.

Um provisionamento de chave dentro do GDPR é a proteção de dados por design e por padrão, e ajudar a sua capacidade de atender a essa provisão são os recursos do Windows 10, como a criptografia de dispositivo do BitLocker. O BitLocker usa a tecnologia Trusted Platform Module (TPM), que fornece funções relacionadas a segurança baseadas em hardware. Esse chip do processador de criptografia inclui vários mecanismos de segurança físicos para torná-lo resistente à adulteração e o software mal-intencionado não pode violar as funções de segurança do TPM.

O chip inclui vários mecanismos de segurança física para torná-lo resistente a adulterações nas funções de segurança do TPM por software mal-intencionado. Algumas das principais vantagens do uso da tecnologia TPM são a possibilidade de:

-   Gerar, armazenar e limitar o uso de chaves de criptografia.

-   Use a tecnologia TPM para autenticação de dispositivo de plataforma usando a chave RSA exclusiva do TPM, que é gravada em si mesma.

-   Ajuda a garantir a integridade da plataforma, executando e armazenando medidas de segurança.

Proteção avançada de dispositivo adicional relevante para sua operação sem violações de dados incluem a inicialização confiável do Windows para ajudar a manter a integridade do sistema, garantindo que o malware não possa iniciar antes das defesas do sistema.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Dando suporte à sua jornada de conformidade do GDPR
Os principais recursos do Windows Server podem ajudá-lo a implementar com eficiência e eficiência os mecanismos de segurança e privacidade que o GDPR exige para fins de conformidade. Embora o uso desses recursos não garanta sua conformidade, eles oferecerão suporte aos seus esforços para fazer isso.

O sistema operacional do servidor reside em uma camada estratégica na infraestrutura de uma organização, oferecendo novas oportunidades para criar camadas de proteção contra ataques que podem roubar dados e interromper seus negócios. Os principais aspectos do GDPR, como privacidade por design, proteção de dados e controle de acesso, precisam ser abordados em sua infraestrutura de ti no nível do servidor.

Trabalhando para ajudar a proteger a identidade, o sistema operacional e as camadas de virtualização, o Windows Server 2016 ajuda a bloquear os vetores de ataque comuns usados para obter acesso ilícito aos seus sistemas: credenciais roubadas, malware e uma malha de virtualização comprometida. Além de reduzir os riscos para os negócios, os componentes de segurança incorporados ao Windows Server 2016 ajudam a atender aos requisitos de conformidade para as principais normas governamentais e de segurança do setor. 

Essas proteções de identidade, de sistema operacional e de virtualização permitem que você proteja melhor seu datacenter executando o Windows Server como uma VM em qualquer nuvem e limite a capacidade dos invasores de comprometer credenciais, iniciar malware e permanecer sem detecção no seu rede. Da mesma forma, quando implantado como um host Hyper-V, o Windows Server 2016 oferece garantia de segurança para seus ambientes de virtualização por meio de máquinas virtuais blindadas e recursos de firewall distribuído. Com o Windows Server 2016, o sistema operacional do servidor se torna um participante ativo em sua segurança de datacenter.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Proteger suas credenciais e limitar os privilégios de administrador 
O controle sobre o acesso a dados pessoais e os sistemas que processam esses dados é uma área com o GDPR que tem requisitos específicos, incluindo o acesso por administradores. Identidades privilegiadas são contas que têm privilégios elevados, como contas de usuário que são membros dos administradores de domínio, administradores de empresa, administradores locais ou até mesmo grupos de usuários avançados. Essas identidades também podem incluir contas que receberam privilégios diretamente, como executar backups, desligar o sistema ou outros direitos listados no nó atribuição de direitos de usuário no console de política de segurança local.

Como princípio de controle de acesso geral e em linha com o GDPR, você precisa proteger essas identidades com privilégios de comprometimento por invasores em potencial. Primeiro, é importante entender como as identidades são comprometidas; em seguida, você pode planejar para impedir que os invasores obtenham acesso a essas identidades privilegiadas.

#### <a name="how-do-privileged-identities-get-compromised"></a>Como as identidades privilegiadas ficam comprometidas?
As identidades com privilégios podem ficar comprometidas quando as organizações não têm diretrizes para protegê-las. Veja estes exemplos:

- **Mais privilégios do que o necessário.** Um dos problemas mais comuns é que os usuários têm mais privilégios do que o necessário para executar sua função de trabalho. Por exemplo, um usuário que gerencia o DNS pode ser um administrador do AD. Geralmente, isso é feito para evitar a necessidade de configurar diferentes níveis de administração. No entanto, se essa conta for comprometida, o invasor terá privilégios elevados automaticamente.

- **Conectado constantemente com privilégios elevados.** Outro problema comum é que os usuários com privilégios elevados podem usá-lo por um tempo ilimitado. Isso é muito comum com os profissionais de ti que entram em um computador desktop usando uma conta privilegiada, permanecem conectados e usam a conta com privilégios para navegar na Web e usar email (funções típicas de trabalho de ti). A duração ilimitada de contas com privilégios torna a conta mais suscetível a ataques e aumenta as chances de que a conta seja comprometida.

- **Pesquisa de engenharia social.** A maioria das ameaças de credenciais começa pesquisando a organização e, em seguida, conduzida por meio da engenharia social. Por exemplo, um invasor pode executar um ataque de phishing por email para comprometer contas legítimas (mas não necessariamente contas com privilégios elevados) que têm acesso à rede de uma organização. O invasor usa essas contas válidas para executar pesquisas adicionais em sua rede e identificar contas com privilégios que podem executar tarefas administrativas. 

- **Aproveite contas com privilégios elevados.** Mesmo com uma conta de usuário normal e não com privilégios elevados na rede, os invasores podem obter acesso a contas com permissões elevadas. Um dos métodos mais comuns de fazer isso é usando os ataques Pass-the-hash ou Pass-the-token. Para obter mais informações sobre as técnicas Pass-the-hash e outras informações sobre roubo de credenciais, consulte os recursos na [página Pass-the-hash (PtH)](https://technet.microsoft.com/dn785092.aspx).

Há, obviamente, outros métodos que os invasores podem usar para identificar e comprometer identidades privilegiadas (com novos métodos sendo criados todos os dias). Portanto, é importante que você estabeleça práticas para que os usuários façam logon com contas com privilégios mínimos para reduzir a capacidade dos invasores de obter acesso a identidades privilegiadas. As seções abaixo descrevem a funcionalidade em que o Windows Server pode mitigar esses riscos.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Just-in-time admin (JIT) e apenas administrador suficiente (JEA)
Embora a proteção contra ataques Pass-the-hash ou Pass-the-ticket seja importante, as credenciais de administrador ainda podem ser roubadas por outros meios, incluindo engenharia social, funcionários descontentes e força bruta. Portanto, além de isolar as credenciais o máximo possível, você também deseja uma maneira de limitar o alcance de privilégios de nível de administrador, caso eles sejam comprometidos.

Hoje, muitas contas de administrador são excessivamente privilegiadas, mesmo que tenham apenas uma área de responsabilidade. Por exemplo, um administrador de DNS, que exige um conjunto muito estreito de privilégios para gerenciar servidores DNS, geralmente recebe privilégios de nível de administrador de domínio. Além disso, como essas credenciais são concedidas para perpetuity, não há nenhum limite de quanto tempo elas podem ser usadas.

Cada conta com privilégios de nível de administrador de domínio desnecessários aumenta sua exposição a invasores que buscam comprometer as credenciais. Para minimizar a área de superfície para ataque, você deseja fornecer apenas o conjunto específico de direitos que um administrador precisa para realizar o trabalho – e apenas para a janela de tempo necessária para concluí-lo.

Usando apenas administração suficiente e administração just-in-time, os administradores podem solicitar os privilégios específicos de que precisam para a janela exata do tempo necessário. Para um administrador de DNS, por exemplo, usar o PowerShell para habilitar apenas a administração suficiente permite que você crie um conjunto limitado de comandos que estão disponíveis para gerenciamento de DNS.

Se o administrador de DNS precisar fazer uma atualização para um de seus servidores, ele solicitará acesso para gerenciar o DNS usando Microsoft Identity Manager 2016. O fluxo de trabalho de solicitação pode incluir um processo de aprovação, como a autenticação de dois fatores, que pode chamar o telefone celular do administrador para confirmar sua identidade antes de conceder os privilégios solicitados. Uma vez concedido, esses privilégios de DNS fornecem acesso à função do PowerShell para o DNS por um período de tempo específico.

Imagine este cenário se as credenciais do administrador do DNS fossem roubadas. Primeiro, como as credenciais não têm privilégios de administrador anexados a elas, o invasor não poderá obter acesso ao servidor DNS – ou a outros sistemas – para fazer alterações. Se o invasor tentar solicitar privilégios para o servidor DNS, a autenticação de dois fatores solicitará que ele confirme sua identidade. Como não é provável que o invasor tenha o telefone celular do administrador do DNS, a autenticação falharia. Isso bloquearia o invasor do sistema e alertaria a organização de ti de que as credenciais podem ser comprometidas.

Além disso, muitas organizações usam a [solução de senha de administrador local gratuita (LAPs)](http://aka.ms/laps) como um mecanismo de administração JIT simples, mas poderoso, para seus sistemas cliente e servidor. O recurso de LAPSos fornece gerenciamento de senhas de contas locais de computadores ingressados no domínio. As senhas são armazenadas em Active Directory (AD) e protegidas pelo e pela ACL (lista de controle de acesso) para que somente usuários qualificados possam lê-lo ou solicitar sua redefinição.

Conforme observado no [Guia de mitigação de roubo de credenciais do Windows](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> "_as ferramentas e as técnicas que os criminosos usam para realizar o roubo de credenciais e reutilizar ataques aprimorados, invasores mal-intencionados estão encontrando mais fácil atingir suas metas. O roubo de credenciais geralmente se baseia em práticas operacionais ou exposição de credenciais de usuário, portanto, as atenuações efetivas exigem uma abordagem holística que atenda às pessoas, aos processos e à tecnologia. Além disso, esses ataques dependem do invasor roubar credenciais depois de comprometer um sistema para expandir ou manter o acesso, para que as organizações devam conter falhas rapidamente implementando estratégias que impedem que os invasores se movimentem livremente e não sejam detectados em um rede comprometida._ "

Uma consideração importante de design para o Windows Server foi a mitigação do roubo de credenciais — em particular, as credenciais derivadas. A Credential Guard fornece segurança significativamente aprimorada contra roubo e reutilização de credenciais derivadas implementando uma alteração arquitetônica significativa no Windows, projetada para ajudar a eliminar ataques de isolamento baseados em hardware, em vez de simplesmente tentar Proteja-se contra eles.

Embora o uso da proteção de credenciais do Windows Defender, NTLM e credenciais derivadas de Kerberos sejam protegidos usando a segurança baseada em virtualização, as técnicas de ataque de roubo de credenciais e as ferramentas usadas em muitos ataques direcionados são bloqueadas. O malware com privilégios administrativos em execução no sistema operacional não pode extrair segredos protegidos pela segurança baseada em virtualização. Embora o Windows Defender Credential Guard seja uma poderosa mitigação, os ataques de ameaças persistentes provavelmente mudarão para novas técnicas de ataque, e você também deverá incorporar o Device Guard, conforme descrito abaixo, juntamente com outras estratégias de segurança e arquiteturas.

#### <a name="windows-defender-credential-guard"></a>Proteção de credenciais do Windows Defender
O Windows Defender Credential Guard usa segurança baseada em virtualização para isolar informações de credenciais, impedindo que hashes de senha ou tíquetes Kerberos sejam interceptados. Ele usa um processo de autoridade de segurança local isolada (LSA) totalmente novo, que não pode ser acessado pelo restante do sistema operacional. Todos os binários usados pelo LSAs isolado são assinados com certificados que são validados antes de serem iniciados no ambiente protegido, tornando os ataques de tipo Pass-the-hash completamente ineficazes.

O Windows Defender Credential Guard usa:

- Segurança baseada em virtualização (obrigatória). Também necessário:

    - CPU de 64 bits

    - Extensões de virtualização de CPU, mais tabelas de páginas estendidas

    - Hipervisor do Windows

- Inicialização segura (obrigatório)

- TPM 2.0 distinto ou de firmware (preferencial - fornece a associação ao hardware)

Você pode usar o Windows Defender Credential Guard para ajudar a proteger identidades com privilégios protegendo as credenciais e derivações de credencial no Windows Server 2016. Para obter mais informações sobre os requisitos do Windows Defender Credential Guard, consulte [proteger credenciais de domínio derivado com o Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Proteção de credencial remota do Windows Defender
A proteção de credenciais remota do Windows Defender no Windows Server 2016 e na atualização de aniversário do Windows 10 também ajuda a proteger as credenciais para usuários com conexões de área de trabalho remota. Anteriormente, qualquer pessoa que estivesse usando Serviços de Área de Trabalho Remota precisaria fazer logon em seu computador local e, em seguida, precisaria fazer logon novamente quando executo uma conexão remota com seu computador de destino. Esse segundo logon passa as credenciais para o computador de destino, expondo-as a ataques de passagem de hash ou de passagem de tíquete.

Com a proteção remota de credenciais do Windows Defender, o Windows Server 2016 implementa o logon único para sessões de Área de Trabalho Remota, eliminando a necessidade de inserir novamente seu nome de usuário e senha. Em vez disso, ele aproveita as credenciais que você já usou para fazer logon em seu computador local. Para usar a proteção remota de credenciais do Windows Defender, o cliente e o servidor Área de Trabalho Remota devem atender aos seguintes requisitos:

- Deve ser ingressado em um domínio Active Directory e estar no mesmo domínio ou em um domínio com uma relação de confiança.

- Deve usar a autenticação Kerberos.

- Deve estar executando pelo menos o Windows 10 versão 1607 ou o Windows Server 2016.  

- O Área de Trabalho Remota aplicativo clássico do Windows é necessário. O aplicativo Área de Trabalho Remota Plataforma Universal do Windows não dá suporte à proteção remota de credenciais do Windows Defender.

Você pode habilitar a proteção remota de credenciais do Windows Defender usando uma configuração do registro no servidor Área de Trabalho Remota e Política de Grupo ou um parâmetro Conexão de Área de Trabalho Remota no cliente Área de Trabalho Remota. Para obter mais informações sobre como habilitar a proteção remota de credenciais do Windows Defender, consulte [proteger área de trabalho remota credenciais com o Windows Defender Remote Credential Guard](https://docs.microsoft.com/windows/access-protection/remote-credential-guard). Assim como no Windows Defender Credential Guard, você pode usar o Windows Defender Remote Credential Guard para ajudar a proteger as identidades com privilégios no Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Proteger o sistema operacional para executar seus aplicativos e a infraestrutura
Impedir ameaças cibernéticos também exige a localização e o bloqueio de malware e ataques que buscam obter controle ao subverter as práticas operacionais padrão da sua infraestrutura. Se os invasores puderem fazer com que um sistema operacional ou aplicativo seja executado de forma não predeterminada e não viável, provavelmente ele usará esse sistema para realizar ações mal-intencionadas. O Windows Server 2016 fornece camadas de proteção que bloqueiam invasores externos que executam software mal-intencionado ou exploram vulnerabilidades. O sistema operacional usa uma função ativa para proteger a infraestrutura e os aplicativos ao alertar os administradores sobre a atividade que indica que um sistema foi violado.

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
O Windows Server 2016 inclui o Windows Defender Device Guard para garantir que apenas softwares confiáveis possam ser executados no servidor. Usando a segurança baseada em virtualização, ele pode limitar quais binários podem ser executados no sistema com base na política da organização. Se qualquer coisa, exceto os binários especificados, tentar executar, o Windows Server 2016 o bloqueará e registrará a tentativa com falha para que os administradores possam ver que houve uma possível violação. A notificação de violação é uma parte crítica dos requisitos para a conformidade do GDPR.

O Windows Defender Device Guard também é integrado ao PowerShell para que você possa autorizar quais scripts podem ser executados em seu sistema. Em versões anteriores do Windows Server, os administradores poderiam ignorar a imposição de integridade de código simplesmente excluindo a política do arquivo de código. Com o Windows Server 2016, você pode configurar uma política que é assinada pela sua organização para que apenas uma pessoa com acesso ao certificado que assinou a política possa alterar a política.

#### <a name="control-flow-guard"></a>Proteção de fluxo de controle 
O Windows Server 2016 também inclui proteção interna contra algumas classes de ataques de corrupção de memória. A aplicação de patch nos servidores é importante, mas sempre há uma chance de que o malware possa ser desenvolvido para uma vulnerabilidade que ainda não foi identificada. Alguns dos métodos mais comuns para explorar essas vulnerabilidades são fornecer dados incomuns ou extremos a um programa em execução. Por exemplo, um invasor pode explorar uma vulnerabilidade de estouro de buffer fornecendo mais entrada a um programa do que o esperado e saturando a área reservada pelo programa para manter uma resposta. Isso pode corromper a memória adjacente que pode conter um ponteiro de função.

Quando o programa chama essa função, ele pode ir para um local não intencional especificado pelo invasor. Esses ataques também são conhecidos como ataques de JOP (programação orientada a salto). A proteção de fluxo de controle impede ataques JOP, colocando restrições rígidas sobre o código do aplicativo que pode ser executado – especialmente instruções de chamada indiretas. Ele adiciona verificações de segurança leves para identificar o conjunto de funções no aplicativo que são destinos válidos para chamadas indiretas. Quando um aplicativo é executado, ele verifica se esses destinos de chamada indireta são válidos.

Se a verificação de proteção do fluxo de controle falhar em tempo de execução, o Windows Server 2016 encerrará imediatamente o programa, interrompendo qualquer exploração que tente chamar indiretamente um endereço inválido. O protetor de fluxo de controle fornece uma camada adicional importante de proteção para o Device Guard. Se um aplicativo listado em branco tiver sido comprometido, ele poderá ser executado desmarcado pelo Device Guard, pois a triagem de dispositivo veria que o aplicativo foi assinado e é considerado confiável.

Mas como a proteção de fluxo de controle pode identificar se o aplicativo está em execução em uma ordem não predeterminada e não viável, o ataque falharia, impedindo a execução do aplicativo comprometido. Juntas, essas proteções tornam muito difícil para os invasores injetarem malwares em software em execução no Windows Server 2016.

Os desenvolvedores que criam aplicativos nos quais os dados pessoais serão tratados são incentivados a habilitar a proteção de fluxo de controle (CFG) em seus aplicativos. Esse recurso está disponível no Microsoft Visual Studio 2015 e é executado em versões "com reconhecimento de CFG" do Windows — as versões x86 e x64 para desktop e servidor do Windows 10 e do Windows 8.1 Update (KB3000850). Você não precisa habilitar o CFG para cada parte do seu código, pois uma combinação de CFG habilitado e código não habilitado para CFG será executada corretamente. Mas a falha ao habilitar CFG para todo o código pode abrir lacunas na proteção. Além disso, o código de CFG habilitado funciona bem em versões "CFG sem reconhecimento" do Windows e, portanto, é totalmente compatível com eles.

#### <a name="windows-defender-antivirus"></a>Windows Defender Antivírus
O Windows Server 2016 inclui os recursos de detecção ativa e líder do setor do Windows Defender para bloquear malwares conhecidos. O Windows Defender antivírus (AV) funciona junto com o Windows Defender Device Guard e o protetor de fluxo de controle para impedir que um código mal-intencionado de qualquer tipo seja instalado nos servidores. Ele é ativado por padrão – o administrador não precisa realizar nenhuma ação para que ele comece a funcionar. O Windows Defender AV também é otimizado para dar suporte a várias funções de servidor no Windows Server 2016. No passado, os invasores usaram shells como o PowerShell para iniciar código binário mal-intencionado. No Windows Server 2016, agora o PowerShell está integrado ao Windows Defender AV para verificar a existência de malware antes de iniciar o código.

O Windows Defender AV é uma solução de antimalware interna que fornece gerenciamento de segurança e antimalware para desktops, computadores portáteis e servidores. O Windows Defender AV foi significativamente aprimorado desde que foi introduzido no Windows 8. O Windows Defender antivírus no Windows Server usa uma abordagem de várias pontas para melhorar o AntiMalware:

- **Proteção entregue na nuvem** ajuda a detectar e bloquear novo malware em segundos, mesmo que o malware não tenha sido visto antes.

- O **Contexto local avançado** aprimora a forma como o malware é identificado. O Windows Server informa o Windows Defender AV não apenas sobre conteúdo como arquivos e processos, mas também de onde veio o conteúdo, onde ele foi armazenado e muito mais. 

- **Sensores globais extensivos** ajudam a manter o Windows Defender AV atual e a reconhecer até mesmo o malware mais recente. Isso é feito de duas maneiras: coletando os dados de contexto local avançado de pontos de extremidade e analisando centralmente esses dados.

- A **prova de adulteração** ajuda a proteger o próprio Windows Defender antivírus contra ataques de malware. Por exemplo, o Windows Defender AV usa processos protegidos, o que impede que processos não confiáveis tentem violar os componentes do Windows Defender AV, suas chaves do registro e assim por diante.

- Os **recursos de nível empresarial** fornecem aos profissionais de ti as ferramentas e as opções de configuração necessárias para tornar o Windows Defender AV uma solução antimalware de classe empresarial.

#### <a name="enhanced-security-auditing"></a>Auditoria de segurança aprimorada 
O Windows Server 2016 alerta ativamente os administradores para possíveis tentativas de violação com a auditoria de segurança aprimorada que fornece informações mais detalhadas, que podem ser usadas para detecção de ataque mais rápida e análise forense. Ele registra eventos da proteção de fluxo de controle, do Windows Defender Device Guard e de outros recursos de segurança em um único local, facilitando para os administradores determinar quais sistemas podem estar em risco.

As novas categorias de evento incluem:

- **Auditar Associação de grupo.** Permite auditar as informações de associação de grupo no token de logon de um usuário. Os eventos são gerados quando as associações de grupo são enumeradas ou consultadas no PC em que a sessão de logon foi criada. 
 
- **Auditar atividade de PnP.** Permite que você faça a auditoria quando o plug and Play detecta um dispositivo externo, que pode conter malware. Os eventos PnP podem ser usados para rastrear alterações no hardware do sistema. Uma lista de IDs de fornecedor de hardware está incluída no evento.

O Windows Server 2016 integra-se facilmente com sistemas SIEM (gerenciamento de eventos de incidente de segurança), como o Microsoft Operations Management Suite (OMS), que pode incorporar as informações em relatórios de inteligência sobre possíveis violações. A profundidade das informações fornecidas pela auditoria aprimorada permite que as equipes de segurança identifiquem e respondam a possíveis violações com mais rapidez e eficiência.

### <a name="secure-virtualization"></a>Virtualização segura
Hoje em dia, as empresas virtualizam tudo o que podem, de SQL Server ao SharePoint para Domínio do Active Directory controladores. As VMs (máquinas virtuais) simplesmente facilitam a implantação, o gerenciamento, a manutenção e a automatização da sua infraestrutura. Mas, quando se trata de segurança, as malhas de virtualização comprometidas se tornaram um novo vetor de ataque que é difícil de se defender – até agora. Do ponto de vista do GDPR, você deve pensar em proteger as VMs como protegeria os servidores físicos, incluindo o uso da tecnologia TPM da VM.

O Windows Server 2016 muda fundamentalmente como as empresas podem proteger a virtualização, incluindo várias tecnologias que permitem que você crie máquinas virtuais que serão executadas somente em sua própria malha; ajudar a proteger os dispositivos de armazenamento, rede e host em que eles são executados.

#### <a name="shielded-virtual-machines"></a>Máquinas virtuais blindadas
As mesmas coisas que tornam as máquinas virtuais tão fáceis de migrar, fazer backup e replicar, também facilitam a modificação e a cópia. Uma máquina virtual é apenas um arquivo, portanto, não está protegida na rede, no armazenamento, em backups ou em outro lugar. Outro problema é que os administradores de malha – sejam um administrador de armazenamento ou um administrador de rede – têm acesso a todas as máquinas virtuais.

Um administrador comprometido na malha pode resultar facilmente em dados comprometidos entre máquinas virtuais. Tudo o que o invasor deve fazer é usar as credenciais comprometidas para copiar quaisquer arquivos de VM que desejarem em uma unidade USB e extraí-los da organização, onde esses arquivos de VM podem ser acessados de qualquer outro sistema. Se qualquer uma dessas VMs roubadas fosse um controlador de domínio Active Directory, por exemplo, o invasor poderia exibir facilmente o conteúdo e usar técnicas de força bruta prontamente disponíveis para decifrar as senhas no banco de dados Active Directory, permitindo, por fim, acesso para todo o restante dentro de sua infraestrutura.

O Windows Server 2016 introduz máquinas virtuais blindadas (VMs blindadas) para ajudar a proteger contra cenários como aquele que acabou de descrever. As VMs blindadas incluem um dispositivo TPM virtual, que permite às organizações aplicar a criptografia BitLocker às máquinas virtuais e garantir que elas sejam executadas somente em hosts confiáveis para ajudar a proteger contra armazenamento comprometido, rede e administradores de host. As VMs blindadas são criadas usando VMs de geração 2, que dão suporte ao firmware de Unified Extensible Firmware Interface (UEFI) e têm o TPM virtual.

#### <a name="host-guardian-service"></a>Serviço guardião de host
Junto com VMs blindadas, o serviço guardião de host é um componente essencial para a criação de uma malha de virtualização segura. Seu trabalho é atestar a integridade de um host Hyper-V antes de permitir que uma VM blindada inicialize ou migre para esse host. Ele mantém as chaves para VMs blindadas e não as liberará até que a integridade da segurança seja garantida. Há duas maneiras de exigir que os hosts Hyper-V atestam para o serviço guardião de host.

O primeiro e mais seguro é o atestado de hardware confiável. Essa solução requer que suas VMs blindadas estejam em execução em hosts com chips TPM 2,0 e UEFI 2.3.1. Esse hardware é necessário para fornecer a inicialização medida e as informações de integridade do kernel do sistema operacional exigidas pelo serviço guardião de host para garantir que o host Hyper-V não tenha sido adulterado.

As organizações de ti têm a alternativa de usar o atestado de administrador confiável, que pode ser desejável se o hardware do TPM 2,0 não estiver em uso na sua organização. Esse modelo de atestado é fácil de implantar porque os hosts são simplesmente colocados em um grupo de segurança e o serviço guardião de host é configurado para permitir que VMs blindadas sejam executadas em hosts que são membros do grupo de segurança. Com esse método, não há nenhuma medida complexa para garantir que a máquina host não tenha sido violada. No entanto, você elimina a possibilidade de que VMs não criptografadas percorrendo a porta em unidades USB ou que a VM seja executada em um host não autorizado. Isso ocorre porque os arquivos da VM não serão executados em nenhum computador diferente daqueles no grupo designado. Se você ainda não tiver um hardware do TPM 2,0, poderá começar com o atestado confiável de administrador e alternar para o atestado de hardware confiável quando o hardware for atualizado.

#### <a name="virtual-machine-trusted-platform-module"></a>Trusted Platform Module de máquina virtual
O Windows Server 2016 dá suporte a TPM para máquinas virtuais, o que permite que você dê suporte a tecnologias de segurança avançada, como o BitLocker® a criptografia de unidade em máquinas virtuais. Você pode habilitar o suporte do TPM em qualquer máquina virtual do Hyper-V de geração 2 usando o Gerenciador do Hyper-V ou o cmdlet Enable-VMTPM do Windows PowerShell.

Você pode proteger o TPM virtual (vTPM) usando as chaves de criptografia locais armazenadas no host ou armazenadas no serviço guardião de host. Portanto, embora o serviço guardião de host exija mais infraestrutura, ele também fornece mais proteção.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Firewall de rede distribuída usando rede definida pelo software
Uma maneira de melhorar a proteção em ambientes virtualizados é segmentar a rede de forma que permita que as VMs se comuniquem apenas com os sistemas específicos necessários para funcionar. Por exemplo, se seu aplicativo não precisar se conectar à Internet, você poderá particioná-lo, eliminando esses sistemas como destinos de invasores externos. A SDN (rede definida pelo software) no Windows Server 2016 inclui um firewall de rede distribuído que permite que você crie dinamicamente as políticas de segurança que podem proteger seus aplicativos contra ataques provenientes de dentro ou fora de uma rede. Esse firewall de rede distribuída adiciona camadas à sua segurança, permitindo que você Isole seus aplicativos na rede. As políticas podem ser aplicadas em qualquer lugar em sua infraestrutura de rede virtual, isolando o tráfego de VM para VM, tráfego de VM para host ou tráfego de VM para Internet, quando necessário – para sistemas individuais que podem ter sido comprometidos ou programaticamente em várias sub-redes. Os recursos de rede definidos pelo software do Windows Server 2016 também permitem rotear ou espelhar o tráfego de entrada para dispositivos virtuais que não são da Microsoft. Por exemplo, você pode optar por enviar todo o tráfego de email por meio de um dispositivo virtual Barracuda para proteção adicional de filtragem de spam. Isso permite que você facilmente camadas de segurança adicional no local ou na nuvem.

### <a name="other-gdpr-considerations-for-servers"></a>Outras considerações de GDPR para servidores
O GDPR inclui requisitos explícitos de notificação de violação, onde uma violação de dados pessoais significa "_uma violação de segurança que leva à destruição acidental ou ilegal, perda, alteração, divulgação não autorizada ou acesso a dados pessoais transmitido, armazenado ou processado de outra forma._ "  Obviamente, você não pode começar a avançar para atender aos rigorosos requisitos de notificação de GDPR em 72 horas se não for possível detectar a violação em primeiro lugar.

Conforme observado no White paper da central de segurança do [Windows, pós-violação: Lidando com ameaças avançadas](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> "_Ao contrário da pré-associação, a pós-violação pressupõe que já ocorreu uma violação – agindo como um Flight Recorder e o CSI (investigadores de cena de crime). A pós-violação fornece às equipes de segurança as informações e o conjunto de ferramentas necessários para identificar, investigar e responder a ataques que, caso contrário, permanecerão não detectados e abaixo do radar._ "

Nesta seção, veremos como o Windows Server pode ajudá-lo a atender suas obrigações de notificação de violação do GDPR. Isso começa com a compreensão dos dados de ameaça subjacentes disponíveis para a Microsoft que são coletados e analisados para seu benefício e como, por meio da ATP (proteção avançada contra ameaças) do Windows Defender, que os dados podem ser fundamentais para você.

#### <a name="insightful-security-diagnostic-data"></a>Dados de diagnóstico de segurança inteligente
Por aproximadamente duas décadas, a Microsoft vem transformando ameaças em inteligência útil que pode ajudar a fortalecer sua plataforma e proteger os clientes. Hoje em dia, com as imensas vantagens da computação fornecidas pela nuvem, estamos encontrando novas maneiras de usar nossas ferramentas ricas de análise direcionadas para atacar de forma inteligente as violações e proteger nossos clientes.

Aplicando uma combinação de processos automatizados e manuais, aprendizado de máquina e especialistas humanos, nós podemos criar um gráfico de segurança inteligente que aprende consigo mesmo e evolui em tempo real, reduzindo o nosso tempo coletivo para detectar e responder a incidentes novamente em nossos produtos.

![Grafo de segurança do Microsoft Intelligence](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

O escopo da inteligência contra ameaças da Microsoft abrange, literalmente, bilhões de pontos de dados: 35.000.000.000 mensagens verificadas mensalmente, 1.000.000.000 clientes em segmentos corporativos e de consumidor acessando mais de 200 serviços de nuvem e as autenticações 14.000.000.000 executadas diariamente. Todos esses dados são reunidos em seu nome pela Microsoft para criar o Gráfico de Segurança Inteligente que pode ajudá-lo a proteger sua porta frontal de maneira dinâmica de ficar seguro, permanecer produtivo e atender aos requisitos do GDPR.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Detectar ataques e investigação forense
Até mesmo as melhores defesas de ponto de extremidade podem ser violadas eventualmente, conforme cyberattacks se tornam mais sofisticados e direcionados. Dois recursos podem ser usados para ajudar com a detecção de possíveis violações – a ATP (proteção avançada contra ameaças) do Windows Defender e o Microsoft Advanced Threat Analytics (ATA).

Proteção Avançada contra Ameaças do Windows Defender (ATP) ajuda a detectar, investigar e responder a ataques avançados e violações de dados em suas redes. Os tipos de violação de dados que o GDPR espera que você proteja contra as medidas de segurança técnica para garantir a confidencialidade, a integridade e a disponibilidade contínuas de sistemas de processamento e dados pessoais.

Entre os principais benefícios do Windows Defender ATP estão os seguintes:

- **Detectando o não detectável.** Sensores criados em profundidade no kernel do sistema operacional, especialistas em segurança do Windows e óticas exclusivas de mais de 1.000.000.000 computadores e sinais em todos os serviços da Microsoft.

- **Interno, não Unido.** Sem agente, com alto desempenho e impacto mínimo, com capacidade de nuvem; gerenciamento fácil sem implantação. 

- **Painel único do vidro para segurança do Windows.** Explore 6 meses de experiência avançada, linha do tempo, unificando eventos de segurança do Windows Defender ATP, do Windows Defender antivírus e do Windows Defender Device Guard.

- **Poder do Microsoft Graph.** Aproveita o grafo de segurança do Microsoft Intelligence para integrar a detecção e exploração com a assinatura do Office 365 ATP, para rastrear e responder a ataques.

Leia mais em [novidades na versão prévia do Windows Defender ATP Creators Update](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

O ATA é um produto local que ajuda a detectar o comprometimento de identidade em uma organização. O ATA pode capturar e analisar o tráfego de rede para autenticação, autorização e protocolos de coleta de informações (como Kerberos, DNS, RPC, NTLM e outros protocolos). O ATA usa esses dados para criar um perfil comportamental sobre os usuários e outras entidades em uma rede para que ele possa detectar anomalias e padrões de ataque conhecidos. A tabela a seguir lista os tipos de ataque detectados pelo ATA.


|Tipo de ataque |Descrição |
|---------|---------|
|Ataques mal-intencionados |Esses ataques são detectados procurando ataques de uma lista conhecida de tipos de ataque, incluindo:<ul><li>Passagem de tíquete (PtT)</li><li>Pass-the-hash (PtH)</li><li>Sobrepass-The-hash</li><li>PAC forjado (MS14-068)</li><li>Bilhete dourado</li><li>Replicações mal-intencionadas</li><li>Reconhecimento</li><li>Força bruta</li><li>Execução remota</li></ul>Para obter uma lista completa de ataques mal-intencionados que podem ser detectados e sua descrição, consulte [quais atividades suspeitas podem ser detectadas pelo ATA?](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportamento anormal |Esses ataques são detectados usando a análise comportamental e usam o Machine Learning para identificar atividades questionáveis, incluindo:<ul><li>Logons anormais</li><li>Ameaças desconhecidas</li><li>Compartilhamento de senha</li><li>Movimento lateral</li></ul>|
|Problemas de segurança e riscos |Esses ataques são detectados examinando a configuração atual da rede e do sistema, incluindo:<ul><li>Confiança quebrada</li><li>Protocolos fracos</li><li>Vulnerabilidades de protocolo conhecidas</li></ul>|

Você pode usar o ATA para ajudar a detectar invasores tentando comprometer identidades privilegiadas. Para obter mais informações sobre como implantar o ATA, consulte os tópicos planejar, projetar e implantar na [documentação do Advanced Threat Analytics](https://docs.microsoft.com/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Conteúdo relacionado para soluções do Windows Server 2016 associadas

- **Windows Defender antivírus:** https://www.youtube.com/watch?v=P1aNEy09NaI e https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Proteção avançada contra ameaças do Windows Defender:** https://www.youtube.com/watch?v=qxeGa3pxIwg e https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Proteção de dispositivo do Windows Defender:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Proteção de credenciais do Windows Defender:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **Proteção de fluxo de controle:** https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx

- **Segurança e garantia:** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Aviso de isenção de responsabilidade
Este artigo é um comentário em GDPR, como Microsoft interpreta, a partir da data de publicação. Passamos muito tempo com GDPR e gostam de pensar que temos sido considerados uma consideração sobre sua intenção e significado. Mas a aplicação de GDPR é altamente específica de fato, e nem todos os aspectos e interpretações de GDPR são bem liquidadas.

Como resultado, este artigo é fornecido apenas para fins informativos e não ser considerado como advogado ou para determinar como GDPR podem se aplicar a você e sua organização. Encorajamos você a trabalhar com um profissional legalmente qualificados para discutir GDPR, como ele se aplica especificamente para sua organização e como melhor garantir a conformidade.

A MICROSOFT NÃO OFERECE NENHUMA GARANTIA EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA EM RELAÇÃO ÀS INFORMAÇÕES CONTIDAS NESTE ARTIGO. Este artigo é fornecido "na condição em que se encontra". Informações e exibições expressas neste artigo, incluindo URL e outras referências de site da Internet, podem ser alteradas sem aviso prévio.

Este artigo não fornece direitos legais e nenhuma propriedade intelectual sobre qualquer produto da Microsoft.  Você pode copiar e usar este artigo apenas para fins de referência interna.  

Publicado em setembro de 2017<br>
Versão 1.0<br>
© 2017 Microsoft. Todos os direitos reservados.


