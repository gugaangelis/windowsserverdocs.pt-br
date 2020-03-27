---
title: Realizar hash prévio e pré-carregar conteúdo no servidor de cache hospedado (opcional)
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f47fbb5c10828d16a18b5cf486e620b3ace458c7
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318458"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Pré-hash e pré-carregamento de conteúdo no servidor de cache hospedado \(\) opcional

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar os procedimentos nesta seção para fazer o autohash de conteúdo em seus servidores de conteúdo, adicionar o conteúdo a pacotes de dados e, em seguida, pré-carregar o conteúdo em seus servidores de cache hospedados. 

Esses procedimentos são opcionais porque não é necessário prefazer o hash e pré-carregar o conteúdo em seus servidores de cache hospedados. 

Se você não pré-carregar o conteúdo, os dados são adicionados automaticamente ao cache hospedado, pois os clientes o baixam pela conexão WAN.

>[!IMPORTANT]
>Embora esses procedimentos sejam coletivamente opcionais, se você decidir prehash e pré-carregar conteúdo em seus servidores de cache hospedados, a execução de ambos os procedimentos será necessária.

- [Criar pacotes de dados do servidor de conteúdo para conteúdo &#40;da Web e de arquivo opcional&#41;](8-Bc-Data-Packages.md)
  
- [Importar pacotes de dados no servidor &#40;de cache hospedado opcional&#41;](9-Bc-Import-Data.md)

Para continuar com este guia, consulte [criar pacotes de dados do servidor de conteúdo para conteúdo &#40;da&#41;Web e de arquivo opcional](8-Bc-Data-Packages.md).