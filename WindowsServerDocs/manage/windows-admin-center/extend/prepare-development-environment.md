---
title: Preparar seu ambiente de desenvolvimento
description: Como preparar o SDK do Windows Admin Center do seu ambiente de desenvolvimento (Project Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 09/18/2018
ms.openlocfilehash: fe519498e8021bde67b87ec7f78b3e1b9a64160b
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766939"
---
# <a name="prepare-your-development-environment"></a>Preparar seu ambiente de desenvolvimento

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Vamos começar a desenvolver extensões com o SDK do centro de administração do Windows!  Neste documento, abordaremos o processo para colocar seu ambiente em funcionamento para criar e testar uma extensão do centro de administração do Windows.

> [!NOTE]
> Novo no SDK do Windows Admin Center?  Saiba mais sobre [As extensões do Windows Admin Center](extensibility-overview.md)

Para preparar seu ambiente de desenvolvimento, execute as seguintes etapas:

## <a name="install-prerequisites"></a>Instalar pré-requisitos

Para começar a desenvolver com o SDK, baixe e instale os seguintes pré-requisitos:

* [Centro de administração do Windows](../overview.md) (versão de visualização ou GA)
* Visual Studio ou [Visual Studio Code](https://code.visualstudio.com)
* [Node.js](https://nodejs.org/en/download/releases/) (versão 10.3.0)
* [Gerenciador de pacotes de nó](https://npmjs.com/get-npm) (8.12.0 ou posterior)
* [NuGet](https://www.nuget.org/downloads) (para extensões de publicação)

> [!NOTE]
> Você precisa instalar e executar o Windows Admin Center no modo de desenvolvimento para siga as etapas abaixo. Modo de dev permite que o Windows Admin Center carregar os pacotes de extensão não assinados. O centro de administração do Windows só pode ser instalado no modo dev em um computador com Windows 10.
>
>  Para habilitar o modo Dev, instale o Windows Admin Center na linha de comando com o parâmetro DEV_MODE = 1. No exemplo a seguir, substitua ```<version>``` com a versão que você está instalando, isto é, ```WindowsAdminCenter1809.msi```.
>
> ```msiexec /i WindowsAdminCenter<version>.msi DEV_MODE=1```

## <a name="install-global-dependencies"></a>Instalar dependências globais

Em seguida, instale ou atualize as dependências necessárias para seus projetos, com o Gerenciador de pacotes de nó. Essas dependências serão instaladas globalmente e estarão disponíveis para todos os projetos.

```
npm install -g npm

npm install -g @angular/cli@7.1.2

npm install -g gulp
npm install -g typescript
npm install -g tslint
npm install -g windows-admin-center-cli
```

>[!NOTE]
>Você pode instalar uma versão mais recente do @angular/cli , mas lembre-se de que, se você instalar uma versão maior que 7.1.2, receberá um aviso durante a etapa de Build do Gulp que a versão da CLI local não corresponde à versão instalada.

## <a name="next-steps"></a>Próximas etapas

Agora que seu ambiente está preparado, você está pronto para começar a criar o conteúdo.

- Criar uma extensão de [ferramenta](develop-tool.md)
- Criar uma extensão da [solução](develop-solution.md)
- Criar um [plug-in de gateway](develop-gateway-plugin.md)
- Saiba mais com nossos [guias](guides.md)

## <a name="sdk-design-toolkit"></a>Kit de ferramentas de design do SDK

Confira o [Kit de ferramentas de design do SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip)do Windows Admin Center! Esse kit de ferramentas foi criado para ajudá-lo a simular rapidamente as extensões no PowerPoint usando estilos, controles e modelos de página do centro de administração do Windows. Veja como a extensão pode ser exibida no centro de administração do Windows antes de começar a codificar.