---
title: modo
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cde0e9786f72823f446202f1c87ad8e9e181d29c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848197"
---
# <a name="mode"></a>modo



Exibe o status do sistema, altera as configurações do sistema ou reconfigura portas ou dispositivos. Se usado sem parâmetros, **modo** exibe todos os atributos controláveis do console e os dispositivos COM disponíveis.

Você pode usar **modo** para executar as seguintes tarefas — cada tarefa usa uma sintaxe diferente:
-   [Para configurar uma porta de comunicação serial](#BKMK_1)
-   [Para exibir o status de todos os dispositivos ou de um único dispositivo](#BKMK_2)
-   [Para redirecionar a saída de uma porta paralela para uma porta de comunicação serial](#BKMK_3)
-   [Para selecionar, atualizar ou exibir os números das páginas de código para o console](#BKMK_4)
-   [Para alterar o tamanho do buffer da tela do prompt de comando](#BKMK_5)
-   [Para definir a taxa de digitação do teclado](#BKMK_6)

## <a name="BKMK_1"></a>Para configurar uma porta de comunicação serial

### <a name="syntax"></a>Sintaxe

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Com\<M>[:]|Especifica o número de porta de comunicação Prncnfg.vbshronous async.|
|baud=\<B>|Especifica a taxa de transmissão em bits por segundo. A tabela a seguir lista as abreviações válidas para *B* e suas taxas de relacionados.</br>-   **11** = 110 baud</br>-   **15** = 150 baud</br>-   **30** = 300 baud</br>-   **60** = 600 baud</br>-   **12** = 1200 baud</br>-   **24** = 2400 baud</br>-   **48** = 4800 baud</br>-   **96** = 9600 baud</br>-   **19** = baud 19.200|
|parity=\<P>|Especifica como o sistema usa o bit de paridade para verificar se há erros de transmissão. A tabela a seguir lista os valores válidos para *P*. O valor padrão é **eletrônico**. Nem todos os computadores suportam os valores **m** e **s**.</br>-   **n** = nenhum</br>-   **e** = even</br>-   **o** = odd</br>-   **m** = marca</br>-   **s** = space|
|data=\<D>|Especifica o número de bits de dados em um caractere. Os valores válidos para **1!d** estão no intervalo de 5 a 8. O valor padrão é 7. Nem todos os computadores suportam os valores 5 e 6.|
|stop=\<S>|Especifica o número de bits de parada que define o final de um caractere de: 1, 1.5 ou 2. Se a taxa de transmissão for 110, o valor padrão é 2. Caso contrário, o valor padrão é 1. Nem todos os computadores suportam o valor 1.5.|
|to={on | off}|Especifica se o processamento de tempo limite infinito está ativado ou desativado. O padrão é off.|
|xon={on | off}|Especifica se o protocolo xon ou xoff para controle de fluxo de dados está ativado ou desativado.|
|odsr={on | off}|Especifica se o handshake de saída que utiliza o circuito de conjunto de dados pronto (DSR) está ativado ou desativado.|
|octs={on | off}|Especifica se o handshake de saída que utiliza o circuito pronto para enviar (CTS) está ativado ou desativado.|
|dtr={on | configurações | hs}|Especifica se o circuito Data Terminal Ready (DTR) está ativado ou desativado ou definido para handshake.|
|rts={on | configurações | hs | tg}|Especifica se o circuito de solicitação para enviar (RTS) é definido como ativado, desativado, handshake ou ativar/desativar.|
|idsr={on | off}|Especifica se a sensibilidade do DSR circuito está ativado ou desativado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_2"></a>Para exibir o status de todos os dispositivos ou de um único dispositivo

### <a name="syntax"></a>Sintaxe

```
mode [<Device>] [/status]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Dispositivo >|Especifica o nome do dispositivo para o qual você deseja exibir o status.|
|/status|Solicita o status de impressoras paralelas redirecionadas. Você pode abreviar o **/status** opção de linha de comando como **/sta**.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários

Se usado sem parâmetros, **modo** exibe o status de todos os dispositivos que estão instalados no seu sistema.

## <a name="BKMK_3"></a>Para redirecionar a saída de uma porta paralela para uma porta de comunicação serial

### <a name="syntax"></a>Sintaxe

```
mode lpt<N>[:]=com<M>[:]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|lpt\<N>[:]|Obrigatório. Especifica a porta paralela. Os valores válidos para *N* estão no intervalo de 1 a 3.|
|com\<M>[:]|Obrigatório. Especifica a porta serial. Os valores válidos para *M* estão no intervalo de 1 a 4.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários

Você deve ser um membro do grupo Administradores para redirecionar a impressão.

### <a name="examples"></a>Exemplos

Para configurar seu sistema para que ele envia a saída de impressora paralela para uma impressora serial, você deve usar o **modo** comando duas vezes. Na primeira vez, use **modo** para configurar a porta serial. Na segunda vez, use **modo** redirecionar a saída da impressora paralela para a porta serial especificada na primeira **modo** comando.

Por exemplo, se a impressora serial opera a 4800 baud com paridade par, e ele está conectado à porta COM1 (primeira conexão serial em seu computador), digite:
```
mode com1 48,e,,,b
mode lpt1=com1
```
Se você redirecionar a saída da impressora paralela de LPT1 para COM1, mas, em seguida, você decidir que deseja imprimir um arquivo usando LPT1, digite o comando a seguir antes de imprimir o arquivo:
```
mode lpt1
```
Esse comando impede que o redirecionamento de arquivo de LPT1 para COM1.

## <a name="BKMK_4"></a>Para selecionar, atualizar ou exibir os números das páginas de código para o console

### <a name="syntax"></a>Sintaxe

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Dispositivo >|Obrigatório. Especifica o dispositivo para o qual você deseja selecionar uma página de código. ÍCONE é o nome só é válido para um dispositivo.|
|Selecionar página de código =|Obrigatório. Especifica qual página de código para usar com o dispositivo especificado. Você pode abreviar **codepage** **selecionar** como **cp** **sel**.|
|\<YYY>|Obrigatório. Especifica o número da página de código para selecionar. A lista a seguir mostra cada código página com suporte e seu país/região ou linguagem.</br>437: Estados Unidos</br>850: Multilíngue (Latino I)</br>852: Eslavo (Latim II)</br>855: Cirílico (Russo)</br>857: Turco</br>860: Português</br>861: islandês</br>863: Francês (Canadá)</br>865: Nórdico</br>866: Russo</br>869: Grego moderno|
|codepage|Obrigatório. Exibe os números do código de páginas (se houver) que são selecionados para o dispositivo especificado.|
|/status|Exibe os números das páginas de código atual selecionadas para o dispositivo especificado. Você pode abreviar **/status** à **/sta**. Se você especificar **/status**, **página de código do modo** exibe os números das páginas de código que são selecionados para o dispositivo especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_5"></a>Para alterar o tamanho do buffer da tela do prompt de comando

### <a name="syntax"></a>Sintaxe

```
mode con[:] [cols=<C>] [lines=<N>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|con[:]|Obrigatório. Indica que a alteração se aplica a janela do Prompt de comando.|
|cols=\<C>|Especifica o número de colunas no buffer da tela do prompt de comando.|
|lines=\<N>|Especifica o número de linhas no buffer da tela do prompt de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_6"></a>Para definir a taxa de digitação do teclado

### <a name="syntax"></a>Sintaxe

```
mode con[:] [rate=<R> delay=<D>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|con[:]|Obrigatório. Refere-se ao teclado.|
|rate=\<R>|Especifica a taxa em que um caractere é repetido na tela quando você mantém uma tecla pressionada.|
|delay=\<D>|Especifica a quantidade de tempo que devem decorrer depois que você pressione e mantenha uma tecla antes que a saída de caractere se repete.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários

-   A taxa de digitação é a taxa na qual um caractere se repete quando você mantém pressionada a tecla desse caractere. A taxa de digitação tem dois componentes, a taxa e o atraso. Alguns teclados não reconhecem esse comando.
-   Usando **taxa = * * * R*

    Valores válidos estão no intervalo de 1 a 32. Esses valores são iguais a aproximadamente de 2 a 30 caracteres por segundo. O valor padrão é 20 para teclados compatíveis com o IBM AT e 21 para teclados PS/2 compatíveis com IBM. Se você definir a taxa, você também deve definir o atraso.
-   Usando o **atraso**=*1!d*

    Os valores válidos para *1!d* são 1, 2, 3 e 4 (representando 0,25, 0,50, 0,75 e 1 segundo). O valor padrão é 2. Se você definir o atraso, você também deve definir a taxa.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)