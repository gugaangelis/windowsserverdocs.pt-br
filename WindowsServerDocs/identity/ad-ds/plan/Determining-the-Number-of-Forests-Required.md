---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: "Determinando o número de florestas necessária"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a461dbb2b5bf9d2ca1bb6a336cb11ace775fb1cd
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-number-of-forests-required"></a>Determinando o número de florestas necessária

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para determinar o número de florestas que deve ser implantada, você precisa cuidadosamente identificar os requisitos de isolamento e autonomia para cada grupo em sua organização e avaliar esses requisitos são mapeadas para os modelos de design de floresta apropriada.  
  
Ao determinar o número de florestas para implantar para sua organização, considere o seguinte:  
  
-   Requisitos de isolamento limitam suas escolhas de design. Portanto, se você identificar requisitos de isolamento, certifique-se de que os grupos, na verdade, exigem o isolamento de dados e autonomia de dados não é suficiente para suas necessidades. Certifique-se de que os vários grupos em sua organização claramente entender os conceitos de autonomia e isolamento.  
  
-   Negociar o design pode ser um processo demorado. Ele pode ser difícil para grupos para vêm de um contrato sobre propriedade e usa para recursos disponíveis. Certifique-se de que você permite tempo suficiente para os grupos em sua organização conduza uma pesquisa adequada para identificar suas necessidades. Definir prazos firma para decisões de design e obtenha consenso de todas as partes nos prazos estabelecidos.  
  
-   Determinar o número de florestas para implantar envolve equilibrar custos contra benefícios. Um modelo de floresta única é a opção mais econômica e requer o mínimo de sobrecarga administrativa. Embora um grupo na organização talvez prefira operações do serviço autônomo, pode ser mais econômico para a organização inscrever-se ao fornecimento de serviços de um grupo de informática informações centralizado e confiável. Isso permite que o grupo de gerenciamento de dados próprios sem criar os custos do gerenciamento de serviços adicionados. Equilibrar custos contra benefícios pode exigir a entrada de patrocinador executivo.  
  
    Uma única floresta é a configuração mais fácil de gerenciar. Ele permite a colaboração máxima dentro do ambiente porque:  
  
    -   Todos os objetos em uma única floresta estão listados no catálogo global. Portanto, é necessária nenhuma sincronização entre florestas.  
  
    -   Gerenciamento de uma infraestrutura duplicado não é necessário.  
  
-   Não recomendamos co-ownership de uma única floresta pelas duas organizações de TI separado e autônomas. No futuro, as metas dos dois grupos de TI podem mudar, para que eles não possam aceitar controle compartilhado.  
  
-   Não recomendamos a administração de serviço terceirizados a mais de um fora do parceiro. Organizações multinacionais que têm grupos em diferentes países ou regiões podem optar por terceirizar a administração de serviço para outro parceiro externa para cada país ou região. Como vários parceiros externos não podem ser isolados uns dos outros, as ações de um parceiro podem afetar o serviço do outro, o que torna difícil manter os parceiros responsáveis para seus contratos de nível de serviço.  
  
-   Somente uma instância de um domínio do Active Directory deve existir a qualquer momento. Microsoft não oferece suporte a clonagem, dividindo ou cópia controladores de domínio de um domínio em uma tentativa de estabelecer uma segunda instância do mesmo domínio. Para saber mais sobre essa limitação, consulte a seção a seguir.  
  
## <a name="restructuring-limitations"></a>Limitações reestruturação  
Quando uma empresa adquire outra empresa, unidade de negócios, ou linha de produto, a empresa compra também pode querer adquirir ativos de TI correspondentes do vendedor. Especificamente, o comprador talvez queira adquirir alguns ou todos os controladores de domínio que hospedam as contas de usuário, contas de computador e grupos de segurança que correspondem aos ativos de negócios que devem ser adquirido. Os únicos métodos com suporte para o comprador adquirir os ativos de TI que são armazenados na floresta do Active Directory do vendedor são da seguinte maneira:  
  
1.  Adquira a única instância da floresta, incluindo todos os controladores de domínio e dados do diretório em toda a floresta do vendedor.  
  
2.  Migre os dados necessários do diretório da floresta ou domínios do vendedor para um ou mais dos domínios do comprador. O destino para tal uma migração pode ser um inteiramente nova floresta ou um ou mais domínios existentes que já são implantados na floresta do comprador.  
  
Essa limitação de suporte existe porque:  
  
-   Cada domínio em uma floresta do Active Directory é atribuído a uma identidade exclusiva durante a criação da floresta. Copiar controladores de domínio de um domínio original para um comprometimentos domínio clonados a segurança dos domínios e da floresta. Ameaças ao domínio original e o domínio clonado incluem o seguinte:  
  
    -   O compartilhamento de senhas que podem ser usadas para obter acesso aos recursos  
  
    -   Opiniões relacionadas a contas de usuário privilegiado e grupos  
  
    -   Mapeamento de endereços IP para nomes de computador  
  
    -   Adições, exclusões e modificações de informações de diretório se os controladores de domínio em um domínio clonado nunca estabelecem conectividade de rede com controladores de domínio do domínio original  
  
-   Domínios clonados compartilham uma identidade de segurança comuns; Portanto, relações de confiança não podem ser estabelecidas entre eles, mesmo se um ou ambos os domínios foram renomeados.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Modelos de Design de floresta](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Requisitos de Design de mapeamento para modelos de Design de floresta](Forest-Design-Models.md)  
  
-   [Usando o modelo de floresta de domínio organizacional](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


