---
title: Preparar seu ambiente de desenvolvimento
description: Preparar o desenvolvimento do seu ambiente do SDK do Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b1a0672ee374f3e2d1339c43576db0e5cabdc36
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080913"
---
# Prepare seu ambiente de desenvolvimento

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Vamos começar a desenvolver extensões com o Windows Admin Center SDK!  Neste documento, abordaremos o processo para obter seu ambiente de backup e em execução para criar e testar uma extensão para o Windows Admin Center.

> [!NOTE]
> Novo no SDK do Windows Admin Center?  Saiba mais sobre [As extensões do Windows Admin Center](extensibility-overview.md)

Para preparar seu ambiente de desenvolvimento, execute as seguintes etapas:

## Pré-requisitos de instalação

Para começar a desenvolver com o SDK, baixe e instale os seguintes pré-requisitos:

* [O Windows Admin Center](https://aka.ms/WACDownloadPage) (Versão GA ou visualização)
* Visual Studio ou [Visual Studio Code](http://code.visualstudio.com)
* [Gerenciador de pacotes do nó](https://npmjs.com/get-npm) (8.12.0 ou posterior)
* [NuGet](https://www.nuget.org/downloads) (para extensões de publicação)

> [!NOTE]
> Você precisa instalar e executar o Windows Admin Center no modo de desenvolvimento para siga as etapas abaixo. Modo de dev permite que o Windows Admin Center carregar os pacotes de extensão não assinados.
>
>  Para habilitar o modo Dev, instale o Windows Admin Center na linha de comando com o parâmetro DEV_MODE = 1. No exemplo a seguir, substitua ```<version>``` com a versão que você está instalando, isto é, ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## Instalar dependências globais

Em seguida, instalar ou atualizar dependências necessárias para seus projetos, com o Gerenciador de pacotes do nó. Essas dependências serão instaladas globalmente e estarão disponíveis para todos os projetos.

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>Você pode instalar uma versão posterior do @angular/cli, porém lembre-se de que, se você instalar uma versão maior do que 1.6.5, você receberá um aviso durante a etapa de compilação gulp que a versão local cli não coincide com a versão instalada.

## Próximas etapas

Agora que seu ambiente está preparado, você estará pronto para iniciar a criação de conteúdo.

- Criar uma extensão de [ferramenta](develop-tool.md)
- Criar uma extensão da [solução](develop-solution.md)
- Criar um [plug-in de gateway](develop-gateway-plugin.md)
- Saiba mais com nossos [guias](guides.md)

## Kit de ferramentas de design SDK

Confira nossos Windows Admin Center [Kit de ferramentas de design SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)! Esse kit de ferramentas foi projetado para ajudá-lo rapidamente simular extensões no PowerPoint usando o Windows Admin Center estilos, controles e modelos de página. Consulte sua extensão possível aparência no Centro de administração do Windows antes de começar a codificar!

