---
title: Esquema de URI de clientes da área de trabalho remota
description: Saiba mais sobre o esquema Uniform Resource Identifier para clientes de área de trabalho remota
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
ms.openlocfilehash: f2934fed43c8f4feec2f321d684cc3593933eb5d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297465"
---
# Suporte de cliente da área de trabalho remota esquema Universal Resource Identifier (URI)

>Aplica-se a: Windows Server, versão 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Habilitando um esquema de URI Uniform Resource Identifier () oferece uma maneira para integrar recursos dos clientes de área de trabalho remota entre plataformas de profissionais de TI e desenvolvedores e enriquece a experiência do usuário, permitindo que: 

- Aplicativos de terceiros para iniciar a área de trabalho remota Microsoft e iniciar sessões remotas com configurações predefinidas (fornecidas como parte da cadeia de caracteres URI).
- Usuários finais para iniciar conexões remotas usando URLs previamente configuradas.

>[!NOTE]
> Usando um URI para conectar-se para o cliente de área de trabalho remota não há suporte para sistemas operacionais Windows - use o URI com MacOS, iOS e Android dispositivos.

Área de trabalho remota Microsoft usa o rdp://query_string de esquema URI para armazenar as configurações de atributo pré-configurados que são usadas ao iniciar o cliente. As cadeias de caracteres de consulta representam um único ou um conjunto de atributos RDP fornecidos na URL. 

Os atributos RDP são separados pelo símbolo de e comercial (&). Por exemplo, ao se conectar a um computador, a cadeia de caracteres é:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Esta tabela fornece uma lista completa de atributos com suporte que pode ser usado com o iOS, Mac e Android clientes da área de trabalho remotos. (Um "x" na coluna plataforma indica que o atributo é compatível. Os valores denotados por divisas (<>) representam os valores que são compatíveis com os clientes de área de trabalho remota).

| **Atributo RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| Permitir composição da área de trabalho = i:&lt;0 ou 1&gt;                    | x       | x   | x   |
| permitir que a suavização de fonte = i:<0 ou 1&gt;                         | x       | x   | x   |
| alternativo shell = s:&lt;cadeia de caracteres&gt;                              | x       | x   | x   |
| [audiomode = i:&lt;0, 1 ou 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [nível de autenticação = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| conectar-se ao console = i:&lt;0 ou 1&gt;                           | x       | x   | x   |
| desabilitar as configurações de cursor = i:&lt;0 ou 1&gt;                      | x       | x   | x   |
| Desabilitar arrastar janela por completo = i:&lt;0 ou 1&gt;                     | x       | x   | x   |
| desabilitar o menu anims = i:&lt;0 ou 1&gt;                           | x       | x   | x   |
| desativar temas = i:&lt;0 ou 1&gt;                               | x       | x   | x   |
| desabilitar o papel de parede = i:&lt;0 ou 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (esse é o único valor com suporte) | x       | x   |     |
| [desktopheight = i:&lt;valor em pixels&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;valor em pixels&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [domínio = s:&lt;cadeia de caracteres&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [completos do endereço = s:&lt;cadeia de caracteres&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname = s:&lt;cadeia de caracteres&gt;                  | x | x | x |
| [gatewayusagemethod = i:&lt;1 ou 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [prompt para credenciais no cliente = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo = s:&lt;cadeia de caracteres&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline = s:&lt;cadeia de caracteres&gt;         | x | x | x |
| remoteapplicationmode = i:&lt;0 ou 1&gt;            | x | x | x |
| remoteapplicationprogram = s:&lt;cadeia de caracteres&gt;         | x | x | x |
| diretório de trabalho do shell = s:&lt;cadeia de caracteres&gt;          | x | x | x |
| Nome do servidor de redirecionamento de uso = i:&lt;0 ou 1&gt;      | x | x | x |
| [nome de usuário = s:&lt;cadeia de caracteres&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [id do modo de tela = i:&lt;1 ou 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [sessão bpp = i:&lt;15, 8, 16, 24 ou 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [usar multimon = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |