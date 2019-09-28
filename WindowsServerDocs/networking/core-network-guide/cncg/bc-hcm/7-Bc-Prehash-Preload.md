---
title: Realizar hash prévio e pré-carregar conteúdo no servidor de cache hospedado (opcional)
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe206576278b09e4a360c7bb27f5ff076af97be7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356250"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Pré-hash e pré-carregamento de conteúdo no servidor de cache hospedado \(Optional @ no__t-1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar os procedimentos nesta seção para fazer o autohash de conteúdo em seus servidores de conteúdo, adicionar o conteúdo a pacotes de dados e, em seguida, pré-carregar o conteúdo em seus servidores de cache hospedados. 

Esses procedimentos são opcionais porque não é necessário prefazer o hash e pré-carregar o conteúdo em seus servidores de cache hospedados. 

Se você não pré-carregar o conteúdo, os dados são adicionados automaticamente ao cache hospedado, pois os clientes o baixam pela conexão WAN.

>[!IMPORTANT]
>Embora esses procedimentos sejam coletivamente opcionais, se você decidir prehash e pré-carregar conteúdo em seus servidores de cache hospedados, a execução de ambos os procedimentos será necessária.

- [Criar pacotes de dados do servidor de conteúdo para conteúdo &#40;da Web e de arquivo opcional&#41;](8-Bc-Data-Packages.md)
  
- [Importar pacotes de dados no servidor &#40;de cache hospedado opcional&#41;](9-Bc-Import-Data.md)

Para continuar com este guia, consulte [criar pacotes de dados do servidor de conteúdo para conteúdo &#40;da&#41;Web e de arquivo opcional](8-Bc-Data-Packages.md).