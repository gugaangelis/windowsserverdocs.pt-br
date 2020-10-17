---
title: waitfor
description: Artigo de referência para o comando WAITFOR, que envia ou aguarda um sinal em um sistema.
ms.topic: reference
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 157861a02abe8fbf851a6ef12460b257a7628803
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156126"
---
# <a name="waitfor"></a>waitfor

Envia ou aguarda um sinal em um sistema. Esse comando é usado para sincronizar computadores em uma rede.

## <a name="syntax"></a>Sintaxe

```
waitfor [/s <computer> [/u [<domain>\]<user> [/p [<password>]]]] /si <signalname>
waitfor [/t <timeout>] <signalname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /s `<computer>` | Especifica o nome ou o endereço IP de um computador remoto (não use barras invertidas). O padrão é o computador local. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. Se você não usar esse parâmetro, o sinal será transmitido para todos os sistemas em um domínio. Se você usar esse parâmetro, o sinal será enviado somente para o sistema especificado. |
| /u `[<domain>]<user>` | Executa o script usando as credenciais da conta de usuário especificada. Por padrão, **WAITFOR** usa as credenciais do usuário atual. |
| /p `[\<password>]` | Especifica a senha da conta de usuário que é especificada no parâmetro **/u** . |
| /si | Envia o sinal especificado pela rede. Esse parâmetro também permite que você ative um sinal manualmente. |
| /t `<timeout>` | Especifica o número de segundos a aguardar por um sinal. Por padrão, **WAITFOR** espera indefinidamente. |
| `<signalname>` | Especifica o sinal que **WAITFOR** espera ou envia. Esse parâmetro não diferencia maiúsculas de minúsculas e não pode exceder 225 caracteres. Os caracteres válidos incluem a-z, A-Z, 0-9 e o conjunto de caracteres estendidos ASCII (128-255). |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Você pode executar várias instâncias de **WAITFOR** em um único computador, mas cada instância de **WAITFOR** deve aguardar um sinal diferente. Somente uma instância de **WAITFOR** pode aguardar um determinado sinal em um determinado computador.

- Os computadores só poderão receber sinais se estiverem no mesmo domínio que o computador que está enviando o sinal.

- Você pode usar esse comando ao testar as compilações de software. Por exemplo, o computador de compilação pode enviar um sinal para vários computadores executando **WAITFOR** após a conclusão bem-sucedida da compilação. No recebimento do sinal, o arquivo em lotes que inclui o **WAITFOR** pode instruir os computadores a iniciarem imediatamente a instalação do software ou a execução de testes na compilação compilada.

## <a name="examples"></a>Exemplos

Para aguardar até que o sinal *espresso\build007* seja recebido, digite:

```
waitfor espresso\build007
```

Por padrão, **WAITFOR** espera indefinidamente por um sinal.

Para aguardar *10 segundos* para o sinal *espresso\compile007* ser recebido antes do tempo limite, digite:

```
waitfor /t 10 espresso\build007
```

Para ativar manualmente o sinal *espresso\build007* , digite:

```
waitfor /si espresso\build007
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
