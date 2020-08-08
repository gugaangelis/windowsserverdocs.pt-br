---
ms.assetid: 6f50476c-a1f1-48fb-999b-76c4c3816496
title: Planejar para comprometimento
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c7347f247b9e637c610d73e0b37018bd54572dea
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994297"
---
# <a name="planning-for-compromise"></a>Planejar para comprometimento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Número da lei um: ninguém acredita que algo errado pode acontecer com eles, até mesmo.* - [10 leis imutáveis da administração de segurança](/previous-versions/cc722488(v=technet.10))

Os planos de recuperação de desastres em muitas organizações concentram-se na recuperação de desastres regionais ou falhas que resultam em perda de serviços de computação. No entanto, ao trabalhar com clientes comprometidos, muitas vezes descobrimos que a recuperação de comprometimento intencional está ausente em seus planos de restituição de desastres. Isso é particularmente verdadeiro quando o comprometimento resulta em roubo de propriedade intelectual ou destruição intencional que aproveita os limites lógicos (como a destruição de todos os domínios de Active Directory ou todos os servidores) em vez de limites físicos (como destruição de um datacenter). Embora uma organização possa ter planos de resposta a incidentes que definam atividades iniciais a serem executadas quando um comprometimento for descoberto, esses planos geralmente omitim as etapas para se recuperar de um comprometimento que afeta toda a infraestrutura de computação.

Como o Active Directory fornece recursos avançados de gerenciamento de identidade e acesso para usuários, servidores, estações de trabalho e aplicativos, ele é invariavelmente direcionado a invasores. Se um invasor obtiver acesso altamente privilegiado a um domínio de Active Directory ou controlador de domínio, esse acesso poderá ser utilizado para acessar, controlar ou até mesmo destruir toda a floresta Active Directory.

Este documento abordou alguns dos ataques mais comuns contra o Windows e Active Directory e as contramedidas que você pode implementar para reduzir a superfície de ataque, mas a única maneira de se recuperar no caso de um comprometimento completo do Active Directory é estar preparado para o comprometimento antes que isso aconteça. Esta seção enfoca os detalhes de implementação técnica do que as seções anteriores deste documento e mais sobre recomendações de alto nível que você pode usar para criar uma abordagem holística e abrangente para proteger e gerenciar os ativos críticos de negócios e de ti da sua organização.

Se sua infraestrutura nunca foi atacada, resisteu a tentar violações ou se sucumbiu a ataques e foi totalmente comprometida, você deve planejar a realidade inevitável que será atacado novamente e novamente. Não é possível evitar ataques, mas, na verdade, pode ser possível evitar violações significativas ou comprometer o atacado. Cada organização deve avaliar de forma minuciosa seus programas de gerenciamento de riscos existentes e fazer os ajustes necessários para ajudar a reduzir o nível geral da vulnerabilidade, tornando os investimentos equilibrados em prevenção, detecção, confinamento e recuperação.

Para criar defesas efetivas e ainda fornecer serviços aos usuários e às empresas que dependem de sua infraestrutura e aplicativos, talvez seja necessário considerar as maneiras novas de prevenir, detectar e conter o comprometimento em seu ambiente e, em seguida, recuperar-se do comprometimento. As abordagens e recomendações deste documento podem não ajudá-lo a reparar uma instalação do Active Directory comprometida, mas pode ajudá-lo a proteger seu próximo.

As recomendações para a recuperação de uma floresta de Active Directory são apresentadas no [Windows Server 2012: planejamento para Active Directory a restauração da floresta](https://www.microsoft.com/download/details.aspx?id=16506). Você pode impedir que seu novo ambiente seja completamente comprometido, mas mesmo que não seja possível, você terá ferramentas para recuperar e reobter o controle do seu ambiente.

## <a name="rethinking-the-approach"></a>Relembrando a abordagem
*Número da lei oito: a dificuldade de defender uma rede é diretamente proporcional à sua complexidade.* - [10 leis imutáveis da administração de segurança](/previous-versions/cc722488(v=technet.10))

Geralmente, é bem aceito que, se um invasor obtiver acesso de sistema, administrador, raiz ou equivalente a um computador, independentemente do sistema operacional, esse computador não poderá mais ser considerado confiável, não importa quantos esforços são feitos para "limpar" o sistema. Active Directory não é diferente. Se um invasor tiver obtido acesso privilegiado a um controlador de domínio ou uma conta altamente privilegiada no Active Directory, a menos que você tenha um registro de cada modificação que o invasor faz ou um bom backup, você nunca poderá restaurar o diretório para um estado completamente confiável.

Quando um servidor membro ou uma estação de trabalho é comprometido e alterado por um invasor, o computador não é mais confiável, mas os servidores e estações de trabalho descomprometidos vizinhos ainda podem ser confiáveis. O comprometimento de um computador não significa que todos os computadores estejam comprometidos.

No entanto, em um domínio Active Directory, todos os controladores de domínio hospedam réplicas do mesmo banco de dados AD DS. Se um único controlador de domínio for comprometido e um invasor modificar o banco de dados AD DS, essas modificações serão replicadas para todos os outros controladores de domínio no domínio e, dependendo da partição na qual as modificações são feitas, a floresta. Mesmo que você reinstale todos os controladores de domínio na floresta, basta reinstalar os hosts nos quais o banco de dados do AD DS reside. Modificações mal-intencionadas em Active Directory serão replicadas para controladores de domínio instalados com facilidade assim que forem replicadas para controladores de domínio em execução há anos.

Na avaliação de ambientes comprometidos, normalmente descobrimos que o que acredita ser a primeira violação "evento" foi, na verdade, disparado após semanas, meses ou até mesmo anos depois que os invasores contivessem comprometido inicialmente o ambiente. Os invasores normalmente obtiveram as credenciais para contas altamente privilegiadas desde que uma violação foi detectada e aproveitaram essas contas para comprometer o diretório, controladores de domínio, servidores membro, estações de trabalho e até mesmo sistemas conectados que não são do Windows.

Essas descobertas são consistentes com várias descobertas no relatório de investigações de violação de dados 2012 da Verizon, que afirma que:

-   98% de violações de dados originadas de agentes externos

-   85% das violações de dados levaram semanas ou mais para descobrir

-   92% dos incidentes foram descobertos por terceiros e

-   97% das violações foram podem ser evitadoss por meio de controles simples ou intermediários.

Um compromisso com o grau descrito anteriormente é praticamente irreparável, e o Conselho padrão para "achatar e recompilar" todos os sistemas comprometidos simplesmente não é viável ou até mesmo possível se Active Directory tiver sido comprometido ou destruído. Mesmo a restauração para um estado válido conhecido não elimina as falhas que permitiam que o ambiente fosse comprometido em primeiro lugar.

Embora seja necessário defender cada faceta de sua infraestrutura, um invasor só precisa encontrar falhas suficientes em suas defesas para chegar ao objetivo desejado. Se o seu ambiente for relativamente simples e original, e historicamente gerenciado, a implementação das recomendações fornecidas anteriormente neste documento poderá ser uma proposta simples.

No entanto, descobrimos que o ambiente mais antigo, maior e complexo, quanto mais provável é que as recomendações neste documento sejam inviáveis ou até mesmo impossíveis de implementar. É muito mais difícil proteger uma infraestrutura após o fato do que começar a ser atualizada e construir um ambiente resistente a ataques e comprometimento. Mas, como observado anteriormente, não é uma pequena tarefa recriar uma floresta Active Directory inteira. Por esses motivos, recomendamos uma abordagem direcionada mais focada para proteger suas florestas de Active Directory.

Em vez de se concentrar e tentar corrigir todas as coisas "desfeitas", considere uma abordagem em que você priorize com base no que é mais importante para seus negócios e em sua infraestrutura. Em vez de tentar corrigir um ambiente preenchido com sistemas e aplicativos desatualizados e mal configurados, considere criar um novo ambiente pequeno e seguro no qual você possa portar com segurança os usuários, os sistemas e as informações mais importantes para o seu negócio.

Nesta seção, descrevemos uma abordagem pela qual você pode criar uma original AD DS floresta que funciona como uma "barco de vida" ou "célula segura" para sua infraestrutura de negócios principal. Uma floresta original é simplesmente uma floresta Active Directory recentemente instalada que normalmente é limitada em tamanho e escopo, e que é criada usando sistemas operacionais atuais, aplicativos e os princípios descritos na [redução da superfície de ataque de Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md).

Ao implementar as definições de configuração recomendadas em uma floresta recém-criada, você pode criar uma AD DS instalação criada desde o início com configurações e práticas seguras, e você pode reduzir os desafios que acompanham o suporte a sistemas e aplicativos herdados. Embora as instruções detalhadas para o design e a implementação de um original AD DS instalação estejam fora do escopo deste documento, você deve seguir alguns princípios e diretrizes gerais para criar uma "célula segura" na qual você pode alojar seus ativos mais críticos. Essas diretrizes são as seguintes:

1.  Identifique princípios para separar e proteger ativos críticos.

2.  Defina um plano de migração limitado baseado em risco.

3.  Aproveite as migrações "nonmigratory" quando necessário.

4.  Implemente "destruição criativa".

5.  Isole os sistemas e aplicativos herdados.

6.  Simplifique a segurança para os usuários finais.

### <a name="identifying-principles-for-segregating-and-securing-critical-assets"></a>Identificar princípios para separar e proteger ativos críticos

As características do ambiente original que você cria para alojar ativos críticos podem variar muito. Por exemplo, você pode optar por criar uma floresta original na qual você migra somente usuários VIP e dados confidenciais que somente os usuários podem acessar. Você pode criar uma floresta original na qual você migra não apenas usuários VIP, mas que você implementa como uma floresta administrativa, implementando os princípios descritos em [reduzindo a superfície de ataque de Active Directory](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md) para criar contas administrativas seguras e hosts que podem ser usados para gerenciar suas florestas herdadas da floresta original. Você pode implementar uma floresta "desenvolvida especificamente" que hospeda contas VIP, contas com privilégios e sistemas que exigem segurança adicional, como servidores que executam Active Directory serviços de certificados (AD CS) com o único objetivo de segregá-los de florestas menos seguras. Por fim, você pode implementar uma floresta original que se torna o local de fato para todos os novos usuários, sistemas, aplicativos e dados, permitindo que você eventualmente encerre sua floresta herdada por meio do desgaste.

Independentemente de sua floresta original contiver alguns usuários e sistemas ou se ela for a base para uma migração mais agressiva, você deve seguir esses princípios em seu planejamento:

1.  Suponha que suas florestas herdadas foram comprometidas.

2.  Não configure um ambiente original para confiar em uma floresta herdada, embora você possa configurar um ambiente herdado para confiar em uma floresta original.

3.  Não migre contas de usuário ou grupos de uma floresta herdada para um ambiente original se houver uma possibilidade de que as associações de grupo das contas, o histórico SID ou outros atributos possam ter sido modificados de forma mal-intencionada. Em vez disso, use abordagens "nonmigratory" para popular uma floresta original. (As abordagens de Nonmigratory são descritas posteriormente nesta seção.)

4.  Não migre computadores de florestas herdadas para florestas original. Implementar servidores instalados recentemente na floresta original, instalar aplicativos nos servidores instalados recentemente e migrar dados de aplicativos para os sistemas recém-instalados. Para servidores de arquivos, copie dados para servidores instalados recentemente, defina ACLs usando usuários e grupos na nova floresta e, em seguida, crie servidores de impressão de maneira semelhante.

5.  Não permita a instalação de sistemas operacionais herdados ou aplicativos na floresta original. Se um aplicativo não puder ser atualizado e instalado recentemente, deixe-o na floresta herdada e considere a destruição criativa para substituir a funcionalidade do aplicativo.

### <a name="defining-a-limited-risk-based-migration-plan"></a>Definindo um plano de migração limitado baseado em risco
Criar um plano de migração limitado baseado em risco simplesmente significa que, ao decidir quais usuários, aplicativos e dados serão migrados para sua floresta original, você deverá identificar os destinos de migração com base no grau de risco para o qual sua organização será exposta se um dos usuários ou sistemas for comprometido. Os usuários VIP cujas contas têm mais probabilidade de ser alvo de invasores devem estar hospedados na floresta original. Os aplicativos que fornecem funções corporativas vitais devem ser instalados em servidores com nova compilação na floresta original, e dados altamente confidenciais devem ser movidos para servidores seguros na floresta original.

Se você ainda não tiver uma visão clara dos usuários, sistemas, aplicativos e dados mais críticos para os negócios em seu ambiente de Active Directory, trabalhe com unidades de negócios para identificá-los. Qualquer aplicativo necessário para operar a empresa deve ser identificado, assim como os servidores em que os aplicativos críticos são executados ou dados críticos são armazenados. Ao identificar os usuários e recursos necessários para que sua organização continue a funcionar, você cria uma coleção priorizada naturalmente de ativos nos quais concentrar seus esforços.

### <a name="leveraging-nonmigratory-migrations"></a>Aproveitando as migrações "Nonmigratory"
Quer você saiba que seu ambiente foi comprometido, suspeite de que ele foi comprometido ou simplesmente prefira não migrar dados herdados e objetos de uma instalação herdada de Active Directory para uma nova, considere as abordagens de migração que não tecnicamente "migram" objetos.

### <a name="user-accounts"></a>Contas de usuário
Em uma migração Active Directory tradicional de uma floresta para outra, o atributo SIDHistory (histórico de SID) em objetos de usuário é usado para armazenar o SID dos usuários e os SIDs de grupos dos quais os usuários eram membros na floresta herdada. Se as contas de usuários forem migradas para uma nova floresta e acessarem recursos na floresta herdada, os SIDs no histórico de SID serão usados para criar um token de acesso que permita que os usuários acessem recursos aos quais tinham acesso antes de as contas serem migradas.

A manutenção do histórico SID, no entanto, tem um problema comprovado em alguns ambientes porque popular os tokens de acesso dos usuários com os SIDs atuais e históricos pode resultar em inchar de token. O excesso de tokens é um problema no qual o número de SIDs que devem ser armazenados no token de acesso de um usuário usa ou excede a quantidade de espaço disponível no token.

Embora os tamanhos de token possam ser aumentados para uma extensão limitada, a solução definitiva para o excesso de tokens é reduzir o número de SIDs associados a contas de usuário, independentemente de racionalizar as associações de grupo, eliminar o histórico de SID ou uma combinação de ambos. Para obter mais informações sobre o inchar de token, consulte [MaxTokenSize e o token de Kerberos inflado](/archive/blogs/shanecothran/maxtokensize-and-kerberos-token-bloat).

Em vez de migrar usuários de um ambiente herdado (especialmente um em que as associações de grupo e os históricos SID podem ser comprometidos) usando o histórico de SID, considere aproveitar os aplicativos de metadiretório para "migrar" os usuários, sem carregar históricos de SID na nova floresta. Quando as contas de usuário são criadas na nova floresta, você pode usar um aplicativo de metadiretório para mapear as contas para suas contas correspondentes na floresta herdada.

Para fornecer as novas contas de usuário acesso aos recursos na floresta herdada, você pode usar a ferramenta de metadiretório para identificar grupos de recursos aos quais as contas herdadas dos usuários receberam acesso e, em seguida, adicionar as novas contas dos usuários a esses grupos. Dependendo da sua estratégia de grupo na floresta herdada, talvez seja necessário criar grupos locais de domínio para acesso a recursos ou converter grupos existentes em grupos locais de domínio para permitir que as novas contas sejam adicionadas aos grupos de recursos. Concentrando-se primeiro nos aplicativos e dados mais críticos e migrando-os para o novo ambiente (com ou sem o histórico de SID), você pode limitar a quantidade de esforços gastos no ambiente herdado.



### <a name="servers-and-workstations"></a>Servidores e estações de trabalho
Em uma migração tradicional de uma floresta Active Directory para outra, a migração de computadores geralmente é relativamente simples em comparação com a migração de usuários, grupos e aplicativos. Dependendo da função do computador, a migração para uma nova floresta pode ser tão simples quanto a separação de um domínio antigo e a junção de um novo. No entanto, migrar as contas de computador intactas em uma floresta original derrota a finalidade de criar um ambiente novo. Em vez de migrar contas de computador (potencialmente comprometidas, configuradas incorretamente ou desatualizadas) para uma nova floresta, você deve instalar com atualização servidores e estações de trabalho no novo ambiente. Você pode migrar dados de sistemas na floresta herdada para sistemas na floresta original, mas não os sistemas que hospedam os dados.

### <a name="applications"></a>Aplicativos

Os aplicativos podem apresentar o desafio mais significativo em qualquer migração de uma floresta para outra, mas no caso de uma migração de "nonmigratory", um dos princípios mais básicos que você deve aplicar é que os aplicativos na floresta original devem ser atuais, compatíveis e instalados de forma recente. Os dados podem ser migrados de instâncias do aplicativo na floresta antiga sempre que possível. Em situações em que um aplicativo não pode ser "recriado" na floresta original, você deve considerar abordagens como destruição criativa ou isolamento de aplicativos herdados, conforme descrito na seção a seguir.

### <a name="implementing-creative-destruction"></a>Implementando a destruição criativa
A destruição criativa é um termo econômico que descreve o desenvolvimento econômico criado pela destruição de uma ordem anterior. Nos últimos anos, o termo foi aplicado à tecnologia da informação. Normalmente, ele se refere a mecanismos pelos quais a infraestrutura antiga é eliminada, não atualizando-a, mas substituindo-a por algo totalmente novo. A 2011 [Gartner Symposium ITXPO](http://www.gartner.com/technology/symposium/orlando/) para cios e executivos de ti sêniores apresentou destruição criativa como um de seus principais temas para redução de custos e aumentos na eficiência. Melhorias na segurança são possíveis como um crescimento natural do processo.

Por exemplo, uma organização pode ser composta por várias unidades de negócios que usam um aplicativo diferente que executa uma funcionalidade semelhante, com graus variados de modernização e suporte de fornecedor. Historicamente, pode ser responsável por manter o aplicativo de cada unidade de negócios separadamente, e os esforços de consolidação consistem em tentar descobrir qual aplicativo oferecia a melhor funcionalidade e, em seguida, migrar dados para esse aplicativo dos outros.

Na destruição criativa, em vez de manter aplicativos desatualizados ou redundantes, você implementa aplicativos totalmente novos para substituir os dados antigos, migrar para os novos aplicativos e encerrar os aplicativos antigos e os sistemas nos quais eles são executados. Em alguns casos, você pode implementar a destruição criativa de aplicativos herdados implantando um novo aplicativo em sua própria infraestrutura, mas sempre que possível, considere a possibilidade de portar o aplicativo para uma solução baseada em nuvem.

Ao implantar aplicativos baseados em nuvem para substituir aplicativos internos herdados, você não apenas reduz os esforços e os custos de manutenção, mas reduz a superfície de ataque da sua organização, eliminando sistemas herdados e aplicativos que apresentam vulnerabilidades para que os invasores possam aproveitar. Essa abordagem fornece uma maneira mais rápida para uma organização obter a funcionalidade desejada ao mesmo tempo que elimina os destinos herdados na infraestrutura. Embora o princípio da destruição criativa não se aplique a todos os ativos de ti, ele fornece uma opção viável com frequência para eliminar sistemas e aplicativos herdados e, ao mesmo tempo, implantar aplicativos robustos, seguros e baseados em nuvem.

### <a name="isolating-legacy-systems-and-applications"></a>Isolando sistemas e aplicativos herdados
Um crescimento natural da migração de seus sistemas e usuários críticos para os negócios para um ambiente original e seguro é que sua floresta herdada conterá sistemas e informações menos valiosas. Embora os sistemas herdados e os aplicativos que permanecem no ambiente menos seguro possam apresentar um risco elevado de comprometimento, eles também representam uma severidade reduzida de comprometimento. Ao rehomear e modernizar seus ativos críticos de negócios, você pode se concentrar na implantação de gerenciamento e monitoramento efetivos, enquanto não precisa acomodar configurações e protocolos herdados.

Quando você tiver rehomeado seus dados críticos para uma floresta do original, poderá avaliar as opções para isolar ainda mais os sistemas herdados e aplicativos em sua floresta de AD DS "principal". Embora você possa implementar a destruição criativa para substituir um aplicativo e os servidores nos quais ele é executado, em outros casos, você pode considerar o isolamento adicional dos sistemas e aplicativos menos seguros. Por exemplo, um aplicativo que é usado por alguns usuários, mas que requer credenciais herdadas, como hashes do LAN Manager, pode ser migrado para um pequeno domínio que você cria para dar suporte a sistemas para os quais não há opções de substituição.

Ao remover esses sistemas de domínios nos quais eles forçaram a implementação das configurações herdadas, você pode aumentar posteriormente a segurança dos domínios Configurando-os para dar suporte apenas a sistemas operacionais e aplicativos atuais. Contudo, é preferível descomissionar sistemas e aplicativos herdados sempre que possível. Se o encerramento simplesmente não for viável para um pequeno segmento de sua população herdada, separá-lo em um domínio (ou floresta) separado permite que você execute melhorias incrementais no restante da instalação herdada.

### <a name="simplifying-security-for-end-users"></a>Simplificando a segurança para usuários finais
Na maioria das organizações, os usuários que têm acesso às informações mais confidenciais devido à natureza de suas funções na organização geralmente têm a menor quantidade de tempo para o aprendizado de controles e restrições de acesso complexos. Embora você tenha um programa abrangente de educação em segurança para todos os usuários em sua organização, você também deve se concentrar em tornar a segurança o mais simples de usar, implementando controles que são transparentes e simplificando os princípios para os quais os usuários aderem.

Por exemplo, você pode definir uma política na qual executivos e outros VIPs são necessários para usar estações de trabalho seguras para acessar dados e sistemas confidenciais, permitindo que eles usem seus outros dispositivos para acessar dados menos confidenciais. Esse é um princípio simples para os usuários se lembrarem, mas você pode implementar vários controles de back-end para ajudar a impor a abordagem.

Você pode usar a [garantia de mecanismo de autenticação](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd391847(v=ws.10)) para permitir que os usuários acessem dados confidenciais somente se tiverem feito logon em seus sistemas seguros usando seus cartões inteligentes e puderem usar restrições de direitos de usuário e IPsec para controlar os sistemas dos quais eles podem se conectar a repositórios de dados confidenciais. Você pode usar o [Microsoft Data Classification Toolkit](https://www.microsoft.com/download/details.aspx?id=27123) para criar uma infraestrutura de classificação de arquivos robusta e pode implementar o [controle de acesso dinâmico](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd391847(v=ws.10)) para restringir o acesso a dados com base nas características de uma tentativa de acesso, traduzindo as regras de negócio para controles técnicos.

Da perspectiva do usuário, acesso a dados confidenciais de um sistema protegido "apenas funciona" e tentando fazer isso por meio de um sistema desprotegido "simplesmente não". No entanto, da perspectiva de monitoramento e gerenciamento de seu ambiente, você está ajudando a criar padrões identificáveis em como os usuários acessam dados e sistemas confidenciais, facilitando a detecção de tentativas de acesso anômalas.

Em ambientes nos quais a resistência do usuário é longa, senhas complexas resultaram em políticas de senha insuficientes, especialmente para usuários VIP, considere abordagens alternativas para autenticação, seja por meio de cartões inteligentes (que vêm em vários fatores forma e com recursos adicionais para reforçar a autenticação), controles biométricos, como leitores de dedo, ou até mesmo dados de autenticação protegidos por chips de TPM (Trusted Platform Module) nos computadores dos usuários. Embora a autenticação multifator não impeça ataques de roubo de credenciais se um computador já estiver comprometido, fornecendo aos usuários controles de autenticação fáceis de usar, você poderá atribuir senhas mais robustas às contas de usuários para as quais os controles de nome de usuário e senha tradicionais são difíceis.
