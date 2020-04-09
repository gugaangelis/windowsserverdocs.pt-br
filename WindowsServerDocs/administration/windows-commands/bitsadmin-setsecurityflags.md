---
title: bitsadmin setsecurityflags
description: O tópico de comandos do Windows para Bitsadmin setsecurityflags, que define sinalizadores para HTTP que determinam se o BITS deve verificar a lista de certificados revogados, ignorar determinados erros de certificado e definir a política a ser usada quando um servidor redireciona a solicitação HTTP.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0da5cbf5-5f7f-4833-bbbe-c4e8379a78ab
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a7b857bb398e3061a3435a730bf9a751ee2c5e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849139"
---
# <a name="bitsadmin-setsecurityflags"></a>bitsadmin setsecurityflags

Define sinalizadores para HTTP que determinam se o BITS deve verificar a lista de certificados revogados, ignorar determinados erros de certificado e definir a política a ser usada quando um servidor redireciona a solicitação HTTP. O valor é um inteiro sem sinal.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetSecurityFlags <Job> <Value>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|{1&gt;Valor&lt;1}|Ver comentários|

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

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define os sinalizadores de segurança para habilitar uma verificação de CRL para o trabalho chamado *myJob*.
```
C:\>bitsadmin /SetSecurityFlags myJob 0x0001
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)