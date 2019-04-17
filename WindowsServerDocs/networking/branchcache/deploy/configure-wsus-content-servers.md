---
title: Configurar servidores de conteúdo do Windows Server Update Services (WSUS)
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8200a0905f7bc5c403288a22faece5f84eac8af9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurar servidores de conteúdo do Windows Server Update Services (WSUS)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Depois de instalar o recurso BranchCache e iniciar o serviço BranchCache, servidores WSUS devem ser configurados para armazenar arquivos de atualização no computador local. 

Quando você configura servidores WSUS para armazenar arquivos de atualização no computador local, os metadados de atualização e os arquivos de atualização são baixados por e armazenados diretamente após o servidor WSUS. Isso garante que os computadores cliente BranchCache recebem arquivos de atualização do produto Microsoft do servidor WSUS e não diretamente da Website Microsoft Update.  
  
Para obter mais informações sobre a sincronização do WSUS, consulte [Configurando sincronizações de atualização](https://technet.microsoft.com/en-us/library/mt612311.aspx)  