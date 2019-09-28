---
ms.assetid: 50bd2566-e03c-4884-b5c4-895c8aab80aa
title: Identificar os participantes do projeto de implantação
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: bb5ded466a45061649585a747be74adcdc1148cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408775"
---
# <a name="identifying-the-deployment-project-participants"></a>Identificar os participantes do projeto de implantação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa ao estabelecer um projeto de implantação para o Domínio do Active Directory Service (AD DS) é estabelecer as equipes de projeto de design e implantação que serão responsáveis por gerenciar a fase de design e a fase de implantação do ciclo de projeto de Active Directory. Além disso, você deve identificar os indivíduos e grupos que serão responsáveis por possuir e manter o diretório após a conclusão da implantação.  
  
-   [Definindo funções específicas do projeto](#BKMK_1)  
  
-   [Estabelecendo proprietários e administradores](#BKMK_2)  
  
-   [Criando equipes de projeto](#BKMK_3)  
  
## <a name="BKMK_1"></a>Definindo funções específicas do projeto  
Uma etapa importante no estabelecimento das equipes de projeto é identificar os indivíduos que devem manter funções específicas do projeto. Isso inclui o patrocinador executivo, o arquiteto do projeto e o gerente de projeto. Esses indivíduos são responsáveis por executar o projeto de implantação Active Directory.  
  
Depois que você se deparar com o arquiteto de projeto e o gerente de projeto, essas pessoas estabelecerão canais de comunicação em toda a organização, criarão agendamentos de projeto e identificarão os indivíduos que serão membros das equipes do projeto, começando pelo vários proprietários.  
  
### <a name="executive-sponsor"></a>Patrocinador executivo  
Implantar uma infraestrutura como o AD DS pode ter um impacto amplo em uma organização. Por esse motivo, é importante ter um patrocinador executivo que entenda o valor comercial da implantação, dê suporte ao projeto no nível executivo e possa ajudar a resolver conflitos em toda a organização.  
  
### <a name="project-architect"></a>Arquiteto do projeto  
Cada projeto de implantação Active Directory requer um arquiteto de projeto para gerenciar o design de Active Directory e o processo de tomada de decisão de implantação. O arquiteto fornece experiência técnica para ajudar no processo de criação e implantação de AD DS.  
  
> [!NOTE]  
> Se nenhum pessoal existente em sua organização tiver experiência de design de diretório, talvez você queira contratar um consultor externo que seja especialista em design e implantação de Active Directory.  
  
As responsabilidades do arquiteto de projeto Active Directory incluem o seguinte:  
  
-   Proprietário do design de Active Directory  
  
-   Compreendendo e registrando a lógica para as principais decisões de design  
  
-   Garantir que o design atenda às necessidades de negócios da organização  
  
-   Estabelecendo consenso entre equipes de design, implantação e operações  
  
-   Noções básicas sobre as necessidades de aplicativos integrados ao AD DS  
  
O design final de Active Directory deve refletir uma combinação de metas de negócios e decisões técnicas. Portanto, o arquiteto do projeto deve examinar as decisões de design para garantir que elas se alinhem às metas de negócios.  
  
### <a name="project-manager"></a>Gerente de projeto  
O gerente de projeto facilita a cooperação entre as unidades de negócios e os grupos de gerenciamento de tecnologia. Idealmente, o Active Directory gerente de projeto de implantação é alguém de dentro da organização que está familiarizado com as políticas operacionais do grupo de ti e os requisitos de design para os grupos que estão se preparando para implantar AD DS. O gerente de projeto supervisiona todo o projeto de implantação, começando com o design e continuando a implementação e garante que o projeto permaneça em agenda e dentro do orçamento. As responsabilidades do gerente de projeto incluem o seguinte:  
  
-   Fornecendo planejamento de projeto básico, como agendamento e orçamento  
  
-   Gerando progresso no projeto de Active Directory Design e implantação  
  
-   Garantindo que os indivíduos apropriados estejam envolvidos em cada parte do processo de design  
  
-   Servindo como ponto único de contato para o projeto de implantação Active Directory  
  
-   Estabelecendo a comunicação entre equipes de design, implantação e operações  
  
-   Estabelecendo e mantendo a comunicação com o patrocinador executivo em todo o projeto de implantação  
  
## <a name="BKMK_2"></a>Estabelecendo proprietários e administradores  
Em um projeto de implantação Active Directory, as pessoas que são proprietários são responsáveis pelo gerenciamento para garantir que as tarefas de implantação sejam concluídas e que Active Directory especificações de design atendam às necessidades da organização. Os proprietários não têm necessariamente acesso ou manipulam a infraestrutura de diretório diretamente. Os administradores são os indivíduos responsáveis por concluir as tarefas de implantação necessárias. Os administradores têm o acesso à rede e as permissões necessárias para manipular o diretório e sua infraestrutura.  
  
A função do proprietário é estratégica e gerencial. Os proprietários são responsáveis por se comunicar com os administradores as tarefas necessárias para a implementação do design de Active Directory, como a criação de novos controladores de domínio dentro da floresta. Os administradores são responsáveis por implementar o design na rede de acordo com as especificações de design.  
  
Em grandes organizações, diferentes indivíduos preenchem as funções Owner e Administrator; no entanto, em algumas pequenas organizações, o mesmo indivíduo pode atuar como o proprietário e o administrador.  
  
### <a name="service-and-data-owners"></a>Proprietários de serviços e dados  
O gerenciamento de AD DS diariamente envolve dois tipos de proprietários:  
  
-   Proprietários de serviço responsáveis pelo planejamento e manutenção de longo prazo da infraestrutura de Active Directory e para garantir que o diretório continue a funcionar e que as metas estabelecidas nos contratos de nível de serviço sejam mantidas  
  
-   Proprietários de dados que são responsáveis pela manutenção das informações armazenadas no diretório. Isso inclui gerenciamento de conta de usuário e computador e gerenciamento de recursos locais, como servidores membro e estações de trabalho.  
  
É importante identificar o serviço de Active Directory e os proprietários de dados antecipadamente para que eles possam participar tanto do processo de design quanto possível. Como os proprietários de serviço e de dados são responsáveis pela manutenção de longo prazo do diretório após a conclusão do projeto de implantação, é importante que esses indivíduos forneçam informações sobre as necessidades organizacionais e estejam familiarizados com como e por que certas decisões de design são feitas. Os proprietários de serviço incluem o proprietário da floresta, o proprietário do sistema de nomenclatura de Domínio do Active Directory (DNS) e o proprietário da topologia do site. Os proprietários de dados incluem proprietários da UO (unidade organizacional).  
  
### <a name="service-and-data-administrators"></a>Administradores de serviços e dados  
A operação do AD DS envolve dois tipos de administradores: administradores de serviço e administradores de dados. Os administradores de serviço implementam decisões de política feitas por proprietários de serviço e lidam com as tarefas cotidianas associadas à manutenção do serviço de diretório e da infraestrutura. Isso inclui o gerenciamento de controladores de domínio que hospedam o serviço de diretório, o gerenciamento de outros serviços de rede, como o DNS, necessário para AD DS, o controle da configuração de configurações de toda a floresta e a garantia de que o diretório seja sempre Há.  
  
Os administradores de serviço também são responsáveis por concluir tarefas de implantação de Active Directory em andamento que são necessárias após a conclusão do processo inicial de implantação do Windows Server 2008 Active Directory. Por exemplo, conforme as demandas no diretório aumentam, os administradores de serviço criam controladores de domínio adicionais e estabelecem ou removem relações de confiança entre domínios, conforme necessário. Por esse motivo, a equipe de implantação Active Directory precisa incluir administradores de serviço.  
  
Você deve ter cuidado para atribuir funções de administrador de serviço somente a indivíduos confiáveis na organização. Como esses indivíduos têm a capacidade de modificar os arquivos do sistema em controladores de domínio, eles podem alterar o comportamento de AD DS. Você deve garantir que os administradores de serviço em sua organização sejam indivíduos que estão familiarizados com as políticas operacionais e de segurança que estão em vigor em sua rede e que entendem a necessidade de impor essas políticas.  
  
Os administradores de dados são usuários em um domínio que são responsáveis por manter os dados armazenados em AD DS, como contas de usuário e de grupo, e para manter os computadores que são membros de seu domínio. Os administradores de dados controlam subconjuntos de objetos no diretório e não têm controle sobre a instalação ou a configuração do serviço de diretório.  
  
As contas de administrador de dados não são fornecidas por padrão. Depois que a equipe de design determina como os recursos devem ser gerenciados para a organização, os proprietários de domínio devem criar contas de administrador de dados e delegar a elas as permissões apropriadas com base no conjunto de objetos para os quais os administradores serão responsáveis .  
  
É melhor limitar o número de administradores de serviço em sua organização ao número mínimo necessário para garantir que a infraestrutura continue a funcionar. A maior parte do trabalho administrativo pode ser concluída pelos administradores de dados. Os administradores de serviço exigem um conjunto de habilidades muito mais amplo porque são responsáveis por manter o diretório e a infraestrutura que oferece suporte a ele. Os administradores de dados exigem apenas as habilidades necessárias para gerenciar sua parte do diretório. Dividir as atribuições de trabalho dessa forma resulta em economia de custos para a organização porque apenas um pequeno número de administradores precisa ser treinado para operar e manter o diretório inteiro e sua infraestrutura.  
  
Por exemplo, um administrador de serviços precisa entender como adicionar um domínio a uma floresta. Isso inclui como instalar o software para converter um servidor em um controlador de domínio e como manipular o ambiente DNS para que o controlador de domínio possa ser mesclado diretamente no ambiente de Active Directory. Um administrador de dados só precisa saber como gerenciar os dados específicos para os quais eles são responsáveis, como a criação de novas contas de usuário para novos funcionários em seu departamento.  
  
A implantação de AD DS requer coordenação e comunicação entre vários grupos diferentes envolvidos na operação da infraestrutura de rede. Esses grupos devem indicar aos proprietários de serviço e de dados que são responsáveis por representar os vários grupos durante o processo de design e implantação.  
  
Depois que o projeto de implantação for concluído, esses proprietários de serviço e de dados continuarão sendo responsáveis pela parte da infraestrutura gerenciada por seu grupo. Em um ambiente Active Directory, esses proprietários são o proprietário da floresta, o DNS para AD DS proprietário, o proprietário da topologia do site e o proprietário da UO. As funções desses serviços e proprietários de dados são explicadas nas seções a seguir.  
  
#### <a name="forest-owner"></a>Proprietário da floresta  
O proprietário da floresta é normalmente um gerente de ti (tecnologia da informação) sênior na organização responsável pelo processo de implantação de Active Directory e que, por fim, se responsabiliza por manter a entrega de serviço dentro da floresta após a a implantação foi concluída. O proprietário da floresta atribui indivíduos para preencher as outras funções de propriedade identificando a equipe-chave na organização que é capaz de contribuir com as informações necessárias sobre a infraestrutura de rede e as necessidades administrativas. O proprietário da floresta é responsável pelo seguinte:  
  
-   Implantação do domínio raiz da floresta para criar a floresta  
  
-   Implantação do primeiro controlador de domínio em cada domínio para criar os domínios necessários para a floresta  
  
-   Associações dos grupos de administradores de serviço em todos os domínios da floresta  
  
-   Criação do design da estrutura da UO para cada domínio na floresta  
  
-   Delegação de autoridade administrativa para proprietários da UO  
  
-   Alterações no esquema  
  
-   Alterações nas definições de configuração de toda a floresta  
  
-   Implementação de determinadas configurações de política de Política de Grupo, incluindo políticas de conta de usuário de domínio, como política refinada de bloqueio de conta e senha  
  
-   Configurações de política de negócios que se aplicam a controladores de domínio  
  
-   Quaisquer outras configurações de Política de Grupo que são aplicadas no nível de domínio  
  
O proprietário da floresta tem autoridade sobre toda a floresta. É responsabilidade do proprietário da floresta definir Política de Grupo e políticas de negócios e selecionar os indivíduos que são administradores de serviços. O proprietário da floresta é um proprietário de serviço.  
  
#### <a name="dns-for-ad-ds-owner"></a>DNS para AD DS proprietário  
O DNS para AD DS proprietário é um indivíduo que tem uma compreensão completa da infraestrutura de DNS existente e do namespace existente da organização.  
  
O DNS para AD DS proprietário é responsável pelo seguinte:  
  
-   Servindo como uma ligação entre a equipe de design e o grupo de ti que atualmente possui a infraestrutura de DNS  
  
-   Fornecendo as informações sobre o namespace DNS existente da organização para auxiliar na criação do novo namespace de Active Directory  
  
-   Trabalhando com a equipe de implantação para garantir que a nova infraestrutura de DNS seja implantada de acordo com as especificações da equipe de design e que ela esteja funcionando corretamente  
  
-   Gerenciando o DNS para AD DS infraestrutura, incluindo o serviço do servidor DNS e os dados DNS  
  
O DNS para AD DS proprietário é um proprietário de serviço.  
  
#### <a name="site-topology-owner"></a>Proprietário da topologia de site  
O proprietário da topologia de site está familiarizado com a estrutura física da rede da organização, incluindo o mapeamento de sub-redes individuais, roteadores e áreas de rede que estão conectados por meio de links lentos. O proprietário da topologia do site é responsável pelo seguinte:  
  
-   Noções básicas sobre a topologia de rede física e como ela afeta AD DS  
  
-   Noções básicas sobre como a implantação de Active Directory afetará a rede  
  
-   Determinando o Active Directory sites lógicos que precisam ser criados  
  
-   Atualizando objetos do site para controladores de domínio quando uma sub-rede é adicionada, modificada ou removida  
  
-   Criando links de site, pontes de link de site e objetos de conexão manual  
  
O proprietário da topologia do site é um proprietário do serviço.  
  
#### <a name="ou-owner"></a>Proprietário da UO  
O proprietário da UO é responsável por gerenciar os dados armazenados no diretório. Essa pessoa precisa estar familiarizada com as políticas operacionais e de segurança que estão em vigor na rede. Os proprietários da UO podem executar apenas as tarefas que foram delegadas a eles pelos administradores de serviço e podem executar somente as tarefas nas UOs às quais elas são atribuídas. As tarefas que podem ser atribuídas ao proprietário da UO incluem o seguinte:  
  
-   Executando todas as tarefas de gerenciamento de conta em sua UO atribuída  
  
-   Gerenciando estações de trabalho e servidores membro que são membros de sua UO atribuída  
  
-   Delegando autoridade a administradores locais em sua UO atribuída  
  
O proprietário da UO é um proprietário de dados.  
  
## <a name="BKMK_3"></a>Criando equipes de projeto  
Active Directory equipes de projeto são grupos temporários que são responsáveis por concluir Active Directory tarefas de design e implantação. Quando o projeto de implantação de Active Directory é concluído, os proprietários assumem a responsabilidade pelo diretório e as equipes de projeto podem disbandr.  
  
O tamanho das equipes do projeto varia de acordo com o tamanho da organização. Em pequenas organizações, uma única pessoa pode cobrir várias áreas de responsabilidade em uma equipe de projeto e estar envolvida em mais de uma fase da implantação. Organizações de grande porte podem exigir equipes maiores com indivíduos diferentes ou até mesmo equipes diferentes que abrangem diferentes áreas de responsabilidade. O tamanho das equipes não é importante, desde que todas as áreas de responsabilidade sejam atribuídas e que as metas de design da organização sejam atendidas.  
  
### <a name="identifying-potential-forest-owners"></a>Identificando possíveis proprietários de floresta  
Identifique os grupos dentro da sua organização que possuem e controle os recursos necessários para fornecer serviços de diretório aos usuários na rede. Esses grupos são considerados potenciais proprietários de floresta.  
  
A separação do serviço e da administração de dados no AD DS possibilita que o grupo de ti de infraestrutura (ou grupos) de uma organização gerencie o serviço de diretório enquanto os administradores locais em cada grupo gerenciam os dados que pertencem a seus próprios grupos. Os possíveis proprietários de floresta têm a autoridade necessária sobre a infraestrutura de rede para implantar e dar suporte a AD DS.  
  
Para organizações que têm um grupo de infraestrutura de ti centralizado, o grupo de ti é geralmente o proprietário da floresta e, portanto, o proprietário potencial da floresta para qualquer implantação futura. As organizações que incluem vários grupos de ti de infraestrutura independentes têm vários proprietários de floresta em potencial. Se sua organização já tiver uma infraestrutura de Active Directory em vigor, todos os proprietários de floresta atuais também serão proprietários potenciais de floresta para novas implantações.  
  
Selecione um dos possíveis proprietários da floresta para atuar como o proprietário da floresta para cada floresta que você está considerando para a implantação. Esses proprietários potenciais da floresta são responsáveis por trabalhar com a equipe de design para determinar se sua floresta será ou não implantada na verdade ou se um curso alternativo de ação (como ingressar em outra floresta existente) é um melhor uso dos recursos disponíveis e ainda atende às suas necessidades. O proprietário da floresta (ou proprietários) em sua organização são membros da equipe de design de Active Directory.  
  
### <a name="establishing-a-design-team"></a>Estabelecendo uma equipe de design  
A equipe de design de Active Directory é responsável por reunir todas as informações necessárias para tomar decisões sobre o design de estrutura lógica Active Directory.  
  
As responsabilidades da equipe de design incluem o seguinte:  
  
-   Determinando quantas florestas e domínios são necessários e quais são as relações entre as florestas e os domínios  
  
-   Trabalhando com proprietários de dados para garantir que o design atenda aos seus requisitos administrativos e de segurança  
  
-   Trabalhar com os administradores de rede atuais para garantir que a infraestrutura de rede atual ofereça suporte ao design e que o design não afete negativamente os aplicativos existentes implantados na rede  
  
-   Trabalhando com representantes do grupo de segurança da organização para garantir que o design atenda às políticas de segurança estabelecidas  
  
-   Criando estruturas de UO que permitem níveis apropriados de proteção e a delegação adequada de autoridade para os proprietários dos dados  
  
-   Trabalhar com a equipe de implantação para testar o design em um ambiente de laboratório para garantir que ele funcione como planejado e modificando o design conforme necessário para resolver quaisquer problemas que ocorram  
  
-   Criar um design de topologia de site que atenda aos requisitos de replicação da floresta, impedindo a sobrecarga da largura de banda disponível. Para obter mais informações sobre como criar a topologia do site, consulte [projetando a topologia do site para o Windows Server 2008 AD DS](https://technet.microsoft.com/library/cc772013.aspx).  
  
-   Trabalhando com a equipe de implantação para garantir que o design seja implementado corretamente  
  
A equipe de design inclui os seguintes membros:  
  
-   Possíveis proprietários de floresta  
  
-   Arquiteto do projeto  
  
-   Gerente de projeto  
  
-   Indivíduos responsáveis por estabelecer e manter políticas de segurança na rede  
  
Durante o processo de design da estrutura lógica, a equipe de design identifica os outros proprietários. Esses indivíduos devem começar a participar do processo de design assim que forem identificados. Depois que o projeto de implantação é enviado para a equipe de implantação, a equipe de design é responsável por supervisionar o processo de implantação para garantir que o design seja implementado corretamente. A equipe de design também faz alterações no design com base nos comentários dos testes.  
  
### <a name="establishing-a-deployment-team"></a>Estabelecendo uma equipe de implantação  
A equipe de implantação Active Directory é responsável por testar e implementar o design de estrutura lógica Active Directory. Isso envolve as seguintes tarefas:  
  
-   Estabelecendo um ambiente de teste que emula suficientemente o ambiente de produção  
  
-   Testando o design implementando a floresta e a estrutura de domínio propostas em um ambiente de laboratório para verificar se ele atende às metas de cada proprietário de função  
  
-   Desenvolvendo e testando quaisquer cenários de migração propostos pelo design em um ambiente de laboratório  
  
-   Certificando-se de que cada proprietário desligou o processo de teste para garantir que os recursos de design corretos estejam sendo testados  
  
-   Testando a operação de implantação em um ambiente piloto  
  
Quando as tarefas de design e teste forem concluídas, a equipe de implantação executará as seguintes tarefas:  
  
-   Cria as florestas e domínios de acordo com o design da estrutura lógica Active Directory  
  
-   Cria os sites e objetos de link de site conforme necessário com base no design da topologia do site  
  
-   Garante que a infraestrutura DNS esteja configurada para dar suporte a AD DS e que quaisquer namespaces novos sejam integrados ao namespace existente da organização  
  
A equipe de implantação do Active Directory inclui os seguintes membros:  
  
-   Proprietário da floresta  
  
-   DNS para AD DS proprietário  
  
-   Proprietário da topologia de site  
  
-   Proprietários da UO  
  
A equipe de implantação trabalha com os administradores de serviço e de dados durante a fase de implantação para garantir que os membros da equipe de operações estejam familiarizados com o novo design. Isso ajuda a garantir uma transição tranqüila de propriedade quando a operação de implantação é concluída. Na conclusão do processo de implantação, a responsabilidade de manter o novo ambiente de Active Directory passa para a equipe de operações.  
  
### <a name="documenting-the-design-and-deployment-teams"></a>Documentando as equipes de design e implantação  
Documente os nomes e as informações de contato das pessoas que farão parte do design e da implantação de AD DS. Identifique quem será responsável por cada função nas equipes de design e implantação. Inicialmente, essa lista inclui os possíveis proprietários da floresta, o gerente de projeto e o arquiteto do projeto. Ao determinar o número de florestas que serão implantadas, talvez seja necessário criar novas equipes de design para florestas adicionais. Observe que você precisará atualizar sua documentação à medida que as associações da equipe forem alteradas e ao identificar os vários proprietários do Active Directory durante o processo de design. Para uma planilha para ajudá-lo a documentar as equipes de design e implantação para cada floresta, baixe o Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip de ajudas de trabalho para o Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e abra " Informações da equipe de design e implantação "(DSSLOGI_1. doc).  
  


