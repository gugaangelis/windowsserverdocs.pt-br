---
title: Gerenciamento de recursos de armazenamento remoto
description: Este artigo descreve como gerenciar recursos de armazenamento em um computador remoto
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5c6dc9c931e130e36e01655de05fbd209f50f3dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394080"
---
# <a name="managing-remote-storage-resources"></a>Gerenciamento de recursos de armazenamento remoto

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para gerenciar recursos de armazenamento em um computador remoto, você tem duas opções:

-   Conectar ao computador remoto do Gerenciador de recursos do servidor de arquivos Microsoft<sup>®</sup> Management Console (MMC) snap-in (que, em seguida, você pode usar para gerenciar os recursos remotos).
-   Use as ferramentas de linha de comando instaladas com o Gerenciador de Recursos de Servidor de Arquivos.

Qualquer opção permite que você trabalhe com cotas, arquivos de triagem, gerencie classificações, agende tarefas de gerenciamento de arquivo e gere relatórios com esses recursos remotos.

> [!Note]
> O Gerenciador de recursos do servidor de arquivos pode gerenciar recursos no computador local ou em um computador remoto, mas não em ambos ao mesmo tempo.

Por exemplo, você pode:

-   Conectar a outro computador no domínio usando o snap-in do MMC do Gerenciador de recursos do servidor de arquivos e examinar a utilização de armazenamento em um volume ou uma pasta localizado no computador remoto.
-   Criar modelos de cota e triagem de arquivo em um servidor local e, em seguida, usar as ferramentas de linha de comando para importar esses modelos para um servidor de arquivos localizado em uma filial.

Esta seção inclui os seguintes tópicos:

-   [Conectar a um computador remoto](connect-to-remote-computer.md)
-   [Ferramentas de linha de comando](command-line-tools.md)
