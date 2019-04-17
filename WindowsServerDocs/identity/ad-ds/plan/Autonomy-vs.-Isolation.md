---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Isolamento de autonomia vs.
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1cd852eabf1391dbf40cee7b354f3f9138adae0b
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="autonomy-vs-isolation"></a>Isolamento de autonomia vs.

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode criar a estrutura lógica do Active Directory para alcançar um destes procedimentos:  
  
-   **Autonomia**. Envolve um controle independente, mas não exclusivo de um recurso. Quando você conseguir autonomia, os administradores têm autoridade para gerenciar recursos de forma independente; No entanto, os administradores com maior autoridade existem que também têm controle sobre esses recursos e pode assumir o controle imediatamente se necessário. Você pode criar a estrutura lógica do Active Directory para obter os seguintes tipos de autonomia:  
  
    -   **Serviço autonomia**. Esse tipo de autonomia envolve controlo todo ou parte do gerenciamento de serviços.  
  
    -   **Autonomia de dados**. Esse tipo de autonomia envolve controle sobre todos ou parte dos dados armazenados no diretório ou em computadores membro associados ao diretório.  
  
-   **Isolamento**. Envolve um controle independente e exclusivo de um recurso. Quando você atingir o isolamento, os administradores têm autoridade para gerenciar um recurso de forma independente, e nenhum outro administrador pode controlar distância do recurso. Você pode criar a estrutura lógica do Active Directory para obter os seguintes tipos de isolamento:  
  
    -   **Isolamento de serviço**. Impede que os administradores (em vez dos administradores que são especificamente designados para controlar o gerenciamento de serviço) controlar ou interferir em gerenciamento de serviço.  
  
    -   **O isolamento de dados**. Impede que os administradores (em vez dos administradores que são especificamente designados para dados de controle ou modo de exibição) controlar ou exibir um subconjunto dos dados no diretório ou em computadores membro associados ao diretório.  
  
Os administradores que exigem apenas autonomia aceitam que outros administradores que têm autoridade administrativa igual ou maior igual ou maior controle sobre o gerenciamento de dados ou serviço. Os administradores que exigem o isolamento de ter exclusivo controle sobre o gerenciamento de dados ou serviço. Criar um design para alcançar autonomia é geralmente mais barata que criar um design para alcançar o isolamento.  
  
Nos Serviços Active Directory domínio (AD DS), os administradores podem delegar a administração de serviço e administração de dados para conseguirem autonomia ou isolamento entre organizações. A combinação de gerenciamento de serviço, requisitos de gerenciamento, autonomia e isolamento de dados de uma organização afetam os contêineres do Active Directory que são usados para delegar a administração.  
  
## <a name="isolation-and-autonomy-requirements"></a>Requisitos de autonomia e isolamento  
O número de florestas que você precisa para implantar baseia-se nos requisitos de autonomia e isolamento de cada grupo em sua organização. Para identificar seus requisitos de design de floresta, você deve identificar os requisitos de autonomia e isolamento para todos os grupos em sua organização. Especificamente, você deve identificar a necessidade de isolamento de dados, autonomia de dados, o isolamento de serviço e autonomia de serviço. Você também deve identificar áreas de conectividade limitada em sua organização.  
  
### <a name="data-isolation"></a>Isolamento de dados  
O isolamento de dados envolve exclusivo controle sobre dados pelo grupo ou organização que possui os dados. É importante observar que os administradores de serviço têm a capacidade de assumir o controle de um recurso de administradores de dados. E os administradores de dados não têm a capacidade de impedir que os administradores de serviços de acessar os recursos que eles controlam. Portanto, você não consegue isolamento de dados quando outro grupo dentro da organização é responsável por administração de serviço. Se um grupo exigir o isolamento de dados, esse grupo também deve assumir responsabilidade para administração de serviço.  
  
Como os dados armazenados no AD DS e em computadores que tenha ingressados no AD DS não podem ser isolados de administradores de serviços, é a única maneira de um grupo dentro de uma organização de atingir o isolamento de dados completo criar uma floresta separada para esses dados. As organizações para que as consequências de um ataque por software mal-intencionado ou por um administrador de serviço forçado são significativas poderá optar por criar uma floresta separada para alcançar o isolamento de dados. Requisitos legais normalmente criam uma necessidade para esse tipo de isolamento de dados. Por exemplo:  
  
-   Uma instituição financeira é exigida por lei para limitar o acesso a dados que pertencem aos clientes em uma jurisdição em particular para usuários, computadores e administradores localizados nessa jurisdição. Embora a instituição confie administradores de serviços que funcionam fora da área protegida, se a limitação de acesso for violada, instituição não estará mais capaz de fazer negócios nessa jurisdição. Portanto, a instituição financeira deve isolar os dados dos administradores de serviço fora jurisdição. Observe que a criptografia não é sempre uma alternativa para essa solução. Criptografia não pode proteger dados de administradores de serviços.  
  
-   Um empreiteiro defesa é exigido por lei para limitar o acesso aos dados de projeto para um conjunto especificado de usuários. Embora o empreiteiro confie administradores de serviço que sistemas de computador relacionados aos outros projetos de controle, uma violação dessa limitação de acesso fará com que o empreiteiro perder negócios.  
  
    > [!NOTE]  
    > Se você tiver um requisito de isolamento de dados, você deve decidir se você precisa isolar seus dados de administradores de serviços ou de dados administradores e usuários comuns. Se o requisito de isolamento é baseado em isolamento de dados administradores e usuários comuns, você pode usar listas de controle de acesso (ACLs) para isolar os dados. Para os fins deste processo de design, o isolamento de dados administradores e usuários comuns não é considerado um requisito de isolamento de dados.  
  
### <a name="data-autonomy"></a>Autonomia de dados  
Autonomia de dados envolve a capacidade de um grupo ou a organização de gerenciar seus próprios dados, incluindo tomar decisões administrativas sobre os dados e realizar tarefas administrativas necessárias sem a necessidade de aprovação de autoridade de outra.  
  
Autonomia de dados não impede que os administradores de serviços na floresta acessar os dados. Por exemplo, um grupo de pesquisa em uma organização de grande pode deseja ser capaz de gerenciar seus dados específicos do projeto propriamente ditos, mas não é necessário proteger os dados de outros administradores na floresta.  
  
### <a name="service-isolation"></a>Isolamento de serviço  
Isolamento de serviço envolve controle exclusivo da infraestrutura do Active Directory. Grupos que exigem o isolamento de serviço exigem que nenhum administrador fora do grupo pode interferir com a operação do serviço de diretório.  
  
Requisitos legais ou operacionais normalmente criam uma necessidade de isolamento de serviço. Por exemplo:  
  
-   Uma empresa de fabricação tem um aplicativo crítico que controla o equipamento no chão de fábrica. Interrupções no serviço em outras partes da rede da organização não podem ser permitidas para interferir com a operação do chão de fábrica.  
  
-   Uma empresa de hospedagem fornece serviços para vários clientes. Cada cliente requer o isolamento de serviço para que qualquer interrupção do serviço que afeta um cliente não afeta os outros clientes.  
  
### <a name="service-autonomy"></a>Autonomia de serviço  
Autonomia de serviço envolve a capacidade de gerenciar a infraestrutura sem um requisito para o controle exclusivo; Por exemplo, quando um grupo deseja fazer alterações para a infraestrutura (por exemplo, adicionando ou removendo domínios, modificando o namespace do sistema de nome de domínio (DNS) ou modificar o esquema) sem a aprovação do proprietário floresta.  
  
Autonomia de serviço pode ser necessária dentro de uma organização para um grupo que deseja ser capaz de controlar o nível de serviço do AD DS (, adicionando e removendo controladores de domínio, conforme necessário) ou para um grupo que precisa ser capaz de instalar aplicativos habilitados para diretórios que exigem extensões de esquema.  
  
## <a name="limited-connectivity"></a>Conectividade limitada  
Se um grupo em sua organização possui redes que são separados por dispositivos que restringem ou limitam a conectividade entre redes (por exemplo, firewalls e dispositivos de conversão de endereços de rede (NAT)), isso pode afetar o design de floresta. Quando você identifica seus requisitos de design de floresta, certifique-se de observar os locais onde você tiver conectividade limitada de rede. Essas informações são necessárias para que você possa tomar decisões sobre o design de floresta.  
  


