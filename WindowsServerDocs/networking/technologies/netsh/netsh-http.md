---
title: Comandos Netsh para HTTP (Hypertext Transfer Protocol)
description: Use netsh http para consultar e definir configurações e parâmetros de HTTP.sys.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401880"
---
# <a name="netsh-http-commands"></a>Comandos Netsh http


Use **netsh http** para consultar e definir configurações e parâmetros de HTTP.sys.  

>[!TIP]
>Se você estiver usando o Windows PowerShell em um computador que executa o Windows Server 2016 ou o Windows 10, insira **netsh** e pressione Enter. No prompt netsh, digite **http** e pressione Enter para obter o prompt netsh http.
>
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;netsh http\>

Os comandos netsh http disponíveis são:

- [add iplisten](#add-iplisten)
- [add sslcert](#add-sslcert)
- [add timeout](#add-timeout)
- [add urlacl](#add-urlacl)
- [delete cache](#delete-cache)
- [delete iplisten](#delete-iplisten)
- [delete sslcert](#delete-sslcert)
- [delete timeout](#delete-timeout)
- [delete urlacl](#delete-urlacl)
- [flush logbuffer](#flush-logbuffer)
- [show cachestate](#show-cachestate)
- [show iplisten](#show-iplisten)
- [show servicestate](#show-servicestate)
- [show sslcert](#show-sslcert)
- [show timeout](#show-timeout)
- [show urlacl](#show-urlacl)

## <a name="add-iplisten"></a>add iplisten

Adiciona um novo endereço IP à lista de escuta de IP, excluindo o número da porta.

**Sintaxe**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parâmetros**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | O endereço IPv4 ou IPv6 a ser adicionado à lista de escuta de IP. A lista de escuta de IP é usada para definir o escopo da lista de endereços aos quais o serviço HTTP associa-se. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6. | Necessária |

---

**Exemplos**

A seguir, estão quatro exemplos do comando **add iplisten**.

-   add iplisten ipaddress=fe80::1
-   add iplisten ipaddress=1.1.1.1
-   add iplisten ipaddress=0.0.0.0
-   add iplisten ipaddress=::

---

## <a name="add-sslcert"></a>add sslcert

Adiciona uma nova associação de certificado do servidor SSL e as políticas de certificado de cliente correspondentes a um endereço IP e uma porta.

**Sintaxe**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parâmetros**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Especifica o endereço IP e a porta para a associação. Um caractere de dois-pontos (:) é usado como delimitador entre o endereço IP e o número da porta.                        | Necessária |
|                 **certhash**                 |                                     Especifica o hash de SHA do certificado. Esse hash tem 20 bytes e é especificado como uma cadeia de caracteres hexadecimal.                                      | Necessária |
|                  **appid**                   |                                                                  Especifica o GUID para identificar o aplicativo proprietário.                                                                  | Necessária |
|              **certstorename**               |                                  Especifica o nome do repositório para o certificado. O padrão é MY. O certificado deve ser armazenado no contexto do computador local.                                  | Opcional |
|        **verifyclientcertrevocation**        |                                                      Especifica a verificação de Ativada/desativada de revogação de certificados de cliente.                                                       | Opcional |
| **verifyrevocationwithcachedclientcertonly** |                                      Especifica se o uso somente do certificado de cliente em cache para verificação de revogação está habilitado ou desabilitado.                                       | Opcional |
|                **usagecheck**                |                                                      Especifica se a verificação de uso está habilitada ou desabilitada. O padrão é habilitada.                                                       | Opcional |
|         **revocationfreshnesstime**          | Especifica o intervalo de tempo, em segundos, para verificar se há uma CRL (lista de certificados revogados) atualizada. Se esse valor for zero, a nova CRL será atualizada somente se a anterior expirar. | Opcional |
|           **urlretrievaltimeout**            |                            Especifica o intervalo de tempo limite (em milissegundos) após a tentativa de recuperar a lista de certificados revogados para a URL remota.                            | Opcional |
|             **sslctlidentifier**             |                Especifica a lista dos emissores de certificado que podem ser confiáveis. Essa lista pode ser um subconjunto dos emissores de certificado que são confiáveis para o computador.                 | Opcional |
|             **sslctlstorename**              |                                                Especifica o nome do repositório de certificados em LOCAL_MACHINE em que SslCtlIdentifier é armazenado.                                                | Opcional |
|              **dsmapperusage**               |                                                        Especifica se os mapeadores do DS estão habilitados ou desabilitados. O padrão é desabilitado.                                                         | Opcional |
|          **clientcertnegotiation**           |                                              Especifica se a negociação do certificado está habilitada ou desabilitada. O padrão é desabilitado.                                               | Opcional |

---

**Exemplos**

A seguir, está um exemplo do comando **add sslcert**.

add sslcert ipport=1.1.1.1:443 certhash=0102030405060708090A0B0C0D0E0F1011121314 appid={00112233-4455-6677-8899- AABBCCDDEEFF}

---

## <a name="add-timeout"></a>add timeout

Adiciona um tempo limite global ao serviço.

**Sintaxe** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parâmetros**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Tipo de tempo limite para a configuração.                                     |
|    **value**    | Valor do tempo limite (em segundos). Se o valor estiver em notação hexadecimal, adicione o prefixo 0x. |

---

**Exemplos**

A seguir, estão dois exemplos do comando **add timeout**.

-   add timeout timeouttype=idleconnectiontimeout value=120
-   add timeout timeouttype=headerwaittimeout value=0x40

---

## <a name="add-urlacl"></a>add urlacl

Adiciona uma entrada de reserva de URL (Uniform Resource Locator). Esse comando reserva a URL para contas e usuários não administradores. A DACL pode ser especificada usando um nome de conta do NT com os parâmetros listen e delegate ou usando uma cadeia de caracteres SDDL.

**Sintaxe**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parâmetros**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Especifica a URL (Uniform Resource Locator) totalmente qualificada.                                           | Necessária |
|   **user**   |                                                      Especifica o nome do usuário ou do grupo de usuários                                                       | Necessária |
|  **listen**  | Especifica um dos seguintes valores: sim: permitir que o usuário registre URLs. Este é o valor padrão. não: negar que o usuário registre URLs. | Opcional |
| **delegate** |  Especifica um dos seguintes valores: sim: permitir que o usuário delegue URLs não: negar que o usuário delegue URLs. Este é o valor padrão.  | Opcional |
|   **sddl**   |                                                Especifica uma cadeia de caracteres SDDL que descreve a DACL.                                                 | Opcional |

---

**Exemplos**

A seguir, estão quatro exemplos do comando **add urlacl**.

- add urlacl url=https://+:80/MyUri user=DOMAIN\\ user
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user listen=yes
- add urlacl url=<https://www.contoso.com:80/MyUri> user=DOMAIN\\user delegate=no
- add urlacl url=https://+:80/MyUri sddl=...

---

## <a name="delete-cache"></a>delete cache

Exclui todas as entradas ou uma entrada especificada do cache do URI do kernel do serviço HTTP.

**Sintaxe**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parâmetros**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Especifica a URL (Uniform Resource Locator) totalmente qualificada que você deseja excluir.                     | Opcional |
| **recursive** | Especifica se todas as entradas no cache de URL são removidas. **Sim**: remover todas as entradas **não**: não remover todas as entradas | Opcional |

---

**Exemplos**

A seguir, estão dois exemplos do comando **delete cache**.

- delete cache url=<https://www.contoso.com:80/myresource/> recursive=yes
- delete cache

---

## <a name="delete-iplisten"></a>delete iplisten

Exclui um endereço IP da lista de escuta de IP. A lista de escuta de IP é usada para definir o escopo da lista de endereços aos quais o serviço HTTP associa-se.

**Sintaxe**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parâmetros**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipaddress** | O endereço IPv4 ou IPv6 a ser excluído da lista de escuta de IP. A lista de escuta de IP é usada para definir o escopo da lista de endereços aos quais o serviço HTTP associa-se. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6. Isso não inclui o número da porta. | Necessária |

---


**Exemplos**

A seguir, estão quatro exemplos do comando **delete iplisten**.

-   delete iplisten ipaddress=fe80::1
-   delete iplisten ipaddress=1.1.1.1
-   delete iplisten ipaddress=0.0.0.0
-   delete iplisten ipaddress=::

---

## <a name="delete-sslcert"></a>delete sslcert


Excluir as associações de certificado do servidor SSL e as políticas de certificado de cliente correspondentes a um endereço IP e uma porta.

**Sintaxe**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parâmetros**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica o endereço IPv4 ou IPv6 e a porta para a qual as associações de certificado SSL são excluídas. Um caractere de dois-pontos (:) é usado como delimitador entre o endereço IP e o número da porta. | Necessária |

---


**Exemplos**

A seguir, estão três exemplos do comando **delete sslcert**.

- delete sslcert ipport=1.1.1.1:443
- delete sslcert ipport=0.0.0.0:443
- delete sslcert ipport=[::]:443

---

## <a name="delete-timeout"></a>delete timeout

Exclui um tempo limite global e faz com que o serviço reverta para os valores padrão.

**Sintaxe**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Parâmetros**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | Especifica o tipo de configuração de tempo limite. | Necessária |

---


**Exemplos**

A seguir, estão dois exemplos do comando **delete timeout**.

-   delete timeout timeouttype=idleconnectiontimeout
-   delete timeout timeouttype=headerwaittimeout

---

## <a name="delete-urlacl"></a>delete urlacl

Exclui as reservas de URL.

**Sintaxe**

```powershell
delete urlacl [ url= ] URL
```

**Parâmetros**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Especifica a URL (Uniform Resource Locator) totalmente qualificada que você deseja excluir. | Necessária |

---


**Exemplos**

A seguir, estão dois exemplos do comando **delete urlacl**.

- delete urlacl url=https://+:80/MyUri
- delete urlacl url=<https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>flush logbuffer

Libera os buffers internos para os arquivos de log.

**Sintaxe**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>show cachestate

Lista os recursos de URI armazenados em cache e suas propriedades associadas. Esse comando lista todos os recursos e suas propriedades associadas que são armazenadas em cache no cache de resposta HTTP ou exibe um recurso e suas propriedades associadas.

**Sintaxe**

```powershell
show cachestate [ [url= ] URL]
```

**Parâmetros**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica a URL totalmente qualificada que você deseja exibir. Se não for especificado, todas as URLs serão exibidas. A URL também pode ser um prefixo para URLs registradas. | Opcional |

---


**Exemplos**

Veja abaixo dois exemplos do comando **show cachestate**:

- show cachestate url=<https://www.contoso.com:80/myresource>
- show cachestate

---

## <a name="show-iplisten"></a>show iplisten

Exibe todos os endereços IP na lista de escuta de IP. A lista de escuta de IP é usada para definir o escopo da lista de endereços aos quais o serviço HTTP associa-se. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6.

**Sintaxe**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>show servicestate

Exibe um instantâneo do serviço HTTP.

**Sintaxe**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parâmetros**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Exibir**   | Especifica se deve ser exibido um instantâneo do estado do serviço HTTP com base na sessão do servidor ou nas filas de solicitação. | Opcional |
| **Verbose** |                Especifica se é devem ser exibidas informações detalhadas que também mostram informações de propriedade.                | Opcional |

---

**Exemplos**

A seguir, estão dois exemplos do comando **show servicestate**.

-   show servicestate view="session"
-   show servicestate view="requestq"

---

## <a name="show-sslcert"></a>show sslcert

Exibe associações de certificado do servidor de protocolo SSL e as políticas de certificado de cliente correspondentes a um endereço IP e uma porta.

**Sintaxe**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parâmetros**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica o endereço IPv4 ou IPv6 e a porta para a qual as associações de certificado SSL são exibidas. Um caractere de dois-pontos (:) é usado como delimitador entre o endereço IP e o número da porta. Se você não especificar ipport, todas as associações serão exibidas. | Necessária |

---


**Exemplos**

A seguir, estão cinco exemplos do comando **show sslcert**.

-   show sslcert ipport=[fe80::1]:443
-   show sslcert ipport=1.1.1.1:443
-   show sslcert ipport=0.0.0.0:443
-   show sslcert ipport=[::]:443
-   show sslcert

---

## <a name="show-timeout"></a>show timeout

Exibe, em segundos, os valores de tempo limite do serviço HTTP.

**Sintaxe**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>show urlacl

Exibe DACLs (listas de controle de acesso discricional) para a URL reservada especificada ou todas as URLs reservadas.

**Sintaxe**

```powershell
show urlacl [ [url= ] URL]
```

**Parâmetros**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica a URL totalmente qualificada que você deseja exibir. Se não for especificado, todas as URLs serão exibidas. | Opcional |

---


**Exemplos**

A seguir, estão três exemplos do comando **show urlacl**.

- show urlacl url=https://+:80/MyUri
- show urlacl url=<https://www.contoso.com:80/MyUri>
- show urlacl

---