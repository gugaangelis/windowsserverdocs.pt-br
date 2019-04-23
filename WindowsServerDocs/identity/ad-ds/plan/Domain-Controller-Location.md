---
ms.assetid: cc2834ec-8f66-4209-aba3-402d710cd1bd
title: Localização do controlador de domínio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6b66b224278f15b6abeecbef8fe0778a98159bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872617"
---
# <a name="domain-controller-location"></a>Localização do controlador de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Clientes usam o sistema de nome de domínio (DNS) para localizar controladores de domínio para completar operações como processar solicitações de logon ou pesquisando o diretório para os recursos publicados. Controladores de domínio registram uma variedade de registros no DNS para ajudar os clientes e outros computadores localização-los. Esses registros são coletivamente denominados os registros de localizador.  
  
Controladores de domínio também usarem o DNS para localizar outros controladores de domínio e para executar tarefas como a replicação. O processo pelo qual os controladores de domínio localizam outros controladores de domínio é o mesmo que o processo pelo qual os clientes localizam os controladores de domínio.  
  


