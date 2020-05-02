---
title: mode
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2bd7875687aa112c500a966e0ebbcb5af142eb83
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723951"
---
# <a name="mode"></a>mode



Exibe o status do sistema, altera as configurações do sistema ou reconfigura portas ou dispositivos. Se usado sem parâmetros, o **modo** exibirá todos os atributos controláveis do console e os dispositivos com disponíveis.

Você pode usar o **modo** para executar as seguintes tarefas – cada tarefa usa uma sintaxe diferente:
-   [Para configurar uma porta de comunicações serial](#BKMK_1)
-   [Para exibir o status de todos os dispositivos ou de um único dispositivo](#BKMK_2)
-   [Para redirecionar a saída de uma porta paralela para uma porta de comunicações serial](#BKMK_3)
-   [Para selecionar, atualizar ou exibir os números das páginas de código do console](#BKMK_4)
-   [Para alterar o tamanho do buffer da tela do prompt de comando](#BKMK_5)
-   [Para definir a taxa de digitação do teclado](#BKMK_6)

## <a name="to-configure-a-serial-communications-port"></a><a name=BKMK_1></a>Para configurar uma porta de comunicações serial

### <a name="syntax"></a>Sintaxe

```
mode com<M>[:] [baud=<B>] [parity=<P>] [data=<D>] [stop=<S>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

#### <a name="parameters"></a>Parâmetros

|  Parâmetro  |                                                                                                                                                                                     Descrição                                                                                                                                                                                     |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| >\<M com [:]  |                                                                                                                                                      Especifica o número da porta de comunicação assíncrona prncnfg. vbshronous.                                                                                                                                                      |
|  baud =\<B>  | Especifica a taxa de transmissão em bits por segundo. A tabela a seguir lista as abreviações válidas para *B* e suas taxas relacionadas.</br>-   **11** = 110 baud</br>-   **15** = 150 baud</br>-   **30** = 300 baud</br>-   **60** = 600 baud</br>-   **12** = 1200 baud</br>-   **24** = 2400 baud</br>-   **48** = 4800 baud</br>-   **96** = 9600 baud</br>-   **19** = 19.200 baud |
| paridade =\<P> |                              Especifica como o sistema usa o bit de paridade para verificar se há erros de transmissão. A tabela a seguir lista os valores válidos para *P*. O valor padrão é **e**. Nem todos os computadores dão suporte aos valores **m** e **s**.</br>-   **n** = nenhum</br>-   **e** = par</br>-   **o** = ímpar</br>-   **m** = marca</br>-   **s** = espaço                              |
|  dados =\<D>  |                                                                                                    Especifica o número de bits de dados em um caractere. Os valores válidos para **d** estão no intervalo de 5 a 8. O valor padrão é 7. Nem todos os computadores dão suporte aos valores 5 e 6.                                                                                                     |
|  Stop =\<S>  |                                                                                  Especifica o número de bits de parada que definem o final de um caractere: 1, 1,5 ou 2. Se a taxa de transmissão for 110, o valor padrão será 2. Caso contrário, o valor padrão será 1. Nem todos os computadores dão suporte ao valor 1,5.                                                                                   |
|   para = {on    |                                                                                                                                                                                        {1&gt;off&lt;1}}                                                                                                                                                                                         |
|   Xon = {on   |                                                                                                                                                                                        {1&gt;off&lt;1}}                                                                                                                                                                                         |
|  odsr = {on   |                                                                                                                                                                                        {1&gt;off&lt;1}}                                                                                                                                                                                         |
|  Octs = {on   |                                                                                                                                                                                        {1&gt;off&lt;1}}                                                                                                                                                                                         |
|   DTR = {on   |                                                                                                                                                                                         Desligar                                                                                                                                                                                         |
|   RTS = {on   |                                                                                                                                                                                         Desligar                                                                                                                                                                                         |
|  IDSR = {on   |                                                                                                                                                                                        {1&gt;off&lt;1}}                                                                                                                                                                                         |
|     /?      |                                                                                                                                                                        Exibe a ajuda no prompt de comando.                                                                                                                                                                         |

## <a name="to-display-the-status-of-all-devices-or-of-a-single-device"></a><a name=BKMK_2></a>Para exibir o status de todos os dispositivos ou de um único dispositivo

### <a name="syntax"></a>Sintaxe

```
mode [<Device>] [/status]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> de dispositivo|Especifica o nome do dispositivo para o qual você deseja exibir o status.|
|/status|Solicita o status de qualquer impressora paralela redirecionada. Você pode abreviar a opção de linha de comando **/status** como **/STA**.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários

Se usado sem parâmetros, o **modo** exibirá o status de todos os dispositivos que estão instalados no sistema.

## <a name="to-redirect-output-from-a-parallel-port-to-a-serial-communications-port"></a><a name=BKMK_3></a>Para redirecionar a saída de uma porta paralela para uma porta de comunicações serial

### <a name="syntax"></a>Sintaxe

```
mode lpt<N>[:]=com<M>[:]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|>\<LPT N [:]|Obrigatórios. Especifica a porta paralela. Os valores válidos para *N* estão no intervalo de 1 a 3.|
|>\<M com [:]|Obrigatórios. Especifica a porta serial. Os valores válidos para *M* estão no intervalo de 1 a 4.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários

Você deve ser um membro do grupo Administradores para redirecionar a impressão.

### <a name="examples"></a>Exemplos

Para configurar seu sistema para que ele envie uma saída de impressora paralela para uma impressora serial, você deve usar o comando **Mode** duas vezes. Na primeira vez, use o **modo** para configurar a porta serial. Na segunda vez, use o **modo** para redirecionar a saída de impressora paralela para a porta serial que você especificou no primeiro comando de **modo** .

Por exemplo, se a sua impressora serial operar a 4800 baud com paridade par e estiver conectada à porta COM1 (a primeira conexão serial em seu computador), digite:
```
mode com1 48,e,,,b
mode lpt1=com1
```
Se você redirecionar a saída de impressora paralela de LPT1 para COM1, mas decidir que deseja imprimir um arquivo usando LPT1, digite o seguinte comando antes de imprimir o arquivo:
```
mode lpt1
```
Esse comando impede o redirecionamento do arquivo de LPT1 para COM1.

## <a name="to-select-refresh-or-display-the-numbers-of-the-code-pages-for-the-console"></a><a name=BKMK_4></a>Para selecionar, atualizar ou exibir os números das páginas de código do console

### <a name="syntax"></a>Sintaxe

```
mode <Device> codepage select=<YYY>
mode <Device> codepage [/status]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> de dispositivo|Obrigatórios. Especifica o dispositivo para o qual você deseja selecionar uma página de código. CON é o único nome válido para um dispositivo.|
|página de código Select =|Obrigatórios. Especifica qual página de código usar com o dispositivo especificado. Você pode abreviar **CodePage** **Select** as **CP** **SEL**.|
|\<YYY>|Obrigatórios. Especifica o número da página de código a ser selecionada. A lista a seguir mostra cada página de código com suporte e seu país/região ou idioma.</br>437: Estados Unidos</br>850: multilíngue (I latino)</br>852: eslavo (latino II)</br>855: cirílico (russo)</br>857: Turco</br>860: Português</br>861: Islandês</br>863: Canadá – francês</br>865: nórdico</br>866: Russo</br>869: grego moderno|
|codepage|Obrigatórios. Exibe os números das páginas de código (se houver) selecionadas para o dispositivo especificado.|
|/status|Exibe os números das páginas de código atuais selecionadas para o dispositivo especificado. Você pode abreviar **/status** para **/STA**. Independentemente de você especificar **/status**, o **modo CodePage** exibirá os números das páginas de código selecionadas para o dispositivo especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="to-change-the-size-of-the-command-prompt-screen-buffer"></a><a name=BKMK_5></a>Para alterar o tamanho do buffer da tela do prompt de comando

### <a name="syntax"></a>Sintaxe

```
mode con[:] [cols=<C>] [lines=<N>]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|con [:]|Obrigatórios. Indica que a alteração se aplica à janela de prompt de comando.|
|cols =\<C>|Especifica o número de colunas no buffer da tela do prompt de comando.|
|linhas =\<N>|Especifica o número de linhas no buffer da tela do prompt de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="to-set-the-keyboard-typematic-rate"></a><a name=BKMK_6></a>Para definir a taxa de digitação do teclado

### <a name="syntax"></a>Sintaxe

```
mode con[:] [rate=<R> delay=<D>]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|con [:]|Obrigatórios. Refere-se ao teclado.|
|taxa =\<R>|Especifica a taxa na qual um caractere é repetido na tela quando você mantém uma tecla pressionada.|
|atraso =\<D>|Especifica a quantidade de tempo que o decorrerá depois que você pressionar e manter uma tecla pressionada antes da repetição da saída de caracteres.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários

- A taxa de digitação é a taxa na qual um caractere se repete quando você mantém a chave para esse caractere. A taxa de digitação tem dois componentes, a taxa e o atraso. Alguns teclados não reconhecem esse comando.
- Usando **Rate =**<em>R</em>

  Os valores válidos estão no intervalo de 1 a 32. Esses valores são iguais a aproximadamente de 2 a 30 caracteres por segundo. O valor padrão é 20 para teclados compatíveis com IBM e 21 para teclados compatíveis com IBM PS/2. Se você definir a taxa, também deverá definir o atraso.
- Usando o **atraso**=*D*

  Os valores válidos para *D* são 1, 2, 3 e 4 (representando 0,25, 0,50, 0,75 e 1 segundo). O valor padrão é 2. Se você definir o atraso, também deverá definir a taxa.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)