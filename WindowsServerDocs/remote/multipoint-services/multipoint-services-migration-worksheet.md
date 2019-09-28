---
title: Planilha de planejamento para migração de serviços do MultiPoint
description: Fornece planilhas de planejamento para ajudá-lo a migrar para os serviços do MultiPoint no Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: d3d2ecca4062d28d210196d9191e08710eb731c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394633"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>Planilha de planejamento para migração de serviços do MultiPoint

>Aplica-se a: Windows Server 2016

Use as listas e tabelas a seguir para coletar as configurações necessárias durante a migração dos serviços do MultiPoint.

## <a name="source-server-settings"></a>Configurações do servidor de origem

Você pode encontrar as configurações do servidor na guia **início** do Gerenciador do MultiPoint. Coloque uma marca de seleção ao lado de cada configuração em uso no servidor de origem.

- Permitir que uma conta tenha várias sessões.
- Permitir que este computador seja gerenciado remotamente.
- Permitir o monitoramento das áreas de trabalho deste computador.
- Sempre iniciar no modo de console.
- Não mostrar notificação de privacidade no primeiro logon do usuário.
- Atribua um IP exclusivo a cada estação.
- Permitir mensagens instantâneas entre o painel do MultiPoint e as sessões de usuário neste computador.
- Permitir orquestração de sessões de usuário do painel do MultiPoint e do administrador.
- Permitir que as estações usem a renderização de hardware de GPU.

## <a name="managed-servers-and-computers"></a>Computadores e servidores gerenciados

Registre os nomes dos servidores e computadores gerenciados. Você pode encontrar essas informações na guia **página inicial** do MultiPoint Manager.

| Computer | Nome do computador |
|----------|---------------|
| 1        |               |
| 2        |               |
| 3        |               |
| 4        |               |
| 5        |               |
| 6        |               |
| 7        |               |
| 8        |               |
| 9        |               |
| 10       |               |


## <a name="stations"></a>Rádio

Registre as estações locais e suas configurações. Você pode encontrar essas informações na guia **estações** no MultiPoint Manager.

| #  | Nome da estação | Conta de usuário de logon automático | Orientação de exibição |
|----|--------------|-------------------------|---------------------|
| 1  |              |                         |                     |
| 2  |              |                         |                     |
| 3  |              |                         |                     |
| 4  |              |                         |                     |
| 5  |              |                         |                     |
| 6  |              |                         |                     |
| 7  |              |                         |                     |
| 8  |              |                         |                     |
| 9  |              |                         |                     |
| 10 |              |                         |                     |

## <a name="administrators-and-multipoint-dashboard-users"></a>Administradores e usuários do painel do MultiPoint

Copie os nomes de usuário para os administradores e os usuários do painel do MultiPoint. Você pode encontrar essas informações na guia **usuários** no MultiPoint Manager.

Administradores:

- Nome do usuário:
- Nome do usuário:
- Nome do usuário:
- Nome do usuário:
- Nome do usuário:
- Nome do usuário:

Usuários do painel:

- Nome do usuário:
- Nome do usuário:
- Nome do usuário:
- Nome do usuário:
- Nome do usuário:

## <a name="vdi-template-and-virtual-desktops"></a>Modelo de VDI e áreas de trabalho virtuais

Registre as informações do modelo de VDI e os nomes das áreas de trabalho virtuais na implantação dos serviços do MultiPoint. Você pode encontrar essas informações na guia **áreas de trabalho virtuais** no MultiPoint Manager.

**Local do modelo de VDI**: 

| # | Nome da área de trabalho virtual      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |