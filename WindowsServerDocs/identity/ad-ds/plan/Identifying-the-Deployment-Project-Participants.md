---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: "Identificando os participantes do projeto de implantação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57c24c2b2b48c712445ef7dca453de586d33a130
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-the-deployment-project-participants"></a>Identificando os participantes do projeto de implantação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa do estabelecimento de um projeto de implantação para o serviço de domínio do Active Directory (AD DS) é estabelecer o design e ciclo de equipes de projeto de implantação que serão responsáveis por gerenciar a fase de design e a fase de implantação do projeto do Active Directory. Além disso, você deve identificar as pessoas e os grupos que serão responsáveis por proprietário e manter o diretório após a implantação for concluída.  
  
-   [Definir funções específicas do projeto](#BKMK_1)  
  
-   [O estabelecimento de proprietários e os administradores](#BKMK_2)  
  
-   [Equipes de projeto de construção](#BKMK_3)  
  
## <a name="BKMK_1"></a>Definir funções específicas do projeto  
Uma etapa importante ao estabelecer as equipes de projeto é identificar as pessoas que são para manter a funções específicas do projeto. Isso inclui o patrocinador executivo, o arquiteto do projeto e o Gerenciador de projeto. Essas pessoas são responsáveis por executar o projeto de implantação do Active Directory.  
  
Depois que você nomeia o arquiteto do projeto e o Gerenciador de projeto, essas pessoas estabelecem canais de comunicação em toda a organização, criar agendamento de projetos e identificam as pessoas que serão membros das equipes de projeto, começando com os vários proprietários.  
  
### <a name="executive-sponsor"></a>Executivo  
Implantar uma infraestrutura como o AD DS pode ter um impacto de longo alcance em uma organização. Por esse motivo, é importante ter um patrocinador executivo que compreende o valor de negócios da implantação, compatível com o projeto no nível do executivo e pode ajudar a resolver conflitos em toda a organização.  
  
### <a name="project-architect"></a>Arquiteto do projeto  
Cada projeto de implantação do Active Directory exige que um arquiteto do projeto gerenciar o processo de chamadas tomada de decisão design e implantação do Active Directory. O arquiteto fornece conhecimento técnico para ajudar com o processo de projetar e implantar o AD DS.  
  
> [!NOTE]  
> Se nenhuma equipe existente em sua organização tem design diretório experiência, convém contratar um consultor externo que é um especialista em design do Active Directory e implantação.  
  
As responsabilidades do arquiteto do Active Directory projeto incluem o seguinte:  
  
-   Ao qual pertence o design do Active Directory  
  
-   Compreendendo e gravação a lógica para decisões de design importantes  
  
-   Garantir que o design atenda às necessidades de negócios da organização  
  
-   Estabelecer um consenso entre as equipes de operações, implantação e design  
  
-   Noções básicas sobre as necessidades de aplicativos do AD DS integrada  
  
O design final do Active Directory deve refletir uma combinação de metas de negócios e decisões técnicas. Portanto, o arquiteto do projeto deve revisar as decisões de design para garantir que eles se alinham com as metas de negócios.  
  
### <a name="project-manager"></a>Gerente de projetos  
O Gerenciador de projeto facilita a cooperação em unidades de negócios e entre grupos de gerenciamento de tecnologia. Idealmente, o Gerenciador de projeto de implantação do Active Directory é alguém dentro da organização que esteja familiarizada com as políticas operacionais do grupo de TI e os requisitos de design para os grupos que estão se preparando para implantar o AD DS. O Gerenciador de projeto inspeciona o projeto de implantação inteira, a partir do design e continuando com a implementação e garante que o projeto permaneça no cronograma e dentro do orçamento. As responsabilidades do gerente incluem o seguinte:  
  
-   Fornecendo projeto básico planejar como agendamento e orçamento  
  
-   O progresso de direção no projeto design e implantação do Active Directory  
  
-   Garantindo que as pessoas apropriadas estão envolvidas em cada parte do processo de design  
  
-   Servindo como único ponto de contato para o projeto de implantação do Active Directory  
  
-   Estabelecer comunicação entre as equipes de operações, implantação e design  
  
-   Estabelecer e manter a comunicação com o patrocinador executivo durante todo o projeto de implantação  
  
## <a name="BKMK_2"></a>O estabelecimento de proprietários e os administradores  
Em um projeto de implantação do Active Directory, pessoas que são proprietários serão responsabilizadas pelo gerenciamento para garantir que essa implantação tarefas são concluídas e esse design do Active Directory especificações atendem às necessidades da organização. Os proprietários não necessariamente têm acesso a ou manipular a infraestrutura de diretório diretamente. Os administradores estão os indivíduos responsáveis por concluir as tarefas de implantação necessários. Os administradores têm o acesso à rede e as permissões necessárias para manipular o diretório e sua infraestrutura.  
  
A função do proprietário é estratégica e administrativa. Os proprietários serão responsáveis por comunicar aos administradores as tarefas necessárias para a implementação do design, como a criação de novos controladores de domínio na floresta do Active Directory. Os administradores são responsáveis por implementar o design da rede de acordo com as especificações de design.  
  
Em grandes organizações, pessoas diferentes preenchem funções proprietário e administrador; No entanto, em algumas pequenas empresas, a mesma pessoa pode atuar como o proprietário e o administrador.  
  
### <a name="service-and-data-owners"></a>Proprietários de serviços e dados  
Gerenciar o AD DS diariamente envolve dois tipos de proprietários:  
  
-   Proprietários de serviços que são responsáveis para manutenção de longo prazo e planejamento da infraestrutura do Active Directory e para garantir que o diretório continue funcionando e que as metas estabelecidas em contratos de nível de serviço são mantidas  
  
-   Proprietários de dados que são responsáveis pela manutenção das informações armazenadas no diretório. Isso inclui o usuário e o gerenciamento de contas de computador e o gerenciamento de recursos locais, como servidores membros e estações de trabalho.  
  
É importante identificar os proprietários de dados e serviços do Active Directory antecipadamente para que eles podem participar como grande parte do processo de design quanto possível. Como os proprietários de serviço e os dados são responsáveis pela manutenção a longo prazo do diretório depois que o projeto de implantação for concluído, é importante para essas pessoas para fornecer entrada em relação a necessidades da organização e estar familiarizado com como e por que certas decisões de design são feitas. Os proprietários de serviços incluem o proprietário da floresta, o proprietário do Active Directory domínio nomenclatura DNS (sistema) e o proprietário de topologia do site. Os proprietários de dados incluem os proprietários de unidade organizacional (UO).  
  
### <a name="service-and-data-administrators"></a>Administradores de serviços e dados  
A operação do AD DS envolve dois tipos de administradores: administradores e dados de serviço. Administradores de serviços implementam política decisões feitas pelos proprietários do serviço e tratam as tarefas diárias associadas à manutenção do serviço de diretório e infraestrutura. Isso inclui o gerenciamento de controladores de domínio que hospedam o serviço de diretório, gerenciamento de outros serviços de rede como DNS que são necessários para o AD DS, controlando a configuração das configurações de toda a floresta e garantindo que o diretório sempre está disponível.  
  
Administradores de serviços também serão responsáveis por concluir tarefas de implantação do Active Directory em andamento que são necessárias após a conclusão do processo de implantação do Active Directory do Windows Server 2008 inicial. Por exemplo, como demandas do aumento de diretório, administradores de serviços criar controladores de domínio adicionais e estabelecem ou remover relações de confiança entre domínios, conforme necessário. Por esse motivo, a equipe de implantação do Active Directory precisa incluir administradores de serviços.  
  
Você deve ter cuidado para atribuir funções do administrador de serviço somente a indivíduos confiáveis na organização. Como essas pessoas têm a capacidade de modificar os arquivos do sistema em controladores de domínio, eles poderão alterar o comportamento do AD DS. Você deve garantir que os administradores de serviços em sua organização são indivíduos que estão familiarizados com o operacionais e políticas de segurança que estejam em vigor em sua rede e que entende a necessidade de impor essas políticas.  
  
Administradores de dados são os usuários em um domínio que serão responsáveis por manter dados armazenados no AD DS, como contas de usuário e grupo e para manter os computadores que são membros do seu domínio. Os administradores de dados controlam subconjuntos de objetos dentro do diretório e não tem controle sobre a instalação ou a configuração do serviço de diretório.  
  
Contas de administrador de dados não são fornecidas por padrão. Depois que a equipe de design determina como recursos devem ser gerenciados para a organização, os proprietários de domínio devem criar contas de administrador de dados e delegá-los as permissões adequadas com base no conjunto de objetos para que os administradores devem ser responsável.  
  
É melhor limitar o número de administradores de serviços em sua organização para o número mínimo necessário para garantir que a infraestrutura continuará a funcionar. A maior parte do trabalho administrativo pode ser concluída por administradores de dados. Administradores de serviços exigem uma quantidade maior conjunto de habilidades porque eles são responsáveis por manter o diretório e a infraestrutura que dá suporte a ele. Os administradores de dados exigem apenas as habilidades necessárias para gerenciar sua parte do diretório. Dividir atribuições de trabalho dessa maneira resulta em economias para a organização porque apenas um pequeno número de administradores precisa ser treinados para operar e manter todo o diretório e sua infraestrutura.  
  
Por exemplo, um administrador de serviço precisa entender como adicionar um domínio a uma floresta. Isso inclui como instalar o software para converter um servidor em um controlador de domínio e como manipular o ambiente de DNS para que o controlador de domínio pode ser mesclado perfeitamente no ambiente do Active Directory. Um administrador de dados só precisa saber como gerenciar os dados específicos que eles são responsáveis por como a criação de novas contas de usuário para novos funcionários em seu departamento.  
  
Implantando o AD DS requer coordenação e comunicação entre vários grupos diferentes envolvidos na operação da infraestrutura de rede. Esses grupos devem indicar proprietários de serviços e dados que são responsáveis por representar os vários grupos durante o processo de design e implantação.  
  
Depois que o projeto de implantação for concluído, esses proprietários de serviços e dados continuam sendo responsável pela parte da infraestrutura gerenciada pelo seu grupo. Em um ambiente do Active Directory, estes proprietários são o proprietário da floresta, o DNS para proprietário do AD DS, o proprietário do site topologia e o proprietário de UO. As funções destes proprietários de serviço e os dados descritas nas seções a seguir.  
  
#### <a name="forest-owner"></a>Proprietário da floresta  
O proprietário da floresta normalmente é gerente de informática informações sênior na organização quem é responsável pelo processo de implantação do Active Directory e quem é o principal responsável por manter atendimento dentro da floresta depois que a implantação for concluída. O proprietário da floresta atribui indivíduos para preencher as outras funções de propriedade, identificando pessoal principal dentro da organização que são capazes de contribuem com informações necessárias sobre infraestrutura de rede e necessidades administrativas. O proprietário da floresta é responsável pelo seguinte:  
  
-   Implantação do domínio raiz da floresta para criar a floresta  
  
-   Implantação do primeiro controlador de domínio em cada domínio para criar os domínios necessários para a floresta  
  
-   Membros dos grupos de administrador de serviço em todos os domínios da floresta  
  
-   Criação do design da estrutura de UO para cada domínio na floresta  
  
-   Delegação de autoridade administrativa para proprietários de UO  
  
-   Alterações no esquema  
  
-   Alterações nas definições de configuração de toda a floresta  
  
-   Implementação de determinadas configurações de política de política de grupo, incluindo políticas de conta de usuário de domínio, como política de bloqueio de conta e senha refinada  
  
-   Configurações de política de negócios que se aplicam aos controladores de domínio  
  
-   Outras configurações de política de grupo que são aplicadas no nível do domínio  
  
O proprietário da floresta tem autoridade ao longo de toda a floresta. É responsabilidade do proprietário floresta para configurar políticas de negócios e da política de grupo e selecione as pessoas que são administradores de serviços. O proprietário da floresta é um proprietário de serviço.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS para proprietário do AD DS  
O DNS para proprietário do AD DS é uma pessoa que tenha um entendimento abrangente de infraestrutura de DNS e o namespace existente da organização.  
  
O DNS para proprietário do AD DS é responsável pelo seguinte:  
  
-   Serve como uma ligação entre a equipe de design e o grupo de TI que atualmente possui a infraestrutura DNS  
  
-   Fornecendo as informações sobre o namespace DNS existente da organização para ajudar na criação do novo namespace do Active Directory  
  
-   Trabalhando com a equipe de implantação para assegurar que a nova infraestrutura DNS é implantada de acordo com as especificações da equipe de design e se ele está funcionando corretamente  
  
-   Gerenciando o DNS para a infraestrutura do AD DS, incluindo o serviço de servidor DNS e dados DNS  
  
O DNS para proprietário do AD DS é um proprietário de serviço.  
  
#### <a name="site-topology-owner"></a>Proprietário do site topologia  
O proprietário do site topologia esteja familiarizado com a estrutura física da rede da organização, incluindo o mapeamento de sub-redes individuais, roteadores e áreas de rede que estão conectadas por meio de links lento. O proprietário de topologia de site é responsável pelo seguinte:  
  
-   Noções básicas sobre a topologia de rede física e como ele afeta o AD DS  
  
-   Noções básicas sobre como a implantação do Active Directory afetará a rede  
  
-   Determinando os sites lógicos do Active Directory que precisam ser criados  
  
-   Atualizando os objetos de site para controladores de domínio quando uma sub-rede é adicionada, modificado ou removidos  
  
-   Criando links de site, pontes e objetos de conexão manual  
  
O proprietário de topologia de site é um proprietário de serviço.  
  
#### <a name="ou-owner"></a>Proprietário de UO  
O proprietário de UO é responsável por gerenciar os dados armazenados no diretório. Essa pessoa precisa estar familiarizado com o operacionais e políticas de segurança que estão no lugar na rede. Os proprietários de UO podem executar apenas as tarefas que foram delegadas-los por administradores de serviços, e eles podem executar somente as tarefas nas UOs para que elas são atribuídas. Tarefas que podem ser atribuídas ao proprietário UO incluem o seguinte:  
  
-   Executar todas as tarefas de gerenciamento de conta dentro da UO respectiva atribuído  
  
-   Gerenciamento de estações de trabalho e servidores membro que são membros da UO seu atribuído  
  
-   Delegar autoridade para os administradores locais dentro da UO respectiva atribuído  
  
O proprietário de UO é um proprietário dos dados.  
  
## <a name="BKMK_3"></a>Equipes de projeto de construção  
As equipes de projeto do Active Directory são grupos temporários que serão responsáveis por concluir tarefas de design e implantação do Active Directory. Quando o projeto de implantação do Active Directory estiver concluído, os proprietários assumem responsabilidade para o diretório, e podem dispensar as equipes de projeto.  
  
O tamanho das equipes projeto varia de acordo com o tamanho da organização. Em pequenas empresas, uma única pessoa pode abranger várias áreas de responsabilidade de uma equipe de projeto e se envolver em mais de uma fase da implantação. Grandes organizações podem exigir equipes maiores com pessoas diferentes ou até mesmo diferentes equipes que cobre as diferentes áreas de responsabilidade. O tamanho das equipes não é importante, desde que todas as áreas de responsabilidade são atribuídas e as metas de design da organização sejam atendidas.  
  
### <a name="identifying-potential-forest-owners"></a>Identificando possíveis proprietários de floresta  
Identifique os grupos em sua organização que possui e controla os recursos necessários para fornecer serviços de diretório para os usuários na rede. Esses grupos são considerados prováveis proprietários da floresta.  
  
A separação de administração de serviço e os dados no AD DS torna possível para a infraestrutura IT grupo (ou grupos) de uma organização para gerenciar o serviço de diretório, enquanto os administradores locais em cada grupo gerenciam os dados que pertencem aos seus próprios grupos. Os proprietários de floresta potenciais tem autoridade necessária ao longo da infraestrutura de rede implantar e suporte ao AD DS.  
  
Para organizações que têm uma infraestrutura de centralizado um grupo de TI, o grupo de TI costuma ser o proprietário da floresta e, portanto, o proprietário de floresta potencial para qualquer implantações futuras. As organizações que incluem um número de infraestrutura independente IT grupos têm um número de possíveis proprietários de floresta. Se sua organização já tiver uma infraestrutura do Active Directory no local, qualquer proprietários de floresta atual também são possíveis proprietários da floresta para implantações de novas.  
  
Selecione um dos proprietários de floresta potencial para atuar como o proprietário da floresta para cada floresta que você está considerando para implantação. Estes possíveis proprietários de floresta são responsáveis por trabalhar com a equipe de design para determinar ou não sua floresta realmente será implantada ou se um alternativo curso de ação (como ingressar em outra floresta existente) é um melhor uso dos recursos disponíveis e ainda atende às suas necessidades. O proprietário da floresta (ou proprietários) em sua organização são membros da equipe de design do Active Directory.  
  
### <a name="establishing-a-design-team"></a>Estabelecendo uma equipe de design  
A equipe de design do Active Directory é responsável por reunir todas as informações necessárias para tomar decisões sobre o design de estrutura lógica do Active Directory.  
  
As responsabilidades da equipe de design incluem o seguinte:  
  
-   Determinar quantas florestas e domínios são necessários e quais são as relações entre os domínios e florestas  
  
-   Trabalhando com os proprietários de dados para garantir que o design atenda aos seus requisitos de segurança e administrativos  
  
-   Trabalhando com os administradores de rede atual para garantir que a infraestrutura de rede atual suporta o design e que o design não afetará negativamente existentes aplicativos implantados na rede  
  
-   Trabalhando com representantes do grupo de segurança da organização para garantir que o design atenda às políticas de segurança estabelecidas  
  
-   Criar estruturas de UO que permitem apropriados níveis de proteção e a delegação de autoridade adequada para os proprietários de dados  
  
-   Trabalhando com a equipe de implantação para testar o design em um laboratório ambiente para garantir que ele funcione como planejado e modificar o design como necessário para resolver quaisquer problemas que ocorrem  
  
-   Criando um design de topologia de site que atenda aos requisitos de replicação da floresta enquanto evita que a sobrecarga de largura de banda disponível. Para obter mais informações sobre como projetar a topologia de site, consulte [projetar o Site topologia para Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Trabalhando com a equipe de implantação para garantir que o design é implementado corretamente  
  
A equipe de design inclui os seguintes membros:  
  
-   Possíveis proprietários de floresta  
  
-   Arquiteto do projeto  
  
-   Gerente de projetos  
  
-   Pessoas que são responsáveis por estabelecer e manter políticas de segurança na rede  
  
Durante o processo de design de estrutura lógica, a equipe de design identifica os outros proprietários. Essas pessoas devem começar a participar do processo de design, assim que eles são identificados. Depois que o projeto de implantação é entregue para a equipe de implantação, a equipe de design é responsável por supervisionar o processo de implantação para garantir que o design é implementado corretamente. A equipe de design também faz alterações no design com base nos comentários de teste.  
  
### <a name="establishing-a-deployment-team"></a>Estabelecendo uma equipe de implantação  
A equipe de implantação do Active Directory é responsável por testar e implementar o design de estrutura lógica do Active Directory. Isso envolve as seguintes tarefas:  
  
-   O estabelecimento de um ambiente de teste que suficientemente emula o ambiente de produção  
  
-   Testando o design Implementando a estrutura proposta de floresta e domínio em um ambiente de laboratório para verificar se ele atende as metas de cada proprietário de função  
  
-   Desenvolver e testar os cenários de migração propostos por design em um ambiente de laboratório  
  
-   Certificando-se de que cada proprietário sai do sistema sobre o processo de teste para garantir que os recursos de design corretos estão sendo testados  
  
-   Testando a operação de implantação em um ambiente piloto  
  
Quando o design e testes tarefas são concluídas, a equipe de implantação executa as seguintes tarefas:  
  
-   Cria as florestas e domínios de acordo com o design de estrutura lógica do Active Directory  
  
-   Cria os objetos de link de site conforme necessário e sites com base no design de topologia de site  
  
-   Garante que a infraestrutura DNS está configurada para oferecer suporte ao AD DS e que qualquer novo namespaces são integrados ao namespace existente da organização  
  
A equipe de implantação do Active Directory inclui os seguintes membros:  
  
-   Proprietário da floresta  
  
-   DNS para proprietário do AD DS  
  
-   Proprietário do site topologia  
  
-   Proprietários de UO  
  
A equipe de implantação funciona com os administradores de serviços e dados durante a fase de implantação para garantir que os membros da equipe de operações esteja familiarizados com o novo design. Isso ajuda a assegurar uma transição suave de propriedade quando a operação de implantação é concluída. Após a conclusão do processo de implantação, a responsabilidade para manter o ambiente do Active Directory novo passa para a equipe de operações.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentar as equipes de design e implantação  
Documente os nomes e informações de contato para as pessoas que participarão do design e implantação do AD DS. Identifica quem será responsável por cada função as equipes de design e implantação. Inicialmente, essa lista inclui os proprietários de floresta possíveis, o Gerenciador de projeto e o arquiteto do projeto. Ao determinar o número de florestas implantada, você talvez precise criar novas equipes de design para florestas adicionais. Observe que você precisará atualizar sua documentação conforme mudam de associações de equipe e como a identificar os proprietários do Active Directory vários durante o processo de design. Para uma planilha para ajudá-lo a documentar as equipes de design e implantação de cada floresta, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip do trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e abra "Informações de equipe de implantação e Design" (DSSLOGI_1.doc).  
  


