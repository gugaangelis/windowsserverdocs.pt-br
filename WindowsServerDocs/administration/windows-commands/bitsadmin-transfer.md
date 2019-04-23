---
title: bitsadmin Transfer
description: Tópico de comandos do Windows para **bitsadmin transferência** -transfere um ou mais arquivos.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2ef29a242a834fae42d1de3994a82aedcf87ec2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852457"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Transfere um ou mais arquivos. Para transferir mais de um arquivo, especificar vários \<RemoteFileName\>-\<LocalFileName\> pares. Os pares são delimitados por espaço.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Nome|O nome do trabalho. Ao contrário da maioria dos comandos, **nome** só pode ser um nome e não um GUID.|
|Tipo|Opcional – Especifique o tipo de trabalho. Use **/baixar** (o padrão) para um trabalho de download ou **/carregar** para um trabalho de upload.|
|Priority|Opcional – defina o job_priority como um dos seguintes valores:</br>-EM PRIMEIRO PLANO</br>-ALTA</br>-   NORMAL</br>-BAIXA|
|ACLFlags|Opcional – indica que você deseja manter o proprietário e as informações de ACL com o arquivo que está sendo baixado. Por exemplo, para manter o proprietário e o grupo com o arquivo, defina sinalizadores `OG`. Especifique um ou mais dos seguintes sinalizadores:</br>-/O: Copie informações do proprietário com o arquivo.</br>-   G: Copie informações de grupo com o arquivo.</br>-UNIDADE D: Copie informações da DACL com o arquivo.</br>-S: Copie informações da SACL com o arquivo.|
|\/DINÂMICO|Configura o trabalho com [ **BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), que alivia os requisitos do lado do servidor.|
|RemoteFileName|O nome do arquivo quando transferidos para o servidor.|
|LocalFileName|O nome do arquivo que reside localmente.|

## <a name="remarks"></a>Comentários

Por padrão, o serviço BITSAdmin cria um trabalho de download que é executado no **NORMAL** prioridade e atualiza a janela de comando com informações sobre o andamento até que a transferência for concluída ou até que ocorra um erro crítico. O serviço conclui o trabalho se ele transfere todos os arquivos com êxito e cancela o trabalho se ocorrer um erro crítico. O serviço não cria o trabalho se não for possível adicionar arquivos ao trabalho ou se você especificar um valor inválido para *tipo* ou *Job_Priority*. Para transferir mais de um arquivo, especificar vários *RemoteFileName*-*LocalFileName* pares. Os pares são delimitados por espaço.

> [!NOTE]
> O comando BITSAdmin continuará a ser executado se ocorrer um erro transitório. Para concluir o comando, pressione CTRL + C.

## <a name="BKMK_examples"></a>Exemplos

Do início do exemplo a seguir uma transferência de trabalhos com denominado *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)