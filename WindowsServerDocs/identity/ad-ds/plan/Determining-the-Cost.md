---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Determinando o custo
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: acf68e45a1d914bcbf5e780f51d2455fe43ab3e3
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939406"
---
# <a name="determining-the-cost"></a>Determinando o custo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você atribui valores de custo a links de site para favorecer conexões baratas em conexões caras. Determinados aplicativos e serviços, como o localizador de controlador de domínio (DCLocator) e os namespaces de Sistema de Arquivos Distribuído (DFSN), também usam informações de custo para localizar os recursos mais próximos. O custo do link do site pode ser usado para determinar qual controlador de domínio será contatado pelos clientes em um site se o controlador de domínio para o domínio especificado não existir nesse site. O cliente entra em contato com o controlador de domínio usando o link de site que tem o menor custo atribuído a ele.

Recomendamos que o valor de custo seja definido em todo o site. Geralmente, o custo é baseado não apenas na largura de banda total do link, mas também na disponibilidade, na latência e no custo monetário do link.

Para determinar os custos a serem colocados em links de site, documente a velocidade de conexão para cada link de site. Consulte a planilha "locais geográficos e links de comunicação" (DSSTOPO_1.doc) em [coleta de informações de rede](../../ad-ds/plan/Collecting-Network-Information.md) para obter informações sobre a velocidade de conexão que você identificou.

A tabela a seguir lista as velocidades para diferentes tipos de redes.

|Tipo de rede|Velocidade|
|----------------|---------|
|Muito lento|56 quilobits por segundo (Kbps)|
|Lento|64 Kbps|
|Rede digital de serviços integrados (ISDN)|64 kbps ou 128 kbps|
|Frame Relay|Taxa variável, normalmente entre 56 kbps e 1,5 megabits por segundo (Mbps)|
|T1|1.5 Mbps|
|T3|45 Mbps|
|10Baset|10 Mbps|
|Modo de transferência assíncrona (ATM)|Taxa variável, normalmente entre 155 Mbps e 622 Mbps|
|100Baset|100 Mbps|
|Gigabit Ethernet|1 gigabit por segundo (Gbps)|

Use a tabela a seguir para calcular o custo de cada link de site com base na velocidade de link de WAN (velocidade de rede de longa distância). Para a velocidade de link de WAN que não está listada na tabela, você pode calcular um fator de custo relativo dividindo 1.024 pelo log da largura de banda disponível, conforme medido em Kbps.

|Largura de banda disponível (Kbps)|Custo|
|--------------------------------|--------|
|9,6|1.042|
|19,2|798|
|38,4|644|
|56|586|
|64|567|
|128|486|
|256|425|
|512|378|
|1\.024|340|
|2.048|309|
|4\.096|283|

Esses custos não refletem as diferenças na confiabilidade entre os links de rede. Defina custos mais altos em qualquer link de rede propenso a falhas para que você não precise confiar nesses links para replicação. Ao definir custos de link de site mais altos, você pode controlar o failover de replicação quando um link de site falha.



