---
title: Alterações no Nano Server no Canal Semestral do Windows Server
description: No novo modelo de manutenção do Windows Server, o Nano Server é um contêiner apenas do sistema operacional, com algumas alterações de recurso.
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 05/21/2019
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: c12ca84826a92fa045eb84b55e7406392161280b
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452798"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Alterações no Nano Server no Canal Semestral do Windows Server

>Aplica-se a: Windows Server, o canal semestral

Se você já estiver executando o Nano Server, o [janela Server o canal semestral](../get-started-19/servicing-channels-19.md) modelo de serviço será familiar, já que ele anteriormente foi atendido pelo Branch atual para o modelo de negócios (CBB). Canal semestral do Windows Server é um novo nome para o mesmo modelo. Nesse modelo, espera-se versões de atualização de recurso do Nano Server de duas a três vezes por ano.

No entanto, começando com o Windows Server, versão 1803, Nano Server está disponível apenas como uma **imagem de sistema operacional base do contêiner**. Você deve executá-lo como um contêiner em um host de contêiner, como uma instalação Server Core do Windows Server. A execução de um contêiner com base no Nano Server nesta versão difere de versões anteriores das seguintes maneiras:

- O Nano Server foi otimizado para aplicativos .NET Core.
- .NET Core é e ainda menor que a versão do Windows Server 2016.
- PowerShell Core, .NET Core e WMI não são mais incluídos por padrão, mas você pode incluir pacotes de contêiner do [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) e [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) ao criar seu contêiner.
- Não há mais uma pilha de atendimento incluída no Nano Server. A Microsoft publica um contêiner Nano atualizado ao Hub do Docker que você reimplantou.
- Você soluciona os problemas do novo Nano Container usando o Docker.
- Agora você pode executar contêineres Nano no IoT Core.

## <a name="related-topics"></a>Tópicos relacionados

- [Documentação do contêiner do Windows](http://aka.ms/windowscontainers)
- [Visão geral da janela Server o canal semestral](../get-started-19/servicing-channels-19.md)
