---
title: netcfg
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8f8368aaff16592a55cc9def84d593cf323f28ee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373295"
---
# <a name="netcfg"></a>netcfg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala o Ambiente de Pré-Instalação do Windows (WinPE), uma versão leve do Windows usada para implantar estações de trabalho.   
## <a name="syntax"></a>Sintaxe  
```  
netcfg [/v] [/e] [/winpe] [/l ] /c /i  
```  
### <a name="parameters"></a>Parâmetros  
|Parâmetro|Descrição|  
|-------|--------|  
|/v|Executar no modo detalhado (detalhado)|  
|/e|Usar variáveis de ambiente de serviço durante a instalação e desinstalação|  
|/winpe|Instala TCP/IP, NetBIOS e cliente Microsoft para decisões de pré-instalação do Windows|  
|/l|Fornece o local do INF|  
|/c|Fornece a classe do componente a ser instalado; protocolo, serviço ou cliente|  
|/i|Fornece a ID do componente|  
|/s|Fornece o tipo de componentes a serem mostrados<br /><br />\tA = adaptadores, n = componentes de rede|  
|/?|Exibe a ajuda no prompt de comando.|  
## <a name="BKMK_Examples"></a>Disso  
Para instalar o *exemplo* de protocolo usando c:\oemdir\example.inf:  
```  
netcfg /l c:\oemdir\example.inf /c p /i example  
```  
Para instalar o serviço *MS_Server* :  
```  
netcfg /c s /i MS_Server  
```  
Para instalar o ambiente de pré-instalação TCP/IP, NetBIOS e cliente Microsoft para Windows  
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
Para mostrar todos os componentes net instalados:  
```  
netcfg /s n  
```  
Para mostrar os caminhos de associação que contêm *MS_TCPIP*:  
```  
netcfg /b ms_tcpip  
```  
## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
