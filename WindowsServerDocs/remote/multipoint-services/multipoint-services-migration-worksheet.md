---
title: Planilha de planejamento para a migração do MultiPoint Services
description: Fornece as planilhas de planejamento para ajudá-lo a migrar para o MultiPoint Services no Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: a9d9b62bced9be90c658b79338c6f4ef07710fc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880577"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>Planilha de planejamento para a migração do MultiPoint Services

>Aplica-se a: Windows Server 2016

Use as listas e tabelas a seguir para coletar as configurações que necessárias durante a migração do MultiPoint Services.

## <a name="source-server-settings"></a>Configurações do servidor de origem

Você pode encontrar as configurações do servidor sobre o **Home** guia no Gerenciador do MultiPoint. Coloque uma marca de seleção ao lado de cada configuração em uso no servidor de origem.

- Permitir que uma conta tenha várias sessões.
- Permitir que este computador seja gerenciado remotamente.
- Permitir o monitoramento de áreas de trabalho deste computador.
- Sempre inicie no modo de console.
- Não mostre notificação de privacidade no primeiro logon do usuário.
- Atribua um IP exclusivo a cada estação.
- Permita mensagens Instantâneas entre o MultiPoint Dashboard e sessões de usuário neste computador.
- Permitir a orquestração de sessões de usuário do MultiPoint Dashboard e de administrador.
- Permite que as estações usam a renderização de hardware GPU.

## <a name="managed-servers-and-computers"></a>Computadores e servidores gerenciados

Registre os nomes dos servidores gerenciados e computadores. Você pode encontrar essas informações sobre o **Home** guia no Gerenciador do MultiPoint.

| Computador | Nome do computador |
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


## <a name="stations"></a>Estações

Registre estações locais e suas configurações. Você pode encontrar essas informações sobre o **estações** guia no Gerenciador do MultiPoint.

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

## <a name="administrators-and-multipoint-dashboard-users"></a>Os administradores e usuários do MultiPoint Dashboard

Copie os nomes de usuário para os administradores e usuários do MultiPoint Dashboard. Você pode encontrar essas informações sobre o **usuários** guia no Gerenciador do MultiPoint.

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

## <a name="vdi-template-and-virtual-desktops"></a>Modelo VDI e áreas de trabalho virtuais

Registre as informações do modelo VDI e os nomes das áreas de trabalho virtuais em sua implantação de MultiPoint Services. Você pode encontrar essas informações sobre o **áreas de trabalho virtuais** guia no Gerenciador do MultiPoint.

**Local do modelo de VDI**: 

| # | Nome da área de trabalho virtual      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |