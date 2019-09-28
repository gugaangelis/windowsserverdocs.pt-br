---
title: bitsadmin setsecurityflags
description: O tópico de comandos do Windows para **Bitsadmin setsecurityflags** -define sinalizadores para http que determinam se o bits deve verificar a lista de certificados revogados, ignorar determinados erros de certificado e definir a política a ser usada quando um servidor redireciona a solicitação HTTP.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: acc5a64ef7c82b14e6815b6d51dda5ea4700dcad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380414"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags



Define sinalizadores para HTTP que determinam se o BITS deve verificar a lista de certificados revogados, ignorar determinados erros de certificado e definir a política a ser usada quando um servidor redireciona a solicitação HTTP. O valor é um inteiro sem sinal.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|Valor|Ver comentários|

## <a name="remarks"></a>Comentários

O parâmetro **Value** pode conter um ou mais dos seguintes sinalizadores de notificação.

|Ação|Representação binária|
|------|---------------------|
|Habilitar verificação de CRL|Definir o bit menos significativo|
|Ignorar nome comum inválido no certificado do servidor|Definir o 2º bit à direita|
|Ignorar data inválida no certificado do servidor|Definir o terceiro bit à direita|
|Ignorar autoridade de certificação inválida no certificado do servidor|Definir o 4º bit à direita|
|Ignorar uso inválido do certificado|Definir o quinto bit à direita|
|Política de redirecionamento|Controlado pelo nono para 11 bits a partir da direita</br>0, 0, 0-os redirecionamentos serão automaticamente permitidos.</br>0, 0, 1-o nome remoto na interface IBackgroundCopyFile será atualizado se ocorrer um redirecionamento.</br>0, 1, 0-os BITS falharão no trabalho se ocorrer um redirecionamento.|
|Permitir redirecionamento de HTTPS para HTTP|Definir o 12 bits à direita|

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir define os sinalizadores de segurança para habilitar uma verificação de CRL para o trabalho chamado *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)