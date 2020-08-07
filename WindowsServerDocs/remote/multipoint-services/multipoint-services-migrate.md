---
title: Migrar para os serviços do MultiPoint no Windows Server 2016
description: Saiba como migrar de uma versão anterior dos serviços do MultiPoint
ms.date: 07/29/2016
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: ce5fb28f92808d736f66f1f900228aac09d98bf5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955333"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Migração de serviços do MultiPoint no Windows Server 2016
>Aplica-se a: Windows Server 2016

Você pode migrar de uma versão anterior do Windows Server 2016 MultiPoint Services para a versão RTM dos serviços do MultiPoint. As informações a seguir fornecem informações de preparação e etapas de migração e verificação.

Documentação e ferramentas de migração facilitam a migração de dados e configurações de função de servidor de um servidor existente para um servidor de destino que esteja executando o Windows Server 2016. Utilizando o processo descrito neste guia, você pode simplificar o processo de migração, reduzir o tempo da migração, aumentar a precisão do processo e ajudar a eliminar possíveis conflitos que, de outro modo, poderiam ocorrer durante a migração.

## <a name="what-to-know-before-you-begin"></a>O que saber antes de começar
Antes de começar o processo de migração, observe o seguinte:

- O processo de migração não coleta automaticamente ou registra configurações para aplicativos na função de serviços do MultiPoint. Você deve criar um plano de migração personalizado para todos os aplicativos que deseja migrar. Isso também é verdadeiro ao usar o recurso de áreas de trabalho virtuais no MultiPoint Services.
- Este guia não fornece orientação para mover dados salvos em pastas de usuário ou compartilhadas no servidor MultiPoint. Isso se aplica a estações regulares e estações de área de trabalho virtuais.
- Este guia não contém instruções sobre como migrar quando o servidor de origem estiver executando várias funções. Se o servidor estiver executando várias funções, você precisará criar um procedimento de migração personalizado específico para seu ambiente de servidor, com base nas informações fornecidas nos guias de migração de função.
- Este guia não contém informações para migrar Serviços de Área de Trabalho Remota CALS. Para obter essas informações, consulte [migrar serviços de área de trabalho remota licenças de acesso para cliente (RDS CALs)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11)).

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Cenários de migração com suporte para os serviços do MultiPoint no Windows Server 2016
Os serviços de função de serviço do MultiPoint estão disponíveis no Windows Server 2016 Standard e no datacenter. Este guia de migração descreve como migrar os serviços de função dos serviços do MultiPoint de um servidor de origem que executa o Windows Server 2016 para um servidor de destino que executa a mesma versão.

## <a name="scenarios-that-are-not-supported"></a>Cenários sem suporte

Não há suporte para os cenários de migração a seguir:

- Migrando ou atualizando do Windows MultiPoint Server 2012 e 2011.
- Migrar de um servidor de origem para um servidor de destino que está sendo executado no sistema operacional com um idioma de interface do usuário do sistema diferente instalado.
- Migrando o serviço de função dos serviços do MultiPoint de servidores físicos para máquinas virtuais.
- Migrando quaisquer aplicativos ou configurações de aplicativo do MultiPoint Server.

## <a name="the-impact-of-migration-on-multipoint-services"></a>O impacto da migração nos serviços do MultiPoint
Lembre-se de que a função de serviços do MultiPoint não estará disponível durante a migração. Planeje a migração de dados para ocorrer fora dos horários de pico, a fim de minimizar o tempo de inatividade e reduzir o impacto para os usuários. Avise os usuários que os recursos estarão indisponíveis durante esses horários.

## <a name="migration-information-and-steps"></a>Etapas e informações de migração
Use as informações a seguir para planejar e realizar a migração dos serviços do MultiPoint:

- [Reúna as informações de que você precisa para a migração.](multipoint-services-migration-preparation.md)
- [Migre o serviço de função dos serviços do MultiPoint.](multipoint-services-migration-steps.md)
- [Validar a migração e executar tarefas de limpeza após a migração](multipoint-services-post-migration-steps.md)
