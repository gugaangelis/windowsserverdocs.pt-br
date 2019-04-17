---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: "Cenário implemente retenção de informações em servidores de arquivos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62339170895190435adb9a97d565877ff78d97d7
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Cenário: Implementar retenção de informações em servidores de arquivos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um período de retenção é a quantidade de tempo em que um documento deve ser mantidos antes de ele expirou. Dependendo da organização, o período de retenção pode ser diferente. Você pode classificar arquivos em uma pasta como tendo um período de retenção curtos, médio ou a longo prazo e, em seguida, atribua um prazo para cada período. Convém manter um arquivo indefinidamente colocando-o em espera legal.  
  
## <a name="BKMK_OVER"></a>Descrição do cenário  
Infraestrutura de classificação de arquivo e o Gerenciador de recursos do servidor de arquivos usa tarefas de gerenciamento de arquivo e classificação de arquivos para aplicar períodos de retenção para um conjunto de arquivos. Você pode atribuir um período de retenção em uma pasta e, em seguida, usar uma tarefa de gerenciamento de arquivo para configurar um período de retenção atribuído quanto tempo é a última. Quando os arquivos na pasta estão prestes a expirar, o proprietário do arquivo recebe um email de notificação. Você também pode classificar um arquivo como se estivessem em legais, para que a tarefa de gerenciamento de arquivo não expira o arquivo.  
  
Você pode encontrar informações de planejamento para configurar retenção em [planejar a retenção de informações em servidores de arquivos](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
Você pode encontrar as etapas para classificar os arquivos para o controle legal e configurando um período de retenção em [implantar implementando retenção de informações em servidores de arquivos #40 etapas de demonstração &; 41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> Esse cenário aborda apenas como classificar manualmente um documento para o controle legal. No entanto, é possível no Windows Server 2012 classificar automaticamente os documentos para o controle legal. Uma maneira de fazer isso é criar um classificador do Windows PowerShell que compara o proprietário do arquivo a uma lista de contas de usuário que estejam sob controle legal. Se o proprietário do arquivo é uma parte da lista de conta de usuário, o arquivo é classificado para o controle legal.  
  
## <a name="in-this-scenario"></a>Neste cenário  
Este cenário é parte do cenário de controle de acesso dinâmico. Para obter informações adicionais sobre o controle de acesso dinâmico, consulte:  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Recursos incluídos neste cenário  
A tabela a seguir lista os recursos que fazem parte desse cenário e descreve como suporte.  
  
|Recurso|Como ele dá suporte a esse cenário|  
|-----------|---------------------------------|  
|[Visão geral do Gerenciador de recursos do servidor de arquivos](https://technet.microsoft.com/library/hh831701.aspx)|Infraestrutura de classificação de arquivo é um recurso que está incluído no Gerenciador de recursos de servidor de arquivos.|  
|[Arquivo e visão geral dos serviços de armazenamento](https://technet.microsoft.com/library/hh831487.aspx)|Gerenciador de recursos do servidor de arquivos é um recurso que está incluído com a função de servidor Serviços de arquivo.|  
  
  


