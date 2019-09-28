---
title: Adicionar impressoras
description: Adicione uma impressora para os usuários dos serviços do MultiPoint.
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1f6d3ca-8caf-4aa0-b0ea-93cdfd3f3f37
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: e5da3d4919eba9f36e4b28f7c2d2fb3e3de02804
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405137"
---
# <a name="add-printers"></a>Adicionar impressoras
Use os procedimentos deste tópico para disponibilizar uma impressora local para todos os usuários em um sistema de serviços do MultiPoint.  
  
> [!NOTE]  
> Se você estiver usando contas de domínio com serviços do MultiPoint, os usuários poderão usar qualquer impressora de rede de suas estações.  
  
1.  Conecte a impressora ao MultiPoint Server.  
  
2.  Configure a impressora como uma impressora compartilhada:  
  
    1.  Faça logon no computador do MultiPoint Server como administrador.  
  
    2.  Na tela **Inicial** , abra o **Painel de Controle**.  
  
    3.  No painel de controle, clique em **hardware**e em **dispositivos e impressoras**.  
  
    4.  Em **impressoras e aparelhos de fax**, clique com o botão direito do mouse na impressora e clique em **Propriedades da impressora**.  
  
    5.  Clique na guia **compartilhamento** .  
  
    6.  Clique em **compartilhar esta impressora**, especifique um nome de compartilhamento para a impressora e clique em **OK**.  
  
Os usuários conectados a qualquer estação conectada ao computador dos serviços do MultiPoint poderão ver e usar a impressora. 