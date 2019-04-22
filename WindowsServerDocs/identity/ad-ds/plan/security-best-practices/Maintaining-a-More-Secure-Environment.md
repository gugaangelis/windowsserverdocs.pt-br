---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: Manter um ambiente mais seguro
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 554f841473d79edbac19ce4c53e99f5a43fa2b23
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821697"
---
# <a name="maintaining-a-more-secure-environment"></a>Manter um ambiente mais seguro

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Lei número dez: Tecnologia não é uma panaceia.* - [10 leis imutáveis da segurança de administração](https://technet.microsoft.com/library/cc722488.aspx)  
  
Quando você tiver criado um ambiente gerenciável e seguro para seus ativos críticos de negócios, o foco deve mudar para garantindo que ele seja mantido com segurança. Embora você tem controles técnicos específicos para aumentar a segurança de suas instalações do AD DS, a tecnologia sozinha não protegerá um ambiente no qual ele não funciona em parceria com a empresa para manter uma infraestrutura segura e utilizável. As recomendações de alto níveis nesta seção destinam-se a ser usado como diretrizes que você pode usar para desenvolver não apenas a segurança em vigor, mas o gerenciamento de ciclo de vida em vigor.  
  
Em alguns casos, sua organização de TI talvez já tenha uma relação de trabalho com unidades de negócios, o que facilitará a implementação dessas recomendações. Em organizações no qual ele e unidades de negócios não estão intimamente ligadas, talvez você precise obter primeiro patrocínio executivo para esforços para forjar uma relação mais estreita entre IT e unidades de negócios. O [resumo executivo](../../../ad-ds/manage/component-updates/Executive-Summary.md) se destina a ser útil como um documento autônomo para revisão executivo e ele pode ser disseminado para os tomadores de decisão em sua organização.  
  
## <a name="creating-business-centric-security-practices-for-active-directory"></a>Criação de práticas recomendadas de segurança centrados no negócio para o Active Directory  
No passado, a tecnologia da informação em várias organizações foi vista como uma estrutura de suporte e um centro de custo. Os departamentos de TI geralmente amplamente foram separados de usuários de negócios e interações limitadas a um modelo de solicitação-resposta em que os negócios recursos solicitados e IT respondida.  
  
Como a tecnologia evoluiu e proliferam, a visão de "um computador em cada área de trabalho" tem vêm com eficiência para passar para a maior parte do mundo e até mesmo foi eclipsada pela ampla gama de facilmente acessíveis tecnologias disponíveis hoje. Tecnologia da informação não é uma função de suporte, é uma função comercial principal. Se sua organização não pôde continuar a funcionar se todos os serviços de TI não estavam disponíveis, os negócios da organização é, pelo menos em parte, tecnologia da informação.  
  
Para criar planos de recuperação de comprometimento em vigor, os serviços de TI devem trabalhar de perto com unidades de negócios em sua organização para identificar não apenas os componentes mais críticos do cenário de TI, mas as funções críticas exigidas pelos negócios. Identificando o que é importante para sua organização como um todo, você pode se concentrar em proteger os componentes que têm o maior valor. Isso não é uma recomendação para shirk a segurança dos dados e sistemas de valor baixo. Em vez disso, como definir níveis de serviço de tempo de atividade do sistema, você deve considerar definir níveis de controle de segurança e monitoramento com base no nível de importância do ativo.  
  
Quando você tiver investido na criação de um ambiente atual, seguro e gerenciável, você pode mudar o foco para gerenciá-los com eficiência e garantir que você tenha os processos de gerenciamento eficaz do ciclo de vida que não são determinados somente pelo IT, mas a empresa. Para fazer isso, você precisa não apenas para parceiros com a empresa, mas investir o negócio de "propriedade" dos dados e sistemas no Active Directory.  
  
Quando os sistemas e dados introduzidos no Active Directory sem proprietários designados, os proprietários de negócios e IT, não há nenhuma cadeia clara de responsabilidade para o provisionamento, gerenciamento, monitoramento, atualização e encerramento, eventualmente, o sistema. Isso resulta em infraestruturas em que sistemas de expõem a organização a riscos, mas não podem ser encerrados porque a propriedade não está clara. Para gerenciar com eficácia o ciclo de vida de usuários, dados, aplicativos e sistemas gerenciados pela instalação do Active Directory, você deve seguir os princípios descritos nesta seção.  
  
### <a name="assign-a-business-owner-to-active-directory-data"></a>Atribuir um proprietário de negócios a dados do Active Directory  
Dados no Active Directory devem ter um proprietário de negócios identificados, ou seja, um departamento especificado ou o usuário que é o ponto de contato para decisões sobre o ciclo de vida do ativo. Em alguns casos, o proprietário da empresa de um componente do Active Directory será um usuário ou o departamento de TI. Componentes de infra-estrutura como controladores de domínio, servidores DHCP e DNS e Active Directory serão provavelmente ser "pertencem" IT. Para dados que são adicionados ao AD DS para dar suporte aos negócios (por exemplo, novos funcionários, novos aplicativos e novos repositórios de informações), uma empresa designada de unidade ou o usuário deve ser associado aos dados.  
  
Se você usa o Active Directory para a propriedade registro de dados no diretório, ou se você implementar um banco de dados separado para acompanhar os ativos de TI, nenhuma conta de usuário deve ser criada, nenhum servidor ou estação de trabalho deve ser instalada e nenhum aplicativo deve ser implantado sem um proprietário designado do registro. Tentando estabelecer a propriedade dos sistemas depois que eles já foram implantados em produção pode ser um desafio na melhor das hipóteses e é impossível em alguns casos. Portanto, a propriedade deve ser estabelecida no momento em que os dados são introduzidos no Active Directory.  
  
### <a name="implement-business-driven-lifecycle-management"></a>Implementar o gerenciamento de ciclo de vida orientados pelos negócios  
Gerenciamento de ciclo de vida deve ser implementado para todos os dados no Active Directory. Por exemplo, quando um novo aplicativo é introduzido em um domínio do Active Directory, o proprietário de negócios do aplicativo, em intervalos regulares, deve atestar o uso contínuo do aplicativo. Quando uma nova versão de um aplicativo for lançada, o proprietário de negócios do aplicativo deve ser informado e deve decidir se e quando a nova versão será implementada.  
  
Se um proprietário da empresa optar por não aprovar a implantação de uma nova versão de um aplicativo, o proprietário de negócios também deve ser notificado da data quando a versão atual não terão suporte e deve ser responsável por determinar se o aplicativo será ser desativados ou substituídos. Manter os aplicativos herdados em execução e sem suporte não deve ser uma opção.  
  
Quando as contas de usuário são criadas no Active Directory, seus gerentes de registro devem ser notificados na criação do objeto e necessárias para atestar a validade da conta em intervalos regulares. Implementando um negócio controlado por ciclo de vida e Atestado regular da validade dos dados, as pessoas que são mais bem equipadas para identificar anomalias nos dados são as pessoas que examinar os dados.  
  
Por exemplo, os invasores podem criar contas de usuário que parecem ser contas válidas, seguindo as convenções de nomenclatura da sua organização e o posicionamento de objeto. Para detectar esses criações de contas, você pode implementar uma tarefa diária que retorna todos os objetos de usuário sem um proprietário de negócios designado para que você possa investigar as contas. Se os invasores criam contas e atribuir um proprietário de negócios, com a implementação de uma tarefa que relata a criação do novo objeto para o proprietário de negócios designado, o proprietário da empresa pode identificar rapidamente se a conta é legítima.  
  
Você deve implementar abordagens semelhantes aos grupos de segurança e de distribuição. Embora alguns grupos podem ser grupos funcionais criados pelo IT, com a criação de cada grupo com um proprietário designado, você pode recuperar todos os grupos pertencentes a um usuário designado e exigir que o usuário atestar a validade de suas associações. Como a abordagem usada com a criação de conta de usuário, você pode disparar reporting modificações de grupo para o proprietário de negócios designados. A rotina mais torna-se de um proprietário de negócios atestar a validade ou invalidade dos dados no Active Directory, mais bem equipados que são identificar anomalias que podem indicar falhas do processo ou o comprometimento real.  
  
### <a name="classify-all-active-directory-data"></a>Classificar todos os dados do Active Directory  
Além de gravação a um proprietário de negócios para todos os dados do Active Directory no momento em que ele é adicionado ao diretório, você também deve exigir os proprietários de empresas fornecer classificação para os dados. Por exemplo, se um aplicativo armazenar dados críticos de negócios, o proprietário da empresa deverá rotular como tal, o aplicativo de acordo com a infraestrutura de classificação da sua organização.  
  
Algumas organizações implementam políticas de classificação de dados que Rotular dados acordo com o dano que exposição dos dados incorreria se ele foi roubado ou exposto. Outras organizações implementam a classificação de dados que os dados rótulos por nível de importância, requisitos de acesso e retenção. Independentemente do modelo de classificação de dados em uso na sua organização, você deve garantir que você seja capaz de aplicar a classificação de dados do Active Directory, não apenas aos dados de "file". Se uma conta de usuário for uma conta de VIP, devem ser identificado em seu banco de dados de classificação de ativos (se você implementa isso por meio do uso de atributos nos objetos no AD DS, ou se você implanta bancos de dados de classificação de ativo separado).  
  
Dentro de seu modelo de classificação de dados, você deve incluir a classificação para dados do AD DS, como a seguir.  
  
### <a name="systems"></a>Sistemas  
Você não deve apenas classificar dados, mas também suas populações de servidor. Para cada servidor, você deve saber qual sistema operacional está instalado, quais funções gerais de servidor fornece, quais aplicativos estão em execução no servidor, o proprietário IT do registro e o proprietário da empresa do registro, onde aplicável. Para obter todos os dados ou aplicativos em execução no servidor, você deve exigir a classificação e o servidor deve ser protegido de acordo com os requisitos para as cargas de trabalho que ele dá suporte e as classificações aplicadas ao sistema e dados. Você também pode agrupar os servidores usando a classificação de suas cargas de trabalho, que permite que você identifique rapidamente os servidores que devem ser o mais se aproxima monitorados e configurados de forma mais rigorosa.  
  
### <a name="applications"></a>Aplicativos  
Você deve classificar os aplicativos por funcionalidade (o que eles fazem), a base de usuários (que usa os aplicativos) e o sistema operacional em que são executadas. Você deve manter os registros que contêm informações de versão, status de patch e outras informações pertinentes. Você também deve classificar aplicativos pelos tipos de dados, manipular, conforme descrito anteriormente.  
  
### <a name="users"></a>Usuários  
Se você chamá-los usuários "VIP", contas críticas, ou usar um rótulo diferente, as contas nas instalações do Active Directory que têm mais probabilidade de ser alvo de invasores, devem ser marcadas e monitoradas. Na maioria das organizações, simplesmente não é viável para monitorar todas as atividades de todos os usuários. No entanto, se você for capaz de identificar as contas críticas em sua instalação do Active Directory, você pode monitorar essas contas para que as alterações conforme descrito anteriormente neste documento.  
  
Você também pode começar a criar um banco de dados de "comportamentos esperados" para essas contas, como você as contas de auditoria. Por exemplo, se você achar que um determinado executivo usa sua estação de trabalho segura para acessar os dados críticos de negócios de seu escritório e em sua casa, mas raramente de outros locais, se você vir as tentativas de acesso a dados usando sua conta de um computador não autorizado ou um na metade em todo o planeta do local em que você sabe que o executivo não está localizado no momento, mais rapidamente, você pode identificar e investigar esse comportamento anormal.  
  
Integrando informações comerciais com sua infraestrutura, você pode usar essas informações de negócios para ajudar a identificar falsos positivos. Por exemplo, se executive viagem é gravada em um calendário que é acessível a equipe de TI responsável por monitorar o ambiente, você pode correlacionar as tentativas de conexão com locais conhecidos dos executivos.  
  
Digamos que um executivo de normalmente está localizado em Chicago e usa uma estação de trabalho protegida para acessar dados críticos de negócios de sua mesa de trabalho e um evento é disparado por uma falha na tentativa de acessar os dados de uma estação de trabalho segura, localizada em Atlanta. Se você for capaz de verificar que o executivo está atualmente em Atlanta, você pode resolver o evento, entrando em contato com o executivo ou o Assistente de um executivo para determinar se a falha de acesso foi o resultado do executivo esquecer de usar a estação de trabalho protegida acesse os dados. Criando um programa que usa as abordagens descritas [planejar para comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), você pode começar a criar um banco de dados de comportamentos esperados para contas mais "importante" em sua instalação do Active Directory que pode potencialmente ajudá-lo mais rapidamente, detectar e responder a ataques.  
  


