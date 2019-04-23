---
title: Comandos Netsh para interface portproxy
description: Use os comandos do netsh interface portproxy para agir como proxies entre aplicativos e redes IPv4 e IPv6.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 194a418fe6b33e312a3f2529e82d50d76cd15f4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842477"
---
# <a name="netsh-interface-portproxy-commands"></a>Comandos netsh interface portproxy

Use o **netsh interface portproxy** comandos para agir como proxies entre aplicativos e redes IPv4 e IPv6. Você pode usar esses comandos para estabelecer o serviço de proxy das seguintes maneiras:

-   IPv4-aplicativos e as mensagens enviadas para outros aplicativos e computadores configurados para IPv4.

-   Mensagens de aplicativo e computadores configurados pelo IPv4 enviada para o IPv6 configurado computadores e aplicativos.

-   Mensagens de aplicativo e computador configurado para IPv6 enviados ao IPv4 configurado computadores e aplicativos.

-   IPv6-aplicativos e as mensagens enviadas para outros aplicativos e computadores configurados para IPv6.

Ao gravar arquivos em lotes ou scripts que usam esses comandos, cada comando deve começar com **netsh interface portproxy**. Por exemplo, ao usar o **excluir v4tov6** comando para especificar que o servidor portproxy exclui um endereço e porta IPv4 na lista de endereços IPv4 para o qual o servidor escuta, o arquivo em lotes ou script deve usar a sintaxe a seguir:

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

Os comandos do netsh disponíveis interface portproxy são:

-   [Adicionar v4tov4](#add-v4tov4)

-   [Adicionar v4tov6](#add-v4tov6)

-   [Adicionar v6tov4](#add-v6tov4)

-   [Adicionar v6tov6](#add-v6tov6)

-   [delete v4tov4](#delete-v4tov4)

-   [delete v4tov6](#delete-v4tov6)

-   [delete v6tov6](#delete-v6tov6)

-   [reset](#reset)

-   [set v4tov4](#set-v4tov4)

-   [set v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [set v6tov6](#set-v6tov6)

-   [Mostrar tudo](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>Adicionar v4tov4

O servidor portproxy escuta para mensagens enviadas para uma porta específica e o endereço IPv4 e mapeia a porta e o IPv4 de endereço para enviar as mensagens recebidas após estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros


| | |
|-----|--------|----------|
| **listenport**     | Especifica a porta IPv4, pelo número da porta ou o serviço de nome para escuta.                                                                                                                      | Obrigatório |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **connectport**    | Especifica a porta IPv4, pelo número da porta ou o serviço de nome, ao qual se conectar. Se **connectport** não for especificado, o padrão é o valor de **listenport** no computador local.              |          |
| **listenaddress**  | Especifica o endereço IPv4 para o qual escutar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **protocol**       | Especifica o protocolo a ser usado.                                                                                                                                                                    |          |

## <a name="add-v4tov6"></a>Adicionar v4tov6

O servidor portproxy escuta as mensagens enviadas a uma porta específica e o endereço IPv4 e mapas de endereço de uma porta e o IPv6 para enviar as mensagens recebidas após estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|-----------|-------------|----------|
| **listenport**     | Especifica a porta IPv4, pelo número da porta ou o serviço de nome para escuta.       | Obrigatório |
| **connectaddress** | Especifica o endereço IPv6 para o qual se conectar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **connectport**    | Especifica a porta IPv6, por número da porta ou o serviço de nome, ao qual se conectar. Se **connectport** não for especificado, o padrão é o valor de **listenport** no computador local.              |          |
| **listenaddress**  | Especifica o endereço IPv4 para escuta. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local.  |          |
| **protocol**       | Especifica o protocolo a ser usado.                                                                                                                                                                    |          |

## <a name="add-v6tov4"></a>Adicionar v6tov4

O servidor portproxy escuta as mensagens enviadas a uma porta específica e o endereço IPv6 e mapeia a porta e o IPv4 de endereço ao qual enviar as mensagens recebidas após estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|------------|-------------|----------|
| **listenport**     | Especifica a porta IPv6, por número da porta ou o serviço de nome para escuta.              | Obrigatório |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **connectport**    | Especifica a porta IPv4, pelo número da porta ou o serviço de nome, ao qual se conectar. Se **connectport** não for especificado, o padrão é o valor de **listenport** no computador local.              |          |
| **listenaddress**  | Especifica o endereço IPv6 para escuta. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local.  |          |
| **protocol**       | Especifica o protocolo a ser usado.      |          |

## <a name="add-v6tov6"></a>Adicionar v6tov6

O servidor portproxy escuta as mensagens enviadas a uma porta específica e o endereço IPv6 e mapeia a porta e o IPv6 de endereço ao qual enviar as mensagens recebidas após estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|-------------|------------------|----------|
| **listenport**     | Especifica a porta IPv6, por número da porta ou o serviço de nome para escuta.       | Obrigatório |
| **connectaddress** | Especifica o endereço IPv6 para o qual se conectar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **connectport**    | Especifica a porta IPv6, por número da porta ou o serviço de nome, ao qual se conectar. Se **connectport** não for especificado, o padrão é o valor de **listenport** no computador local.              |          |
| **listenaddress**  | Especifica o endereço IPv6 para escuta. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local.  |          |
| **protocol**       | Especifica o protocolo a ser usado.                                                                                                                                                                    |          |

## <a name="delete-v4tov4"></a>Excluir v4tov4

O servidor portproxy exclui um endereço IPv4 da lista de portas e endereços para o qual o servidor escuta IPv4.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Especifica a porta IPv4 a ser excluída.                                                                       | Obrigatório |
| **listenaddress** | Especifica o endereço IPv4 a ser excluído. Se um endereço não for especificado, o padrão é o computador local. |          |
| **protocol**      | Especifica o protocolo a ser usado.                                                                           |          |

## <a name="delete-v4tov6"></a>Excluir v4tov6

O servidor portproxy exclui um endereço e porta IPv4 na lista de endereços IPv4 para o qual o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Especifica a porta IPv4 a ser excluída.                                                                       | Obrigatório |
| **listenaddress** | Especifica o endereço IPv4 a ser excluído. Se um endereço não for especificado, o padrão é o computador local. |          |
| **protocol**      | Especifica o protocolo a ser usado.                                                                           |          |

## <a name="delete-v6tov4"></a>Excluir v6tov4

O servidor portproxy exclui uma porta IPv6 e o endereço da lista de endereços IPv6 para o qual o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Especifica a porta IPv6 a ser excluída.                                                                       | Obrigatório |
| **listenaddress** | Especifica o endereço IPv6 para excluir. Se um endereço não for especificado, o padrão é o computador local. |          |
| **protocol**      | Especifica o protocolo a ser usado.                                                                           |          |

## <a name="delete-v6tov6"></a>Excluir v6tov6

O servidor portproxy exclui um endereço IPv6 da lista de endereços IPv6 para o qual o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|-------------------|----------------------------------------------------------------------------------------------------------|----------|
| **listenport**    | Especifica a porta IPv6 a ser excluída.                                                                       | Obrigatório |
| **listenaddress** | Especifica o endereço IPv6 para excluir. Se um endereço não for especificado, o padrão é o computador local. |          |
| **protocol**      | Especifica o protocolo a ser usado.                                                                           |          |

## <a name="reset"></a>Redefinir

Redefine o estado de configuração de IPv6.

### <a name="syntax"></a>Sintaxe

`reset`

## <a name="set-v4tov4"></a>conjunto v4tov4

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criada com o **add v4tov4** de comando ou adiciona uma nova entrada à lista que mapeia os pares de porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|--------------------|---------------------------|----------|
| **listenport**     | Especifica a porta IPv4, pelo número da porta ou o serviço de nome para escuta.     | Obrigatório |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **connectport**    | Especifica a porta IPv4, pelo número da porta ou o serviço de nome, ao qual se conectar. Se **connectport** não for especificado, o padrão é o valor de **listenport** no computador local.              |          |
| **listenaddress**  | Especifica o endereço IPv4 para o qual escutar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **protocol**       | Especifica o protocolo a ser usado.                                                                                                                                                                    |          |

## <a name="set-v4tov6"></a>conjunto v4tov6

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criada com o **adicionar v4tov6** de comando ou adiciona uma nova entrada à lista que mapeia os pares de porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|--------------------|---------------------|----------|
| **listenport**     | Especifica a porta IPv4, pelo número da porta ou o serviço de nome para escuta.     | Obrigatório |
| **connectaddress** | Especifica o endereço IPv6 para o qual se conectar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **connectport**    | Especifica a porta IPv6, por número da porta ou o serviço de nome, ao qual se conectar. Se **connectport** não for especificado, o padrão é o valor de **listenport** no computador local.              |          |
| **listenaddress**  | Especifica o endereço IPv4 para escuta. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local.  |          |
| **protocol**       | Especifica o protocolo a ser usado.                                                                                                                                                                    |          |

## <a name="set-v6tov4"></a>set v6tov4

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criada com o **adicionar v6tov4** de comando ou adiciona uma nova entrada à lista que mapeia os pares de porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|--------------------|----------------------|----------|
| **listenport**     | Especifica a porta IPv6, por número da porta ou o serviço de nome para escuta.      | Obrigatório |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local. |          |
| **connectport**    | Especifica a porta IPv4, pelo número da porta ou o serviço de nome, ao qual se conectar. Se **connectport** não for especificado, o padrão é o valor de **listenport** no computador local.              |          |
| **listenaddress**  | Especifica o endereço IPv6 para escuta. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local.  |          |
| **protocol**       | Especifica o protocolo a ser usado.                                                                                                                                                                    |          |

## <a name="set-v6tov6"></a>conjunto v6tov6

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criada com o **add v6tov6** de comando ou adiciona uma nova entrada à lista que mapeia os pares de porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|   |   |
|--------------------|-------------------------|----------|
| **listenport**     | Especifica a porta IPv6, por número da porta ou o serviço de nome para escuta.   | Obrigatório |
| **connectaddress** | Especifica o endereço IPv6 para o qual se conectar. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão é o computador local.  |          |
| **connectport**    | Especifica a porta IPv6, por número da porta ou o serviço de nome, ao qual se conectar. Se **connectport** não for especificado, o padrão é o valor de **listenport** no computador local.               |          |
| **listenaddress**  | Especifica o endereço IPv6 para escuta. Os valores aceitáveis são endereços IP, nome NetBIOS do computador ou nome DNS do computador. Se você não especificar um endereço, o padrão é o computador local. |          |
| **protocol**       | Especifica o protocolo a ser usado.                                                                                                                                                                     |          |

## <a name="show-all"></a>exibir tudo

Exibe todos os parâmetros de portproxy, incluindo pares de endereço/porta para v4tov4, v4tov6, v6tov4 e v6tov6.

### <a name="syntax"></a>Sintaxe

`show all`


## <a name="show-v4tov4"></a>Mostrar v4tov4

Exibe os parâmetros de portproxy v4tov4.

### <a name="syntax"></a>Sintaxe

`show v4tov4`

## <a name="show-v4tov6"></a>Mostrar v4tov6

Exibe os parâmetros de portproxy v4tov6.

### <a name="syntax"></a>Sintaxe

`show v4tov6`

## <a name="show-v6tov4"></a>show v6tov4

Exibe os parâmetros de portproxy v6tov4.

### <a name="syntax"></a>Sintaxe

`show v6tov4`


## <a name="show-v6tov6"></a>Mostrar v6tov6

Exibe os parâmetros de portproxy v6tov6.

### <a name="syntax"></a>Sintaxe

`show v6tov6`

---
