---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: Implantar declarações em florestas
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 21ddcbd3b71a8d623950f1600b654e04ecc41f1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357571"
---
# <a name="deploy-claims-across-forests"></a>Implantar declarações em florestas

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server 2012, um tipo de declaração é uma asserção sobre o objeto ao qual ele está associado. Tipos de declaração são definidos por floresta no Active Directory. Há muitos cenários em que uma entidade de segurança pode precisar atravessar um limite de confiança para acessar recursos em uma floresta confiável. A transformação de declarações entre florestas no Windows Server 2012 permite que você transforme as declarações de saída e de entrada que atravessam as florestas para que as declarações sejam reconhecidas e aceitas nas florestas confiante e confiável. Estes são alguns dos cenários reais para a transformação de declarações:  
  
-   Florestas confiantes podem usar transformação de declaração como uma proteção contra elevação de privilégio, filtrando as declarações de entrada com valores específicos.  
  
    Florestas confiantes também podem emitir declarações para entidades provenientes de um limite de confiança, se a floresta confiável não oferecer suporte nem emitir quaisquer declarações.  
  
-   Florestas confiáveis podem usar a transformação de declaração para impedir que certos tipos de declaração e declarações com determinados valores saiam para a floresta confiante.  
  
-   Você também pode usar transformação de declaração para mapear diferentes tipos de declaração entre as florestas confiantes e confiáveis. Isso pode ser usado para generalizar o tipo de declaração, o valor da declaração, ou ambos. Sem isso, você precisará padronizar os dados entre as florestas antes de usar as declarações. Generalizar declarações entre as florestas confiantes e confiáveis reduz os custos de TI.  
  
## <a name="claim-transformation-rules"></a>Regras de transformação de declaração  
A sintaxe de linguagem da regra de transformação divide uma única regra em duas partes principais: uma série de instruções de condição e a declaração do problema. Cada instrução de condição tem dois subcomponentes: o identificador de declaração e a condição. A instrução Issue contém palavras-chave, delimitadores e uma expressão de problema. A declaração da condição começa opcionalmente com uma variável de identificador de declaração, que representa a declaração de entrada correspondente. A condição verifica a expressão. Se a declaração de entrada não corresponder à condição, o mecanismo de transformação ignorará a declaração do problema e avaliará a próxima declaração de entrada em relação à regra de transformação. Se todas as condições corresponderem à declaração de entrada, ele processará a declaração do problema.  
  
Para obter informações detalhadas sobre linguagem de regras de declaração, consulte [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md).  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>Vinculando políticas de transformação de declarações a florestas  
Há dois componentes envolvidos na configuração de políticas de transformação de declaração: objetos de política de transformação de declaração e o link de transformação da declaração. Os objetos de política residem no contexto de nomenclatura de configuração em uma floresta e contêm informações de mapeamento das declarações. O link especifica a quais florestas confiantes e confiáveis o mapeamento se aplica.  
  
É importante compreender se a floresta é confiante ou confiável, porque essa é a base para vincular objetos de política de transformação. Por exemplo, a floresta confiável é a floresta que contém as contas de usuário que requerem acesso. A floresta confiante é a floresta que contém os recursos aos quais você deseja dar acesso aos usuários. As declarações percorrem a mesma direção que a entidade que requer o acesso. Por exemplo, se houver uma relação de confiança unidirecional da floresta contoso.com com a floresta adatum.com, as declarações fluirão da adatum.com para contoso.com, que permite que os usuários da adatum.com acessem recursos na contoso.com.  
  
Por padrão, uma floresta confiável permite a passagem de todas as declarações e uma floresta confiante elimina todas as declarações de entrada que recebe.  
  
## <a name="in-this-scenario"></a>Neste cenário  
A orientação a seguir está disponível para este cenário:  
  
-   [Etapas de demonstração de &#40;implantar declarações entre florestas&#41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [Linguagem de regras de transformação de declarações](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e os recursos que fazem parte deste cenário e descreve como dar suporte a ele.  
  
|Função/recurso|Como este cenário tem suporte|  
|-----------------|---------------------------------|  
|Active Directory Domain Services|Nesse cenário, você precisa configurar duas florestas do Active Directory com uma relação de confiança bidirecional. Você tem declarações nas duas florestas. Você também pode definir políticas de acesso central na floresta confiante na qual residem os recursos.|  
|Função dos Serviços de Arquivo e Armazenamento|Nesse cenário, a classificação de dados é aplicada aos recursos nos servidores de arquivos. A política de acesso central é aplicada à pasta a qual você deseja conceder acesso ao usuário. Depois da transformação, a declaração concede acesso de usuário a recursos com base na política de acesso central aplicada à pasta no servidor de arquivos.|  
  


