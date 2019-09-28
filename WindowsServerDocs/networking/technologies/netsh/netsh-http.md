---
title: Comandos netsh para HTTP (Hypertext Transfer Protocol)
description: Use netsh http para consultar e definir configurações e parâmetros de HTTP. sys.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ''
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7b9032eb05128532c8bf90a0db2f685b4435e6eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401880"
---
# <a name="netsh-http-commands"></a>Comandos Netsh http


Use **netsh http** para consultar e definir configurações e parâmetros de http. sys.  

>[!TIP]
>Se você estiver usando o Windows PowerShell em um computador que executa o Windows Server 2016 ou o Windows 10, digite **netsh** e pressione Enter. No prompt do netsh, digite **http** e pressione ENTER para obter o prompt http do netsh.
>
>&nbsp; @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 @ no__t-5 @ no__t-6netsh http @ no__t-7

Os comandos netsh http disponíveis são:

- [Adicionar iplisten](#add-iplisten)
- [Adicionar sslcert](#add-sslcert)
- [Adicionar tempo limite](#add-timeout)
- [Adicionar urlacl](#add-urlacl)
- [Excluir cache](#delete-cache)
- [excluir iplisten](#delete-iplisten)
- [excluir sslcert](#delete-sslcert)
- [excluir tempo limite](#delete-timeout)
- [excluir urlacl](#delete-urlacl)
- [liberar logbuffer](#flush-logbuffer)
- [Mostrar CacheState](#show-cachestate)
- [Mostrar iplisten](#show-iplisten)
- [Mostrar ServiceState](#show-servicestate)
- [Mostrar sslcert](#show-sslcert)
- [Mostrar tempo limite](#show-timeout)
- [Mostrar urlacl](#show-urlacl)

## <a name="add-iplisten"></a>Adicionar iplisten

Adiciona um novo endereço IP à lista de escuta de IP, excluindo o número da porta.

**Sintaxe**

```powershell
add iplisten [ ipaddress= ] IPAddress
```

**Parâmetro**

|               |                                                                                                                                                                                                                          |          |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **IP** | O endereço IPv4 ou IPv6 a ser adicionado à lista de escuta de IP. A lista de escuta de IP é usada para delimitar a lista de endereços aos quais o serviço HTTP é associado. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6. | Obrigatório |

---

**Exemplos**

A seguir, há quatro exemplos do comando **Add iplisten** .

-   Adicionar iplisten IPAddress = FE80:: 1
-   Adicionar iplisten IPAddress = 1.1.1.1
-   Adicionar iplisten IPAddress = 0.0.0.0
-   Adicionar IPAddress iplisten =::

---

## <a name="add-sslcert"></a>Adicionar sslcert

Adiciona uma nova associação de certificado de servidor SSL e as políticas de certificado de cliente correspondentes para um endereço IP e uma porta.

**Sintaxe**

```powershell
add sslcert [ ipport= ] IPAddress:port [ certhash= ] CertHash [ appid= ] GUID [ [ certstorename= ] CertStoreName [ verifyclientcertrevocation= ] enable | disable [verifyrevocationwithcachedclientcertonly= ] enable | disable [ usagecheck= ] enable | disable [ revocationfreshnesstime= ] U-Int [ urlretrievaltimeout= ] U-Int [sslctlidentifier= ] SSLCTIdentifier [ sslctlstorename= ] SLCtStoreName [ dsmapperusage= ] enable | disable [ clientcertnegotiation= ] enable | disable ] ]
```

**Parâmetro**


|                                              |                                                                                                                                                                                          |          |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|                  **ipport**                  |                       Especifica o endereço IP e a porta para a associação. Um caractere de dois-pontos (:) é usado como um delimitador entre o endereço IP e o número da porta.                        | Obrigatório |
|                 **certhash**                 |                                     Especifica o hash de SHA do certificado. Esse hash tem 20 bytes de comprimento e é especificado como uma cadeia de caracteres hexadecimal.                                      | Obrigatório |
|                  **appid**                   |                                                                  Especifica o GUID para identificar o aplicativo proprietário.                                                                  | Obrigatório |
|              **certstorename**               |                                  Especifica o nome do repositório para o certificado. O padrão é MY. O certificado deve ser armazenado no contexto do computador local.                                  | Opcional |
|        **verifyclientcertrevocation**        |                                                      Especifica a verificação ativada/desativada de revogação de certificados de cliente.                                                       | Opcional |
| **verifyrevocationwithcachedclientcertonly** |                                      Especifica se o uso somente do certificado de cliente em cache para verificação de revogação está habilitado ou desabilitado.                                       | Opcional |
|                **usagecheck**                |                                                      Especifica se a verificação de uso está habilitada ou desabilitada. O padrão é habilitado.                                                       | Opcional |
|         **revocationfreshnesstime**          | Especifica o intervalo de tempo, em segundos, para verificar se há uma CRL (lista de certificados revogados) atualizada. Se esse valor for zero, a nova CRL será atualizada somente se a anterior expirar. | Opcional |
|           **urlretrievaltimeout**            |                            Especifica o intervalo de tempo limite (em milissegundos) após a tentativa de recuperar a lista de certificados revogados para a URL remota.                            | Opcional |
|             **sslctlidentifier**             |                Especifica a lista dos emissores de certificado que podem ser confiáveis. Essa lista pode ser um subconjunto dos emissores de certificado que são confiáveis para o computador.                 | Opcional |
|             **sslctlstorename**              |                                                Especifica o nome do repositório de certificados em LOCAL_MACHINE em que SslCtlIdentifier está armazenado.                                                | Opcional |
|              **dsmapperusage**               |                                                        Especifica se os Mapeadores DS estão habilitados ou desabilitados. O padrão é desabilitado.                                                         | Opcional |
|          **clientcertnegotiation**           |                                              Especifica se a negociação do certificado está habilitada ou desabilitada. O padrão é desabilitado.                                               | Opcional |

---

**Exemplos**

Veja a seguir um exemplo do comando **add sslcert** .

Adicionar SSLCERT IPPORT = 1.1.1.1:443 CERTHASH = 0102030405060708090A0B0C0D0E0F1011121314 AppID = {00112233-4455-6677-8899-AABBCCDDEEFF}

---

## <a name="add-timeout"></a>Adicionar tempo limite

Adiciona um tempo limite global ao serviço.

**Sintaxe** 

```powershell
add timeout [ timeouttype= ] IdleConnectionTimeout | HeaderWaitTimeout [ value=] U-Short
```

**Parâmetro**

|                 |                                                                                                     |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **timeouttype** |                                    Tipo de tempo limite para configuração.                                     |
|    **value**    | Valor do tempo limite (em segundos). Se o valor estiver em notação hexadecimal, adicione o prefixo 0x. |

---

**Exemplos**

A seguir, dois exemplos do comando **Adicionar tempo limite** .

-   Adicionar Timeout Timeout = valor de idleConnectionTimeout = 120
-   Adicionar tempo limite Timeout = HeaderWaitTimeout valor = 0x40

---

## <a name="add-urlacl"></a>Adicionar urlacl

Adiciona uma entrada de reserva de URL (localizador de recursos uniforme). Esse comando reserva a URL para contas e usuários não administradores. A DACL pode ser especificada usando um nome de conta do NT com os parâmetros Listen e delegate ou usando uma cadeia de caracteres SDDL.

**Sintaxe**

```powershell
add urlacl [ url= ] URL [ [user=] User [ [ listen= ] yes | no [ delegate= ] yes | no ] | [ sddl= ] SDDL ]
```

**Parâmetro**

|              |                                                                                                                                                  |          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------|----------|
|   **url**    |                                          Especifica o Uniform Resource Locator (URL) totalmente qualificado.                                           | Obrigatório |
|   **user**   |                                                      Especifica o nome do usuário ou do grupo de usuários                                                       | Obrigatório |
|  **ouvir**  | Especifica um dos seguintes valores: Sim: Permitir que o usuário registre URLs. Este é o valor padrão. Não: Negar que o usuário registre URLs. | Opcional |
| **delegá** |  Especifica um dos seguintes valores: Sim: Permitir que o usuário delegue URLs não: Negar o usuário de delegar URLs. Este é o valor padrão.  | Opcional |
|   **SDDL**   |                                                Especifica uma cadeia de caracteres SDDL que descreve a DACL.                                                 | Opcional |

---

**Exemplos**

A seguir, há quatro exemplos do comando **Add urlacl** .

- Adicionar URL urlacl = https://+:80/MyUri usuário = domínio @ no__t-1user
- Adicionar URL urlacl = <https://www.contoso.com:80/MyUri> usuário = domínio @ no__t-1user Listen = Sim
- Adicionar URL urlacl = <https://www.contoso.com:80/MyUri> usuário = domínio @ no__t-1user delegado = não
- Adicionar URL urlacl = https://+:80/MyUri SDDL =...

---

## <a name="delete-cache"></a>Excluir cache

Exclui todas as entradas ou uma entrada especificada do cache do URI do kernel do serviço HTTP.

**Sintaxe**

```powershell
delete cache [ [ url= ] URL [ [recursive= ] yes | no ]
```

**Parâmetro**

|               |                                                                                                                              |          |
|---------------|------------------------------------------------------------------------------------------------------------------------------|----------|
|    **url**    |                    Especifica o Uniform Resource Locator (URL) totalmente qualificado que você deseja excluir.                     | Opcional |
| **recursiva** | Especifica se todas as entradas no cache de URL são removidas. **Sim**: remover todas as entradas **não**: não remover todas as entradas | Opcional |

---

**Exemplos**

A seguir, dois exemplos do comando **Excluir cache** .

- excluir URL do cache = <https://www.contoso.com:80/myresource/> recursivo = Sim
- Excluir cache

---

## <a name="delete-iplisten"></a>excluir iplisten

Exclui um endereço IP da lista de escuta de IP. A lista de escuta de IP é usada para delimitar a lista de endereços aos quais o serviço HTTP é associado.

**Sintaxe**

```powershell
delete iplisten [ ipaddress= ] IPAddress
```

**Parâmetro**

|               |                                                                                                                                                                                                                                                                     |          |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **IP** | O endereço IPv4 ou IPv6 a ser excluído da lista de escuta de IP. A lista de escuta de IP é usada para delimitar a lista de endereços aos quais o serviço HTTP é associado. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6. Isso não inclui o número da porta. | Obrigatório |

---


**Exemplos**

A seguir, há quatro exemplos do comando **delete iplisten** .

-   excluir iplisten IPAddress = FE80:: 1
-   excluir iplisten IPAddress = 1.1.1.1
-   excluir iplisten IPAddress = 0.0.0.0
-   excluir iplisten IPAddress =::

---

## <a name="delete-sslcert"></a>excluir sslcert


Exclui as associações de certificado do servidor SSL e as políticas de certificado do cliente correspondentes para um endereço IP e uma porta.

**Sintaxe**

```powershell
delete sslcert [ ipport= ] IPAddress:port
```

**Parâmetro**

|            |                                                                                                                                                                                          |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica o endereço IPv4 ou IPv6 e a porta para a qual as associações de certificado SSL são excluídas. Um caractere de dois-pontos (:) é usado como um delimitador entre o endereço IP e o número da porta. | Obrigatório |

---


**Exemplos**

Veja a seguir três exemplos do comando **delete sslcert** .

- excluir SSLCERT IPPORT = 1.1.1.1:443
- excluir SSLCERT IPPORT = 0.0.0.0:443
- excluir SSLCERT IPPORT = [::]: 443

---

## <a name="delete-timeout"></a>excluir tempo limite

Exclui um tempo limite global e faz com que o serviço reverta para os valores padrão.

**Sintaxe**

```powershell
delete timeout [ timeouttype= ] idleconnectiontimeout | headerwaittimeout
```

**Parâmetro**

|                 |                                        |          |
|-----------------|----------------------------------------|----------|
| **timeouttype** | Especifica o tipo de configuração de tempo limite. | Obrigatório |

---


**Exemplos**

A seguir, dois exemplos do comando **excluir tempo limite** .

-   excluir Timeout = idleConnectionTimeout
-   excluir Timeout = HeaderWaitTimeout

---

## <a name="delete-urlacl"></a>excluir urlacl

Exclui as reservas de URL.

**Sintaxe**

```powershell
delete urlacl [ url= ] URL
```

**Parâmetro**

|         |                                                                                       |          |
|---------|---------------------------------------------------------------------------------------|----------|
| **url** | Especifica o Uniform Resource Locator (URL) totalmente qualificado que você deseja excluir. | Obrigatório |

---


**Exemplos**

A seguir, dois exemplos do comando **delete urlacl** .

- excluir URL urlacl = https://+:80/MyUri
- excluir URL urlacl = <https://www.contoso.com:80/MyUri>

---

## <a name="flush-logbuffer"></a>liberar logbuffer

Libera os buffers internos para os arquivos de log.

**Sintaxe**

```powershell
flush logbuffer
```

---

## <a name="show-cachestate"></a>Mostrar CacheState

Lista os recursos de URI armazenados em cache e suas propriedades associadas. Esse comando lista todos os recursos e suas propriedades associadas que são armazenadas em cache no cache de resposta HTTP ou exibe um único recurso e suas propriedades associadas.

**Sintaxe**

```powershell
show cachestate [ [url= ] URL]
```

**Parâmetro**

|         |                                                                                                                                                    |          |
|---------|----------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica a URL totalmente qualificada que você deseja exibir. Se não for especificado, exiba todas as URLs. A URL também pode ser um prefixo para URLs registradas. | Opcional |

---


**Exemplos**

A seguir, dois exemplos do comando **show CacheState** :

- Mostrar URL do CacheState = <https://www.contoso.com:80/myresource>
- Mostrar CacheState

---

## <a name="show-iplisten"></a>Mostrar iplisten

Exibe todos os endereços IP na lista de escuta de IP. A lista de escuta de IP é usada para delimitar a lista de endereços aos quais o serviço HTTP é associado. "0.0.0.0" significa qualquer endereço IPv4 e "::" significa qualquer endereço IPv6.

**Sintaxe**

```powershell
show iplisten
```

---

## <a name="show-servicestate"></a>Mostrar ServiceState

Exibe um instantâneo do serviço HTTP.

**Sintaxe**
```powershell
show servicestate [ [ view= ] session | requestq ] [ [ verbose= ] yes | no ]
```

**Parâmetro**

|             |                                                                                                                      |          |
|-------------|----------------------------------------------------------------------------------------------------------------------|----------|
|  **Exibir**   | Especifica se é para exibir um instantâneo do estado do serviço HTTP com base na sessão do servidor ou nas filas de solicitação. | Opcional |
| **Extensa** |                Especifica se é para exibir informações detalhadas que também mostram informações de propriedade.                | Opcional |

---

**Exemplos**

A seguir, dois exemplos do comando **show ServiceState** .

-   Mostrar exibição de ServiceState = "sessão"
-   Mostrar exibição de ServiceState = "requestq"

---

## <a name="show-sslcert"></a>Mostrar sslcert

Exibe as associações de certificado do servidor protocolo SSL (SSL) e as políticas de certificado do cliente correspondentes para um endereço IP e uma porta.

**Sintaxe**

```powershell
show sslcert [ ipport= ] IPAddress:port
```

**Parâmetro**

|            |                                                                                                                                                                                                                                                |          |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| **ipport** | Especifica o endereço IPv4 ou IPv6 e a porta para a qual as associações de certificado SSL são exibidas. Um caractere de dois-pontos (:) é usado como um delimitador entre o endereço IP e o número da porta. Se você não especificar ipport, todas as associações serão exibidas. | Obrigatório |

---


**Exemplos**

A seguir estão cinco exemplos do comando **show sslcert** .

-   Mostrar SSLCERT IPPORT = [FE80:: 1]: 443
-   Mostrar SSLCERT IPPORT = 1.1.1.1:443
-   Mostrar SSLCERT IPPORT = 0.0.0.0:443
-   Mostrar SSLCERT IPPORT = [::]: 443
-   Mostrar sslcert

---

## <a name="show-timeout"></a>Mostrar tempo limite

Exibe, em segundos, os valores de tempo limite do serviço HTTP.

**Sintaxe**

```powershell
show timeout
```

---

## <a name="show-urlacl"></a>Mostrar urlacl

Exibe listas de controle de acesso discricional (DACLs) para a URL reservada especificada ou todas as URLs reservadas.

**Sintaxe**

```powershell
show urlacl [ [url= ] URL]
```

**Parâmetro**

|         |                                                                                                |          |
|---------|------------------------------------------------------------------------------------------------|----------|
| **url** | Especifica a URL totalmente qualificada que você deseja exibir. Se não for especificado, exiba todas as URLs. | Opcional |

---


**Exemplos**

Veja a seguir três exemplos do comando **show urlacl** .

- Mostrar URL urlacl = https://+:80/MyUri
- Mostrar URL urlacl = <https://www.contoso.com:80/MyUri>
- Mostrar urlacl

---