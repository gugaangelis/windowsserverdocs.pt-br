---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: "A função do Pipeline requerimentos judiciais ou Extrajudiciais"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e433b89ab7f225b589d94d98ce57c76007ef41aa
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claims-pipeline"></a>A função do Pipeline requerimentos judiciais ou Extrajudiciais
O pipeline de declarações em serviços de Federação do Active Directory \(AD FS\) representa o caminho requerimentos judiciais ou Extrajudiciais devem seguir por meio do serviço de Federação antes que eles podem ser emitidos. O serviço de Federação gerencia todo o processo end\ to\ ponta de requerimentos judiciais ou Extrajudiciais fluindo pelos vários estágios do pipeline requerimentos judiciais ou Extrajudiciais, que também inclui o processamento de regras de declaração pelo mecanismo de regras de declaração.  
  
Para obter mais informações sobre a reivindicação regras, consulte [a função de reivindicar regras](The-Role-of-Claim-Rules.md). Para saber mais sobre como o mecanismo de regras de declaração processa as regras, consulte [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md).  
  
A seção a seguir descreve o processo que o serviço de Federação inspeciona mais detalhadamente.  
  
## <a name="claims-pipeline-process"></a>Declarações do pipeline de processo  
O processo de pipeline requerimentos judiciais ou Extrajudiciais consiste em três estágios de nível de high\. Cada etapa neste processo inicializa o mecanismo de regra de declaração para processar reivindicação regras que são específicos para esse estágio. Esses estágios incluem \ (na ordem em que elas occur\):  
  
1.  Aceitar entradas requerimentos judiciais ou Extrajudiciais — nesse estágio do pipeline de declarações é usado para extrair as declarações de entrada do token e eliminar declarações que não são esperadas ou confiável. Depois que eles são extraídos, as regras de aceitação que compõem a regra de transformação de aceitação definidas para um requerimentos judiciais ou Extrajudiciais confiança provedor são executadas. Essas regras podem ser usadas para passar pela ou adicionar novas declarações que podem ser usadas nos estágios subsequentes do pipeline de declarações. A saída desse estágio é usada como uma entrada para o segundo e terceiro estágio.  
  
2.  Autorizar o solicitante requerimentos judiciais ou Extrajudiciais — este estágio é usado pelo mecanismo de declarações para emitir permitir ou negar requerimentos judiciais ou Extrajudiciais com base em se o token solicitante tem permissão para obter um token para o terceiro determinado ou não. No entanto, antes que isso pode ocorrer definir as regras de autorização que compõem uma regra de autorização de emissão ou definir a regra de autorização de delegação para uma terceira confiança de terceiros são foi executado.  
  
3.  A emissão de declarações de saída-esse estágio é usado para emitir declarações de saída e enviá-los ao longo do pipeline onde eles serão empacotados em um token de segurança. No entanto, antes que isso pode ocorrer as regras de emissão que compõem a regra de transformação de emissão definido para uma terceira confiança de terceiros são foi executada, que irá determinar o que diz ser emitida como declarações de saída.  
  
Todos os três estágios acima executam regras requerimentos judiciais ou Extrajudiciais de processamento, mas usam um conjunto de regras diferente. Conforme descrito acima, cada estágio tem um conjunto de regras com base no emissor de declarações de entrada associado \(the acceptance rules\) ou o serviço de destino para o qual os claimincludes estão sendo emitidos \ (rules\ emissão e autorização).  
  
Requerimentos judiciais ou Extrajudiciais são independentes de token\, mas são transmitidas pela rede encapsulada em tokens de segurança. As regras de reivindicação operam sobre requerimentos judiciais ou Extrajudiciais, independentemente do formato do token de segurança de entrada ou saída.  
  
Requerimentos judiciais ou Extrajudiciais regras contenham a lógica definida administrator\ pelo qual o mecanismo de requerimentos judiciais ou Extrajudiciais será aceitar as declarações de entrada, autorizar requerimentos judiciais ou Extrajudiciais com base na identidade do solicitante e emitir declarações que são necessárias por um terceiro. Por fim, é o mecanismo de declarações que determina o que requerimentos judiciais ou Extrajudiciais passará para o token de segurança que será emitido após a declaração tem sido flui através do pipeline de declarações.  
  
Conforme mostrado na ilustração a seguir, o pipeline de declarações é responsável por todo o processo end\ to\ ponta de fluindo uma reivindicação pelos vários estágios de pipeline para terminar com uma declaração emitida que será enviada por uma terceira confiança de terceiros. A declaração de saída na ilustração representa a declaração emitida.  
  
![Funções do AD FS](media/adfs2_pipeline.gif)  
  
Embora ele não é mostrado na ilustração, é o mecanismo de declarações que executa o processamento real das regras em cada estágio. Para obter mais informações, consulte [a função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md).  
  

