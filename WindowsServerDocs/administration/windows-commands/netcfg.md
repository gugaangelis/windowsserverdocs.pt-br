---
title: netcfg
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2daaab7-12db-4e36-b70c-db8906d084f7 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5945da45ee01fd5bf5f89a7835c4bae0b5534c4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723800"
---
# <a name="netcfg"></a>netcfg

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Instala o Ambiente de Pré-Instalação do Windows (WinPE), uma versão leve do Windows usada para implantar estações de trabalho.
## <a name="syntax"></a>Sintaxe
```
netcfg [/v] [/e] [/winpe] [/l ] /c /i
```
#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/v|Executar no modo **detalhado** (detalhado)|
|/e|Usar variáveis de **ambiente** de serviço durante a instalação e desinstalação|
|/winpe|Instala TCP/IP, NetBIOS e cliente Microsoft para o ambiente de pré-instalação do Windows (WinPE)|
|/l|Fornece o **local** do inf|
|/c|Fornece a **classe** do componente a ser instalado; protocolo, serviço ou cliente|
|/i|Fornece a **ID** do componente|
|/s|Fornece o tipo de componentes a serem **mostrados**.<p>\tA = adaptadores, n = componentes de rede|
|/b|Exibe os **caminhos de associação**, quando seguido por uma cadeia de caracteres que contém o nome do caminho.|
|/?|Exibe a **ajuda** no prompt de comando.|

## <a name="examples"></a>Exemplos

Para instalar o *exemplo* de protocolo usando c:\oemdir\example.inf:
```
netcfg /l c:\oemdir\example.inf /c p /i example
```
Para instalar o serviço de *MS_Server* :
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
Para desinstalar o *MS_IPX*de componentes:
```
netcfg /u MS_IPX
```
Para mostrar todos os componentes net instalados:
```
netcfg /s n
```
Para exibir os caminhos de associação que contêm *MS_TCPIP*:
```
netcfg /b ms_tcpip
```
## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
