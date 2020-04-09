---
title: Testando a Experiência do Usuário
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 1b1a2040-4cfd-48bf-8d04-3ffde9c26b9b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 938b64d96111a3ed128ec184acde56f20cb1abda
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819779"
---
# <a name="testing-the-customer-experience"></a>Testando a Experiência do Usuário

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para verificar a experiência do usuário e suas personalizações de parceiro, percorra a Configuração Inicial de um computador de destino. Recomenda-se que você conclua a Configuração Inicial ao menos uma vez manualmente para passar pela experiência do usuário. Se você fez uma parceria entre as marcas do Dashboard, é necessário concluir a Configuração Inicial para verificar a identidade visual. Se você comarcar o site de Acesso via Web remoto, terá que acessar o ServerName/< ServerName\> para verificar a identidade visual (< ServerName\> é o nome do servidor). Você pode usar a seção Configuração Inicial do arquivo cfg.ini para automatizar o teste da experiência do usuário. Para obter mais informações sobre a criação dessa seção no arquivo cfg.ini, consulte [Criar o Arquivo Cfg.ini](Create-the-Cfg.ini-File.md).  
  
> [!IMPORTANT]
>  Execute o comando Sysprep.exe para preparar a imagem para implantação antes de testar a experiência da Configuração Inicial. Para obter mais informações sobre a execução do Sysprep.exe, consulte [Preparando a Imagem para Implantação](Preparing-the-Image-for-Deployment.md).  
  
> [!IMPORTANT]
>  Para testar a Configuração Inicial, é necessária uma conexão de rede. O DHCP não está configurado ou instalado no servidor, o que possibilita o teste da rede sem interferência.  
  
 Para verificar as Informações de Suporte de parceiro, no Dashboard, clique na seta para baixo ao lado do botão Ajuda.  
  
## <a name="see-also"></a>Consulte também  
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparar a imagem para implantação](Preparing-the-Image-for-Deployment.md)