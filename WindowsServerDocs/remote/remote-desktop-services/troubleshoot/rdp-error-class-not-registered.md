---
title: Os clientes não conseguem se conectar e recebem o erro "Classe não registrada"
description: Solução do erro “Classe não registrada” com a conexão de área de trabalho remota.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 7a98e894f3eaf47e257ab39c640e93101fd76fc8
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265898"
---
# <a name="clients-cant-connect-and-get-the-class-not-registered-error"></a>Os clientes não conseguem se conectar e recebem o erro "Classe não registrada"

Quando você tenta se conectar a um computador remoto usando um cliente que está executando o Windows 10 versão 1709 ou posterior, o cliente não pode se conectar enquanto o servidor Host da Sessão da Área de Trabalho Remota relata uma mensagem que contém o código de erro "Classe não registrada (0x80040154)".

Esse problema ocorre quando o usuário que está tentando se conectar tem um perfil do usuário obrigatório. Para resolver esse problema, instale a [atualização do Windows 10 de 24 de julho de 2018 – KB4338817 (Build do SO 16299.579)](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817).
