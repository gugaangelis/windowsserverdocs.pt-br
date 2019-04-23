---
title: Instalar MultiPoint Services
description: Saiba como instalar e configurar o MultiPoint Services no Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 52a824bbca3e9f2e1c7823601f6208ae19ae50ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866217"
---
# <a name="install-multipoint-services"></a>Instalar MultiPoint Services
Se você estiver instalando um servidor do zero, siga estas instruções para instalar o MultiPoint Services.  

Depois de ter instalado o Windows Server 2016 com êxito entre como administrador. Use o Gerenciador do servidor onde você pode habilitar o MultiPoint Services. O Gerenciador do servidor é aberto automaticamente na inicialização. No, selecione painel **adicionar funções e recursos** para habilitar o MultiPoint Services e siga as instruções no assistente.

Na seção para o tipo de instalação, você pode ir com o 
- Instalação baseada em função ou recurso ou
- Instalação de serviços de área de trabalho remota

Para implantações padrão do MultiPoint Services, é recomendável para selecionar a instalação dos serviços de área de trabalho remota que permite que você selecione convenientemente a função do MultiPoint Services em tipo de implantação. Para a instalação baseada em função, será necessário selecionar **MultiPoint Services** na lista de funções. O servidor será reiniciado após a instalação bem-sucedida.  
  
## <a name="configure-your-primary-station"></a>Configurar sua estação principal  
  
1.  Sobre o **criar uma estação do MultiPoint Server** página, digite a letra especificada usando o teclado do monitor. A entrada de chave correta associa o teclado e mouse para essa estação.  
2.  Entre como administrador.  