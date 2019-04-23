---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: Cenário assistência para acesso negado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829087"
---
# <a name="scenario-access-denied-assistance"></a>Cenário: assistência para acesso negado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os usuários receberão uma mensagem de acesso negado ao tentar acessar pastas e arquivos compartilhados em um servidor de arquivos para o qual eles não têm permissões. Os administradores muitas vezes não têm o contexto apropriado para solucionar problema de acesso, o que torna difícil resolver o problema.  
  
## <a name="scenario-description"></a>Descrição do cenário  
Acesso negado assistência é um novo recurso no Windows Server 2012, que fornece as seguintes maneiras para solucionar problemas relacionados ao acesso a arquivos e pastas:  
  
-   **Autoassistência.** Se um usuário puder determinar e corrigir o problema para obter o acesso solicitado, não haverá muito impacto nos negócios e nenhuma exceção especial será necessária na política de acesso central. A assistência para acesso negado fornece uma mensagem de acesso negado que os administradores do servidor de arquivos podem personalizar com informações específicas às suas organizações. Por exemplo, um administrador pode definir a mensagem de modo que os usuários possam solicitar acesso de um proprietário de dados sem envolver o administrador do servidor de arquivos.  
  
-   **Assistência pelo proprietário dos dados.** Você pode definir uma lista de distribuição para pastas compartilhadas e configurá-la para que o proprietário da pasta receba uma notificação por email quando um usuário precisar de acesso. Se o proprietário dos dados não souber como ajudar o usuário a obter acesso, o proprietário poderá encaminhar essas informações ao administrador do servidor de arquivos.  
  
-   **Assistência pelo administrador do servidor de arquivos.** Este tipo de assistência está disponível quando o usuário não pode corrigir um problema e o proprietário dos dados não pode ajudar.  Windows Server 2012 fornece uma interface do usuário em que os administradores de servidor de arquivos podem exibir as permissões efetivas para um usuário em um arquivo ou pasta para que ele seja mais fácil solucionar problemas de acesso.  
  
Assistência para acesso negado no Windows Server 2012 fornece aos administradores de servidor de arquivos os detalhes de acesso relevantes para que possam determinar o problema e as ferramentas adequadas para que ele podem fazer alterações de configuração para atender à solicitação de acesso. Por exemplo, um usuário pode seguir este processo para acessar um arquivo ao qual atualmente não têm acesso:  
  
-   O usuário tenta ler um arquivo no \\\financeshares compartilhados pasta, mas o servidor exibirá uma mensagem acesso negado.  
  
-    Windows Server 2012 exibe as informações de assistência para acesso negado ao usuário com uma opção para solicitar assistência.  
  
-   Se o usuário solicitar acesso ao recurso, o servidor enviará um email com as informações de solicitação de acesso para o proprietário da pasta.  
  
Você pode encontrar as informações de planejamento para configurar a assistência para acesso negado em [Plan for Access-Denied Assistance](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Você pode encontrar etapas sobre como configurar a assistência para acesso negado na [implantar assistência &#40;passo a passo&#41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>Neste cenário  
Este cenário faz parte do cenário de Controle de Acesso Dinâmico. Para obter informações adicionais sobre o Controle de Acesso Dinâmico, consulte:  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Aplicações práticas  
Assistência para acesso negado no Windows Server 2012 contribui para o controle de acesso dinâmico, dando aos usuários a capacidade de solicitar acesso a arquivos e pastas compartilhados diretamente a partir de uma mensagem de acesso negado.  
  
## <a name="BKMK_NEW"></a>Recursos incluídos neste cenário  
A tabela a seguir lista os recursos que fazem parte deste cenário e descreve como dar suporte a ele.  
  
|Recurso|Como este cenário tem suporte|  
|-----------|---------------------------------|  
|[Visão geral do Gerenciador de recursos de servidor de arquivos](https://technet.microsoft.com/library/hh831701.aspx)|A assistência para acesso negado pode ser configurada usando o console do Gerenciador de Recursos de Servidor de Arquivos.|  
|[Visão geral dos serviços de armazenamento e de arquivo](https://technet.microsoft.com/library/hh831487.aspx)|O Gerenciador de Recursos de Servidor de Arquivos é um serviço da função Serviços de Arquivo e Armazenamento e é composto por um conjunto de recursos que podem ser usados​para administrar os servidores de arquivos em sua rede.|  
  


