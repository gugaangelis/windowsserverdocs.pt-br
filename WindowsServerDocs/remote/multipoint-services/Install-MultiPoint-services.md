---
title: Instalar MultiPoint Services
description: Saiba como instalar e configurar os serviços do MultiPoint no Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 442699afe40ee67e4cd4f13572d1a482f675b84a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395375"
---
# <a name="install-multipoint-services"></a>Instalar MultiPoint Services
Se você estiver instalando um servidor do zero, siga estas instruções para instalar os serviços do MultiPoint.  

Depois de instalar o Windows Server 2016, conecte-se com êxito como administrador. Use o Gerenciador do Servidor em que você pode habilitar os serviços do MultiPoint. O Gerenciador do Servidor é aberto automaticamente na inicialização. No painel, selecione **adicionar funções e recursos** para habilitar os serviços do MultiPoint e siga as instruções no assistente.

Na seção do tipo de instalação, você pode usar o 
- Instalação baseada em função ou em recurso ou
- Instalação do Serviços de Área de Trabalho Remota

Para implantações padrão do MultiPoint Services, recomendamos selecionar a instalação do Serviços de Área de Trabalho Remota, que permite que você selecione convenientemente a função de serviços do MultiPoint em tipo de implantação. Para a instalação baseada em função, você precisará selecionar **Serviços do MultiPoint** na lista de funções. O servidor será reinicializado após a instalação bem-sucedida.  
  
## <a name="configure-your-primary-station"></a>Configurar sua estação principal  
  
1.  Na página **criar uma estação do MultiPoint Server** , digite a letra especificada do teclado para esse monitor. A entrada de chave correta associa o teclado e o mouse para essa estação.  
2.  Entre como administrador.  