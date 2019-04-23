---
title: Apresentando o Windows Server, versão 1709
description: Como obter, instalar e ativar
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 12/5/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: 96f098457b9bac4541421c7889aefb030e3d8804
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868797"
---
# <a name="introducing-windows-server-version-1709"></a>Apresentando o Windows Server, versão 1709

>Aplica-se a: Windows Server (canal semestral)

**Windows Server, versão 1709 é a primeira versão no novo canal semestral.** 

## <a name="what-the-semi-annual-channel-is--and-isnt"></a>O que o canal Semestral engloba
Como o primeiro lançamento neste novo canal, o Windows Server, versão 1709 *não* é uma "atualização" ou um "service pack" do Windows Server 2016. É o primeiro dos lançamentos semestrais de servidor em uma **faixa de novos lançamentos** para clientes que estejam mudando em "cadência de nuvem", como aqueles em ciclos de desenvolvimento rápido ou hosters acompanhando os investimentos mais recentes do Hyper-V. Cada versão neste roteiro terá suporte por 18 meses a partir do lançamento inicial. Para saber mais sobre o Canal Semestral, além de obter **dicas para decidir em qual canal ingressar** (ou em qual permanecer), confira [Visão geral do Canal Semestral](semi-annual-channel-overview.md).


**O produto do LTSC (Canal de Manutenção de Longo Prazo) atual é o Windows Server 2016**. O LTSC é o ideal se você precisar de estabilidade e a previsibilidade de longo prazo no sistema operacional de servidor para oferecer suporte a aplicativos e cargas de trabalho tradicionais. Se desejar permanecer no LTSC, você deverá instalar (ou continuar a usar) o Windows Server 2016, que pode ser instalado no modo Server Core ou Servidor com o modo de Experiência Desktop. Consulte [Introdução ao Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics) para obter mais detalhes.


## <a name="whats-different-about-1709"></a>Qual é a diferença para o 1709?

O Windows Server, versão 1709 é executado no modo Server Core. Isso significa que não há uma interface gráfica do usuário para gerenciamento remoto. No entanto, ele oferece vantagens, como requisitos de hardware menores, superfície de ataque muito menor e uma redução na necessidade de atualizações. Se estiver começando a trabalhar com o Server Core, [Gerenciar um servidor do Server Core](../administration/server-core/server-core-manage.md) ajudará você a se acostumar com esse ambiente. [Gerenciar o Windows Server 2016](../administration/manage-windows-server.md) mostra as várias opções para gerenciar servidores remotamente.

[Novidades no Windows Server versão 1709](whats-new-in-windows-server-1709.md) introduze você aos novos recursos e funcionalidades adicionadas ao Windows Server, versão 1709.

### <a name="why-does-windows-server-version-1709-offer-only-the-server-core-installation-option"></a>Por que o Windows Server, versão 1709 oferece somente a opção de instalação do Server Core?
Uma das etapas mais importantes no planejamento de cada versão do Windows Server é ouvir os comentários dos clientes, ou seja, como você está usando o Windows Server? Quais novos recursos afetarão as implantações do Windows Server e, consequentemente, suas atividades diárias? Seus comentários nos dizem que a disponibilização de inovações do modo mais rápido e eficiente possível é uma prioridade. Ao mesmo tempo, para os clientes que inovam mais rapidamente, fomos informados que você esta usando principalmente scripts de linha de comando com o PowerShell para gerenciar seus datacenters e, dessa forma não há uma grande necessidades por GUI de área de trabalho na instalação do Windows Server com Experiência Desktop. Ao se concentrar na opção de instalação do Server Core, podemos dedicar mais recursos para novas inovações, enquanto mantemos a funcionalidade e a compatibilidade de aplicativos tradicional da plataforma Windows Server. Caso tenha comentários sobre este ou outros problemas referentes ao Windows Server e às versões futuras, você pode fazer sugestões e comentários por meio do [Hub de Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).


### <a name="what-about-nano-server"></a>E quanto ao Nano Server?
O Nano Server está disponível como um sistema operacional de contêiner. Consulte [Mudanças no Nano Server no Canal semestral do Windows Server](nano-in-semi-annual-channel.md) para obter mais detalhes.

## <a name="additional-information-about-this-release"></a>Mais informações sobre esta versão
Para obter uma visão geral das principais informações do Windows Server, versão 1709, consulte estes tópicos antes de instalá-lo:

- Qual hardware é necessário para executá-lo? Consulte [Requisitos do sistema](system-requirements.md); os requisitos de sistema para esta versão são iguais aos do Windows Server 2016.
- Quais novos recursos e funcionalidades foram adicionadas? Consulte [Novidades no Windows Server versão 1709](whats-new-in-windows-server-1709.md)
- O que foi removido? Consulte [Recursos removido ou com substituição planejada a partir do Windows Server (versão 1709)](Removed-Features-1709.md)
- Quais problemas exclusivos desta versão precisam ser contornados? Consulte [Notas de versão: problemas importantes no Windows Server, versão 1709](server-1709-relnotes.md)


## <a name="where-to-obtain-windows-server-version-1709"></a>Onde obter o Windows Server, versão 1709

Esta versão deve ser instalada como uma instalação limpa.

- VLSC: Os clientes de licença de volume com [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) pode obter esta versão, vá para o [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) e clicando em **entrar**. Em seguida, clique em **Downloads e chaves** e procure por esta versão. 

- O Windows Server, versão 1709 também está disponível em [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Os participantes **assinaturas do Visual Studio:** Se você já participar nas assinaturas do Visual Studio, você pode obter o Windows Server, versão 1709 indo para o [página de download de assinante do Visual Studio](https://my.visualstudio.com/downloads?pid=2347) e concluir o download disponível lá. Caso ainda não seja um assinante, acesse [Assinaturas do Visual Studio](https://www.visualstudio.com/subscriptions/) para se inscrever e, em seguida, acesse a [página de download de Assinante do Visual Studio](https://my.visualstudio.com/downloads?pid=2347) como mostrado acima. As versões obtidas por meio de Assinaturas do Visual Studio destinam-se somente a desenvolvimento e teste.




## <a name="activating-windows-server-version-1709"></a>Ativar o Windows Server, versão 1709

- Se você obteve esta versão do Centro de serviços de licenciamento por volume, é possível ativá-lo usando a chave de ativação do Windows Server 2016.
- Se estiver usando o Microsoft Azure, esta versão deve ser ativada automaticamente.
- Se você obtiver essa versão em Assinaturas do Visual Studio, ele deve ser ativado automaticamente.