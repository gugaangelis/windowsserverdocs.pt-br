---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Planejar para comprometimento
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0f6a30c91590667a0894b52ec7c188a43bd5e351
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835567"
---
# <a name="planning-for-compromise"></a>Planejar para comprometimento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Lei número um: Ninguém acredita que algo ruim acontecer a eles, até que ele faz.* - [10 leis imutáveis da segurança de administração](https://technet.microsoft.com/library/cc722488.aspx)  
  
Planos de recuperação de desastres em muitas organizações se concentrará na recuperação de desastres regionais ou falhas que resultam em perda de serviços de computação. No entanto, ao trabalhar com os clientes comprometidos, normalmente achamos que a recuperação de comprometimento intencional está ausente em seus planos de recuperação de desastres. Isso é especialmente verdadeiro quando o comprometimento resulta em roubo de propriedade intelectual ou destruição intencional que aproveita os limites lógicos (por exemplo, a destruição de todos os domínios do Active Directory ou todos os servidores) em vez dos limites físicos (como destruição de um Data Center). Embora uma organização pode ter planos de resposta a incidentes que definem atividades inicias a ser tomada quando um comprometimento é descoberto, esses planos geralmente omitir as etapas para se recuperar de um comprometimento que afeta toda a infraestrutura de computação.  
  
Como o Active Directory fornece recursos avançados de gerenciamento de identidade e acesso para usuários, servidores, estações de trabalho e aplicativos, ela invariavelmente é alvo de invasores. Se um invasor obtiver acesso altamente privilegiado a um domínio do Active Directory ou um controlador de domínio, que o acesso pode ser utilizado para acessar, controlar ou mesmo destruir toda a floresta do Active Directory.  
  
Este documento discutiu alguns dos ataques mais comuns em relação ao Windows e do Active Directory e contramedidas que você pode implementar para reduzir a superfície de ataque, mas a única maneira de recuperar em caso de comprometimento completo do Active Directory deve ser preparado para comprometimento antes que elas ocorram. Esta seção concentra-se menos em detalhes de implementação técnica que seções anteriores deste documento, e mais sobre recomendações de alto nível que você pode usar para criar uma abordagem holística e abrangente para proteger e gerenciar a sua organização do crítica ativos de TI e de negócios.  
  
Se sua infraestrutura nunca tenha sido atacada, tem resistiram à tentativa de violações, ou tem succumbed a ataques e foi totalmente comprometido, você deve planejar para a realidade inevitável que você ser atacado e, novamente. Não é possível evitar ataques, mas na verdade ser possível para evitar violações significativas ou o comprometimento de atacado. Todas as organizações se aproxime devem avaliar seus programas de gerenciamento de risco existentes e fazer os ajustes necessários para ajudar a reduzir seu nível geral de vulnerabilidade, fazendo com balanceamento de investimentos em prevenção, detecção, retenção e recuperação.  
  
Para criar defesas em vigor enquanto ainda fornecem serviços aos usuários e empresas que dependem de sua infraestrutura e aplicativos, você pode precisa considerar novas maneiras para prevenir, detectar e contêm comprometimento em seu ambiente e, em seguida, recuperar de o comprometimento. As abordagens e as recomendações neste documento poderão não ajudá-lo a reparar uma instalação do Active Directory comprometida, mas pode ajudar a proteger seu outro.  
  
Recomendações para a recuperação de uma floresta do Active Directory são apresentadas em [Windows Server 2012: Planejando a recuperação de floresta do Active Directory de](https://www.microsoft.com/download/details.aspx?id=16506). Você poderá impedir que seu novo ambiente completamente seja comprometido, mas, mesmo se não for possível, você terá as ferramentas para recuperar e retomar o controle de seu ambiente.  
  
## <a name="rethinking-the-approach"></a>Repensando a abordagem  
*Lei número oito: A dificuldade da defesa de rede é diretamente proporcional à sua complexidade.* - [10 leis imutáveis da segurança de administração](https://technet.microsoft.com/library/cc722488.aspx)  
  
É geralmente bem aceito que se um invasor tiver obtido de sistema, administrador, raiz ou acesso equivalente a um computador, independentemente do sistema operacional, esse computador pode não ser considerado confiável, não importa quantas esforços são feitos para "Limpar" o sistema. Active Directory não é diferente. Se um invasor tiver obtido acesso privilegiado para um controlador de domínio ou uma conta altamente privilegiada no Active Directory, a menos que você tenha um registro de cada modificação que faz com que o invasor ou um backup válido, você nunca poderá restaurar o diretório para um completamente estado confiável.  
  
Quando um servidor membro ou estação de trabalho for comprometida e alterada por um invasor, o computador não é mais confiável, mas estações de trabalho e servidores não comprometidos vizinhos ainda podem ser confiáveis. Comprometimento de um computador não implica que todos os computadores estão comprometidos.  
  
No entanto, em um domínio do Active Directory, todos os controladores de domínio hospedam réplicas do mesmo banco de dados do AD DS. Se um único controlador de domínio será comprometido e um invasor modifica o banco de dados do AD DS, essas modificações replicam cada controlador de domínio no domínio e, dependendo da partição em que as modificações são feitas, a floresta. Mesmo se você reinstalar cada controlador de domínio na floresta, você simplesmente estiver reinstalando os hosts no qual reside o banco de dados do AD DS. As modificações maliciosas do Active Directory serão replicadas para controladores de domínio recém-instalado tão facilmente quanto eles serão replicadas para controladores de domínio que estava executando por anos.  
  
Avaliar ambientes comprometidos, normalmente achamos que o que parecia ser o primeira violação "evento" foi, na verdade, disparado depois de semanas, meses ou até anos depois que os invasores inicialmente tinham compromete o ambiente. Os invasores geralmente obtidas as credenciais para contas altamente privilegiadas longo antes de uma violação foi detectada e eles aproveitados essas contas para comprometer o diretório, controladores de domínio, servidores membro, estações de trabalho e até mesmo conectado não-Windows sistemas.  
  
Essas descobertas são consistentes com as várias descobertas no 2012 dados violação investigações de relatório da Verizon, que declara que:  
  
-   98% de violações de dados que surgiu com agentes externos  
  
-   85% das violações de dados demorou semanas ou mais para descobrir  
  
-   92% dos incidentes foram descobertos por um terceiro, e  
  
-   97% de violações eram podem ser evitados Embora simples ou intermediário de controles.  
  
Um comprometimento no grau descrito anteriormente é efetivamente irreparável e o aviso padrão para "e reconstrua" todos os sistemas comprometidos simplesmente não é possível viável ou até mesmo se o Active Directory foi comprometido ou destruído. Até mesmo restaurar para um estado bom conhecido não elimina a falha que permitiu o ambiente para ser comprometida em primeiro lugar.  
  
Embora você deve se defender de todas as facetas de sua infraestrutura, um invasor só precisa encontrar suficiente falhas em suas defesas para obter até a meta desejada. Se seu ambiente é relativamente simples e puro e historicamente bem gerenciado, em seguida, implementando as recomendações fornecidas neste documento pode ser uma proposta simples.  
  
No entanto, descobrimos que os mais antigos, maior e mais complexo o ambiente, mais provável é que as recomendações neste documento serão inviável ou até mesmo impossível implementar. É muito mais difícil de proteger uma infraestrutura após o fato de que ele é começar do zero e construir um ambiente que é resistente a ataques e comprometer. Mas como mencionado anteriormente, é sem realizar pequenas recompilar toda a floresta do Active Directory. Por esses motivos, recomendamos um mais precisos, a abordagem direcionada para proteger suas florestas do Active Directory.  
  
Em vez de enfocar e tentar corrigir todas as coisas que são "interrompidas", considere uma abordagem em que você priorize com base no que é mais importante para o seu negócio e em sua infraestrutura. Em vez de tentar corrigir um ambiente repleto de sistemas desatualizados, configuradas incorretamente e aplicativos, considere a criação de um novo ambiente de pequeno e seguro no qual você pode portar com segurança os usuários, sistemas e informações que são mais importantes para seu negócios.  
  
Nesta seção, descrevemos um método pelo qual você pode criar uma floresta do AD DS puro que serve como um "barco vida" ou "segura" célula para sua infraestrutura do negócio principal. Uma floresta puro é simplesmente uma recém-instalado floresta de Active Directory que geralmente é limitada em tamanho e o escopo e o que é criado por meio de sistemas operacionais, aplicativos e com os princípios descritos no [reduzindo o Active Directory Superfície de ataque do](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).  
  
Implementando as configurações recomendadas em uma floresta recém-criado, você pode criar uma instalação do AD DS que é criada a partir do zero com configurações de segurança e práticas recomendadas, e você pode reduzir os desafios que acompanham a dar suporte a sistemas herdados e aplicativos. Enquanto as instruções detalhadas para o design e implementação de uma instalação original do AD DS estão fora do escopo deste documento, você deve seguir alguns princípios gerais e diretrizes para criar uma "célula segura" na qual você pode hospedar seu mais críticos ativos. Essas diretrizes são da seguinte maneira:  
  
1.  Identifique os princípios para separar e proteger ativos críticos.  
  
2.  Defina um plano de migração limitado, com base em risco.  
  
3.  Aproveite as migrações "nonmigratory" onde for necessário.  
  
4.  Implementar "destruição criativa."  
  
5.  Isole aplicativos e sistemas herdados.  
  
6.  Simplifique a segurança para os usuários finais.  
  
### <a name="identifying-principles-for-segregating-and-securing-critical-assets"></a>Identificando os princípios para separar e proteger ativos críticos  

As características do ambiente original que você cria para ativos críticos de casa podem variar muito. Por exemplo, você pode optar por criar uma floresta puro na qual você migra apenas os usuários VIP e dados confidenciais que somente os usuários podem acessar. Você pode criar uma floresta puro na qual você não migra apenas usuários VIP, mas que você implementar como uma floresta administrativa, implementar os princípios descritos em [reduzindo a superfície de ataque do Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) criar seguro contas administrativas e hosts que podem ser usados para gerenciar suas florestas herdadas da floresta puro. Você pode implementar uma "criado especificamente" floresta que hospeda contas VIP, contas privilegiadas e sistemas que requerem segurança adicional, como servidores que executam os serviços de certificados do Active Directory (AD CS) com o único objetivo de separando-os de menos seguras florestas. Por fim, você pode implementar uma floresta puro que se torna o local de fato para todos os novos usuários, sistemas, aplicativos e dados, permitindo que você eventualmente encerrar sua floresta herdada por meio de desgaste dos.  
  
Independentemente de se sua floresta puro contém alguns usuários e sistemas ou ele constitui a base para uma migração mais agressiva, você deve seguir esses princípios em seu planejamento:  
  
1.  Suponha que suas florestas herdadas tem sido comprometidas.  
  
2.  Não configure um ambiente puro para confiar em uma floresta herdada, embora você possa configurar um ambiente herdado para confiar em uma floresta puro.  
  
3.  Não migre contas de usuário ou grupos de uma floresta herdada para um ambiente puro se houver uma possibilidade de que as associações de grupo de contas, o histórico de SID ou outros atributos podem ter sido maliciosamente modificados. Em vez disso, use "nonmigratory" abordagens para preencher uma floresta puro. (Nonmigratory abordagens são descritas mais adiante nesta seção.)  
  
4.  Não migre computadores de florestas herdadas para florestas puro. Implementar servidores recém-instalada na floresta puro, instalar aplicativos nos servidores recém-instalado e migrar dados de aplicativo para os sistemas instalados recentemente. Para servidores de arquivos, copiar dados para servidores recém-instalada, definir ACLs usando usuários e grupos na nova floresta e, em seguida, criar servidores de impressão de forma semelhante.  
  
5.  Não é possível fazer a instalação de sistemas operacionais herdados ou aplicativos na floresta original. Se um aplicativo não pode ser atualizado e instalado recentemente, deixá-lo na floresta herdada e considere destruição criativa para substituir a funcionalidade do aplicativo.  
  
### <a name="defining-a-limited-risk-based-migration-plan"></a>Definindo um plano de migração limitado, com base em risco  
Criando limitada, plano de migração baseada em riscos simplesmente significa que ao decidir quais usuários, aplicativos e dados para migrar para sua floresta puro, você deve identificar destinos de migração com base no grau de risco para o qual sua organização é exposta se um de os usuários ou sistemas está comprometido. Usuários VIP cujas contas são mais prováveis de ser alvo de invasores, devem ser armazenados na floresta original. Aplicativos que fornecem funções de negócios vitais devem ser instaladas em servidores recentemente criados na floresta original, e dados altamente confidenciais que devem ser movidos para protegido servidores na floresta original.  
  
Se você ainda não tiver uma visão clara dos usuários críticos de negócios, sistemas, aplicativos e dados em seu ambiente do Active Directory, trabalhar com unidades de negócios para identificá-los. Qualquer aplicativo necessário para a empresa opere deve ser identificado, assim como todos os servidores nos quais executar aplicativos críticos ou dados críticos são armazenados. Ao identificar os usuários e recursos que são necessários para sua organização continuem a funcionar, você cria uma coleção naturalmente priorizada de ativos para concentrar seus esforços.  
  
### <a name="leveraging-nonmigratory-migrations"></a>Aproveitando as migrações "Nonmigratory"  
Se você souber que seu ambiente foi comprometido, suspeito que ele tenha sido comprometido ou simplesmente prefere não migrar dados herdados e objetos de uma instalação do Active Directory herdada para um novo, considere as abordagens de migração que fazer não tecnicamente "Migre" objetos.  
  
### <a name="user-accounts"></a>Contas de usuário  
Em uma tradicional migração do Active Directory de uma floresta para outra, o atributo SIDHistory (histórico de SID) em objetos de usuário é usado para armazenar o SID dos usuários e os SIDs dos grupos de que os usuários eram membros na floresta herdado. Se as contas de usuários são migradas para uma nova floresta e acessarem recursos da floresta herdado, os SIDs no histórico de SID são usados para criar um token de acesso que permite que os usuários acessem os recursos aos quais eles tinham acesso antes que as contas foram migradas.  
  
Manter o histórico de SID, no entanto, provou ser problemático em alguns ambientes porque preenchendo os tokens de acesso dos usuários com SIDs atuais e históricos pode resultar em inchaço do token. Inchaço de token é um problema em que usa o número de SIDs que devem ser armazenados no token de acesso do usuário ou excede a quantidade de espaço disponível no token.  
  
Embora os tamanhos de token podem ser aumentados até certo ponto, a solução definitiva para inchaço de token é reduzir o número de SIDs associados às contas de usuário, por racionalizar associações de grupo, eliminando o histórico de SID ou uma combinação de ambos. Para obter mais informações sobre excesso de token, consulte [MaxTokenSize e inchar Token do Kerberos](http://blogs.technet.com/b/shanecothran/archive/2010/07/16/maxtokensize-and-kerberos-token-bloat.aspx).  
  
Em vez de migração de usuários de um ambiente herdado (especialmente um em que as associações de grupo e o SID históricos podem ser comprometidos) usando o histórico de SID, considere a aplicativos metadiretórios "migrar" usuários, sem precisar levar históricos SID para a nova floresta. Quando as contas de usuário são criadas na nova floresta, você pode usar um aplicativo de metadiretório para mapear as contas de suas contas correspondentes na floresta herdada.  
  
Para fornecer um novo acesso de contas de usuário aos recursos na floresta herdado, use as ferramentas de metadiretório para identificar grupos de recursos no qual as herdados contas de usuários tinham acesso e, em seguida, adicionar as novas contas de usuários a esses grupos. Dependendo de sua estratégia de grupo na floresta herdada, você precisa criar grupos de domínio local para acessar recursos ou converter grupos existentes em grupos de domínio local para permitir que as novas contas sejam adicionados a grupos de recursos. Concentrando-se pela primeira vez em aplicativos mais críticos e dos dados e migrá-los para o novo ambiente (com ou sem histórico de SID), você pode limitar a quantidade de esforço feito no ambiente herdado.  
  

  
### <a name="servers-and-workstations"></a>Servidores e estações de trabalho  
Em uma migração tradicional do Active Directory de uma floresta para outra, migrar computadores geralmente é relativamente simple em comparação com a migração de usuários, grupos e aplicativos. Dependendo da função de computador, migrar para uma nova floresta pode ser tão simple quanto ao sair de um domínio antigo e ingressar em um novo. No entanto, migrando contas de computador intactas para derrotas uma floresta intacta a finalidade de criar um ambiente novo. Em vez de migrar (potencialmente comprometido, configurado incorretamente ou desatualizado) contas de computador para uma nova floresta, você deve instalar recentemente servidores e estações de trabalho no novo ambiente. Você pode migrar dados de sistemas na floresta herdado para sistemas na floresta puro, mas não os sistemas que armazenam os dados.  
  
### <a name="applications"></a>Aplicativos  

Aplicativos podem apresentar o desafio mais significativo em qualquer migração de uma floresta para outra, mas no caso de uma migração "nonmigratory", um dos princípios mais básicos, que você deve aplicar é que os aplicativos na floresta original devem ser atuais, suporte e recentemente instalado. Dados podem ser migrados de instâncias de aplicativo na floresta antiga sempre que possível. Em situações em que um aplicativo não pode ser "recriado" na floresta puro, você deve considerar abordagens, como isolamento de aplicativos herdados ou destruição criativa, conforme descrito na seção a seguir.  
  
### <a name="implementing-creative-destruction"></a>Implementando a destruição criativa  
Destruição Creative é um termo de economia que descreve o desenvolvimento econômico criado pela destruição de um pedido anterior. Nos últimos anos, o termo foi aplicado a tecnologia da informação. Ele geralmente se refere aos mecanismos por qual infra-estrutura antiga é eliminada, não ao atualizá-lo, mas por substituí-la por algo completamente novo. O 2011 [Gartner Simpósio ITXPO](http://www.gartner.com/technology/symposium/orlando/) para CIOs e executivos de TI sênior apresentado destruição criativa como um de seus temas principais para redução de custos e aumenta eficiência. Melhorias na segurança são possíveis como um crescimento natural do processo.  

Por exemplo, uma organização pode ser composta de várias unidades de negócios que usam um aplicativo diferente que executa uma funcionalidade semelhante, com graus variados de suporte modernidade e fornecedor. Historicamente, IT pode ser responsável por manter o aplicativo de cada unidade de negócios separadamente e esforços de consolidação consistiria tentando descobrir qual aplicativo ofereceu a funcionalidade melhor e, em seguida, migrar dados para que aplicativo de outros.  
  
Na destruição criativa, em vez de manter aplicativos redundantes ou desatualizados, implementar aplicativos totalmente novos para substituir o antigo, migrar dados para os novos aplicativos e encerrar os aplicativos antigos e os sistemas em que são executadas. Em alguns casos, você pode implementar creative destruição de aplicativos herdados, implantando um novo aplicativo em sua própria infraestrutura, mas sempre que possível, você deve considerar a portabilidade do aplicativo para uma solução baseada em nuvem em vez disso.  
  
Implantando aplicativos baseados em nuvem para substituir os aplicativos herdados, você não só reduz os custos e os trabalhos de manutenção, mas reduzir a superfície de ataque da sua organização, eliminando os sistemas herdados e aplicativos que apresentam vulnerabilidades os invasores utilizarem. Essa abordagem fornece uma maneira mais rápida para uma organização obter a funcionalidade desejada enquanto elimina simultaneamente destinos herdados na infraestrutura. Embora o princípio de destruição creative não se aplica a todos os ativos de TI, ele fornece uma opção viável com frequência para eliminar os aplicativos e sistemas herdados ao implantar simultaneamente aplicativos robustos, seguros e baseado em nuvem.  
  
### <a name="isolating-legacy-systems-and-applications"></a>Isolando aplicativos e sistemas herdados  
Um crescimento natural da migração de seus sistemas e usuários de negócios críticos para um ambiente seguro e puro é que sua floresta herdada conterá informações valiosas menor e sistemas. Embora os sistemas herdados e aplicativos que permanecem no ambiente menos seguro podem apresentar risco elevado de comprometimento, eles também representam uma severidade de redução de comprometimento. Ao rehoming e modernizar seus ativos críticos de negócios, você pode se concentrar em um gerenciamento eficiente de implantação e monitoramento ao mesmo tempo, não precisando acomodar os protocolos e as configurações herdadas.  
  
Quando você tiver movidos dados críticos para uma floresta puro, você pode avaliar as opções para isolar ainda mais os sistemas herdados e aplicativos em sua floresta do AD DS "main". Embora você pode implementar destruição criativa para substituir um aplicativo e os servidores em que é executado, em outros casos, você pode considerar isolamento adicionais dos sistemas e aplicativos menos seguros. Por exemplo, um aplicativo que é usado por alguns usuários, mas que requer credenciais herdadas como hashes do LAN Manager podem ser migradas para um domínio pequeno você cria para dar suporte a sistemas em que você não tenha que nenhuma opção de substituição.  
  
Removendo esses sistemas de domínios em que eles forçados a implementação de configurações herdadas, você subsequentemente pode aumentar a segurança dos domínios Configurando-os para dar suporte a apenas os aplicativos e sistemas operacionais atuais. No entanto, é preferível encerrar sistemas herdados e aplicativos sempre que possível. Se o encerramento simplesmente não é viável para um pequeno segmento da população do herdados, separar-o em um domínio separado (ou floresta) permite executar melhorias incrementais no restante da instalação herdada.  
  
### <a name="simplifying-security-for-end-users"></a>Simplificando a segurança para os usuários finais  
Na maioria das organizações, os usuários que têm acesso às informações confidenciais devido à natureza de suas funções na organização geralmente têm a menor quantidade de tempo para dedicar a controles e restrições de acesso complexos de aprendizado. Embora você deve ter um programa de treinamento de segurança abrangente para todos os usuários em sua organização, você deve também se concentrar em torna tão simples de usar quanto possível com a implementação de controles que são os princípios de simplificar e transparentes ao qual os usuários de segurança aderir.  
  
Por exemplo, você pode definir uma política em que os executivos e outros VIPs são necessários para usar estações de trabalho seguras para acessar dados confidenciais e sistemas, permitindo que eles usem seus outros dispositivos para acessar dados menos confidenciais. Esse é um princípio simple a serem lembradas pelos usuários, mas você pode implementar uma série de controles de back-end para ajudar a impor a abordagem.  

Você pode usar [garantia do mecanismo de autenticação](https://technet.microsoft.com/library/dd391847(v=WS.10).aspx) para permitir que os usuários acessem dados confidenciais apenas se eles já fez logon no seus sistemas seguros usando cartões inteligentes e podem usar o IPsec e restrições para controle de direitos de usuário a sistemas do qual eles possam se conectar a repositórios de dados confidenciais. Você pode usar o [Microsoft Data Classification Toolkit](https://www.microsoft.com/download/details.aspx?id=27123) criar uma infraestrutura de classificação de arquivos robusto e você pode implementar [controle de acesso dinâmico](http://blogs.technet.com/b/windowsserver/archive/2012/05/22/introduction-to-windows-server-2012-dynamic-access-control.aspx) para restringir o acesso a dados com base em características de uma tentativa de acesso, traduzindo regras de negócio para controles técnicos.  
  
Da perspectiva do usuário, acessar dados confidenciais de um sistema seguro "simplesmente funciona" e tentar fazer isso partir de um sistema não seguro "simplesmente não". No entanto, da perspectiva do monitoramento e gerenciamento de seu ambiente, você está ajudando a criar padrões de identificação em como os usuários acessam dados confidenciais e sistemas, tornando mais fácil para detectar tentativas de acesso anormais.  
  
Em ambientes em que o usuário a resistência a senhas longas e complexas resultou em diretivas de senha insuficientes, especialmente para os usuários de VIP, considere as abordagens alternativas para autenticação, seja por meio de cartões inteligentes (que são fornecidos em uma série de fatores de formulário e com recursos adicionais para fortalecer a autenticação), controles biométricos, como leitores de passar o dedo ou dados de autenticação, mesmo que são protegidos pelo confiam chips de platform module (TPM) com suporte nos computadores dos usuários. Embora a autenticação multifator não impede ataques de roubo de credenciais, se um computador já estiver comprometido, fornecendo aos usuários os controles de autenticação fácil de usar, você pode atribuir senhas mais seguras para as contas de usuários para quem tradicional usuário controles de nome e senha são complicados.  
