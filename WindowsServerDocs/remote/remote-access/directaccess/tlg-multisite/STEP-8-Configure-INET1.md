---
title: ETAPA 8 configurar INET1
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 693acb5c-dffc-4484-8286-163bb67724c9
ms.author: coreyp
author: coreyp-at-msft
ms.openlocfilehash: d6967b975b3a950c90de465872832d623755a494
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281414"
---
# <a name="step-8-configure-inet1"></a>ETAPA 8: Configurar INET1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Para habilitar os computadores cliente para se conectar aos servidores de acesso remoto pela Internet, você deve configurar uma entrada DNS para 2 EDGE1 em INET1.  
  
### <a name="to-create-the-2-edge1-dns-entry"></a>Para criar a entrada DNS EDGE1 2  
  
1.  Sobre o **inicie** tela, digite**Dnsmgmt. msc**, e pressione ENTER.  
  
2.  Na árvore de console, abra **zonas de pesquisa direta**, clique em **contoso.com**, em seguida, clique com botão direito **contoso.com**e, em seguida, clique em **novo Host (A ou AAAA)** .  
  
3.  Na **nome**, digite **EDGE1 2**. Na **endereço IP**, digite **131.107.0.20**. Clique em **Adicionar Host**, clique em **OK** e, em seguida, clique em **Concluído**.  
  


