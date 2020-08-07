---
title: prnjobs
description: Artigo de referência para o comando prnjobs, que pausa, retoma, cancela e lista trabalhos de impressão.
ms.topic: article
ms.assetid: 5ad34199-7a5a-40c1-8053-bccd5929df43
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: d955f50761e1229e0a1acf21a9f2179525bd7ee4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884739"
---
# <a name="prnjobs"></a>prnjobs

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pausa, retoma, cancela e lista trabalhos de impressão. Esse comando é um script Visual Basic localizado no `%WINdir%\System32\printing_Admin_Scripts\<language>` diretório. Para usar esse comando em um prompt de comando, digite **cscript** seguido pelo caminho completo para o arquivo prnjobs ou altere os diretórios para a pasta apropriada. Por exemplo: `cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnjobs.vbs`.

## <a name="syntax"></a>Sintaxe

```
cscript prnjobs {-z | -m | -x | -l | -?} [-s <Servername>] [-p <Printername>] [-j <JobID>] [-u <Username>] [-w <password>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -Z | Pausa o trabalho de impressão especificado pelo parâmetro **-j** . |
| -M | Retoma o trabalho de impressão especificado pelo parâmetro **-j** . |
| -X | Cancela o trabalho de impressão especificado pelo parâmetro **-j** . |
| -l | Lista todos os trabalhos de impressão em uma fila de impressão. |
| -s`<Servername>` | Especifica o nome do computador remoto que hospeda a impressora que você deseja gerenciar. Se você não especificar um computador, o computador local será usado. |
| -p`<Printername>` | Obrigatórios. Especifica o nome da impressora que você deseja gerenciar. |
| -j`<JobID>` | Especifica (por número de ID) o trabalho de impressão que você deseja cancelar. |
| -u `<Username>` -w`<password>` | Especifica uma conta com permissões para se conectar ao computador que hospeda a impressora que você deseja gerenciar. Todos os membros do grupo de administradores locais do computador de destino têm essas permissões, mas as permissões também podem ser concedidas a outros usuários. Se você não especificar uma conta, deverá estar conectado sob uma conta com essas permissões para que o comando funcione. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, "nome do computador").

### <a name="examples"></a>Exemplos

Para pausar um trabalho de impressão com uma ID de trabalho de 27 enviada para o computador remoto chamado HRServer para impressão na impressora denominada colorprinter, digite:

```
cscript prnjobs.vbs -z -s HRServer -p colorprinter -j 27
```

Para listar todos os trabalhos de impressão atuais na fila para a impressora local denominada colorprinter_2, digite:

```
cscript prnjobs.vbs -l -p colorprinter_2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
