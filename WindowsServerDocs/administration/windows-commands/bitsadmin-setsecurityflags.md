---
title: Bitsadmin setsecurityflags
description: Tópico de comandos do Windows para **setsecurityflags bitsadmin** -conjuntos de sinalizadores para HTTP que determinam se BITS devem verificar a lista de revogação de certificados, ignorar determinados erros de certificado e definir a política para usar quando um servidor Redireciona a solicitação HTTP.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a7f74146a26314ddb4fa92f85e5a40267d0f0d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858617"
---
# <a name="bitsadmin-setsecurityflags"></a>Bitsadmin setsecurityflags



Define os sinalizadores para HTTP que determinam se BITS devem verificar a lista de revogação de certificados, ignorar determinados erros de certificado e definir a política a ser usada quando um servidor redireciona a solicitação HTTP. O valor é um inteiro sem sinal.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Valor|Consulte os comentários|

## <a name="remarks"></a>Comentários

O **valor** parâmetro pode conter um ou mais dos seguintes sinalizadores de notificação.

|Ação|Representação binária|
|------|---------------------|
|Habilitar verificação CRL|Definir o bit menos significativo|
|Ignorar nome comum inválido no certificado do servidor|Definir o bit do 2º da direita|
|Ignorar uma data inválida no certificado do servidor|Definir o bit 3 da direita|
|Ignorar a autoridade de certificação inválido no certificado do servidor|Definir o bit 4º da direita|
|Ignorar o uso inválido do certificado|Definir o bit de 5ª da direita|
|Política de redirecionamento|Controlado pelos bits dia 9 a 11 da direita</br>0, 0,0 - redirecionamentos serão permitidos automaticamente.</br>0, 0,1 - nome remoto na interface do IBackgroundCopyFile será atualizado se ocorrer um redirecionamento.</br>0, 1,0 - BITS falhará o trabalho se ocorrer um redirecionamento.|
|Permitir o redirecionamento de HTTPS para HTTP|Definir o bit 12 da direita|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir define os sinalizadores de segurança para habilitar uma verificação CRL do trabalho nomeado *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)