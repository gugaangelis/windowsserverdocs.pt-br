---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: A função do pipeline de declaração
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5076a686b5d0b9a539f6cad8594aaf84dccc3edb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887057"
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claims-pipeline"></a>A função do pipeline de declaração
O pipeline de declarações nos serviços de Federação do Active Directory \(do AD FS\) representa o caminho que as declarações devem seguir por meio do serviço de Federação antes que eles podem ser emitidos. O serviço de Federação gerencia todo final\-para\-terminar o processo de fluxo de declarações por vários estágios do pipeline de declarações, que também inclui o processamento de regras de declaração pelo mecanismo de regra de declaração.  
  
Para obter mais informações sobre regras de declaração, consulte [The Role of Claim Rules](The-Role-of-Claim-Rules.md). Para obter mais informações sobre como o mecanismo de regra de declaração processa as regras, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
A seção a seguir descreve o processo que o serviço de federação supervisiona mais detalhadamente.  
  
## <a name="claims-pipeline-process"></a>Processo de pipeline de declarações  
O processo de pipeline de declarações consiste em três alta\-estágios de nível. Cada etapa neste processo inicializa o mecanismo de regra de declaração para processar regras de declaração que são específicas para essa etapa. Esses estágios incluem \(na ordem em que eles ocorrem\):  
  
1.  Aceitar declarações de entrada — Esse estágio no pipeline de declarações é usado para extrair as declarações de entrada do token e eliminar as declarações que não são esperadas ou confiáveis. Depois que elas forem extraídas, as regras de aceitação que compõem a regra de transformação de aceitação definidas para a relação de confiança de um provedor de declarações são executadas. Essas regras podem ser usadas para passar ou adicionar novas declarações que podem ser usadas nos estágios subsequentes do pipeline de declarações. A saída desse estágio é usada como entrada para o segundo e terceiro estágios.  
  
2.  Autorizando o solicitante de declarações — Esse estágio é usado pelo mecanismo de declarações para emitir uma permissão ou negar as declarações com base em se o solicitante de token tem permissão para obter um token para a terceira parte confiável ou não. No entanto, antes que isso possa ocorrer, as regras de autorização que compõem a regra de autorização de emissão ou a regra de autorização de delegação para o objeto de confiança de terceira parte confiável são executadas.  
  
3.  Emitir declarações de saída — Esse estágio é usado para emitir declarações de saída e enviá-las pelo pipeline, no qual serão empacotadas em um token de segurança. No entanto, antes que isso possa ocorrer, as regras de emissão que compõem a regra de transformação de emissão definida para um objeto de confiança de terceira parte confiável são executadas, o que determinará quais declarações serão emitidos como declarações de saída.  
  
Todos os três estágios realizam o processamento de regras de declarações, mas usam um conjunto diferente de regras. Conforme descrito acima, cada estágio tem um conjunto de regras com base no emissor das declarações de entrada associado \(regras de aceitação\) ou o serviço de destino para o qual estão sendo emitidos as claimincludes \(autorização e regras de emissão\).  
  
As declarações são token\-independente, mas que são transmitidos pela rede encapsulada nos tokens de segurança. As regras de declaração operam em declarações independentemente do formato do token de segurança de entrada ou saída.  
  
Regras de declarações contêm o administrador\-lógica definida pelo qual o mecanismo de declarações aceitará as declarações de entrada, autorizar declarações com base na identidade do solicitante e emitir declarações que são necessários para uma terceira parte confiável. Por fim, é o mecanismo de declarações que determina quais declarações passarão para o token de segurança que será emitido após a declaração passar pelo pipeline de declarações.  
  
Conforme mostrado na ilustração a seguir, o pipeline de declarações é responsável por todo final\-para\-terminar o processo de fluxo de uma declaração por meio de vários estágios do pipeline para terminar com uma declaração emitida que será enviada ao longo de uma terceira parte confiável objeto de confiança. A declaração de saída na ilustração representa a declaração emitida.  
  
![Funções do AD FS](media/adfs2_pipeline.gif)  
  
Embora não seja mostrado na ilustração, é o mecanismo de declarações que executa o processamento real das regras em cada estágio. Para obter mais informações, consulte [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  

