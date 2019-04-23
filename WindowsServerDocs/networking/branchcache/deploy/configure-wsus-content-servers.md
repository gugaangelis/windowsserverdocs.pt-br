---
title: Configurar servidores de conteúdo do WSUS (Windows Server Update Services)
description: Este tópico faz parte do BranchCache Deployment Guide para Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8576282be92f02daf716da82ea75eddc755ee5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873837"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurar servidores de conteúdo do WSUS (Windows Server Update Services)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de instalar o recurso BranchCache e iniciar o serviço BranchCache, é necessário configurar os servidores do WSUS para armazenar arquivos de atualização no computador local. 

Ao configurar servidores do WSUS para armazenar arquivos de atualização no computador local, os metadados de atualização e os arquivos de atualização são baixados e armazenados diretamente no servidor do WSUS. Isso garante que os computadores cliente BranchCache recebam os arquivos de atualização dos produtos da Microsoft do servidor do WSUS e não diretamente do site do Microsoft Update.  
  
Para obter mais informações sobre a sincronização do WSUS, consulte [configurar sincronizações de atualização](https://technet.microsoft.com/library/mt612311.aspx)  