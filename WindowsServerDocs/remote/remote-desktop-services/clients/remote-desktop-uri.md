---
title: Esquema de URI de clientes da Área de Trabalho Remota
description: Saiba mais sobre o esquema de Uniform Resource Identifier para clientes da Área de Trabalho Remota
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/11/2018
ms.localizationpriority: medium
ms.openlocfilehash: 02f970cb2e793c1e342a2818a2bca3900327fa9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855999"
---
# <a name="remote-desktop-client-universal-resource-identifier-uri-scheme-support"></a>Suporte ao esquema de URI (Identificador de Recurso Universal) do cliente da Área de Trabalho Remota

>Aplica-se a: Windows Server, versão 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

A habilitação de um esquema URI (Uniform Resource Identifier) concede aos profissionais de TI e desenvolvedores uma maneira para integrar os recursos dos clientes de Área de Trabalho Remota entre plataformas e enriquece a experiência do usuário ao permitir que: 

- Aplicativos de terceiros iniciem a Área de Trabalho Remota da Microsoft e iniciem sessões remotas com configurações predefinidas (fornecidas como parte da cadeia de caracteres do URI).
- Usuários finais iniciem conexões remotas usando URLs (Uniform Resource Locators) pré-configuradas.

>[!NOTE]
> NÃO há suporte para uso de um URI para conectar ao cliente da Área de Trabalho Remota nos sistemas operacionais Windows - use o URI com dispositivos MacOS, iOS e Android.

A Área de Trabalho Remota da Microsoft usa o esquema de URI rdp://query_string para armazenar configurações de atributos predefinidas que são usadas ao iniciar o cliente. As cadeias de caracteres de consulta representam um único ou um conjunto de atributos do RDP fornecidos na URL. 

Os atributos RDP são separados pelo símbolo de e comercial (&). Por exemplo, ao se conectar a um computador, a cadeia de caracteres é:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Esta tabela fornece uma lista completa de atributos com suporte que podem ser usados com os clientes de Área de Trabalho Remota do iOS, Mac e Android. (Um "x" na coluna de plataforma indica que há suporte para o atributo. Os valores indicados por divisas (<>) representam os valores que são suportados pelos clientes de área de trabalho remota.)

| **Atributo RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| allow desktop composition=i:&lt;0 ou 1&gt;                    | x       | x   | x   |
| allow font smoothing=i:<0 ou 1&gt;                         | x       | x   | x   |
| alternate shell=s:&lt;string&gt;                              | x       | x   | x   |
| [audiomode=i:&lt;0, 1, ou 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [authentication level=i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| connect to console=i:&lt;0 or 1&gt;                           | x       | x   | x   |
| disable cursor settings=i:&lt;0 ou 1&gt;                      | x       | x   | x   |
| disable full window drag=i:&lt;0 ou 1&gt;                     | x       | x   | x   |
| disable menu anims=i:&lt;0 ou 1&gt;                           | x       | x   | x   |
| disable themes=i:&lt;0 ou 1&gt;                               | x       | x   | x   |
| disable wallpaper=i:&lt;0 ou 1&gt;                            | x       | x   | x   |
| [drivestoredirect=s:*](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (esse é o único valor com suporte) | x       | x   |     |
| [desktopheight=i:&lt;valor em pixels&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth=i:&lt;valor em pixels&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [domain=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [full address=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname=s:&lt;string&gt;                  | x | x | x |
| [gatewayusagemethod=i:&lt;1 ou 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [prompt for credentials on client=i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters=i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline=s:&lt;string&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 ou 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;string&gt;         | x | x | x |
| shell working directory=s:&lt;string&gt;          | x | x | x |
| Use redirection server name=i:&lt;0 ou 1&gt;      | x | x | x |
| [username=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [screen mode id=i:&lt;1 ou 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [session bpp=i:&lt;8, 15, 16, 24 ou 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [use multimon=i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |