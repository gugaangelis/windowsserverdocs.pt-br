---
title: exemplos de Bitsadmin
description: Os exemplos a seguir mostram como usar a ferramenta bitsadmin para executar as tarefas mais comuns.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb8f8374-ba6e-4a68-85a1-9a95b8215354
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/31/2018
ms.openlocfilehash: a98e1a876c972b0f146ff37aff0a77399b684e99
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308557"
---
# <a name="bitsadmin-examples"></a>exemplos de Bitsadmin

Os exemplos a seguir mostram como usar o `bitsadmin` ferramenta para executar as tarefas mais comuns.

## <a name="transfer-a-file"></a>Transfira um arquivo

O **/transferência** switch é um atalho para executar as tarefas listadas abaixo. Essa opção cria o trabalho, adiciona os arquivos para o trabalho, ativa o trabalho na fila de transferência e conclui o trabalho. BITSAdmin continua Mostrar informações sobre o andamento na janela do MS-DOS, até que a transferência for concluída ou ocorre um erro.

**Bitsadmin /transfer myDownloadJob /download /priority normal `https://downloadsrv/10mb.zip c:\\10mb.zip`**

## <a name="create-a-download-job"></a>Criar um trabalho de download

Use o **/ criar** switch para criar um trabalho de download nomeado myDownloadJob.

**Bitsadmin / criar myDownloadJob**

BITSAdmin retorna um GUID que identifica exclusivamente o trabalho. Use o nome GUID ou o trabalho nas chamadas subsequentes. O texto a seguir é um exemplo de saída.

``` syntax
Created job {C775D194-090F-431F-B5FB-8334D00D1CB6}.
```

Em seguida, use o **/AddFile.** switch para adicionar um ou mais arquivos para o trabalho de download.

## <a name="add-files-to-the-download-job"></a>Adicionar arquivos ao trabalho de download

Use o **/AddFile.** switch para adicionar um arquivo para o trabalho. Repita esta chamada para cada arquivo que você deseja adicionar. Se vários trabalhos usam myDownloadJob como seu nome, você deve substituir myDownloadJob com o GUID do trabalho para identificar exclusivamente o trabalho.

**Bitsadmin /addfile myDownloadJob https://downloadsrv/10mb.zip c:\\10mb.zip**

Para ativar o trabalho na fila de transferência, use o **/retomar** alternar.

## <a name="activate-the-download-job"></a>Ativar o trabalho de download

Quando você cria um novo trabalho, o BITS suspende o trabalho. Para ativar o trabalho na fila de transferência, use o **/retomar** alternar. Se vários trabalhos usam myDownloadJob como seu nome, você deve substituir myDownloadJob com o GUID do trabalho para identificar exclusivamente o trabalho.

**Bitsadmin /resume myDownloadJob**

Para determinar o andamento do trabalho, use o **/List**, **/info**, ou **/monitor** alternar.

## <a name="determine-the-progress-of-the-download-job"></a>Determinar o andamento do trabalho de download

Use o **/info** switch para determinar o progresso de um trabalho. Se vários trabalhos usam myDownloadJob como seu nome, você deve substituir myDownloadJob com o GUID do trabalho para identificar exclusivamente o trabalho.

**Bitsadmin /info myDownloadJob /verbose**

O **/info** switch retorna o estado do trabalho e o número de arquivos e bytes transferidos. Quando o estado é transferido, o BITS foi transferida com êxito todos os arquivos no trabalho. O **/verbose** argumento fornece detalhes completos do trabalho. O texto a seguir é um exemplo de saída.

``` syntax
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

Para receber informações de todos os trabalhos na fila de transferência, use o **/List** ou **/monitor** alternar.

## <a name="completing-the-download-job"></a>Concluindo o trabalho de download

Quando o estado do trabalho é transferido, o BITS foi transferida com êxito todos os arquivos no trabalho. No entanto, os arquivos não estão disponíveis até que você use o **/ completo** alternar. Se vários trabalhos usam myDownloadJob como seu nome, você deve substituir myDownloadJob com o GUID do trabalho para identificar exclusivamente o trabalho.

**Bitsadmin / concluir myDownloadJob**

## <a name="monitoring-jobs-in-the-transfer-queue"></a>Monitoramento de trabalhos na fila de transferência

Use o **/lista**, **/monitor**, ou **/info** switch para monitorar trabalhos na fila de transferência. O **/lista** switch fornece informações para todos os trabalhos na fila.

**/list Bitsadmin**

O **/lista** switch retorna o estado do trabalho e o número de bytes transferidos para todos os trabalhos na fila de transferência e de arquivos. O texto a seguir é um exemplo de saída.

``` syntax
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN

Listed 2 job(s).
```

Use o **/monitor** switch para monitorar todos os trabalhos na fila. O **/monitor** switch atualiza os dados a cada 5 segundos. Para interromper a atualização, pressione CTRL + C.

**Bitsadmin /monitor**

O **/monitor** switch retorna o estado do trabalho e o número de bytes transferidos para todos os trabalhos na fila de transferência e de arquivos. O texto a seguir é um exemplo de saída.

``` syntax
MONITORING BACKGROUND COPY MANAGER(5 second refresh)
{6AF46E48-41D3-453F-B7AF-A694BBC823F7} job1 SUSPENDED 0 / 0 0 / 0
{482FCAF0-74BF-469B-8929-5CCD028C9499} job2 TRANSIENT_ERROR 0 / 1 0 / UNKNOWN
{0B138008-304B-4264-B021-FD04455588FF} job3 TRANSFERRED 1 / 1 100379370 / 100379370
```

## <a name="deleting-jobs-from-the-transfer-queue"></a>Excluir trabalhos da fila de transferência

Use o **/redefinir** switch para remover todos os trabalhos de fila de transferência.

**/reset Bitsadmin**

O texto a seguir é um exemplo de saída.

``` syntax
{DC61A20C-44AB-4768-B175-8000D02545B9} canceled.
{BB6E91F3-6EDA-4BB4-9E01-5C5CBB5411F8} canceled.
2 out of 2 jobs canceled.
```
