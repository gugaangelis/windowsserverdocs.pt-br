---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Mapeamento de requisitos de design para modelos de design de floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d65b03dc255de5523c48c2bb9359530b8e7c3167
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408769"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Mapeamento de requisitos de design para modelos de design de floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A maioria dos grupos em sua organização pode compartilhar uma única floresta organizacional gerenciada por um único grupo de tecnologia de informação (TI) e que contém as contas de usuário e os recursos para todos os grupos que compartilham a floresta. Essa floresta compartilhada, chamada de floresta organizacional inicial, é a base do modelo de design de floresta para a organização.  

Como a floresta organizacional inicial pode hospedar vários grupos na organização, o proprietário da floresta deve estabelecer contratos de nível de serviço com cada grupo para que todas as partes entendam o que é esperado. Isso protege os grupos individuais e o proprietário da floresta estabelecendo expectativas de serviço acordadas.  

Se nem todos os grupos em sua organização puderem compartilhar uma única floresta organizacional, você deverá expandir seu design de floresta para acomodar as necessidades dos diferentes grupos. Isso envolve identificar os requisitos de design que se aplicam aos grupos com base em suas necessidades de autonomia e isolamento e se eles têm ou não uma rede de conectividade limitada e, em seguida, identificando o modelo de floresta que você pode usar para acomodar esses requirement. A tabela a seguir lista os cenários de modelo de design de floresta com base nos fatores de autonomia, isolamento e conectividade. Depois de identificar o cenário de design de floresta que melhor atenda às suas necessidades, determine se você precisa tomar decisões adicionais para atender às suas especificações de design.  

> [!NOTE]  
> Se um fator estiver listado como N/A, não será uma consideração, pois outros requisitos também acomodam esse fator.  

|Cenário|Conectividade limitada|Isolamento de dados|Autonomia de dados|Isolamento de serviço|Autonomia do serviço|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Cenário 1: unir uma floresta existente para autonomia de dados](#BKMK_1)|Não|Não|Sim|Não|Não|  
|[Cenário 2: usar uma floresta organizacional ou um domínio para autonomia de serviço](#BKMK_2)|Não|Não|N/D|Não|Sim|  
|[Cenário 3: usar uma floresta organizacional ou uma floresta de recursos para o isolamento de serviço](#BKMK_3)|Não|Não|N/D|Sim|N/D|  
|[Cenário 4: usar uma floresta organizacional ou uma floresta de acesso restrito para isolamento de dados](#BKMK_4)|N/D|Sim|N/D|N/D|N/D|  
|[Cenário 5: usar uma floresta organizacional ou reconfigurar o firewall para conectividade limitada](#BKMK_5)|Sim|Não|N/D|Não|Não|  
|[Cenário 6: usar uma floresta ou domínio organizacional e reconfigurar o firewall para autonomia de serviço com conectividade limitada](#BKMK_6)|Sim|Não|N/D|Não|Sim|  
|[Cenário 7: usar uma floresta de recursos e reconfigurar o firewall para isolamento de serviço com conectividade limitada](#BKMK_7)|Sim|Não|N/D|Sim|N/D|  

## <a name="BKMK_1"></a>Cenário 1: unir uma floresta existente para autonomia de dados  

Você pode atender a um requisito de autonomia de dados simplesmente hospedando o grupo em UOs (unidades organizacionais) em uma floresta organizacional existente. Delegue o controle sobre as UOs para os administradores de dados desse grupo para alcançar a autonomia dos dados. Para obter mais informações sobre como delegar o controle usando UOs, consulte [criando um design de unidade organizacional](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Cenário 2: usar uma floresta organizacional ou um domínio para autonomia de serviço  

Se um grupo em sua organização identificar a autonomia do serviço como um requisito, recomendamos que você primeiro reconsidere esse requisito. A obtenção de autonomia de serviço cria mais sobrecarga de gerenciamento e custos adicionais para a organização. Certifique-se de que o requisito de autonomia de serviço não seja simplesmente conveniente e que você possa justificar os custos envolvidos para atender a esse requisito.  
  
Você pode atender a um requisito de autonomia de serviço seguindo um destes procedimentos:  

- Criando uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer autonomia de serviço em uma floresta organizacional separada. Atribua um indivíduo desse grupo para ser o proprietário da floresta. Se o grupo precisar acessar ou compartilhar recursos com outras florestas na organização, eles poderão estabelecer uma relação de confiança entre a floresta organizacional e as outras florestas.  

- Usando domínios organizacionais. Coloque os usuários, grupos e computadores em um domínio separado em uma floresta organizacional existente. Esse modelo fornece apenas a autonomia de serviço no nível de domínio e não para autonomia de serviço completa, isolamento de serviço ou isolamento de dados.  

Para obter mais informações sobre como usar domínios organizacionais, consulte [usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

## <a name="BKMK_3"></a>Cenário 3: usar uma floresta organizacional ou uma floresta de recursos para o isolamento de serviço  

Você pode atender a um requisito de isolamento de serviço seguindo um destes procedimentos:  

- Usando uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer o isolamento de serviço em uma floresta organizacional separada. Atribua um indivíduo desse grupo para ser o proprietário da floresta. Se o grupo precisar acessar ou compartilhar recursos com outras florestas na organização, eles poderão estabelecer uma relação de confiança entre a floresta organizacional e as outras florestas. No entanto, não recomendamos essa abordagem porque o acesso aos recursos por meio de grupos universais é muito restrito em cenários de confiança de floresta.  

- Usando uma floresta de recursos. Coloque recursos e contas de serviço em uma floresta de recursos separada, mantendo as contas de usuário em uma floresta organizacional existente. Se necessário, as contas alternativas poderão ser criadas na floresta de recursos para acessar recursos na floresta de recursos se a floresta organizacional ficar indisponível. As contas alternativas devem ter a autoridade necessária para fazer logon na floresta de recursos e manter o controle dos recursos até que a floresta organizacional esteja novamente online.  

   Estabeleça uma relação de confiança entre o recurso e as florestas organizacionais para que os usuários possam acessar os recursos na floresta ao usar suas contas de usuário regulares. Essa configuração permite o gerenciamento centralizado de contas de usuário, permitindo que os usuários façam fallback para contas alternativas na floresta de recursos se a floresta organizacional ficar indisponível.  

As considerações sobre o isolamento de serviço incluem o seguinte:

- As florestas criadas para o isolamento de serviço podem confiar em domínios de outras florestas, mas não devem incluir usuários de outras florestas em seus grupos de administradores de serviço. Se os usuários de outras florestas estiverem incluídos em grupos administrativos na floresta isolada, a segurança da floresta isolada potencialmente poderá ser comprometida porque os administradores de serviço na floresta não têm controle exclusivo.  

- Desde que os controladores de domínio estejam acessíveis em uma rede, eles estão sujeitos a ataques (como ataques de negação de serviço) de software mal-intencionado nessa rede. Você pode fazer o seguinte para se proteger contra a possibilidade de um ataque:  

   - Controladores de domínio de host somente em redes que são consideradas seguras.  

   - Limite o acesso à rede ou às redes que hospedam os controladores de domínio.  

- O isolamento de serviço requer a criação de uma floresta adicional. Avalie se o custo de manutenção da infraestrutura para dar suporte à floresta adicional supera os custos associados à perda de acesso aos recursos devido à falta de disponibilidade de uma floresta organizacional.  

## <a name="BKMK_4"></a>Cenário 4: usar uma floresta organizacional ou uma floresta de acesso restrito para isolamento de dados  

Você pode obter o isolamento de dados seguindo um destes procedimentos:  

- Usando uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer isolamento de dados em uma floresta organizacional separada. Atribua um indivíduo desse grupo para ser o proprietário da floresta. Se o grupo precisar acessar ou compartilhar recursos com outras florestas na organização, estabeleça uma relação de confiança entre a floresta organizacional e as outras florestas. Somente os usuários que precisam de acesso às informações classificadas existem na nova floresta organizacional. Os usuários têm uma conta que usam para acessar dados classificados em sua própria floresta e dados não classificados em outras florestas por meio de relações de confiança.  

- Usando uma floresta de acesso restrito. Essa é uma floresta separada que contém os dados restritos e as contas de usuário que são usadas para acessar esses dados. As contas de usuário separadas são mantidas nas florestas organizacionais existentes que são usadas para acessar os recursos irrestritos na rede. Nenhuma confiança é criada entre a floresta de acesso restrito e outras florestas na empresa. Você pode restringir ainda mais a floresta implantando a floresta em uma rede física separada, para que ela não possa se conectar a outras florestas. Se você implantar a floresta em uma rede separada, os usuários deverão ter duas estações de trabalho: uma para acessar a floresta restrita e outra para acessar as áreas não restritas da rede.  

As considerações para criar florestas para isolamento de dados incluem o seguinte:  

- As florestas organizacionais criadas para o isolamento de dados podem confiar em domínios de outras florestas, mas os usuários de outras florestas não devem ser incluídos em nenhum dos seguintes itens:  

  - Grupos responsáveis pelo gerenciamento de serviços ou grupos que podem gerenciar a associação de grupos de administradores de serviços  

  - Grupos que têm controle administrativo sobre computadores que armazenam dados protegidos  

  - Grupos que têm acesso a dados protegidos ou grupos que são responsáveis pelo gerenciamento de objetos de usuário ou objetos de grupo que têm acesso aos dados protegidos  

    Se os usuários de outra floresta estiverem incluídos em qualquer um desses grupos, um comprometimento da outra floresta poderá levar a um comprometimento da floresta isolada e à divulgação de dados protegidos.  

- Outras florestas podem ser configuradas para confiar na floresta organizacional criada para o isolamento de dados para que os usuários na floresta isolada possam acessar recursos em outras florestas. No entanto, os usuários da floresta isolada nunca devem fazer logon interativamente nas estações de trabalho na floresta confiante. O computador na floresta confiante pode ser potencialmente comprometido por software mal-intencionado e pode ser usado para capturar as credenciais de logon do usuário.  

   > [!NOTE]
   > Para impedir que os servidores em uma floresta confiante representem os usuários da floresta isolada e, em seguida, acessarem recursos na floresta isolada, o proprietário da floresta pode desabilitar a autenticação delegada ou usar o recurso de delegação restrita. Para obter mais informações sobre autenticação delegada e delegação restrita, consulte [delegando a autenticação](https://go.microsoft.com/fwlink/?LinkId=106614).  

- Talvez seja necessário estabelecer um firewall entre a floresta organizacional e as outras florestas na organização para limitar o acesso do usuário a informações fora de sua floresta.  

- Embora a criação de uma floresta separada permita o isolamento de dados, contanto que os controladores de domínio na floresta isolada e computadores que hospedam informações protegidas sejam acessíveis em uma rede, eles estão sujeitos a ataques iniciados a partir de computadores na rede. As organizações que decidem que o risco de ataque é muito alta ou que a consequência de um ataque ou violação de segurança é muito grande para limitar o acesso à rede ou às redes que estão hospedando os controladores de domínio e os computadores que hospedam dados protegidos . Limitar o acesso pode ser feito usando tecnologias como firewalls e IPsec (Internet Protocol Security). Em casos extremos, as organizações podem optar por manter os dados protegidos em uma rede independente que não tenha nenhuma conexão física com nenhuma outra rede na organização.  

   > [!NOTE]  
   > Se existir alguma conectividade de rede entre uma floresta de acesso restrito e outra rede, a possibilidade existe para que os dados na área restrita sejam transmitidos para a outra rede.  

## <a name="BKMK_5"></a>Cenário 5: usar uma floresta organizacional ou reconfigurar o firewall para conectividade limitada  

Para atender a um requisito de conectividade limitado, você pode executar um dos seguintes procedimentos:  

- Coloque os usuários em uma floresta organizacional existente e, em seguida, abra o firewall o suficiente para permitir que Active Directory tráfego passe.  

- Use uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo para o qual a conectividade é limitada em uma floresta organizacional separada. Atribua um indivíduo desse grupo para ser o proprietário da floresta. A floresta organizacional fornece um ambiente separado no outro lado do firewall. A floresta inclui contas de usuário e recursos que são gerenciados na floresta, para que os usuários não precisem passar pelo firewall para realizar suas tarefas diárias. Usuários ou aplicativos específicos podem ter necessidades especiais que exigem a capacidade de passar pelo firewall para contatar outras florestas. Você pode atender a essas necessidades individualmente abrindo as interfaces apropriadas no firewall, incluindo as necessárias para que as relações de confiança funcionem.  

Para obter mais informações sobre como configurar firewalls para uso com o Active Directory Domain Services (AD DS), consulte [Active Directory em redes segmentadas por firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_6"></a>Cenário 6: usar uma floresta ou domínio organizacional e reconfigurar o firewall para autonomia de serviço com conectividade limitada  

Se um grupo em sua organização identificar a autonomia do serviço como um requisito, recomendamos que você primeiro reconsidere esse requisito. A obtenção de autonomia de serviço cria mais sobrecarga de gerenciamento e custos adicionais para a organização. Certifique-se de que o requisito de autonomia de serviço não seja simplesmente conveniente e que você possa justificar os custos envolvidos para atender a esse requisito.  

Se a conectividade limitada for um problema e você tiver um requisito de autonomia de serviço, você poderá executar um dos seguintes procedimentos:  

- Use uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer autonomia de serviço em uma floresta organizacional separada. Atribua um indivíduo desse grupo para ser o proprietário da floresta. A floresta organizacional fornece um ambiente separado no outro lado do firewall. A floresta inclui contas de usuário e recursos que são gerenciados na floresta, para que os usuários não precisem passar pelo firewall para realizar suas tarefas diárias. Usuários ou aplicativos específicos podem ter necessidades especiais que exigem a capacidade de passar pelo firewall para contatar outras florestas. Você pode atender a essas necessidades individualmente abrindo as interfaces apropriadas no firewall, incluindo as necessárias para que as relações de confiança funcionem.  

- Coloque os usuários, grupos e computadores em um domínio separado em uma floresta organizacional existente. Esse modelo fornece apenas a autonomia de serviço no nível de domínio e não para autonomia de serviço completa, isolamento de serviço ou isolamento de dados. Outros grupos na floresta devem confiar nos administradores de serviço do novo domínio para o mesmo grau em que confiam no proprietário da floresta. Por esse motivo, não recomendamos essa abordagem. Para obter mais informações sobre como usar domínios organizacionais, consulte [usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

Você também precisa abrir o firewall o suficiente para permitir que Active Directory tráfego passe. Para obter mais informações sobre como configurar firewalls para uso com o AD DS, consulte [Active Directory em redes segmentadas por firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_7"></a>Cenário 7: usar uma floresta de recursos e reconfigurar o firewall para isolamento de serviço com conectividade limitada  

Se a conectividade limitada for um problema e você tiver um requisito para o isolamento de serviço, você poderá executar um dos seguintes procedimentos:  

- Use uma floresta organizacional. Coloque os usuários, grupos e computadores para o grupo que requer o isolamento de serviço em uma floresta organizacional separada. Atribua um indivíduo desse grupo para ser o proprietário da floresta. A floresta organizacional fornece um ambiente separado no outro lado do firewall. A floresta inclui contas de usuário e recursos que são gerenciados na floresta, para que os usuários não precisem passar pelo firewall para realizar suas tarefas diárias. Usuários ou aplicativos específicos podem ter necessidades especiais que exigem a capacidade de passar pelo firewall para contatar outras florestas. Você pode atender a essas necessidades individualmente abrindo as interfaces apropriadas no firewall, incluindo as necessárias para que as relações de confiança funcionem.  

- Use uma floresta de recursos. Coloque recursos e contas de serviço em uma floresta de recursos separada, mantendo as contas de usuário em uma floresta organizacional existente. Pode ser necessário criar algumas contas de usuário alternativas na floresta de recursos para manter o acesso à floresta de recursos se a floresta organizacional ficar indisponível. As contas alternativas devem ter a autoridade necessária para fazer logon na floresta de recursos e manter o controle dos recursos até que a floresta organizacional esteja novamente online.  

   Estabeleça uma relação de confiança entre o recurso e as florestas organizacionais para que os usuários possam acessar os recursos na floresta ao usar suas contas de usuário regulares. Essa configuração permite o gerenciamento centralizado de contas de usuário, permitindo que os usuários façam fallback para contas alternativas na floresta de recursos se a floresta organizacional ficar indisponível.  

As considerações sobre o isolamento de serviço incluem o seguinte:  

- As florestas criadas para o isolamento de serviço podem confiar em domínios de outras florestas, mas não devem incluir usuários de outras florestas em seus grupos de administradores de serviço. Se os usuários de outras florestas estiverem incluídos em grupos administrativos na floresta isolada, a segurança da floresta isolada potencialmente poderá ser comprometida porque os administradores de serviço na floresta não têm controle exclusivo.  

- Desde que os controladores de domínio estejam acessíveis em uma rede, eles estão sujeitos a ataques (como ataques de negação de serviço) de computadores na rede. Você pode fazer o seguinte para se proteger contra a possibilidade de um ataque:  

   - Controladores de domínio de host somente em redes que são consideradas seguras.  

   - Limite o acesso à rede ou às redes que hospedam os controladores de domínio.  

- O isolamento de serviço requer a criação de uma floresta adicional. Avalie se o custo de manutenção da infraestrutura para dar suporte à floresta adicional supera os custos associados à perda de acesso aos recursos devido à falta de disponibilidade de uma floresta organizacional.  

   Usuários ou aplicativos específicos podem ter necessidades especiais que exigem a capacidade de passar pelo firewall para contatar outras florestas. Você pode atender a essas necessidades individualmente abrindo as interfaces apropriadas no firewall, incluindo as necessárias para que as relações de confiança funcionem.  

Para obter mais informações sobre como configurar firewalls para uso com o AD DS, consulte [Active Directory em redes segmentadas por firewalls](https://go.microsoft.com/fwlink/?LinkId=37928).  
