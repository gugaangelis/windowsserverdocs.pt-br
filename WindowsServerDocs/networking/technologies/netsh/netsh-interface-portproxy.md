---
title: Comandos netsh para interface portproxy
description: Use os comandos netsh interface portproxy para atuar como proxies entre redes IPv4 e IPv6 e aplicativos.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 6244213ea689f07230ce53288e52959972112037
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405510"
---
# <a name="netsh-interface-portproxy-commands"></a>Comandos netsh interface portproxy

Use os comandos **netsh interface portproxy** para atuar como proxies entre redes IPv4 e IPv6 e aplicativos. Você pode usar esses comandos para estabelecer o serviço de proxy das seguintes maneiras:

-   Mensagens de computador e aplicativo configuradas pelo IPv4 enviadas a outros computadores e aplicativos configurados para IPv4.

-   Mensagens de computador e aplicativo configuradas pelo IPv4 enviadas a aplicativos e computadores configurados para IPv6.

-   Mensagens de computador e aplicativo configuradas pelo IPv6 enviadas a aplicativos e computadores configurados por IPv4.

-   Mensagens de computador e aplicativo configuradas pelo IPv6 enviadas a outros computadores e aplicativos configurados para IPv6.

Ao gravar arquivos em lotes ou scripts usando esses comandos, cada comando deve começar com a **interface do netsh**. Por exemplo, ao usar o comando **delete v4tov6** para especificar que o servidor portproxy exclui uma porta IPv4 e um endereço da lista de endereços IPv4 para os quais o servidor escuta, o arquivo em lotes ou o script deve usar a seguinte sintaxe:

```PowerShell
netsh interface portproxy delete v4tov6listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address| HostName}] [[protocol=]tcp]
```

Os comandos do netsh interface portproxy disponíveis são:

-   [Adicionar v4tov4](#add-v4tov4)

-   [Adicionar v4tov6](#add-v4tov6)

-   [Adicionar v6tov4](#add-v6tov4)

-   [Adicionar v6tov6](#add-v6tov6)

-   [excluir v4tov4](#delete-v4tov4)

-   [excluir v4tov6](#delete-v4tov6)

-   [excluir v6tov6](#delete-v6tov6)

-   [definido](#reset)

-   [definir v4tov4](#set-v4tov4)

-   [definir v4tov6](#set-v4tov6)

-   [setv6tov4](#set-v6tov4)

-   [definir v6tov6](#set-v6tov6)

-   [Mostrar tudo](#show-all)

-   [Mostrar v4tov4](#show-v4tov4)

-   [Mostrar v4tov6](#show-v4tov6)

-   [Mostrar v6tov4](#show-v6tov4)

-   [Mostrar v6tov6](#show-v6tov6)


## <a name="add-v4tov4"></a>Adicionar v4tov4

O servidor portproxy escuta as mensagens enviadas a uma porta e um endereço IPv4 específicos e mapeia uma porta e um endereço IPv4 para enviar as mensagens recebidas depois de estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName}] [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName}] [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros


|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv4, por número da porta ou nome do serviço, para escuta.                                                            |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv4, por número da porta ou nome do serviço, ao qual se conectar. Se **ConnectPort** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv4 para o qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|    **protocolo**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="add-v4tov6"></a>Adicionar v4tov6

O servidor portproxy escuta mensagens enviadas a uma porta e um endereço IPv4 específicos e mapeia uma porta e um endereço IPv6 para enviar as mensagens recebidas depois de estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv4, por número da porta ou nome do serviço, para escuta.                                                            |
| **connectaddress** | Especifica o endereço IPv6 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv6, por número da porta ou nome do serviço, ao qual se conectar. Se **ConnectPort** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv4 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocolo**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="add-v6tov4"></a>Adicionar v6tov4

O servidor portproxy escuta mensagens enviadas a uma porta e um endereço IPv6 específicos e mapeia uma porta e um endereço IPv4 para o qual enviar as mensagens recebidas depois de estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv6, por número da porta ou nome do serviço, para escuta.                                                            |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv4, por número da porta ou nome do serviço, ao qual se conectar. Se **ConnectPort** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv6 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocolo**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="add-v6tov6"></a>Adicionar v6tov6

O servidor portproxy escuta as mensagens enviadas a uma porta e um endereço IPv6 específicos e mapeia uma porta e um endereço IPv6 para o qual enviar as mensagens recebidas depois de estabelecer uma conexão TCP separada.

### <a name="syntax"></a>Sintaxe

```PowerShell
add v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv6, por número da porta ou nome do serviço, para escuta.                                                            |
| **connectaddress** | Especifica o endereço IPv6 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv6, por número da porta ou nome do serviço, ao qual se conectar. Se **ConnectPort** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv6 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocolo**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="delete-v4tov4"></a>excluir v4tov4

O servidor portproxy exclui um endereço IPv4 da lista de portas e endereços IPv4 para os quais o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v4tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica a porta IPv4 a ser excluída.                                    |
| **listenaddress** | Especifica o endereço IPv4 a ser excluído. Se um endereço não for especificado, o padrão será o computador local. |
|   **protocolo**    |                                      Especifica o protocolo a ser usado.                                      |

## <a name="delete-v4tov6"></a>excluir v4tov6

O servidor portproxy exclui uma porta IPv4 e um endereço da lista de endereços IPv4 para os quais o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell 
delete v4tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica a porta IPv4 a ser excluída.                                    |
| **listenaddress** | Especifica o endereço IPv4 a ser excluído. Se um endereço não for especificado, o padrão será o computador local. |
|   **protocolo**    |                                      Especifica o protocolo a ser usado.                                      |

## <a name="delete-v6tov4"></a>excluir v6tov4

O servidor portproxy exclui uma porta IPv6 e um endereço da lista de endereços IPv6 para os quais o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v6tov4 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica a porta IPv6 a ser excluída.                                    |
| **listenaddress** | Especifica o endereço IPv6 a ser excluído. Se um endereço não for especificado, o padrão será o computador local. |
|   **protocolo**    |                                      Especifica o protocolo a ser usado.                                      |

## <a name="delete-v6tov6"></a>excluir v6tov6

O servidor portproxy exclui um endereço IPv6 da lista de endereços IPv6 para os quais o servidor escuta.

### <a name="syntax"></a>Sintaxe

```PowerShell
delete v6tov6 listenport= {Integer | ServiceName} [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                   |                                                                                                          |
|-------------------|----------------------------------------------------------------------------------------------------------|
|  **listenport**   |                                    Especifica a porta IPv6 a ser excluída.                                    |
| **listenaddress** | Especifica o endereço IPv6 a ser excluído. Se um endereço não for especificado, o padrão será o computador local. |
|   **protocolo**    |                                      Especifica o protocolo a ser usado.                                      |

## <a name="reset"></a>Definido

Redefine o estado de configuração do IPv6.

### <a name="syntax"></a>Sintaxe

`reset`

## <a name="set-v4tov4"></a>definir v4tov4

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criado com o comando **Add v4tov4** ou adiciona uma nova entrada à lista que mapeia pares de porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v4tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv4, por número da porta ou nome do serviço, para escuta.                                                            |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv4, por número da porta ou nome do serviço, ao qual se conectar. Se **ConnectPort** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv4 para o qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|    **protocolo**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="set-v4tov6"></a>definir v4tov6

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criado com o comando **Add v4tov6** ou adiciona uma nova entrada à lista que mapeia pares de porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v4tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv4Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv4, por número da porta ou nome do serviço, para escuta.                                                            |
| **connectaddress** | Especifica o endereço IPv6 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv6, por número da porta ou nome do serviço, ao qual se conectar. Se **ConnectPort** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv4 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocolo**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="set-v6tov4"></a>definir v6tov4

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criado com o comando **Add v6tov4** ou adiciona uma nova entrada à lista que mapeia pares de porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell
set v6tov4 listenport= {Integer | ServiceName} [[connectaddress=] {IPv4Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                   |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                           Especifica a porta IPv6, por número da porta ou nome do serviço, para escuta.                                                            |
| **connectaddress** | Especifica o endereço IPv4 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local. |
|  **connectport**   |       Especifica a porta IPv4, por número da porta ou nome do serviço, ao qual se conectar. Se **ConnectPort** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv6 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|    **protocolo**    |                                                                                  Especifica o protocolo a ser usado.                                                                                   |

## <a name="set-v6tov6"></a>definir v6tov6

Modifica os valores de parâmetro de uma entrada existente no servidor portproxy criado com o comando **add v6tov6** ou adiciona uma nova entrada à lista que mapeia pares de porta/endereço.

### <a name="syntax"></a>Sintaxe

```PowerShell 
set v6tov6 listenport= {Integer | ServiceName} [[connectaddress=] {IPv6Address | HostName} [[connectport=] {Integer | ServiceName}] [[listenaddress=] {IPv6Address | HostName} [[protocol=]tcp]
```

### <a name="parameters"></a>Parâmetros

|                    |                                                                                                                                                                                                    |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   **listenport**   |                                                            Especifica a porta IPv6, por número da porta ou nome do serviço, para escuta.                                                            |
| **connectaddress** | Especifica o endereço IPv6 ao qual se conectar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se um endereço não for especificado, o padrão será o computador local.  |
|  **connectport**   |        Especifica a porta IPv6, por número da porta ou nome do serviço, ao qual se conectar. Se **ConnectPort** não for especificado, o padrão será o valor de **listenport** no computador local.        |
| **listenaddress**  | Especifica o endereço IPv6 no qual escutar. Os valores aceitáveis são endereço IP, nome NetBIOS do computador ou nome DNS do computador. Se você não especificar um endereço, o padrão será o computador local. |
|    **protocolo**    |                                                                                   Especifica o protocolo a ser usado.                                                                                   |

## <a name="show-all"></a>exibir tudo

Exibe todos os parâmetros de portproxy, incluindo pares de porta/endereço para v4tov4, v4tov6, v6tov4 e v6tov6.

### <a name="syntax"></a>Sintaxe

`show all`


## <a name="show-v4tov4"></a>Mostrar v4tov4

Exibe os parâmetros de portproxy do v4tov4.

### <a name="syntax"></a>Sintaxe

`show v4tov4`

## <a name="show-v4tov6"></a>Mostrar v4tov6

Exibe os parâmetros de portproxy do v4tov6.

### <a name="syntax"></a>Sintaxe

`show v4tov6`

## <a name="show-v6tov4"></a>Mostrar v6tov4

Exibe os parâmetros de portproxy do v6tov4.

### <a name="syntax"></a>Sintaxe

`show v6tov4`


## <a name="show-v6tov6"></a>Mostrar v6tov6

Exibe os parâmetros de portproxy do v6tov6.

### <a name="syntax"></a>Sintaxe

`show v6tov6`

---
