---
title: waitfor
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4a91dd3822cf4d8dd904f473f146a2f0ee54c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840157"
---
# <a name="waitfor"></a>waitfor



Envia ou espera por um sinal em um sistema. **WAITFOR** é usado para sincronizar os computadores em uma rede.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/s \<Computer>|Especifica o nome ou endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando.|
|/u [\<Domain>\]<User>|Executa o script usando as credenciais da conta de usuário especificada. Por padrão, **waitfor** usa credenciais do usuário atual.|
|/p [\<Password>]|Especifica a senha da conta de usuário que é especificada na **/u** parâmetro.|
|/si|Envia o sinal especificado pela rede.|
|/t \<Timeout>|Especifica o número de segundos de espera por um sinal. Por padrão, **waitfor** aguardará indefinidamente.|
|\<SignalName>|Especifica o sinal de que **waitfor** aguarda ou envia. *SignalName* não diferencia maiusculas de minúsculas.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Nomes de sinal não podem exceder 225 caracteres. Caracteres válidos incluem a-z, A-Z, 0-9 e o ASCII estendido de conjunto de caracteres (de 128 a 255).
-   Se você não usar **/s**, o sinal é transmitido para todos os sistemas em um domínio. Se você usar **/s**, o sinal será enviado somente ao sistema especificado.
-   Você pode executar várias instâncias do **waitfor** em um único computador, mas cada instância do **waitfor** deve esperar um sinal diferente. Apenas uma instância do **waitfor** pode esperar que um determinado sinal em um determinado computador.
-   Você pode ativar um sinal manualmente usando o **/si** opção de linha de comando.
-   **WAITFOR** é executado somente no XP do Windows e servidores que executam um sistema operacional Windows Server 2003, mas ele pode enviar sinais para qualquer computador executando o sistema operacional Windows.
-   Computadores só poderão receber sinais se eles estiverem no mesmo domínio que o computador de envio do sinal.
-   Você pode usar **waitfor** quando testar builds de software. Por exemplo, o computador de compilação pode enviar um sinal para vários computadores que executam **waitfor** depois que a compilação foi concluída com êxito. Após o recebimento do sinal, o arquivo em lotes que inclui **waitfor** pode instruir os computadores para iniciar imediatamente a instalação de software ou execução de testes no build compilado.

## <a name="BKMK_examples"></a>Exemplos

Para esperar até que o sinal "espresso\build007" é recebido, digite:
```
waitfor espresso\build007
```
Por padrão, **waitfor** aguarda indefinidamente por um sinal.

Para esperar o sinal de "espresso\compile007" ser recebida antes do tempo limite de 10 segundos, digite:
```
waitfor /t 10 espresso\build007
```
Para ativar manualmente o valor "espresso\build007", digite:
```
waitfor /si espresso\build007
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)