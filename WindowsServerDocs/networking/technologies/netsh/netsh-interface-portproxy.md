---
title: Comandos netsh para interface portproxy
description: Use os comandos netsh interface portproxy para funcionar como proxies entre as redes e aplicativos IPv4 e IPv6.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 08/30/2018
ms.openlocfilehash: e9c4cff4d1424c244857cf75be41d445b299f1f2
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80853739"
---
# <a name="netsh-interface-portproxy-commands"></a>Comandos netsh interface portproxy

Use os comandos **netsh interface portproxy** para funcionar como proxies entre as redes e aplicativos IPv4 e IPv6. Você pode usar esses comandos para estabelecer o serviço de proxy das seguintes maneiras:

-   Mensagens de computador e aplicativo configuradas para IPv4 enviadas para outros computadores e aplicativos configurados para IPv4.

-   Mensagens de computador e aplicativo configuradas para IPv4 enviadas para computadores e aplicativos configurados para IPv6.

-   Mensagens de computador e aplicativo configuradas para IPv6 enviadas para computadores e aplicativos configurados para IPv4.

-   Mensagens de computador e aplicativo configuradas para IPv6 enviadas para outros computadores e aplicativos configurados para IPv6.

Ao gravar arquivos ou scripts em lotes usando esses comandos, cada comando deve começar com **netsh interface portproxy**. Por exemplo, ao usar o comando **delete v4tov6** para especificar que o servidor portproxy exclua uma porta e um endereço IPv4 da lista de endereços IPv4 para os quais o servidor escuta, o script ou o arquivo em lotes deverá usar a seguinte sintaxe:

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

Os comandos netsh interface portproxy disponíveis são:

-   [add v4tov4](#add-v4tov4)

-   [add v4tov6](#add-v4tov6)

-   [add v6tov4](#add-v6tov4)

-   [add v6tov6](#add-v6tov6)

-   [delete v4tov4](#delete-v4tov4)

-   [delete v4tov6](#delete-v4tov6)

-   [delete v6tov6](#delete-v6tov6)

-   [reset](#reset)

-   [set v4tov4](#set-v4tov4)

-   [set v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [set v6tov6](#set-v6tov6)

-   [show all](#show-all)

-   [show v4tov4](#show-v4tov4)

-   [show v4tov6](#show-v4tov6)

-   [show v6tov4](#show-v6tov4)

-   [show v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>add v4tov4

O servidor portproxy escuta mensagens enviadas a uma porta e um endereço IPv4 específicos e mapeia uma porta e um endereço IPv4 para enviar as mensagens recebidas após estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv4, por número da porta ou nome do serviço, na qual escutar.                                                            |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv4, por número da porta ou nome do serviço, à qual se conectar. Se **connectport** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv4 para o qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|    **protocol**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="add-v4tov6"></a>add v4tov6

O servidor portproxy escuta mensagens enviadas a uma porta e um endereço IPv4 específicos e mapeia uma porta e um endereço IPv6 para enviar as mensagens recebidas após estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv4, por número da porta ou nome do serviço, na qual escutar.                                                            |
| **connectaddress** | Especifica o endereço IPv6 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv6, por número da porta ou nome do serviço, à qual se conectar. Se **connectport** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv4 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocol**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="add-v6tov4"></a>add v6tov4

O servidor portproxy escuta mensagens enviadas a uma porta e um endereço IPv6 específicos e mapeia uma porta e um endereço IPv4 para os quais enviar as mensagens recebidas após estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv6, por número da porta ou nome do serviço, na qual escutar.                                                            |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv4, por número da porta ou nome do serviço, à qual se conectar. Se **connectport** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv6 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocol**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="add-v6tov6"></a>add v6tov6

O servidor portproxy escuta mensagens enviadas a uma porta e um endereço IPv6 específicos e mapeia uma porta e um endereço IPv6 para os quais enviar as mensagens recebidas após estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv6, por número da porta ou nome do serviço, na qual escutar.                                                            |
| **connectaddress** | Especifica o endereço IPv6 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv6, por número da porta ou nome do serviço, à qual se conectar. Se **connectport** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv6 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocol**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="delete-v4tov4"></a>delete v4tov4

O servidor portproxy exclui um endereço IPv4 da lista de portas e endereços IPv4 para os quais o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica a porta IPv4 a ser excluída.                                    |
| **listenaddress** | Especifica o endereço IPv4 a ser excluído. Se um endereço não for especificado, o padrão será o computador local. |
|   **protocol**    |                                      Especifica o protocolo a ser usado.                                      |

## <a name="delete-v4tov6"></a>delete v4tov6

O servidor portproxy exclui uma porta e um endereço IPv4 da lista de endereços IPv4 para os quais o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica a porta IPv4 a ser excluída.                                    |
| **listenaddress** | Especifica o endereço IPv4 a ser excluído. Se um endereço não for especificado, o padrão será o computador local. |
|   **protocol**    |                                      Especifica o protocolo a ser usado.                                      |

## <a name="delete-v6tov4"></a>delete v6tov4

O servidor portproxy exclui uma porta e um endereço IPv6 da lista de endereços IPv6 para os quais o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica a porta IPv6 a ser excluída.                                    |
| **listenaddress** | Especifica o endereço IPv6 a ser excluído. Se um endereço não for especificado, o padrão será o computador local. |
|   **protocol**    |                                      Especifica o protocolo a ser usado.                                      |

## <a name="delete-v6tov6"></a>delete v6tov6

O servidor portproxy exclui um endereço IPv6 da lista de endereços IPv6 para os quais o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica a porta IPv6 a ser excluída.                                    |
| **listenaddress** | Especifica o endereço IPv6 a ser excluído. Se um endereço não for especificado, o padrão será o computador local. |
|   **protocol**    |                                      Especifica o protocolo a ser usado.                                      |

## <a name="reset"></a>reset

Redefine o estado de configuração do IPv6.

### <a name="syntax"></a>Sintaxe

`reset`

## <a name="set-v4tov4"></a>set v4tov4

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criado com o comando **add v4tov4** ou adiciona uma nova entrada à lista que mapeia os pares porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv4, por número da porta ou nome do serviço, na qual escutar.                                                            |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv4, por número da porta ou nome do serviço, à qual se conectar. Se **connectport** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv4 para o qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|    **protocol**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="set-v4tov6"></a>set v4tov6

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criado com o comando **add v4tov6** ou adiciona uma nova entrada à lista que mapeia os pares porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv4, por número da porta ou nome do serviço, na qual escutar.                                                            |
| **connectaddress** | Especifica o endereço IPv6 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv6, por número da porta ou nome do serviço, à qual se conectar. Se **connectport** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv4 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocol**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="set-v6tov4"></a>set v6tov4

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criado com o comando **add v6tov4** ou adiciona uma nova entrada à lista que mapeia os pares porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv6, por número da porta ou nome do serviço, na qual escutar.                                                            |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv4, por número da porta ou nome do serviço, à qual se conectar. Se **connectport** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv6 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocol**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="set-v6tov6"></a>set v6tov6

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criado com o comando **add v6tov6** ou adiciona uma nova entrada à lista que mapeia os pares porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

#### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            Especifica a porta IPv6, por número da porta ou nome do serviço, na qual escutar.                                                            |
| **connectaddress** | Especifica o endereço IPv6 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|  **connectport**   |        Especifica a porta IPv6, por número da porta ou nome do serviço, à qual se conectar. Se **connectport** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv6 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se você não especificar um endereço, o padrão será o computador local. |
|    **protocol**    |                                                                                   Especifica o protocolo a ser usado.                                                                                   |

## <a name="show-all"></a>exibir tudo

Exibe todos os parâmetros portproxy, incluindo pares porta/endereço para v4tov4, v4tov6, v6tov4 e v6tov6.

### <a name="syntax"></a>Sintaxe

`show all`


## <a name="show-v4tov4"></a>show v4tov4

Exibe os parâmetros v4tov4 portproxy.

### <a name="syntax"></a>Sintaxe

`show v4tov4`

## <a name="show-v4tov6"></a>show v4tov6

Exibe os parâmetros v4tov6 portproxy.

### <a name="syntax"></a>Sintaxe

`show v4tov6`

## <a name="show-v6tov4"></a>show v6tov4

Exibe os parâmetros v6tov4 portproxy.

### <a name="syntax"></a>Sintaxe

`show v6tov4`


## <a name="show-v6tov6"></a>show v6tov6

Exibe os parâmetros v6tov6 portproxy.

### <a name="syntax"></a>Sintaxe

`show v6tov6`

---
