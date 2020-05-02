---
title: waitfor
description: Tópico de referência para WAITFOR, que envia ou aguarda um sinal em um sistema. **WAITFOR** é usado para sincronizar computadores em uma rede.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1264fa3bffde303577bd56a0f1f68a6d7b2d98c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720218"
---
# <a name="waitfor"></a>waitfor



Envia ou aguarda um sinal em um sistema. **WAITFOR** é usado para sincronizar computadores em uma rede.



## <a name="syntax"></a>Sintaxe

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

### <a name="parameters"></a>Parâmetros

|       Parâmetro       |                                                                                         Descrição                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s \<> do computador     | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. |
| /u [\<domínio>\]<User> |                              Executa o script usando as credenciais da conta de usuário especificada. Por padrão, **WAITFOR** usa as credenciais do usuário atual.                               |
|   /p [\<senha>]    |                                                    Especifica a senha da conta de usuário que é especificada no parâmetro **/u** .                                                     |
|          /si          |                                                                        Envia o sinal especificado pela rede.                                                                        |
|     /t \<Timeout>     |                                              Especifica o número de segundos a aguardar por um sinal. Por padrão, **WAITFOR** espera indefinidamente.                                               |
|     \<Signalname>     |                                                Especifica o sinal que **WAITFOR** espera ou envia. *Signalname* não diferencia maiúsculas de minúsculas.                                                 |
|          /?           |                                                                             Exibe a ajuda no prompt de comando.                                                                             |

## <a name="remarks"></a>Comentários

-   Os nomes de sinal não podem exceder 225 caracteres. Os caracteres válidos incluem a-z, A-Z, 0-9 e o conjunto de caracteres estendidos ASCII (128-255).
-   Se você não usar **/s**, o sinal será transmitido para todos os sistemas em um domínio. Se você usar **/s**, o sinal será enviado somente para o sistema especificado.
-   Você pode executar várias instâncias de **WAITFOR** em um único computador, mas cada instância de **WAITFOR** deve aguardar um sinal diferente. Somente uma instância de **WAITFOR** pode aguardar um determinado sinal em um determinado computador.
-   Você pode ativar um sinal manualmente usando a opção de linha de comando **/si** .
-   **WAITFOR** só é executado no Windows XP e em servidores que executam um sistema operacional windows Server 2003, mas pode enviar sinais para qualquer computador que esteja executando um sistema operacional Windows.
-   Os computadores só poderão receber sinais se estiverem no mesmo domínio que o computador que está enviando o sinal.
-   Você pode usar **WAITFOR** quando testar as compilações de software. Por exemplo, o computador de compilação pode enviar um sinal para vários computadores executando **WAITFOR** após a conclusão bem-sucedida da compilação. No recebimento do sinal, o arquivo em lotes que inclui o **WAITFOR** pode instruir os computadores a iniciarem imediatamente a instalação do software ou a execução de testes na compilação compilada.

## <a name="examples"></a>Exemplos

Para aguardar até que o sinal espresso\build007 seja recebido, digite:
```
waitfor espresso\build007
```
Por padrão, **WAITFOR** espera indefinidamente por um sinal.

Para aguardar 10 segundos para o sinal espresso\compile007 ser recebido antes do tempo limite, digite:
```
waitfor /t 10 espresso\build007
```
Para ativar manualmente o sinal espresso\build007, digite:
```
waitfor /si espresso\build007
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)