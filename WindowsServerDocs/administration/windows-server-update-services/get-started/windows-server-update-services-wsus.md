---
title: Introdução ao Windows Server Update Services (WSUS)
description: Tópico Windows Server Update Service (WSUS)-uma visão geral da função de servidor e seus aplicativos práticos
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 89247f91f616233fc6e4967a0457ff34fac221da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361652"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O WSUS (Windows Server Update Services) permite que os administradores de Tecnologia da Informação implantem as atualizações mais recentes dos produtos da Microsoft. Você pode usar o WSUS para gerenciar totalmente a distribuição de atualizações que são lançadas por meio do Microsoft Update para computadores na sua rede. Este tópico fornece uma visão geral dessa função de servidor, além de mais informações sobre como implantar e manter o WSUS.

## <a name="wsus-server-role-description"></a>Descrição da função de servidor do WSUS
Um servidor WSUS fornece recursos que você pode usar para gerenciar e distribuir atualizações por meio de um console de gerenciamento. Um servidor WSUS também pode ser a origem de atualização para outros servidores WSUS na organização. O servidor WSUS que atua como fonte de atualização é chamado de servidor upstream. Em uma implementação do WSUS, pelo menos um servidor WSUS em sua rede deve ser capaz de se conectar ao Microsoft Update para obter informações de atualização disponíveis. Como administrador, você pode determinar com base na segurança e na configuração da rede-quantos outros servidores WSUS se conectam diretamente ao Microsoft Update.

### <a name="practical-applications"></a>Aplicações práticas
O gerenciamento de atualizações é o processo de controlar a implantação e manutenção de versões provisórias de software em ambientes de produção. Ele ajuda a manter a eficiência operacional, superar vulnerabilidades de segurança e manter a estabilidade do seu ambiente de produção. Se sua organização não puder determinar e manter um nível de confiança conhecido em seus sistemas operacionais e aplicativos, talvez haja inúmeras vulnerabilidades de segurança que, se exploradas por hackers, poderão resultar em perda de receita e de propriedade intelectual. Amenizar essa ameaça exige que você tenha sistemas devidamente configurados, use os programas de software mais recentes e instale as atualizações recomendadas de software.

Os principais cenários em que o WSUS agrega valor aos seus negócios são:

-   Gerenciamento de atualizações centralizado

-   Automação do gerenciamento de atualizações

### <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

> [!NOTE]
> A atualização de qualquer versão do Windows Server que dê suporte ao WSUS 3,2 para o Windows Server 2012 R2 requer que você desinstale o WSUS 3,2 primeiro.
> 
> No Windows Server 2012, a atualização de qualquer versão do Windows Server com o WSUS 3,2 instalado será bloqueada durante o processo de instalação se o WSUS 3,2 for detectado. Nesse caso, será solicitado que você primeiro desinstale Windows Server Update Services antes de atualizar o servidor.
> 
> No entanto, devido às alterações nesta versão do Windows Server e do Windows Server 2012 R2, ao atualizar de qualquer versão do Windows Server e do WSUS 3,2, a instalação não é bloqueada. Falha ao desinstalar o WSUS 3,2 antes de executar uma atualização do Windows Server 2012 R2 fará com que as tarefas pós-instalação do WSUS no Windows Server 2012 R2 falhem. Nesse caso, a única medida corretiva conhecida é formatar o disco rígido e reinstalar o Windows Server.

O Windows Server Update Services é uma função de servidor interna que inclui as seguintes melhorias:

-   Pode ser adicionado e removido usando o Gerenciador do Servidor

-   Inclui cmdlets do Windows PowerShell para gerenciar as tarefas administrativas mais importantes no WSUS

-   Adiciona o recurso de hash SHA256 para aumentar a segurança

-   Fornece separação de cliente e servidor: as versões do WUA (agente de Windows Update) podem ser fornecidas independentemente do WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Usando o Windows PowerShell para gerenciar o WSUS
Para que os administradores de sistema automatizem suas operações, eles precisam de cobertura por meio de automação por linha de comando. A meta principal é facilitar a administração do WSUS permitindo que os administradores de sistema automatizem suas operações do dia a dia.

**Qual é o valor agregado desta alteração?**

Com a exposição das principais operações do WSUS por meio do Windows PowerShell, os administradores de sistema podem aumentar a produtividade, reduzir a curva de aprendizado de novas ferramentas e diminuir os erros devido a expectativas frustradas geradas por falta de consistência em operações semelhantes.

**O que passou a funcionar de maneira diferente?**

Nas versões anteriores do sistema operacional Windows Server, não havia cmdlets do Windows PowerShell, e a automação do gerenciamento de atualizações era um desafio. Os cmdlets do Windows PowerShell para operações do WSUS proporcionam flexibilidade e agilidade para o administrador de sistema.

## <a name="in-this-collection"></a>Nesta coleção
Os seguintes guias para planejamento, implantação e gerenciamento do WSUS estão nesta coleção:

-   [Implantar o Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Gerenciar atualizações usando o Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


