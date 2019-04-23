---
title: Como iniciar sua jornada pelo Regulamento de Proteção Geral de Dados (GDPR) para Windows Server 2016
description: Use este artigo para entender o que é GDPR e sobre os produtos que a Microsoft fornece para ajudá-lo a seguir em direção a conformidade.
keywords: privacidade, GDPR
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: be9509de0291924bb95733f995b447230bb75214
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870127"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>Comece sua jornada de regulamentação de proteção geral de dados (GDPR) para o Windows Server 

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

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

- **Direitos de privacidade aprimorada de pessoal.** Proteção de dados reforçada para residentes da UE, garantindo que eles tenham o direito de acessar seus dados pessoais, para corrigir imprecisões nesses dados, para apagar dados, para contestar o processamento de seus dados pessoais e para movê-los.

- **Aumento de imposto para proteger dados pessoais.** Responsabilidade reforçada de organizações que processam dados pessoais, fornecendo maior clareza sobre responsabilidade em garantir a conformidade.

- **Os relatórios de violação obrigatórios dados pessoais.** As organizações que controlam os dados pessoais são necessárias para relatar violações de dados pessoais que representam um risco para os direitos e a liberdade de indivíduos para suas autoridades de supervisão sem atrasos desnecessários e, onde possível, não depois de 72 horas após ficarem cientes da violação.

Como você pode prever, o GDPR pode ter um impacto significativo em seus negócios, potencialmente exigindo que você atualize as políticas de privacidade, implemente e fortaleça controles de proteção de dados e viole procedimentos de notificação, implante políticas altamente transparentes, e invista ainda mais em IT e treinamento. Microsoft Windows 10 pode ajudá-lo efetivamente e lidar com alguns desses requisitos com eficiência.

## <a name="personal-and-sensitive-data"></a>Dados pessoais e confidenciais
Como parte de seus esforços para estar em conformidade com a GDPR, você precisará entender como a norma define dados pessoais e confidenciais e como essas definições se relacionam aos dados mantidos por sua organização. Com base no que compreensão, que você poderá descobrir onde esses dados são criados, processada, gerenciados e armazenados.

O GDPR considera dados pessoais como quaisquer informações relacionadas a uma pessoa identificável ou identificada naturalmente. Que podem incluir identificação direta (como seu nome legal) e identificação indireta (como informações específicas que tornam claro que é você as referências de dados). O GDPR também deixa claro que o conceito de dados pessoais inclui identificadores online (como, endereços IP, IDs de dispositivo móvel) e dados de localização.

O GDPR apresenta definições específicas para dados genéticos (como a sequência de gene de um indivíduo) e os dados biométricos. Dados de genética e biométricos juntamente com outras subcategorias de dados pessoais (dados pessoais, revelando origem racial ou étnica, opiniões políticas, religiosas ou filosóficas ou filiação: dados relativos a integridade; ou dados relativos a vida sexual ou orientação sexual da pessoa) são tratados como dados pessoais confidenciais sob o GDPR. Dados pessoais confidenciais recebem proteções avançadas e geralmente requerem o consentimento explícito de uma pessoa onde esses dados devem ser processados.

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

-   **Descobrir.** Identifique quais dados pessoais que você tem e onde estão. 

-   **Gerencie.** Gerencie como dados pessoais são usados e acessados.

-   **Protege.** Estabelece controles de segurança para evitar, detectar e responder a vulnerabilidades e violações de dados.  

-   **Relatório.** Age em solicitações de dados, violações de dados de relatório e mantém a documentação necessária.

    ![Diagrama sobre como as 4 etapas principais do GDPR funcionam juntas](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Para cada uma das etapas, criamos recursos em várias soluções da Microsoft, que podem ser usados para ajudá-lo a atender os requisitos dessa etapa, recursos e ferramentas de exemplo. Embora este artigo não é um guia abrangente de "instruções", incluímos links para você descobrir mais detalhes e mais informações estão disponíveis na [seção GDPR do Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Privacidade e segurança do Windows Server
O GDPR exige que você implementar medidas de segurança técnicas e organizacionais apropriadas para proteger dados pessoais e sistemas de processamento. No contexto do GDPR, seus ambientes de servidor físicos e virtuais são potencialmente processando dados pessoais e confidenciais. Processamento pode significar qualquer operação ou conjunto de operações, como coleta de dados, armazenamento e recuperação.

Sua capacidade para atender a esse requisito e implementar medidas de segurança técnicas apropriadas deve refletir as ameaças que enfrentam no ambiente de TI cada vez mais hostil de hoje. O cenário de ameaças de segurança atual é de ameaças agressivas e obstinadas. Nos anos anteriores, invasores mal-intencionados se concentravam principalmente em receber o reconhecimento da comunidade por meio de seus ataques ou a emoção de colocar temporariamente um sistema offline. Desde então, a motivação dos invasores passou a ser ganhar dinheiro, o que inclui o sequestro de computadores e dados até os proprietários pagarem o resgate exigido.

Ataques modernos cada vez mais se concentram no roubo de propriedade intelectual em grande escala; na degradação do sistema visado que pode resultar em perda financeira; e agora até mesmo o cyberterrorismo que ameaça a segurança de indivíduos, empresas e interesses nacionais em todo o mundo. Esses invasores são geralmente indivíduos altamente treinados e especialistas em segurança, alguns dos quais estão a serviço de nações que possuem grandes orçamentos e recursos humanos aparentemente ilimitados. Ameaças como essas exigem uma abordagem que pode superar esse desafio.

Essas ameaças não são apenas um risco à sua capacidade de manter o controle de quaisquer dados pessoais ou confidenciais, mas também são um risco material a sua empresa em um todo. Considere os dados recentes do McKinsey Ponemon Institute, Verizon e da Microsoft:

- O custo médio do tipo de violação de dados ao GDPR esperam que você relate US$ 3,5 milhões.

- 63% dessas violações envolvem senhas fracas ou roubadas que o GDPR espera que você resolva.

- Mais de 300.000 novas amostras de malware são criadas e espalhadas todos os dias, tornando sua tarefa de resolver a proteção de dados ainda mais desafiadora.

Conforme visto com os recentes ataques de Ransomware, uma vez chamados o preto Azeroth da Internet, os invasores serão depois dos destinos maiores do que podem se dar ao luxo de pagamento mais com consequências potencialmente catastróficas. O GDPR inclui penalidades que tornam seus sistemas, incluindo desktops e laptops, que contêm dados pessoais e confidenciais, de fato, destinos avançados.

Dois princípios-chave têm interativa e continuam guiar o desenvolvimento do Windows:

- **Segurança.** Os dados que armazenam a nossos softwares e serviços em nome de nossos clientes devem ser protegidos contra danos e usados ou modificados somente de forma apropriada. Modelos de segurança devem ser fácil para os desenvolvedores a entender e desenvolver seus aplicativos.

- **Privacidade.** Os usuários devem ser no controle de como seus dados são usados. As políticas para o uso de informações devem ficar claras para o usuário. Os usuários devem estar no controle de quando e se o usuário receber informações para fazer melhor uso de seu tempo. Deve ser fácil para os usuários especifiquem o uso adequado das suas informações incluindo controlando o uso de email enviam.

Microsoft permaneceu sólido em relação a esses princípios recentemente observados pela CEO da Microsoft, Satya Nadella, 

> "_Conforme o mundo continua mudando e requisitos de negócios evoluem, algumas coisas são consistentes: demanda do cliente de segurança e privacidade._ "

Conforme você trabalha para cumprir o GDPR, compreender a função dos servidores físicos e virtuais em criar, acessar, processamento, armazenamento e gerenciamento de dados que podem ser classificado como pessoal e dados potencialmente confidenciais no GDPR são importantes. O Windows Server fornece recursos que ajudarão você a cumpram os requisitos de GDPR para implementar medidas de segurança técnicas e organizacionais apropriadas para proteger dados pessoais.

A postura de segurança do Windows Server 2016 não é um bolt em; é um princípio de arquitetura. Além disso, pode ser mais bem compreendido em entidades de quatro:

- **Protege.** Foco em andamento e a inovação em medidas preventivas; Bloqueie ataques conhecidos e malware conhecido.

- **Detecte.** Abrangente de ferramentas para ajudá-lo anormalidades identificar e responder a ataques mais rápidos de monitoramento.

- **Responda.** Principais tecnologias de resposta e recuperação mais profundo que a experiência de consultoria.

- **Isole.** Isolar segredos de dados e componentes do sistema operacional, limitar privilégios de administrador e medir rigorosamente a integridade do host.

Com o Windows Server, sua capacidade de proteger, detectar e se defender contra os tipos de ataques que podem levar a violações de dados melhorou muito. Considerando requisitos rígidos que envolvem às notificações de violação dentro do GDPR, garantir que seus sistemas de desktop e laptop estão bem protegiam diminuirá os riscos decorrentes que poderiam resultar na notificação e análise de violação caras.

A seção a seguir, você verá como o Windows Server fornece recursos que se ajustam diretamente no estágio de "Proteger" da sua jornada de conformidade de GDPR. Esses recursos se encaixam em três cenários de proteção:

- **Proteger suas credenciais e limitar privilégios de administrador.** Windows Server 2016 ajuda a implementar essas alterações, para ajudar a impedir que o sistema que está sendo usado como um ponto de início para invasões ainda mais.

- **Proteger o sistema operacional para executar seus aplicativos e infraestrutura.** Windows Server 2016 fornece camadas de proteção, o que ajuda a bloquear os invasores externos que executam o software mal-intencionado ou exploração de vulnerabilidades.

- **Virtualização segura.** Windows Server 2016 permite a virtualização segura, usando VMs Blindadas e malha protegida. Isso ajuda a criptografar e executar suas máquinas virtuais em hosts confiáveis em sua malha, melhor protegê-los contra ataques mal-intencionados.

Esses recursos, discutidos mais detalhadamente abaixo com referências a requisitos específicos de GDPR, são criados com base na proteção avançada de dispositivo que ajuda a manter a integridade e a segurança do sistema operacional e dados.

Uma cláusula de chave dentro do GDPR é a proteção de dados por design e, por padrão e ajudá-lo com sua capacidade de atender a esse provisionamento são recursos dentro do Windows 10, como criptografia de dispositivo de disco BitLocker. O BitLocker usa a tecnologia Trusted Platform Module (TPM), que fornece funções relacionadas à segurança baseada em hardware. Esse chip de processador de criptografia inclui vários mecanismos de segurança física para torná-lo resistente à adulteração e software mal-intencionado não consegue violar as funções de segurança do TPM.

O chip inclui vários mecanismos de segurança física para torná-lo resistente a adulterações nas funções de segurança do TPM por software mal-intencionado. Algumas das principais vantagens do uso da tecnologia TPM são a possibilidade de:

-   Gerar, armazenar e limitar o uso de chaves de criptografia.

-   Usar a tecnologia TPM para autenticação de dispositivo de plataforma com a chave RSA de autogravação exclusiva do TPM.

-   Ajuda a garantir a integridade da plataforma, executando e armazenando medidas de segurança.

Proteção avançada de dispositivo adicional relevante para sua operação sem violações de dados incluem a inicialização confiável do Windows para ajudar a manter a integridade do sistema, garantindo que o malware não possa iniciar antes das defesas do sistema.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Suporte a sua jornada de conformidade de GDPR
Principais recursos no Windows Server podem ajudá-lo com eficiência e eficácia implementar os mecanismos de segurança e privacidade que exige que o GDPR quanto à conformidade. Embora o uso desses recursos não garantirá a conformidade, que aceitarão seus esforços para fazer isso.

O sistema operacional reside em uma camada estratégica na infra-estrutura da organização, oferecendo novas oportunidades para criar camadas de proteção contra ataques que poderia roubar dados e interromper o seu negócio. Principais aspectos da GDPR, como privacidade por Design, proteção de dados e controle de acesso precisam ser tratados dentro de sua infraestrutura de TI no nível do servidor.

Trabalhando para ajudar a proteger a identidade, sistema operacional e as camadas de virtualização, o Windows Server 2016 ajuda a bloquear os vetores de ataque comuns usados para obter acesso ilícito aos seus sistemas: roubo de credenciais, malware e uma malha de virtualização comprometido. Além de reduzir o risco de negócios, os componentes de segurança incorporada ao Ajuda do Windows Server 2016 atender aos requisitos de conformidade para a chave governo e setor normas de segurança. 

Essas identidades, o sistema operacional e o proteções de virtualização que você possa proteger melhor o seu datacenter, executando o Windows Server como uma VM em qualquer nuvem e limitar a capacidade de invasores para comprometer credenciais, inicie o malware e não ser detectados no seu rede. Da mesma forma, quando implantado como um host Hyper-V, o Windows Server 2016 oferece garantia de segurança para seus ambientes de virtualização por meio de recursos de firewall distribuído e VMs Blindadas. Com o Windows Server 2016, o sistema operacional se torna um participante ativo na segurança de seu datacenter.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Proteger suas credenciais e limitar privilégios de administrador 
Controle de acesso a dados pessoais e os sistemas que processam os dados é uma área com o GDPR que tem requisitos específicos, incluindo o acesso por administradores. Identidades com privilégios são todas as contas que têm privilégios elevados, como contas de usuário que são membros de administradores de domínio, administradores de empresa, administradores locais ou até mesmo os grupos de usuários avançados. Essas identidades também podem incluir contas que receberam privilégios diretamente, como a execução de backups, desligar o sistema ou outros direitos listados no nó de atribuição de direitos de usuário no console de diretiva de segurança Local.

Como um princípio de controle de acesso geral e em linha com o GDPR, você precisa proteger essas identidades com privilégios de comprometimento por invasores potenciais. Primeiro, é importante entender como as identidades são comprometidas; em seguida, você pode planejar impedir que invasores obtenham acesso a essas identidades com privilégios.

#### <a name="how-do-privileged-identities-get-compromised"></a>Como comprometidas identidades com privilégios?
Identidades com privilégios podem obter comprometidas quando as organizações não têm diretrizes para protegê-los. Veja estes exemplos:

- **Mais privilégios do que o necessário.** Um dos problemas mais comuns é que os usuários tenham mais privilégios que são necessárias para executar sua função de trabalho. Por exemplo, um usuário que gerencia o DNS pode ser um administrador do AD. Geralmente, isso é feito para evitar a necessidade de configurar diferentes níveis de administração. No entanto, se essa conta for comprometida, o invasor automaticamente tenha privilégios elevados.

- **Constantemente conectado com privilégios elevados.** Outro problema comum é que os usuários com privilégios elevados podem usá-lo por um tempo ilimitado. Isso é muito comum com profissionais de TI que entrar em um computador desktop usando uma conta privilegiada, permanecerem conectado e usam a conta com privilégios para navegar na web e usar o email (trabalho típico de IT funções de trabalho). Duração ilimitada de contas privilegiadas torna a conta mais suscetíveis a ataques e aumenta as chances de que a conta será comprometida.

- **Pesquisa de engenharia social.** A maioria das ameaças de credencial começam, pesquisando a organização e, em seguida, realizados por meio de engenharia social. Por exemplo, um invasor pode executar um ataque de phishing de email ao comprometimento de contas legítimos (mas não necessariamente contas com privilégios elevados) que têm acesso à rede da organização. O invasor, em seguida, usa essas contas válidas para executar a pesquisa adicional na sua rede e para identificar contas com privilégios que podem executar tarefas administrativas. 

- **Utilize contas com privilégios elevados.** Mesmo com uma conta de usuário normal, sem privilégios elevados na rede, os invasores podem ganhar acesso a contas com permissões elevadas. Então, um dos métodos mais comuns de fazer é usando o Pass-the-Hash ou ataques de passagem de Token. Para obter mais informações sobre o Pass-the-Hash e outras técnicas de roubo de credenciais, consulte os recursos sobre o [páginaPass-the-Hash (PtH)](https://technet.microsoft.com/dn785092.aspx).

É claro, há outros métodos que os invasores podem usar para identificar e comprometer identidades com privilégios (com novos métodos que estão sendo criados diariamente). Portanto, é importante que você estabeleça práticas recomendadas para os usuários façam logon com contas com privilégios mínimos para reduzir a capacidade de invasores para obter acesso para identidades com privilégios. As seções a seguir descrevem a funcionalidade em que o Windows Server pode atenuar esses riscos.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Administração just-in-Time (JIT) e Just Enough (JEA) de Admin
Enquanto a proteção contra Pass-the-Hash ou ataques Pass-the-Ticket é importante, as credenciais de administrador ainda podem ser roubadas por outros meios, incluindo engenharia social, funcionários descontentes e força bruta. Portanto, além de isolar credenciais tanto quanto possíveis, você também quer uma maneira para limitar o alcance dos privilégios de administrador, caso eles sejam comprometidos.

Atualmente, muitas contas de administrador estão com excesso de privilégios, mesmo se tiverem apenas uma área de responsabilidade. Por exemplo, um administrador DNS, que requer um conjunto muito estreito de privilégios para gerenciar servidores DNS, geralmente é concedido privilégios de nível de administrador de domínio. Além disso, porque essas credenciais são concedidas para perpetuamente, não há nenhum limite em quanto tempo eles podem ser usados.

Todas as contas com privilégios de nível de administrador de domínio desnecessários aumenta sua exposição aos invasores que buscam comprometer credenciais. Para minimizar a área da superfície de ataque, você deseja fornecer somente o conjunto específico de direitos que um administrador precisa fazer o trabalho – e apenas para a janela de tempo necessária para concluí-lo.

Usando o Just-in-Time e administração Just Enough Administration, os administradores podem solicitar privilégios específicos necessários para a janela exata de tempo necessária. Para um administrador DNS, por exemplo, usando o PowerShell para habilitar a administração Just Enough lhe permite criar um conjunto limitado de comandos que estão disponíveis para o gerenciamento de DNS.

Se o administrador DNS precisa fazer uma atualização para um dos seus servidores, ela pode solicitar acesso para gerenciar o DNS usando o Microsoft Identity Manager 2016. O fluxo de trabalho de solicitação pode incluir um processo de aprovação, como autenticação de dois fatores, que poderia chamar celular do administrador para confirmar sua identidade antes de conceder os privilégios solicitados. Uma vez concedido, esses privilégios DNS fornecem acesso à função do PowerShell para o DNS para um período de tempo específico.

Considere este cenário se as credenciais do administrador DNS foram roubadas. Em primeiro lugar, desde que as credenciais tiverem sem privilégios de administrador anexados a eles, o invasor seria capaz de acessar o servidor DNS – ou outros sistemas – fazer nenhuma alteração. Se o invasor tentou solicitar privilégios para o servidor DNS, autenticação de segundo fator seria peça para confirmar sua identidade. Desde que ele não é provável que o invasor tenha telefone celular do administrador DNS, a autenticação falhará. Isso seria o invasor fora do sistema de bloqueio e a organização de TI de alerta que as credenciais podem estar comprometidas.

Além disso, muitas organizações usam a versão gratuita [solução de senha de Administrador Local (LAPS)](http://aka.ms/laps) como um mecanismo de administração de JIT simple e poderosa para seus sistemas de servidor e cliente. A capacidade de LAPS fornece gerenciamento de senhas de contas locais de computadores ingressados no domínio. As senhas são armazenadas no Active Directory (AD) e protegidas por e lista de controle de acesso (ACL) portanto, os usuários qualificados só podem lê-lo ou solicitar sua redefinição.

Conforme observado na [guia de atenuação de roubo de credencial do Windows](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> "_os criminosos ferramentas e técnicas de usam para a realização de roubo de credenciais e melhorar a reutilização de ataques, invasores mal-intencionados estão achando mais fácil de atingir suas metas. Roubo de credenciais geralmente se baseia em práticas operacionais ou exposição de credencial do usuário, para que as mitigações em vigor exigem uma abordagem holística que trata de pessoas, processos e tecnologia. Além disso, esses ataques dependem do invasor roubo de credenciais depois de comprometer um sistema para expandir ou manter o acesso, para que as organizações devem conter violações rapidamente implementando estratégias que impedem que os invasores mover livremente e ser detectado, em um rede comprometido._ "

Uma consideração de design importantes para o Windows Server era reduzir o roubo de credenciais — em particular, derivados de credenciais. Credential Guard fornece segurança significativamente aprimorada contra roubo de credenciais derivadas e reutilização por implementar uma alteração significativa de arquitetura em Windows projetado para ajudar a eliminar ataques de isolamento com base em hardware em vez de simplesmente tentar Defenda-los.

Enquanto usando o Windows Defender Credential Guard, NTLM e Kerberos derivados de credenciais são protegidos usando a segurança baseada em virtualização, técnicas de ataque de roubo de credenciais e as ferramentas usadas em muitos ataques direcionados são bloqueados. O malware com privilégios administrativos em execução no sistema operacional não pode extrair segredos protegidos pela segurança baseada em virtualização. Enquanto o Windows Defender Credential Guard é uma poderosa mitigação, ameaça persistente ataques será provavelmente shift para novas técnicas de ataque e você também deve incorporar o Device Guard, conforme descrito abaixo, junto com outras estratégias de segurança e arquiteturas.

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard usa segurança baseada em virtualização para isolar as informações de credenciais, impedindo que os hashes de senha ou tíquetes Kerberos serem interceptados. Ele usa um processo autoridade de segurança Local (LSA) isolado totalmente nova, que não está acessível para o restante do sistema operacional. Usado por LSA isolado de todos os binários são assinados com certificados que são validados antes de iniciá-los no ambiente protegido, tornando a ataques do tipo Pass-the-Hash completamente ineficazes.

Usa o Windows Defender Credential Guard:

- Segurança baseada em virtualização (obrigatório). Também é necessário:

    - CPU de 64 bits

    - Extensões de virtualização de CPU, além de tabelas de páginas estendidas

    - Hipervisor do Windows

- Inicialização segura (obrigatório)

- TPM 2.0 distinto ou de firmware (preferencial - fornece a associação ao hardware)

Você pode usar o Windows Defender Credential Guard para ajudar a proteger identidades com privilégios, protegendo as credenciais e derivados de credenciais no Windows Server 2016. Para obter mais informações sobre os requisitos do Windows Defender Credential Guard, consulte [derivado de proteger as credenciais de domínio com o Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender Remote Credential Guard
Windows Defender Remote Credential Guard no Windows Server 2016 e atualização de aniversário do Windows 10 também ajuda a proteger as credenciais para usuários com conexões de área de trabalho remota. Anteriormente, qualquer pessoa que usar os serviços de área de trabalho remota precisaria fazer logon no seu computador local e, em seguida, ser necessária para fazer logon novamente quando eles executado uma conexão remota para seu computador de destino. Esse segundo logon seria passar credenciais para o computador de destino, expô-los a Pass-the-Hash ou ataques Pass-the-Ticket.

Com o Windows Defender Remote Credential Guard, Windows Server 2016 implementa o logon único para sessões de área de trabalho remota, eliminando a necessidade de inserir novamente seu nome de usuário e senha. Em vez disso, ele utiliza as credenciais que você já tiver usado para fazer logon no seu computador local. Para usar o Windows Defender Remote Credential Guard, o cliente de área de trabalho remota e o servidor devem atender aos seguintes requisitos:

- Deve estar associado a um domínio do Active Directory e estar no mesmo domínio ou em um domínio com uma relação de confiança.

- Deve usar a autenticação Kerberos.

- Deve estar executando pelo menos Windows 10 versão 1607 ou Windows Server 2016.  

- O aplicativo de Windows clássico de área de trabalho remota é necessário. O aplicativo de área de trabalho Universal Windows Platform remoto não oferece suporte de Credential Guard remoto do Windows Defender.

Você pode habilitar o Windows Defender Remote Credential Guard usando uma configuração do registro no servidor de área de trabalho remota e diretiva de grupo ou um parâmetro de Conexão de área de trabalho remota no cliente de área de trabalho remota. Para obter mais informações sobre como habilitar o Windows Defender Remote Credential Guard, consulte [credenciais de área de trabalho remota proteger com o Windows Defender Remote Credential Guard](https://docs.microsoft.com/windows/access-protection/remote-credential-guard). Como com de do Windows Defender Credential Guard, você pode usar de Credential Guard remoto do Windows Defender para ajudar a proteger identidades com privilégios no Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Proteger o sistema operacional para executar seus aplicativos e infraestrutura
Impedindo a ameaças cibernéticas também requer localização e o bloqueio de malware e ataques que buscam obter o controle Subvertendo as práticas operacionais padrão da sua infraestrutura. Se os invasores podem obter um sistema operacional ou aplicativo para ser executado de maneira não predeterminado e não é viável, provavelmente estão usando esse sistema para executar ações mal-intencionadas. Windows Server 2016 fornece camadas de proteção que bloqueiam os invasores externos executando um software mal-intencionado ou exploração de vulnerabilidades. O sistema operacional leva a uma função ativa na proteção dos aplicativos e infraestrutura, alertando os administradores de atividade que indica que um sistema tiver sido violado.

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 inclui o Windows Defender Device Guard para garantir que somente software confiável pode ser executado no servidor. Usando a segurança baseada em virtualização, ele pode limitar quais binários podem ser executados no sistema com base na política da organização. Se qualquer coisa que não sejam os binários especificados será executado, o Windows Server 2016 bloqueia e registra a falha na tentativa de forma que os administradores podem ver que houve uma possível violação. Notificação de violação é uma parte crítica dos requisitos de conformidade com GDPR.

Proteção do dispositivo do Windows Defender também é integrada com o PowerShell, de modo que você pode autorizar quais scripts possam ser executados em seu sistema. Em versões anteriores do Windows Server, os administradores podem ignorar a imposição de integridade de código ao simplesmente excluir a política do arquivo de código. Com o Windows Server 2016, você pode configurar uma política que é assinada pela sua organização para que apenas uma pessoa com acesso para o certificado que assinou a política pode alterar a política.

#### <a name="control-flow-guard"></a>Proteção de fluxo de controle 
Windows Server 2016 também inclui proteção interna contra algumas classes de ataques de corrupção de memória. Os servidores de aplicação de patch é importante, mas sempre há uma chance de que o malware poderia ser desenvolvido para uma vulnerabilidade que ainda não foram identificada. Alguns dos métodos mais comuns para a exploração dessas vulnerabilidades são fornecer dados incomuns ou extremos para um programa em execução. Por exemplo, um invasor pode explorar uma vulnerabilidade de estouro de buffer, fornecendo mais entradas para um programa que o esperado e ultrapassar a área reservada pelo programa para manter uma resposta. Isso pode corromper adjacentes na memória que podem conter um ponteiro de função.

Quando o programa chamar por meio dessa função, ele pode, em seguida, ir para um local não intencional, especificado pelo invasor. Esses ataques também são conhecidas como ataques de (JOP) programação orientada a salto. Proteção de fluxo de controle impede ataques JOP, colocando uma forte restrições em qual código de aplicativo pode ser executada – especialmente indireta chamar instruções. Ele adiciona verificações de segurança leve para identificar o conjunto de funções no aplicativo que são alvos válidos para chamadas indiretas. Quando um aplicativo é executado, ele verifica se esses destinos de chamada indireta são válidos.

Se a verificação de proteção de fluxo de controle falhar em tempo de execução, o Windows Server 2016 encerra imediatamente o programa, interromper qualquer exploração que tenta chamar indiretamente um endereço inválido. Proteção de fluxo de controle fornece uma camada adicional importante de proteção para proteção do dispositivo. Se um aplicativo de lista branca tiver sido comprometido, seria capaz de executar não verificado pela proteção do dispositivo, como o Device Guard triagem veria que o aplicativo foi assinado e é considerado confiável.

Mas porque a proteção de fluxo de controle pode identificar se o aplicativo está em execução em uma ordem não predeterminado, não é viável, o ataque falharia, impedindo a execução do aplicativo comprometida. Juntas, essas proteções tornam muito difícil para invasores injetar malware em software em execução no Windows Server 2016.

Os desenvolvedores que criam aplicativos onde os dados pessoais serão manipulados são incentivados a habilitar a proteção de fluxo de controle (CFG) em seus aplicativos. Esse recurso está disponível no Microsoft Visual Studio 2015 e é executado no "Reconhecimento de CFG" versões do Windows — o x86 e x64 versões para Desktop e servidor do Windows 10 e Windows 8.1 Update (KB3000850). Você não precisa habilitar o CFG para todas as partes do seu código, como uma mistura de CFG habilitado e código não-CFG habilitado será executado corretamente. Mas falha ao habilitar o CFG para todo o código pode abrir as lacunas na proteção. Além disso, o CFG habilitado o código funciona bem em "CFG sem reconhecimento de" versões do Windows e, portanto, é totalmente compatível com eles.

#### <a name="windows-defender-antivirus"></a>Windows Defender Antivírus
Windows Server 2016 inclui os setor à esquerda e ativa recursos de detecção do Windows Defender para bloquear malware conhecido. Windows Defender antivírus (AV) funciona em conjunto com o Windows Defender Device Guard e a proteção de fluxo de controle para impedir que código mal-intencionado de qualquer tipo que está sendo instalado em seus servidores. Ele é ativado por padrão – o administrador não precisa realizar nenhuma ação para que ele comece a trabalhar. Windows Defender AV também é otimizado para dar suporte as várias funções de servidor no Windows Server 2016. No passado, os invasores usavam shells, como o PowerShell para iniciar o código mal-intencionado de binário. No Windows Server 2016, o PowerShell agora está integrado com o Windows Defender AV para verificação de malware antes de iniciar o código.

Windows Defender AV é uma solução antimalware interna que fornece gerenciamento de segurança e antimalware para desktops, computadores portáteis e servidores. Windows Defender AV foi melhorada significativamente desde que foi apresentada no Windows 8. Windows Defender antivírus no Windows Server usa uma abordagem em várias frentes para melhorar o antimalware:

- **Proteção entregue na nuvem** ajuda a detectar e bloquear novo malware em segundos, mesmo que o malware não tenha sido visto antes.

- O **Contexto local avançado** aprimora a forma como o malware é identificado. Windows Server informa ao Windows Defender AV não apenas sobre conteúdo, como arquivos e processos, mas também onde o conteúdo de origem, onde ele foi armazenado e muito mais. 

- **Sensores globais amplo** ajudam a manter o Windows Defender AV atual e ciente do mesmo malware mais recente. Isso é feito de duas maneiras: coletando os dados de contexto local avançado de pontos de extremidade e analisando centralmente esses dados.

- **A violação** ajuda a proteger o Windows Defender AV em si contra ataques de malware. Por exemplo, o Windows Defender AV usa processos protegido, o que impede que os processos não confiáveis tentar adulterar os componentes do Windows Defender AV, suas chaves de registro e assim por diante.

- **Recursos de nível empresarial** fornecer aos profissionais de TI as ferramentas e opções de configuração necessárias para tornar o Windows Defender AV uma solução antimalware de classe empresarial.

#### <a name="enhanced-security-auditing"></a>Auditoria de segurança aprimorada 
Windows Server 2016 ativamente alerta os administradores de possíveis tentativas de violação com auditoria de segurança avançada que fornece informações mais detalhadas, que podem ser usadas para análise forense e detecção de ataque mais rápida. Ele registra os eventos da proteção de fluxo de controle, proteção do dispositivo do Windows Defender e outros recursos de segurança em um único local, tornando mais fácil para os administradores determinem o que os sistemas podem estar em risco.

Novas categorias de eventos incluem:

- **Associação de grupo de auditoria.** Permite auditar as informações de associação de grupo no token de logon do usuário. Eventos são gerados quando as associações de grupo são enumeradas ou consultadas no computador em que a sessão de logon foi criada. 
 
- **Auditar a atividade de PnP.** Permite fazer auditoria quando plug and play detecta um dispositivo externo – que pode conter malware. Eventos de PnP podem ser usados para rastrear as alterações no hardware do sistema. Uma lista de IDs de fornecedor de hardware está incluída no evento.

Windows Server 2016 se integra facilmente com sistemas de SIEM (gerenciamento) de eventos de incidente de segurança, como o Microsoft OMS Operations Management Suite (), que pode incorporar as informações em relatórios de inteligência sobre possíveis violações. A profundidade das informações fornecidas pela auditoria aprimorada habilita as equipes de segurança identificar e responder a possíveis violações de forma mais rápida e eficaz.

### <a name="secure-virtualization"></a>Virtualização segura
As empresas hoje virtualizar tudo o que pode, do SQL Server para o SharePoint para controladores de domínio do Active Directory. Máquinas virtuais (VMs) simplesmente tornam mais fácil de implantar, gerenciar, serviço e automatizar sua infraestrutura. Mas quando se trata de segurança, malhas de virtualização comprometido tornaram-se um novo vetor de ataque que é difícil de se defender contra – até agora. Da perspectiva do GDPR, é necessário pensar sobre como proteger VMs seria ao proteger servidores físicos, incluindo o uso da tecnologia do TPM VM.

Windows Server 2016 fundamentalmente altera como as empresas podem virtualização segura, incluindo várias tecnologias que permitem que você criar máquinas virtuais que serão executados somente em sua própria malha; ajudando a proteger contra o armazenamento, rede e serem executados em dispositivos do host.

#### <a name="shielded-virtual-machines"></a>Máquinas virtuais blindadas
As mesmas coisas que tornam muito fácil migrar as máquinas virtuais, backup e replicação, também tornam mais fácil de modificar e copiar. Uma máquina virtual é apenas um arquivo, portanto, ele não está protegido na rede, no armazenamento, em backups, ou em outro lugar. Outro problema é que os administradores da malha – sejam um administrador de armazenamento ou um administrador de rede – tem acesso a todas as máquinas virtuais.

Um administrador comprometido na malha pode facilmente resultar na comprometimento de dados entre máquinas virtuais. Tudo o que o invasor deve fazer é usar as credenciais comprometidas para copiar quaisquer arquivos VM que desejarem em uma unidade USB e orientá-lo fora da organização, onde esses arquivos da VM podem ser acessados de qualquer outro sistema. Se qualquer uma dessas VMs roubado fosse um controlador de domínio do Active Directory, por exemplo, o invasor facilmente pode exibir o conteúdo e usar técnicas de força bruta prontamente disponíveis para quebrar as senhas no banco de dados do Active Directory, por fim, dando-lhes acesso para todo o resto dentro de sua infra-estrutura.

Windows Server 2016 apresenta blindado VMs (máquinas virtuais Blindadas) para ajudar a proteger contra cenários como a que acabei de descrever. VMs blindadas incluem um dispositivo TPM virtual, que permite que as organizações a aplicar a criptografia de BitLocker para as máquinas virtuais e garantir que eles executarem somente em hosts confiáveis para ajudar a proteger contra administradores de host, rede e armazenamento comprometido. VMs blindadas são criadas usando VMs da geração 2, que dão suporte a firmware UEFI Unified Extensible Firmware Interface () e têm o TPM virtual.

#### <a name="host-guardian-service"></a>Serviço guardião de host
Juntamente com VMs Blindadas, o serviço guardião de Host é um componente essencial para a criação de uma malha de virtualização segura. Sua tarefa é atestar a integridade de um host Hyper-V antes de permitir uma VM Blindada para inicializar ou migrar para o host. Ele mantém as chaves para VMs Blindadas e não vai liberá-los até que a integridade da segurança é garantida. Há duas maneiras que você pode exigir que os hosts Hyper-V atestar a serviço guardião de Host.

A primeira e mais seguro, são Atestado confiável de hardware. Essa solução requer que as VMs Blindadas estão em execução em hosts que têm chips do TPM 2.0 e UEFI 2.3.1. Esse hardware é necessária para fornecer a inicialização medida e informações de integridade do kernel do sistema operacional exigidas pelo serviço guardião de Host para garantir que o host Hyper-V não foram violadas.

As organizações de TI tem a alternativa de usar o atestado de Admin confiável, que pode ser desejável se o hardware do TPM 2.0 não está em uso na sua organização. Esse modelo de Atestado é fácil de implantar como hosts são simplesmente colocados em um grupo de segurança e o serviço guardião de Host está configurado para permitir que as VMs Blindadas para execução em hosts que são membros do grupo de segurança. Com esse método, não há nenhuma medida complexa para garantir que o computador host não foi adulterado. No entanto, você eliminar a possibilidade de não criptografada de unidades do VMs saiam pela porta USB ou que a VM será executado em um host não autorizado. Isso ocorre porque os arquivos da VM não será executado em qualquer computador que não estejam no grupo designado. Se ainda não tiver o hardware do TPM 2.0, você pode começar com o atestado de Admin confiável e alternar para Atestado confiável de hardware, quando o hardware é atualizado.

#### <a name="virtual-machine-trusted-platform-module"></a>Máquina virtual Trusted Platform Module
Windows Server 2016 dá suporte a TPM para máquinas virtuais, que permite que você ofereça suporte a tecnologias de segurança avançadas, como criptografia de unidade do BitLocker® em máquinas virtuais. Você pode habilitar o suporte do TPM em qualquer máquina virtual de geração 2 Hyper-V usando o Gerenciador do Hyper-V ou o cmdlet Enable-VMTPM Windows PowerShell.

Você pode proteger o TPM virtual (vTPM), usando as chaves de criptografia locais armazenado no host ou armazenados no serviço guardião de Host. Portanto, enquanto o serviço guardião de Host exige mais infraestrutura, ele também fornece mais proteção.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Usando a rede definida pelo software de firewall de rede distribuída
É uma maneira de melhorar a proteção em ambientes virtualizados segmentar a rede de uma maneira que permite que as VMs se comunicar somente com os sistemas específicos necessários para a função. Por exemplo, se seu aplicativo não precisa se conectar à Internet, você pode particionar-o, eliminando esses sistemas como destinos de invasores externos. A rede definida por software (SDN) no Windows Server 2016 inclui um firewall de rede distribuída que permite que você crie dinamicamente as políticas de segurança que podem proteger seus aplicativos contra ataques provenientes de dentro ou fora de uma rede. Esse firewall de rede distribuída adiciona camadas para sua segurança, permitindo que você isolar os aplicativos na rede. Políticas podem ser aplicadas em qualquer lugar em sua infraestrutura de rede virtual, isolar o tráfego de VM para VM, o tráfego de host de VM ou o tráfego de VM para a Internet, onde for necessário – para sistemas individuais que podem ter sido comprometidos ou por meio de programação em várias sub-redes. Recursos de rede definida pelo software Windows Server 2016 também permitem que você rotear ou espelhar o tráfego de entrada para soluções de virtualização não Microsoft. Por exemplo, você pode optar por enviar todo o tráfego de email por meio de uma solução de virtualização Barracuda para proteção de filtragem de spam adicionais. Isso permite que você facilmente a camada de segurança adicional no local ou na nuvem.

### <a name="other-gdpr-considerations-for-servers"></a>Outras considerações de GDPR para servidores
O GDPR inclui normas explícitas para notificação de violação em que uma violação de dados pessoais significa, "_uma violação de segurança, levando a destruição acidental ou ilegal, perda, a alteração, divulgação não autorizada de, ou acesso a, pessoal dados transmitidos, armazenado ou caso contrário, processado._ "  Obviamente, você não pode começar a seguir em frente para atender às exigências de notificação do GDPR dentro de 72 horas, se você não pode detectar a violação em primeiro lugar.

Conforme observado na Central de segurança do Windows white paper, [violação de postagem: Lidando com ameaças avançadas](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> "_Ao contrário de violação de pré-lançamento, pós-violação pressupõe uma violação já ocorreu – que atua como um gravador de voo e o investigador de cena do Crime (CSI). Pós-violação fornece as equipes de segurança de informações e o conjunto de ferramentas necessárias para identificar, investigar e responder a ataques que, caso contrário, permanecerá não detectados e abaixo do radar._ "

Nesta seção vamos examinar como o Windows Server pode ajudá-lo a atender às suas obrigações de notificação de violação do GDPR. Isso começa com noções básicas sobre os dados subjacentes de ameaça disponíveis para a Microsoft que é coletada e analisado para o seu benefício e como, por meio do Windows Defender Advanced Threat ATP (proteção), esses dados podem ser essenciais para você.

#### <a name="insightful-security-diagnostic-data"></a>Dados de diagnóstico de segurança inteligente
Por quase duas décadas, Microsoft tem sido transformando ameaças em inteligência úteis que pode ajudar a fortalecer sua plataforma e proteger os clientes. Hoje em dia, com as imensas vantagens da computação fornecidas pela nuvem, estamos encontrando novas maneiras de usar nossas ferramentas ricas de análise direcionadas para atacar de forma inteligente as violações e proteger nossos clientes.

Aplicando uma combinação de processos automatizados e manuais, aprendizado de máquina e especialistas humanos, nós podemos criar um gráfico de segurança inteligente que aprende consigo mesmo e evolui em tempo real, reduzindo o nosso tempo coletivo para detectar e responder a incidentes novamente em nossos produtos.

![Gráfico de segurança de inteligência da Microsoft](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

O escopo da inteligência de ameaças da Microsoft abrange, literalmente, bilhões de pontos de dados: 35 bilhões de mensagens verificados mensalmente, 1 bilhão de clientes em segmentos corporativos e de consumidor acessar mais de 200 serviços de nuvem e as autenticações de 14 bilhões executadas diariamente. Todos esses dados são extraídos em seu nome pela Microsoft para criar o gráfico de segurança inteligente que pode ajudá-lo a proteger sua porta da frente de forma dinâmica para se manter seguro, permanecer produtivos e atender aos requisitos de GDPR.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Detectar ataques e investigação forense
Até mesmo as melhores defesas de ponto de extremidade podem ser violadas eventualmente, conforme cyberattacks se tornam mais sofisticados e direcionados. Dois recursos podem ser usados para ajudar com a detecção de violação potencial – Windows Defender Advanced Threat ATP (proteção) e Microsoft Advanced Threat Analytics (ATA).

Proteção Avançada contra Ameaças do Windows Defender (ATP) ajuda a detectar, investigar e responder a ataques avançados e violações de dados em suas redes. Os tipos de violação de dados de GDPR espera que você para se proteger contra por meio de medidas de segurança técnica para garantir a confidencialidade em andamento, integridade e disponibilidade dos dados pessoais e sistemas de processamento.

Entre os principais benefícios do Windows Defender ATP são os seguintes:

- **Detectando o não puder ser detectada.** Sensores criados profundamente o kernel do sistema operacional, especialistas de segurança do Windows e exclusiva ótica mais de 1 bilhão máquinas e sinais em todos os serviços da Microsoft.

- **Interna, não acrescentada.** Sem agente, com impacto mínimo, em nuvem; e de alto desempenho facilitar o gerenciamento sem nenhuma implantação. 

- **Único painel de segurança do Windows.** Explore 6 meses de rich, máquina-linha do tempo, unificando eventos de segurança do Windows Defender ATP, o Windows Defender antivírus e a proteção do dispositivo do Windows Defender.

- **Poder do Microsoft graph.** Aproveita a inteligência de segurança do Microsoft Graph para integrar a detecção e a exploração de assinatura de ATP do Office 365, para acompanhar novamente e responder a ataques.

Leia mais em [O que há de novo na prévia da atualização dos criadores do Windows Defender ATP](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

O ATA é um produto no local que ajuda a detectar o comprometimento de identidade em uma organização. O ATA pode capturar e analisar o tráfego de rede para autenticação, autorização e protocolos (como Kerberos, DNS, RPC, NTLM e outros protocolos) a coleta de informações. O ATA usa esses dados para criar um perfil comportamental sobre usuários e outras entidades em uma rede, para que ele possa detectar anomalias e padrões de ataques conhecidos. A tabela a seguir lista os tipos de ataque detectados pelo ATA.


|Tipo de ataque |Descrição |
|---------|---------|
|Ataques mal-intencionados |Esses ataques são detectados pelo procurando por ataques de uma lista conhecida de tipos de ataque, incluindo:<ul><li>Pass-the-Ticket (PtT)</li><li>Pass-the-Hash (PtH)</li><li>Overpass-the-Hash.</li><li>PAC forjado (MS14-068)</li><li>Golden Ticket</li><li>Replicações mal-intencionadas</li><li>Reconhecimento</li><li>Força bruta</li><li>Execução remota</li></ul>Para obter uma lista completa de ataques mal-intencionados que podem ser detectados e suas descrições, consulte [detectar quais atividades suspeitas pode o ATA?](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportamento anormal |Esses ataques são detectados usando análise comportamental e usam aprendizado de máquina para identificar atividades questionáveis, incluindo:<ul><li>Logons anormais</li><li>Ameaças desconhecidas</li><li>Compartilhamento de senha</li><li>Movimentação lateral</li></ul>|
|Riscos e problemas de segurança |Esses ataques são detectados observando a rede atual da configuração do sistema, incluindo:<ul><li>Confiança quebrada</li><li>Protocolos fracos</li><li>Vulnerabilidades de protocolo conhecidas</li></ul>|

Você pode usar o ATA para ajudar a detectar os invasores tentando comprometer identidades com privilégios. Para obter mais informações sobre a implantação do ATA, consulte os tópicos de planejamento, Design e implantação na [documentação do Advanced Threat Analytics](https://docs.microsoft.com/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Conteúdo relacionado de soluções associados do Windows Server 2016

- **Windows Defender antivírus:** https://www.youtube.com/watch?v=P1aNEy09NaI e https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **O Windows Defender, proteção avançada contra ameaças:** https://www.youtube.com/watch?v=qxeGa3pxIwg e https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Proteção do dispositivo do Windows Defender:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard de:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **Proteção de fluxo de controle:** https://msdn.microsoft.com/en-us/library/windows/desktop/mt637065(v=vs.85).aspx

- **Garantia e segurança:** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Aviso de isenção
Este artigo é um comentário em GDPR, como Microsoft interpreta, a partir da data de publicação. Nós gastamos muito tempo com GDPR e deseja que fizemos pense sobre sua intenção e o significado. Mas a aplicação de GDPR é altamente específica de fato, e nem todos os aspectos e interpretações de GDPR são bem liquidadas.

Como resultado, este artigo é fornecido apenas para fins informativos e não ser considerado como advogado ou para determinar como GDPR podem se aplicar a você e sua organização. Encorajamos você a trabalhar com um profissional legalmente qualificados para discutir GDPR, como ele se aplica especificamente para sua organização e como melhor garantir a conformidade.

A MICROSOFT NÃO OFERECE NENHUMA GARANTIA EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA EM RELAÇÃO ÀS INFORMAÇÕES CONTIDAS NESTE ARTIGO. Este artigo é fornecido "na condição em que se encontra". Informações e exibições expressas neste artigo, incluindo URL e outras referências de site da Internet, podem ser alteradas sem aviso prévio.

Este artigo não fornece direitos legais e nenhuma propriedade intelectual sobre qualquer produto da Microsoft.  Você pode copiar e usar este artigo apenas para fins de referência interna.  

Publicado em setembro de 2017<br>
Versão 1.0<br>
© 2017 Microsoft. Todos os direitos reservados.


