---
title: netcfg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aed535f843da6d735526ea97c07f94564dc00dc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871317"
---
# <a name="netcfg"></a>netcfg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala o ambiente de pré-instalação do Windows (WinPE), uma versão leve do Windows usada para implantar estações de trabalho.   
## <a name="syntax"></a>Sintaxe  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/v|Executar em modo detalhado de (detalhado)|  
|/e|Usar variáveis de ambiente do serviço durante a instalação e desinstalação|  
|/winpe|Instala o TCP/IP, NetBIOS e o cliente da Microsoft para o ambiente de pré-instalação do Windows|  
|/l|Fornece a localização do INF|  
|/c|Fornece a classe do componente a ser instalado; protocolo, serviço ou cliente|  
|/i|Fornece a ID do componente|  
|/s|Fornece o tipo de componentes para mostrar<br /><br />\tA = adaptadores, n = componentes de rede|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="BKMK_Examples"></a>Exemplos  
Para instalar o protocolo *exemplo* usando c:\oemdir\example.inf:  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
Para instalar o *MS_Server* serviço:  
```  
netcfg /c s /i MS_Server  
```  
Para instalar o TCP/IP, NetBIOS e o Microsoft Client para o ambiente de pré-instalação do Windows  
```  
netcfg /v /winpe  
```  
Para exibir se o componente *MS_IPX* está instalado:  
```  
netcfg /q MS_IPX  
```  
Para desinstalar o componente *MS_IPX*:  
```  
netcfg /u MS_IPX  
```  
Para mostrar todos os componentes de rede instalados:  
```  
netcfg /s n  
```  
A apresentações de associação de caminhos que contenham *MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)  
