---
title: ETAPA 8 configurar o INET1
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: ca8a49e612bef9c4a72c47e8d0e147a900686f84
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861539"
---
# <a name="step-8-configure-inet1"></a>ETAPA 8: configurar INET1

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Para permitir que computadores cliente se conectem a servidores de acesso remoto pela Internet, você deve configurar uma entrada DNS para 2-EDGE1 no INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Para criar a entrada DNS de 2 EDGE1  
  
1.  Na tela **Iniciar** , digite**DNSMGMT. msc**e pressione Enter.  
  
2.  Na árvore de console, abra **zonas de pesquisa direta**, clique em **contoso.com**, clique com o botão direito do mouse em **contoso.com**e clique em **novo host (A ou aaaa)** .  
  
3.  Em **nome**, digite **2-EDGE1**. Em **endereço IP**, digite **131.107.0.20**. Clique em **Adicionar Host**, clique em **OK** e, em seguida, clique em **Concluído**.  
  


