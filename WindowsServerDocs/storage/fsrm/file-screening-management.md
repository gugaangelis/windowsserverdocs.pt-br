---
title: Gerenciamento de triagem de arquivos
description: Este artigo descreve como criar triagens de arquivo, gerar notificações, definir modelos de triagem e criar exceções de triagem de arquivo
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c189386ac340e9362c8774340732f8689f17effe
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957433"
---
# <a name="file-screening-management"></a>Gerenciamento de triagem de arquivos

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

No nó **Gerenciamento de triagem de arquivo** do snap-in do MMC do Gerenciador de recursos do servidor de arquivos, você pode executar as seguintes tarefas:

-   Criar triagens de arquivo para controlar os tipos de arquivos que os usuários podem salvar e gerar notificações quando os usuários tentam salvar arquivos não autorizados.
-   Definir modelos que podem ser aplicados a novos volumes de triagem ou pastas e que podem ser usados em toda uma organização.
-   Criar exceções de triagem de arquivo que estendem a flexibilidade das regras de triagem.

Por exemplo, você pode:

-   Certificar-se de que nenhum arquivo de música é armazenado em pastas pessoais em um servidor, mas você pode permitir o armazenamento de tipos específicos de arquivos de mídia que oferecem suporte ao gerenciamento de direitos legais ou estar em conformidade com políticas da empresa. Nesse mesmo cenário, convém dar a um vice-presidente os privilégios especiais da empresa para armazenar qualquer tipo de arquivos na pasta pessoal.
-   Implemente um processo de triagem para notificá-lo por email quando um arquivo executável for armazenado em uma pasta compartilhada, incluindo informações sobre o usuário que armazenou o arquivo e o local exato do arquivo, para que possam assumir as medidas preventivas apropriadas.

Esta seção inclui os tópicos a seguir:

-   [Definir grupos de arquivos para triagem](define-file-groups-for-screening.md)
-   [Criar uma triagem de arquivo](create-file-screen.md)
-   [Criar uma exceção de triagem de arquivo](create-file-screen-exception.md)
-   [Criar um modelo de triagem de arquivo](create-file-screen-template.md)
-   [Editar propriedades de modelo de triagem de arquivo](edit-file-screen-template-properties.md)

> [!Note]
> Para definir notificações por email e determinados recursos de relatório, você deve configurar primeiro as opções gerais do Gerenciador de recursos do servidor de arquivos.

## <a name="additional-references"></a>Referências adicionais

-   [Definir opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)


