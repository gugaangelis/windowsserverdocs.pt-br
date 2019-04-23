---
title: Migrar para o MultiPoint Services no Windows Server 2016
description: Saiba como migrar de uma versão anterior do MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 24c35c31bf920c41bafa16901ee30a023565dad8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861197"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Migração do multiPoint Services no Windows Server 2016
>Aplica-se a: Windows Server 2016

Você pode migrar de uma versão anterior do Windows Server 2016 MultiPoint Services para a versão RTM do MultiPoint Services. As informações a seguir fornecem etapas de informações de migração e verificação de preparação.

Ferramentas e documentação de migração facilitam a migração das configurações de função de servidor e dados de um servidor existente para um servidor de destino que está executando o Windows Server 2016. Utilizando o processo descrito neste guia, você pode simplificar o processo de migração, reduzir o tempo da migração, aumentar a precisão do processo e ajudar a eliminar possíveis conflitos que, de outro modo, poderiam ocorrer durante a migração. 

## <a name="what-to-know-before-you-begin"></a>O que saber antes de começar
Antes de começar o processo de migração, observe o seguinte:

- O processo de migração não automaticamente coletar ou gravar configurações de aplicativos na função do MultiPoint Services. Você deve criar um plano de migração personalizado para todos os aplicativos que você deseja migrar. Isso também é verdadeiro ao usar o recurso de áreas de trabalho virtuais no MultiPoint Services.
- Este guia não fornece diretrizes para mover os dados salvos no usuário ou pastas compartilhadas no MultiPoint server. Isso se aplica a estações regulares e de área de trabalho virtual estações.
- Este guia contém instruções sobre como migrar quando o servidor de origem estiver executando várias funções. Se o servidor estiver executando várias funções, você precisa criar um procedimento de migração personalizado específico para seu ambiente de servidor, com base nas informações fornecidas nos guias de migração de função.
- Este guia não contém informações para migrar serviços de área de trabalho remotas CALS. Para obter essas informações, consulte [migrar Remote Desktop Services licenças de acesso cliente (RDS CALs)](https://technet.microsoft.com/library/dd851844.aspx).

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Cenários de migração com suporte do MultiPoint Services no Windows Server 2016
Os serviços de função de serviço do MultiPoint está disponível no Windows Server 2016 Standard e Datacenter. Este guia de migração descreve como migrar os serviços de função do Multipoint Services de um servidor de origem executando o Windows Server 2016 para um servidor de destino executando a mesma versão.

## <a name="scenarios-that-are-not-supported"></a>Cenários sem suporte

Não há suporte para os cenários de migração a seguir:

- Migração ou atualização do Windows MultiPoint Server 2012 e 2011.
- Migrando de um servidor de origem para um servidor de destino que está em execução no sistema operacional com um sistema diferente de idioma da interface do usuário instalado.
- Migrando o serviço de função do MultiPoint Services de servidores físicos para máquinas virtuais.
- Migrando todos os aplicativos ou configurações de aplicativo do MultiPoint Server.

## <a name="the-impact-of-migration-on-multipoint-services"></a>O impacto da migração no MultiPoint Services
Lembre-se de que a função do MultiPoint Services não estará disponível durante a migração. Planeje a migração de dados para ocorrer fora dos horários de pico, a fim de minimizar o tempo de inatividade e reduzir o impacto para os usuários. Avise os usuários que os recursos estarão indisponíveis durante esses horários.

## <a name="migration-information-and-steps"></a>Etapas e informações de migração
Use as informações a seguir para planejar e executar a migração do MultiPoint Services:

- [Colete as informações necessárias para a migração.](multipoint-services-migration-preparation.md)
- [Migre o serviço de função do MultiPoint Services.](multipoint-services-migration-steps.md)
- [Validar a migração e fazer quaisquer tarefas de limpeza após a migração](multipoint-services-post-migration-steps.md)