---
title: Configurando o Kerberos para o endereço IP
description: Suporte a Kerberos para SPNs baseados em IP
author: daveba
ms.author: daveba
ms.openlocfilehash: 1061364528100fe005e80f64c6315f9fca69ad98
ms.sourcegitcommit: 2082335e1260826fcbc3dccc208870d2d9be9306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69980294"
---
# <a name="kerberos-clients-allow-ipv4-and-ipv6-address-hostnames-in-service-principal-names-spns"></a>Os clientes Kerberos permitem nomes de host de endereço IPv4 e IPv6 em SPNs (nome da entidade de serviço)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A partir do Windows 10 versão 1507 e do Windows Server 2016, os clientes Kerberos podem ser configurados para dar suporte a nomes de host IPv4 e IPv6 em SPNs.

Por padrão, o Windows não tentará a autenticação Kerberos para um host se o nome do host for um endereço IP. Ele voltará para outros protocolos de autenticação habilitados, como o NTLM. No entanto, os aplicativos, às vezes, são codificados para usar endereços IP, o que significa que o aplicativo voltará ao NTLM e não usará o Kerberos. Isso pode causar problemas de compatibilidade à medida que os ambientes se movem para desabilitar o NTLM.

Para reduzir o impacto de desabilitar o NTLM, foi introduzida uma nova funcionalidade que permite que os administradores usem endereços IP como nomes de host em nome da entidade de serviço. Esse recurso é habilitado no cliente por meio de um valor de chave do registro.

```cmd
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\Kerberos\Parameters" /v TryIPSPN /t REG_DWORD /d 1 /f
```

Para configurar o suporte para nomes de host de endereço IP em SPNs, crie uma entrada TryIPSPN. Essa entrada não existe no Registro por padrão. Depois de criar a entrada, altere o valor DWORD para 1. Esse valor de registro precisará ser definido em cada computador cliente que precisa acessar recursos protegidos por Kerberos por endereço IP.

## <a name="configuring-a-service-principal-name-as-ip-address"></a>Configurando um nome da entidade de serviço como endereço IP

Um nome da entidade de serviço é um identificador exclusivo usado durante a autenticação Kerberos para identificar um serviço na rede. Um SPN é composto por um serviço, um nome de host e, opcionalmente, uma `service/hostname[:port]` porta na `host/fs.contoso.com`forma de tal como. O Windows registrará vários SPNs em um objeto de computador quando um computador for ingressado em Active Directory.

Os endereços IP normalmente não são usados no lugar de nomes de host porque os endereços IP geralmente são temporários. Isso pode levar a conflitos e falhas de autenticação, pois as concessões de endereço expiram e são renovadas. Portanto, o registro de um SPN baseado em endereço IP é um processo manual e só deve ser usado quando for impossível alternar para um nome de host baseado em DNS.

A abordagem recomendada é usar a ferramenta [SetSPN. exe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) . Observe que um SPN só pode ser registrado para uma única conta no Active Directory de cada vez, portanto, é recomendável que os endereços IP tenham concessões estáticas se o DHCP for usado.

```
Setspn -s <service>/ip.address> <domain-user-account>  
```

Exemplo:

```
Setspn -s host/192.168.1.1 server01
```
