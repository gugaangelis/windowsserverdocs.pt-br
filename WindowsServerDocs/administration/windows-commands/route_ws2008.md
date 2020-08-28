---
title: route
description: Artigo de referência para o comando Route, que modifica e exibe entradas na tabela de roteamento IP local.
ms.topic: reference
ms.assetid: afcd666c-0cef-47c2-9bcc-02d202b983b3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a15e9190ac135a49cfacfd259a7058765cafa8a4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033754"
---
# <a name="route"></a>route

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe e modifica as entradas na tabela de roteamento IP local. Se usado sem parâmetros, **Route** exibe a ajuda no prompt de comando.

> [!IMPORTANT]
> Esse comando estará disponível somente se o protocolo TCP/IP estiver instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.

## <a name="syntax"></a>Sintaxe

```
route [/f] [/p] [<command> [<destination>] [mask <netmask>] [<gateway>] [metric <metric>]] [if <interface>]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /f | Limpa a tabela de roteamento de todas as entradas que não são rotas de host (rotas com uma máscara de rede de 255.255.255.255), a rota de redes de auto-retorno (rotas com um destino de 127.0.0.0 e uma máscara de difusão de 255.0.0.0) ou uma rota de multicast (rotas com um destino de 224.0.0.0 e uma máscara de rede de 240.0.0.0). Se isso for usado em conjunto com um dos comandos (como adicionar, alterar ou excluir), a tabela será limpa antes da execução do comando. |
| /p | Quando usado com o comando Add, a rota especificada é adicionada ao registro e é usada para inicializar a tabela de roteamento de IP sempre que o protocolo TCP/IP é iniciado. Por padrão, as rotas adicionadas não são preservadas quando o protocolo TCP/IP é iniciado. Quando usado com o comando Print, a lista de rotas persistentes é exibida. Esse parâmetro é ignorado para todos os outros comandos. As rotas persistentes são armazenadas no local do registro **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\tcpip\parameters\persistentroutes**. |
| `<command>` | Especifica o comando que você deseja executar. Os comandos válidos incluem:<ul><li>**Adicionar** -adiciona uma rota.</li><li>**alterar** – modifica uma rota existente.</li><li>**excluir:** -exclui uma ou mais rotas.</li><li>**Imprimir** – imprime uma rota ou rotas.</li></ul> |
| `<destination>` | Especifica o destino de rede da rota. O destino pode ser um endereço de rede IP (em que os bits de host do endereço de rede são definidos como 0), um endereço IP para uma rota de host ou 0.0.0.0 para a rota padrão. |
| mascara `<netmask>` | Especifica o destino de rede da rota. O destino pode ser um endereço de rede IP (em que os bits de host do endereço de rede são definidos como 0), um endereço IP para uma rota de host ou 0.0.0.0 para a rota padrão. |
| `<gateway>` | Especifica o endereço IP de encaminhamento ou próximo salto sobre o qual o conjunto de endereços definido pelo destino de rede e a máscara de sub-rede podem ser acessados. Para rotas de sub-rede conectadas localmente, o endereço de gateway é o endereço IP atribuído à interface que está anexada à sub-rede. Para rotas remotas, disponíveis em um ou mais roteadores, o endereço de gateway é um endereço IP acessível diretamente que é atribuído a um roteador vizinho. |
| medidas `<metric>` | Especifica uma métrica de custo inteiro (variando de 1 a 9999) para a rota, que é usada ao escolher entre várias rotas na tabela de roteamento que mais se assemelham ao endereço de destino de um pacote que está sendo encaminhado. A rota com a métrica mais baixa é escolhida. A métrica pode refletir o número de saltos, a velocidade do caminho, a confiabilidade do caminho, a taxa de transferência do caminho ou as propriedades administrativas. |
| que `<interface>` | Especifica o índice de interface para a interface sobre a qual o destino está acessível. Para obter uma lista de interfaces e seus índices de interface correspondentes, use a exibição do comando Route Print. Você pode usar valores decimais ou hexadecimais para o índice de interface. Para valores hexadecimais, preceda o número hexadecimal com 0x. Quando o parâmetro If é omitido, a interface é determinada do endereço do gateway. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Valores grandes na coluna **métrica** da tabela de roteamento são o resultado de permitir que TCP/IP determine automaticamente a métrica para rotas na tabela de roteamento com base na configuração de endereço IP, máscara de sub-rede e gateway padrão para cada interface de LAN. A determinação automática da métrica da interface, habilitada por padrão, determina a velocidade de cada interface e ajusta as métricas de rotas para cada interface para que a interface mais rápida crie as rotas com a métrica mais baixa. Para remover as métricas grandes, desabilite a determinação automática da métrica de interface das propriedades avançadas do protocolo TCP/IP para cada conexão de LAN.

- Os nomes podem ser usados para o *destino* se houver uma entrada apropriada no arquivo de *redes* locais armazenado na `systemroot\System32\Drivers\\` pasta. Os nomes podem ser usados para o *Gateway* , desde que possam ser resolvidos para um endereço IP por meio de técnicas de resolução de nome de host padrão, como consultas de DNS (sistema de nomes de domínio), uso do arquivo de hosts locais armazenados na `systemroot\system32\drivers\\` pasta e resolução de nomes NetBIOS.

- Se o comando for **Print** ou **delete**, o parâmetro de *Gateway* poderá ser omitido e os curingas poderão ser usados para o destino e o gateway. O valor de *destino* pode ser um valor curinga especificado por um asterisco `(*)` . Se o destino especificado contiver um asterisco `(*)` ou um ponto de interrogação (?), ele será tratado como um curinga e somente as rotas de destino correspondentes serão impressas ou excluídas. O asterisco corresponde a qualquer cadeia de caracteres e o ponto de interrogação corresponde a qualquer caractere único. Por exemplo, `10.\*.1, 192.168.\*` , `127.\*` e `\*224\*` são todos os usos válidos do curinga asterisco.

- Usando uma combinação sem suporte de um valor de destino e máscara de sub-rede (máscara de sub-rede) exibe uma mensagem de erro "rota: máscara de endereço de gateway inadequado". Essa mensagem de erro é exibida quando o destino contém um ou mais bits definidos como 1 em locais de bits em que o bit da máscara de sub-rede correspondente está definido como 0. Para testar essa condição, expresse a máscara de destino e de sub-rede usando a notação binária. A máscara de sub-rede na notação binária consiste em uma série de 1 bits, representando a parte do endereço de rede do destino e uma série de 0 bits, representando a parte do endereço do host do destino. Verifique para determinar se há bits no destino que são definidos como 1 para a parte do destino que é o endereço de host (conforme definido pela máscara de sub-rede).

## <a name="examples"></a>Exemplos

Para exibir todo o conteúdo da tabela de roteamento IP, digite:

```
route print
```

Para exibir as rotas na tabela de roteamento de IP que começam com 10, digite:

```
route print 10.*
```

Para adicionar uma rota padrão com o endereço de gateway padrão de 192.168.12.1, digite:

```
route add 0.0.0.0 mask 0.0.0.0 192.168.12.1
```

Para adicionar uma rota ao 10.41.0.0 de destino com a máscara de sub-rede de 255.255.0.0 e o endereço do próximo salto de 10.27.0.1, digite:

```
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1
```

Para adicionar uma rota persistente ao 10.41.0.0 de destino com a máscara de sub-rede de 255.255.0.0 e o endereço do próximo salto de 10.27.0.1, digite:

```
route /p add 10.41.0.0 mask 255.255.0.0 10.27.0.1
```

Para adicionar uma rota para o 10.41.0.0 de destino com a máscara de sub-rede de 255.255.0.0, o endereço do próximo salto de 10.27.0.1 e a métrica de custo de 7, digite:

```
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 metric 7
```

Para adicionar uma rota para o 10.41.0.0 de destino com a máscara de sub-rede de 255.255.0.0, o endereço do próximo salto de 10.27.0.1 e usando o índice de interface 0x3, digite:

```
route add 10.41.0.0 mask 255.255.0.0 10.27.0.1 if 0x3
```

Para excluir a rota para o 10.41.0.0 de destino com a máscara de sub-rede de 255.255.0.0, digite:

```
route delete 10.41.0.0 mask 255.255.0.0
```

Para excluir todas as rotas na tabela de roteamento de IP que começam com 10, digite:

```
route delete 10.*
```

Para alterar o endereço do próximo salto da rota com o destino de 10.41.0.0 e a máscara de sub-rede de 255.255.0.0 de 10.27.0.1 para 10.27.0.25, digite:

```
route change 10.41.0.0 mask 255.255.0.0 10.27.0.25
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
