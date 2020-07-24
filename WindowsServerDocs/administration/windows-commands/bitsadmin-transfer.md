---
title: bitsadmin transfer
description: Artigo de referência para o comando Bitsadmin Transfer, que transfere um ou mais arquivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b2d03fb379c879f445a30dd0f3daf762fed23c7
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955428"
---
# <a name="bitsadmin-transfer"></a>bitsadmin transfer

Transfere um ou mais arquivos. Por padrão, o serviço BITSAdmin cria um trabalho de download que é executado em prioridade **normal** e atualiza a janela de comando com informações de progresso até que a transferência seja concluída ou até que ocorra um erro crítico,

O serviço concluirá o trabalho se ele transferir com êxito todos os arquivos e cancelar o trabalho se ocorrer um erro crítico. O serviço não criará o trabalho se não for possível adicionar arquivos ao trabalho ou se você especificar um valor inválido para o *tipo* ou *job_priority*. Para transferir mais de um arquivo, especifique vários `<RemoteFileName>-<LocalFileName>` pares. Os pares devem ser delimitados por espaço.

> [!NOTE]
> O comando BITSAdmin continuará a ser executado se ocorrer um erro transitório. Para encerrar o comando, pressione CTRL + C.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /transfer <name> [<type>] [/priority <job_priority>] [/ACLflags <flags>] [/DYNAMIC] <remotefilename> <localfilename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| name | O nome do trabalho. Este comando não pode ser um GUID. |
| tipo | Opcional. Define o tipo de trabalho, incluindo:<ul><li>**Baixar.** O valor padrão. Escolha esse tipo para baixar trabalhos.</li><li>**Carregar.** Escolha este tipo para trabalhos de carregamento.</li></ul> |
| priority | Opcional. Define a prioridade do trabalho, incluindo:<ul><li>FOREGROUND</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |
| ACLflags | Opcional. Indica que você deseja manter as informações de proprietário e ACL com o arquivo que está sendo baixado. Especifique um ou mais dos valores, incluindo:<ul><li>**o** -copie as informações do proprietário com o arquivo.</li><li>**g** -copiar informações do grupo com o arquivo.</li><li>**d** – copiar informações de DACL (lista de controle de acesso discricionário) com o arquivo.</li><li>**s** -copiar informações de SACL (lista de controle de acesso) do sistema com o arquivo.</li></ul> |
| /DYNAMIC | Configura o trabalho usando [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/win32/api/bits5_0/ne-bits5_0-bits_job_property_id), que libera os requisitos do lado do servidor. |
| remotefilename | O nome do arquivo depois que ele é transferido para o servidor. |
| localfilename | O nome do arquivo que reside localmente. |

## <a name="examples"></a>Exemplos

Para iniciar um trabalho de transferência chamado *myDownloadJob*:

```
bitsadmin /transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
