---
title: ETAPA 8 configurar o INET1
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: lizross
author: eross-msft
manager: mtillman
ms.openlocfilehash: eed2aef3cf1104d070feb43c37243a92e48e5206
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636759"
---
# <a name="step-8-configure-inet1"></a>ETAPA 8: configurar INET1

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Para permitir que computadores cliente se conectem a servidores de acesso remoto pela Internet, você deve configurar uma entrada DNS para 2-EDGE1 no INET1.

### <a name="to-create-the-2-edge1-dns-entry"></a>Para criar a entrada DNS de 2 EDGE1

1.  Na tela **Iniciar** , digite**DNSMGMT. msc**e pressione Enter.

2.  Na árvore de console, abra **zonas de pesquisa direta**, clique em **contoso.com**, clique com o botão direito do mouse em **contoso.com**e clique em **novo host (A ou aaaa)**.

3.  Em **nome**, digite **2-EDGE1**. Em **endereço IP**, digite **131.107.0.20**. Clique em **Adicionar Host**, clique em **OK** e, em seguida, clique em **Concluído**.



