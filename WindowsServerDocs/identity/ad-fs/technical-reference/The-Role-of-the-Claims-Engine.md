---
ms.assetid: 8b15d44e-e4e6-4510-aa91-cc7ec7161b0a
title: "A função do mecanismo requerimentos judiciais ou Extrajudiciais"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 930b6f8034f17d8902104419042f944b82e90b4f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claims-engine"></a>A função do mecanismo requerimentos judiciais ou Extrajudiciais
No nível mais alto, o mecanismo de declarações do serviços de Federação do Active Directory \(AD FS\) é um mecanismo baseado em rule\ que é dedicado a servindo e processar solicitações de declaração do serviço de Federação. O mecanismo de declarações é a única entidade dentro do serviço de Federação é responsável por executando cada um dos conjuntos de regras em todas as relações de confiança federada que você configurou e entrega o resultado de saída para o pipeline de declarações.  
  
Enquanto o pipeline de declarações é mais um conceito lógico de declarações o processo de end\ to\ ponta para transferir as regras de declaração são um elemento administrativo real que você pode usar para personalizar o fluxo de declarações durante o processo de execução de regras de declaração. Para obter mais informações sobre o processo de pipeline, consulte [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
Conforme mostrado na ilustração a seguir, o ato de aceitar \(acceptance rules\) requerimentos judiciais ou Extrajudiciais recebidas, autorizar declarações solicitantes \(authorization rules\) e a emissão de declarações de saída \(issuance rules\) por meio de regras de declaração em todas as relações de confiança federada em sua organização é realizado pelo mecanismo de declarações.  
  
![Funções do AD FS](media/adfs2_enginepipeline.gif)  
  
## <a name="claim-rules-execution-process"></a>Processo de execução de regras de declaração  
Quando você configura uma relação de confiança de provedor de declarações ou terceiro confiar em sua organização com regras de declaração, set\(s\) de regra a declaração para o act essa confiança como um guardião para declarações de entrada, invocando o mecanismo de declarações para aplicar a lógica necessária com as regras de declaração para determinar se deve emitir todas as alegações e as solicitações emitir.  
  
A seção a seguir descreve cada uma das etapas que ocorrem pelo mecanismo de durante o fluxo de declarações durante o processo de execução de regras de declaração. Cada uma das etapas conforme descrito a seguir ocorre para cada estágio descrito no processo de pipeline de declarações. Estas etapas incluem:  
  
-   Etapa 1 – inicialização  
  
-   Etapa 2 – execução  
  
-   Etapa 3 – resultado da execução  
  
Para obter mais informações sobre o processo de pipeline, consulte [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="step-1--initialization"></a>Etapa 1 – inicialização  
Na primeira etapa do processo de execução de regras de declaração, o mecanismo de declarações aceita as declarações de entrada, primeiro adicionando-os para o *conjunto de declarações de entrada*. Um conjunto de declarações de entrada é análogo a um cache na memória que é usado para armazenar temporariamente os dados apenas como um processo exigido requer que os dados a ser disponibilizadas para recuperação. O conjunto de declarações de entrada dados serão descartados após a execução da regra é concluído.  
  
#### <a name="adding-a-claim-to-the-input-claim-set-for-a-rule-set"></a>Adicionando uma declaração para a declaração de entrada definido para um conjunto de regras  
O conjunto de declarações de entrada é criado pelo mecanismo de requerimentos judiciais ou Extrajudiciais quando ele precisa armazenar temporariamente reivindicação dados na memória enquanto ele processa a lógica associada a um conjunto de regras de declaração. O mecanismo de requerimentos judiciais ou Extrajudiciais copia todas as declarações de entrada para o conjunto de declarações de entrada onde pode ser recuperados pela primeira regra no conjunto de regras.  
  
Por exemplo, na ilustração abaixo, o mecanismo de requerimentos judiciais ou Extrajudiciais lê as declarações de A e B de entrada declarações e as copia o conjunto de declarações de entrada. Depois que eles estão no conjunto de solicitação de entrada, o mecanismo de requerimentos judiciais ou Extrajudiciais recupera e processa requerimentos judiciais ou Extrajudiciais A e B como entrada para a lógica na primeira regra no conjunto de regras de declaração.  
  
![Funções do AD FS](media/adfs2_context1.gif)  
  
Todas as regras em um conjunto de regras de declaração compartilham o mesmo conjunto de solicitação de entrada. Cada regra nesse conjunto pode adicionar ao conjunto compartilhado declaração de entrada, afetando, assim, todas as regras subsequentes no conjunto.  
  
### <a name="step-2--execution"></a>Etapa 2 – execução  
Nesta etapa do processo de regras de declaração, reivindicação regras são processadas quando o mecanismo de declarações em ordem cronológica percorre todas as regras dentro de um conjunto de regras específica, um por vez. Cada regra dentro de um conjunto de regras só será executada uma vez e é executada na ordem em que eles aparecem de cima para baixo conforme exibido na caixa de diálogo Editar regras de declaração no AD FS gerenciamento snap\-in. A regra de declaração que está na parte superior da regra definida é processada primeiro e, em seguida, regras subsequentes são processadas até que todas as regras foram executadas.  
  
Conforme definido no idioma de regra reivindicação, uma regra de declaração consiste em duas partes, condição e emissão de instrução. Os processos de primeira mecanismo requerimentos judiciais ou Extrajudiciais a parte de condição usando os dados na entrada reivindicar conjunto para determinar se a condição especificada na regra se aplica para as declarações contidas na entrada de conjunto de declarações \ (as declarações que correspondem a condição da regra são chamadas de uma correspondência claims\). Se todas as alegações correspondentes for encontradas, o mecanismo de requerimentos judiciais ou Extrajudiciais executa a declaração de emissão da regra para cada conjunto das declarações correspondentes. A declaração de emissão da regra pode realizar qualquer uma das seguintes tarefas com declarações correspondentes:  
  
1.  Copie uma reivindicação correspondente para o conjunto de declarações de saída  
  
2.  Os campos de declaração de transformação e criar uma nova declaração uma apenas o conjunto de declarações de entrada ou conjuntos de declarações de avaliação e de saída.  
  
3.  Use o claim\(s\) correspondentes como uma chave para pesquisa de informações de mais de um repositório de atributo para criar novo claim\(s\) uma apenas o conjunto de declarações de entrada ou conjuntos de declarações de entrada e saída.  
  
#### <a name="adding-a-claim-to-the-output-claim-set-for-a-rule-set"></a>Adicionando uma declaração para a declaração de saída definido para um conjunto de regras  
O *conjunto de declarações de saída* é um local na memória que é inicialmente vazio e é importante porque o mecanismo de requerimentos judiciais ou Extrajudiciais retornará apenas os requerimentos judiciais ou Extrajudiciais que residem na declaração de saída definida após a conclusão do processo de execução. Isso significa que todas as alegações residam apenas a declaração de entrada definido e não no conjunto de declaração de saída serão ignoradas quando chegar a hora para calcular o conjunto final de declarações de saída.  
  
#### <a name="adding-a-claim-to-both-claim-sets-for-a-rule-set"></a>Adicionando uma declaração para os dois conjuntos de declarações para um conjunto de regras  
Como uma regra é processada, as declarações são ou adicionado na entrada de conjunto de declarações ou em ambos os a entrada do conjunto de declarações e conjunto de declarações com base na instrução que é usada na declaração de emissão da regra de saída. O idioma de regra de declaração refere-se a essas instruções como *adicionar* ou *problema*.  
  
Se o *adicionar* instrução é usada, as declarações são adicionadas à apenas o conjunto de declarações de entrada e as declarações existirão somente para os fins da execução e deixarão de existir após a conclusão da execução. Se o *problema* instrução é usada, as declarações são adicionadas para o conjunto de declarações de entrada e o conjunto de declarações de saída e as declarações serão retornadas na declaração de saída definida após a conclusão da execução. Para saber mais sobre essas instruções, consulte [a função da linguagem de regra de declaração](The-Role-of-the-Claim-Rule-Language.md).  
  
Se a parte de condição de regra dentro de um conjunto de regras não corresponde a quaisquer requerimentos judiciais ou Extrajudiciais no conjunto de solicitação de entrada, a parte da política de emissão da regra é ignorada e, portanto, não é adicionados para o conjunto de declarações de saída ou para o conjunto de declarações de entrada. A ilustração a seguir e as etapas correspondentes mostram o que acontece quando o mecanismo de requerimentos judiciais ou Extrajudiciais executa uma regra de transformação:  
  
![Funções do AD FS](media/adfs2_context2.gif)  
  
1.  Declarações de entrada são adicionadas para a declaração de entrada definida pelo mecanismo de declarações.  
  
2.  Quando a primeira regra é executado, ele vê as declarações A e B, que estão no momento no conjunto de declarações as declarações somente a entrada de tempo, e processa a parte da lógica da regra na regra 1 condicional.  
  
3.  Desde que a declaração de um estiver presente no conjunto de solicitação de entrada, a condição de regra é considerada VERDADEIRO \ (correspondência reivindicação A\) e uma nova declaração de C é adicionada conjunto de declarações para a entrada e saída do conjunto de declarações.  
  
4.  Regra 2 agora pode usar as declarações A, B e C \ (todas as reclamações na entrada reivindicar Set \) como entrada para processar sua lógica.  
  
Para saber mais sobre a transformação de declarações, consulte [quando usar uma regra de declaração de transformação](When-to-Use-a-Transform-Claim-Rule.md).  
  
### <a name="step-3--execution-result"></a>Etapa 3 – resultado da execução  
O estágio final da execução do conjunto de regras reivindicação começa depois que todas as regras foram executadas dentro de um conjunto de regras de determinado e o conjunto final de declarações está presente no conjunto de declaração de saída. Neste ponto, o mecanismo de requerimentos judiciais ou Extrajudiciais retorna o contexto da reivindicação saída definido como a saída da execução do conjunto de regras. A partir desse ponto em diante é o pipeline de declarações que assume e move nessa saída final para o próximo estágio em seu processo.  
  
## <a name="sending-the-execution-output-to-the-claims-pipeline"></a>Enviar a saída de execução para o pipeline de declarações  
Quando as declarações mecanismo processos uma conjunto de regras, que conjunto de regras tem seus próprio dedicados locais na memória para sua entrada e conjuntos de solicitação de saída. Isso significa que a entrada e saída reivindicar conjuntos usados por um conjunto de regras são separados provenientes da entrada e saída reivindicar conjuntos usados em outro conjunto de regras.  
  
Depois que todo o processo é executado para um conjunto de regras de dar \ (as etapas 1, 2 e 3\), as declarações de saída emitidas recentemente \ (conteúdo da declaração de saída Set \) será usado como entrada para o próxima conjunto no pipeline de declarações de regras. Isso permite requerimentos judiciais ou Extrajudiciais fluir da saída de um conjunto de regras para a entrada para outro conjunto de regras, conforme mostrado na ilustração a seguir.  
  
![Funções do AD FS](media/adfs2_enginecontexts.gif)  
  
> [!NOTE]  
> Embora o conjunto de regras de emissão também é um estágio do pipeline crítico, ilustração acima não mostrá-lo apenas para fins de simplificar a ilustração. Para obter uma ilustração que mostra o conjunto de regras de emissão e como isso se encaixa no pipeline de declarações, consulte [a função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md).  
  
Nesse caso, a saída das regras da aceitação é usada pelo pipeline de fluxo o conjunto final de declarações produzido pelas regras de aceitação para o segundo estágio do pipeline, que é o processamento de regras de autorização. Neste ponto, todo o reivindicar o processo de execução de regras \ (above\ as etapas 1, 2 e 3) executaria novamente para o conjunto de regras de autorização. O ciclo se repete até que a regra de emissão definida \ (o estágio final no pipeline\) foi concluído.  
  
Depois que as declarações de saída finalizadas devolvidas do mecanismo para o conjunto de regras de emissão, eles serão empacotados em um token SAML e o serviço de Federação envia o token de volta para o cliente.  
  
## <a name="processing-authorization-rules"></a>Regras de autorização de processamento  
Se o conjunto de regras de declaração que está sendo executado durante a etapa 2 do processo de execução de regras de declaração consiste em regras de autorização \ (que tem um diferente entrada e saída reivindicar conjuntos de aceitação ou emissão rules\), em seguida, essas regras de autorização serão executado para determinar se um token solicitante está autorizado a obter um token de segurança para um determinado terceiro parte do serviço de federação com base em declarações do solicitante.  
  
O objetivo das regras de autorização é emitir uma permissão ou negação reivindicação com base em se o usuário é para ter permissão para obter um token para o terceiro determinado ou não. Conforme mostrado na ilustração a seguir, a saída da execução autorização é usada pelo pipeline para determinar se o conjunto de regras de emissão é executado ou não — com base na presença ou ausência da e \ permitir / ou negar reivindicação — mas a saída da execução de autorização em si não é usada como uma entrada para o conjunto de regras de declaração.  
  
![Funções do AD FS](media/adfs2_authorization.gif)  
  
Para saber mais sobre a autorização de declarações, consulte [quando usar uma regra de declaração de autorização](When-to-Use-an-Authorization-Claim-Rule.md).  
  

