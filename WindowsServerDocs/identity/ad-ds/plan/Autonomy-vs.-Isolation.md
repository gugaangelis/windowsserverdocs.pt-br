---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomia x isolamento
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: eccbfc7767821a15d32d6aabef156861cb409f40
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941206"
---
# <a name="autonomy-vs-isolation"></a>Autonomia x isolamento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode criar sua estrutura de Active Directory lógica para obter um dos seguintes:

-   **Autonomia**. Envolve o controle independente, mas não exclusivo de um recurso. Quando você alcança autonomia, os administradores têm autoridade para gerenciar recursos de forma independente; no entanto, os administradores com maior autoridade existem que também têm controle sobre esses recursos e podem assumir o controle, se necessário. Você pode criar sua estrutura de Active Directory lógica para obter os seguintes tipos de autonomia:

    -   **Autonomia do serviço**. Esse tipo de autonomia envolve o controle sobre todo ou parte do gerenciamento de serviços.

    -   **Autonomia de dados**. Esse tipo de autonomia envolve o controle sobre todos ou parte dos dados armazenados no diretório ou em computadores membros que ingressaram no diretório.

-   **Isolamento**. Envolve o controle independente e exclusivo de um recurso. Quando você atinge o isolamento, os administradores têm autoridade para gerenciar um recurso de forma independente, e nenhum outro administrador pode retirar o controle do recurso. Você pode projetar sua estrutura lógica de Active Directory para obter os seguintes tipos de isolamento:

    -   **Isolamento de serviço**. Impede que os administradores (que não sejam os administradores especificamente designados para controlar o gerenciamento de serviços) controlem ou interfiram no gerenciamento de serviços.

    -   **Isolamento de dados**. Impede que os administradores (que não sejam os administradores especificamente designados para controlar ou exibir dados) controlem ou exibam um subconjunto de dados no diretório ou em computadores membros que ingressaram no diretório.

Os administradores que precisam apenas de autonomia aceitam que outros administradores com autoridade administrativa igual ou maior tenham controle igual ou maior sobre o gerenciamento de serviços ou de dados. Os administradores que precisam de isolamento têm controle exclusivo sobre o gerenciamento de serviços ou de dados. Criar um design para atingir a autonomia é geralmente menos caro do que criar um design para atingir o isolamento.

No Active Directory Domain Services (AD DS), os administradores podem delegar a administração de serviços e a administração de dados para atingir a autonomia ou o isolamento entre as organizações. A combinação de gerenciamento de serviços, gerenciamento de dados, autonomia e requisitos de isolamento de uma organização Impacta os contêineres de Active Directory que são usados para delegar a administração.

## <a name="isolation-and-autonomy-requirements"></a>Requisitos de isolamento e autonomia
O número de florestas que você precisa implantar baseia-se nos requisitos de autonomia e isolamento de cada grupo dentro de sua organização. Para identificar os requisitos de design da floresta, você deve identificar os requisitos de autonomia e isolamento para todos os grupos em sua organização. Especificamente, você deve identificar a necessidade de isolamento de dados, autonomia de dados, isolamento de serviço e autonomia de serviço. Você também deve identificar áreas de conectividade limitada em sua organização.

### <a name="data-isolation"></a>Isolamento dos dados
O isolamento de dados envolve o controle exclusivo sobre os dados pelo grupo ou pela organização que possui os dados. É importante observar que os administradores de serviço têm a capacidade de assumir o controle de um recurso de administradores de dados. E os administradores de dados não têm a capacidade de impedir que os administradores de serviço acessem os recursos que eles controlam. Portanto, você não pode obter isolamento de dados quando outro grupo dentro da organização é responsável pela administração do serviço. Se um grupo exigir isolamento de dados, esse grupo também deverá assumir a responsabilidade pela administração do serviço.

Como os dados armazenados em AD DS e em computadores ingressados no AD DS não podem ser isolados dos administradores de serviço, a única maneira de um grupo dentro de uma organização atingir o isolamento de dados completo é criar uma floresta separada para esses dados. Organizações para as quais as consequências de um ataque por software mal-intencionado ou por um administrador de serviço forçado são substanciais podem optar por criar uma floresta separada para obter isolamento de dados. Normalmente, os requisitos legais criam uma necessidade desse tipo de isolamento de dados. Por exemplo:

-   Uma instituição financeira é exigida por lei para limitar o acesso a dados que pertencem a clientes em uma jurisdição específica para usuários, computadores e administradores localizados nessa jurisdição. Embora a instituição confie nos administradores de serviço que trabalham fora da área protegida, se a limitação de acesso for violada, a instituição não poderá mais fazer negócios nessa jurisdição. Portanto, a instituição financeira deve isolar os dados dos administradores de serviço fora dessa jurisdição. Observe que a criptografia nem sempre é uma alternativa para essa solução. A criptografia pode não proteger dados de administradores de serviço.

-   Um prestador de defesa é exigido por lei para limitar o acesso a dados do projeto a um conjunto especificado de usuários. Embora o prestador de serviços confie nos administradores de serviço que controlam os sistemas de computador relacionados a outros projetos, uma violação dessa limitação de acesso fará com que o prestador perca negócios.

    > [!NOTE]
    > Se você tiver um requisito de isolamento de dados, deverá decidir se precisa isolar seus dados de administradores de serviço ou de administradores de dados e usuários comuns. Se seu requisito de isolamento for baseado no isolamento de administradores de dados e usuários comuns, você poderá usar ACLs (listas de controle de acesso) para isolar os dados. Para os fins deste processo de design, o isolamento de administradores de dados e usuários comuns não é considerado um requisito de isolamento de dados.

### <a name="data-autonomy"></a>Autonomia de dados
A autonomia de dados envolve a capacidade de um grupo ou organização de gerenciar seus próprios dados, incluindo tomar decisões administrativas sobre os dados e executar qualquer tarefa administrativa necessária sem a necessidade de aprovação de outra autoridade.

A autonomia de dados não impede que os administradores de serviço na floresta acessem os dados. Por exemplo, um grupo de pesquisa em uma grande organização pode querer ser capaz de gerenciar seus próprios dados específicos do projeto, mas não precisa proteger os dados de outros administradores na floresta.

### <a name="service-isolation"></a>Isolamento de serviço
O isolamento de serviço envolve o controle exclusivo da infraestrutura de Active Directory. Os grupos que exigem o isolamento de serviço exigem que nenhum administrador fora do grupo possa interferir na operação do serviço de diretório.

Os requisitos operacionais ou legais normalmente criam uma necessidade de isolamento de serviço. Por exemplo:

-   Uma empresa de manufatura tem um aplicativo crítico que controla equipamentos no chão de fábrica. As interrupções no serviço em outras partes da rede da organização não podem ter permissão para interferir na operação do piso de fábrica.

-   Uma empresa de hospedagem fornece serviço a vários clientes. Cada cliente requer o isolamento de serviço para que qualquer interrupção de serviço que afete um cliente não afete os outros clientes.

### <a name="service-autonomy"></a>Autonomia do serviço
A autonomia do serviço envolve a capacidade de gerenciar a infraestrutura sem necessidade de controle exclusivo; por exemplo, quando um grupo deseja fazer alterações na infraestrutura (como adicionar ou remover domínios, modificar o namespace do sistema de nomes de domínio (DNS) ou modificar o esquema) sem a aprovação do proprietário da floresta.

A autonomia do serviço pode ser necessária em uma organização para um grupo que deseja ser capaz de controlar o nível de serviço de AD DS (adicionando e removendo controladores de domínio, conforme necessário) ou para um grupo que precisa ser capaz de instalar aplicativos habilitados para diretório que exigem extensões de esquema.

## <a name="limited-connectivity"></a>Conectividade limitada
Se um grupo em sua organização possuir redes que são separadas por dispositivos que restringem ou limitam a conectividade entre redes (como firewalls e dispositivos NAT), isso pode afetar o design da floresta. Ao identificar os requisitos de design de sua floresta, lembre-se de anotar os locais em que você tem conectividade de rede limitada. Essas informações são necessárias para permitir que você tome decisões sobre o design da floresta.



