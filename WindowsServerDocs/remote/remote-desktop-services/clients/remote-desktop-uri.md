---
title: Esquema de URI de Área de Trabalho Remota
description: Saiba mais sobre o esquema de Uniform Resource Identifier para clientes da Área de Trabalho Remota
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 06/01/2020
ms.localizationpriority: medium
ms.openlocfilehash: 082943114d00d9325e81edbd5cbeff14ba413703
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955083"
---
# <a name="remote-desktop-uri-scheme"></a>Esquema de URI de Área de Trabalho Remota

> Aplica-se a: Windows Server, versão 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Este documento define o formato de URIs (Uniform Resource Identifiers) para a Área de Trabalho Remota. Esses esquemas de URI permitem que os clientes da Área de Trabalho Remota sejam invocados com vários comandos.

## <a name="ms-rd-uri-scheme"></a>Esquema de URI ms-rd

>[!NOTE]
> No momento, o esquema de URI ms-rd somente é compatível com o cliente da Área de Trabalho do Windows (MSRDC).

O URI ms-rd fornece a opção de especificar um comando para o cliente e um conjunto de parâmetros específicos para o comando usando o seguinte formato:

```
ms-rd:command?parameters
```

Os parâmetros usam o formato de cadeia de caracteres de consulta do par chave-valor separado por & para fornecer informações adicionais para o comando dado:

```
param1=value1&param2=value2&…
```

### <a name="commands-and-parameters"></a>Comandos e parâmetros

Aqui está a lista de comandos atualmente com suporte e dos parâmetros correspondentes.

Usar `ms-rd:` sem nenhum comando inicia o cliente.

#### <a name="subscribe"></a>Subscribe

Esse comando inicia o cliente e o processo de assinatura.

**Nome do comando:** subscribe

**Parâmetros do comando:**

| Parâmetro | Descrição                  | Valores |
|-----------|------------------------------|--------|
| url       | Especifica a URL do workspace. | Uma URL válida, como <https://contoso.com>. |

**Exemplo:** ms-rd:subscribe?url=https://contoso.com

## <a name="legacy-rdp-uri-scheme"></a>Esquema de URI rdp herdado

>[!NOTE]
> O esquema de URI a seguir só é compatível com os clientes dos dispositivos macOS, iOS e Android. Ele está sendo substituído pelo novo URI ms-rd acima.

A Área de Trabalho Remota da Microsoft usa o esquema de URI rdp://query_string para armazenar configurações de atributos predefinidas que são usadas ao iniciar o cliente. As cadeias de caracteres de consulta representam um único ou um conjunto de atributos do RDP fornecidos na URL.

Os atributos RDP são separados pelo símbolo de e comercial (&). Por exemplo, ao se conectar a um computador, a cadeia de caracteres é:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Esta tabela fornece uma lista completa de atributos com suporte que podem ser usados com os clientes de Área de Trabalho Remota do iOS, Mac e Android. (Um "x" na coluna de plataforma indica que há suporte para o atributo. Os valores indicados por divisas (<>) representam os valores que são suportados pelos clientes de área de trabalho remota.)

| Atributo RDP                                           | Android | Mac | iOS |
|---------------------------------------------------------|---------|-----|-----|
| allow desktop composition=i:&lt;0 ou 1&gt;              | x       | x   | x   |
| allow font smoothing=i:<0 ou 1&gt;                      | x       | x   | x   |
| alternate shell=s:&lt;string&gt;                        | x       | x   | x   |
| [audiomode=i:&lt;0, 1, ou 2&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393707(v=ws.10)) | x       | x   | x   |
| [authentication level=i:&lt;0 ou 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393709(v=ws.10)) | x       | x   | x   |
| connect to console=i:&lt;0 or 1&gt;                     | x       | x   | x   |
| disable cursor settings=i:&lt;0 ou 1&gt;                | x       | x   | x   |
| disable full window drag=i:&lt;0 ou 1&gt;               | x       | x   | x   |
| disable menu anims=i:&lt;0 ou 1&gt;                     | x       | x   | x   |
| disable themes=i:&lt;0 ou 1&gt;                         | x       | x   | x   |
| disable wallpaper=i:&lt;0 ou 1&gt;                      | x       | x   | x   |
| [drivestoredirect=s:*](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393728(v=ws.10)) (esse é o único valor com suporte) | x       | x   |     |
| [desktopheight=i:&lt;valor em pixels&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393702(v=ws.10)) |         | x   |     |
| [desktopwidth=i:&lt;valor em pixels&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393697(v=ws.10))  |         | x   |     |
| [domain=s:&lt;string&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393673(v=ws.10))                 | x | x | x |
| [full address=s:&lt;string&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393661(v=ws.10))           | x | x | x |
| gatewayhostname=s:&lt;string&gt;                  | x | x | x |
| [gatewayusagemethod=i:&lt;1 ou 2&gt;](/windows/win32/termserv/imsrdpclienttransportsettings-gatewayusagemethod)                | x | x | x |
| [prompt for credentials on client=i:&lt;0 ou 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393660(v=ws.10)) |   | x |   |
| [loadbalanceinfo=s:&lt;string&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393684(v=ws.10))                  | x | x | x |
| [redirectprinters=i:&lt;0 ou 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393671(v=ws.10))                 |   | x |   |
| remoteapplicationcmdline=s:&lt;string&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 ou 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;string&gt;         | x | x | x |
| shell working directory=s:&lt;string&gt;          | x | x | x |
| Use redirection server name=i:&lt;0 ou 1&gt;      | x | x | x |
| [username=s:&lt;string&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393678(v=ws.10))                  | x | x | x |
| [screen mode id=i:&lt;1 ou 2&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393692(v=ws.10))            |   | x |   |
| [session bpp=i:&lt;8, 15, 16, 24 ou 32&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393680(v=ws.10)) |   | x |   |
| [use multimon=i:&lt;0 ou 1&gt;](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff393695(v=ws.10))              |   | x |   |
