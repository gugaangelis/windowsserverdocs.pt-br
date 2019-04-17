---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Requisitos de Design de mapeamento para modelos de Design de floresta
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 48a2b03c6e29afcca565e861383a831050ef4233
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Requisitos de Design de mapeamento para modelos de Design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A maioria dos grupos em sua organização podem compartilhar uma única floresta organizacional que é gerenciado por um grupo de informática informações única e que contém as contas de usuário e recursos para todos os grupos que compartilham a floresta. Essa floresta compartilhada, chamada de floresta organizacional inicial, é a base do modelo de design de floresta para a organização.  
  
Porque a floresta organizacional inicial pode hospedar vários grupos na organização, o proprietário da floresta deve estabelecer contratos de nível de serviço com cada grupo para que todas as partes entender o que é esperado deles. Isso protege os grupos individuais e o proprietário da floresta, estabelecendo expectativas concordou em serviço.  
  
Se não todos os grupos em sua organização podem compartilhar uma única floresta organizacional, você deve expandir seu design de floresta para acomodar as necessidades dos grupos diferentes. Isso envolve identificando os requisitos de design que se aplicam aos grupos de acordo com suas necessidades de autonomia e isolamento e ou não têm uma rede de conectividade limitada e, em seguida, identificando o modelo de floresta que você pode usar para acomodar esses requisitos. A tabela a seguir lista os cenários de modelo de design de floresta com base nas autonomia, isolamento e fatores de conectividade. Depois de identificar o cenário de design de floresta que melhor atenda às suas necessidades, determine se você precisar fazer qualquer decisões adicionais para atender às suas especificações de design.  
  
> [!NOTE]  
> Se um fator está listado como n/d, ele não é uma consideração porque outros requisitos de acomodar também que o fator.  
  
|Cenário|Conectividade limitada|Isolamento de dados|Autonomia de dados|Isolamento de serviço|Autonomia de serviço|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Cenário 1: Associar uma floresta existente para autonomia de dados](#BKMK_1)|Não|Não|Sim|Não|Não|  
|[Cenário 2: Usar uma floresta organizacional ou domínio para autonomia de serviço](#BKMK_2)|Não|Não|N/D|Não|Sim|  
|[Cenário 3: Usar uma floresta organizacional ou floresta de recurso de isolamento de serviço](#BKMK_3)|Não|Não|N/D|Sim|N/D|  
|[Cenário 4: Usar uma floresta organizacional ou floresta de acesso restrito para isolamento de dados](#BKMK_4)|N/D|Sim|N/D|N/D|N/D|  
|[Cenário 5: Usar uma floresta organizacional ou reconfigurar o firewall para conectividade limitada](#BKMK_5)|Sim|Não|N/D|Não|Não|  
|[Cenário 6: Use uma floresta organizacional ou domínio e reconfigurar o firewall para autonomia de serviço com conectividade limitada](#BKMK_6)|Sim|Não|N/D|Não|Sim|  
|[Cenário 7: Use uma floresta do recurso e reconfigurar o firewall para isolamento de serviço com conectividade limitada](#BKMK_7)|Sim|Não|N/D|Sim|N/D|  
  
## <a name="BKMK_1"></a>Cenário 1: Associar uma floresta existente para autonomia de dados  
Você pode atender a um requisito para autonomia de dados simplesmente armazenando o grupo de unidades organizacionais (UOs) em uma floresta organizacional existente. Delegar controle sobre as UOs aos administradores de dados do grupo para obter autonomia de dados. Para obter mais informações sobre como delegar controle usando unidades organizacionais, consulte [criando um Design de unidade organizacional](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Cenário 2: Usar uma floresta organizacional ou domínio para autonomia de serviço  
Se um grupo em sua organização identifica autonomia de serviço como um requisito, recomendamos que você reconsidere primeiro esse requisito. Alcançar autonomia de serviço cria mais sobrecarga de gerenciamento e custos adicionais para a organização. Certifique-se de que o requisito para autonomia de serviço não é simplesmente por conveniência e que você pode justificar os custos envolvidos na reunião esse requisito.  
  
Você pode atender a um requisito para autonomia de serviço seguindo um destes procedimentos:  
  
-   Criando uma floresta organizacional. Coloque os usuários, grupos e computadores do grupo que exige autonomia de serviço em uma floresta organizacional separada. Atribua uma pessoa física desse grupo para ser o proprietário da floresta. Se o grupo tem recursos de acesso e o compartilhamento com outras florestas na organização, eles podem estabelecer uma relação de confiança entre sua floresta organizacional e as outras florestas.  
  
-   Usando domínios organizacionais. Coloque os usuários, grupos e computadores em um domínio separado em uma floresta organizacional existente. Esse modelo fornece para autonomia de serviço no nível do domínio apenas e não para autonomia de serviço completo, serviços isolamento ou isolamento de dados.  
  
Para obter mais informações sobre o uso de domínios organizacionais, consulte [usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  
  
## <a name="BKMK_3"></a>Cenário 3: Usar uma floresta organizacional ou floresta de recurso de isolamento de serviço  
Você pode atender a um requisito para isolamento de serviço seguindo um destes procedimentos:  
  
-   Usando uma floresta organizacional. Coloque os usuários, grupos e computadores do grupo que requer o isolamento de serviço em uma floresta organizacional separado. Atribua uma pessoa física desse grupo para ser o proprietário da floresta. Se o grupo tem recursos de acesso e o compartilhamento com outras florestas na organização, eles podem estabelecer uma relação de confiança entre sua floresta organizacional e as outras florestas. No entanto, não recomendamos essa abordagem porque o acesso a recursos por meio de grupos universais intensamente é restrito em cenários de confiança de floresta.  
  
-   Usando uma floresta do recurso. Coloque recursos e contas de serviço em uma floresta de recursos separado, manter contas de usuário em uma floresta organizacional existente. Se necessário, alternativas contas podem ser criadas na floresta de recursos para acessar recursos na floresta do recurso se floresta organizacional não estiver disponível. As contas alternativas devem ter a autoridade necessária para efetuar logon na floresta do recurso e manter o controle dos recursos até que a floresta organizacional é online novamente.  
  
    Estabelece uma relação de confiança entre o recurso e florestas organizacionais, para que os usuários podem acessar os recursos na floresta enquanto estiver usando suas contas de usuário normal. Essa configuração permite o gerenciamento centralizado de contas de usuário ao mesmo tempo, permitindo que os usuários fallback para contas alternativas na floresta do recurso se floresta organizacional não estiver disponível.  
  
Considerações para isolamento de serviço incluem o seguinte:  
  
-   Florestas criadas para isolamento de serviço podem confiar em domínios de outras florestas, mas não devem incluir os usuários de outras florestas em seus grupos de administradores de serviço. Se os usuários de outras florestas estão incluídos em grupos administrativos na floresta isolado, a segurança da floresta isolada potencialmente pode ser comprometida, pois os administradores de serviços na floresta não têm controle exclusivo.  
  
-   Como controladores de domínio estão acessíveis em uma rede, eles estarão sujeitos a ataques (por exemplo, ataques de negação de serviço) de software mal-intencionado na rede. Você pode fazer o seguinte para se proteger contra a possibilidade de um ataque:  
  
    -   Controladores de domínio do host apenas em redes que são consideradas seguras.  
  
    -   Limite o acesso à rede ou redes que hospeda os controladores de domínio.  
  
-   Isolamento de serviço requer a criação de uma floresta adicional. Avalie se o custo de manter a infraestrutura para dar suporte a floresta adicional supera os custos associados a perda de acesso aos recursos devido a uma floresta organizacional que não estão disponíveis.  
  
## <a name="BKMK_4"></a>Cenário 4: Usar uma floresta organizacional ou floresta de acesso restrito para isolamento de dados  
Você pode obter o isolamento de dados seguindo um destes procedimentos:  
  
-   Usando uma floresta organizacional. Coloque os usuários, grupos e computadores do grupo que requer o isolamento de dados em uma floresta organizacional separado. Atribua uma pessoa física desse grupo para ser o proprietário da floresta. Se o grupo tem recursos de acesso e o compartilhamento com outras florestas na organização, estabelece uma relação de confiança entre a floresta organizacional e as outras florestas. Somente os usuários que exigem acesso às informações confidenciais constar a nova floresta organizacional. Os usuários têm uma conta que eles usam dados de acesso classificado em sua própria floresta e dados não classificados em outras florestas por meio de relações de confiança.  
  
-   Usando uma floresta de acesso restrito. Isso é uma floresta separada que contém os dados restritos e as contas de usuário que são usadas para acessar os dados. Contas de usuário separados são mantidas nas florestas organizacionais existentes que são usadas para acessar os recursos na rede irrestritos. São criadas sem relações de confiança entre a floresta de acesso restrito e outras florestas na empresa. Você pode restringir ainda mais floresta Implantando floresta em uma rede física separada, para que ele não consegue se conectar a outras florestas. Se você implantar floresta em uma rede separada, os usuários devem ter duas estações de trabalho: um para acesso restrita floresta e outra para acessar as áreas nonrestricted da rede.  
  
Considerações para a criação de florestas para isolamento de dados incluem o seguinte:  
  
-   Florestas organizacionais criadas para isolamento de dados podem confiar em domínios de outras florestas, mas os usuários de outras florestas não devem ser incluídos em qualquer um destes procedimentos:  
  
    -   Grupos responsáveis pelo gerenciamento de serviços ou grupos que podem gerenciar a associação dos grupos de administradores de serviço  
  
    -   Grupos com controle administrativo sobre computadores que armazenam dados protegidos  
  
    -   Grupos que têm acesso a dados protegidos ou grupos que são responsáveis pelo gerenciamento de objetos de usuário ou grupo que têm acesso a dados protegidos  
  
    Se os usuários de outra floresta estão incluídos em qualquer um desses grupos, um comprometimento de outra floresta pode levar um comprometimento da floresta isolado e divulgação de dados protegidos.  
  
-   Outras florestas podem ser configuradas para confiar em floresta organizacional criada para isolamento de dados para que os usuários da floresta isolado podem acessar recursos em outras florestas. No entanto, os usuários da floresta isolado nunca interativamente devem fazer logon para estações de trabalho na floresta confiante. O computador na floresta confiante pode potencialmente ser comprometido por software mal-intencionado e pode ser usado para capturar as credenciais de logon do usuário.  
  
    > [!NOTE]  
    > Para impedir que os servidores em uma floresta confiável representando os usuários da floresta isolado e, em seguida, acessando recursos da floresta isolado, o proprietário da floresta pode desabilitar a autenticação delegada ou usar o recurso de delegação restrita. Para saber mais sobre autenticação delegada e delegação restrita, consulte delegação de autenticação ([https://go.microsoft.com/fwlink/?LinkId=106614](https://go.microsoft.com/fwlink/?LinkId=106614)).  
  
-   Talvez seja necessário estabelecer um firewall entre a floresta organizacional e as outras florestas da organização para limitar o acesso de usuário às informações de fora da sua floresta.  
  
-   Embora a criação de uma floresta separada permite o isolamento de dados, desde que os controladores de domínio na floresta isolada e computadores essas informações de host protegido estão acessíveis em uma rede, eles estarão sujeitos a ataques iniciados a partir de computadores na rede. As organizações que decida que o risco de ataque for muito alto ou que consequência de uma violação de segurança ou de ataque for muito grande precisam limitar o acesso à rede ou redes que hospedam os controladores de domínio e os computadores que hospedam dados protegidos. Limitar o acesso pode ser feito usando tecnologias, como firewalls e protocolo IPSec (IPsec). Em casos extremos, as organizações podem optar por manter os dados protegidos em uma rede independente que não tem nenhuma conexão física para qualquer outra rede da organização.  
  
    > [!NOTE]  
    > Se existir qualquer conectividade de rede entre uma floresta de acesso restrito e outra rede, existe a possibilidade de dados na área restrita a serem transmitidos para a outra rede.  
  
## <a name="BKMK_5"></a>Cenário 5: Usar uma floresta organizacional ou reconfigurar o firewall para conectividade limitada  
Para atender a um requisito de conectividade limitada, você pode fazer um destes procedimentos:  
  
-   Coloque os usuários em uma floresta organizacional existente e, em seguida, abra o firewall suficiente para permitir o tráfego do Active Directory passar por meio.  
  
-   Use uma floresta organizacional. Coloque os usuários, grupos e computadores do grupo para o qual a conectividade é limitada em uma floresta organizacional separada. Atribua uma pessoa física desse grupo para ser o proprietário da floresta. Floresta organizacional oferece um ambiente separado do outro lado do firewall. Floresta inclui contas de usuário e recursos que são gerenciados na floresta, para que os usuários não precisam passar pelo firewall para realizar suas tarefas diárias. Usuários específicos ou aplicativos podem ter necessidades especiais que requerem a funcionalidade para passar pelo firewall para entrar em contato com outras florestas. Você pode atender a essas necessidades individualmente, abrindo as interfaces apropriadas no firewall, incluindo aqueles necessários para as relações de confiança funcionar.  
  
Para obter mais informações sobre como configurar firewalls para uso com os serviços de domínio do Active Directory (AD DS), consulte Active Directory em redes segmentadas por Firewalls ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  
## <a name="BKMK_6"></a>Cenário 6: Use uma floresta organizacional ou domínio e reconfigurar o firewall para autonomia de serviço com conectividade limitada  
Se um grupo em sua organização identifica autonomia de serviço como um requisito, recomendamos que você reconsidere primeiro esse requisito. Alcançar autonomia de serviço cria mais sobrecarga de gerenciamento e custos adicionais para a organização. Certifique-se de que o requisito para autonomia de serviço não é simplesmente por conveniência e que você pode justificar os custos envolvidos na reunião esse requisito.  
  
Se a conectividade limitada é um problema, e você tiver um requisito para autonomia de serviço, você pode fazer um destes procedimentos:  
  
-   Use uma floresta organizacional. Coloque os usuários, grupos e computadores do grupo que exige autonomia de serviço em uma floresta organizacional separada. Atribua uma pessoa física desse grupo para ser o proprietário da floresta. Floresta organizacional oferece um ambiente separado do outro lado do firewall. Floresta inclui contas de usuário e recursos que são gerenciados na floresta, para que os usuários não precisam passar pelo firewall para realizar suas tarefas diárias. Usuários específicos ou aplicativos podem ter necessidades especiais que requerem a funcionalidade para passar pelo firewall para entrar em contato com outras florestas. Você pode atender a essas necessidades individualmente, abrindo as interfaces apropriadas no firewall, incluindo aqueles necessários para as relações de confiança funcionar.  
  
-   Coloque os usuários, grupos e computadores em um domínio separado em uma floresta organizacional existente. Esse modelo fornece para autonomia de serviço no nível do domínio apenas e não para autonomia de serviço completo, serviços isolamento ou isolamento de dados. Outros grupos na floresta devem confiar os administradores de serviços do novo domínio com o mesmo grau que eles confiam o proprietário da floresta. Por esse motivo, não recomendamos essa abordagem. Para obter mais informações sobre o uso de domínios organizacionais, consulte [usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  
  
Você também precisa abrir o firewall suficiente para permitir o tráfego do Active Directory passagem. Para obter mais informações sobre como configurar firewalls para uso com o AD DS, consulte Active Directory em redes segmentadas por Firewalls ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  
## <a name="BKMK_7"></a>Cenário 7: Use uma floresta do recurso e reconfigurar o firewall para isolamento de serviço com conectividade limitada  
Se a conectividade limitada é um problema, e você tiver um requisito para isolamento de serviço, você pode fazer um destes procedimentos:  
  
-   Use uma floresta organizacional. Coloque os usuários, grupos e computadores do grupo que requer o isolamento de serviço em uma floresta organizacional separado. Atribua uma pessoa física desse grupo para ser o proprietário da floresta. Floresta organizacional oferece um ambiente separado do outro lado do firewall. Floresta inclui contas de usuário e recursos que são gerenciados na floresta, para que os usuários não precisam passar pelo firewall para realizar suas tarefas diárias. Usuários específicos ou aplicativos podem ter necessidades especiais que requerem a funcionalidade para passar pelo firewall para entrar em contato com outras florestas. Você pode atender a essas necessidades individualmente, abrindo as interfaces apropriadas no firewall, incluindo aqueles necessários para as relações de confiança funcionar.  
  
-   Use uma floresta do recurso. Coloque recursos e contas de serviço em uma floresta de recursos separado, manter contas de usuário em uma floresta organizacional existente. Talvez seja necessário criar algumas contas de usuário alternativas na floresta de recursos para manter o acesso à floresta recurso se floresta organizacional não estiver disponível. As contas alternativas devem ter a autoridade necessária para efetuar logon na floresta do recurso e manter o controle dos recursos até que a floresta organizacional é online novamente.  
  
    Estabelece uma relação de confiança entre o recurso e florestas organizacionais, para que os usuários podem acessar os recursos na floresta enquanto estiver usando suas contas de usuário normal. Essa configuração permite o gerenciamento centralizado de contas de usuário ao mesmo tempo, permitindo que os usuários fallback para contas alternativas na floresta do recurso se floresta organizacional não estiver disponível.  
  
Considerações para isolamento de serviço incluem o seguinte:  
  
-   Florestas criadas para isolamento de serviço podem confiar em domínios de outras florestas, mas não devem incluir os usuários de outras florestas em seus grupos de administradores de serviço. Se os usuários de outras florestas estão incluídos em grupos administrativos na floresta isolado, a segurança da floresta isolada potencialmente pode ser comprometida, pois os administradores de serviços na floresta não têm controle exclusivo.  
  
-   Como controladores de domínio estão acessíveis em uma rede, eles estarão sujeitos a ataques (por exemplo, ataques de negação de serviço) de computadores na rede. Você pode fazer o seguinte para se proteger contra a possibilidade de um ataque:  
  
    -   Controladores de domínio do host apenas em redes que são consideradas seguras.  
  
    -   Limite o acesso à rede ou redes que hospeda os controladores de domínio.  
  
-   Isolamento de serviço requer a criação de uma floresta adicional. Avalie se o custo de manter a infraestrutura para dar suporte a floresta adicional supera os custos associados a perda de acesso aos recursos devido a uma floresta organizacional que não estão disponíveis.  
  
    Usuários específicos ou aplicativos podem ter necessidades especiais que requerem a funcionalidade para passar pelo firewall para entrar em contato com outras florestas. Você pode atender a essas necessidades individualmente, abrindo as interfaces apropriadas no firewall, incluindo aqueles necessários para as relações de confiança funcionar.  
  
Para obter mais informações sobre como configurar firewalls para uso com o AD DS, consulte Active Directory em redes segmentadas por Firewalls ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  


