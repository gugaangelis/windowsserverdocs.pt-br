---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: Retenção de implementar do cenário de informações em servidores de arquivos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59fd7f0a0a4d9ed8f5cec57b17be21e1aa4cd592
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880247"
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Cenário: implementar a retenção de informações em servidores de arquivos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um período de retenção é a quantidade de tempo que um documento deve ser mantido antes de expirar. Dependendo da organização, o período de retenção pode ser diferente. Você pode classificar arquivos em uma pasta com períodos de retenção de curto, médio e longo prazo e atribuir um cronograma para cada período. Talvez queira manter um arquivo indefinidamente colocando-o em bloqueio legal.  
  
## <a name="BKMK_OVER"></a>Descrição do cenário  
A Infraestrutura de Classificação de Arquivos e o Gerenciador de Recursos de Servidor de Arquivos usam tarefas de gerenciamento de arquivos e classificação de arquivos para aplicar períodos de retenção para um conjunto de arquivos. Você pode atribuir um período de retenção em uma pasta e usar uma tarefa de gerenciamento de arquivos para configurar o tempo de duração de um período de retenção atribuído. Quando os arquivos da pasta estão prestes a expirar, o proprietário do arquivo recebe um email de notificação. Você também pode classificar um arquivo como estando em bloqueio legal para que a tarefa de gerenciamento de arquivos não faça o arquivo expirar.  
  
Você pode encontrar as informações de planejamento para configurar a retenção em [Plan for Retention of Information on File Servers](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
Você pode encontrar etapas para classificar arquivos para retenção legal e configurar um período de retenção em [implantar implementando retenção de informações em servidores de arquivos &#40;passo a passo&#41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> Esse cenário discute somente como classificar manualmente um documento para retenção legal. No entanto, é possível no Windows Server 2012 para classificar automaticamente documentos para retenção legal. Uma maneira de fazer isso é criar um classificador do Windows PowerShell que compara o proprietário do arquivo com uma lista de contas de usuário que estão sob retenção legal. Se o proprietário do arquivo fizer parte da lista de conta de usuário, o arquivo será classificado para retenção legal.  
  
## <a name="in-this-scenario"></a>Neste cenário  
Este cenário faz parte do cenário de Controle de Acesso Dinâmico. Para obter informações adicionais sobre o Controle de Acesso Dinâmico, consulte:  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Recursos incluídos neste cenário  
A tabela a seguir lista os recursos que fazem parte deste cenário e descreve como dar suporte a ele.  
  
|Recurso|Como este cenário tem suporte|  
|-----------|---------------------------------|  
|[Visão geral do Gerenciador de recursos de servidor de arquivos](https://technet.microsoft.com/library/hh831701.aspx)|A Infraestrutura de Classificação de Arquivos é um recurso incluído no Gerenciador de Recursos de Servidor de Arquivos.|  
|[Visão geral dos serviços de armazenamento e de arquivo](https://technet.microsoft.com/library/hh831487.aspx)|O Gerenciador de Recursos de Servidor de Arquivos é um recurso incluído com a função de servidor Serviços de Arquivo.|  
  
  


