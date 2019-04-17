---
title: "Comece sua jornada estatuto de proteção de dados geral (GDPR) para Windows Server 2016"
description: "Use este artigo para entender o que é GDPR e sobre os produtos Microsoft fornece para ajudá-lo a começar em direção a conformidade."
keywords: Privacidade, GDPR
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: e68b8d926f7749f7fcecf5f3752822c54db5ce76
ms.sourcegitcommit: c5aa1eedd1383b8b8f6b8fcb0af995c28277002a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2018
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>Comece sua jornada estatuto de proteção de dados geral (GDPR) para o Windows Server 

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este artigo fornece informações sobre o GDPR, incluindo o que é, e os produtos Microsoft fornece para ajudar você a ficar em conformidade.

## <a name="introduction"></a>Introdução
Em 25 de maio de 2018, as leis de privacidade Europeu é devido a entram em vigor que define uma nova barra global para direitos de privacidade, segurança e conformidade.

Regulamento de proteção de dados geral ou GDPR, é fundamentalmente sobre protegendo e permitindo que os direitos de privacidade de indivíduos. O GDPR estabelece requisitos de privacidade rígidas global que regem como gerenciar e proteger dados pessoais enquanto respeitar escolha individual — não importa onde dados são enviados, processados ou armazenados.

A Microsoft e nossos clientes agora estão em uma viagem para alcançar as metas de privacidade do GDPR. Na Microsoft, acreditamos privacidade é um direito fundamental, e acreditamos que o GDPR é uma importante etapa para esclarecer e habilitando direitos de privacidade individuais. Mas reconhecemos que o GDPR exigirá alterações significativas por organizações em todo o mundo.

Nós descrevemos nosso compromisso com a GDPR e como podemos estiver dando suporte a nossos clientes dentro do [obter GDPR ' em conformidade com a Microsoft Cloud](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99) postagem de blog pelo nosso diretor de privacidade [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/) e o [Earning seu relação de confiança com compromissos contratuais para a norma de proteção de dados geral](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99)"postagem de blog por [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/) -vice-presidente corporativo da Microsoft e o representante jurídico.

Embora sua jornada de conformidade GDPR pode parecer um desafio, estamos aqui para ajudá-lo. Para obter informações específicas sobre o GDPR, nossos compromissos e como começar sua jornada, visite o [seção GDPR do Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="gdpr-and-its-implications"></a>GDPR e suas implicações
O GDPR é um estatuto complexo que pode exigir alterações significativas na forma como você coleta, usa e gerenciar dados pessoais. A Microsoft tem um longo histórico de ajudar nossos clientes a cumprir as normas complexas e quando se trata de se preparar para o GDPR, estamos seu parceiro nesta jornada.

O GDPR impõe regras em organizações que oferecem bens e serviços para pessoas na União Europeia (UE), ou que coletam e analisar dados vinculados a residentes da UE, não importa onde essas empresas estão localizadas. Entre os principais elementos do GDPR são as seguintes:

- **Direitos de privacidade pessoal avançados.** Reforçada proteção de dados para residentes da UE, garantindo que eles têm o direito de acessar seus dados pessoais, para imprecisões corretos nesses dados para apagar dados, ao objeto de processamento de seus dados pessoais e para movê-lo.

- **Imposto maior para proteger os dados pessoais.** Estrutura reforçada responsabilidade de organizações que processam dados pessoais, fornecendo maior clareza de responsabilidade garantir a conformidade.

- **Relatórios de violação de dados pessoais obrigatórios.** As organizações que controlam os dados pessoais são necessárias para violações de dados pessoais de relatório que representam um risco para os direitos e a liberdade de indivíduos para suas autoridades de supervisão sem atrasos desnecessários e, onde possível, não depois de 72 horas de uma vez que eles ficar cientes de violação.

Como você pode antecipar, o GDPR pode ter um impacto significativo em seus negócios, potencialmente exigir que você atualizar as políticas de privacidade, implementar e fortalecer controles de proteção de dados e violar procedimentos de notificação, implantar políticas altamente transparentes, e ainda mais investir em IT e treinamento. Microsoft Windows 10 pode ajudá-lo efetivamente e lidar com alguns desses requisitos com eficiência.

## <a name="personal-and-sensitive-data"></a>Dados pessoais e confidenciais
Como parte de seus esforços para estar em conformidade com a GDPR, você precisará entender como a norma define dados pessoais e confidenciais e como essas definições se relacionam aos dados mantidos por sua organização. Com base no que Noções básicas sobre, que você poderá descobrir onde esses dados são criados, processadas, gerenciadas e armazenados.

O GDPR considera dados pessoais para ser quaisquer informações relacionadas a uma pessoa identificável ou identificada natural. Que podem incluir identificação direta (como seu nome legal) e identificação indireta (como informações específicas que o torna limpá-lo é a você as referências de dados). O GDPR também deixa claro que o conceito de dados pessoais inclui identificadores online (como, endereços IP, IDs de dispositivo móvel) e dados de localização.

O GDPR apresenta definições específicas para genética dados (como a sequência de gene um indivíduo) e os dados biométricos. Dados genética e dados biométricos juntamente com outros sub categorias de dados pessoais (dados pessoais, revelando origem racial ou étnica, opiniões políticas, religiosas ou filosóficas ou filiação: dados relativos a integridade; ou dados relativos a um a vida útil de sexo ou orientação sexual da pessoa) são tratados como dados pessoais confidenciais sob o GDPR. Dados pessoais confidenciais são suportados proteções avançadas e geralmente requer o consentimento explícito de uma pessoa onde esses dados devem ser processadas.

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>Exemplos de informações referentes a uma pessoa natural identificada ou identificável (sujeito dados)
Esta lista fornece exemplos de vários tipos de informações que ser controladas por meio de GDPR. Isso não é uma lista completa.

-   Nome

-   Número de identificação (como SSN)

-   Dados de localização (por exemplo, endereço residencial)

-   Identificador online (como endereço de email, nomes de tela, endereço IP, IDs de dispositivo)

-   Pseudoanônima com dados (como, usando uma chave para identificar indivíduos)

-   Dados genética (como biológicos amostras de outra pessoa)

-   Dados biométricos (como impressões digitais, reconhecimento facial)

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>Guia de introdução sobre a jornada até a conformidade de GDPR
Dada a quantidade é complicado para se tornar GDPR compatível, é altamente recomendável que você não esperar para preparar até que começa de imposição. Você deve revisar sua privacidade e as práticas de gerenciamento de dados agora. Nós recomendamos que você comece sua jornada de conformidade GDPR, concentrando-se em quatro etapas principais:

-   **Descubra.** Identifique quais dados pessoais que você tem e onde estejam. 

-   **Gerencie.** Regem dados pessoais como é usada e acessados.

-   **Protege.** Estabelece controles de segurança para evitar, detectar e responder a vulnerabilidades e violações de dados.  

-   **Relatório.** Agir sobre solicitações de dados, violações de dados de relatório e mantenha a documentação necessária.

    ![Diagrama sobre como as 4 etapas principais GDPR funcionam juntos](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

Para cada uma das etapas, criamos recursos em várias soluções da Microsoft, que podem ser usados para ajudá-lo a atender aos requisitos dessa etapa, recursos e ferramentas de exemplo. Embora este artigo não é um guia de "instruções" abrangente, incluímos links para você saber mais detalhes e mais informações estão disponíveis no [seção GDPR do Microsoft Trust Center](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr).

## <a name="windows-server-security-and-privacy"></a>Privacidade e segurança do Windows Server
O GDPR requer que você implementar medidas de segurança de técnicas e organizacional apropriadas para proteger dados pessoais e os sistemas de processamento. No contexto do GDPR, seus ambientes de servidores físicos e virtuais são potencialmente processando dados pessoais e confidenciais. Processamento pode significar qualquer operação ou um conjunto de operações, como a coleta de dados, armazenamento e recuperação.

Sua capacidade de atender a esse requisito e implementar medidas de segurança technical apropriado deve refletir as ameaças que enfrentam no ambiente de TI cada vez mais hostil de hoje. Cenário de ameaças de segurança de hoje é uma das ameaças agressivas e obstinadas. Nos anos anteriores, invasores mal-intencionados se concentravam principalmente em tenham reconhecimento da comunidade por meio de seus ataques ou a emoção do temporariamente colocando um sistema offline. Desde então, motivação dos invasores passou ganhando dinheiro, incluindo segurando dispositivos e dados até que o proprietário paga o resgate exigido.

Ataques modernos cada vez mais se concentrar no roubo de propriedade intelectual em grande escala; na degradação do sistema que pode resultar em perda financeira; e agora até mesmo o cyberterrorismo que ameaça a segurança de indivíduos, empresas e interesses nacionais em todo o mundo. Esses invasores são geralmente indivíduos altamente treinados e especialistas em segurança, alguns dos quais estão a serviço de estados nacionais que possuem grandes orçamentos e recursos humanos aparentemente ilimitados. Ameaças como essas exigem uma abordagem que pode superar esse desafio.

Não só são essas ameaças um risco à sua capacidade de manter o controle de quaisquer dados pessoais ou confidenciais, que talvez seja necessário, mas são também um risco de material a seus negócios em geral. Considere a possibilidade de dados recentes do McKinsey, Ponemon Institute, Verizon e Microsoft:

- O custo médio do tipo de violação de dados a GDPR esperam que você relatar é US $3. 5M.

- 63% dessas violações envolvem fracas ou roubadas senhas que o GDPR espera ao endereço.

- Mais de 300.000 novas amostras de malware são criadas e afaste todos os dias, tornando sua tarefa ainda mais desafiador para proteção de dados de endereço.

Conforme visto com os ataques Ransomware recentes, chamados uma vez a praga preta do Internet, os invasores serão após alvos maiores que podem pagar para pagar mais com consequências potencialmente catastróficas. O GDPR inclui penalidades que tornam seus sistemas, incluindo desktops e notebooks, que contêm dados pessoais e confidenciais realmente destinos avançada.

Dois princípios-chave têm interativa e continuam orientar o desenvolvimento do Windows:

- **Segurança.** Os dados de que nosso software e nossos serviços armazenam em nome de nossos clientes devem ser protegidos contra danos e usados ou modificados apenas de maneiras apropriadas. Modelos de segurança devem ser fácil para os desenvolvedores entender e criar em seus aplicativos.

- **Privacidade.** Os usuários devem estar no controle de como os dados são usados. Políticas para o uso de informações devem ficar claras para o usuário. Os usuários devem ser no controle de quando e se eles recebem informações para fazer o melhor uso do tempo. Ele deve ser fácil para os usuários especifiquem o uso apropriado de suas respectivas informações incluindo controlando o uso de email que eles enviam.

Microsoft permaneceu sólido contra esses princípios recentemente notados pela CEO da Microsoft, Satya Nadella 

> "_Conforme o mundo continua a alterar e requisitos de negócios evoluem, algumas coisas que sejam consistentes: demanda do cliente para segurança e privacidade. _"

Como você trabalha para estar em conformidade com a GDPR, Noções básicas sobre a função de seus servidores físicos e virtuais em criar, acessar, processamento, armazenar e gerenciar dados que podem se qualificar como pessoais e dados potencialmente confidenciais sob o GDPR são importantes. Windows Server fornece recursos que ajudarão a cumprir os requisitos de GDPR implementar medidas de segurança de técnicas e organizacional apropriadas para proteger dados pessoais.

A postura de segurança do Windows Server 2016 não é um raio-on; ele é um princípio arquitetônico. Além disso, ele pode ser entendido melhor em quatro objetos:

- **Protege.** Foco em andamento e inovação em medidas preventivas; bloquear ataques conhecidos e conhecidas de malware.

- **Detecte.** Abrangente de ferramentas para ajudá-lo anormalidades especiais e responder a ataques mais rápidas de monitoramento.

- **Responda.** Principais tecnologias de resposta e recuperação mais profundo consultoria experiência.

- **Isole.** Isolar segredos de dados e componentes do sistema operacional, limitar privilégios de administrador e medir rigorosamente o funcionamento de host.

Com o Windows Server, sua capacidade de proteger, detectar e defenda-se contra os tipos de ataques que podem levar a violações de dados foi bastante aprimorada. Considerando requisitos rígidos que envolvem às notificações de violação dentro do GDPR, garantir que seus sistemas de desktop e laptop são bem protegiam diminuirá os riscos decorrentes que poderiam resultar na notificação e análise de violação barata.

A seção a seguir, você verá como o Windows Server fornece recursos que se enquadram centralizado no estágio de "Proteger" de sua jornada de conformidade GDPR. Esses recursos se enquadram em três cenários de proteção:

- **Proteger suas credenciais e limitar privilégios de administrador.** Windows Server 2016 ajuda a implementar essas mudanças, para ajudar a impedir que o sistema está sendo usado como um ponto de partida para ainda mais invasões.

- **Proteger o sistema operacional para executar seus aplicativos e infraestrutura.** Windows Server 2016 fornece camadas de proteção, que é útil para impedir que invasores externos executando o software mal-intencionado ou vulnerabilidades de exploração.

- **Virtualização segura.** Windows Server 2016 permite a virtualização segura, usando protegido máquinas virtuais e malha protegidos. Isso ajuda a criptografar e execute suas máquinas virtuais em hosts confiáveis em sua malha, melhor protegendo-los contra ataques mal-intencionados.

Esses recursos, discutidos com mais detalhes abaixo com referências aos requisitos específicos de GDPR, são criados em cima de proteção avançada de dispositivo que ajuda a manter a integridade e a segurança do sistema operacional e dos dados.

Uma chave provisionar dentro do GDPR é proteção de dados por design e por padrão, e ajuda com sua capacidade de atender esta cláusula são recursos no Windows 10, como criptografia de dispositivo BitLocker. O BitLocker usa a tecnologia Trusted Platform Module (TPM), que fornece funções relacionadas à segurança baseada em hardware. Esse chip de processador criptográfico inclui vários mecanismos de segurança física para torná-lo resistente a adulterações e software mal-intencionado é a adulterações com as funções de segurança do TPM.

O chip inclui vários mecanismos de segurança física para torná-lo resistente a adulterações e software mal-intencionado é a adulterações com as funções de segurança do TPM. Algumas das principais vantagens do uso da tecnologia TPM são a possibilidade de:

-   Gerar, armazenar e limitar o uso de chaves de criptografia.

-   Use a tecnologia TPM para autenticação de dispositivo de plataforma usando a chave RSA exclusiva do TPM que é gravado no próprio.

-   Ajude a garantir a integridade da plataforma realizando e armazenando medidas de segurança.

Proteção avançada de dispositivo adicionais relevante para seu operando sem violações de dados incluem a inicialização confiável do Windows para ajudar a manter a integridade do sistema, garantindo malware é não é possível iniciar antes das defesas do sistema.

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server: Sua jornada de conformidade GDPR suporte
Principais recursos dentro do Windows Server podem ajudá-lo de forma eficiente implementar os mecanismos de segurança e privacidade que requer o GDPR conformidade. Embora o uso desses recursos não garante a conformidade, elas darão suporte seus esforços para fazer isso.

O sistema operacional fica em uma camada estratégica na infraestrutura da organização, oferecendo novas oportunidades para criar camadas de proteção contra ataques que podem roubar dados e interrompam seus negócios. Aspectos-chave de GDPR como privacidade por Design, proteção de dados e controle de acesso precisam ser tratados dentro de sua infraestrutura de TI no nível do servidor.

Trabalhando para proteger a identidade, o sistema operacional e a camadas de virtualização, Windows Server 2016 ajuda a bloquear os vetores de ataque comum usados para acessar seus sistemas ilícito: roubo de credenciais, malware e uma malha de virtualização comprometido. Além de reduzir o risco de negócios, os componentes de segurança integrado ao Windows Server 2016 ajuda atender aos requisitos de conformidade de chave governo e do setor normas de segurança. 

Esses identidade, do sistema operacional e proteções de virtualização permitem que você proteger melhor seu datacenter executando o Windows Server como uma VM em qualquer nuvem e limitar a capacidade de invasores comprometer as credenciais, inicie o malware e permanecer não detectados no seu rede. Da mesma forma, quando implantado como um host do Hyper-V, o Windows Server 2016 oferece garantia de segurança para seus ambientes de virtualização por meio de recursos de firewall distribuído e protegido máquinas virtuais. Com o Windows Server 2016, o sistema operacional se torna um participante ativo em sua segurança datacenter.

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>Proteger suas credenciais e limitar privilégios de administrador 
Controle sobre o acesso aos dados pessoais e os sistemas que processam os dados, é uma área com a GDPR que possui requisitos específicos, incluindo o acesso por administradores. Identidades privilegiadas são todas as contas que têm privilégios elevados, como contas de usuário que são membros dos administradores de domínio, administradores de empresa, os administradores locais ou até mesmo os grupos de usuários avançados. Tais identidades também podem incluir contas que foram concedidas privilégios diretamente, como executar backups, desligar o sistema ou outros direitos listados no nó atribuição de direitos de usuário no console de política de segurança Local.

Como um princípio de controle de acesso geral e em linha com o GDPR, você precisa proteger essas identidades privilegiadas de comprometimento por invasores em potencial. Primeiro, é importante compreender como identidades estão comprometidas; em seguida, você pode planejar impedir que invasores acessem identidades esses privilegiado.

#### <a name="how-do-privileged-identities-get-compromised"></a>Como ficar comprometidas identidades privilegiadas?
Identidades privilegiadas podem ficar comprometidas quando as organizações não têm diretrizes para protegê-los. Estes são exemplos:

- **Mais privilégios do que são necessárias.** Um dos problemas mais comuns é que os usuários têm mais privilégios do que são necessários para executar sua função no trabalho. Por exemplo, um usuário que gerencia DNS pode ser um administrador de anúncios. Geralmente, isso é feito para evitar a necessidade de configurar diferentes níveis de administração. No entanto, se tal uma conta é comprometida, o invasor automaticamente tem privilégios elevados.

- **Constantemente conectado com privilégios elevados.** Outro problema comum é que os usuários com privilégios elevados podem usá-lo por um tempo ilimitado. Isso é muito comum com os profissionais de TI que fazer logon um computador desktop usando uma conta com privilégios, permanecem conectado e usam a conta com privilégios para navegar na web e usar email (trabalho típico de IT funções de trabalho). Duração ilimitada de contas privilegiadas faz com que a conta mais suscetíveis a ataques e aumenta as chances de que a conta será comprometida.

- **Pesquisa de engenharia social.** A maioria das ameaças de credencial começa pesquisando a organização e, em seguida, conduzida pelo engenharia social. Por exemplo, um invasor pode realizar um ataque de phishing de email para contas legítimos comprometimento (mas não necessariamente com privilégios elevadas contas) que tenham acesso à rede da organização. O invasor usa essas contas válidas para realizar pesquisas adicionais em sua rede e para identificar contas privilegiadas que podem realizar tarefas administrativas. 

- **Aproveite as contas com privilégios elevados.** Mesmo com uma conta de usuário normal, sem execução privilegiada na rede, os invasores podem ter acesso às contas com permissões elevadas. Portanto, um dos métodos mais comuns de fazer é usando o Pass-the-Hash ou ataques Pass-the-Token. Para obter mais informações sobre o Pass-the-Hash e outras técnicas de roubo de credenciais, consulte os recursos sobre o [Pass-the-Hash (PtH)](https://technet.microsoft.com/en-us/dn785092.aspx).

É claro, há outros métodos que os invasores podem usar para identificar e comprometer identidades privilegiadas (com novos métodos sendo criados todos os dias). Portanto, é importante que você estabelece práticas recomendadas para os usuários façam logon com contas de menos privilegiada para reduzir a capacidade de invasores acessem identidades privilegiadas. As seções abaixo descrevem funcionalidade onde o Windows Server pode atenuar esses riscos.

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>Administração just-in-Time (JIT) e suficientes administrador (JEA)
Enquanto a proteção contra Pass-the-Hash ou Pass-the-Ticket ataques é importante, credenciais de administrador ainda podem ser roubadas por outros meios, incluindo engenharia social, funcionários insatisfeitos e ataques de força bruta. Portanto, além de isolar credenciais tanto quanto possíveis, você também querem uma maneira de limitar o alcance de privilégios de administrador no caso de serem comprometidos.

Hoje em dia, muitas contas de administrador são com excesso de privilégios, mesmo se têm apenas uma área de responsabilidade. Por exemplo, um administrador DNS, que requer um conjunto muito estreito de privilégios para gerenciar servidores DNS, geralmente é concedido privilégios de nível do administrador do domínio. Além disso, porque essas credenciais são concedidas para sempre, não há nenhum limite quanto tempo eles podem ser usados.

Cada conta com privilégios de nível do administrador do domínio desnecessários aumenta a exposição para os invasores que tentam comprometer as credenciais. Para minimizar a superfície de ataque, você deseja fornecer apenas o conjunto específico de direitos que um administrador precisa fazer o trabalho – e somente para a janela de tempo necessário para concluir a ele.

Usando apenas suficiente de administração e Just-in-Time administração, os administradores podem solicitar os privilégios específicos que precisam da janela exata de tempo necessário. Para um administrador DNS, por exemplo, usando o PowerShell para permitir que apenas suficiente administração permite criar um conjunto limitado de comandos que estão disponíveis para o gerenciamento de DNS.

Se o administrador DNS precisa fazer uma atualização para uma das suas servidores, ela pode solicitar acesso para gerenciar o DNS usando o Microsoft Identity Manager 2016. O fluxo de trabalho de solicitação pode incluir um processo de aprovação, como autenticação de dois fatores, que poderia chamar celular do administrador para confirmar sua identidade antes de conceder os privilégios solicitados. Depois de concedida, esses privilégios DNS fornecem acesso à função do PowerShell para o DNS para um determinado intervalo de tempo.

Imagine esse cenário se as credenciais do administrador do DNS foram roubadas. Primeiro, já que as credenciais tiverem sem privilégios de administrador ligados a eles, o invasor não será capaz de acessar o servidor DNS – ou outros sistemas – façam alterações. Se o invasor tentou solicitar privilégios para o servidor DNS, autenticação de fator second pergunta-los para confirmar sua identidade. Desde que não é provável que o invasor tenha celular do administrador DNS, a autenticação falhará. Isso seria o invasor no sistema de bloqueio e alertar o departamento de TI que as credenciais podem ser comprometidas.

Além disso, muitas organizações usam o gratuito [solução de senha de Administrador Local (VOLTAS)](http://aka.ms/laps) como um mecanismo de administração de JIT simple, mas poderoso para seus sistemas de servidor e cliente. A funcionalidade de VOLTAS fornece gerenciamento de senhas de conta local de computadores ingressados no domínio. As senhas são armazenadas no Active Directory (AD) e protegidas pelo e lista de controle de acesso (ACL) então os usuários só qualificados podem lê-lo ou solicitar sua restauração.

Conforme observado no [Windows credenciais roubo mitigação guia](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095), 

> "_os criminosos ferramentas e técnicas usam para realizar o roubo de credenciais e melhorar a reutilização de ataques, invasores mal-intencionados são encontrá-lo mais fácil atingir seus objetivos. Roubo de credenciais geralmente depende práticas operacionais ou exposição de credenciais do usuário, para que mitigações eficazes exigem uma abordagem abrangente que aborda as pessoas, processos e tecnologia. Além disso, esses ataques dependem o invasor roubo de credenciais depois de comprometer um sistema para expandir ou persistir acesso, para que as organizações devem conter violações rapidamente implementando estratégias que impedir que invasores mover livremente e sem ser detectado em um rede comprometido. _"

Uma consideração importante do design para o Windows Server foi minimizando o roubo de credenciais — em particular, as credenciais derivadas. O Credential Guard oferece segurança aprimorada significativamente contra roubo de credenciais derivadas e reutilização implementando uma alteração arquitetônica significativa no Windows, projetado para ajudar a eliminar a ataques de isolamento baseada em hardware, em vez de simplesmente tentando Defenda-se contra-los.

Enquanto usando o Windows Defender Credential Guard, NTLM e Kerberos credenciais derivadas estejam protegidas usando a segurança baseada em virtualização, ataques de roubo de credenciais técnicas e ferramentas usadas em muitos ataques direcionados são bloqueados. O malware em execução no sistema operacional com privilégios administrativos não pode extrair segredos protegidos pela segurança baseada em virtualização. Enquanto o Windows Defender Credential Guard seja uma mitigação eficiente, ataques de ameaças persistentes provavelmente adotarão novas técnicas de ataque e você também deverá incorporar o Device Guard, conforme descrito abaixo, juntamente com outras estratégias de segurança e arquiteturas.

#### <a name="windows-defender-credential-guard"></a>Proteção de credenciais do Windows Defender
Windows Defender Credential Guard utiliza segurança baseada em virtualização para isolar informações de credenciais, impedindo que hashes de senha ou tíquetes Kerberos sendo interceptada. Ele usa um processo autoridade de segurança Local (LSA) isolado totalmente novo, que não é acessível ao restante do sistema operacional. Todos os binários usados pela LSA isolada são assinados com certificados que são validados antes de iniciá-los no ambiente protegido, tornando os ataques do tipo Pass-the-Hash completamente ineficazes.

Proteção de credenciais do Windows Defender usa:

- Segurança baseada em virtualização (obrigatório). Também é necessário:

    - CPU de 64 bits

    - Extensões de virtualização de CPU, além de tabelas de página estendida

    - Hipervisor do Windows

- Inicialização segura (obrigatória)

- O TPM 2.0 seja discreto ou firmware (preferencial - fornece a associação de hardware)

Você pode usar o Windows Defender Credential Guard para ajudar a proteger as identidades privilegiadas protegendo as credenciais e os elementos derivados de credenciais no Windows Server 2016. Para obter mais informações sobre os requisitos do Windows Defender Credential Guard, consulte [derivado de proteger as credenciais de domínio com o Windows Defender Credential Guard](https://docs.microsoft.com/en-us/windows/access-protection/credential-guard/credential-guard).

#### <a name="windows-defender-remote-credential-guard"></a>Proteção de credencial remota do Windows Defender
Windows Defender remoto Credential Guard no Windows Server 2016 e atualização de aniversário do Windows 10 também ajuda a proteger as credenciais para usuários com conexões de área de trabalho remota. Anteriormente, qualquer pessoa que use os serviços de área de trabalho remota precisaria fazer logon sua máquina local e, em seguida, ser necessárias para fazer logon novamente quando eles realizada uma conexão remota em sua máquina de destino. Essa segunda logon seria passar credenciais para o computador de destino, expô-los a Pass-the-Hash ou Pass-the-Ticket ataques.

Com o Windows Defender remoto Credential Guard, o Windows Server 2016 implementa logon único para sessões de área de trabalho remota, eliminando a necessidade de inserir novamente seu nome de usuário e senha. Em vez disso, ele utiliza as credenciais que você já usou para fazer logon em sua máquina local. Para usar o Windows Defender remoto Credential Guard, o cliente de área de trabalho remota e o servidor devem atender aos seguintes requisitos:

- Deve estar associado a um domínio do Active Directory e estar no mesmo domínio ou em um domínio com uma relação de confiança.

- Deve usar a autenticação Kerberos.

- Deve estar executando pelo menos Windows 10 versão 1607 ou Windows Server 2016.  

- Aplicativo clássico do Windows a área de trabalho remota é necessário. O aplicativo Remote Desktop plataforma Universal do Windows não dá suporte a proteção de credenciais remota do Windows Defender.

Você pode habilitar o Windows Defender remoto Credential Guard usando uma configuração de registro no servidor de área de trabalho remota e política de grupo ou um parâmetro de Conexão de área de trabalho remota no cliente de área de trabalho remota. Para obter mais informações sobre como ativar o Windows Defender remoto Credential Guard, consulte [credenciais proteger área de trabalho remota com o Windows Defender remoto Credential Guard](https://docs.microsoft.com/en-us/windows/access-protection/remote-credential-guard). Como com o Windows Defender Credential Guard, você pode usar a proteção de credenciais remota do Windows Defender para ajudar a proteger as identidades privilegiadas no Windows Server 2016.

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>Proteger o sistema operacional para executar seus aplicativos e a infraestrutura
Também impedir ameaças cibernéticas requer a localização e bloquear o malware e ataques que buscam controlar Subvertendo as práticas de operacionais padrão de sua infraestrutura. Se os invasores podem obter um sistema operacional ou o aplicativo seja executado de forma não predeterminado, não seja viável, eles provavelmente estão usando desse sistema realizasse ações mal-intencionado. Windows Server 2016 fornece camadas de proteção que bloqueiam invasores externos executando o software mal-intencionado ou vulnerabilidades de exploração. O sistema operacional assume uma função ativa em proteger a infraestrutura e aplicativos alertando administradores a atividade que indica que um sistema foi violado.

#### <a name="windows-defender-device-guard"></a>Device Guard do Windows Defender
Windows Server 2016 inclui o Windows Defender Device Guard para garantir que somente softwares confiáveis podem ser executados no servidor. Usando a segurança baseada em virtualização, ele pode limitar quais binários podem ser executados no sistema com base na política da organização. Se nada, que não sejam os binários especificados tenta executar, o Windows Server 2016 bloqueia e registra a tentativa com falha para que os administradores possam ver o que houve uma violação potencial. Notificação de violação é uma parte essencial dos requisitos de conformidade GDPR.

Device Guard do Windows Defender também está integrado com o PowerShell para que você pode autorizar quais scripts podem ser executados em seu sistema. Em versões anteriores do Windows Server, os administradores podem ignorar a imposição de integridade de código, simplesmente excluindo a política do arquivo de código. Com o Windows Server 2016, você pode configurar uma política que é assinada por sua organização para que somente uma pessoa com acesso ao certificado assinado a política pode alterar a política.

#### <a name="control-flow-guard"></a>Proteção do fluxo de controle 
Windows Server 2016 também inclui proteção interna contra algumas classes de ataques de corrupção de memória. Os servidores de aplicação de patch é importante, mas sempre há uma chance de que o malware pode ser desenvolvido para uma vulnerabilidade ainda não foi identificada. Alguns dos métodos mais comuns para explorar essas vulnerabilidades são fornecer dados incomuns ou extremos para um programa em execução. Por exemplo, um invasor pode explorar uma vulnerabilidade de estouro de buffer, fornecendo a entrada mais para um programa que o esperado e saturação a área reservada pelo programa para manter uma resposta. Isso pode danificar a memória adjacente que pode conter um ponteiro de função.

Quando o programa chama essa função, ele, em seguida, pode ir para um local não intencional especificado pelo invasor. Esses ataques também são conhecidos como orientado por atalhos programação ataques (JOP). Proteção de fluxo de controle impede os ataques JOP colocando firme restrições em qual código do aplicativo pode ser executado – especialmente indireto chamar instruções. Ele adiciona verificações de segurança leve para identificar o conjunto de funções no aplicativo que são válidos destinos para chamadas indiretos. Quando um aplicativo é executado, ele verifica se esses destinos chamada indireto são válidos.

Se a verificação de proteção de fluxo de controle falhar no tempo de execução, o Windows Server 2016 encerra imediatamente o programa, quebrando qualquer exploração que tenta chamar indiretamente um endereço inválido. Proteção de fluxo de controle fornece uma camada adicional importante de proteção ao Device Guard. Se um aplicativo listado em branco foi comprometido, seria capaz de executar desmarcada pelo Device Guard, porque o Device Guard triagem veria que o aplicativo foi assinado e é considerado confiável.

Mas, como proteção de fluxo de controle pode identificar se o aplicativo está em execução em uma ordem não predeterminado, não seja viável, o ataque falharia, impedindo a execução do aplicativo comprometida. Juntas, essas proteções tornam difícil para invasores injetar o malware no software em execução no Windows Server 2016.

Os desenvolvedores que criam aplicativos onde dados pessoais serão manipulados são incentivados a habilitar o controle de fluxo CFG (proteção) em seus aplicativos. Esse recurso está disponível no Microsoft Visual Studio 2015 e é executado em versões "CFG reconhecimento de" do Windows — versões x86 e x64 para área de trabalho e servidor do Windows 10 e Windows 8.1 Update (KB3000850). Você não tem que habilitar o CFG para cada parte do seu código, como uma mistura de CFG habilitado e não-CFG habilitado código será executado sem problemas. Mas falha habilitar o CFG para todo o código pode abrir lacunas na proteção. Além disso, o CFG habilitado código funcione bem em versões "Sem reconhecimento CFG" do Windows e, portanto, é totalmente compatível com eles.

#### <a name="windows-defender-antivirus"></a>Antivírus do Windows Defender
Windows Server 2016 inclui os setor líder, ativa recursos de detecção do Windows Defender para impedir que malwares conhecidos. Windows Defender antivírus (AV) funciona em conjunto com o Device Guard do Windows Defender e a proteção de fluxo de controle para evitar que código malicioso de qualquer natureza seja instalado em seus servidores. Ela é ativada por padrão – o administrador não precisa fazer nada para que ele começar a trabalhar. Windows Defender AV também é otimizado para dar suporte as várias funções de servidor no Windows Server 2016. No passado, os invasores usavam shells como o PowerShell para iniciar mal-intencionados binário. No Windows Server 2016, PowerShell agora está integrado com o Windows Defender AV para verificar se há malware antes de iniciar o código.

Windows Defender AV é uma solução antimalware interna que oferece gerenciamento de segurança e antimalware para desktops, computadores portáteis e servidores. Windows Defender AV foi aprimorado significativamente desde que ele foi introduzido no Windows 8. Antivírus do Windows Defender no Windows Server usa uma abordagem diversificada para melhorar o antimalware:

- **Proteção de nuvem-entregue** ajuda a detectar e bloquear novos malwares em segundos, mesmo que o malware nunca já foi visto antes.

- **Contexto local avançado** melhora como malware é identificado. Windows Server informa o Windows Defender AV não apenas sobre conteúdo como arquivos e processos, mas também onde o conteúdo provém, onde ele foi armazenado e muito mais. 

- **Sensores globais extensivos** ajudar a manter o Windows Defender AV atualizado e informado até mesmo do malware mais recente. Isso é feito de duas maneiras: coletando os dados de contexto local avançado de pontos de extremidade e analisando centralmente esses dados.

- **A violação** ajuda a proteger o Windows Defender AV em si contra ataques de malware. Por exemplo, o Windows Defender AV usa processos protegidos, o que impede que processos não confiáveis tentem violar componentes do Windows Defender AV, suas chaves do registro e assim por diante.

- **Recursos de nível empresarial** dar profissionais de TI as ferramentas e opções de configuração necessárias para tornar o Windows Defender AV uma solução antimalware de classe empresarial.

#### <a name="enhanced-security-auditing"></a>Auditoria de segurança avançada 
Windows Server 2016 ativamente alertando administradores de possíveis tentativas de violação com a auditoria de segurança avançada que fornece mais informações detalhadas, que podem ser usadas para detecção de ataque mais rápida e a perícia. Ele registra em log eventos de proteção de fluxo de controle, Device Guard do Windows Defender e outros recursos de segurança em um local, tornando mais fácil para os administradores determinar o que sistemas podem estar em risco.

Novas categorias de eventos incluem:

- **Auditar associação de grupo.** Permite auditar as informações de associação de grupo no token de logon do usuário. Eventos são gerados quando associações de grupo são enumeradas ou consultadas no computador em que a sessão de logon foi criada. 
 
- **Auditoria das atividades PnP.** Permite auditar quando plug and play detecta um dispositivo externo – o que pode conter malware. Eventos PnP podem ser usados para rastrear alterações feitas no hardware do sistema. Uma lista de IDs de fornecedor de hardware é incluída no evento.

Windows Server 2016 integra-se facilmente com sistemas de gerenciamento (SIEM) de evento incidente de segurança, como Microsoft Operations Management Suite (OMS), que pode incorporar as informações em relatórios de inteligência em potenciais violações. A profundidade das informações fornecidas pela auditoria avançado permite que as equipes de segurança identificar e responder a possíveis violações com mais rapidez e eficiência.

### <a name="secure-virtualization"></a>Virtualização segura
As empresas hoje virtualizar tudo o que eles podem do SQL Server ao SharePoint para os controladores de domínio do Active Directory. VMs (máquinas virtuais) simplesmente facilitam a implantar, gerenciar, serviço e automatizar sua infraestrutura. Mas quando se trata de segurança, virtualização comprometido malhas tornaram um novo vetor de ataque que é difícil se defender – até agora. De uma perspectiva GDPR, você deve pensar sobre como proteger VMs como você protegerá servidores físicos, incluindo o uso da tecnologia de VM TPM.

Windows Server 2016 fundamentalmente altera como as empresas podem segura virtualização, incluindo várias tecnologias que permitem que você crie máquinas virtuais que será executado apenas em sua própria estrutura; ajudando a proteger contra o armazenamento, rede e eles são executados em dispositivos do host.

#### <a name="shielded-virtual-machines"></a>Protegidos de máquinas virtuais
As mesmas coisas que tornam as máquinas virtuais tão fácil migrar, backup e replicar, também torná-los mais fácil modificar e copiar. Uma máquina virtual é um arquivo, para que ele não estiver protegido na rede, no armazenamento, em backups, ou em outro local. Outro problema é que os administradores de malha – se ele forem um administrador de armazenamento ou um administrador de rede – têm acesso a todas as máquinas virtuais.

Um administrador comprometido na malha facilmente pode resultar em comprometimento de dados em máquinas virtuais. Tudo o que o invasor precisa fazer é usar as credenciais comprometidas copiar quaisquer arquivos VM quiserem em uma unidade USB e orientá-lo para fora da organização, onde os arquivos VM podem ser acessados de qualquer outro sistema. Se qualquer uma dessas VMs roubadas fosse um controlador de domínio do Active Directory, por exemplo, o invasor facilmente pode exibir o conteúdo e use técnicas de ataques de força bruta prontamente disponíveis para obter as senhas no banco de dados do Active Directory, em última análise, permitindo que eles acessem para todo o restante em sua infraestrutura.

Windows Server 2016 apresenta protegido VMs (máquinas virtuais protegido) para ajudar a proteger contra cenários como aquela que acabamos de descrever. VMs protegidas de incluem um dispositivo virtual do TPM, permitindo que as organizações aplicar a criptografia BitLocker para as máquinas virtuais e certifique-se de que eles são executados somente em hosts confiáveis para ajudar a proteger contra administradores de host, rede e armazenamento comprometido. VMs protegidas são criadas usando a geração 2 VMs, que dão suporte a firmware Unified Extensible Firmware Interface (UEFI) e têm TPM virtual.

#### <a name="host-guardian-service"></a>Serviço guardião de host
Junto com VMs protegido, o serviço guardião de Host é um componente essencial para a criação de uma malha de virtualização segura. Sua tarefa é atestar a integridade de um host do Hyper-V antes de permitir uma VM protegido para inicializar ou migrar para o host. Ele mantém as chaves para VMs protegido e não liberará-las até que a integridade de segurança é garantida. Há duas maneiras que você pode exigir hosts Hyper-V atestar para o serviço guardião de Host.

A primeira e mais segura, são o atestado de hardware confiável. Essa solução requer que sua VMs protegido está executando em hosts que tenham chips TPM 2.0 e UEFI 2.3.1. Esse hardware é necessário para fornecer a inicialização medida e informações de integridade do kernel do sistema operacional exigidas pelo serviço guardião de Host para garantir que o host do Hyper-V não foi violadas.

As organizações de TI tem a alternativa de uso de Atestado confiável de administrador, que pode ser desejável se o hardware do TPM 2.0 não está em uso em sua organização. Esse modelo de Atestado é fácil de implantar porque hosts simplesmente são colocados em um grupo de segurança e o serviço guardião de Host está configurado para permitir VMs protegido ser executado em hosts que são membros do grupo de segurança. Com esse método, não há nenhuma medida complexa para garantir que o computador host ainda não foi violado. No entanto, você elimina a possibilidade de não criptografado VMs saiam pela porta USB unidades ou que a VM será executado em um host não autorizado. Isso ocorre porque os arquivos VM não será executado em qualquer computador que não seja o grupo designado. Se você ainda não tiver hardware TPM 2.0, você pode começar com Atestado confiável de administração e alternar para hardware confiável Atestado quando seu hardware é atualizado.

#### <a name="virtual-machine-trusted-platform-module"></a>Módulo de plataforma confiável de máquina virtual
Windows Server 2016 dá suporte a TPM para máquinas virtuais, que permite que você dê suporte a tecnologias de segurança avançada, como criptografia de unidade BitLocker® em máquinas virtuais. Você pode habilitar o suporte a TPM em qualquer máquina virtual de geração 2 Hyper-V usando o Gerenciador do Hyper-V ou o cmdlet do Windows PowerShell Enable-VMTPM.

Você pode proteger virtual TPM (vTPM) usando as chaves de criptografia locais armazenado no host ou armazenada no serviço guardião de Host. Portanto, enquanto o serviço guardião de Host requer infraestrutura mais, ele também fornece mais proteção.

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>Usando a rede definido pelo software de firewall de rede distribuído
Uma maneira de melhorar a proteção em ambientes virtualizados é segmentar a rede de maneira que permita VMs conversar somente os sistemas específicos necessários para funcionar. Por exemplo, se seu aplicativo não precisa se conectar à Internet, você pode particioná-la, eliminando esses sistemas como destinos contra invasores externos. A rede definido pelo software (SDN) no Windows Server 2016 inclui um firewall de rede distribuída que permite que você crie dinamicamente as políticas de segurança que podem proteger seus aplicativos contra ataques provenientes de dentro ou fora de uma rede. Esse firewall de rede distribuída adiciona camadas à segurança, permitindo que você isolar seus aplicativos na rede. Políticas podem ser aplicadas em qualquer lugar em sua infraestrutura de rede virtual, isolando VM-to-VM tráfego, tráfego VM-to-host ou VM-to-Internet onde necessário – o tráfego para sistemas individuais que podem estar comprometidos ou programaticamente em várias sub-redes. Recursos de rede definidos pelo software Windows Server 2016 também permitem rotear ou espelhar o tráfego de entrada para dispositivos virtuais não são da Microsoft. Por exemplo, você pode optar por enviar todo o tráfego de email por meio de um dispositivo virtual Barracuda para proteção de filtragem de spam adicional. Isso permite que você facilmente camada adicional de segurança ambos os locais ou na nuvem.

### <a name="other-gdpr-considerations-for-servers"></a>Outras considerações GDPR para servidores
O GDPR inclui requisito explícito para onde significa uma violação de dados pessoais, às notificações de violação "_uma violação de segurança, levando a destruição acidental ou ilegal, perda, alteração, divulgação não autorizada de, ou acessem, dados pessoais transmitidas, armazenadas ou caso contrário, processada. _"  Obviamente, você não pode começar a mover para frente para atender as exigências de notificação GDPR dentro de 72 horas, se você não consegue detectar a violação em primeiro lugar.

Conforme observado no Centro de segurança do Windows white paper, [violação Post: lidar com ameaças avançadas](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf,)

> "_Diferentemente Pre-violação, pós-violação pressupõe uma violação de já ter ocorrido – atua como um gravador de pré-lançamento e Crime cena investigador CSI (). Pós-Post-breach fornece as equipes de segurança as informações e conjunto de ferramentas necessárias para identificar, investigar e responder a ataques de outra forma permanecerá detectados e abaixo do radar. _"

Nesta seção, veremos como o Windows Server podem ajudar você a cumprir suas obrigações de notificação de violação GDPR. Isso começa com noções básicas sobre os dados de ameaça subjacente disponíveis para a Microsoft que é coletado e analisado para seu benefício e como, por meio de Windows Defender avançada ameaça proteção (ATP), esses dados podem ser essenciais para você.

#### <a name="insightful-security-diagnostic-data"></a>Dados de diagnóstico de segurança perspicazes
Por quase duas décadas, Microsoft tem sido transformando ameaças em inteligência útil que pode ajudar a fortalecer a sua plataforma e proteger os clientes. Hoje em dia, com as vantagens de computação imenso suportadas pela nuvem, nós estão encontrando novas maneiras de usar nossos mecanismos de análise avançada conduzidos por inteligência de ameaça para proteger nossos clientes.

Aplicando uma combinação de processos automatizados e manuais, aprendizado de máquina e especialistas humanos, nós pode criar um gráfico de segurança inteligente que aprende com em si e evolui em tempo real, reduzindo o nosso tempo coletivo para detectar e responder a incidentes de novo na nossos produtos.

![Gráfico de segurança de inteligência do Microsoft](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

O escopo de inteligência de ameaça da Microsoft abrange, literalmente, bilhões de pontos de dados: 35 bilhões de mensagens mensal, digitalizado 1 bilhão de clientes em toda empresa e segmentos de consumidor acessando mais de 200 serviços em nuvem e bilhão de 14 autenticações realizadas diariamente. Todos esses dados são agrupados em seu nome pela Microsoft para criar o gráfico de segurança inteligente que podem ajudá-lo a proteger sua casa de forma dinâmica permaneça seguro, permanecer produtivo e atender aos requisitos da GDPR.

#### <a name="detecting-attacks-and-forensic-investigation"></a>Detectar ataques e investigação forense
Até mesmo as defesas de ponto de extremidade melhor podem ser violadas eventualmente, conforme cyberattacks se tornam mais sofisticadas e direcionadas. Dois recursos podem ser usados para ajudar com a detecção de violação potencial - proteção de ameaças avançadas (ATP) do Windows Defender e análises de ameaça avançada (ATA) da Microsoft.

Proteção de ameaças avançadas (ATP) do Windows Defender ajuda a detectar, investigue e responder a ataques avançados e violações de dados em suas redes. Os tipos de violação de dados a GDPR espera para se proteger contra por meio de medidas de segurança técnica para garantir a confidencialidade em andamento, a integridade e a disponibilidade de dados pessoais e sistemas de processamento.

Entre as principais vantagens do Windows Defender ATP são as seguintes:

- **Detectando o detectados.** Sensores integrados profundo ao sistema operacional e do kernel, especialistas de segurança do Windows, ótica exclusiva em mais de 1 bilhão máquinas e sinais de em todos os serviços Microsoft.

- **Integrado, não acrescentada.** Sem agentes, com alto desempenho e um impacto mínimo, baseado em nuvem; gerenciamento fácil com nenhuma implantação. 

- **Painel único de aumento de segurança do Windows.** Explore seis meses de máquina-timeline sofisticada, unificar eventos de segurança do Windows Defender ATP, o antivírus do Windows Defender e o Device Guard do Windows Defender.

- **Energia do gráfico de Microsoft.** Aproveita o gráfico de segurança de inteligência do Microsoft para integrar a detecção e a exploração de assinatura do Office 365 ATP, acompanhar novamente e responder a ataques.

Leia mais na [que há de novo na visualização do Windows Defender ATP criadores Update](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/).

ATA é um produto no local que ajuda a detectar o comprometimento de identidade em uma organização. ATA pode capturar e analisar o tráfego de rede para autenticação, autorização e protocolos (por exemplo, Kerberos, DNS, RPC, NTLM e outros protocolos) de coleta de informações. ATA utiliza esses dados para criar um perfil comportamental sobre usuários e outras entidades em uma rede para que ele possa detectar anomalias e padrões de ataques conhecidos. A tabela a seguir lista os tipos de ataque detectados pelo ATA.


|Tipo de ataque |Descrição |
|---------|---------|
|Ataques mal intencionados |Esses ataques são detectadas, procurando por ataques de uma lista conhecida de tipos de ataque, incluindo:<ul><li>Pass-the-Ticket (CTT)</li><li>Pass-the-Hash (PtH)</li><li>Ponte-the-Hash.</li><li>Falsificados PAC (MS14-068)</li><li>Tíquete "ouro"</li><li>Replicação mal-intencionado</li><li>Reconhecimento</li><li>Ataques de força bruta</li><li>Execução remota</li></ul>Para obter uma lista completa de ataques maliciosos que podem ser detectados e suas descrições, consulte [detectar qual suspeitos atividades pode ATA?](https://docs.microsoft.com/en-us/advanced-threat-analytics/understand-explore/ata-threats).|
|Comportamento anormal |Esses ataques são detectados usando a análise comportamental e usam máquina aprendendo a identificar a atividades duvidosas, incluindo:<ul><li>Logins anômalas</li><li>Ameaças desconhecidas</li><li>Compartilhamento de senha</li><li>Movimento lateral</li></ul>|
|Riscos e problemas de segurança |Esses ataques são detectados, observando a rede atual e a configuração do sistema, incluindo:<ul><li>Confiança quebrada</li><li>Protocolos fracos</li><li>Vulnerabilidades conhecidas de protocolo</li></ul>|

Você pode usar ATA para ajudar a detectar invasores tentar comprometer identidades privilegiadas. Para obter mais informações sobre como implantar ATA, veja os tópicos de planejamento, Design e implantar no [documentação de análise avançada de ameaça](https://docs.microsoft.com/en-us/advanced-threat-analytics/).

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>Conteúdo relacionado associado soluções do Windows Server 2016

- **Antivírus do Windows Defender:** https://www.youtube.com/watch?v=P1aNEy09NaI e https://docs.microsoft.com/en-us/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender avançada proteção contra ameaças:** https://www.youtube.com/watch?v=qxeGa3pxIwg e https://docs.microsoft.com/en-us/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Device Guard do Windows Defender:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/en-us/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI e https://docs.microsoft.com/en-us/windows/access-protection/credential-guard/credential-guard

- **Proteção do fluxo de controle:** https://msdn.microsoft.com/en-us/library/windows/desktop/mt637065(v=vs.85).aspx

- **Garantia e segurança:** https://docs.microsoft.com/en-us/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>Isenção de responsabilidade
Este artigo é um comentário em GDPR, como Microsoft interpreta, a partir da data de publicação. Nós gastamos muito tempo com GDPR e deseja que fizemos pense sobre sua intenção e o significado. Mas a aplicação de GDPR é altamente específica de fato, e nem todos os aspectos e interpretações de GDPR são bem liquidadas.

Como resultado, este artigo é fornecido apenas para fins informativos e não ser considerado como advogado ou para determinar como GDPR podem se aplicar a você e sua organização. Encorajamos você a trabalhar com um profissional legalmente qualificados para discutir GDPR, como ele se aplica especificamente para sua organização e como melhor garantir a conformidade.

A Microsoft NÃO FAZ NENHUMA GARANTIA, EXPRESSAS, IMPLÍCITAS OU LEGAIS, COMO AS INFORMAÇÕES NESTE ARTIGO. Este artigo é fornecido "como-é." Informações e pontos de vista expressados neste artigo, incluindo URL e outras referências a sites da Internet, poderão ser alterados sem aviso prévio.

Este artigo não concede a você nenhum direito legal a nenhuma propriedade intelectual de nenhum produto da Microsoft.  Você pode copiar e usar este artigo para internos e referência somente finalidades.  

Publicado de 2017 de setembro<br>
Versão 1.0<br>
© 2017 Microsoft. Todos os direitos reservados.


