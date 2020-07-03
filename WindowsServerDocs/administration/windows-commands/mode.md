---
title: mode
description: Artigo de referência para o comando de modo, que exibe o status do sistema, altera as configurações do sistema ou reconfigura portas ou dispositivos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b59b04f2-b41d-42df-b5be-19c3721445b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5794b80f7457b133d3e5b599cb12613469ad58eb
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936050"
---
# <a name="mode"></a>mode

Exibe o status do sistema, altera as configurações do sistema ou reconfigura portas ou dispositivos. Se usado sem parâmetros, o **modo** exibirá todos os atributos controláveis do console e os dispositivos com disponíveis.

## <a name="serial-port"></a>Porta serial

Configura uma porta de comunicações serial e define o handshake de saída.

### <a name="syntax"></a>Sintaxe

```
mode com<m>[:] [baud=<b>] [parity=<p>] [data=<d>] [stop=<s>] [to={on|off}] [xon={on|off}] [odsr={on|off}] [octs={on|off}] [dtr={on|off|hs}] [rts={on|off|hs|tg}] [idsr={on|off}]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro  | Descrição |
| ---------- | ----------- |
| `com<m>[:]` | Especifica o número da porta de comunicação assíncrona prncnfg. vbshronous. |
| `baud=<b>`  | Especifica a taxa de transmissão em bits por segundo. Os valores válidos incluem:<ul><li>**11** -110 baud</li><li>**15** -150 baud</li><li>**30** a 300 bauds</li><li>**60** -600 baud</li><li>**12** a 1200 bauds</li><li>**24** -2400 baud</li><li>**48** -4800 baud</li><li>**96** -9600 baud</li><li>**19** -19.200 baud</li></ul> |
| `parity=<p>` | Especifica como o sistema usa o bit de paridade para verificar se há erros de transmissão. Os valores válidos incluem:<ul><li>**n** -nenhum</li><li>**e** -par (valor padrão)</li><li>**o** -ímpar</li><li>marca **m**</li><li>espaço em **s**</li></ul>Nem todos os dispositivos dão suporte ao uso dos parâmetros **m** ou **s** . |
| `data=<d>` | Especifica o número de bits de dados em um caractere. Os valores válidos variam de **5** a **8**. O valor padrão é **7**. Nem todos os dispositivos dão suporte aos valores **5** e **6**. |
| `stop=<s>` | Especifica o número de bits de parada que definem o final de um caractere: **1**, **1,5**ou **2**. Se a taxa de transmissão for **110**, o valor padrão será **2**. Caso contrário, o valor padrão será **1**. Nem todos os dispositivos dão suporte ao valor **1,5**. |
| `to={on | off}` | Especifica se o dispositivo usa o processamento de tempo infinita. O valor padrão é **off**. Ativar essa opção **em** significa que o dispositivo nunca deixará de esperar para receber uma resposta de um computador cliente ou host. |
| `xon={on | off}` | Especifica se o sistema permite o protocolo XON/XOFF. Esse protocolo fornece controle de fluxo para comunicações seriais, aumentando a confiabilidade, mas reduzindo o desempenho. |
| `odsr={on | off}` | Especifica se o sistema ativa o handshake de saída de DSR (Data Set Ready). |
| `octs={on | off}` | Especifica se o sistema ativa o handshake de saída Clear to send (CTS). |
| `dtr={on | off | hs}` | Especifica se o sistema ativa o handshake de saída de DTR (terminal data Ready). Definir esse valor como **on** Mode, fornece um sinal constante para mostrar que o terminal está pronto para enviar dados. Definir esse valor para o modo **HS** fornece um sinal de handshake entre os dois terminais. |
| `rts={on | off | hs | tg}` | Especifica se o sistema ativa a solicitação para enviar o handshake de saída (RTS). Definir esse valor como **on** Mode, fornece um sinal constante para mostrar que o terminal está pronto para enviar dados. Definir esse valor para o modo **HS** fornece um sinal de handshake entre os dois terminais. Definir esse valor como modo **TG** fornece uma maneira de alternar entre os Estados pronto e não pronto. |
| `idsr={on | off}` | Especifica se o sistema ativa a sensibilidade de DSR. Você deve ativar essa opção para usar o handshake de DSR.
| /? | Exibe a ajuda no prompt de comando. |

## <a name="device-status"></a>Status do dispositivo

Exibe o status de um dispositivo especificado. Se usado sem parâmetros, o **modo** exibirá o status de todos os dispositivos instalados no sistema.

### <a name="syntax"></a>Sintaxe

```
mode [<device>] [/status]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<device>` | Especifica o nome do dispositivo para o qual você deseja exibir o status. Os nomes padrão incluem, LPT1: a LPT3:, COM1: a COM9: e CON. |
| /status | Solicita o status de qualquer impressora paralela redirecionada. Você também pode usar **/STA** como uma versão abreviada desse comando. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="redirect-printing"></a>Redirecionar impressão

Redireciona a saída da impressora. Você deve ser um membro do grupo Administradores para redirecionar a impressão.

> [!NOTE]
Para configurar seu sistema para que ele envie uma saída de impressora paralela para uma impressora serial, você deve usar o comando **Mode** duas vezes. Na primeira vez, você deve usar o **modo** para configurar a porta serial. Na segunda vez, você deve usar o **modo** para redirecionar a saída da impressora paralela para a porta serial especificada no primeiro comando de **modo** .

### <a name="syntax"></a>Sintaxe

```
mode LPT<n>[:]=COM<m>[:]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| LPT `<n>` [:] | Especifica o número de LPT a configurar. Normalmente, isso significa fornecer um valor de **LTP1: a LTP3:**, a menos que o sistema inclua suporte especial a portas paralelas. Este parâmetro é necessário.|
| COM `<m>` [:] | Especifica a porta COM a ser configurada. Normalmente, isso significa fornecer um valor de **COM1: a COM9:**, a menos que o sistema tenha hardware especial para portas com adicionais. Este parâmetro é necessário. |
| /? | Exibe a ajuda no prompt de comando.|

#### <a name="examples"></a>Exemplos

Para redirecionar uma impressora serial que opere em 4800 bauds com paridade par e esteja conectado à porta COM1 (a primeira conexão serial em seu computador), digite:

```
mode com1 48,e,,,b
mode lpt1=com1
```

Para redirecionar a saída da impressora paralela de LPT1 para COM1 e, em seguida, imprimir um arquivo usando LPT1, digite o seguinte comando antes de imprimir o arquivo:

```
mode lpt1
```

Esse comando impede o redirecionamento do arquivo de LPT1 para COM1.

## <a name="select-code-page"></a>Selecionar página de código

Configura ou consulta as informações da página de código para um dispositivo selecionado.

### <a name="syntax"></a>Sintaxe

```
mode <device> codepage select=<yyy>
mode <device> codepage [/status]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<device>` |Especifica o dispositivo para o qual você deseja selecionar uma página de código. CON é o único nome válido para um dispositivo. Este parâmetro é necessário. |
| codepage | Especifica qual página de código usar com o dispositivo especificado. Você também pode usar **CP** como uma versão abreviada deste comando. Este parâmetro é necessário. |
| Select =`<yyy>` | Especifica o número da página de código a ser usada com o dispositivo. As páginas de código com suporte, por país/região ou idioma, incluem:<ul><li>**437:** Estados Unidos</li><li>**850:** Multilíngue (Latino I)</li><li>**852:** Eslavo (latino II)</li><li>**855:** Cirílico (russo)</li><li>**857:** Turco</li><li>**860:** Português</li><li>**861:** Islandês</li><li>**863:** Canadá-francês</li><li>**865:** Nórdico</li><li>**866:** Russo</li><li>**869:** Grego moderno</li></ul> Este parâmetro é necessário. |
| /status | Exibe os números das páginas de código atuais selecionadas para o dispositivo especificado. Você também pode usar **/STA** como uma versão abreviada desse comando. Independentemente de você especificar **/status**, o comando **Mode CodePage** exibirá os números das páginas de código selecionadas para o dispositivo especificado. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="display-mode"></a>Modo de exibição

Altera o tamanho do buffer da tela do prompt de comando

### <a name="syntax"></a>Sintaxe

```
mode con[:] [cols=<c>] [lines=<n>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| con [:] | Indica que a alteração se aplica à janela de prompt de comando. Este parâmetro é necessário. |
| cols =`<c>` | Especifica o número de colunas no buffer da tela do prompt de comando. A configuração padrão é 80 colunas, mas você pode defini-la para qualquer valor. Se você não usar o padrão, os valores típicos serão 40 e 135 colunas. O uso de valores não padrão pode resultar em problemas de aplicativo de prompt de comando. |
| linhas =`<n>` | Especifica o número de linhas no buffer da tela do prompt de comando. O valor padrão é 25, mas você pode defini-lo para qualquer valor. Se você não usar o padrão, o outro valor típico será de 50 linhas. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="typematic-rate"></a>Taxa de digitação

Define a taxa de digitação do teclado. A taxa de digitação é a velocidade na qual o Windows pode repetir um caractere quando você pressiona a tecla em um teclado.

> [!NOTE]
> Alguns teclados não reconhecem esse comando.

### <a name="syntax"></a>Sintaxe

```
mode con[:] [rate=<r> delay=<d>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| con [:] | Especifica o teclado. Este parâmetro é necessário. |
| taxa =`<r>` | Especifica a taxa na qual um caractere é repetido na tela quando você mantém uma tecla pressionada. O valor padrão é 20 caracteres por segundo para teclados compatíveis com IBM e 21 para teclados compatíveis com IBM PS/2, mas você pode usar qualquer valor de 1 a 32. Se você definir esse parâmetro, também deverá definir o parâmetro **Delay** .|
| atraso =`<d>` | Especifica a quantidade de tempo que o decorrerá depois que você pressionar e manter uma tecla pressionada antes da repetição da saída de caracteres. O valor padrão é 2 (. 50 segundos), mas você também pode usar 1 (. 25 segundos), 3 (. 75 segundos) ou 4 (1 segundo). Se você definir esse parâmetro, também deverá definir o parâmetro de **taxa** . |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)