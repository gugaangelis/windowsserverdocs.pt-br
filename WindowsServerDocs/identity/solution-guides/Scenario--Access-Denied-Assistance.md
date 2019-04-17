---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: "Assistência de acesso negado do cenário"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-access-denied-assistance"></a>Cenário: Assistência de acesso negado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os usuários receberão uma mensagem de acesso negado quando eles tentam acessar arquivos e pastas em um servidor de arquivos para que eles não têm permissões compartilhadas. Muitas vezes, os administradores não têm o contexto adequado para solucionar o problema de acesso, o que torna difícil de resolver o problema.  
  
## <a name="scenario-description"></a>Descrição do cenário  
Acesso negado assistência é um novo recurso no Windows Server 2012, que fornece as seguintes maneiras de solucionar problemas relacionados ao acesso a arquivos e pastas:  
  
-   **Assistência Self.** Se um usuário pode determinar o problema e corrigir o problema, para que eles podem obter o acesso solicitado, o impacto para os negócios é baixo, e nenhuma exceção especial é necessários na política de acesso central. Assistência de acesso negado fornece uma mensagem de acesso negado que os administradores de servidor de arquivos podem personalizar com informações específicas em suas organizações. Por exemplo, um administrador pode definir a mensagem para que os usuários podem solicitar acesso de um proprietário de dados sem envolver o administrador de servidor de arquivos.  
  
-   **Assistência pelo proprietário dados.** Você pode definir uma lista de distribuição para pastas compartilhadas e configurá-lo para que o proprietário da pasta recebe uma notificação por email quando um usuário precisa de acesso. Se o proprietário dos dados não sabe como ajudar o usuário a acessar, o proprietário pode encaminhar essa informação ao administrador de servidor de arquivos.  
  
-   **Assistência pelo administrador do servidor de arquivos.** Esse tipo de Ajuda está disponível quando o usuário não pode corrigir um problema e o proprietário dos dados não pode ajudar.  Windows Server 2012 fornece uma interface do usuário em que os administradores de servidor de arquivos podem exibir as permissões efetivas para um usuário em um arquivo ou pasta para que seja mais fácil solucionar problemas de acesso.  
  
Assistência de acesso negado no Windows Server 2012 fornece os detalhes relevantes acesso administradores de servidor de arquivos para que eles possam determinar o problema e ferramentas apropriadas para que eles podem fazer alterações de configuração para atender à solicitação de acesso. Por exemplo, um usuário pode seguir esse processo para acessar um arquivo que eles atualmente não têm acesso a:  
  
-   O usuário tentar ler um arquivo na pasta compartilhada \\\financeshares, mas o servidor exibe uma mensagem de acesso negado.  
  
-    Windows Server 2012 exibe as informações de acesso negado assistência ao usuário com uma opção para solicitar assistência.  
  
-   Se o usuário solicitar acesso ao recurso, o servidor envia um email com as informações de solicitação de acesso para o proprietário da pasta.  
  
Você pode encontrar informações de planejamento para configurar o acesso negado assistência na [planejar para obter assistência Access-Denied](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1).  
  
Você pode encontrar as etapas sobre como configurar o acesso negado assistência na [Deploy Access-Denied assistência #40 etapas de demonstração &; 41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>Neste cenário  
Este cenário é parte do cenário de controle de acesso dinâmico. Para obter informações adicionais sobre o controle de acesso dinâmico, consulte:  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>Aplicativos práticos  
Assistência de acesso negado no Windows Server 2012 contribui para o controle de acesso dinâmico, oferecendo aos usuários a capacidade de solicitar acesso a arquivos e pastas compartilhados diretamente em uma mensagem de acesso negado.  
  
## <a name="BKMK_NEW"></a>Recursos incluídos neste cenário  
A tabela a seguir lista os recursos que fazem parte desse cenário e descreve como suporte.  
  
|Recurso|Como ele dá suporte a esse cenário|  
|-----------|---------------------------------|  
|[Visão geral do Gerenciador de recursos do servidor de arquivos](https://technet.microsoft.com/library/hh831701.aspx)|Assistência de acesso negado pode ser configurada usando o console do Gerenciador de recursos do servidor de arquivos no servidor de arquivos.|  
|[Arquivo e visão geral dos serviços de armazenamento](https://technet.microsoft.com/library/hh831487.aspx)|Gerenciador de recursos do servidor de arquivos é um serviço de função Serviços de arquivo e armazenamento, e ele é composto de um conjunto de recursos que podem ser usados para administrar os servidores de arquivos em sua rede.|  
  


