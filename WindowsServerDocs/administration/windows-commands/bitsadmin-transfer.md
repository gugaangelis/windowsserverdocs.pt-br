---
title: bitsadmin Transfer
description: Tópico de comandos do Windows para **transferência de Bitsadmin** – transfere um ou mais arquivos.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a12e6e2023c979d5b0c095c1eddd77eb5155d1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380346"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Transfere um ou mais arquivos. Para transferir mais de um arquivo, especifique vários pares \<RemoteFileName @ no__t-1 @ no__t-2 @ no__t-3LocalFileName @ no__t-4. Os pares são delimitados por espaço.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Nome|O nome do trabalho. Ao contrário da maioria dos comandos, o **nome** pode ser apenas um nome e não um GUID.|
|type|Opcional — especifique o tipo de trabalho. Use **/Download** (o padrão) para um trabalho de download ou **/upload** para um trabalho de upload.|
|Priority|Opcional — defina o job_priority para um dos seguintes valores:</br>-PRIMEIRO PLANO</br>-ALTA</br>-NORMAL</br>-BAIXO|
|ACLFlags|Opcional — indica que você deseja manter as informações de proprietário e ACL com o arquivo que está sendo baixado. Por exemplo, para manter o proprietário e o grupo com o arquivo, defina sinalizadores como `OG`. Especifique um ou mais dos seguintes sinalizadores:</br>MINÚSCULA Copie as informações do proprietário com o arquivo.</br>M Copie as informações do grupo com o arquivo.</br>3D Copie informações de DACL com o arquivo.</br>& Copie as informações da SACL com o arquivo.|
|\/DYNAMIC|Configura o trabalho com [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), que libera os requisitos do lado do servidor.|
|RemoteFileName|O nome do arquivo quando transferido para o servidor.|
|LocalFileName|O nome do arquivo que reside localmente.|

## <a name="remarks"></a>Comentários

Por padrão, o serviço BITSAdmin cria um trabalho de download que é executado em prioridade **normal** e atualiza a janela de comando com informações de progresso até que a transferência seja concluída ou até que ocorra um erro crítico. O serviço concluirá o trabalho se ele transferir com êxito todos os arquivos e cancelar o trabalho se ocorrer um erro crítico. O serviço não criará o trabalho se não for possível adicionar arquivos ao trabalho ou se você especificar um valor inválido para o *tipo* ou *Job_Priority*. Para transferir mais de um arquivo, especifique vários pares de *RemoteFileName*-*LocalFileName* . Os pares são delimitados por espaço.

> [!NOTE]
> O comando BITSAdmin continuará a ser executado se ocorrer um erro transitório. Para encerrar o comando, pressione CTRL + C.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir inicia um trabalho de transferência com o nome *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)