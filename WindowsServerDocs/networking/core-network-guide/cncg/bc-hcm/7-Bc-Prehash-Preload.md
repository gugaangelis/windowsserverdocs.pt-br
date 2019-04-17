---
title: Prehash e pré-carregar conteúdo no servidor de Cache hospedado (opcional)
description: Este guia fornece instruções sobre como implantar BranchCache em modo de cache hospedado em computadores que executam o Windows Server 2016 e o Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 7e79c66a-8555-4d8e-8691-d6c37377aab4
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0e7ffaac4e427222d5539195ecef91768f61c4a3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="prehash-and-preload-content-on-the-hosted-cache-server-optional"></a>Prehash e pré-carregar o conteúdo no hospedado Cache \(Optional\) Server

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você pode usar os procedimentos desta seção prehash conteúdo em seus servidores de conteúdo, adicione o conteúdo para pacotes de dados e, em seguida, pré-carregar o conteúdo em seus servidores de cache hospedado. 

Esses procedimentos são opcionais, porque não é necessário para conteúdo prehash e pré-carregamento em seus servidores de cache hospedado. 

Se você não pré-carregar conteúdo, dados são adicionados para o cache hospedado automaticamente como clientes baixá-lo sobre a conexão WAN.

>[!IMPORTANT]
>Embora esses procedimentos são coletivamente opcionais, se você decidir prehash e pré-carregamento de conteúdo em seus servidores de cache hospedado, executar os dois procedimentos é necessária.

- [Criar pacotes de dados do servidor de conteúdo para a Web e o conteúdo do arquivo e 40; opcional & #41;](8-Bc-Data-Packages.md)
  
- [Importar pacotes de dados no servidor o Cache hospedado & #40; opcional & #41;](9-Bc-Import-Data.md)

Para continuar com este guia, consulte [criar conteúdo pacotes do servidor de dados para a Web e conteúdo do arquivo #40 opcional &; 41; ](8-Bc-Data-Packages.md).