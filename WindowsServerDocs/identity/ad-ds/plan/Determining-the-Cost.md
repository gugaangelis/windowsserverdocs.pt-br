---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Determinando o custo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b32d1930fcef944fbd719338797f0f29b70fa29d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-cost"></a>Determinando o custo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Atribuir custo valores links de site para favorecer conexões baratos via conexões caros. Determinados aplicativos e serviços, como o localizador de controlador de domínio (DCLocator) e distribuídos arquivo sistema Namespaces DFSN (), também usam informações de custo para localizar os recursos mais próximos. Custo do link pode ser usado para determinar qual controlador de domínio é contatado por clientes em um site se o controlador de domínio para o domínio especificado não existir nesse site. O cliente contata o controlador de domínio usando o link de site que tem o menor custo atribuído a ele.  
  
Recomendamos que o valor de custo ser definido em uma base de todo o site. Custo baseia geralmente não só na largura de banda total do link, mas também sobre a disponibilidade, a latência e o custo monetário do link.  
  
Para determinar os custos colocar nos links de sites, documente a velocidade de conexão para cada link de site. Consulte a planilha de (DSSTOPO_1.doc) "Localizações geográficas e comunicação Links" em [Coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) para obter informações sobre a velocidade de conexão que você identificou.  
  
A tabela a seguir lista as velocidades de diferentes tipos de redes.  
  
|Tipo de rede|Velocidade|  
|----------------|---------|  
|Muito lento|56 quilobits por segundo (Kbps)|  
|Lento|64 kbps|  
|Rede Digital de serviços integrados (ISDN)|64 kbps ou 128 Kbps|  
|Retransmissão|Taxa de variável, comumente entre 56 Kbps e 1,5 megabits por segundo (Mbps)|  
|T1|1,5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|Modo de transferência assíncrona (ATM)|Taxa de variável, comumente entre 155 Mbps e 622 Mbps|  
|100BaseT|100 Mbps|  
|Gigabit Ethernet|1 gigabit por segundo (Gbps)|  
  
Use a tabela a seguir para calcular o custo de cada link de site com base na velocidade de rede de longa distância velocidade do link (WAN). Para a velocidade do link WAN que não esteja listada na tabela, você pode calcular um fator de custo relativo dividindo 1.024 pelo log da largura de banda disponível, conforme medido em Kbps.  
  
|Largura de banda disponível (Kbps)|Custo|  
|--------------------------------|--------|  
|9.6|1,042|  
|19.2|798|  
|38.4|644|  
|56|586|  
|64|567|  
|128|486|  
|256|425|  
|512|378|  
|1,024|340|  
|2,048|309|  
|4,096|283|  
  
Esses custos não refletir diferenças confiabilidade entre os links de rede. Defina custos mais altos em quaisquer links sujeito a falhas de rede para que você não tenha que dependem desses links para replicação. Ao custos de link site mais altos configuração, você pode controlar failover de replicação quando um link de site falha.  
  


