---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Identificar os participantes do projeto de implantação
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 962825494c613dfef808ce54dfba22727abf6878
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834647"
---
# <a name="identifying-the-deployment-project-participants"></a>Identificar os participantes do projeto de implantação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa do estabelecimento de um projeto de implantação para o serviço de domínio do Active Directory (AD DS) é estabelecer o design e o ciclo de equipes de projeto de implantação que serão responsáveis por gerenciar a fase de design e a fase de implantação do projeto do Active Directory. Além disso, você deve identificar os indivíduos e grupos que serão responsáveis por proprietário e manter o diretório após a conclusão da implantação.  
  
-   [Definindo funções específicas do projeto](#BKMK_1)  
  
-   [Estabelecer os proprietários e administradores](#BKMK_2)  
  
-   [Equipes de projeto de construção](#BKMK_3)  
  
## <a name="BKMK_1"></a>Definindo funções específicas do projeto  
Uma etapa importante em estabelecer as equipes de projeto é identificar as pessoas que devem conter funções específicas do projeto. Isso inclui o patrocinador executivo, o arquiteto de projeto e o gerente de projeto. Essas pessoas são responsáveis por executar o projeto de implantação do Active Directory.  
  
Depois que você designa o arquiteto de projeto e o gerente de projeto, esses indivíduos estabelecer canais de comunicação em toda a organização, criar cronogramas do projeto e identificar os indivíduos que serão membros das equipes de projeto, começando com o vários proprietários.  
  
### <a name="executive-sponsor"></a>Patrocinador executivo  
Implantação de uma infraestrutura como o AD DS pode ter um impacto abrangente em uma organização. Por esse motivo, é importante ter um patrocinador executivo que compreende o valor comercial da implantação, oferece suporte o projetos em nível executivo e pode ajudar a resolver conflitos em toda a organização.  
  
### <a name="project-architect"></a>Arquiteto de projeto  
Cada projeto de implantação do Active Directory exige que um arquiteto de projeto gerenciar o Active Directory processo de design e implantação de tomada de decisão. O arquiteto fornece experiência técnica para ajudá-lo com o processo de projetar e implantar o AD DS.  
  
> [!NOTE]  
> Se nenhuma equipe existente em sua organização tiver o design do experiência, você talvez queira contratar um consultor externo que é um especialista em design do Active Directory e a implantação.  
  
As responsabilidades do arquiteto de projeto do Active Directory incluem o seguinte:  
  
-   Proprietário do design do Active Directory  
  
-   Noções básicas sobre e gravar a lógica para as principais decisões de design  
  
-   Garantir que o design atende às necessidades de negócios da organização  
  
-   Estabelecendo um consenso entre as equipes de operações, implantação e design  
  
-   Noções básicas sobre as necessidades de aplicativos do AD DS integrada  
  
O design do Active Directory final deve refletir uma combinação de decisões técnicas e metas de negócios. Portanto, o arquiteto de projeto deve examinar as decisões de design para garantir que eles estejam alinhadas aos objetivos de negócios.  
  
### <a name="project-manager"></a>Gerente de projeto  
O gerente de projeto facilita a cooperação entre unidades de negócios e entre grupos de gerenciamento de tecnologia. Idealmente, o gerente de projeto de implantação do Active Directory é alguém de dentro da organização que esteja familiarizada com as políticas operacionais do grupo de TI e os requisitos de design para os grupos que estiver se preparando para implantar o AD DS. O gerente de projeto supervisiona o projeto de implantação inteiro, começando com o design e continuando pela implementação e garante que o projeto permanece no prazo e dentro do orçamento. As responsabilidades do gerente de projeto incluem o seguinte:  
  
-   Planejamento de como o agendamento e o orçamento de projeto básico de fornecimento  
  
-   Conduzindo o progresso no projeto de design e implantação do Active Directory  
  
-   Garantir que as pessoas apropriadas estão envolvidas em cada parte do processo de design  
  
-   Que serve como ponto único de contato para o projeto de implantação do Active Directory  
  
-   Para estabelecer a comunicação entre as equipes de operações, implantação e design  
  
-   Estabelecer e manter a comunicação com o patrocinador executivo em todo o projeto de implantação  
  
## <a name="BKMK_2"></a>Estabelecer os proprietários e administradores  
Em um projeto de implantação do Active Directory, os indivíduos que são proprietários são mantidos responsáveis pelo gerenciamento para certificar-se de que as tarefas são concluídas de implantação e esse design do Active Directory especificações de atender às necessidades da organização. Proprietários não necessariamente têm acesso a ou manipular a infra-estrutura de diretório diretamente. Os administradores são os indivíduos responsáveis por concluir as tarefas de implantação necessárias. Os administradores têm o acesso à rede e as permissões necessárias para manipular o diretório e sua infra-estrutura.  
  
A função do proprietário é estratégico e administrativos. Os proprietários são responsáveis por comunicar-se aos administradores as tarefas necessárias para a implementação do design, como a criação de novos controladores de domínio na floresta do Active Directory. Os administradores são responsáveis por implementar o design de rede de acordo com as especificações de design.  
  
Em grandes organizações, pessoas diferentes preenchem as funções de proprietário e administrador; No entanto, em algumas organizações de pequeno, a mesma pessoa pode atuar como o proprietário e o administrador.  
  
### <a name="service-and-data-owners"></a>Proprietários de serviços e dados  
Gerenciar o AD DS em uma base diária envolve dois tipos de proprietários:  
  
-   Proprietários de serviço que são responsáveis pelo planejamento e de longo prazo de manutenção da infraestrutura do Active Directory e para garantir que o diretório continua a funcionar e que as metas estabelecidas em contratos de nível de serviço são mantidas  
  
-   Proprietários de dados que são responsáveis pela manutenção das informações armazenadas no diretório. Isso inclui o usuário e o gerenciamento de conta de computador e o gerenciamento de recursos locais, como servidores membro e estações de trabalho.  
  
É importante identificar os proprietários de serviços e dados do Active Directory no início, para que eles podem participar como grande parte do processo de design quanto possível. Como os proprietários de serviço e os dados são responsáveis pela manutenção de longo prazo do diretório depois que o projeto de implantação for concluído, é importante que essas pessoas para fornecer entrada sobre necessidades organizacionais e estar familiarizado com como e por que são feitas determinadas decisões de design. Os proprietários de serviços incluem o proprietário da floresta, o proprietário do domínio de nomenclatura System (DNS do Active Directory) e o proprietário da topologia de site. Os proprietários de dados incluem os proprietários da unidade organizacional (UO).  
  
### <a name="service-and-data-administrators"></a>Administradores de serviços e dados  
A operação do AD DS envolve dois tipos de administradores: administradores e administradores de dados de serviço. Os administradores de serviço implementam as decisões de política feitas pelos proprietários do serviço e lidar com as tarefas diárias associadas à manutenção do serviço de diretório e infraestrutura. Isso inclui gerenciamento de controladores de domínio que estão hospedando o serviço de diretório, gerenciamento de outros serviços de rede, como DNS que são necessários para o AD DS, controlar a definição das configurações de toda a floresta e garantindo que o diretório esteja sempre disponível.  
  
Os administradores de serviço também são responsáveis por concluir tarefas de implantação do Active Directory em andamento que são necessárias depois que o processo de implantação do Active Directory do Windows Server 2008 inicial for concluído. Por exemplo, como as demandas sobre o aumento de diretório, os administradores de serviço criar controladores de domínio adicionais e estabelecerem ou remover relações de confiança entre domínios, conforme necessário. Por esse motivo, a equipe de implantação do Active Directory precisa incluir os administradores de serviço.  
  
Você deve ter cuidado para atribuir funções de administrador de serviço somente a pessoas confiáveis na organização. Como essas pessoas têm a capacidade de modificar os arquivos do sistema nos controladores de domínio, eles podem alterar o comportamento do AD DS. Você deve garantir que os administradores de serviço em sua organização são indivíduos que estão familiarizados com operacional e as políticas de segurança que estão em vigor na sua rede e que compreendem a necessidade de aplicar essas políticas.  
  
Os administradores de dados são usuários dentro de um domínio que são responsáveis por manter os dados armazenados no AD DS, como contas de usuário e grupo e para manter os computadores que são membros de seus domínios. Os administradores de dados controlam subconjuntos de objetos dentro do diretório e não tem controle sobre a instalação ou configuração do serviço de diretório.  
  
Contas de administrador de dados não são fornecidas por padrão. Depois que a equipe de projeto determina como os recursos devem ser gerenciados para a organização, os proprietários do domínio devem criar contas de administrador de dados e delegar a eles as permissões apropriadas com base no conjunto de objetos para que os administradores devem ser responsável .  
  
É melhor limitar o número de administradores de serviço em sua organização para o número mínimo necessário para garantir que a infra-estrutura continua a funcionar. A maioria do trabalho administrativo pode ser concluída por administradores de dados. Os administradores de serviço exigir um muito mais amplo conjunto de habilidades porque eles são responsáveis por manter o diretório e a infraestrutura que dá suporte a ele. Os administradores de dados exigem apenas as habilidades necessárias para gerenciar sua parte do diretório. Dividir as atribuições de trabalho dessa maneira resulta em economia de custo para a organização porque somente um pequeno número de administradores precisa ser treinados para operar e manter todo o diretório e sua infra-estrutura.  
  
Por exemplo, um administrador de serviço precisa entender como adicionar um domínio a uma floresta. Isso inclui como instalar o software para converter um servidor em um controlador de domínio e como manipular o ambiente de DNS para que o controlador de domínio pode ser mesclado perfeitamente ao ambiente do Active Directory. Um administrador de dados só precisa saber como gerenciar os dados específicos que eles são responsáveis por como a criação de novas contas de usuário para os novos funcionários em seus departamentos.  
  
Implantando o AD DS exige a coordenação e a comunicação entre muitos grupos diferentes envolvidos na operação da infraestrutura de rede. Esses grupos devem designa a proprietários de serviços e dados que são responsáveis por que representa os vários grupos durante o processo de design e implantação.  
  
Depois que o projeto de implantação for concluído, esses proprietários de serviço e os dados continuam a ser responsável por parte da infraestrutura gerenciada pelo seu grupo. Em um ambiente do Active Directory, esses proprietários são o proprietário da floresta, o DNS para o proprietário do AD DS, o proprietário da topologia de site e o proprietário da UO. As funções desses proprietários de serviço e os dados são explicadas nas seções a seguir.  
  
#### <a name="forest-owner"></a>Proprietário da floresta  
Normalmente, o proprietário da floresta é gerente de TI (tecnologia) informações sênior na organização quem é responsável por processo de implantação do Active Directory e quem é o principal responsável pela manutenção de entrega de serviço na floresta depois do implantação for concluída. O proprietário da floresta atribui indivíduos para preencher as outras funções de propriedade por meio da identificação pessoal principal dentro da organização que podem contribuir com as informações necessárias sobre necessidades administrativas e de infraestrutura de rede. O proprietário da floresta é responsável pelo seguinte:  
  
-   Implantação do domínio de raiz da floresta para criar a floresta  
  
-   Implantação do primeiro controlador de domínio em cada domínio para criar os domínios necessários para a floresta  
  
-   Associações de grupos de administradores de serviço em todos os domínios da floresta  
  
-   Criação do design da estrutura de UO para cada domínio na floresta  
  
-   Delegação de autoridade administrativa para proprietários de UO  
  
-   Alterações no esquema  
  
-   Alterações na configuração de floresta  
  
-   Implementação de determinadas configurações de política de diretiva de grupo, incluindo diretivas de conta de usuário de domínio, como a diretiva de bloqueio de conta e senha refinada  
  
-   Configurações de política de negócios que se aplicam aos controladores de domínio  
  
-   Outras configurações de diretiva de grupo que são aplicadas no nível do domínio  
  
O proprietário da floresta tem autoridade sobre toda a floresta. É responsabilidade do proprietário da floresta para definir a política de grupo e políticas de negócios e selecionar os indivíduos que são administradores de serviço. O proprietário da floresta é um proprietário de serviço.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS para o proprietário do AD DS  
O DNS para o proprietário do AD DS é um indivíduo que tem uma compreensão completa da infra-estrutura DNS existente e o namespace existente da organização.  
  
O DNS para o proprietário do AD DS é responsável pelo seguinte:  
  
-   Servindo como um elo entre a equipe de design e o grupo de TI que atualmente possui a infraestrutura do DNS  
  
-   Fornecer as informações sobre o namespace DNS existente da organização para ajudar na criação do novo namespace do Active Directory  
  
-   Trabalhando com a equipe de implantação para certificar-se de que a nova infra-estrutura do DNS é implantada de acordo com as especificações da equipe de design e se ele está funcionando corretamente  
  
-   Gerenciando o DNS para a infraestrutura do AD DS, incluindo o serviço do servidor DNS e os dados DNS  
  
O DNS para o proprietário do AD DS é um proprietário de serviço.  
  
#### <a name="site-topology-owner"></a>Proprietário da topologia de site  
O proprietário da topologia de site está familiarizado com a estrutura física da rede da organização, incluindo o mapeamento de áreas de rede que estão conectadas por meio de links lentos, roteadores e sub-redes individuais. O proprietário da topologia de site é responsável pelo seguinte:  
  
-   Noções básicas sobre a topologia de rede física e como ele afeta o AD DS  
  
-   Noções básicas sobre como a implantação do Active Directory terá impacto sobre a rede  
  
-   Determinando os sites de lógica do Active Directory que precisa ser criada  
  
-   Atualizando objetos de site para controladores de domínio quando uma sub-rede é adicionada, modificado ou removido  
  
-   Criação de links de site, pontes de link de site e os objetos de conexão manual  
  
O proprietário da topologia de site é um proprietário de serviço.  
  
#### <a name="ou-owner"></a>Proprietário de UO  
O proprietário de UO é responsável por gerenciar dados armazenados no diretório. Essa pessoa precisa estar familiarizados com o operacional e as políticas de segurança que estão em vigor na rede. Os proprietários de UO podem executar somente as tarefas que foram delegadas a eles pelos administradores de serviço e eles podem executar somente as tarefas em UOs aos quais são atribuídas. Tarefas que podem ser atribuídas ao proprietário da UO incluem o seguinte:  
  
-   Executar todas as tarefas de gerenciamento de conta dentro da UO respectiva atribuído  
  
-   Gerenciando estações de trabalho e servidores de membro que são membros de sua UO atribuído  
  
-   Delegando autoridade para administradores locais dentro da UO respectiva atribuído  
  
O proprietário de UO é um proprietário de dados.  
  
## <a name="BKMK_3"></a>Equipes de projeto de construção  
As equipes de projeto do Active Directory são grupos temporários que são responsáveis por concluir tarefas de design e implantação do Active Directory. Quando o projeto de implantação do Active Directory for concluído, os proprietários assuma a responsabilidade para o diretório e as equipes de projeto podem dispensar.  
  
O tamanho das equipes de projeto varia de acordo com o tamanho da organização. Em pequenas empresas, uma única pessoa pode abranger várias áreas de responsabilidade de uma equipe de projeto e estar envolvido em mais de uma fase da implantação. Grandes organizações podem exigir que as equipes maiores com pessoas diferentes ou até mesmo diferentes que abrangem as diferentes áreas de responsabilidade. O tamanho das equipes não é importante, desde que todas as áreas de responsabilidade são atribuídas e as metas de design da organização forem atendidas.  
  
### <a name="identifying-potential-forest-owners"></a>Identificação de possíveis proprietários da floresta  
Identifica os grupos em sua organização que possui e controla os recursos necessários para fornecer serviços de diretório para os usuários na rede. Esses grupos são considerados possíveis proprietários da floresta.  
  
A separação da administração de serviço e os dados no AD DS torna possível para a infraestrutura de IT grupo (ou grupos) de uma organização para gerenciar o serviço de diretório, enquanto os administradores locais em cada grupo de gerenciam os dados que pertencem a seus próprios grupos. Possíveis proprietários da floresta tem a autoridade necessária sobre a infraestrutura de rede para implantar e oferecer suporte ao AD DS.  
  
Para organizações que têm uma infraestrutura centralizada de grupo de TI, o grupo de TI geralmente é o proprietário da floresta e, portanto, o proprietário da floresta potencial para quaisquer implantações futuras. As organizações que incluem um número de independente de infra-estrutura grupos têm um número de possíveis proprietários da floresta. Se sua organização já tiver uma infra-estrutura do Active Directory em vigor, qualquer proprietários da floresta atual também são possíveis proprietários da floresta para novas implantações.  
  
Selecione um dos possíveis proprietários de floresta para atuar como o proprietário da floresta para cada floresta que você está considerando para implantação. Essas possíveis proprietários da floresta serão responsáveis por trabalhar com a equipe de design para determinar se sua floresta será realmente implantada ou se um curso alternativo de ação (como ingressando em outra floresta existente) é um melhor uso dos recursos disponíveis e ainda atende às suas necessidades. O proprietário da floresta (ou proprietários) em sua organização são membros da equipe de design do Active Directory.  
  
### <a name="establishing-a-design-team"></a>Estabelecimento de uma equipe de design  
A equipe de design do Active Directory é responsável por coletar todas as informações necessárias para tomar decisões sobre design da estrutura lógica do Active Directory.  
  
As responsabilidades da equipe de design incluem o seguinte:  
  
-   Determinar quantas florestas e domínios são necessários e o que as relações entre os domínios e florestas  
  
-   Trabalhando com os proprietários de dados para garantir que o design atenda aos seus requisitos de segurança e administrativos  
  
-   Trabalhando com os administradores de rede atual para garantir que a infraestrutura de rede atual dá suporte a design e que o design não prejudiquem os aplicativos existentes implantados na rede  
  
-   Trabalhando com os representantes do grupo de segurança da organização para garantir que o design atende às diretivas de segurança estabelecidas  
  
-   Criando estruturas de UO que permitem o níveis apropriados de proteção e a delegação adequada de autoridade para os proprietários de dados  
  
-   Trabalhando com a equipe de implantação para o design de teste em um laboratório de ambiente para garantir que ele funcione como planejado e modificar o design como necessárias para resolver quaisquer problemas que ocorrem  
  
-   Criar um design de topologia de site que atende os requisitos de replicação da floresta evitando a sobrecarga de largura de banda disponível. Para obter mais informações sobre como projetar a topologia de site, consulte [projetar o Site de topologia para o Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Trabalhando com a equipe de implantação para garantir que o design é implementado corretamente  
  
A equipe de design inclui os seguintes membros:  
  
-   Possíveis proprietários da floresta  
  
-   Arquiteto de projeto  
  
-   Gerente de projeto  
  
-   Pessoas responsáveis por estabelecer e manter políticas de segurança na rede  
  
Durante o processo de design da estrutura lógica, a equipe de design identifica os outros proprietários. Esses indivíduos devem começar a participar do processo de design, assim que eles são identificados. Depois que o projeto de implantação é transferido para a equipe de implantação, a equipe de design é responsável por supervisionar o processo de implantação para garantir que o design é implementado corretamente. A equipe de design também faz alterações no design com base nos comentários do teste.  
  
### <a name="establishing-a-deployment-team"></a>Estabelecimento de uma equipe de implantação  
A equipe de implantação do Active Directory é responsável por testar e implementar o design da estrutura lógica do Active Directory. Isso envolve as seguintes tarefas:  
  
-   Estabelecer um ambiente de teste que suficientemente emula o ambiente de produção  
  
-   Testando o design, Implementando a estrutura proposta de floresta e domínio em um ambiente de laboratório para verificar que ele cumpra as metas de cada proprietário da função  
  
-   Desenvolver e testar os cenários de migração propostos pelo design em um ambiente de laboratório  
  
-   Certificando-se de que cada proprietário terminar o processo de teste para garantir que os recursos de design corretos estão sendo testados  
  
-   Testando a operação de implantação em um ambiente piloto  
  
Quando o design e as tarefas de teste forem concluídas, a equipe de implantação executa as seguintes tarefas:  
  
-   Cria as florestas e domínios de acordo com o design da estrutura lógica do Active Directory  
  
-   Cria os sites e objetos de link de site conforme necessário com base no design da topologia de site  
  
-   Garante que a infraestrutura do DNS está configurada para oferecer suporte ao AD DS e que os novos namespaces estão integrados no namespace existente da organização  
  
A equipe de implantação do Active Directory inclui os seguintes membros:  
  
-   Proprietário da floresta  
  
-   DNS para o proprietário do AD DS  
  
-   Proprietário da topologia de site  
  
-   Proprietários de UO  
  
A equipe de implantação funciona com os administradores de serviço e os dados durante a fase de implantação para garantir que os membros da equipe de operações estão familiarizados com o novo design. Isso ajuda a garantir uma transição suave de propriedade quando a operação de implantação é concluída. Após a conclusão do processo de implantação, a responsabilidade de manter o novo ambiente do Active Directory passa para a equipe de operações.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentar as equipes de design e implantação  
Documente os nomes e informações de contato para as pessoas que farão parte no design e implantação do AD DS. Identifica quem será responsável por cada função em que as equipes de design e implantação. Inicialmente, essa lista inclui os possíveis proprietários de floresta, o gerente de projeto e o arquiteto de projeto. Ao determinar o número de florestas que você irá implantar, talvez seja necessário criar novas equipes de design para florestas adicionais. Observe que você precisará atualizar a documentação, como alterar as associações de equipe e como identificar os vários proprietários do Active Directory durante o processo de design. Para uma planilha ajudar a documentar as equipes de design e implantação para cada floresta, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip do trabalho auxílios para Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558 ](https://go.microsoft.com/fwlink/?LinkID=102558)) e abra o "Design e implantação de informações da equipe" (DSSLOGI_1.doc).  
  


