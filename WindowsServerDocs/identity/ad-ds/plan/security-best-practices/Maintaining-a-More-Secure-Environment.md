---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: Manter um ambiente mais seguro
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d471ee2ba73ef7425464e1aa928c75318f7dd82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="maintaining-a-more-secure-environment"></a>Manter um ambiente mais seguro

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Lei número dez: tecnologia não é uma solução completa.* - [10 leis imutáveis de administração de segurança](https://technet.microsoft.com/library/cc722488.aspx)  
  
Quando você tiver criado um ambiente gerenciável e seguro para os seus ativos essenciais, o foco deve alternar para garantir que ele seja mantido com segurança. Embora você tem controles technical específicos para aumentar a segurança de suas instalações do AD DS, a tecnologia sozinha não proteger um ambiente no qual ele não funciona em parceria com empresas para manter uma infraestrutura segura e utilizável. As recomendações de alto níveis nesta seção devem ser usados como diretrizes que você pode usar para desenvolver não só de segurança efetiva, mas o gerenciamento de ciclo de vida eficaz.  
  
Em alguns casos, a organização de TI talvez tenha uma relação de trabalho com unidades de negócios, o que facilitará a implementar essas recomendações. Em organizações na qual está e unidades de negócios não são estreitamente vinculadas, talvez você precise obter primeiro patrocínio executivo para esforços para estabelecer uma relação mais próxima entre IT e unidades de negócios. O [resumo executivo](../../../ad-ds/manage/component-updates/Executive-Summary.md) se destina a ser útil como um documento autônomo para análise executiva e ele pode ser espalhado para tomadores de decisão em sua organização.  
  
## <a name="creating-business-centric-security-practices-for-active-directory"></a>Criando as práticas de segurança centradas em empresas para o Active Directory  
No passado, tecnologia da informação em muitas organizações era Vista como uma estrutura de suporte e um centro de custo. Os departamentos de TI geralmente amplamente foram separados do usuários de negócios e interações limitadas a um modelo de resposta da solicitação em que as empresas solicitado recursos e IT respondida.  
  
Como tecnologia tem evoluíram e proliferaram, a visão da "um computador em cada área de trabalho" tem efetivamente vêm para passar para a maior parte do mundo e até mesmo foi ocultaram por ampla variedade de tecnologias facilmente acessíveis disponíveis atualmente. Tecnologia da informação não é uma função de suporte, é uma função de negócios principal. Se sua organização não pode continuar a funcionar se todos os serviços de TI não estavam disponíveis, negócios da sua organização é, pelo menos em parte, tecnologia da informação.  
  
Para criar planos de recuperação de comprometimento eficaz, os serviços de TI devem trabalham em conjunto com unidades de negócios em sua organização para identificar não apenas os componentes mais críticos do cenário de TI, mas as funções essenciais necessárias aos negócios. Identificando o que é importante para sua organização como um todo, você pode focalizar protegendo os componentes que têm o maior valor. Isso não é uma recomendação para shirk a segurança de dados e sistemas de valor baixo. Em vez disso, como definir níveis de serviço para a atividade do sistema, você deve considerar definindo níveis de segurança controle e monitoramento com base na importância do ativo.  
  
Quando você investiram na criação de um ambiente atual, seguro e gerenciável, você pode mudar o foco para gerenciá-lo efetivamente e garantindo que você tenha processos de gerenciamento de ciclo de vida efetivos que não são determinados apenas pelos IT, mas pela empresa. Para conseguir isso, você precisa não só para uma parceria com a empresa, mas para investir os negócios "propriedade" de dados e sistemas no Active Directory.  
  
Quando sistemas e dados forem introduzidos ao Active Directory sem proprietários designados, empresários e proprietários IT, não há nenhuma cadeia clara de responsabilidade para o provisionamento, gerenciamento, monitoramento, atualizando e encerramento, por fim, o sistema. Isso resulta em infraestruturas nos quais sistemas expõem a organização a risco mas não podem ser encerrados porque a propriedade não for clara. Para efetivamente gerenciar o ciclo de vida dos usuários, dados, aplicativos e sistemas gerenciados pela instalação do Active Directory, você deve seguir os princípios descritos nesta seção.  
  
### <a name="assign-a-business-owner-to-active-directory-data"></a>Atribuir um proprietário de negócios para dados do Active Directory  
Dados no Active Directory devem ter um proprietário de negócios identificados, ou seja, um departamento especificado ou o usuário que é o ponto de contato para decisões sobre o ciclo de vida do ativo. Em alguns casos, o proprietário da empresa de um componente do Active Directory será um departamento de TI ou o usuário. Componentes de infraestrutura, como controladores de domínio, servidores DHCP e DNS e Active Directory provavelmente "serão" por IT. Para dados que são adicionados ao AD DS para dar suporte aos negócios (por exemplo, novos funcionários, novos aplicativos e novos repositórios de informação), uma empresa designada unidade ou usuário deve ser associado os dados.  
  
Se você usa o Active Directory para a propriedade registro de dados no diretório ou se você implementar um banco de dados separado para controlar os ativos de TI, nenhuma conta de usuário deve ser criada, não há servidor ou estação de trabalho deve ser instalada e nenhum aplicativo deve ser implantado sem um proprietário designado de registro. Tentar estabelecer a propriedade de sistemas depois que tiver sido implantados em produção pode ser um desafio na melhor das hipóteses e impossível em alguns casos. Portanto, a propriedade deve ser estabelecida no momento em que os dados são introduzidos no Active Directory.  
  
### <a name="implement-business-driven-lifecycle-management"></a>Implementar o gerenciamento de ciclo de vida controlado por empresas  
Gerenciamento do ciclo deve ser implementado para todos os dados no Active Directory. Por exemplo, quando um novo aplicativo é apresentado em um domínio do Active Directory, proprietário da empresa do aplicativo deve, em intervalos regulares, esperado para atestar o uso contínuo do aplicativo. Quando uma nova versão de um aplicativo é lançada, o proprietário da empresa do aplicativo deve ser informado e deve decidir se e quando a nova versão será implementada.  
  
Se um proprietário da empresa optar por não aprovar a implantação de uma nova versão de um aplicativo, esse proprietário da empresa também deve ser notificado de data quando a versão atual não terão suporte e deve ser responsável por determinar se o aplicativo será encerrado ou substituído. Manter os aplicativos herdados em execução e sem suporte não deve ser uma opção.  
  
Quando contas de usuário são criadas no Active Directory, seus gerentes de registro devem ser notificados na criação de objetos e necessária para confirmar a validade da conta em intervalos regulares. Implementando uma empresa controlado por ciclo de vida e o Atestado normal de validade dos dados, as pessoas que são melhor preparadas para identificar anomalias dos dados são as pessoas que examinar os dados.  
  
Por exemplo, os invasores podem criar contas de usuário que parecem ser contas válidas, seguindo as convenções de nomenclatura e o posicionamento do objeto de sua organização. Para detectar essas criações de conta, você pode implementar uma tarefa diária que retorna todos os objetos de usuário sem um proprietário da empresa designado para que você possa investigar as contas. Se os invasores criarem contas e atribuir um proprietário de negócios, implementando uma tarefa que relata a criação de objetos de novo para o proprietário da empresa designado, o proprietário da empresa pode identificar rapidamente se a conta é legítima.  
  
Você deve implementar abordagens semelhantes para grupos de segurança e distribuição. Embora alguns grupos podem estar funcional grupos criados por IT, através da criação de cada grupo com um proprietário designado, você pode recuperar todos os grupos pertencentes a um usuário designado e exigir que o usuário confirmar a validade de seus membros. Assim como a abordagem obtida com a criação de conta de usuário, você pode disparar relatório modificações de grupo para o proprietário da empresa designado. A rotina mais torna-se de um proprietário de negócios atestar a validade ou invalidade de dados no Active Directory, mais equipado que são identificar anomalias que podem indicar falhas de processo ou comprometimento real.  
  
### <a name="classify-all-active-directory-data"></a>Classificar todos os dados do Active Directory  
Além de registrar um proprietário de negócios para todos os dados do Active Directory no momento em que ele é adicionado à pasta, você também deve exigir empresários fornecer classificação para os dados. Por exemplo, se um aplicativo armazenar dados essenciais, o proprietário da empresa deve rotular dessa forma, o aplicativo de acordo com a infraestrutura de classificação da sua organização.  
  
Algumas organizações implementam políticas de classificação de dados dados rótulo acordo com os danos que estará sujeito a exposição dos dados se ele fosse roubado ou exposto. Outras empresas implementam esses dados de rótulos classificação de dados por nível de importância, requisitos de acesso e retenção. Independentemente do modelo de classificação de dados em uso em sua organização, você deve garantir que você pode aplicar classificação para dados do Active Directory, não apenas para dados de "file". Se uma conta do usuário é uma conta VIP, devem ser identificado em seu banco de dados de classificação de ativo (se você implementar isso por meio do uso de atributos nos objetos no AD DS, ou se você implantar bancos de dados de classificação de ativo separado).  
  
Dentro de seu modelo de classificação de dados, você deve incluir a classificação dos dados do AD DS, como a seguir.  
  
### <a name="systems"></a>Sistemas  
Você não deve classificar apenas dados, mas também seus população de servidor. Para cada servidor, você deve saber qual sistema operacional é instalado, quais funções gerais fornece o servidor, quais aplicativos são executados no servidor, o proprietário IT de registro e o proprietário da empresa de registro, quando aplicável. Para todos os dados ou aplicativos em execução no servidor, você deve exigir a classificação e o servidor deve ser protegido de acordo com os requisitos para as cargas de trabalho que ele dá suporte e as classificações aplicadas ao sistema e dados. Você também pode agrupar servidores pela classificação das cargas de trabalho, que permite identificar rapidamente os servidores que devem ser monitorados melhor e mais severamente configurado.  
  
### <a name="applications"></a>Aplicativos  
Você deve classificar aplicativos pela funcionalidade (o que eles fazem), base de usuários (que usa os aplicativos) e o sistema operacional no qual eles são executados. Você deve manter registros que contêm informações de versão, status de patch e outras informações pertinentes. Você também deve classificar aplicativos pelos tipos de dados manipular, conforme descrito anteriormente.  
  
### <a name="users"></a>Usuários  
Se você chamá-los aos usuários "VIP", contas críticas, ou use um rótulo diferente, as contas em suas instalações do Active Directory que têm mais probabilidade de ser atacado por invasores devem ser marcadas e monitoradas. Na maioria das organizações, simplesmente não é possível monitorar todas as atividades de todos os usuários. No entanto, se você pode identificar as contas críticas na sua instalação do Active Directory, você pode monitorar essas contas para alterações conforme descrito anteriormente neste documento.  
  
Você também pode começar a criar um banco de dados de "comportamentos esperados" para essas contas como auditar as contas. Por exemplo, se você achar que um determinado executivo usa sua estação de trabalho segura para acessar dados essenciais do seu escritório e em sua casa, mas raramente de outros locais, se você vir tentativas de acessar dados usando sua conta de um computador não autorizado ou um local até a metade em todo o planeta onde você sabe que o executivo não está localizado no momento, você pode mais rapidamente identificar e investigar esse comportamento anômalos.  
  
Integrando informações de negócios com sua infraestrutura, você pode usar essas informações de negócios para ajudá-lo a identificar falsos positivos. Por exemplo, se viagens executiva é gravada em um calendário que sejam acessível para a equipe de TI responsável pelo monitoramento do ambiente, você pode correlacionar tentativas de conexão com locais conhecidos os executivos.  
  
Digamos que executivo A normalmente está localizado em Chicago e usa uma estação de trabalho segura para acessar dados essenciais de sua mesa, e um evento é disparado por uma falha na tentativa de acessar os dados de uma estação de trabalho inseguro localizada em Atlanta. Se você pode verificar se o executivo está sendo Atlanta, você pode resolver o evento contatando o executivo ou o Assistente do executivo para determinar se a falha de acesso foi o resultado do executivo esquecer de usar a estação de trabalho segura para acessar os dados. Criando um programa que usa as abordagens descritas [planejamento de comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), você pode começar a criar um banco de dados de comportamentos esperados para contas mais "importante" na sua instalação do Active Directory que podem potencialmente ajudá-lo mais rapidamente detectar e respondem a ataques.  
  


