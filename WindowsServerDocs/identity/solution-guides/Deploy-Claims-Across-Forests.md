---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: Implantar requerimentos judiciais ou Extrajudiciais entre florestas
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e47804b9e194706d3b30787a1d6ae4b907467af1
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="deploy-claims-across-forests"></a>Implantar requerimentos judiciais ou Extrajudiciais entre florestas

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server 2012, um tipo de declaração é uma declaração sobre o objeto ao qual ele tem associado. Tipos de declaração são definidos por floresta no Active Directory. Há muitos cenários em que uma entidade de segurança talvez seja necessário percorrer um limite de confiança para acessar os recursos em uma floresta confiável. A transformação de declarações entre florestas no Windows Server 2012 possibilita a transformação de declarações de saída e entrada que atravessem florestas para que as declarações são reconhecidas e aceito nas florestas confiantes e confiáveis. Estes são alguns dos cenários de transformação de declarações do mundo real:  
  
-   Florestas de confiança podem usar reivindicação transformação como um protetor contra elevação de privilégio filtrando as declarações de entrada com valores específicos.  
  
    Florestas de confiança também podem emitir reivindicações de entidades proveniente de um limite de confiança se floresta confiável não oferece suporte ou execute qualquer reivindicação.  
  
-   Florestas confiáveis podem usar a transformação de declaração para impedir que certos tipos de declaração e requerimentos judiciais ou Extrajudiciais com determinados valores apaguem à floresta confiante.  
  
-   Você também pode usar a declaração de tipos entre florestas confiantes e confiáveis de declaração de transformação para mapear diferentes. Isso pode ser usado para generalizar o tipo de declaração, o valor da declaração ou ambos. Sem isso, você precisará padronizar os dados entre as florestas antes de poder usar as declarações. Generalizar requerimentos judiciais ou Extrajudiciais entre as florestas confiáveis e confiantes reduz os custos de TI.  
  
## <a name="claim-transformation-rules"></a>Regras de transformação de declaração  
A sintaxe de linguagem de regra de transformação divide uma única regra em duas partes principais: uma série de instruções de condição e a declaração do problema. Cada instrução condicional tem dois subcomponentes: o identificador de declaração e a condição. A declaração do problema contém palavras-chave, delimitadores e uma expressão do problema. A instrução condicional, opcionalmente, começa com uma variável de identificador reivindicação, que representa a declaração de entrada correspondente. A condição verifica a expressão. Se a declaração de entrada não corresponder a condição, o mecanismo de transformação ignora a declaração do problema e avalia a declaração de entrada próxima contra a regra de transformação. Se todas as condições corresponderem a declaração de entrada, ele processa a declaração do problema.  
  
Para obter informações detalhadas sobre a linguagem de regras de declaração, veja [linguagem de regras de transformação de declarações](Claims-Transformation-Rules-Language.md).  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>Vinculação políticas de transformação de declaração para florestas  
Existem dois componentes envolvidos na configuração de políticas de transformação de declaração: reivindicar objetos de política de transformação e o link de transformação. Os objetos de política dinâmicos no contexto de nomenclatura de configuração em uma floresta, e eles contêm informações de mapeamento para as declarações. O link especifica qual confiando e florestas confiáveis o mapeamento é aplicado.  
  
É importante entender se floresta é floresta confiante ou confiável porque esta é a base para os objetos de política de transformação de vinculação. Por exemplo, a floresta confiável é a floresta que contém as contas de usuário que exigem acesso. Floresta confiante é a floresta que contém recursos que você deseja dar aos usuários acesso a. Declarações de viagem na mesma direção como a objeto que exige acesso de segurança. Por exemplo, se houver uma relação de confiança da floresta contoso.com à floresta adatum.com, se propagam as declarações de adatum.com para contoso.com, que permite que os usuários de adatum.com para acessar recursos em contoso.com.  
  
Por padrão, todas as reclamações de saída passar permite que uma floresta confiável e uma floresta confiante solta todas as reclamações de entrada que ele recebe.  
  
## <a name="in-this-scenario"></a>Neste cenário  
A orientação a seguir está disponível para este cenário:  
  
-   [Implantar requerimentos judiciais ou Extrajudiciais entre florestas #40 etapas de demonstração &; 41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [Linguagem de regras de transformação de declarações](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e recursos que fazem parte desse cenário e descreve como suporte.  
  
|Função/recurso|Como ele dá suporte a esse cenário|  
|-----------------|---------------------------------|  
|Serviços de domínio do Active Directory|Nesse cenário, você precisa configurar dois florestas do Active Directory com uma relação de confiança bidirecional. Você tem requerimentos judiciais ou Extrajudiciais nas duas florestas. Você também pode definir políticas de acesso central em floresta confiante onde residem os recursos.|  
|Função Serviços de arquivo e armazenamento|Nesse cenário, a classificação de dados é aplicada aos recursos nos servidores de arquivos. A política de acesso central é aplicada para a pasta onde você deseja conceder acesso de usuário. Após a transformação, a declaração concede acesso de usuário aos recursos de acordo com a política de acesso central que é aplicada à pasta no servidor de arquivos.|  
  


