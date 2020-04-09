---
ms.assetid: 8b15d44e-e4e6-4510-aa91-cc7ec7161b0a
title: A função do mecanismo de declaração
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3e6f3ab502a224169a0ab0075bceabdd681e3d86
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814329"
---
# <a name="the-role-of-the-claims-engine"></a>A função do mecanismo de declaração
Em seu nível mais alto, o mecanismo de declarações no Serviços de Federação do Active Directory (AD FS) \(AD FS\) é um mecanismo baseado em\-de regras que é dedicado a servir e processar solicitações de declaração para o Serviço de Federação. O mecanismo de declarações é a única entidade no serviço de federação que é responsável por executar cada um dos conjuntos de regras em todas as relações de confiança federadas que você configurou e por entregar o resultado ao pipeline de declarações.  
  
Embora o pipeline de declarações seja mais um conceito lógico do\-final para\-processo final para declarações de fluxo, as regras de declaração são um elemento administrativo real que você pode usar para personalizar o fluxo de declarações durante o processo de execução de regras de declaração. Para obter mais informações sobre o processo de pipeline, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
Conforme mostrado na ilustração a seguir, o ato de aceitar declarações de entrada \(regras de aceitação\), autorizar solicitantes de declarações \(regras de autorização\) e emitir declarações de saída \(regras de emissão\) por meio de regras de declaração em todas as relações de confiança federada em sua organização é executada pelo mecanismo de declarações.  
  
![AD FS funções](media/adfs2_enginepipeline.gif)  
  
## <a name="claim-rules-execution-process"></a>Processo de execução de regras de declaração  
Quando você configura uma relação de confiança de provedor de declarações ou de terceira parte confiável em sua organização com regras de declaração, o conjunto de regras de declaração\(s\) para essa relação de confiança agir como um gatekeeper para declarações de entrada invocando o mecanismo de declarações para aplicar a lógica necessária nas regras de declaração para determinar se deve emitir quaisquer declarações e quais declarações devem ser emitidas.  
  
A seção a seguir descreve cada uma das etapas que ocorrem pelo mecanismo durante o fluxo de declarações através do processo de execução de regras de declaração. Cada uma das etapas descritas a seguir ocorrem para cada estágio descrito no processo de pipeline de declarações. Essas etapas incluem:  
  
-   Etapa 1 – inicialização  
  
-   Etapa 2 – execução  
  
-   Etapa 3 – resultado da execução  
  
Para obter mais informações sobre o processo de pipeline, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="step-1--initialization"></a>Etapa 1 – inicialização  
Na primeira etapa do processo de execução de regras de declaração, o mecanismo de declarações aceita as declarações de entrada primeiro adicionando-as ao *conjunto de declarações de entrada*. Um conjunto de declarações de entrada é análogo a um cache na memória que é usado para armazenar temporariamente dados apenas enquanto um processo necessário exigir que esses dados sejam disponibilizados para recuperação. Os dados do conjunto de declaração de entrada serão descartados após a execução da regra.  
  
#### <a name="adding-a-claim-to-the-input-claim-set-for-a-rule-set"></a>Adicionando uma declaração à declaração de entrada para um conjunto de regras  
O conjunto de declarações de entrada é criado pelo mecanismo de declarações quando ele precisa armazenar temporariamente dados de declaração na memória enquanto processa a lógica associada com um conjunto de regras de declaração. O mecanismo de declarações copia todas as declarações de entrada para o conjunto de declarações de entrada em que elas podem ser recuperadas pela primeira regra no conjunto de regras.  
  
Por exemplo, na ilustração abaixo, o mecanismo de declarações lê as declarações de A e B das declarações de entrada e as copia para o conjunto de declarações de entrada. Depois que estiverem no conjunto de declaração de entrada, o mecanismo de declarações recupera e processa solicitações A e B como entrada para a lógica na primeira regra no conjunto de regras de declaração.  
  
![AD FS funções](media/adfs2_context1.gif)  
  
Todas as regras em um conjunto de regras de declaração compartilham o mesmo conjunto de declarações de entrada. Cada regra nesse conjunto pode ser adicionada ao conjunto de declarações de entrada compartilhado, afetando assim todas as regras subsequentes no conjunto.  
  
### <a name="step-2--execution"></a>Etapa 2 – execução  
Nesta etapa do processo de regras de declaração, as regras de declaração são processadas quando o mecanismo de declarações percorre em ordem cronológica todas as regras dentro de um conjunto de regras específico, um de cada vez. Cada regra dentro de um conjunto de regras é executada apenas uma vez e é executada na ordem em que aparecem de cima para baixo, conforme exibido na caixa de diálogo Editar regras de declaração, no\-snap de gerenciamento de AD FS no. A regra de declaração na parte superior do conjunto de regras é processada primeiro e, em seguida, regras subsequentes são processadas até que todas as regras tenham sido executadas.  
  
Conforme definido na linguagem da regra de declaração, uma regra de declaração consiste em duas partes, declaração de condição e emissão. O mecanismo de declarações primeiro processa a parte da condição usando os dados no conjunto de declarações de entrada para determinar se a condição especificada na regra é verdadeira para as declarações contidas no conjunto de declarações de entrada \(as declarações que correspondem à condição da regra são referidas como declarações correspondentes\). Se quaisquer declarações correspondentes forem encontradas, o mecanismo de declarações executa a instrução de emissão da regra para cada conjunto de declarações correspondentes. A declaração de emissão da regra pode executar qualquer uma das seguintes tarefas com declarações correspondentes:  
  
1.  Copiar uma declaração correspondente para o conjunto de declarações de saída  
  
2.  Transformar os campos de declaração e criar uma nova declaração apenas no conjunto de declarações de entrada ou nos conjuntos de declarações de avaliação e de saída.  
  
3.  Use a declaração de correspondência\(s\) como uma chave para pesquisar mais informações de um repositório de atributos para criar uma nova declaração\(s\) apenas no conjunto de declarações de entrada ou nos conjuntos de declarações de entrada e saída.  
  
#### <a name="adding-a-claim-to-the-output-claim-set-for-a-rule-set"></a>Adicionar uma declaração no conjunto de declarações de saída definido para um conjunto de regras  
O *conjunto de declarações de saída* é um local na memória que é inicialmente vazio e é importante porque o mecanismo de declarações retornará apenas declarações que residem no conjunto de declarações de saída definido após concluir o processo de execução. Isso significa que quaisquer declarações que só residem no conjunto de declarações de entrada e não no conjunto de declarações de saída serão ignoradas quando chegar a hora de calcular o conjunto de declarações de saída final.  
  
#### <a name="adding-a-claim-to-both-claim-sets-for-a-rule-set"></a>Adicionar uma declaração para os dois conjuntos de declarações para um conjunto de regras  
Como uma regra é processada, as declarações são adicionadas no conjunto de declarações de entrada ou no conjunto de declarações de entrada e no conjunto de declarações de saída com base na instrução usada na instrução de emissão da regra. A linguagem da regra de declaração se refere a essas instruções, como *adicionar* ou *emitir*.  
  
Se a declaração *adicionar* for usada, as declarações são adicionadas apenas ao conjunto de declarações de entrada e as declarações existirão apenas para fins de execução e deixarão de existir quando a execução for concluída. Se a declaração *emitir* for usada, as declarações são adicionadas no conjunto de declarações de entrada e no conjunto de declarações de saída e as declarações serão retornadas no conjunto de declaração de saída quando a execução for concluída. Para obter mais informações sobre essas declarações, consulte [The Role of the Claim Rule Language](The-Role-of-the-Claim-Rule-Language.md).  
  
Se a parte de condição de uma regra em um conjunto de regras não corresponder às declarações no conjunto de declaração de entrada, a parte da declaração de emissão da regra é ignorada e, portanto, nenhuma declaração é adicionada no conjunto de declarações de saída ou no conjunto de declarações de entrada. A ilustração a seguir e as etapas correspondentes mostram o que acontece quando o mecanismo de declarações executa uma regra de transformação:  
  
![AD FS funções](media/adfs2_context2.gif)  
  
1.  Declarações de entrada são adicionadas no conjunto de declarações de entrada definido pelo mecanismo de declarações.  
  
2.  Quando a primeira regra é executada, ele vê as declarações A e B, que são no momento as únicas declarações no conjunto de declarações de entrada, e processa a parte condicional da lógica de regra na regra 1.  
  
3.  Como a declaração a está presente no conjunto de declarações de entrada, a condição da regra é determinada como true \(corresponder à declaração de um\) e uma nova declaração C é adicionada ao conjunto de declarações de entrada e ao conjunto de declarações de saída.  
  
4.  A regra 2 agora pode usar as declarações A, B e C \(todas as declarações no conjunto de declarações de entrada\) como entrada para processar sua lógica.  
  
Para obter mais informações sobre a transformação de declarações, consulte [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).  
  
### <a name="step-3--execution-result"></a>Etapa 3 – resultado da execução  
O estágio final da execução do conjunto de regras de declaração começa depois que todas as regras forem executadas dentro de um determinado conjunto de regras fornecido e o conjunto final de declarações estiver presente no conjunto de declarações de saída. Neste ponto, o mecanismo de declarações retorna o contexto do conjunto de declarações de saída como a saída da execução do conjunto de regras. Desse ponto em diante, é o pipeline de declarações que assume e move essa saída final para o próximo estágio em seu processo.  
  
## <a name="sending-the-execution-output-to-the-claims-pipeline"></a>Enviar a saída de execução para o pipeline de declarações  
Quando o mecanismo de declarações processa um conjunto de regras, esse conjunto de regras tem seus próprios locais dedicados na memória para seus conjuntos de entrada e saída. Isso significa que os conjuntos de declarações de entrada e saída usados por um conjunto de regras são separados dos conjuntos de declarações de entrada e saída usados em outro conjunto de regras.  
  
Depois que o processo inteiro tiver sido executado para um conjunto de regras de \(as etapas 1, 2 e 3\), as declarações de saída emitidas recentemente \(conteúdo do conjunto de declarações de entrada\) serão usadas como entrada para o próximo conjunto de regras no pipeline de declarações. Isso permite que as declarações fluam da saída de um conjunto de regras para a entrada de outro conjunto de regras, conforme mostrado na ilustração a seguir.  
  
![AD FS funções](media/adfs2_enginecontexts.gif)  
  
> [!NOTE]  
> Embora o conjunto de regras de emissão também seja um estágio crítico no pipeline, a ilustração acima não o exibe, apenas para simplificar a ilustração. Para obter uma ilustração que mostra o conjunto de regras de emissão e como ele se encaixa no pipeline de declarações, consulte [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
Nesse caso, a saída das regras de aceitação é usada pelo pipeline para transmitir o conjunto final de declarações produzido pelas regras de aceitação para o segundo estágio no pipeline, que é o processamento de regras de autorização. Neste ponto, todo o processo de execução das regras de declaração \(as etapas 1, 2 e 3 acima\) seria executado novamente para o conjunto de regras de autorização. Esse ciclo continua até que o conjunto de regras de emissão \(o estágio final no pipeline\) tenha sido concluído.  
  
Depois que as declarações de saída finalizadas tiverem sido retornadas do mecanismo para o conjunto de regras de emissão, elas serão empacotadas em um token SAML e o serviço de federação enviará o token de volta ao cliente.  
  
## <a name="processing-authorization-rules"></a>Processando regras de autorização  
Se o conjunto de regras de declaração que está sendo executado durante a etapa 2 do processo de execução de regras de declaração consistir em regras de autorização \(que têm conjuntos de declarações de entrada e saída diferentes das regras de aceitação ou de emissão\), essas regras de autorização serão executadas para determinar se um solicitante de token está autorizado a obter um token de segurança para uma determinada parte confiável da Serviço de Federação com base  
  
O objetivo das regras de autorização é emitir uma declaração de permissão ou negação com base em se o usuário deve obter token para a determinada terceira parte confiável ou não. Conforme mostrado na ilustração a seguir, a saída da execução de autorização é usada pelo pipeline para determinar se o conjunto de regras de emissão é executado ou não — com base na presença ou ausência da declaração de permissão e\/ou negação — mas a saída de execução de autorização em si não é usada como uma entrada para o conjunto de regras de declaração.  
  
![AD FS funções](media/adfs2_authorization.gif)  
  
Para obter mais informações sobre a autorização de declarações, consulte [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).  
  

