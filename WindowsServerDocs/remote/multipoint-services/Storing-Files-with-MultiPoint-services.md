---
title: Armazenar arquivos com MultiPoint Services
description: Saiba mais sobre o armazenamento de arquivos nos serviços do MultiPoint
ms.date: 07/22/2016
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6991a62fde0e0083eb5544eed6ec49fef2b10569
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951621"
---
# <a name="storing-files-with-multipoint-services"></a>Armazenar arquivos com MultiPoint Services
Os serviços do MultiPoint oferecem suporte ao armazenamento de arquivos do usuário das seguintes maneiras:

-   **Na partição do sistema operacional da unidade de disco rígido.** Por padrão, os serviços do MultiPoint armazenam arquivos de usuário na unidade de disco rígido com o sistema operacional.

-   **Em uma partição separada da unidade de disco rígido.** Quando o sistema de serviços do MultiPoint estiver configurado pela primeira vez, você poderá *particionar* a unidade de disco rígido. Ou seja, você pode configurar uma seção da unidade para que ela funcione como se fosse uma unidade separada. Isso torna mais fácil restaurar ou atualizar o sistema operacional sem afetar os arquivos do usuário. Para obter mais informações, consulte [criar uma partição ou unidade lógica](https://go.microsoft.com/fwlink/?LinkId=182618) na biblioteca técnica do Windows Server.

-   **Em uma unidade de disco rígido interna ou externa adicional.** Você pode anexar mais unidades internas ou externas de disco rígido aos serviços do MultiPoint para salvar e fazer backup de dados.

-   **Em uma pasta de rede compartilhada.** Para disponibilizar arquivos do usuário de qualquer estação, você pode criar uma pasta compartilhada na rede. Isso requer outro computador ou servidor além do computador que executa os serviços do MultiPoint. Esse é o método recomendado para armazenar arquivos se houver um servidor de arquivos disponível.

    Para sistemas pequenos de 2-3 computadores que executam serviços do MultiPoint sem nenhum servidor de arquivos disponível, um dos computadores dos serviços do MultiPoint pode atuar como o servidor de arquivos para todos os computadores dos serviços do MultiPoint. Em seguida, você criaria contas de usuário para todos os usuários nos serviços do MultiPoint que estão agindo como o servidor de arquivos.

