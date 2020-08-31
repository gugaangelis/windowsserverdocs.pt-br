---
title: mstsc
description: Artigo de referência para o comando mstsc, que cria conexões com Host da Sessão da Área de Trabalho Remota servidores ou outros computadores remotos, edita um arquivo de configuração existente do Conexão de Área de Trabalho Remota (. RDP) e migra os arquivos de conexão herdados que foram criados com o Gerenciador de conexões do cliente para novos arquivos de conexão. rdp.
ms.topic: reference
ms.assetid: 59801227-1e7e-4dbd-96e6-f54102a3ce92
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0a9c1eb46e2d6802c50dfc89e4a3085e6a6afca2
ms.sourcegitcommit: fe356f95188b7ce8e719765f44c0789c065832fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2020
ms.locfileid: "89057558"
---
# <a name="mstsc"></a>mstsc

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria conexões com servidores Host da Sessão da Área de Trabalho Remota ou outros computadores remotos, edita um arquivo de configuração existente Conexão de Área de Trabalho Remota (. RDP) e migra os arquivos de conexão herdados que foram criados com o Gerenciador de conexões do cliente para novos arquivos de conexão. rdp.

## <a name="syntax"></a>Sintaxe

```
mstsc.exe [<connectionfile>] [/v:<server>[:<port>]] [/admin] [/f] [/w:<width> /h:<height>] [/public] [/span]
mstsc.exe /edit <connectionfile>
mstsc.exe /migrate
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ------------|
| `<connectionfile>` | Especifica o nome de um arquivo. rdp para a conexão. |
| /v:`<server>[:<port>]` | Especifica o computador remoto e, opcionalmente, o número da porta à qual você deseja se conectar. |
| /admin | Conecta você a uma sessão para administrar o servidor. |
| /f | Inicia Conexão de Área de Trabalho Remota no modo de tela inteira. |
| /w`<width>` | Especifica a largura da janela de Área de Trabalho Remota. |
| /h`<height>` | Especifica a altura da janela de Área de Trabalho Remota. |
| /Public | Executa Área de Trabalho Remota no modo público. No modo público, as senhas e os bitmaps não são armazenados em cache. |
| /span | Faz a correspondência entre a largura e a altura de Área de Trabalho Remota com a área de trabalho virtual local, abrangendo vários monitores, se necessário. |
| /edit `<connectionfile>` | Abre o arquivo. rdp especificado para edição. |
| /migrate | Migra os arquivos de conexão herdados que foram criados com o Gerenciador de conexões de cliente para novos arquivos de conexão. rdp. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Default. rdp é armazenado para cada usuário como um arquivo oculto na pasta **documentos** do usuário.

- Os arquivos. rdp criados pelo usuário são salvos por padrão na pasta **documentos** do usuário, mas podem ser salvos em qualquer lugar.

- Para abranger os monitores, os monitores devem usar a mesma resolução e devem estar alinhados horizontalmente (ou seja, lado a lado). Atualmente, não há suporte para abranger vários monitores verticalmente no sistema cliente.

### <a name="examples"></a>Exemplos

Para se conectar a uma sessão no modo de tela inteira, digite:

```
mstsc /f
```
ou
```
mstsc /v:computer1 /f
```
Para atribuir largura/altura, digite:

```
mstsc /v:computer1 /w:1920 /h:1080
```
Para abrir um arquivo chamado *filename. rdp* para edição, digite:

```
mstsc /edit filename.rdp
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
