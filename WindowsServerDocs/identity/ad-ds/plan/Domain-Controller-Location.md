---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Localização do controlador de domínio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 48ea357b952738c63274d194b4a5aa5d4adcb3d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408841"
---
# <a name="domain-controller-location"></a>Localização do controlador de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os clientes usam o DNS (sistema de nomes de domínio) para localizar controladores de domínio para concluir operações como processar solicitações de logon ou pesquisar recursos publicados no diretório. Os controladores de domínio registram uma variedade de registros no DNS para ajudar os clientes e outros computadores a localizá-los. Esses registros são coletivamente chamados de registros do localizador.  
  
Os controladores de domínio também usam o DNS para localizar outros controladores de domínio e para executar tarefas como replicação. O processo pelo qual os controladores de domínio localizam outros controladores de domínio é o mesmo que o processo pelo qual os clientes localizam controladores de domínio.  
  


