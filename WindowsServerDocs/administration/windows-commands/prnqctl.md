---
title: prnqctl
description: Tópico de referência para o comando prnqctl, que imprime uma página de teste e pausa ou retoma uma impressora.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8df9dfa7-984c-4276-bb7d-e7675e7c399e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: b8a551e34754771a69af1b41e5da3fd726df1185
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472191"
---
# <a name="prnqctl"></a>prnqctl

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imprime uma página de teste, pausa ou retoma uma impressora e limpa uma fila de impressora. Esse comando é um script Visual Basic localizado no `%WINdir%\System32\printing_Admin_Scripts\<language>` diretório. Para usar esse comando em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prnqctl ou altere os diretórios para a pasta apropriada. Por exemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnqctl`.

## <a name="syntax"></a>Sintaxe

```
cscript Prnqctl {-z | -m | -e | -x | -?} [-s <Servername>] [-p <Printername>] [-u <Username>] [-w <password>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -Z | Pausa a impressão na impressora especificada pelo parâmetro **-p** . |
| -M | Retoma a impressão na impressora especificada pelo parâmetro **-p** . |
| -E | Imprime uma página de teste na impressora especificada pelo parâmetro **-p** . |
| -X | Cancela todos os trabalhos de impressão na impressora especificada pelo parâmetro **-p** . |
| -s`<Servername>` | Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado. |
| -p`<Printername>` | Obrigatórios. Especifica o nome da impressora que você deseja gerenciar. |
| -u `<Username>` -w`<password>` | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, "nome do computador").

### <a name="examples"></a>Exemplos

Para imprimir uma página de teste na impressora Laserprinter1 compartilhada pelo \\ computador Server1, digite:

```
cscript prnqctl -e -s Server1 -p Laserprinter1
```

Para pausar a impressão na impressora Laserprinter1 no computador local, digite:

```
cscript prnqctl -z -p Laserprinter1
```

Para cancelar todos os trabalhos de impressão na impressora Laserprinter1 no computador local, digite:

```
cscript prnqctl -x -p Laserprinter1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
