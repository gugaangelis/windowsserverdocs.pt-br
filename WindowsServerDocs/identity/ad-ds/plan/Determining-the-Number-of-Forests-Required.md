---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Determinando o número de florestas necessárias
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1721190bf592b6f7a1274d60d47bbc755eeff1c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820647"
---
# <a name="determining-the-number-of-forests-required"></a>Determinando o número de florestas necessárias

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para determinar o número de florestas que você deve implantar, você precisa cuidadosamente identificar e avaliar os requisitos de isolamento e autonomia para cada grupo em sua organização e mapear esses requisitos para os modelos de design de floresta apropriada.  
  
Ao determinar o número de florestas ao implantar para sua organização, considere o seguinte:  
  
-   Requisitos de isolamento de limitam suas escolhas de design. Portanto, se você identificar os requisitos de isolamento, certifique-se de que os grupos, na verdade, exigem o isolamento de dados e que a autonomia de dados não é suficiente para suas necessidades. Certifique-se de que os vários grupos em sua organização entender claramente os conceitos de autonomia e isolamento.  
  
-   Negociar o design pode ser um processo demorado. Ele pode ser difícil para os grupos para chegar a um acordo sobre a propriedade e usa para os recursos disponíveis. Certifique-se de que você fornecer tempo suficiente para os grupos em sua organização para conduzir pesquisas adequadas para identificar suas necessidades. Defina prazos firme para decisões de design e obtenha um consenso de todas as partes sobre os prazos estabelecidos.  
  
-   Determinar o número de florestas ao implantar envolve custos em relação a benefícios de balanceamento. Um modelo de floresta única é a opção mais econômica e exige o mínimo de sobrecarga administrativa. Embora um grupo na organização pode preferir as operações de serviço autônoma, pode ser mais econômico para a organização inscrever-se à entrega de serviços de um grupo de TI (tecnologia) de informação centralizados e confiável. Isso permite que o grupo de gerenciamento de dados próprio, sem criar os custos do gerenciamento de serviços. Os custos em relação a benefícios de balanceamento pode exigir entradas do Patrocinador executivo.  
  
    Uma única floresta é a configuração mais fácil de gerenciar. Ele permite a colaboração máximo dentro do ambiente porque:  
  
    -   Todos os objetos em uma única floresta estão listados no catálogo global. Portanto, nenhuma sincronização entre florestas é necessária.  
  
    -   Gerenciamento de uma infra-estrutura duplicado não é necessário.  
  
-   Não recomendamos co-ownership de uma única floresta por duas organizações de TI autônomas e separadas. No futuro, as metas dos dois grupos de TI podem ser alteradas, para que eles não podem aceitar controle compartilhado.  
  
-   Não recomendamos a administração de serviços de terceirização a mais de um fora do parceiro. Organizações multinacionais que têm grupos em diferentes países ou regiões podem optar por terceirizar a administração do serviço a um parceiro externo diferente para cada país ou região. Como vários parceiros externos não podem ser isolados uns dos outros, as ações de um parceiro podem afetar o serviço do outro, o que torna difícil manter os parceiros responsável com relação a seus contratos de nível de serviço.  
  
-   Apenas uma instância de um domínio do Active Directory deve existir a qualquer momento. Microsoft não oferece suporte a clonagem, divisão ou copiando controladores de domínio de um domínio em uma tentativa de estabelecer uma segunda instância do mesmo domínio. Para obter mais informações sobre essa limitação, consulte a seção a seguir.  
  
## <a name="restructuring-limitations"></a>Limitações de reestruturação  
Quando uma empresa adquire outra empresa, unidade de negócios, ou linha de produto, a empresa compra também poderá adquirir correspondentes ativos de TI do vendedor. Especificamente, o comprador talvez queira adquirir alguns ou todos os controladores de domínio que hospedam as contas de usuário, contas de computador e grupos de segurança que correspondem aos ativos de negócios que devem ser adquiridos. Os únicos métodos com suporte para o comprador adquirir os ativos de TI que estão armazenados na floresta do Active Directory do vendedor são da seguinte maneira:  
  
1.  Adquira a única instância da floresta, incluindo todos os controladores de domínio e dados do diretório em toda a floresta do vendedor.  
  
2.  Migre os dados de diretório necessários de domínios ou florestas do vendedor para um ou mais dos domínios do comprador. O destino para essa migração pode ser uma totalmente nova floresta ou um ou mais domínios existentes que já foram implantados na floresta do comprador.  
  
Essa limitação de suporte existe porque:  
  
-   Cada domínio em uma floresta do Active Directory é atribuído a uma identidade exclusiva durante a criação da floresta. Copiando controladores de domínio de um domínio original para um comprometimento de domínio clonado a segurança de domínios e a floresta. Ameaças para o domínio original e o domínio clonado incluem o seguinte:  
  
    -   Compartilhamento de senhas que podem ser usadas para obter acesso aos recursos  
  
    -   Informações sobre grupos e contas de usuário com privilégios  
  
    -   Mapeamento de endereços IP para nomes de computador  
  
    -   Adições, exclusões e modificações de informações de diretório se os controladores de domínio em um domínio clonado nunca estabelecer conectividade de rede com controladores de domínio do domínio original  
  
-   Domínios clonados compartilham uma identidade de segurança comum; Portanto, as relações de confiança não são estabelecidas entre elas, mesmo se um ou ambos os domínios foram renomeados.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Modelos de Design de floresta](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Requisitos de mapeamento de Design para modelos de Design de floresta](Forest-Design-Models.md)  
  
-   [Usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


