---
title: Configurando o Kerberos para o endereço IP
description: Suporte a Kerberos para SPNs baseado em IP
ms.openlocfilehash: aa2685fcff2fdf231e5e5884d25885585f0bd6c9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67279966"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Os clientes do Kerberos permitem nomes de host de endereço de IPv4 e IPv6 em nomes de entidade de serviço (SPNs)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Começando com o Windows 10 versão 1507 e Windows Server 2016, clientes Kerberos podem ser configurados para dar suporte a nomes de host de IPv4 e IPv6 no SPNs.

Por padrão o Windows não tentará a autenticação Kerberos para um host se o nome do host é um endereço IP. Ele fará o fallback para outros protocolos de autenticação habilitadas, como NTLM. No entanto, aplicativos, às vezes, são codificados para usar endereços IP que significa que o aplicativo serão revertida para NTLM e não usar o Kerberos. Isso pode causar problemas de compatibilidade, ambientes de mudar desabilitar o NTLM.

Para reduzir o impacto da desabilitação NTLM uma nova funcionalidade foi introduzida que permite aos administradores a usar endereços IP como nomes de host em nomes de entidade de serviço. Essa funcionalidade é habilitada no cliente por meio de um valor de chave do registro.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

Para configurar o suporte para nomes de host de endereço IP no SPNs, crie uma entrada de TryIPSPN. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. Esse valor de registro precisará ser definida em cada computador cliente que precisa acessar recursos protegidos por Kerberos pelo endereço IP.

## <a name="configuring-a-service-principal-name-as-ip-address"></a>Configurando um nome de entidade de serviço como endereço IP

Um nome de entidade de serviço é um identificador exclusivo usado durante a autenticação Kerberos para identificar um serviço na rede. Um SPN é composto de um serviço, o nome do host e, opcionalmente, uma porta na forma de `service/hostname[:port]` como `host/fs.contoso.com`. Windows registrar vários SPNs para um objeto de computador quando um computador é adicionado ao Active Directory.

Endereços IP não são normalmente usados no lugar de nomes de host como endereços IP costumam ser temporários. Isso pode levar a conflitos e falhas de autenticação, como as concessões de endereço expiraram e renovar. Portanto, registrar um SPN com base no endereço IP é um processo manual e só deve ser usado quando não é possível alternar para um nome de host com base em DNS.

A abordagem recomendada é usar o [Setspn.exe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) ferramenta. Observe que um SPN só pode ser registrado para uma única conta no Active Directory por vez portanto é recomendável que os endereços IP têm concessões estáticas se o DHCP é usado.

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

Exemplo:

```
Setspn -s host/192.168.1.1 server01
```
