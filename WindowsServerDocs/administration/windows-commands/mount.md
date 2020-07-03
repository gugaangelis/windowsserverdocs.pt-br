---
title: montagem
description: Artigo de referência para o comando Mount, que monta os compartilhamentos de rede NFS (Network File System).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 505094251ab6b0053cc3d46801ba5f6170201ecd
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935725"
---
# <a name="mount"></a>montagem

Um utilitário de linha de comando que monta compartilhamentos de rede NFS (Network File System). Quando usado sem opções ou argumentos, a **montagem** exibe informações sobre todos os sistemas de arquivos NFS montados.

> [!NOTE]
> Esse utilitário estará disponível somente se o **Client for NFS** estiver instalado.

## <a name="syntax"></a>Sintaxe

```
mount [-o <option>[...]] [-u:<username>] [-p:{<password> | *}] {\\<computername>\<sharename> | <computername>:/<sharename>} {<devicename> | *}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro  | Descrição |
| ---------- | ----------- |
| -o rsize =`<buffersize>` | Define o tamanho em kilobytes do buffer de leitura. Os valores aceitáveis são 1, 2, 4, 8, 16 e 32; o padrão é 32 KB. |
| -o wSize =`<buffersize>` | Define o tamanho em kilobytes do buffer de gravação. Os valores aceitáveis são 1, 2, 4, 8, 16 e 32; o padrão é 32 KB. |
| -o timeout =`<seconds>` | Define o valor de tempo limite em segundos para uma chamada de procedimento remoto (RPC). Os valores aceitáveis são 0,8, 0,9 e qualquer número inteiro no intervalo 1-60; o padrão é 0,8. |
| -o Retry =`<number>` | Define o número de repetições para uma montagem flexível. Os valores aceitáveis são inteiros no intervalo de 1-10; o padrão é 1. |
| -o mtype =`{soft|hard}` | Define o tipo de montagem para seu compartilhamento NFS. Por padrão, o Windows usa uma montagem flexível. As montagens suaves esgotam o tempo limite mais facilmente quando há problemas de conexão; no entanto, para reduzir a interrupção de e/s durante a reinicialização do servidor NFS, recomendamos o uso de uma montagem rígida.|
| -o anon | Monta como um usuário anônimo. |
| -o NOLOCK | Desabilita o bloqueio (o padrão é **habilitado**). |
| -o CaseSensitive | Força a pesquisa de arquivos no servidor a diferenciar maiúsculas de minúsculas. |
| -o FileAccess =`<mode>` | Especifica o modo de permissão padrão de novos arquivos criados no compartilhamento NFS. Especifique o *modo* como um número de três dígitos no formato *ogw*, em *que o*, *g*e *w* são cada um dos dígitos que representam o acesso concedido ao proprietário do arquivo, ao grupo e ao mundo, respectivamente. Os dígitos devem estar no intervalo de 0-7, incluindo:<ul><li>**0:** Sem acesso</li><li>**1:** x (executar acesso)</li><li>**2:** w (acesso de gravação)</li><li>**3:** WX (acesso de gravação e execução)</li><li>**4:** r (acesso de leitura)</li><li>**5:** RX (acesso de leitura e execução)</li><li>**6:** RW (acesso de leitura e gravação)</li><li>**7:** rwx (leitura, gravação e acesso de execução)</li></ul> |
| -o Lang =`{euc-jp|euc-tw|euc-kr|shift-jis|Big5|Ksc5601|Gb2312-80|Ansi)` | Especifica a codificação de linguagem a ser configurada em um compartilhamento NFS. Você pode usar apenas um idioma no compartilhamento. Esse valor pode incluir qualquer um dos seguintes valores:<ul><li>**EUC-JP:** Japonês</li><li>**EUC-TW:** Chinês</li><li>**EUC-Kr:** Coreano</li><li>**Shift-JIS:** Japonês</li><li>**Big5:** Chinês</li><li>**Ksc5601:** Coreano</li><li>**Gb2312-80:** Chinês simplificado</li><li>**ANSI:** Codificado em ANSI</li></ul> |
| t`<username>` | Especifica o nome de usuário a ser usado para montar o compartilhamento. Se *username* não for precedido por uma barra invertida (* *\** ), ele será tratado como um nome de usuário do UNIX. |
| DTI`<password>` | A senha a ser usada para montar o compartilhamento. Se você usar um asterisco (**&#42;**), a senha será solicitada. |
| `<computername>` | Especifica o nome do servidor NFS. |
| `<sharename>` | Especifica o nome do sistema de arquivos. |
| `<devicename>` | Especifica a letra da unidade e o nome do dispositivo. Se você usar um asterisco (**&#42;**), esse valor representará a primeira letra de driver disponível. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
