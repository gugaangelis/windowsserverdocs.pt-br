---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomia vs. Isolamento
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 765a25d3d1ffdb4df473e1fb5bb65e532934aca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867567"
---
# <a name="autonomy-vs-isolation"></a>Autonomia vs. Isolamento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode criar a estrutura lógica do Active Directory para atingir um destes procedimentos:  
  
-   **Autonomia**. Envolve o controle independente, mas não exclusivo de um recurso. Quando você alcançar a autonomia, os administradores têm autoridade para gerenciar recursos de forma independente; No entanto, os administradores com autoridade maior existem que também tem controle sobre esses recursos e pode assumir o controle imediatamente se necessário. Você pode criar a estrutura lógica do Active Directory para obter os seguintes tipos de autonomia:  
  
    -   **A autonomia do serviço**. Esse tipo de autonomia envolve o controle sobre todo ou parte do gerenciamento de serviços.  
  
    -   **Autonomia de dados**. Esse tipo de autonomia envolve o controle sobre todo ou parte dos dados armazenados no diretório ou em computadores de membro associados para o diretório.  
  
-   **Isolamento**. Envolve o controle exclusivo e independente de um recurso. Quando você obter o isolamento, os administradores têm autoridade para gerenciar um recurso de forma independente e nenhum outro administrador pode eliminar o controle do recurso. Você pode criar a estrutura lógica do Active Directory para obter os seguintes tipos de isolamento:  
  
    -   **Isolamento de serviço**. Impede que os administradores (exceto os administradores que são projetados especificamente para controlar o gerenciamento de serviço) de controle ou interferindo com o gerenciamento de serviços.  
  
    -   **Isolamento de dados**. Impede que os administradores (exceto os administradores que são projetados especificamente para dados de controle ou exibição) controlar ou exibir um subconjunto dos dados no diretório ou em computadores de membro associados para o diretório.  
  
Os administradores que exigem apenas autonomia aceitam que outros administradores com autoridade administrativa igual ou maior tem igual ou maior controle sobre o gerenciamento de dados ou serviço. Os administradores que requerem isolamento de ter controle exclusivo sobre o gerenciamento de dados ou serviço. Criar um design de obter autonomia é geralmente mais barato do que criar um design para obter o isolamento.  
  
No Active Directory Domain Services (AD DS), os administradores podem delegar administração de serviços e administração de dados para alcançar a autonomia ou isolamento entre organizações. A combinação do gerenciamento de serviços, requisitos de isolamento, autonomia e gerenciamento de dados de uma organização afetam os contêineres do Active Directory que são usados para delegar a administração.  
  
## <a name="isolation-and-autonomy-requirements"></a>Requisitos de isolamento e autonomia  
O número de florestas que você precisa implantar baseia-se nos requisitos de autonomia e isolamento de cada grupo em sua organização. Para identificar seus requisitos de design de floresta, você deve identificar os requisitos de autonomia e isolamento para todos os grupos em sua organização. Especificamente, você deve identificar a necessidade de isolamento de dados, autonomia de dados, o isolamento de serviço e a autonomia do serviço. Você também deve identificar áreas de conectividade limitada na sua organização.  
  
### <a name="data-isolation"></a>Isolamento de dados  
Isolamento de dados envolve o controle exclusivo sobre dados por grupo ou organização que possui os dados. É importante observar que os administradores de serviço tem a capacidade de assumir o controle de um recurso de administradores de dados. E os administradores de dados não têm a capacidade de impedir que os administradores de serviço acessem os recursos que eles controlam. Portanto, você não conseguir obter o isolamento de dados quando o outro grupo dentro da organização é responsável pela administração do serviço. Se precisar de um grupo de isolamento de dados, esse grupo também deve assumir a responsabilidade para administração de serviço.  
  
Como os dados armazenados no AD DS e em computadores que ingressaram no AD DS não podem ser isolados de administradores de serviço, a única maneira de um grupo dentro de uma organização para obter o isolamento de dados completo é criar uma floresta separada para os dados. As organizações para o qual as consequências de um ataque por software mal-intencionado ou por um administrador de serviço forçado são substanciais podem optar por criar uma floresta separada para obter o isolamento de dados. Requisitos legais normalmente criam uma necessidade para esse tipo de isolamento de dados. Por exemplo:   
  
-   Uma instituição financeira é exigida por lei para limitar o acesso a dados que pertencem a clientes em uma jurisdição específica para os usuários, computadores e administradores localizados em dessa jurisdição. Embora a instituição confia administradores de serviço que trabalham fora da área protegida, se a limitação de acesso for violada, a instituição não poderá fazer negócios em que jurisdição. Portanto, a instituição financeira deve isolar os dados dos administradores de serviço fora dessa jurisdição. Observe que a criptografia nem sempre é uma alternativa para essa solução. A criptografia não pode proteger dados contra administradores de serviço.  
  
-   Para limitar o acesso aos dados do projeto para um conjunto específico de usuários, um prestador de serviço de defesa é exigido por lei. Embora o prestador de serviço confia administradores de serviço que controlam os sistemas de computador relacionados a outros projetos, uma violação dessa limitação de acesso fará com que o prestador para perder o negócio.  
  
    > [!NOTE]  
    > Se você tiver um requisito de isolamento de dados, você deve decidir se é necessário isolar seus dados de administradores de serviço ou de administradores de dados e usuários comuns. Se o seu requisito de isolamento se baseia em isolamento de administradores de dados e usuários comuns, você pode usar listas de controle de acesso (ACLs) para isolar os dados. Para os fins desse processo de design, o isolamento de usuários comuns e de administradores de dados não é considerado um requisito de isolamento de dados.  
  
### <a name="data-autonomy"></a>Autonomia de dados  
Autonomia de dados envolve a capacidade de gerenciar seus próprios dados, incluindo a tomada de decisões administrativas sobre os dados e executar quaisquer tarefas administrativas sem a necessidade de aprovação de outra autoridade de um grupo ou organização.  
  
Autonomia de dados não impede que os administradores de serviço na floresta acessem os dados. Por exemplo, um grupo de pesquisa em uma empresa grande pode deseja ser capaz de gerenciar seus dados específicos do projeto em si, mas não é necessário para proteger os dados de outros administradores da floresta.  
  
### <a name="service-isolation"></a>Isolamento de serviço  
Isolamento de serviço envolve o controle exclusivo da infra-estrutura do Active Directory. Grupos que exigem isolamento de serviço exigem que nenhum administrador fora do grupo pode interferir na operação do serviço de diretório.  
  
Requisitos legais ou operacionais normalmente criam a necessidade de isolamento de serviço. Por exemplo:   
  
-   Uma empresa de fabricação tem um aplicativo crítico que controla o equipamento no chão de fábrica. Interrupções no serviço em outras partes da rede da organização não podem ser permitidas para interferir na operação de Chão de fábrica.  
  
-   Uma empresa de hospedagem fornece serviço para vários clientes. Cada cliente requer isolamento de serviço para que nenhuma interrupção de serviço que afeta um cliente não afeta os outros clientes.  
  
### <a name="service-autonomy"></a>Autonomia do serviço  
A autonomia do serviço envolve a capacidade de gerenciar a infraestrutura sem um requisito para o controle exclusivo; Por exemplo, quando um grupo deseja fazer alterações na infraestrutura (como adicionar ou remover domínios, modificando o namespace do sistema de nome de domínio (DNS) ou a modificação do esquema) sem a aprovação do proprietário de floresta.  
  
A autonomia do serviço pode ser necessária dentro de uma organização para um grupo que deseja ser capaz de controlar o nível de serviço do AD DS (adicionando e removendo os controladores de domínio, conforme necessário) ou para um grupo que precisa ser capaz de instalar aplicativos habilitados por diretório que exigem extensões de esquema.  
  
## <a name="limited-connectivity"></a>Conectividade limitada  
Se um grupo em sua organização possui redes são separadas por dispositivos que restringem ou limitam a conectividade entre redes (por exemplo, firewalls e dispositivos de conversão de endereço de rede (NAT)), isso pode afetar o design de floresta. Ao identificar seus requisitos de design de floresta, certifique-se de observar os locais onde são limitadas a conectividade de rede. Essas informações são necessárias para permitir que você a tomar decisões sobre o design de floresta.  
  


