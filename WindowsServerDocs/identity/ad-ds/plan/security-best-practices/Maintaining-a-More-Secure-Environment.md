---
ms.assetid: 8f994e2e-6c07-43f0-aef4-75f8b2c9a144
title: Manter um ambiente mais seguro
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d4ddefda4ce9488320927542dcd24b9ac092bdb4
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994313"
---
# <a name="maintaining-a-more-secure-environment"></a>Manter um ambiente mais seguro

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Número da lei dez: a tecnologia não é uma solução.* - [10 leis imutáveis da administração de segurança](/previous-versions/cc722488(v=technet.10))

Quando você tiver criado um ambiente gerenciável e seguro para seus ativos de negócios críticos, seu foco deverá mudar para garantir que ele seja mantido com segurança. Embora você tenha recebido controles técnicos específicos para aumentar a segurança de suas instalações de AD DS, a tecnologia sozinha não protegerá um ambiente no qual ela não funciona em parceria com a empresa para manter uma infraestrutura segura e utilizável. As recomendações de alto nível nesta seção devem ser usadas como diretrizes que você pode usar para desenvolver não apenas a segurança efetiva, mas o gerenciamento eficiente do ciclo de vida.

Em alguns casos, sua organização de ti pode já ter uma relação de trabalho próxima com unidades de negócios, o que facilitará a implementação dessas recomendações. Em organizações nas quais as unidades de ti e de negócios não estão fortemente ligadas, talvez seja necessário primeiro obter o patrocínio executivo para os esforços de forjar uma relação mais próxima entre ti e unidades de negócios. O [Resumo executivo](../../../ad-ds/manage/component-updates/Executive-Summary.md) destina-se a ser útil como um documento autônomo para análise executiva e pode ser disseminado para os tomadores de decisões em sua organização.

## <a name="creating-business-centric-security-practices-for-active-directory"></a>Criando práticas de segurança centradas nos negócios para Active Directory
No passado, a tecnologia da informação em muitas organizações foi vista como uma estrutura de suporte e um centro de custo. Os departamentos de ti costumavam ser amplamente separados dos usuários empresariais e as interações limitadas a um modelo de solicitação-resposta no qual os negócios solicitaram recursos e responderam.

À medida que a tecnologia evoluiu e proliferau, a visão de "um computador em cada área de trabalho" foi efetivamente passada para a maioria do mundo e, até mesmo, foi acompanhada pela ampla variedade de tecnologias facilmente acessíveis disponíveis atualmente. A tecnologia da informação não é mais uma função de suporte, é uma função comercial fundamental. Se a sua organização não pôde continuar a funcionar se todos os serviços de ti estivessem indisponíveis, o negócio da sua organização é, pelo menos, em parte, tecnologia da informação.

Para criar planos de recuperação de comprometimento efetivos, os serviços de ti devem trabalhar junto com as unidades de negócios em sua organização para identificar não apenas os componentes mais críticos do cenário de ti, mas as funções críticas exigidas pela empresa. Ao identificar o que é importante para sua organização como um todo, você pode se concentrar em proteger os componentes que têm mais valor. Isso não é uma recomendação para shirkr a segurança de sistemas de baixo valor e dados. Em vez disso, como você define níveis de serviço para o tempo de atividade do sistema, considere definir níveis de controle de segurança e monitoramento com base na criticalidade do ativo.

Quando você investiu na criação de um ambiente atual, seguro e gerenciável, você pode mudar o foco para gerenciá-lo com eficiência e garantir que você tenha processos de gerenciamento de ciclo de vida efetivos que não são determinados apenas por ele, mas pelos negócios. Para conseguir isso, você precisa não apenas fazer parcerias com os negócios, mas para investir a empresa em "Propriedade" de dados e sistemas em Active Directory.

Quando dados e sistemas são introduzidos em Active Directory sem proprietários designados, proprietários de negócios e proprietários de ti, não há nenhuma cadeia clara de responsabilidade para provisionamento, gerenciamento, monitoramento, atualização e, eventualmente, encerramento do sistema. Isso resulta em infraestruturas nas quais os sistemas expõem a organização a riscos, mas não podem ser descomissionados porque a propriedade não está clara. Para gerenciar efetivamente o ciclo de vida dos usuários, dados, aplicativos e sistemas gerenciados pela instalação do Active Directory, você deve seguir os princípios descritos nesta seção.

### <a name="assign-a-business-owner-to-active-directory-data"></a>Atribuir um proprietário da empresa a dados de Active Directory
Os dados no Active Directory devem ter um proprietário de negócios identificado, ou seja, um departamento ou usuário especificado que seja o ponto de contato para decisões sobre o ciclo de vida do ativo. Em alguns casos, o proprietário da empresa de um componente do Active Directory será um departamento de ti ou usuário. Componentes de infraestrutura, como controladores de domínio, servidores DHCP e DNS, e Active Directory provavelmente serão "de propriedade". Para dados que são adicionados a AD DS para dar suporte à empresa (por exemplo, novos funcionários, novos aplicativos e novos repositórios de informações), uma unidade de negócios ou um usuário designado deve ser associado aos dados.

Se você usar Active Directory para registrar a propriedade dos dados no diretório, ou se você implementar um banco de dado separado para acompanhar ativos de ti, nenhuma conta de usuário deverá ser criada, nenhum servidor ou estação de trabalho deverá ser instalado e nenhum aplicativo deverá ser implantado sem um proprietário designado do registro. A tentativa de estabelecer a propriedade dos sistemas depois que eles foram implantados na produção pode ser desafiadora na melhor das hipóteses e não é possível em alguns casos. Portanto, a propriedade deve ser estabelecida no momento em que os dados são introduzidos em Active Directory.

### <a name="implement-business-driven-lifecycle-management"></a>Implementar o gerenciamento do ciclo de vida orientado aos negócios
O gerenciamento do ciclo de vida deve ser implementado para todos os dados no Active Directory. Por exemplo, quando um novo aplicativo é introduzido em um domínio Active Directory, o proprietário de negócios do aplicativo deve, em intervalos regulares, ser atestado para o uso contínuo do aplicativo. Quando uma nova versão de um aplicativo é liberada, o proprietário da empresa do aplicativo deve ser informado e deve decidir se e quando a nova versão será implementada.

Se um proprietário de negócios optar por não aprovar a implantação de uma nova versão de um aplicativo, o proprietário da empresa também deverá ser notificado sobre a data em que a versão atual não terá mais suporte e deverá ser responsável por determinar se o aplicativo será encerrado ou substituído. Manter aplicativos herdados em execução e sem suporte não deve ser uma opção.

Quando as contas de usuário são criadas no Active Directory, seus gerentes de registro devem ser notificados na criação do objeto e necessários para atestar a validade da conta em intervalos regulares. Ao implementar um ciclo de vida orientado por negócios e um atestado regular da validade dos dados, as pessoas que são mais bem equipadas para identificar anomalias nos dados são as pessoas que revisam os dados.

Por exemplo, os invasores podem criar contas de usuário que parecem ser contas válidas, seguindo as convenções de nomenclatura da sua organização e o posicionamento do objeto. Para detectar essas criações de conta, você pode implementar uma tarefa diária que retorna todos os objetos de usuário sem um proprietário de negócios designado para que você possa investigar as contas. Se os invasores criarem contas e atribuirem um proprietário de negócios, implementando uma tarefa que relata a nova criação de objeto para o proprietário de negócios designado, o proprietário da empresa poderá identificar rapidamente se a conta é legítima.

Você deve implementar abordagens semelhantes a grupos de segurança e distribuição. Embora alguns grupos possam ser grupos funcionais criados por ele, criando cada grupo com um proprietário designado, você pode recuperar todos os grupos pertencentes a um usuário designado e exigir que o usuário atestado para a validade de suas associações. Semelhante à abordagem obtida com a criação de conta de usuário, você pode disparar modificações de grupo de relatórios para o proprietário de negócios designado. Quanto mais rotina ela se tornar um proprietário de negócios para atestar a validade ou invalidação de dados em Active Directory, mais equipada você será identificar anomalias que podem indicar falhas de processo ou o comprometimento real.

### <a name="classify-all-active-directory-data"></a>Classificar todos os dados de Active Directory
Além de registrar um proprietário de negócios para todos os dados de Active Directory no momento em que ele é adicionado ao diretório, você também deve exigir que os proprietários de negócios forneçam classificação para os dados. Por exemplo, se um aplicativo armazena dados críticos para os negócios, o proprietário da empresa deve rotular o aplicativo como tal, de acordo com a infraestrutura de classificação da sua organização.

Algumas organizações implementam políticas de classificação de dados que rotulam dados de acordo com o dano que a exposição dos dados incorreria se fosse roubada ou exposta. Outras organizações implementam a classificação de dados que rotula dados por criticalidade, por requisitos de acesso e por retenção. Independentemente do modelo de classificação de dados em uso em sua organização, você deve garantir que seja capaz de aplicar a classificação aos dados de Active Directory, não apenas aos dados de "arquivo". Se a conta de um usuário for uma conta VIP, ela deverá ser identificada no banco de dados de classificação de ativos (independentemente de você implementá-la por meio do uso de atributos nos objetos no AD DS, ou se você implantar bancos de dados de classificação de ativos separados).

Em seu modelo de classificação de dados, você deve incluir classificação para dados de AD DS, como o seguinte.

### <a name="systems"></a>Sistemas
Você não deve apenas classificar dados, mas também suas populações de servidor. Para cada servidor, você deve saber qual sistema operacional está instalado, quais funções gerais o servidor fornece, quais aplicativos estão em execução no servidor, o proprietário do registro e o proprietário comercial do registro, quando aplicável. Para todos os dados ou aplicativos em execução no servidor, você deve precisar de classificação e o servidor deve ser protegido de acordo com os requisitos para as cargas de trabalho que ele suporta e as classificações aplicadas ao sistema e aos dados. Você também pode agrupar servidores pela classificação de suas cargas de trabalho, o que permite identificar rapidamente os servidores que devem ser os mais bem monitorados e mais rigorosamente configurados.

### <a name="applications"></a>Aplicativos
Você deve classificar os aplicativos por funcionalidade (o que eles fazem), a base de usuários (que usa os aplicativos) e o sistema operacional no qual eles são executados. Você deve manter os registros que contêm informações de versão, o status do patch e qualquer outra informação pertinente. Você também deve classificar os aplicativos pelos tipos de dados que eles manipulam, conforme descrito anteriormente.

### <a name="users"></a>Usuários
Se você chamá-los de usuários "VIP", contas críticas ou usar um rótulo diferente, as contas em suas Active Directory instalações com maior probabilidade de serem direcionadas por invasores devem ser marcadas e monitoradas. Na maioria das organizações, simplesmente não é possível monitorar todas as atividades de todos os usuários. No entanto, se for possível identificar as contas críticas em sua instalação do Active Directory, você poderá monitorar essas contas em busca de alterações, conforme descrito anteriormente neste documento.

Você também pode começar a criar um banco de dados de "comportamentos esperados" para essas contas à medida que audita as contas. Por exemplo, se você descobrir que um executivo específico usa sua estação de trabalho protegida para acessar dados críticos de negócios de seu escritório e de sua casa, mas raramente de outros locais, se você vir tentativas de acessar dados usando sua conta de um computador não autorizado ou um local no meio do planeta em que você sabe que o executivo não está localizado no momento, você pode identificar e investigar mais rapidamente esse comportamento anormal.

Ao integrar as informações de negócios à sua infraestrutura, você pode usar essas informações comerciais para ajudá-lo a identificar falsos positivos. Por exemplo, se a viagem Executiva for registrada em um calendário acessível para a equipe de ti responsável por monitorar o ambiente, você poderá correlacionar as tentativas de conexão com os locais conhecidos dos executivos.

Digamos que o executivo A está localizado normalmente em Chicago e usa uma estação de trabalho protegida para acessar dados críticos para os negócios de sua mesa, e um evento é disparado por uma tentativa com falha de acessar os dados de uma estação de trabalho desprotegida localizada em Atlanta. Se for possível verificar se o executivo está atualmente em Atlanta, você pode resolver o evento entrando em contato com o executivo ou com o assistente do executivo para determinar se a falha de acesso foi o resultado do executivo esquecer de usar a estação de trabalho protegida para acessar os dados. Ao construir um programa que usa as abordagens descritas em [planejamento para o comprometimento](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md), você pode começar a criar um banco de dados de comportamentos esperados para as contas mais "importantes" na instalação do Active Directory que podem ajudá-lo a descobrir e responder a ataques com mais rapidez.

