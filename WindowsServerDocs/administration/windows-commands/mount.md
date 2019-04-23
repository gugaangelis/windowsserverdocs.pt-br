---
title: montagem
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9d13f5eddef80d99c11fe59c6bd5e589fa67546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858367"
---
# <a name="mount"></a>montagem



Você pode usar **montar** montar compartilhamentos de rede do sistema de arquivos de rede (NFS).

## <a name="syntax"></a>Sintaxe

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>Descrição

O **montar** utilitário de linha de comando monta o sistema de arquivos identificado por *ShareName* exportados pelo servidor NFS identificado pela *ComputerName* e a associa com o Letra da unidade especificada por *DeviceName* ou, se um asterisco (**&#42;**) for usado, a primeira letra de driver disponíveis. Os usuários podem acessar o sistema de arquivo exportado como se fosse uma unidade no computador local. Quando usada sem argumentos, ou opções **montar** exibe informações sobre todos os sistemas de arquivos NFS montados.

O **montar** utilitário está disponível somente se o cliente NFS está instalado.

As seguintes opções e argumentos podem ser usados com o **montar** utilitário.

|Termo|Definição|
|----|----------|
|-o rsize =\<buffersize >|Define o tamanho em quilobytes, do buffer de leitura. Os valores aceitáveis são 1, 2, 4, 8, 16 e 32; o padrão é 32 KB.|
|-o wsize =\<buffersize >|Define o tamanho em quilobytes, do buffer de gravação. Os valores aceitáveis são 1, 2, 4, 8, 16 e 32; o padrão é 32 KB.|
|-o timeout=\<seconds>|Define o valor de tempo limite em segundos para uma chamada de procedimento remoto (RPC). Os valores aceitáveis são 0.8, 0.9 e qualquer inteiro no intervalo de 1 a 60; o padrão é 0,8.|
|-o retry=\<number>|Define o número de repetições para uma montagem flexível. Os valores aceitáveis são inteiros no intervalo de 1 a 10; o padrão é 1.|
|-o mtype={soft | hard}|Define o tipo de montagem (o padrão é **reversível**). Independentemente do tipo de montagem **montar** retornará se ele não é possível montar imediatamente o compartilhamento. Depois que o compartilhamento tem sido montado com êxito, no entanto, se o tipo de montagem for **rígido**, Client for NFS continuará a tentar acessar o compartilhamento até que ele seja bem-sucedida. Como resultado, se o servidor estiver disponível, qualquer programa do Windows tentando acessar o compartilhamento será exibida parar de responder ou "congelar", se for o tipo de montagem **rígido**.|
|-o anon|Montagens como um usuário anônimo.|
|-o nolock|Desabilita o bloqueio (o padrão é **habilitado**).|
|-o casesensitive|Força as pesquisas de arquivo no servidor para diferenciar maiusculas de minúsculas.|
|-o fileaccess=\<mode>|Especifica o modo de permissão padrão de novos arquivos criados no compartilhamento de NFS. Especificar *modo* como um número de três dígitos no formato *ogw*, onde *s*, *g*, e *w* é cada um dígito que representa o acesso concedido o proprietário do arquivo, grupo e o mundo, respectivamente. Os dígitos devem estar no intervalo de 0 a 7 com o seguinte significado:</br>-   0: Sem acesso</br>-1: x (acesso de execução)</br>– 2: w (acesso de gravação)</br>-3: wx</br>-4: r (acesso de leitura)</br>-   5: rx</br>-6: rw</br>-   7: rwx|
|-o lang={euc-jp|euc-tw|euc-kr|SHIFT-jis|Big5|ksc5601|gb2312-80|ansi}|Especifica a codificação padrão usado para nomes de arquivo e diretório e, se usado, deve ser definida para um dos seguintes:</br>-   **ansi**</br>-   **Big5** (chinês)</br>-   **euc-jp** (Japanese)</br>-   **Coreia EUC** (coreano)</br>-   **chinês-Taiwan-EUC** (chinês)</br>-   **gb2312-80** (chinês simplificado)</br>-   **KSC5601** (coreano)</br>-   **SHIFT-jis** (japonês)</br>Se essa opção é definida como **ansi** em sistemas configurados para localidades diferentes do inglês, o esquema de codificação é definido como o esquema de codificação padrão para a localidade. Estes são os esquemas de codificação padrão para as localidades indicadas:</br>-Japonês: SHIFT-JIS</br>-Coreano: KS_C_5601-1987</br>-Chinês simplificado: GB2312-80</br>-Chinês tradicional: BIG5|
|-u:\<UserName>|Especifica o nome de usuário a ser usado para montar o compartilhamento. Se *nome de usuário* não é precedido por uma barra invertida (**\**), ele será tratado como um nome de usuário do UNIX.|
|-p:\<senha >|A senha a ser usado para montar o compartilhamento. Se você usar um asterisco (**&#42;**), você será solicitado a senha.|

> [!NOTE]
