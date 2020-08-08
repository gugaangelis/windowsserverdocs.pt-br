---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Determinando o número de florestas necessárias
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 66c63d36a05b525d60a58a7127ff0ffb62c635d8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87941293"
---
# <a name="determining-the-number-of-forests-required"></a>Determinando o número de florestas necessárias

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para determinar o número de florestas que você deve implantar, você precisa identificar cuidadosamente e avaliar os requisitos de isolamento e autonomia para cada grupo em sua organização e mapear esses requisitos para os modelos de design de floresta apropriados.

Ao determinar o número de florestas a serem implantadas para sua organização, considere o seguinte:

-   Os requisitos de isolamento limitam suas opções de design. Portanto, se você identificar os requisitos de isolamento, certifique-se de que os grupos realmente exigem isolamento de dados e que a autonomia de dados não seja suficiente para suas necessidades. Certifique-se de que os vários grupos em sua organização entendam claramente os conceitos de isolamento e autonomia.

-   Negociar o design pode ser um processo demorado. Pode ser difícil para os grupos chegarem a um contrato de propriedade e usos para os recursos disponíveis. Certifique-se de permitir tempo suficiente para os grupos em sua organização realizarem pesquisas adequadas para identificar suas necessidades. Defina prazos para decisões de design e obtenha consenso de todas as partes nos prazos estabelecidos.

-   Determinar o número de florestas a serem implantadas envolve o balanceamento de custos em relação aos benefícios. Um modelo de floresta única é a opção mais econômica e requer a menor quantidade de sobrecarga administrativa. Embora um grupo na organização possa preferir operações de serviço autônomos, pode ser mais econômico para a organização assinar a entrega de serviços de um grupo de ti (tecnologia de informação) centralizado e confiável. Isso permite que o grupo tenha o próprio gerenciamento de dados sem criar os custos adicionais de gerenciamento de serviços. O balanceamento de custos em relação aos benefícios pode exigir a entrada do patrocinador executivo.

    Uma única floresta é a configuração mais fácil de gerenciar. Ele permite a colaboração máxima no ambiente porque:

    -   Todos os objetos em uma única floresta são listados no catálogo global. Portanto, nenhuma sincronização entre florestas é necessária.

    -   O gerenciamento de uma infraestrutura duplicada não é necessário.

-   Não recomendamos a copropriedade de uma única floresta por duas organizações de ti separadas e autônomas. No futuro, os objetivos dos dois grupos de ti podem mudar, para que eles não possam mais aceitar o controle compartilhado.

-   Não é recomendável terceirizar a administração do serviço para mais de um parceiro externo. Organizações multinacionais que têm grupos em diferentes países ou regiões podem optar por terceirizar a administração de serviços para um parceiro externo diferente para cada país ou região. Como vários parceiros externos não podem ser isolados um do outro, as ações de um parceiro podem afetar o serviço do outro, o que dificulta a manutenção dos parceiros em seus contratos de nível de serviço.

-   Somente uma instância de um domínio de Active Directory deve existir a qualquer momento. A Microsoft não oferece suporte à clonagem, divisão ou cópia de controladores de domínio de um domínio em uma tentativa de estabelecer uma segunda instância do mesmo domínio. Para obter mais informações sobre essa limitação, consulte a seção a seguir.

## <a name="restructuring-limitations"></a>Limitações de reestruturação
Quando uma empresa adquire outra empresa, unidade de negócios ou linha de produtos, a empresa de compra também pode querer adquirir os ativos de ti correspondentes do vendedor. Especificamente, o comprador pode querer adquirir alguns ou todos os controladores de domínio que hospedam as contas de usuário, contas de computador e grupos de segurança que correspondem aos ativos de negócios que devem ser adquiridos. Os únicos métodos com suporte para o comprador adquirir os ativos de ti armazenados na floresta Active Directory do vendedor são os seguintes:

1.  Adquira a única instância da floresta, incluindo todos os controladores de domínio e dados de diretório na floresta inteira do vendedor.

2.  Migre os dados de diretório necessários da floresta ou dos domínios do vendedor para um ou mais dos domínios do comprador. O destino para tal migração pode ser uma floresta totalmente nova ou um ou mais domínios existentes que já estão implantados na floresta do comprador.

Essa limitação de suporte existe porque:

-   Cada domínio em uma floresta Active Directory é atribuído a uma identidade exclusiva durante a criação da floresta. A cópia de controladores de domínio de um domínio original para um domínio clonado compromete a segurança dos domínios e da floresta. As ameaças ao domínio original e ao domínio clonado incluem o seguinte:

    -   Compartilhamento de senhas que podem ser usadas para obter acesso aos recursos

    -   Informações sobre grupos e contas de usuário com privilégios

    -   Mapeamento de endereços IP para nomes de computador

    -   Adições, exclusões e modificações de informações de diretório se os controladores de domínio em um domínio clonado estabelecerem conectividade de rede com controladores de domínio do domínio original

-   Os domínios clonados compartilham uma identidade de segurança comum; Portanto, as relações de confiança não podem ser estabelecidas entre elas, mesmo que um ou ambos os domínios tenham sido renomeados.

## <a name="in-this-section"></a>Nesta seção

-   [Modelos de design de floresta](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770439(v=ws.10))

-   [Mapeamento de requisitos de design para modelos de design de floresta](Forest-Design-Models.md)

-   [Usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)

