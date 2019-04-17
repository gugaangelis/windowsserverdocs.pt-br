---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Planejamento de comprometimento
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7cb87e6b9d1ace15050dcbff8b3c6d864c2a18f0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-for-compromise"></a>Planejamento de comprometimento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Lei número um: Ninguém acredita que nada ruim pode acontecê-los, até que ele faz.* - [10 leis imutáveis de administração de segurança](https://technet.microsoft.com/library/cc722488.aspx)  
  
Planos de recuperação de desastres em muitas organizações focam sobre a recuperação de desastres regionais ou falhas que resultam em perda de serviços de computação. No entanto, ao trabalhar com os clientes comprometidos, normalmente achamos que a recuperação de comprometimento intencional está ausente em seus planos de recuperação de desastres. Isso é especialmente verdadeiro quando o comprometimento resulta em roubo de propriedade intelectual ou destruição intencional que aproveita limites lógicos (como destruição de todos os domínios do Active Directory ou todos os servidores) em vez de limites físicos (como destruição de um Data Center). Embora uma organização pode ter planos de resposta a incidentes que definem atividades iniciais quando um compromisso é descoberto, esses planos omitir muitas vezes etapas para recuperar um compromisso que afeta a infraestrutura de computação inteira.  
  
Como o Active Directory fornece recursos avançados de gerenciamento de identidade e acesso para os usuários, servidores, estações de trabalho e aplicativos, ele é invariavelmente atacado por invasores. Se um invasor obtiver altamente privilegiado acesso a um domínio do Active Directory ou o controlador de domínio, esse acesso pode ser utilizado para acessar, controlar ou até mesmo destruir toda a floresta do Active Directory.  
  
Este documento discutiu alguns dos mais comuns ataques contra Windows e do Active Directory e contramedidas que você pode implementar para reduzir a superfície de ataque, mas a maneira somente se a recuperação caso haja algum comprometimento completo do Active Directory é estar preparados para o comprometimento antes que ela ocorra. Esta seção se concentra menos detalhes de implementação technical que seções anteriores deste documento, e obter mais informações sobre as recomendações de alto nível que você pode usar para criar uma abordagem abrangente e abrangente para proteger e gerenciar ativos de TI e de negócios da sua organização.  
  
Se sua infraestrutura nunca tiver sido atacada, tem resisted tentou violações, ou tem succumbed a ataques e foi totalmente comprometido, você deve planejar a realidade inevitável que você ser invadido repetidamente. Não é possível evitar ataques, mas, na verdade, é possível evitar violações de significativas ou comprometimento atacado. Cada organização estreitamente deve avaliar os seus programas de gerenciamento de risco existente e fazer ajustes necessários para ajudar a reduzir o nível geral de vulnerabilidade fazendo investimentos equilibrados na prevenção, detecção, contenção e recuperação.  
  
Para criar defesas eficazes enquanto ainda fornece serviços para os usuários e empresas que dependem de sua infraestrutura e aplicativos, você pode precisar considerar novas maneiras de evitar, detectar e contêm compromisso em seu ambiente e, em seguida, recuperar o comprometimento. As abordagens e recomendações contidas neste documento não podem ajudar você reparar uma instalação do Active Directory comprometida, mas pode ajudar a proteger seu outro.  
  
Recomendações para recuperar uma floresta do Active Directory são apresentadas em [Windows Server 2012: planejando para recuperação de floresta do Active Directory](https://www.microsoft.com/download/details.aspx?id=16506). Você poderá impedir que seu ambiente contra o comprometimento completamente novo, mas mesmo se não estiver, você terá ferramentas para recuperar e recuperar o controle do seu ambiente.  
  
## <a name="rethinking-the-approach"></a>Repensando a abordagem  
*Lei número oito: A dificuldade de defesa uma rede é diretamente proporcional à sua complexidade.* - [10 leis imutáveis de administração de segurança](https://technet.microsoft.com/library/cc722488.aspx)  
  
É geralmente bem aceito que se um invasor tiver obtido sistema, administrador, raiz ou acesso equivalente a um computador, independentemente do sistema operacional, esse computador pode não ser considerado confiável, independentemente de quantas esforços são feitos para o sistema "Limpar". No Active Directory não é diferente. Se um invasor tenha obtido acesso privilegiado para um controlador de domínio ou uma conta altamente privilegiada no Active Directory, a menos que você tenha um registro de cada modificação que faz com que o invasor ou um bom backup, você nunca pode restaurar o diretório para um estado totalmente confiável.  
  
Quando um servidor membro ou estação de trabalho for comprometida e alterada por um invasor, o computador não é mais confiáveis, mas vizinhos incomparável servidores e estações de trabalho arecompromise de um computador não implica que todos os computadores estão comprometidos.  
  
No entanto, em um domínio do Active Directory, todos os controladores de domínio hospedam réplicas do mesmo banco de dados do AD DS. Se um controlador de domínio único for comprometido, e um invasor modifica o banco de dados do AD DS, essas modificações replicam para todos os outro controlador de domínio no domínio e, dependendo da partição na qual as modificações forem feitas, floresta. Mesmo se você reinstalar todos os controladores de domínio na floresta, você simplesmente estiver reinstalando os hosts em que reside o banco de dados do AD DS. Modificações mal-intencionado ao Active Directory replicará para controladores de domínio recém-instalado tão fácil quanto eles serão replicados para controladores de domínio que tenham tido por anos.  
  
Avaliar ambientes comprometidos, normalmente achamos que o que parecia ser o primeiro violação "evento" foi realmente acionado após semanas, meses ou até mesmo anos depois que os invasores tinham inicialmente comprometido o ambiente. Os invasores obtido geralmente as credenciais de contas altamente privilegiadas longas antes de uma violação foi detectada e eles aproveitado essas contas para comprometer o diretório controladores de domínio, servidores membros, estações de trabalho e até mesmo conectado sistemas não Windows.  
  
Esses resultados são consistentes com várias conclusões da Verizon 2012 dados violação investigações relatório, que estabelece que:  
  
-   98% dos dados violações surgiu com agentes externos  
  
-   85% das violações de dados leva semanas ou mais para descobrir  
  
-   92% dos incidentes descobertos por terceiros, e  
  
-   97% dos violações foram evitáveis Embora simples ou intermediários controles.  
  
Um compromisso para o grau descrito anteriormente é efetivamente irreparável e a orientação padrão de "Nivelar e recompile" cada sistema comprometido simplesmente não é possível viável ou até mesmo se o Active Directory foi comprometido ou destruído. Restaurando até mesmo para um estado conhecido bom também não elimina as falhas que permitia o ambiente ser comprometido em primeiro lugar.  
  
Embora você deve se defender cada aspecto de sua infraestrutura, um invasor só precisa encontrar suficiente falhas em suas defesas para acessar seu objetivo desejado. Se o seu ambiente é relativamente simples e impecáveis e historicamente bem gerenciado, em seguida, implementando as recomendações fornecidas anteriormente neste documento pode ser uma proposta simples.  
  
No entanto, descobrimos que as mais antigas, maior e mais complexo o ambiente, mais provável é que as recomendações contidas neste documento serão inviável ou até mesmo impossível implementar. É muito mais difícil proteger uma infraestrutura após o fato de que a começar desde o início e para construir um ambiente que é resistente a ataques e comprometer a segurança. Mas como observado anteriormente, não é nenhuma realizar pequeno recriar toda a floresta do Active Directory. Por esses motivos, recomendamos um mais focado abordagem direcionada para proteger seu florestas do Active Directory.  
  
Em vez de focalizar e tentar corrigir tudo o que são "quebrado", considere uma abordagem em que você priorizar com base no que há de mais importante para sua empresa e em sua infraestrutura. Em vez de tentar corrigir um ambiente preenchido com aplicativos e sistemas desatualizados, configurado incorretamente, considere criar um novo ambiente pequeno e seguro no qual você pode portar com segurança os usuários, sistemas e informações que são mais importantes para sua empresa.  
  
Nesta seção, nós descrevemos uma abordagem por meio do qual você pode criar uma floresta do AD DS impecável que serve como um "vida barco" ou "célula segura" para sua infraestrutura de negócios principal. Uma floresta impecável é simplesmente uma recém-instalado floresta do Active Directory que normalmente é limitado em tamanho e escopo e que tenha sido criado por meio de sistemas operacionais, aplicativos e com os princípios descritos em [reduzindo a superfície de ataque Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
Implementando as configurações recomendadas em uma floresta recém-criado, você pode criar uma instalação do AD DS que é criada a partir do zero com configurações seguras e práticas recomendadas e você pode reduzir os desafios que acompanham o suporte aos aplicativos e sistemas herdados. Enquanto instruções detalhadas para a criação e implementação de uma instalação do AD DS impecável estão fora do escopo deste documento, você deve seguir alguns princípios gerais e diretrizes para criar uma "célula segura" no qual você pode colocar os recursos mais importantes. Estas diretrizes são as seguintes:  
  
1.  Identifique princípios para separar e proteger os ativos críticos.  
  
2.  Defina um plano de migração limitado, com base em risco.  
  
3.  Aproveite migrações "nonmigratory" onde necessário.  
  
4.  Implementar "destruição criativa."  
  
5.  Isole aplicativos e sistemas herdados.  
  
6.  Simplifique a segurança para os usuários finais.  
  
### <a name="identifying-principles-for-segregating-and-securing-critical-assets"></a>Identificando princípios para separar e proteger os ativos críticos  

As características do ambiente de impecáveis que você cria para os ativos críticos domésticos podem variar muito. Por exemplo, você pode optar por criar uma floresta impecável no qual você migrar apenas os usuários VIP e dados confidenciais que somente os usuários podem acessar. Você pode criar uma floresta original em que você não migrar apenas os usuários VIP, mas que você implementa como uma floresta administrativa, implementar os princípios descritos [reduzindo a superfície de ataque Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) para criar contas de administrador seguras e hosts que podem ser usados para gerenciar seu florestas herdadas da floresta impecável. Você pode implementar uma floresta "especificamente" que armazena contas VIP, contas privilegiadas e sistemas exigir segurança adicional, como servidores que executam serviços de certificados do Active Directory (AD CS) com o objetivo exclusivo de separá-los de florestas menos seguro. Por fim, você pode implementar uma floresta impecável que se torna o local de fato para todos os novos usuários, sistemas, aplicativos e dados, permitindo que você acabará de desativação de floresta herdada via desgaste dos.  
  
Independentemente de se floresta impecável contém alguns dos usuários e sistemas ou formam a base para uma migração mais agressiva, você deve seguir estes princípios em seu planejamento:  
  
1.  Suponha que seu florestas herdadas sejam comprometidas.  
  
2.  Não configure um ambiente impecável para confiar em uma floresta herdada, embora você possa configurar um ambiente herdado para confiar em uma floresta impecável.  
  
3.  Não migre contas de usuário ou grupos de uma floresta herdada para um ambiente impecável se há uma possibilidade de que membros do grupo as contas, histórico SID ou outros atributos podem ter sido maneira mal-intencionada modificados. Em vez disso, use "nonmigratory" abordagens para preencher uma floresta impecável. (Nonmigratory abordagens são descritas posteriormente nesta seção.)  
  
4.  Não migre computadores de florestas herdadas para florestas impecáveis. Implementar servidores recém-instalado na floresta impecável, instale aplicativos nos servidores recém-instalado e migrar dados de aplicativo para os sistemas recém-instalado. Para servidores de arquivos, copie dados para servidores recém-instalado, definir ACLs usando usuários e grupos na nova floresta e, em seguida, criar servidores de impressão de maneira semelhante.  
  
5.  Não permitir a instalação de sistemas operacionais herdados ou aplicativos na floresta impecável. Se um aplicativo não pode ser atualizado e instalado recentemente, deixá-lo na floresta herdada e considere destruição criativa para substituir a funcionalidade do aplicativo.  
  
### <a name="defining-a-limited-risk-based-migration-plan"></a>Definindo um plano de migração limitado, com base em risco  
Criando uma limitada, plano de migração com base em risco simplesmente significa que, ao decidir quais usuários, aplicativos e dados migrar para sua floresta impecável, você deve identificar os destinos de migração com base no grau de risco para que sua organização é exposta se um dos usuários ou sistemas for comprometido. Os usuários VIP cujas contas têm mais probabilidade de ser atacado por invasores devem ser armazenados na floresta impecável. Aplicativos que fornecem funções comerciais vitais devem ser instaladas em servidores recém-compilados na floresta impecável e dados altamente confidenciais devem ser movidos para protegido servidores na floresta impecável.  
  
Se você não tiver uma imagem clara dos usuários crítico de negócios, sistemas, aplicativos e dados no ambiente do Active Directory, trabalham com unidades de negócios para identificá-los. Qualquer aplicativo necessário para os negócios operar deve ser identificado, assim como todos os servidores nos quais executar aplicativos críticos ou dados críticos são armazenados. Identificando os usuários e os recursos que são necessários para sua organização continuem a funcionar, você cria uma coleção naturalmente priorizada de ativos para concentrar seus esforços.  
  
### <a name="leveraging-nonmigratory-migrations"></a>Aproveitando migrações "Nonmigratory"  
Se você sabe que seu ambiente foi comprometido, suspeito que ele foi comprometida, ou simplesmente prefere não migrar dados herdados e objetos de uma instalação do Active Directory herdada para um novo, considere a possibilidade de abordagens para a migração não tecnicamente "foi migrado" objetos.  
  
### <a name="user-accounts"></a>Contas de usuário  
Em um tradicional do Active Directory migração de uma floresta para outro, o atributo de histórico SID (histórico SID) em objetos de usuário é usado para armazenar o SID dos usuários e os SIDs de grupos que os usuários eram membros na floresta herdado. Se as contas de usuários são migradas para uma nova floresta e acessarem recursos da floresta herdado, os SIDs no histórico SID são usados para criar um token de acesso que permite que os usuários acessem os recursos aos quais eles tinham acesso antes das contas foram migradas.  
  
Manter histórico SID, no entanto, comprovou problemático em alguns ambientes porque preenchendo os tokens de acesso dos usuários com SIDs atuais e históricas pode resultar em inflar token. Inflar token é um problema em que o número de SIDs que devem ser armazenados no token de acesso do usuário usa ou excede a quantidade de espaço disponível no token.  
  
Embora tamanhos de tokens podem ser aumentados até certo ponto, a melhor solução para Inflar token é reduzir o número de SIDs associados às contas de usuário, seja por racionalização associações de grupo, eliminando histórico SID ou uma combinação de ambos. Para saber mais sobre inflar token, consulte [MaxTokenSize e Kerberos Token inchar](http://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
Em vez de migração de usuários em um ambiente herdado (especialmente aquela em que membros do grupo e SID históricos poderão ser comprometidos) usando o histórico SID, considere aproveitar os aplicativos metadiretório "migrar" usuários, sem executar históricos SID para a nova floresta. Quando contas de usuário são criadas na nova floresta, você pode usar um aplicativo metadiretório para mapear as contas para suas contas correspondentes na floresta herdada.  
  
Para fornecer o acesso de contas de usuário novo aos recursos na floresta herdado, você pode usar as ferramentas metadiretório para identificar os grupos de recursos para a qual os usuários herdados foram concedido contas acesso e, em seguida, adicionar novas contas dos usuários a esses grupos. Dependendo da sua estratégia de grupo na floresta herdada, talvez seja necessário criar grupos de domínio local para acessar recursos ou converter os grupos existentes para grupos de domínio local para permitir que as novas contas a ser adicionada a grupos de recursos. Focalizar primeiro o mais aplicativos e dados essenciais e migrá-los para o novo ambiente (com ou sem histórico SID), você pode limitar a quantidade de esforço feito no ambiente herdado.  
  

  
### <a name="servers-and-workstations"></a>Servidores e estações de trabalho  
Em uma migração tradicional do Active Directory uma floresta para outros, migrando computadores costuma ser relativamente simple em comparação com a migração de usuários, grupos e aplicativos. Dependendo da função do computador, a migração para uma nova floresta pode ser tão simple quanto ao sair de um domínio antigo e ingressar em um novo. No entanto, migrando contas de computador intactas em derrotas uma floresta impecáveis a finalidade da criação de um novo ambiente. Em vez de migrar (potencialmente comprometida, configurado incorretamente ou desatualizado) contas de computador para uma nova floresta, você deve instalar recém-servidores e estações de trabalho no novo ambiente. Você pode migrar dados de sistemas na floresta herdado para sistemas na floresta original, mas não os sistemas que hospedam os dados.  
  
### <a name="applications"></a>Aplicativos  

Aplicativos podem apresentar o desafio mais significativo em qualquer migração de uma floresta para outra, mas, no caso de uma migração de "nonmigratory", um dos princípios mais básicos, que você deve aplicar é que os aplicativos na floresta impecável devem ser atual, com suporte e instalados recentemente. Dados podem ser migrados de instâncias do aplicativo na floresta do antiga onde for possível. Em situações em que um aplicativo não pode ser "recriado" na floresta impecável, você deve considerar abordagens como destruição criativa ou isolamento de aplicativos herdados, conforme descrito na seção a seguir.  
  
### <a name="implementing-creative-destruction"></a>Implementando a destruição criativa  
Destruição criativa é um termo de economia que descreve o desenvolvimento econômico criado pela destruição de um pedido anterior. Nos últimos anos, o termo foi aplicado a tecnologia da informação. Ele geralmente se refere aos mecanismos pelo qual infraestrutura antiga é eliminada, não por atualizá-lo, mas substituindo-o com algo completamente novo. O 2011 [Gartner Simpósio ITXPO](http://www.gartner.com/technology/symposium/orlando/) para seniores executivos de TI e CIOs apresentadas destruição criativa como um dos seus temas principais para redução de custos e aumenta a eficiência. Melhorias na segurança são possíveis como um crescimento natural do processo.  

Por exemplo, uma organização pode ser composta de várias unidades de negócios que usam um aplicativo diferente que realiza uma funcionalidade semelhante, com diversos graus de suporte modernity e fornecedor. Historicamente, IT pode ser responsável por manter o aplicativo do cada unidade de negócios separadamente e esforços de consolidação seriam consistem em tentando descobrir qual aplicativo oferecido a melhor funcionalidade e, em seguida, migração de dados em que o aplicativo dos outros.  
  
Em destruição criativa, em vez de manutenção de aplicativos desatualizados ou redundantes, implementar inteiramente novos aplicativos para substituir a antiga, migrar dados para os novos aplicativos e encerrar os aplicativos antigos e os sistemas em que eles são executados. Em alguns casos, você pode implementar criativa destruição de aplicativos herdados, implantando um novo aplicativo em sua própria infraestrutura, mas sempre que possível, você deve considerar a portabilidade do aplicativo para uma solução baseada em nuvem em vez disso.  
  
Implantando aplicativos baseados em nuvem para substituir herdados aplicativos internos, você não apenas reduzir os custos e esforços de manutenção, mas reduzir a superfície de ataque da sua organização, eliminando sistemas herdados e aplicativos que apresentam vulnerabilidades para invasores aproveitar. Essa abordagem fornece uma maneira mais rápida para uma organização obter funcionalidade desejada eliminando destinos herdados na infraestrutura simultaneamente. Embora o princípio de destruição criativo não se aplica a todos os ativos de TI, ele fornece uma opção viável com frequência para eliminar sistemas herdados e aplicativos durante a implantação simultaneamente aplicativos robustos, seguros baseada na nuvem.  
  
### <a name="isolating-legacy-systems-and-applications"></a>Isolando aplicativos e sistemas herdados  
Um crescimento natural de migração de seus sistemas e usuários essenciais para um ambiente impecável e seguro é que seu floresta herdada irá conter informações menos importantes e sistemas. Embora os sistemas herdados e aplicativos que permanecem no ambiente menos seguro podem apresentar com privilégios elevados risco de comprometimento, eles também representarem severidade reduzida de comprometimento. Rehoming e Modernizando seus ativos essenciais, você pode se concentrar na implantação do gerenciamento eficiente e monitoramento enquanto não a necessidade de acomodar protocolos e configurações herdadas.  
  
Quando tiver movidas seus dados críticos para uma floresta impecável, você pode avaliar opções para isolar ainda mais sistemas herdados e aplicativos em seu "principal" floresta do AD DS. Embora você pode implementar destruição criativa para substituir um aplicativo e os servidores em que ele é executado, em outros casos, você pode considerar isolamento adicionais dos sistemas e aplicativos menos segura. Por exemplo, um aplicativo que é usado por alguns dos usuários, mas que exige credenciais herdadas como hashes do LAN Manager podem ser migradas para um domínio pequeno você cria para dar suporte a sistemas para o qual você não tem nenhuma opção de substituição.  
  
Removendo esses sistemas de domínios em que eles forçado a implementação de configurações herdadas, você pode, posteriormente, aumentar a segurança dos domínios Configurando-os para dar suporte apenas sistemas operacionais e aplicativos. Embora, é preferível de desativação de sistemas herdados e aplicativos sempre que possível. Se aposentadoria simplesmente não for possível para um pequeno segmento de seu preenchimento herdado, separá-lo em um domínio separado (ou floresta) permite que você realizar melhorias incrementais do restante da instalação herdada.  
  
### <a name="simplifying-security-for-end-users"></a>Simplificando a segurança para os usuários finais  
Na maioria das organizações, os usuários que têm acesso às informações mais importantes devido à natureza de suas funções na organização geralmente têm o mínimo de tempo para dedicar para controles e restrições de acesso complexas de aprendizagem. Embora você deve ter um programa de educação segurança abrangente para todos os usuários em sua organização, você também deve focalizar torna mais simples usar quanto possível, Implementando controles que são transparentes e simplificar princípios para que os usuários aderem de segurança.  
  
Por exemplo, você pode definir uma política em que executivos e outros VIPs são necessárias para usar estações de trabalho seguras para acessar dados confidenciais e sistemas, permitindo que eles utilizem seus outros dispositivos para acessar dados menos confidenciais. Esse é um princípio simple para os usuários se lembrarem, mas você pode implementar uma série de controles de back-end para ajudar a impor a abordagem.  

Você pode usar [garantia do mecanismo de autenticação](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) para permitir que os usuários acessem dados confidenciais somente se eles ter se conectado em seus sistemas seguros usando os cartões inteligentes e podem usar IPsec e restrições para controlar os sistemas em que eles podem se conectar a repositórios de dados confidenciais de direitos de usuário. Você pode usar o [Microsoft dados Classification Toolkit](https://www.microsoft.com/download/details.aspx?id=27123) a criação de uma infraestrutura de classificação de arquivo robusto e você pode implementar [controle de acesso dinâmico](http://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) para restringir o acesso aos dados com base nas características de uma tentativa de acesso, traduzindo regras de negócios para controles técnicos.  
  
Da perspectiva do usuário, acessem dados confidenciais em um sistema protegido "funciona simplesmente" e tentar fazer isso de um sistema inseguro "simplesmente não". No entanto, da perspectiva do monitoramento e gerenciamento de seu ambiente, você está ajudando a criar padrões de identificação em como os usuários acessam os dados confidenciais e sistemas, tornando mais fácil para detectar as tentativas de acesso anômalas.  
  
Em ambientes em que o usuário resistência a senhas longas e complexas resultou em políticas de senha insuficiente, particularmente para os usuários VIP, considere abordagens alternativas para autenticação, seja por meio de cartões inteligentes (que vêm em um número de fatores forma e com recursos adicionais para reforçar a autenticação), controles de biometria, como leitores de passar o dedo ou até mesmo autenticação chips de dados protegidos pelo módulo de plataforma confiável (TPM) nos computadores dos usuários. Embora a autenticação multifator não impede os ataques de roubo de credenciais se um computador já estiver do comprometido, fornecendo seus controles de fácil de usar autenticação de usuários, você pode atribuir o senhas mais robustas para as contas de usuários para os quais controles de nome e a senha do usuário tradicional serão complicados.  
