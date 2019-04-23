---
title: Realizar hash prévio e pré-carregar conteúdo no servidor de cache hospedado (opcional)
description: Este guia fornece instruções sobre como implantar o BranchCache no modo de cache hospedado em computadores que executam o Windows Server 2016 e Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b60a1f24b8988d6e394df0faf678467021e0c882
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839387"
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Pré-hash e pré-carregar conteúdo no servidor de Cache hospedado \(opcional\)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar os procedimentos nesta seção para pré-hash do conteúdo em seus servidores de conteúdo, adicione o conteúdo para pacotes de dados e, em seguida, pré-carregar o conteúdo em seus servidores de cache hospedado. 

Esses procedimentos são opcionais porque você não é necessários para prehash e pré-carregamento de conteúdo em seus servidores de cache hospedado. 

Se você não pré-carregar conteúdo, dados são adicionados ao cache hospedado automaticamente como clientes baixá-lo sobre a conexão de WAN.

>[!IMPORTANT]
>Embora esses procedimentos são coletivamente opcionais, se você decidir prehash e pré-carregamento de conteúdo em seus servidores de cache hospedado, é necessário executar os dois procedimentos.

- [Criar pacotes de dados do servidor de conteúdo para a Web e o conteúdo do arquivo &#40;opcional&#41;](8-Bc-Data-Packages.md)
  
- [Importar pacotes de dados no servidor de Cache hospedado &#40;opcional&#41;](9-Bc-Import-Data.md)

Para continuar com este guia, consulte [criar de pacotes de dados de servidor de conteúdo para a Web e o conteúdo do arquivo &#40;opcional&#41;](8-Bc-Data-Packages.md).