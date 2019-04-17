---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: "Percepção de obter seus dados usando a classificação do cenário"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e5248b5fd5e20b7436de9f796367c018d8ef16e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Cenário: Obtenha ideias para seus dados por meio de classificação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dependência de recursos de dados e armazenamento continua a crescer em importância para a maioria das organizações. Os administradores de TI enfrentam o desafio crescente de supervisão infraestruturas de armazenamento maior e mais complexa, enquanto simultaneamente sendo designado a responsabilidade para garantir que esse custo de propriedade total é mantido em níveis razoáveis. Gerenciar recursos de armazenamento é não apenas sobre o volume ou a disponibilidade de dados; também é sobre a aplicação de políticas da empresa e saber como o armazenamento é consumido para habilitar o uso eficiente e conformidade para reduzir os riscos. Infraestrutura de classificação de arquivo fornece percepção seus dados automatizando processos de classificação para que você possa gerenciar seus dados de forma mais eficaz. Os métodos de classificação a seguir estão disponíveis com a infraestrutura de classificação de arquivo: manual, programática e automática. Este tópico enfoca o método de classificação de arquivo automática.  
  
## <a name="BKMK_OVER"></a>Descrição do cenário  
Infraestrutura de classificação de arquivo usa as regras de classificação para digitalizar arquivos automaticamente e classificá-los de acordo com o conteúdo do arquivo. Propriedades de classificação são definidas centralmente no Active Directory para que essas definições podem ser compartilhadas entre servidores de arquivos na organização. Você pode criar regras de classificação que examinam os arquivos para uma cadeia de caracteres padrão ou para uma cadeia de caracteres que corresponde a um padrão (expressão regular). Quando um parâmetro de classificação configurado é encontrado em um arquivo, esse arquivo é classificado como configurados na regra de classificação. Alguns exemplos de regras de classificação incluem:  
  
-   Classificar qualquer arquivo que contém a cadeia de caracteres "Contoso confidenciais" como tendo alto impacto comercial  
  
-   Classificar qualquer arquivo que contém pelo menos 10 números da previdência social como tendo informações pessoalmente identificáveis  
  
Quando um arquivo é classificado, você pode usar um arquivo de tarefa de gerenciamento para executar uma ação em todos os arquivos que são classificadas uma maneira específica. As ações em uma tarefa de gerenciamento de arquivo incluem proteger os direitos associados ao arquivo, a expiração do arquivo e executar uma ação personalizada (como informações para um serviço web de lançamento).  
  
Você pode encontrar informações de planejamento para configurar classificação automática de arquivos no [planejar a classificação de arquivo automática](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955).  
  
Você pode encontrar as etapas sobre como classificar automaticamente os arquivos em [implantar a classificação de arquivo automática #40 etapas de demonstração &; 41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>Neste cenário  
Este cenário é parte do cenário de controle de acesso dinâmico. Para obter informações adicionais sobre o controle de acesso dinâmico, consulte:  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
Infraestrutura de classificação de arquivo no Windows Server 2012 contribui para o controle de acesso dinâmico, permitindo que os proprietários de dados de negócios classificar e rotular dados facilmente. As informações de classificação que são armazenadas na política de acesso central permite que você defina as políticas de acesso para as classes de dados que são fundamentais para negócios.  
  
## <a name="BKMK_NEW"></a>Recursos incluídos neste cenário  
A tabela a seguir lista os recursos que fazem parte desse cenário e descreve como suporte.  
  
|Recurso|Como ele dá suporte a esse cenário|  
|-----------|---------------------------------|  
|[Visão geral do Gerenciador de recursos do servidor de arquivos](https://technet.microsoft.com/library/hh831701.aspx)|Infraestrutura de classificação de arquivo é um recurso que está incluído no Gerenciador de recursos de servidor de arquivos.|  
|[Arquivo e visão geral dos serviços de armazenamento](https://technet.microsoft.com/library/hh831487.aspx)|Gerenciador de recursos do servidor de arquivos é um recurso que está incluído com a função de servidor Serviços de arquivo.|  
  


