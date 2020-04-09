---
title: exemplos de Bitsadmin
description: Os exemplos a seguir mostram como usar a ferramenta Bitsadmin para executar as tarefas mais comuns.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: 96447410f76e4402c456b5ec402cc730480aedaf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850799"
---
# <a name="bitsadmin-examples"></a>exemplos de Bitsadmin

Os exemplos a seguir mostram como usar a ferramenta de `bitsadmin` para executar as tarefas mais comuns.

## <a name="transfer-a-file"></a>Transferir um arquivo

A opção **/Transfer** é um atalho para executar as tarefas listadas abaixo. Essa opção cria o trabalho, adiciona os arquivos ao trabalho, ativa o trabalho na fila de transferência e conclui o trabalho. BITSAdmin continua a mostrar informações de progresso na janela do MS-DOS até que a transferência seja concluída ou um erro ocorra.

`bitsadmin /transfer myDownloadJob /download /priority normal https://downloadsrv/10mb.zip c:\\10mb.zip`

## <a name="create-a-download-job"></a>Criar um trabalho de download

Use a opção **/Create** para criar um trabalho de download chamado myDownloadJob.

### <a name="syntax"></a>Sintaxe

```
bitsadmin /create myDownloadJob
```

BITSAdmin retorna um GUID que identifica exclusivamente o trabalho. Use o GUID ou o nome do trabalho em chamadas subsequentes. O texto a seguir é uma saída de exemplo.

#### <a name="sample-output"></a>Saída de exemplo

`created job {C775D194-090F-431F-B5FB-8334D00D1CB6}`

## <a name="add-files-to-the-download-job"></a>Adicionar arquivos ao trabalho de download

Use a opção **/AddFile** para adicionar um arquivo ao trabalho. Repita essa chamada para cada arquivo que você deseja adicionar.

Se vários trabalhos usarem myDownloadJob como seu nome, você deverá substituir myDownloadJob pelo GUID do trabalho para identificar exclusivamente o trabalho.

### <a name="syntax"></a>Sintaxe

```
bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip
```

## <a name="activate-the-download-job"></a>Ativar o trabalho de download

Quando você cria um novo trabalho, o BITS suspende o trabalho. Para ativar o trabalho na fila de transferência, use a opção **/resume** .

Se vários trabalhos usarem myDownloadJob como seu nome, você deverá substituir myDownloadJob pelo GUID do trabalho para identificar exclusivamente o trabalho.

### <a name="syntax"></a>Sintaxe

`bitsadmin /resume myDownloadJob`

## <a name="determine-the-progress-of-the-download-job"></a>Determinar o progresso do trabalho de download

Use a opção **/info** para retornar o estado do trabalho e o número de arquivos e bytes transferidos. Quando o estado é transferido, o BITS transferiu com êxito todos os arquivos no trabalho.

- Use o argumento **/Verbose** para obter detalhes completos do trabalho.

- Use a opção **/list** ou **/Monitor** para obter todos os trabalhos na fila de transferência.

Se vários trabalhos usarem myDownloadJob como seu nome, você deverá substituir myDownloadJob pelo GUID do trabalho para identificar exclusivamente o trabalho.

### <a name="syntax"></a>Sintaxe

`bitsadmin /info myDownloadJob /verbose`

#### <a name="sample-output"></a>Saída de exemplo

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

## <a name="completing-the-download-job"></a>Concluindo o trabalho de download

Quando o estado do trabalho for transferido, o BITS transferiu com êxito todos os arquivos no trabalho. No entanto, os arquivos não estarão disponíveis até que você use a opção **/Complete**

Se vários trabalhos usarem myDownloadJob como seu nome, você deverá substituir myDownloadJob pelo GUID do trabalho para identificar exclusivamente o trabalho.

### <a name="syntax"></a>Sintaxe

`bitsadmin /complete myDownloadJob`

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Monitoramento de trabalhos na fila de transferência

Use a opção **/list**, **/Monitor**ou **/info** para monitorar trabalhos na fila de transferência. A opção **/list** fornece informações para todos os trabalhos na fila.

## <a name="list-switch"></a>opção/List

A opção **/list** retorna o estado do trabalho e o número de arquivos e bytes transferidos para todos os trabalhos na fila de transferência.

### <a name="syntax"></a>Sintaxe

`bitsadmin /list`

#### <a name="sample-output-for-the-list-switch"></a>Exemplo de saída para a opção/List

```
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

## <a name="monitor-switch"></a>comutador/monitor

A opção **/Monitor** retorna o estado do trabalho e o número de arquivos e bytes transferidos para todos os trabalhos na fila de transferência, atualizando os dados a cada 5 segundos. Para interromper a atualização, pressione CTRL + C.

### <a name="syntax"></a>Sintaxe

`bitsadmin /monitor`

#### <a name="sample-output"></a>Saída de exemplo

```
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Excluindo trabalhos da fila de transferência

Use a opção **/Reset reiniciar** para remover todos os trabalhos da fila de transferência.

### <a name="syntax"></a>Sintaxe

`bitsadmin /reset`

#### <a name="sample-output"></a>Saída de exemplo

```
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
