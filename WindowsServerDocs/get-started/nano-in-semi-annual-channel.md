---
title: Alterações no Nano Server no Canal Semestral do Windows Server
description: No novo modelo de manutenção do Windows Server, o Nano Server é um contêiner apenas do sistema operacional, com algumas alterações de recurso.
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: medium
ms.date: 05/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: 7e68d292c32ce58c786a3242203330fcae985913
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847767"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Alterações no Nano Server no Canal Semestral do Windows Server

>Aplica-se a: Windows Server, o canal semestral


Conforme descrito em [Visão geral de canal semestral do Windows Server](semi-annual-channel-overview.md), Windows Server, versão 1803 é a versão mais recente no canal semestral.

Se você já estiver executando o Nano Server, esse modelo de serviços será familiar, pois foi atendido anteriormente pelo modelo de Branch Atual para Negócios (CBB). O novo Canal semestral do Windows Server é apenas um novo nome para o mesmo modelo. Nesse modelo, espera-se versões de atualização de recurso do Nano Server de duas a três vezes por ano.

No entanto, com essa versão do Windows Server, versão 1803, o Nano Server está disponível apenas como uma **imagem de sistema operacional com base em contêiner**. Você deve executá-lo como um contêiner em um host de contêiner, como uma instalação Server Core do Windows Server. A execução de um contêiner com base no Nano Server nesta versão difere de versões anteriores das seguintes maneiras:

- O Nano Server foi otimizado para aplicativos .NET Core.
- .NET Core é e ainda menor que a versão do Windows Server 2016.
- PowerShell Core, .NET Core e WMI não são mais incluídos por padrão, mas você pode incluir pacotes de contêiner do [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) e [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) ao criar seu contêiner.
- Não há mais uma pilha de atendimento incluída no Nano Server. A Microsoft publica um contêiner Nano atualizado ao Hub do Docker que você reimplantou.
- Você soluciona os problemas do novo Nano Container usando o Docker.
- Agora você pode executar contêineres Nano no IoT Core.

## <a name="related-topics"></a>Tópicos relacionados
Quando o programa Insider é iniciado, encontre mais informações em [Documentação de Contêiner do Windows](http://aka.ms/windowscontainers).
