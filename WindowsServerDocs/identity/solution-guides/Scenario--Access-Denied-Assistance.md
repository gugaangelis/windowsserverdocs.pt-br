---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Acesso ao cenário-assistência negada
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: fe269b92e8c8fcd9fc58c82307ea8b180231a87a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952734"
---
# <a name="scenario-access-denied-assistance"></a>Cenário: assistência para acesso negado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os usuários receberão uma mensagem de acesso negado ao tentar acessar pastas e arquivos compartilhados em um servidor de arquivos para o qual eles não têm permissões. Os administradores muitas vezes não têm o contexto apropriado para solucionar problema de acesso, o que torna difícil resolver o problema.

## <a name="scenario-description"></a>Descrição do cenário
A assistência de acesso negado é um novo recurso do Windows Server 2012, que fornece as seguintes maneiras de solucionar problemas relacionados ao acesso a arquivos e pastas:

-   **Autoassistência.** Se um usuário puder determinar e corrigir o problema para obter o acesso solicitado, não haverá muito impacto nos negócios e nenhuma exceção especial será necessária na política de acesso central. A assistência para acesso negado fornece uma mensagem de acesso negado que os administradores do servidor de arquivos podem personalizar com informações específicas às suas organizações. Por exemplo, um administrador pode definir a mensagem de modo que os usuários possam solicitar acesso de um proprietário de dados sem envolver o administrador do servidor de arquivos.

-   **Assistência pelo proprietário dos dados.** Você pode definir uma lista de distribuição para pastas compartilhadas e configurá-la para que o proprietário da pasta receba uma notificação por email quando um usuário precisar de acesso. Se o proprietário dos dados não souber como ajudar o usuário a obter acesso, o proprietário poderá encaminhar essas informações ao administrador do servidor de arquivos.

-   **Assistência pelo administrador do servidor de arquivos.** Este tipo de assistência está disponível quando o usuário não pode corrigir um problema e o proprietário dos dados não pode ajudar.  O Windows Server 2012 fornece uma interface do usuário em que os administradores de servidor de arquivos podem exibir as permissões efetivas de um usuário em um arquivo ou pasta para que seja mais fácil solucionar problemas de acesso.

A assistência com acesso negado no Windows Server 2012 fornece aos administradores de servidor de arquivos os detalhes de acesso relevantes para que eles possam determinar o problema e as ferramentas apropriadas para que possam fazer alterações de configuração para atender à solicitação de acesso. Por exemplo, um usuário pode seguir este processo para acessar um arquivo ao qual atualmente não têm acesso:

-   O usuário tenta ler um arquivo na \\ pasta compartilhada \financeshares, mas o servidor exibe uma mensagem de acesso negado.

-    O Windows Server 2012 exibe as informações de assistência de acesso negado ao usuário com uma opção para solicitar assistência.

-   Se o usuário solicitar acesso ao recurso, o servidor enviará um email com as informações de solicitação de acesso para o proprietário da pasta.

Você pode encontrar informações de planejamento para a configuração de assistência para acesso negado em [Planejar assistência para acesso negado](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).

Você pode encontrar etapas sobre como configurar a assistência de acesso negado em [implantar a assistência de acesso negado &#40;etapas de demonstração&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).

## <a name="in-this-scenario"></a>Neste cenário
Este cenário faz parte do cenário de Controle de Acesso Dinâmico. Para obter informações adicionais sobre o Controle de Acesso Dinâmico, consulte:

-   [Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)

## <a name="practical-applications"></a>Aplicações práticas
A assistência de acesso negado no Windows Server 2012 contribui para o controle de acesso dinâmico, fornecendo aos usuários a capacidade de solicitar acesso a arquivos e pastas compartilhados diretamente de uma mensagem de acesso negado.

## <a name="features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Recursos incluídos neste cenário
A tabela a seguir lista os recursos que fazem parte deste cenário e descreve como dar suporte a ele.

|Recurso|Como este cenário tem suporte|
|-----------|---------------------------------|
|[File Server Resource Manager Overview](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831701(v=ws.11))|A assistência para acesso negado pode ser configurada usando o console do Gerenciador de Recursos de Servidor de Arquivos.|
|[Visão geral dos Serviços de Arquivo e Armazenamento](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831487(v=ws.11))|O Gerenciador de Recursos de Servidor de Arquivos é um serviço da função Serviços de Arquivo e Armazenamento e é composto por um conjunto de recursos que podem ser usados​para administrar os servidores de arquivos em sua rede.|

