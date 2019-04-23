---
title: Preparar seu ambiente de desenvolvimento
description: Como preparar o SDK do Windows Admin Center do seu ambiente de desenvolvimento (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b1a0672ee374f3e2d1339c43576db0e5cabdc36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834757"
---
# <a name="prepare-your-development-environment"></a>Preparar seu ambiente de desenvolvimento

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Vamos começar a desenvolver extensões com o Windows Admin Center SDK!  Neste documento, abordaremos o processo para obter seu ambiente para cima e em execução para compilar e testar uma extensão para o Windows Admin Center.

> [!NOTE]
> Novo no SDK do Windows Admin Center?  Saiba mais sobre [As extensões do Windows Admin Center](extensibility-overview.md)

Para preparar seu ambiente de desenvolvimento, execute as seguintes etapas:

## <a name="install-prerequisites"></a>Pré-requisitos de instalação

Para começar a desenvolver com o SDK, baixe e instale os seguintes pré-requisitos:

* [Windows Admin Center](https://aka.ms/WACDownloadPage) (versão de GA ou visualização)
* Visual Studio ou [Visual Studio Code](http://code.visualstudio.com)
* [Node Package Manager](https://npmjs.com/get-npm) (8.12.0 ou posterior)
* [NuGet](https://www.nuget.org/downloads) (para extensões de publicação)

> [!NOTE]
> Você precisa instalar e executar o Windows Admin Center no modo de desenvolvimento para siga as etapas abaixo. Modo de dev permite que o Windows Admin Center carregar os pacotes de extensão não assinados.
>
>  Para habilitar o modo Dev, instale o Windows Admin Center na linha de comando com o parâmetro DEV_MODE = 1. No exemplo a seguir, substitua ```<version>``` com a versão que você está instalando, isto é, ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>Instalar dependências globais

Em seguida, instale ou atualize as dependências necessárias para seus projetos, com o Node Package Manager. Essas dependências serão instaladas globalmente e estarão disponíveis para todos os projetos.

```
npm install -g npm

npm install -g @angular/cli@1.6.5

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>Você pode instalar uma versão posterior do @angular/cli, no entanto, esteja ciente de que, se você instalar uma versão maior do que 1.6.5, você receberá um aviso durante a etapa de compilação do gulp que a versão da cli local não coincide com a versão instalada.

## <a name="next-steps"></a>Próximas etapas

Agora que seu ambiente está preparado, você está pronto para começar a criar conteúdo.

- Criar uma extensão de [ferramenta](develop-tool.md)
- Criar uma extensão da [solução](develop-solution.md)
- Criar um [plug-in de gateway](develop-gateway-plugin.md)
- Saiba mais com nossos [guias](guides.md)

## <a name="sdk-design-toolkit"></a>Kit de ferramentas de design SDK

Fazer check-out de nosso Windows Admin Center [Kit de ferramentas de design SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)! Esse kit de ferramentas foi projetado para ajudá-lo rapidamente maquetes de extensões no PowerPoint usando estilos Windows Admin Center, controles e modelos de página. Veja sua extensão possível aparência em Windows Admin Center antes de começar a codificar!

