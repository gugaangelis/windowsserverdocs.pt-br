---
title: "Testando a experiência do cliente"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 223b0e1be3a53e9a7d198dc005fc8725e421db58
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="testing-the-customer-experience"></a>Testando a experiência do cliente

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para verificar a experiência do cliente e verificar suas personalizações de parceiro, executados por meio da configuração inicial de um computador de destino. É recomendável que você conclua a configuração inicial pelo menos uma vez manualmente para percorrer a experiência do cliente. Se tiver usado co-brandindo para o painel, você deve concluir a configuração inicial para verificar a identidade visual. Se tiver usado co-brandindo para o site de acesso via Web remoto, você deve acessar http://<servername\> para verificar a identidade visual (< servername\ > é o nome do servidor). Você pode usar a seção de configuração inicial do arquivo cfg.ini para automatizar testes da experiência do cliente. Para obter mais informações sobre como criar esta seção no arquivo cfg.ini veja [criá-lo Cfg.ini](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Você deve executar o comando Sysprep.exe para preparar a imagem para implantação antes de testar a experiência de configuração inicial. Para obter mais informações sobre como executar Sysprep.exe, consulte [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Uma conexão de rede é necessária para testar a configuração inicial. DHCP não é configurado ou instalado no servidor, que permite que os testes de rede sem interferência.  
  
 Para verificar o parceiro de informações de suporte, no painel, clique na seta para baixo ao lado do botão Ajuda.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)