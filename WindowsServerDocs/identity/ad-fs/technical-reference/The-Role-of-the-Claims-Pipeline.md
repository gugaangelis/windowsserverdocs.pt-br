---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: A função do pipeline de declaração
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 81398a65fdfc510f8d4d3c125b77cc76fa6a8787
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937755"
---
# <a name="the-role-of-the-claims-pipeline"></a>A função do pipeline de declaração
O pipeline de declarações no Serviços de Federação do Active Directory (AD FS) \( AD FS \) representa o caminho que as declarações devem seguir no serviço de Federação antes que possam ser emitidas. O Serviço de Federação gerencia todo o \- \- processo de ponta a ponta de declarações de fluxo por meio de vários estágios do pipeline de declarações, que também inclui o processamento de regras de declaração pelo mecanismo de regra de declaração.

Para obter mais informações sobre regras de declaração, consulte [a função de regras de declaração](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como o mecanismo de regra de declaração processa as regras, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).

A seção a seguir descreve o processo que o serviço de federação supervisiona mais detalhadamente.

## <a name="claims-pipeline-process"></a>Processo de pipeline de declarações
O processo de pipeline de declarações consiste em três estágios de alto \- nível. Cada etapa neste processo inicializa o mecanismo de regra de declaração para processar regras de declaração que são específicas para essa etapa. Esses estágios incluem \( na ordem em que ocorrem \) :

1.  Aceitar declarações de entrada — Esse estágio no pipeline de declarações é usado para extrair as declarações de entrada do token e eliminar as declarações que não são esperadas ou confiáveis. Depois que elas forem extraídas, as regras de aceitação que compõem a regra de transformação de aceitação definidas para a relação de confiança de um provedor de declarações são executadas. Essas regras podem ser usadas para passar ou adicionar novas declarações que podem ser usadas nos estágios subsequentes do pipeline de declarações. A saída desse estágio é usada como entrada para o segundo e terceiro estágios.

2.  Autorizando o solicitante de declarações — Esse estágio é usado pelo mecanismo de declarações para emitir uma permissão ou negar as declarações com base em se o solicitante de token tem permissão para obter um token para a terceira parte confiável ou não. No entanto, antes que isso possa ocorrer, as regras de autorização que compõem a regra de autorização de emissão ou a regra de autorização de delegação para o objeto de confiança de terceira parte confiável são executadas.

3.  Emitir declarações de saída — Esse estágio é usado para emitir declarações de saída e enviá-las pelo pipeline, no qual serão empacotadas em um token de segurança. No entanto, antes que isso possa ocorrer, as regras de emissão que compõem a regra de transformação de emissão definida para um objeto de confiança de terceira parte confiável são executadas, o que determinará quais declarações serão emitidos como declarações de saída.

Todos os três estágios realizam o processamento de regras de declarações, mas usam um conjunto diferente de regras. Conforme descrito acima, cada estágio tem um conjunto associado de regras com base no emissor das declarações de entrada \( as regras de aceitação \) ou o serviço de destino para o qual os claimincludes estão sendo emitidos \( regras de autorização e emissão \) .

As declarações são \- independentes de token, mas são transmitidas pela rede encapsuladas em tokens de segurança. As regras de declaração operam em declarações independentemente do formato do token de segurança de entrada ou saída.

As regras de declarações contêm a \- lógica definida pelo administrador pela qual o mecanismo de declarações aceitará as declarações de entrada, autorizará as declarações com base nas declarações de identidade e emissão do solicitante que são necessárias para uma terceira parte confiável. Por fim, é o mecanismo de declarações que determina quais declarações passarão para o token de segurança que será emitido após a declaração passar pelo pipeline de declarações.

Conforme mostrado na ilustração a seguir, o pipeline de declarações é responsável pelo processo completo \- de ponta a \- ponta de fluir uma declaração por meio de vários estágios de pipeline para terminar com uma declaração emitida que será enviada por uma relação de confiança de terceira parte confiável. A declaração de saída na ilustração representa a declaração emitida.

![AD FS funções](media/adfs2_pipeline.gif)

Embora não seja mostrado na ilustração, é o mecanismo de declarações que executa o processamento real das regras em cada estágio. Para obter mais informações, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).


