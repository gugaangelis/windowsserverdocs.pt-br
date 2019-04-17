---
title: Gerenciamento de triagem de arquivos
description: "Este artigo descreve como criar triagens de arquivo, gerar notificações, definir modelos de triagem e criar exceções de triagem de arquivo"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 52a08ae7eaee81c00985d5334f9abeaa84e30879
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="file-screening-management"></a>Gerenciamento de triagem de arquivos

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

No nó **Gerenciamento de triagem de arquivo** do snap-in do MMC do Gerenciador de recursos do servidor de arquivos, você pode executar as seguintes tarefas:

-   Criar triagens de arquivo para controlar os tipos de arquivos que os usuários podem salvar e gerar notificações quando os usuários tentam salvar arquivos não autorizados.
-   Definir modelos que podem ser aplicados a novos volumes de triagem ou pastas e que podem ser usados em toda uma organização.
-   Criar exceções de triagem de arquivo que estendem a flexibilidade das regras de triagem.

Por exemplo, você pode:

-   Certificar-se de que nenhum arquivo de música é armazenado em pastas pessoais em um servidor, mas você pode permitir o armazenamento de tipos específicos de arquivos de mídia que oferecem suporte ao gerenciamento de direitos legais ou estar em conformidade com políticas da empresa. Nesse mesmo cenário, convém dar a um vice-presidente os privilégios especiais da empresa para armazenar qualquer tipo de arquivos na pasta pessoal.
-   Implemente um processo de triagem para notificá-lo por email quando um arquivo executável for armazenado em uma pasta compartilhada, incluindo informações sobre o usuário que armazenou o arquivo e o local exato do arquivo, para que possam assumir as medidas preventivas apropriadas.

Esta seção inclui os seguintes tópicos:

-   [Definir grupos de arquivos para triagem](define-file-groups-for-screening.md)
-   [Criar uma triagem de arquivo](create-file-screen.md)
-   [Criar uma exceção de triagem de arquivo](create-file-screen-exception.md)
-   [Criar um modelo de triagem de arquivo](create-file-screen-template.md)
-   [Editar propriedades de modelo de triagem de arquivo](edit-file-screen-template-properties.md)

> [!Note]
> Para definir notificações por email e determinados recursos de relatório, você deve configurar primeiro as opções gerais do Gerenciador de recursos do servidor de arquivos.

## <a name="see-also"></a>Veja também

-   [Definindo Opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)


