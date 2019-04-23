---
title: Esquema de URI de clientes de área de trabalho remota
description: Saiba mais sobre o esquema de Uniform Resource Identifier para clientes de área de trabalho remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/11/2018
ms.localizationpriority: medium
ms.openlocfilehash: 86de6468e2fa45c976711aef43a1a274e04498d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870497"
---
# <a name="remote-desktop-client-universal-resource-identifier-uri-scheme-support"></a>Suporte a cliente de área de trabalho remota esquema de identificador de recurso Universal (URI)

>Aplica-se a: Windows Server, versão 1803, Windows Server 2016, Windows Server 2012 R2

A habilitação de um esquema URI (Uniform Resource Identifier) concede aos profissionais de TI e desenvolvedores uma maneira para integrar os recursos dos clientes de Área de Trabalho Remota entre plataformas e enriquece a experiência do usuário ao permitir que: 

- Aplicativos de terceiros iniciem a Área de Trabalho Remota da Microsoft e iniciem sessões remotas com configurações predefinidas (fornecidas como parte da cadeia de caracteres do URI).
- Usuários finais iniciem conexões remotas usando URLs (Uniform Resource Locators) pré-configuradas.

>[!NOTE]
> Não há suporte para uso de um URI para conectar-se para o cliente de área de trabalho remota para os sistemas operacionais Windows - use o URI com dispositivos Android, iOS e MacOS.

A área de trabalho remota Microsoft usa o esquema URI RDP: //QUERY_STRING para armazenar configurações de atributos pré-configurados que são usadas ao iniciar o cliente. As cadeias de caracteres de consulta representam um único ou um conjunto de atributos do RDP fornecidos na URL. 

Os atributos RDP são separados pelo símbolo de e comercial (&). Por exemplo, ao se conectar a um computador, a cadeia de caracteres é:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Esta tabela fornece uma lista completa de atributos com suporte que podem ser usados com os clientes de Área de Trabalho Remota do iOS, Mac e Android. (Um "x" na coluna de plataforma indica que há suporte para o atributo. Os valores indicados por divisas (<>) representam os valores que são suportados pelos clientes de área de trabalho remota.)

| **Atributo do RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| Permitir composição de área de trabalho = i:&lt;0 ou 1&gt;                    | x       | x   | x   |
| Permitir suavização de fonte = i: < 0 ou 1&gt;                         | x       | x   | x   |
| alternativa shell = s:&lt;cadeia de caracteres&gt;                              | x       | x   | x   |
| [audiomode=i:0 = i:&lt;0, 1 ou 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [nível de autenticação = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| conectar-se ao console = i:&lt;0 ou 1&gt;                           | x       | x   | x   |
| desabilitar as configurações de cursor = i:&lt;0 ou 1&gt;                      | x       | x   | x   |
| Disable full window Drag=i:&lt;0 = i:&lt;0 ou 1&gt;                     | x       | x   | x   |
| disable menu anims=i:&lt;0 = i:&lt;0 ou 1&gt;                           | x       | x   | x   |
| Disable Themes=i:&lt;0 = i:&lt;0 ou 1&gt;                               | x       | x   | x   |
| desabilitar o papel de parede = i:&lt;0 ou 1&gt;                            | x       | x   | x   |
| [drivestoredirect=s:*](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (esse é o único valor com suporte) | x       | x   |     |
| [desktopheight = i:&lt;valor em pixels&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;valor em pixels&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [domain=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [endereço completo = s:&lt;cadeia de caracteres&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname=s:&lt;string&gt;                  | x | x | x |
| [gatewayusagemethod=i:1 = i:&lt;1 ou 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [prompt para credenciais no cliente = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters=i:&lt;0 or 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline=s:&lt;string&gt;         | x | x | x |
| remoteapplicationmode=i:&lt;0 = i:&lt;0 ou 1&gt;            | x | x | x |
| remoteapplicationprogram=s:&lt;string&gt;         | x | x | x |
| diretório de trabalho do shell = s:&lt;cadeia de caracteres&gt;          | x | x | x |
| Nome do servidor de redirecionamento de uso = i:&lt;0 ou 1&gt;      | x | x | x |
| [username=s:&lt;string&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [id do modo de tela = i:&lt;1 ou 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [sessão bpp = i:&lt;8, 15, 16, 24 ou 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [Use multimon=i:0 = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |