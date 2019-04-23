---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Determinando o custo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b34f1672311768d644c467fda10dc2fc643282d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847077"
---
# <a name="determining-the-cost"></a>Determinando o custo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você atribuir valores de custo para links de site para favorecer as conexões de baixo custo em conexões caras. Alguns aplicativos e serviços, como o localizador de controladores de domínio (DCLocator) e distribuídas arquivo sistema DFSN (Namespaces), também usam informações de custo para localizar os recursos mais próximos. Custo do link de site pode ser usado para determinar qual controlador de domínio é contatado pelos clientes em um site se o controlador de domínio para o domínio especificado não existe nesse site. O cliente contata o controlador de domínio usando o link de site que tem o menor custo atribuído a ele.  
  
É recomendável que o valor de custo ser definido em uma base de todo o site. Geralmente, o custo é baseado não apenas na largura de banda total do link, mas também sobre a disponibilidade, latência e custo monetário do link.  
  
Para determinar os custos para colocar em links de site, documente a velocidade de conexão para cada link de site. Consulte a planilha "Geográficas locais e Links de comunicação" (DSSTOPO_1.doc) no [Coletando informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) para obter informações sobre a velocidade de conexão que você identificou.  
  
A tabela a seguir lista as velocidades para diferentes tipos de redes.  
  
|Tipo de rede|Velocidade|  
|----------------|---------|  
|Muito lenta|56 quilobits por segundo (Kbps)|  
|Modo Lento|64 kbps|  
|Integrated Services Digital Network (ISDN)|64 kbps ou 128 Kbps|  
|Retransmissão de quadros|Taxa variável, normalmente entre 56 Kbps e 1,5 megabits por segundo (Mbps)|  
|T1|1,5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|Modo de transferência assíncrona (ATM)|Taxa variável, normalmente entre 155 Mbps e 622 Mbps|  
|100BaseT|100 Mbps|  
|Gigabit Ethernet|1 gigabit por segundo (Gbps)|  
  
Use a tabela a seguir para calcular o custo de cada link de site com base na velocidade da rede de longa distância velocidade do link de (longa distância WAN). Para velocidade de link WAN que não esteja listada na tabela, você pode calcular um fator de custo relativo dividindo 1.024 pelo log de largura de banda disponível, conforme medido em Kbps.  
  
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
  
Esses custos não refletem as diferenças de confiabilidade entre links de rede. Definir custos mais altos em todos os links propenso a falhas de rede para que você não precise se baseiam nesses links de replicação. Por mais altos custos de link de site configuração, você pode controlar o failover de replicação quando um link de site falha.  
  


