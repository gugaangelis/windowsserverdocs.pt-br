---
title: ETAPA 8 configurar INET1
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: 707591604a1d030b3abba9395081d2c2e4b7fb1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838807"
---
# <a name="step-8-configure-inet1"></a>ETAPA 8: Configurar INET1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Para habilitar os computadores cliente para se conectar aos servidores de acesso remoto pela Internet, você deve configurar uma entrada DNS para 2 EDGE1 em INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Para criar a entrada DNS EDGE1 2  
  
1.  Sobre o **inicie** tela, digite**Dnsmgmt. msc**, e pressione ENTER.  
  
2.  Na árvore de console, abra **zonas de pesquisa direta**, clique em **contoso.com**, em seguida, clique com botão direito **contoso.com**e, em seguida, clique em **novo Host (A ou AAAA)**.  
  
3.  Na **nome**, digite **EDGE1 2**. Na **endereço IP**, digite **131.107.0.20**. Clique em **Adicionar Host**, clique em **OK** e, em seguida, clique em **Concluído**.  
  


