---
title: Testando a Experiência do Usuário
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838687"
---
# <a name="testing-the-customer-experience"></a>Testando a Experiência do Usuário

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para verificar a experiência do usuário e suas personalizações de parceiro, percorra a Configuração Inicial de um computador de destino. Recomenda-se que você conclua a Configuração Inicial ao menos uma vez manualmente para passar pela experiência do usuário. Se você fez uma parceria entre as marcas do Dashboard, é necessário concluir a Configuração Inicial para verificar a identidade visual. Se você cobranded o acesso via Web remoto, você deve acessar http://<servername\> para verificar a identidade visual (< servername\> é o nome do servidor). Você pode usar a seção Configuração Inicial do arquivo cfg.ini para automatizar o teste da experiência do usuário. Para obter mais informações sobre a criação dessa seção no arquivo cfg.ini, consulte [Criar o Arquivo Cfg.ini](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Execute o comando Sysprep.exe para preparar a imagem para implantação antes de testar a experiência da Configuração Inicial. Para obter mais informações sobre a execução Sysprep.exe, consulte [Preparing the Image for Deployment](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Para testar a Configuração Inicial, é necessária uma conexão de rede. O DHCP não está configurado ou instalado no servidor, o que possibilita o teste da rede sem interferência.  
  
 Para verificar as Informações de Suporte de parceiro, no Dashboard, clique na seta para baixo ao lado do botão Ajuda.  
  
## <a name="see-also"></a>Consulte também  
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)