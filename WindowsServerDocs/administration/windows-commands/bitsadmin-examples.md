---
title: bitsadmin examples
description: Exemplos que mostram como usar a ferramenta Bitsadmin para executar as tarefas mais comuns.
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: 5cf827ebc96c2caf114a9605482a33636689dc25
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894588"
---
# <a name="bitsadmin-examples"></a>bitsadmin examples

Os exemplos a seguir mostram como usar a `bitsadmin` ferramenta para executar as tarefas mais comuns.

## <a name="transfer-a-file"></a>Transferir um arquivo

Para criar um trabalho, adicione arquivos, ative o trabalho na fila de transferência e conclua o trabalho:

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

BITSAdmin continua a mostrar informações de progresso na janela do MS-DOS até que a transferência seja concluída ou um erro ocorra.

## <a name="create-a-download-job"></a>Criar um trabalho de download

Para criar um trabalho de download chamado *myDownloadJob*:

```
bitsadmin /create myDownloadJob
```

BITSAdmin retorna um GUID que identifica exclusivamente o trabalho. Use o GUID ou o nome do trabalho em chamadas subsequentes. O texto a seguir é uma saída de exemplo.

### <a name="sample-output"></a>Saída de exemplo

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>Adicionar arquivos ao trabalho de download

Para adicionar um arquivo ao trabalho:

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

Repita essa chamada para cada arquivo que você deseja adicionar. Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá usar o GUID do trabalho para identificá-lo exclusivamente para conclusão.

## <a name="activate-the-download-job"></a>Ativar o trabalho de download

Depois de criar um novo trabalho, o BITS suspende automaticamente o trabalho. Para ativar o trabalho na fila de transferência:

```
bitsadmin /resume myDownloadJob
```

Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá usar o GUID do trabalho para identificá-lo exclusivamente para conclusão.

## <a name="determine-the-progress-of-the-download-job"></a>Determinar o progresso do trabalho de download

A opção **/info** retorna o estado do trabalho e o número de arquivos e bytes transferidos. Quando o estado é mostrado como `TRANSFERRED` , isso significa que o bits transferiu com êxito todos os arquivos no trabalho. Você também pode adicionar o argumento **/Verbose** para obter detalhes completos do trabalho e **/list** ou **/Monitor** para obter todos os trabalhos na fila de transferência.

Para retornar o estado do trabalho:

```
bitsadmin /info myDownloadJob /verbose
```

Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá usar o GUID do trabalho para identificá-lo exclusivamente para conclusão.

## <a name="complete-the-download-job"></a>Concluir o trabalho de download

Para concluir o trabalho depois que o estado for alterado para `TRANSFERRED` :

```
bitsadmin /complete myDownloadJob
```

Você deve executar a `/complete` opção antes que os arquivos no trabalho fiquem disponíveis. Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá usar o GUID do trabalho para identificá-lo exclusivamente para conclusão.

## <a name="monitor-jobs-in-the-transfer-queue-using-the-list-switch"></a>Monitorar trabalhos na fila de transferência usando a opção/List

Para retornar o estado do trabalho e o número de arquivos e bytes transferidos para todos os trabalhos na fila de transferência:

```
bitsadmin /list
```

### <a name="sample-output"></a>Saída de exemplo

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-monitor-switch"></a>Monitorar trabalhos na fila de transferência usando a opção/monitor

Para retornar o estado do trabalho e o número de arquivos e bytes transferidos para todos os trabalhos na fila de transferência, atualizando os dados a cada 5 segundos:

```
bitsadmin /monitor
```

> [!NOTE]
> Para interromper a atualização, pressione CTRL + C.

### <a name="sample-output"></a>Saída de exemplo

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="monitor-jobs-in-the-transfer-queue-using-the-info-switch"></a>Monitorar trabalhos na fila de transferência usando a opção/info

Para retornar o estado do trabalho e o número de arquivos e bytes transferidos:

```
bitsadmin /info
```

### <a name="sample-output"></a>Saída de exemplo

```
GUID: {482FCAF0-74BF-469B-8929-5CCD028C9499} DISPLAY: myDownloadJob
TYPE: DOWNLOAD STATE: TRANSIENT_ERROR OWNER: domain\user
PRIORITY: NORMAL FILES: 0 / 1 BYTES: 0 / UNKNOWN
CREATION TIME: 12/17/2002 1:21:17 PM MODIFICATION TIME: 12/17/2002 1:21:30 PM
COMPLETION TIME: UNKNOWN
NOTIFY INTERFACE: UNREGISTERED NOTIFICATION FLAGS: 3
RETRY DELAY: 600 NO PROGRESS TIMEOUT: 1209600 ERROR COUNT: 0
PROXY USAGE: PRECONFIG PROXY LIST: NULL PROXY BYPASS LIST: NULL
ERROR FILE:    https://downloadsrv/10mb.zip -> c:\10mb.zip
ERROR CODE:    0x80072ee7 - The server name or address could not be resolved
ERROR CONTEXT: 0x00000005 - The error occurred while the remote file was being
processed.
DESCRIPTION:
JOB FILES:
0 / UNKNOWN WORKING https://downloadsrv/10mb.zip -> c:\10mb.zip
NOTIFICATION COMMAND LINE: none
```

## <a name="delete-jobs-from-the-transfer-queue"></a>Excluir trabalhos da fila de transferência

Para remover todos os trabalhos da fila de transferência, use a opção/Reset reiniciar:

```
bitsadmin /reset
```

### <a name="sample-output"></a>Saída de exemplo

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
