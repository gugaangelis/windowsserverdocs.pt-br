---
title: Introdução ao WSUS (Windows Server Update Services)
description: Tópico sobre o WSUS (Windows Server Update Service) – uma visão geral da função de servidor e suas aplicações práticas
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 3bb365c627840d4152b0dc450e24dd83d82103ca
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828620"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O WSUS (Windows Server Update Services) permite que os administradores de Tecnologia da Informação implantem as atualizações mais recentes dos produtos da Microsoft. Você pode usar o WSUS para gerenciar totalmente a distribuição de atualizações que são lançadas pelo Microsoft Update aos computadores da rede. Este tópico fornece uma visão geral dessa função de servidor, além de mais informações sobre como implantar e manter o WSUS.

## <a name="wsus-server-role-description"></a>Descrição da função de servidor do WSUS
Um servidor do WSUS fornece os recursos necessários para gerenciar e distribuir atualizações por meio de um console de gerenciamento. Um servidor do WSUS também pode ser a fonte de atualização de outros servidores do WSUS na organização. O servidor WSUS que atua como fonte de atualização é chamado de servidor upstream. Em uma implementação do WSUS, pelo menos um servidor do WSUS na rede precisa conseguir se conectar ao Microsoft Update para obter as informações de atualizações disponíveis. Como administrador, você pode determinar, com base na segurança e na configuração da rede, quantos outros servidores WSUS se conectam diretamente ao Microsoft Update.

### <a name="practical-applications"></a>Aplicações práticas
O gerenciamento de atualizações é o processo de controlar a implantação e manutenção de versões provisórias de software em ambientes de produção. Ele ajuda a manter a eficiência operacional, superar vulnerabilidades de segurança e manter a estabilidade do seu ambiente de produção. Se sua organização não puder determinar e manter um nível de confiança conhecido em seus sistemas operacionais e aplicativos, talvez haja inúmeras vulnerabilidades de segurança que, se exploradas por hackers, poderão resultar em perda de receita e de propriedade intelectual. Amenizar essa ameaça exige que você tenha sistemas devidamente configurados, use os programas de software mais recentes e instale as atualizações recomendadas de software.

Os principais cenários em que o WSUS agrega valor aos seus negócios são:

-   Gerenciamento centralizado de atualizações

-   Automação do gerenciamento de atualizações

### <a name="new-and-changed-functionality"></a>Funcionalidade nova e alterada

> [!NOTE]
> A atualização de qualquer versão do Windows Server que seja compatível com WSUS 3.2 para o Windows Server 2012 R2 exige que você primeiro desinstale o WSUS 3.2.
> 
> No Windows Server 2012, a atualização de qualquer versão do Windows Server com o WSUS 3.2 instalado será bloqueada durante o processo de instalação se o WSUS 3.2 for detectado. Nesse caso, será solicitado que você primeiro desinstale o Windows Server Update Services antes de atualizar o servidor.
> 
> No entanto, devido às alterações nesta versão do Windows Server e do Windows Server 2012 R2, ao atualizar de qualquer versão do Windows Server e do WSUS 3.2, a instalação não é bloqueada. A falha na desinstalação do WSUS 3.2 antes de executar uma atualização do Windows Server 2012 R2 causará falha nas tarefas de pós-instalação do WSUS no Windows Server 2012 R2. Nesse caso, a única medida corretiva conhecida é formatar o disco rígido e reinstalar o Windows Server.

O Windows Server Update Services é uma função de servidor interna que inclui as seguintes melhorias:

-   Pode ser adicionado e removido usando o Gerenciador do Servidor

-   Inclui cmdlets do Windows PowerShell para gerenciar as tarefas administrativas mais importantes no WSUS

-   Adiciona o recurso de hash SHA256 para aumentar a segurança

-   Fornece a separação entre cliente e servidor: as versões do WUA (Windows Update Agent) podem ser fornecidas independentemente do WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Usando o Windows PowerShell para gerenciar o WSUS
Para que os administradores de sistema automatizem suas operações, eles precisam de cobertura por meio de automação por linha de comando. A meta principal é facilitar a administração do WSUS permitindo que os administradores de sistema automatizem suas operações do dia a dia.

**Qual é o valor agregado desta alteração?**

Com a exposição das principais operações do WSUS por meio do Windows PowerShell, os administradores de sistema podem aumentar a produtividade, reduzir a curva de aprendizado de novas ferramentas e diminuir os erros devido a expectativas frustradas geradas por falta de consistência em operações semelhantes.

**O que passou a funcionar de maneira diferente?**

Nas versões anteriores do sistema operacional Windows Server, não havia cmdlets do Windows PowerShell, e a automação do gerenciamento de atualizações era um desafio. Os cmdlets do Windows PowerShell para operações do WSUS proporcionam flexibilidade e agilidade para o administrador de sistema.

## <a name="in-this-collection"></a>Nesta coleção
Os seguintes guias para planejamento, implantação e gerenciamento do WSUS estão nesta coleção:

-   [Implantar o Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Gerenciar Atualizações usando o Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


