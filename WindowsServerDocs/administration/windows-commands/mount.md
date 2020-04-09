---
title: montagem
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72239a3dc55bf88638004c5885da6e1da70bcd02
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839389"
---
# <a name="mount"></a>montagem



Você pode usar a **montagem** para montar compartilhamentos de rede NFS (Network File System).

## <a name="syntax"></a>Sintaxe

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Descrição

O utilitário de linha de comando **Mount** monta o sistema de arquivos identificado pelo *ShareName* exportado pelo servidor NFS identificado por *ComputerName* e o associa à letra da unidade especificada por *DeviceName* ou, se um **&#42;** asterisco () for usado, pela primeira letra de driver disponível. Os usuários podem acessar o sistema de arquivos exportado como se fosse uma unidade no computador local. Quando usado sem opções ou argumentos, a **montagem** exibe informações sobre todos os sistemas de arquivos NFS montados.

O utilitário de **montagem** estará disponível somente se o Client for NFS estiver instalado.

As opções e argumentos a seguir podem ser usados com o utilitário de **montagem** .


|          Termo          |                                                                                                                                                                                                                                                Definição                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize =\<BufferSize > |                                                                                                                                                                                            Define o tamanho em kilobytes do buffer de leitura. Os valores aceitáveis são 1, 2, 4, 8, 16 e 32; o padrão é 32 KB.                                                                                                                                                                                            |
| -o wSize =\<BufferSize > |                                                                                                                                                                                           Define o tamanho em kilobytes do buffer de gravação. Os valores aceitáveis são 1, 2, 4, 8, 16 e 32; o padrão é 32 KB.                                                                                                                                                                                            |
| -o timeout =\<segundos >  |                                                                                                                                                                       Define o valor de tempo limite em segundos para uma chamada de procedimento remoto (RPC). Os valores aceitáveis são 0,8, 0,9 e qualquer número inteiro no intervalo 1-60; o padrão é 0,8.                                                                                                                                                                       |
|   -o Retry =\<número >   |                                                                                                                                                                                             Define o número de repetições para uma montagem flexível. Os valores aceitáveis são inteiros no intervalo de 1-10; o padrão é 1.                                                                                                                                                                                             |
|     -o mtype = {Soft     |                                                                                                                                                                                                                                                  rígido                                                                                                                                                                                                                                                   |
|        -o anon         |                                                                                                                                                                                                                                       Monta como um usuário anônimo.                                                                                                                                                                                                                                       |
|       -o NOLOCK        |                                                                                                                                                                                                                                Desabilita o bloqueio (o padrão é **habilitado**).                                                                                                                                                                                                                                |
|    -o CaseSensitive    |                                                                                                                                                                                                                         Força a pesquisa de arquivos no servidor a diferenciar maiúsculas de minúsculas.                                                                                                                                                                                                                          |
| -o FileAccess = modo de\<>  | Especifica o modo de permissão padrão de novos arquivos criados no compartilhamento NFS. Especifique o *modo* como um número de três dígitos no formato *ogw*, em *que o*, *g*e *w* são cada um dos dígitos que representam o acesso concedido ao proprietário do arquivo, ao grupo e ao mundo, respectivamente. Os dígitos devem estar no intervalo de 0-7 com o seguinte significado:</br>-0: sem acesso</br>-1: x (executar acesso)</br>-2: w (acesso de gravação)</br>-3: WX</br>-4: r (acesso de leitura)</br>-5: RX</br>-6: RW</br>-7: rwx |
|    -o Lang = {EUC-JP     |                                                                                                                                                                                                                                                  EUC-TW                                                                                                                                                                                                                                                  |
|     -u:\<nome de usuário >     |                                                                                                                                                                             Especifica o nome de usuário a ser usado para montar o compartilhamento. Se *username* não for precedido por uma barra invertida ( **\\** ), ele será tratado como um nome de usuário do UNIX.                                                                                                                                                                             |
|     -p:\<senha >     |                                                                                                                                                                                          A senha a ser usada para montar o compartilhamento. Se você usar um asterisco ( **&#42;** ), a senha será solicitada.                                                                                                                                                                                          |

> [!NOTE]
