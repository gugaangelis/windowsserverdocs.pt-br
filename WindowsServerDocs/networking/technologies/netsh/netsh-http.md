---
title: Comandos Netsh para Hypertext Transfer protocolo (HTTP)
description: Use o netsh http para consultar e definir parâmetros e configurações de HTTP. sys.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3c5f3927abf1a2394c2dd5b8ea664c7de8d5f614
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446197"
---
# <a name="netsh-http-commands"></a>Comandos Netsh http


Use **netsh http** para consultar e definir configurações de HTTP. sys e parâmetros.  

>[!TIP]
>Se você estiver usando o Windows PowerShell em um computador executando o Windows Server 2016 ou Windows 10, digite **netsh** e pressione Enter. No prompt do netsh, digite **http** e pressione Enter para obter o prompt do netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Netsh http\>

Os comandos do netsh disponíveis http são:

- [Adicionar iplisten](#add-iplisten)
- [Adicionar sslcert](#add-sslcert)
- [Adicionar o tempo limite](#add-timeout)
- [Adicionar urlacl](#add-urlacl)
- [Excluir cache](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [excluir o tempo limite](#delete-timeout)
- [Excluir urlacl](#delete-urlacl)
- [liberar logbuffer](#flush-logbuffer)
- [Mostrar cachestate](#show-cachestate)
- [Mostrar iplisten](#show-iplisten)
- [Mostrar o estado do serviço](#show-servicestate)
- [Mostrar sslcert](#show-sslcert)
- [Mostrar o tempo limite](#show-timeout)
- [Mostrar urlacl](#show-urlacl)

## <a name="add-iplisten"></a>Adicionar iplisten

Adiciona um novo endereço IP à lista de escuta de IP, exceto o número da porta.

**Sintaxe**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parâmetros**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | O endereço IPv4 ou IPv6 a ser adicionado para o IP de lista de escuta. A lista de escuta IP é usada para definir o escopo a lista de endereços para o qual associa o serviço HTTP. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6. | Obrigatório |

---

**Exemplos**

Estes são exemplos de quatro do **adicionar iplisten** comando.

-   Adicionar iplisten ipaddress = FE80:: 1
-   Adicionar iplisten ipaddress = 1.1.1.1
-   Adicionar iplisten ipaddress = 0.0.0.0
-   Adicionar iplisten ipaddress =::

---

## <a name="add-sslcert"></a>Adicionar sslcert

Adiciona um novo certificado de servidor SSL de associação e correspondente de políticas de certificado de cliente para um endereço IP e porta.

**Sintaxe**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parâmetros**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Especifica o endereço IP e porta para a associação. Um caractere de dois-pontos (:) é usado como um delimitador entre o endereço IP e o número da porta.                        | Obrigatório |
|                 **certhash**                 |                                     Especifica o hash SHA do certificado. O hash é de 20 bytes de comprimento e é especificado como uma cadeia de caracteres hexadecimal.                                      | Obrigatório |
|                  **appid**                   |                                                                  Especifica o GUID para identificar o aplicativo proprietário.                                                                  | Obrigatório |
|              **certstorename**               |                                  Especifica o nome do repositório para o certificado. O padrão é MY. Certificado deve ser armazenado no contexto do computador local.                                  | Opcional |
|        **verifyclientcertrevocation**        |                                                      Especifica o ativa ativar/desativar a verificação de revogação de certificados de cliente.                                                       | Opcional |
| **verifyrevocationwithcachedclientcertonly** |                                      Especifica se o uso do certificado de cliente armazenadas em cache apenas para verificação de revogação está habilitado ou desabilitado.                                       | Opcional |
|                **usagecheck**                |                                                      Especifica se a verificação de uso está habilitada ou desabilitada. Padrão está habilitado.                                                       | Opcional |
|         **revocationfreshnesstime**          | Especifica o intervalo de tempo em segundos, para verificar se há uma lista de revogação de certificados atualizada (CRL). Se esse valor for zero, a nova CRL é atualizada apenas se anterior expirar. | Opcional |
|           **urlretrievaltimeout**            |                            Especifica o intervalo de tempo limite (em milissegundos) após a tentativa de recuperar a lista de revogação de certificado para a URL remota.                            | Opcional |
|             **sslctlidentifier**             |                Especifica a lista de emissores de certificado que podem ser confiáveis. Essa lista pode ser um subconjunto dos emissores de certificado confiável para o computador.                 | Opcional |
|             **sslctlstorename**              |                                                Especifica o nome do repositório de certificados em Computador_Local onde SslCtlIdentifier está armazenado.                                                | Opcional |
|              **dsmapperusage**               |                                                        Especifica se os mapeadores de DS está habilitado ou desabilitado. Padrão é disabled.                                                         | Opcional |
|          **clientcertnegotiation**           |                                              Especifica se a negociação do certificado está habilitada ou desabilitada. Padrão é disabled.                                               | Opcional |

---

**Exemplos**

A seguir está um exemplo de como o **adicionar sslcert** comando.

Add sslcert ipport=0.0.0.0:443 certhash 1.1.1.1:443 de = = 0102030405060708090A0B0C0D0E0F1011121314 appid = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>Adicionar o tempo limite

Adiciona um tempo limite global para o serviço.

**Sintaxe** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parâmetros**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Tipo de tempo limite para a configuração.                                     |
|    **Valor**    | Valor do tempo limite (em segundos). Se o valor é em notação hexadecimal, em seguida, adicione o prefixo 0 x. |

---

**Exemplos**

Estes são dois exemplos do **adicionar o tempo limite** comando.

-   Adicionar o tempo limite timeouttype = valor idleconnectiontimeout = 120
-   add timeout timeouttype=headerwaittimeout value=0x40

---

## <a name="add-urlacl"></a>Adicionar urlacl

Adiciona uma entrada de reserva de URL Uniform Resource Locator (). Esse comando reserva a URL para contas e usuários não administradores. A DACL pode ser especificada usando um nome de conta do NT com os parâmetros de escuta e o delegado ou usando uma cadeia de caracteres SDDL.

**Sintaxe**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parâmetros**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Especifica a URL totalmente qualificada Uniform Resource Locator ().                                           | Obrigatório |
|   **user**   |                                                      Especifica o nome de usuário ou grupo de usuários                                                       | Obrigatório |
|  **listen**  | Especifica um dos seguintes valores: Sim: Permitir que o usuário registre URLs. Este é o valor padrão. Não: Nega ao usuário de registro de URLs. | Opcional |
| **delegate** |  Especifica um dos seguintes valores: Sim: Permitir ao usuário delegar URLs não: Nega ao usuário da delegação de URLs. Este é o valor padrão.  | Opcional |
|   **sddl**   |                                                Especifica uma cadeia de caracteres SDDL que descreve a DACL.                                                 | Opcional |

---

**Exemplos**

Estes são exemplos de quatro do **adicionar urlacl** comando.

- add urlacl url=https://+:80/MyUri user=DOMAIN\\user
- Adicionar url urlacl =<https://www.contoso.com:80/MyUri> usuário = DOMAIN\\escuta de usuário = Sim
- Adicionar url urlacl =<https://www.contoso.com:80/MyUri> usuário = DOMAIN\\delegado do usuário = não
- Adicionar url urlacl =https://+:80/MyUri sddl =...

---

## <a name="delete-cache"></a>Excluir cache

Exclui uma entrada especificada, ou todas as entradas de cache URI do kernel de serviço HTTP.

**Sintaxe**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parâmetros**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Especifica a URL totalmente qualificada Uniform Resource localizador () que você deseja excluir.                     | Opcional |
| **recursive** | Especifica se todas as entradas em cache de url são removidas. **Sim**: Remova todas as entradas **nenhuma**: não remover todas as entradas | Opcional |

---

**Exemplos**

Estes são dois exemplos do **excluir cache** comando.

- Excluir cache url =<https://www.contoso.com:80/myresource/> recursiva = Sim
- Excluir cache

---

## <a name="delete-iplisten"></a>Excluir iplisten

Exclui um endereço IP da lista de escuta de IP. A lista de escuta IP é usada para definir o escopo a lista de endereços para o qual associa o serviço HTTP.

**Sintaxe**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parâmetros**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | O endereço IPv4 ou IPv6 a ser excluído do IP de lista de escuta. A lista de escuta IP é usada para definir o escopo a lista de endereços para o qual associa o serviço HTTP. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6. Isso não inclui o número da porta. | Obrigatório |

---


**Exemplos**

Estes são exemplos de quatro do **excluir iplisten** comando.

-   delete iplisten ipaddress=fe80::1
-   Excluir iplisten ipaddress = 1.1.1.1
-   Excluir iplisten ipaddress = 0.0.0.0
-   delete iplisten ipaddress=::

---

## <a name="delete-sslcert"></a>Excluir sslcert


Exclui as associações de certificado de servidor SSL e políticas de certificado de cliente correspondentes para uma porta e endereço IP.

**Sintaxe**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parâmetros**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica o endereço IPv4 ou IPv6 e a porta para o qual as associações de certificado SSL são excluídas. Um caractere de dois-pontos (:) é usado como um delimitador entre o endereço IP e o número da porta. | Obrigatório |

---


**Exemplos**

Estes são os três exemplos do **excluir sslcert** comando.

- Excluir sslcert ipport = 1.1.1.1:443
- Excluir sslcert ipport = 0.0.0.0: 443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>excluir o tempo limite

Exclui um tempo limite global e faz com que o serviço de reverter para os valores padrão.

**Sintaxe**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Parâmetros**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | Especifica o tipo de configuração de tempo limite. | Obrigatório |

---


**Exemplos**

Estes são dois exemplos do **excluir o tempo limite** comando.

-   excluir o tempo limite timeouttype = idleconnectiontimeout
-   delete timeout timeouttype=headerwaittimeout

---

## <a name="delete-urlacl"></a>Excluir urlacl

Exclui as reservas de URL.

**Sintaxe**

```powershell
delete urlacl [ url= ] URL
```

**Parâmetros**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Especifica a URL totalmente qualificada Uniform Resource localizador () que você deseja excluir. | Obrigatório |

---


**Exemplos**

Estes são dois exemplos do **excluir urlacl** comando.

- Excluir urlacl url =https://+:80/MyUri
- Excluir urlacl url =<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>liberar logbuffer

Libera os buffers internos para os arquivos de log.

**Sintaxe**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>Mostrar cachestate

Armazenados em cache listas de recursos URI e suas propriedades associadas. Esse comando lista todos os recursos e suas propriedades associadas que são armazenados em cache no cache de resposta HTTP ou exibe um único recurso e suas propriedades associadas.

**Sintaxe**

```powershell
show cachestate [ [url= ] URL]
```

**Parâmetros**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica a URL totalmente qualificada que você deseja exibir. Se não for especificado, exiba todas as URLs. A URL também pode ser um prefixo para URLs registradas. | Opcional |

---


**Exemplos**

Estes são dois exemplos do **Mostrar cachestate** comando:

- Mostrar url cachestate =<https://www.contoso.com:80/myresource>
- Mostrar cachestate

---

## <a name="show-iplisten"></a>Mostrar iplisten

Exibe todos os endereços IP na lista de escuta de IP. A lista de escuta IP é usada para definir o escopo a lista de endereços para o qual associa o serviço HTTP. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6.

**Sintaxe**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>Mostrar o estado do serviço

Exibe um instantâneo do serviço HTTP.

**Sintaxe**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parâmetros**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Exibir**   | Especifica se deve exibir um instantâneo do estado do serviço HTTP com base em sessão do servidor ou em filas de solicitação. | Opcional |
| **Verbose** |                Especifica se deve exibir informações detalhadas que também mostra informações de propriedade.                | Opcional |

---

**Exemplos**

Estes são dois exemplos do **show servicestate** comando.

-   Mostrar exibição de estado do serviço = "session"
-   Mostrar exibição de estado do serviço = "requestq"

---

## <a name="show-sslcert"></a>Mostrar sslcert

Exibe as associações de certificado de servidor Secure Sockets Layer (SSL) e diretivas de certificado de cliente correspondentes para uma porta e endereço IP.

**Sintaxe**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parâmetros**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica o endereço IPv4 ou IPv6 e a porta para o qual o SSL exibir associações de certificado. Um caractere de dois-pontos (:) é usado como um delimitador entre o endereço IP e o número da porta. Se você não especificar ipport, todas as associações são exibidas. | Obrigatório |

---


**Exemplos**

Estes são exemplos de cinco da **Mostrar sslcert** comando.

-   show sslcert ipport=[fe80::1]:443
-   show sslcert ipport=1.1.1.1:443
-   Mostrar sslcert ipport = 0.0.0.0: 443
-   show sslcert ipport=[::]:443
-   Mostrar sslcert

---

## <a name="show-timeout"></a>Mostrar o tempo limite

Exibe, em segundos, os valores de tempo limite do serviço HTTP.

**Sintaxe**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>Mostrar urlacl

Controle de acesso discricionário exibe listas (DACLs) para a URL reservada especificada ou todas as URLs reservadas.

**Sintaxe**

```powershell
show urlacl [ [url= ] URL]
```

**Parâmetros**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica a URL totalmente qualificada que você deseja exibir. Se não especificado, exiba todas as URLs. | Opcional |

---


**Exemplos**

Estes são os três exemplos do **Mostrar urlacl** comando.

- show urlacl url=https://+:80/MyUri
- show urlacl url=<https://www.contoso.com:80/MyUri>
- Mostrar urlacl

---