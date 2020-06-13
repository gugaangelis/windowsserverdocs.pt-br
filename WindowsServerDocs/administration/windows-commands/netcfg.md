---
title: netcfg
description: Tópico de referência para o comando netcfg, que instala o Ambiente de Pré-Instalação do Windows (WinPE), uma versão leve do Windows usada para implantar estações de trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a308441df55873b205972d703ec52f53345beb5
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721549"
---
# <a name="netcfg"></a>netcfg

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala o Ambiente de Pré-Instalação do Windows (WinPE), uma versão leve do Windows usada para implantar estações de trabalho.

## <a name="syntax"></a>Sintaxe

```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /v | É executado no modo detalhado (detalhado). |
| /e | Usa variáveis de ambiente de manutenção durante a instalação e a desinstalação. |
| /winpe | Instala TCP/IP, NetBIOS e cliente Microsoft para o ambiente de pré-instalação do Windows (WinPE). |
| /l | Fornece o local do arquivo INF. |
| /c | Fornece a classe do componente a ser instalado; **protocolo**, **serviço**ou **cliente**. |
| /i | Fornece a ID do componente. |
| /s | Fornece o tipo de componentes a serem mostrados, incluindo **\tA** para adaptadores ou **n** para componentes net. |
| /b | Exibe os caminhos de associação, quando seguido por uma cadeia de caracteres que contém o nome do caminho. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para instalar o *exemplo* de protocolo usando c:\oemdir\example.inf, digite:

```
netcfg /l c:\oemdir\example.inf /c p /i example
```

Para instalar o serviço de *MS_Server* , digite:

```
netcfg /c s /i MS_Server
```

Para instalar o ambiente de pré-instalação do cliente TCP/IP, NetBIOS e Microsoft para Windows, digite:

```
netcfg /v /winpe
```

Para exibir se o componente *MS_IPX* está instalado, digite:

```
netcfg /q MS_IPX
```

Para desinstalar o componente *MS_IPX*, digite:

```
netcfg /u MS_IPX
```

Para mostrar todos os componentes net instalados, digite:

```
netcfg /s n
```

Para exibir os caminhos de associação que contêm *MS_TCPIP*, digite:

```
netcfg /b ms_tcpip
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
