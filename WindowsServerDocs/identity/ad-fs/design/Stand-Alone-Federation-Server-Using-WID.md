---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Servidor de federação autônomo usando WID
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 760612e3163e4e862ba9beae94597f2a2356687c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972053"
---
# <a name="stand-alone-federation-server-using-wid"></a>Servidor de federação autônomo usando WID

Um \- servidor de Federação autônomo no Serviços de Federação do Active Directory (AD FS) \( AD FS \) consiste em um único servidor que hospeda uma serviço de Federação configurada para usar o wid do banco de dados interno do Windows \( \) . Essa AD FS topologia é para os laboratórios de teste. Não é recomendável para ambientes de produção porque ele tem um limite de apenas um servidor de Federação e não pode ser usado para escalar verticalmente para mais servidores.

Se desejar adicionar servidores de Federação adicionais ao laboratório de teste, você deverá recriar o Serviço de Federação do zero implantando qualquer uma das outras topologias mencionadas mais adiante nesta seção. Portanto, recomendamos que você use essa topologia para um laboratório de teste ou um \- ambiente de prova de \- conceito em sua rede de teste particular na qual um único servidor de Federação é adequado, conforme mostrado na ilustração a seguir.

![servidor usando WID](media/FedServerWID.gif)

## <a name="test-lab-considerations"></a>Considerações de laboratório de teste
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia para ambientes de laboratório de teste.

### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?

-   \( \) Profissionais de ti de tecnologia da informação ou arquitetos de ti que desejam avaliar ou desenvolver uma prova de conceito para essa tecnologia

### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?

-   Fácil de configurar em um ambiente de laboratório de teste

### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?

-   Somente um servidor de Federação por Serviço de Federação \( não tem capacidade de escalar verticalmente para um farm\)

-   Não redundante \( apenas uma única instância do banco de dados de configuração do AD FS existe\)


## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
