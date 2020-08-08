---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: Cenário Obtenha informações sobre seus dados usando a classificação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 7c06cc4bbca7e21ffa5ef58ec6e33fc43b3f9e49
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940239"
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Cenário: aprofunde-se em seus dados por meio da classificação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A dependência de recursos de dados e armazenamento continua a crescer em importância na maioria das organizações. Os administradores de TI enfrentam o desafio crescente de supervisar infraestruturas de armazenamento maiores e mais complexas e, ao mesmo tempo, são responsáveis por assegurar que o custo total de propriedade seja mantido em níveis razoáveis. O gerenciamento dos recursos de armazenamento não se relaciona apenas ao volume ou à disponibilidade dos dados. Trata-se também de impor as políticas da empresa e saber como o armazenamento é consumido para habilitar a utilização e a conformidade eficientes para reduzir o risco. A Infraestrutura de Classificação de Dados oferece um panorama dos dados automatizando os processos de classificação para você poder gerenciar os dados com mais eficácia. Os métodos de classificação a seguir estão disponíveis com a Infraestrutura de Classificação de Arquivos: manual, programático e automático. Este tópico se concentra no método automático de classificação de arquivos.

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrição do cenário
A Infraestrutura de Classificação de Arquivos usa regras de classificação para examinar arquivos automaticamente e classificá-los de acordo com o respectivo conteúdo. As propriedades de classificação são definidas centralmente no Active Directory para que essas definições possam ser compartilhadas entre os servidores de arquivos da organização. Você pode criar regras de classificação que examinem arquivos em busca de uma cadeia de caracteres padrão ou uma cadeia de caracteres que corresponda a um padrão (expressão regular). Quando um parâmetro de classificação configurado é encontrado em um arquivo, esse arquivo é classificado como configurado na regra de classificação. Alguns exemplos de regras de classificação são:

-   Classificar qualquer arquivo que contenha a cadeia de caracteres "contoso Confidential" como tendo alto impacto nos negócios

-   Classificar qualquer arquivo que contenha pelo menos 10 números de cadastro de pessoa física como portador de informações de identificação pessoal

Quando um arquivo é classificado, você pode usar uma tarefa de gerenciamento de arquivos para realizar uma ação sobre qualquer arquivo classificado de um modo específico. As ações de uma tarefa de gerenciamento de arquivos abrangem a proteção dos direitos associados ao arquivo, a expiração do arquivo e a execução de uma ação personalizada (como a publicação de informações em um serviço Web).

Você pode encontrar informações de planejamento para configurar a classificação automática de arquivos em [Planejar para a classificação automática de arquivos](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955).

Você pode encontrar etapas sobre como classificar automaticamente os arquivos em [implantar a classificação automática de arquivos &#40;etapas de demonstração&#41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).

## <a name="in-this-scenario"></a>Neste cenário
Este cenário faz parte do cenário de Controle de Acesso Dinâmico. Para obter informações adicionais sobre o Controle de Acesso Dinâmico, consulte:

-   [Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas
A infraestrutura de classificação de arquivos no Windows Server 2012 contribui para o controle de acesso dinâmico, permitindo que os proprietários de dados comerciais classifiquem e rotulem dados facilmente. As informações de classificação armazenadas na política de acesso central permitem definir políticas de acesso para classes de dados essenciais aos negócios.

## <a name="features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Recursos incluídos neste cenário
A tabela a seguir lista os recursos que fazem parte deste cenário e descreve como dar suporte a ele.

|Recurso|Como este cenário tem suporte|
|-----------|---------------------------------|
|[File Server Resource Manager Overview](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831701(v=ws.11))|A Infraestrutura de Classificação de Arquivos é um recurso incluído no Gerenciador de Recursos de Servidor de Arquivos.|
|[Visão geral dos Serviços de Arquivo e Armazenamento](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831487(v=ws.11))|O Gerenciador de Recursos de Servidor de Arquivos é um recurso incluído com a função de servidor Serviços de Arquivo.|

