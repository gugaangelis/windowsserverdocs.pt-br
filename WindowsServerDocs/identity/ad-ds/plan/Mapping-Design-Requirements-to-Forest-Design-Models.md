---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Requisitos de mapeamento de Design para modelos de Design de floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 35d6322f053c7a02dc1df5430b28f771f57a1ad7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442574"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Requisitos de mapeamento de Design para modelos de Design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A maioria dos grupos em sua organização podem compartilhar uma única floresta organizacional, que é gerenciado por um grupo de TI (tecnologia) informações de único e que contém as contas de usuário e recursos para todos os grupos que compartilham a floresta. Essa floresta compartilhada, chamada de floresta organizacional inicial, é a base do modelo de design de floresta para a organização.  

Como a floresta organizacional inicial pode hospedar vários grupos da organização, o proprietário da floresta deve estabelecer contratos de nível de serviço com cada grupo para que todas as partes entender o que é esperado deles. Isso protege os grupos individuais e o proprietário da floresta, estabelecendo as expectativas de serviço acordado.  

Se nem todos os grupos em sua organização podem compartilhar uma única floresta organizacional, você deve expandir o seu design de floresta para acomodar as necessidades dos diferentes grupos. Isso envolve a identificar os requisitos de design que se aplicam aos grupos de acordo com suas necessidades de autonomia e isolamento, e se eles têm uma rede de conectividade limitada e, em seguida, identificando o modelo de floresta que você pode usar para acomodar os requisitos. A tabela a seguir lista os cenários de modelo de design de floresta com base na autonomia, isolamento e fatores de conectividade. Depois de identificar o cenário de design de floresta que melhor corresponde aos seus requisitos, determine se você precisa quaisquer decisões adicionais para atender às suas especificações de design.  

> [!NOTE]  
> Se um fator é listado como n/d, não é uma consideração porque outros requisitos também acomodar esse fator.  

|Cenário|Conectividade limitada|Isolamento de dados|Autonomia de dados|Isolamento de serviço|Autonomia do serviço|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Cenário 1: Ingressar em uma floresta existente para a autonomia de dados](#BKMK_1)|Não|Não|Sim|Não|Não|  
|[Cenário 2: Usar um domínio ou floresta organizacional para a autonomia do serviço](#BKMK_2)|Não|Não|N/D|Não|Sim|  
|[Cenário 3: Usar uma floresta organizacional ou uma floresta de recursos para isolamento de serviço](#BKMK_3)|Não|Não|N/D|Sim|N/D|  
|[Cenário 4: Usar uma floresta organizacional ou uma floresta de acesso restrito para isolamento de dados](#BKMK_4)|N/D|Sim|N/D|N/D|N/D|  
|[Cenário 5: Use uma floresta organizacional ou reconfigurar o firewall para conectividade limitada](#BKMK_5)|Sim|Não|N/D|Não|Não|  
|[Cenário 6: Usar um domínio ou floresta organizacional e reconfigurar o firewall para a autonomia do serviço com conectividade limitada](#BKMK_6)|Sim|Não|N/D|Não|Sim|  
|[Cenário 7: Use uma floresta de recurso e reconfigurar o firewall para o isolamento de serviço com conectividade limitada](#BKMK_7)|Sim|Não|N/D|Sim|N/D|  

## <a name="BKMK_1"></a>Cenário 1: Ingressar em uma floresta existente para a autonomia de dados  

Você pode atender a um requisito para a autonomia de dados simplesmente ao hospedar o grupo em unidades organizacionais (OUs) em uma floresta organizacional existente. Delegar o controle sobre as UOs para os administradores de dados do grupo para alcançar a autonomia de dados. Para obter mais informações sobre a delegação de controle por meio de OUs, consulte [criando um Design de unidade organizacional](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Cenário 2: Usar um domínio ou floresta organizacional para a autonomia do serviço  

Se um grupo em sua organização identifica a autonomia do serviço como um requisito, é recomendável que você primeiro reconsiderar a esse requisito. Alcançar a autonomia de serviço cria mais sobrecarga de gerenciamento e custos adicionais para a organização. Certifique-se de que o requisito para a autonomia do serviço não é apenas para conveniência e que você pode justificar os custos envolvidos em atender esse requisito.  
  
Você pode atender a um requisito para a autonomia do serviço seguindo um destes procedimentos:  

- Criação de uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer a autonomia do serviço em uma floresta organizacional separada. Atribua um indivíduo de grupo para ser o proprietário da floresta. Se o grupo precisa de acesso ou compartilhamento de recursos com outras florestas da organização, eles podem estabelecer uma relação de confiança entre sua floresta organizacional e as outras florestas.  

- Usando domínios organizacionais. Coloque os usuários, grupos e computadores em um domínio separado em uma floresta organizacional existente. Esse modelo fornece para apenas a autonomia do serviço de nível de domínio e não para a autonomia do serviço completo, serviço isolamento ou isolamento de dados.  

Para obter mais informações sobre como usar domínios organizacionais, consulte [usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

## <a name="BKMK_3"></a>Cenário 3: Usar uma floresta organizacional ou uma floresta de recursos para isolamento de serviço  

Você pode atender a um requisito para isolamento de serviço seguindo um destes procedimentos:  

- Usando uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer o isolamento de serviço em uma floresta organizacional separada. Atribua um indivíduo de grupo para ser o proprietário da floresta. Se o grupo precisa de acesso ou compartilhamento de recursos com outras florestas da organização, eles podem estabelecer uma relação de confiança entre sua floresta organizacional e as outras florestas. No entanto, não recomendamos essa abordagem porque o acesso a recursos por meio de grupos universais é muito restrito em cenários de confiança de floresta.  

- Usando uma floresta de recursos. Colocar recursos e contas de serviço em uma floresta de recurso separado, mantendo as contas de usuário em uma floresta organizacional existente. Se necessário, contas alternativas podem ser criadas na floresta de recursos para acessar recursos na floresta de recursos se a floresta organizacional ficar indisponível. As contas alternativas devem ter a autoridade necessária para fazer logon floresta de recursos e manter o controle dos recursos até que a floresta organizacional é voltar a ficar online.  

   Estabelece uma relação de confiança entre florestas organizacionais e o recurso para que os usuários possam acessar os recursos da floresta usando suas contas de usuário normal. Essa configuração permite o gerenciamento centralizado de contas de usuário, permitindo aos usuários a cair para contas alternativas na floresta de recursos se a floresta organizacional ficar indisponível.  

Considerações para isolamento de serviço incluem o seguinte:

- Florestas criadas para o isolamento de serviço podem confiar em domínios de outras florestas, mas não devem incluir os usuários de outras florestas em seus grupos de administradores de serviço. Se os usuários de outras florestas são incluídos em grupos administrativos na floresta isolado, a segurança da floresta isolada potencialmente pode ser comprometida, pois os administradores de serviço na floresta não tem controle exclusivo.  

- Controladores de domínio estão acessíveis em uma rede, desde que eles estão sujeitos a ataques (como ataques de negação de serviço) de software mal-intencionado na rede. Você pode fazer o seguinte para proteger contra a possibilidade de um ataque:  

   - Controladores de domínio do host somente em redes que são considerados seguros.  

   - Limitar o acesso à rede ou redes hospedando os controladores de domínio.  

- Isolamento de serviço exige a criação de uma floresta de adicional. Avalie se o custo de manter a infraestrutura para dar suporte a florestas adicionais supera os custos associados à perda de acesso aos recursos devido a uma floresta organizacional está indisponível.  

## <a name="BKMK_4"></a>Cenário 4: Usar uma floresta organizacional ou uma floresta de acesso restrito para isolamento de dados  

Você pode obter o isolamento de dados seguindo um destes procedimentos:  

- Usando uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer o isolamento de dados em uma floresta organizacional separada. Atribua um indivíduo de grupo para ser o proprietário da floresta. Se o grupo precisa de acesso ou compartilhamento de recursos com outras florestas da organização, estabelece uma relação de confiança entre a floresta organizacional e as outras florestas. Somente os usuários que precisam de acesso às informações confidenciais existem na nova floresta organizacional. Os usuários têm uma conta que eles usam para acessar classificado os dados em sua própria floresta e de dados não classificados em outras florestas por meio de relações de confiança.  

- Usando uma floresta de acesso restrito. Isso é uma floresta separada que contém os dados restritos e as contas de usuário que são usadas para acessar os dados. Contas de usuário separadas são mantidas nas florestas existentes organizacionais que são usadas para acessar os recursos sem restrições na rede. Não há relações de confiança são criadas entre a floresta de acesso restrito e outras florestas na empresa. Você pode restringir ainda mais a floresta implantando a floresta em uma rede física separada, para que ele não pode se conectar às outras florestas. Se você implantar a floresta em uma rede separada, os usuários devem ter duas estações de trabalho: um para acessar a floresta restrita e outra para acessar as áreas nonrestricted da rede.  

Considerações sobre a criação de florestas para isolamento de dados incluem o seguinte:  

- Organizacionais florestas criadas para o isolamento de dados podem confiar em domínios de outras florestas, mas os usuários de outras florestas não devem ser incluídos em qualquer uma das seguintes opções:  

  - Grupos responsáveis pelo gerenciamento de serviços ou grupos que podem gerenciar a associação de grupos de administradores de serviço  

  - Grupos que têm controle administrativo sobre computadores que armazenam dados protegidos  

  - Protegido de grupos que têm acesso a dados ou grupos responsáveis pelo gerenciamento de objetos de usuário ou objetos de grupo que têm acesso a dados de protegidos  

    Se os usuários de outra floresta forem incluídos em qualquer um desses grupos, um comprometimento de outra floresta pode levar ao comprometimento da floresta isolado e a divulgação dos dados protegidos.  

- Outras florestas podem ser configuradas para confiar na floresta organizacional criada para o isolamento de dados para que os usuários na floresta isolado podem acessar recursos em outras florestas. No entanto, os usuários da floresta isolado interativamente nunca devem fazer logon em estações de trabalho na floresta confiante. O computador na floresta confiante potencialmente pode ser comprometido por software mal-intencionado e pode ser usado para capturar as credenciais de logon do usuário.  

   > [!NOTE]
   > Para impedir que servidores em uma floresta confiante representando os usuários da floresta isolado e, em seguida, acessar recursos na floresta isolado, o proprietário da floresta pode desabilitar a autenticação delegada ou usar o recurso de delegação restrita. Para obter mais informações sobre a autenticação delegada e delegação restrita, consulte [delegando a autenticação](https://go.microsoft.com/fwlink/?LinkId=106614).  

- Talvez seja necessário estabelecer um firewall entre a floresta organizacional e as outras florestas da organização para limitar o acesso de usuário para informações fora de sua floresta.  

- Embora a criação de uma floresta separada habilita o isolamento de dados, desde que os controladores de domínio na floresta isolada e computadores em que essas informações de host protegido são acessíveis em uma rede, estão sujeitos a ataques iniciados a partir de computadores na rede. As organizações que decidir que o risco de ataque é muito alto ou que a consequência de uma violação de segurança ou ataque é muito grande precisam limitar o acesso à rede ou redes que hospedam os controladores de domínio e os computadores que hospedam dados de protegidos . Limitando o acesso pode ser feito por meio de tecnologias como firewalls e Internet Protocol security (IPsec). Em casos extremos, as organizações podem optar por manter os dados protegidos em uma rede independente que não tem nenhuma conexão física com qualquer outra rede da organização.  

   > [!NOTE]  
   > Se houver qualquer conectividade de rede entre uma floresta de acesso restrito e outra rede, existe a possibilidade dos dados na área restrita para serem transmitidos a outra rede.  

## <a name="BKMK_5"></a>Cenário 5: Use uma floresta organizacional ou reconfigurar o firewall para conectividade limitada  

Para atender a um requisito de conectividade limitada, você pode fazer o seguinte:  

- Coloque os usuários em uma floresta organizacional existente e, em seguida, abra o firewall suficiente para permitir o tráfego do Active Directory de passagem.  

- Use uma floresta organizacional. Colocar os usuários, grupos e computadores para o grupo para o qual a conectividade é limitada em uma floresta organizacional separada. Atribua um indivíduo de grupo para ser o proprietário da floresta. A floresta organizacional fornece um ambiente separado no outro lado do firewall. A floresta inclui contas de usuário e recursos que são gerenciados dentro da floresta, para que os usuários precisam atravessar o firewall para realizar suas tarefas diárias. Usuários específicos ou aplicativos podem ter necessidades especiais que exigem a capacidade de passar pelo firewall, entre em contato com outras florestas. Você pode atender a essas necessidades individualmente, abrindo as interfaces apropriadas no firewall, incluindo aqueles necessários para as relações de confiança funcionar.  

Para obter mais informações sobre como configurar firewalls para uso com os serviços de domínio do Active Directory (AD DS), consulte [do Active Directory em redes segmentadas por Firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_6"></a>Cenário 6: Usar um domínio ou floresta organizacional e reconfigurar o firewall para a autonomia do serviço com conectividade limitada  

Se um grupo em sua organização identifica a autonomia do serviço como um requisito, é recomendável que você primeiro reconsiderar a esse requisito. Alcançar a autonomia de serviço cria mais sobrecarga de gerenciamento e custos adicionais para a organização. Certifique-se de que o requisito para a autonomia do serviço não é apenas para conveniência e que você pode justificar os custos envolvidos em atender esse requisito.  

Se a conectividade limitada é um problema, e você tiver um requisito para a autonomia do serviço, você pode fazer o seguinte:  

- Use uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer a autonomia do serviço em uma floresta organizacional separada. Atribua um indivíduo de grupo para ser o proprietário da floresta. A floresta organizacional fornece um ambiente separado no outro lado do firewall. A floresta inclui contas de usuário e recursos que são gerenciados dentro da floresta, para que os usuários precisam atravessar o firewall para realizar suas tarefas diárias. Usuários específicos ou aplicativos podem ter necessidades especiais que exigem a capacidade de passar pelo firewall, entre em contato com outras florestas. Você pode atender a essas necessidades individualmente, abrindo as interfaces apropriadas no firewall, incluindo aqueles necessários para as relações de confiança funcionar.  

- Coloque os usuários, grupos e computadores em um domínio separado em uma floresta organizacional existente. Esse modelo fornece para apenas a autonomia do serviço de nível de domínio e não para a autonomia do serviço completo, serviço isolamento ou isolamento de dados. Outros grupos na floresta devem confiar os administradores de serviço do novo domínio com o mesmo grau que confiam o proprietário da floresta. Por esse motivo, não recomendamos essa abordagem. Para obter mais informações sobre como usar domínios organizacionais, consulte [usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

Você também precisará abrir o firewall suficiente para permitir o tráfego do Active Directory de passagem. Para obter mais informações sobre como configurar firewalls para uso com o AD DS, consulte [do Active Directory em redes segmentadas por Firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_7"></a>Cenário 7: Use uma floresta de recurso e reconfigurar o firewall para o isolamento de serviço com conectividade limitada  

Se a conectividade limitada é um problema, e você tiver um requisito para isolamento de serviço, você pode fazer o seguinte:  

- Use uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer o isolamento de serviço em uma floresta organizacional separada. Atribua um indivíduo de grupo para ser o proprietário da floresta. A floresta organizacional fornece um ambiente separado no outro lado do firewall. A floresta inclui contas de usuário e recursos que são gerenciados dentro da floresta, para que os usuários precisam atravessar o firewall para realizar suas tarefas diárias. Usuários específicos ou aplicativos podem ter necessidades especiais que exigem a capacidade de passar pelo firewall, entre em contato com outras florestas. Você pode atender a essas necessidades individualmente, abrindo as interfaces apropriadas no firewall, incluindo aqueles necessários para as relações de confiança funcionar.  

- Use uma floresta de recursos. Colocar recursos e contas de serviço em uma floresta de recurso separado, mantendo as contas de usuário em uma floresta organizacional existente. Pode ser necessário criar algumas contas de usuário alternativas na floresta de recursos para manter o acesso à floresta de recursos se a floresta organizacional ficar indisponível. As contas alternativas devem ter a autoridade necessária para fazer logon floresta de recursos e manter o controle dos recursos até que a floresta organizacional é voltar a ficar online.  

   Estabelece uma relação de confiança entre florestas organizacionais e o recurso para que os usuários possam acessar os recursos da floresta usando suas contas de usuário normal. Essa configuração permite o gerenciamento centralizado de contas de usuário, permitindo aos usuários a cair para contas alternativas na floresta de recursos se a floresta organizacional ficar indisponível.  

Considerações para isolamento de serviço incluem o seguinte:  

- Florestas criadas para o isolamento de serviço podem confiar em domínios de outras florestas, mas não devem incluir os usuários de outras florestas em seus grupos de administradores de serviço. Se os usuários de outras florestas são incluídos em grupos administrativos na floresta isolado, a segurança da floresta isolada potencialmente pode ser comprometida, pois os administradores de serviço na floresta não tem controle exclusivo.  

- Desde que os controladores de domínio estão acessíveis em uma rede, eles estão sujeitas a ataques (por exemplo, ataques de negação de serviço) de computadores na rede. Você pode fazer o seguinte para proteger contra a possibilidade de um ataque:  

   - Controladores de domínio do host somente em redes que são considerados seguros.  

   - Limitar o acesso à rede ou redes hospedando os controladores de domínio.  

- Isolamento de serviço exige a criação de uma floresta de adicional. Avalie se o custo de manter a infraestrutura para dar suporte a florestas adicionais supera os custos associados à perda de acesso aos recursos devido a uma floresta organizacional está indisponível.  

   Usuários específicos ou aplicativos podem ter necessidades especiais que exigem a capacidade de passar pelo firewall, entre em contato com outras florestas. Você pode atender a essas necessidades individualmente, abrindo as interfaces apropriadas no firewall, incluindo aqueles necessários para as relações de confiança funcionar.  

Para obter mais informações sobre como configurar firewalls para uso com o AD DS, consulte [do Active Directory em redes segmentadas por Firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  
