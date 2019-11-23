---
title: waitfor
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: aecea0ad19ee42e61396eb8b8ccd579b9ce2057b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362602"
---
# <a name="waitfor"></a>waitfor



Envia ou aguarda um sinal em um sistema. **WAITFOR** é usado para sincronizar computadores em uma rede.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

## <a name="parameters"></a>Parâmetros

|       Parâmetro       |                                                                                         Descrição                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s \<computador >     | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. |
| /u [\<\]de domínio > <User> |                              Executa o script usando as credenciais da conta de usuário especificada. Por padrão, **WAITFOR** usa as credenciais do usuário atual.                               |
|   /p [\<a senha >]    |                                                    Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                     |
|          /si          |                                                                        Envia o sinal especificado pela rede.                                                                        |
|     /t \<tempo limite >     |                                              Especifica o número de segundos a aguardar por um sinal. Por padrão, **WAITFOR** espera indefinidamente.                                               |
|     \<Signalname >     |                                                Especifica o sinal que **WAITFOR** espera ou envia. *Signalname* não diferencia maiúsculas de minúsculas.                                                 |
|          /?           |                                                                             Exibe a ajuda no prompt de comando.                                                                             |

## <a name="remarks"></a>Comentários

-   Os nomes de sinal não podem exceder 225 caracteres. Os caracteres válidos incluem a-z, A-Z, 0-9 e o conjunto de caracteres estendidos ASCII (128-255).
-   Se você não usar **/s**, o sinal será transmitido para todos os sistemas em um domínio. Se você usar **/s**, o sinal será enviado somente para o sistema especificado.
-   Você pode executar várias instâncias de **WAITFOR** em um único computador, mas cada instância de **WAITFOR** deve aguardar um sinal diferente. Somente uma instância de **WAITFOR** pode aguardar um determinado sinal em um determinado computador.
-   Você pode ativar um sinal manualmente usando a opção de linha de comando **/si** .
-   **WAITFOR** só é executado no Windows XP e em servidores que executam um sistema operacional windows Server 2003, mas pode enviar sinais para qualquer computador que esteja executando um sistema operacional Windows.
-   Os computadores só poderão receber sinais se estiverem no mesmo domínio que o computador que está enviando o sinal.
-   Você pode usar **WAITFOR** quando testar as compilações de software. Por exemplo, o computador de compilação pode enviar um sinal para vários computadores executando **WAITFOR** após a conclusão bem-sucedida da compilação. No recebimento do sinal, o arquivo em lotes que inclui o **WAITFOR** pode instruir os computadores a iniciarem imediatamente a instalação do software ou a execução de testes na compilação compilada.

## <a name="BKMK_examples"></a>Disso

Para aguardar até que o sinal "espresso\build007" seja recebido, digite:
```
waitfor espresso\build007
```
Por padrão, **WAITFOR** espera indefinidamente por um sinal.

Para aguardar 10 segundos para o sinal "espresso\compile007" ser recebido antes do tempo limite, digite:
```
waitfor /t 10 espresso\build007
```
Para ativar manualmente o sinal "espresso\build007", digite:
```
waitfor /si espresso\build007
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)