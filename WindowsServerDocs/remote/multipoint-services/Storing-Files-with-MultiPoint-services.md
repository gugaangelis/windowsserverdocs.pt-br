---
title: Armazenar arquivos com MultiPoint Services
description: Saiba mais sobre o armazenamento de arquivos no MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b432ca793b156997761f9fadab7340c394e3b553
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817337"
---
# <a name="storing-files-with-multipoint-services"></a>Armazenar arquivos com MultiPoint Services
MultiPoint Services dá suporte a armazenando arquivos do usuário das seguintes maneiras:  
  
-   **Na partição do sistema operacional da unidade de disco rígido.** Por padrão, o MultiPoint Services armazena arquivos de usuário na unidade de disco rígido com o sistema operacional.  
  
-   **Em uma partição separada da unidade de disco rígido.** Quando o sistema MultiPoint Services é configurado pela primeira vez, você pode *partição* o disco rígido. Ou seja, você pode configurar uma seção da unidade para que ele funcione como se fosse uma unidade separada. Isso torna mais fácil restaurar ou atualizar o sistema operacional sem afetar os arquivos do usuário. Para obter mais informações, consulte [criar uma partição ou a unidade lógica](https://go.microsoft.com/fwlink/?LinkId=182618) na biblioteca técnica do Windows Server.  
  
-   **Em uma adicional interna ou externa unidade de disco rígido.** Você pode anexar mais unidades de disco de rígido de internas ou externas ao MultiPoint Services para salvar e fazer backup dos dados.  
  
-   **Em uma pasta de rede compartilhada.** Para disponibilizar arquivos de usuário de qualquer estação, você pode criar uma pasta compartilhada na rede. Isso exige a outro computador ou servidor, além do computador executando o MultiPoint Services. Isso é o método recomendado para armazenar arquivos, se houver um servidor de arquivos.  
  
    Para pequenos sistemas de computadores de 2 a 3 executando o MultiPoint Services sem um servidor disponível, um dos computadores do MultiPoint Services pode agir como servidor de arquivos para todos os computadores do MultiPoint Services. Em seguida, você criaria contas de usuário para todos os usuários MultiPoint Services que está atuando como servidor de arquivos.  
  
