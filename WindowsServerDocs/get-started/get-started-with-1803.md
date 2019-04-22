---
title: Apresentando o Windows Server, versão 1803
description: Como obter, instalar e ativar
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: high
ms.date: 05/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9cf87597-b15d-4f43-8aa1-91e60367f011
ms.openlocfilehash: c0a4917d0fdb3e911204601d6137d8c8a296e57a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812277"
---
# <a name="introducing-windows-server-version-1803"></a>Apresentando o Windows Server, versão 1803

>Aplica-se a: Windows Server (canal semestral)

**Windows Server, versão 1803 é a versão atual no novo canal semestral**


## <a name="what-the-semi-annual-channel-is--and-isnt"></a>O que o canal Semestral engloba
O Windows Server, versão 1803 *não* é uma "atualização" ou "service pack" para o Windows Server 2016. É a versão atual do servidor lançado duas vezes ao ano no roteiro desenvolvido para clientes que estejam mudando em "cadência de nuvem", como nos ciclos de desenvolvimento rápido. Este roteiro é ideal para cenários de aplicativos modernos e inovação como contêineres e microsserviços. Cada versão neste roteiro terá suporte por 18 meses a partir do lançamento inicial. Para saber mais sobre o Canal Semestral, além de obter **dicas para decidir em qual canal ingressar** (ou em qual permanecer), confira [Visão geral do Canal Semestral](semi-annual-channel-overview.md).


**Windows Server 2016 é o produto de canal de manutenção em longo prazo (LTSC) atual.** O LTSC é o ideal se você precisar de estabilidade e a previsibilidade de longo prazo no sistema operacional de servidor para oferecer suporte a aplicativos e cargas de trabalho tradicionais. Se desejar permanecer no LTSC, você deverá instalar (ou continuar a usar) o Windows Server 2016, que pode ser instalado no modo Server Core ou Servidor com o modo de Experiência Desktop. Consulte [Introdução ao Windows Server 2016](https://docs.microsoft.com/windows-server/get-started/server-basics) para obter mais detalhes.


## <a name="whats-different-about-windows-server-version-1803"></a>O que é diferente no Windows Server, versão 1803?

O Windows Server, versão 1803 é executado no modo Server Core. O modo Windows Server Core oferece excelentes vantagens, como requisitos de hardware menores, superfície de ataque muito menor e uma redução na necessidade de atualizações. Como ele tem nenhuma interface gráfica do usuário, o modo Windows Server Core é melhor gerenciado remotamente. Se estiver começando a trabalhar com o Server Core, [Gerenciar um servidor do Server Core](../administration/server-core/server-core-manage.md) ajudará você a se acostumar com esse ambiente. [Gerenciar o Windows Server 2016](../administration/manage-windows-server.md) mostra as várias opções para gerenciar servidores remotamente.

[Novidades no Windows Server versão 1803](whats-new-in-windows-server-1803.md) apresenta a você os novos recursos e funcionalidades adicionadas ao Windows Server, versão 1803.

### <a name="why-does-windows-server-version-1803-offer-only-the-server-core-installation-option"></a>Por que o Windows Server, versão 1803 oferece somente a opção de instalação do Server Core?
Uma das etapas mais importantes no planejamento de cada versão do Windows Server é ouvir os comentários dos clientes, ou seja, como você está usando o Windows Server? Quais novos recursos afetarão as implantações do Windows Server e, consequentemente, suas atividades diárias? Seus comentários nos dizem que a disponibilização de inovações do modo mais rápido e eficiente possível é uma prioridade. Ao mesmo tempo, para os clientes que inovam mais rapidamente, fomos informados que você esta usando principalmente scripts de linha de comando com o PowerShell para gerenciar seus datacenters e, dessa forma não há uma grande necessidades por GUI de área de trabalho na instalação do Windows Server com Experiência Desktop. Ao se concentrar na opção de instalação do Server Core, podemos dedicar mais recursos para novas inovações, enquanto mantemos a funcionalidade e a compatibilidade de aplicativos tradicional da plataforma Windows Server. Caso tenha comentários sobre este ou outros problemas referentes ao Windows Server e às versões futuras, você pode fazer sugestões e comentários por meio do [Hub de Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).


### <a name="what-about-nano-server"></a>E quanto ao Nano Server?
O Nano Server está disponível como um sistema operacional de contêiner. Consulte [Mudanças no Nano Server no Canal semestral do Windows Server](nano-in-semi-annual-channel.md) para obter mais detalhes.

## <a name="additional-information-about-this-release"></a>Mais informações sobre esta versão
Para obter uma visão geral das principais informações do Windows Server, versão 1803, consulte estes tópicos antes de instalá-lo:

- Qual hardware é necessário para executá-lo? Consulte [Requisitos do sistema](system-requirements.md); os requisitos de sistema para esta versão são iguais aos do Windows Server 2016.
- Quais novos recursos e funcionalidades foram adicionadas? Consulte [Novidades no Windows Server versão 1803](whats-new-in-windows-server-1803.md)
- O que foi removido? Consulte [Recursos removidos ou planejado para substituição no Windows Server, versão 1803](windows-server-1803-removed-features.md)
- Quais problemas exclusivos desta versão precisam ser contornados? Consulte [Notas de versão: problemas importantes no Windows Server, versão 1803](server-1803-release-notes.md)


## <a name="where-to-obtain-windows-server-version-1803"></a>Onde obter o Windows Server, versão 1803

Esta versão deve ser instalada como uma instalação limpa.

- Volume Licensing Service Center (VLSC): Os clientes de licença de volume com [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx) pode obter esta versão, vá para o [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx) e clicando em **entrar**. Em seguida, clique em **Downloads e chaves** e procure por esta versão. 

- O Windows Server, versão 1803 também está disponível no [Microsoft Azure](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Assinaturas do Visual Studio: Assinantes do Visual Studio pode obter o Windows Server, versão 1803 baixando-o do [página de download de assinante do Visual Studio](https://my.visualstudio.com/downloads?pid=2347). Caso ainda não seja um assinante, acesse [Assinaturas do Visual Studio](https://www.visualstudio.com/subscriptions/) para se inscrever e, em seguida, acesse a [página de download de Assinante do Visual Studio](https://my.visualstudio.com/downloads?pid=2347) como mostrado acima. As versões obtidas por meio de Assinaturas do Visual Studio destinam-se somente a desenvolvimento e teste.




## <a name="activating-windows-server-version-1803"></a>Ativar o Windows Server, versão 1803

- Se obteve esta versão pelo Centro de atendimento de licenciamento por volume, você poderá ativá-la usando o CSVLK do Windows Server 2016 com seu ambiente de Sistema de gerenciamento de chaves (KMS).
- Se estiver usando o Microsoft Azure, esta versão deve ser ativada automaticamente.
- Se obteve essa versão pelas assinaturas do Visual Studio, você pode ativá-la usando o CSVLK do Windows Server 2016 com seu ambiente de Sistema de gerenciamento de chaves (KMS). 
