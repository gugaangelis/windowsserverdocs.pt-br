---
title: Configurar servidores de conteúdo do WSUS (Windows Server Update Services)
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2993c78e85609ac720fd208971dda7ed67a3610d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319159"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurar servidores de conteúdo do WSUS (Windows Server Update Services)

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Depois de instalar o recurso BranchCache e iniciar o serviço BranchCache, é necessário configurar os servidores do WSUS para armazenar arquivos de atualização no computador local. 

Ao configurar servidores do WSUS para armazenar arquivos de atualização no computador local, os metadados de atualização e os arquivos de atualização são baixados e armazenados diretamente no servidor do WSUS. Isso garante que os computadores cliente BranchCache recebam os arquivos de atualização dos produtos da Microsoft do servidor do WSUS e não diretamente do site do Microsoft Update.  
  
Para obter mais informações sobre a sincronização do WSUS, consulte [Configurando sincronizações de atualização](https://technet.microsoft.com/library/mt612311.aspx)  