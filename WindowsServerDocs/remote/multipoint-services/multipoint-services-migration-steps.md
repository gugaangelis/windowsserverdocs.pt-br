---
title: Etapas para migrar serviços do MultiPoint
description: Orienta você pelas etapas para migrar para os serviços do MultiPoint no Windows Server 2016
ms.date: 07/29/2016
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 0d76e3518801829b852c94d0b112b906abbd92c0
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948929"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>Migrar para os serviços do MultiPoint no Windows Server 2016

>Aplica-se a: Windows Server 2016

Use as etapas a seguir mais as informações coletadas na planilha de planejamento de migração para migrar para os serviços do MultiPoint no Windows Server 2016.

## <a name="transfer-server-settings"></a>Configurações do servidor de transferência
No servidor de destino, abra o Gerenciador do MultiPoint. Clique em **Editar configurações do servidor**. Aplique as configurações de acordo com a planilha de planejamento de migração.

> [!NOTE]
> Se você precisar habilitar a proteção de disco no servidor de destino, aguarde depois de configurar os serviços do MultiPoint.

## <a name="transfer-station-settings"></a>Configurações da estação de transferência
Verifique se as estações estão conectadas ao servidor de destino e todas estão mapeadas antes de aplicar as configurações de estação. As estações serão detectadas automaticamente. Siga as instruções em cada tela de estação para definir o mapeamento de servidor de estações de usuário e dispositivos USB conectados. Aplique as configurações de estação preferencial conforme descrito na planilha de planejamento de migração.

## <a name="migrate-the-vdi-template"></a>Migrar o modelo de VDI

Antes de importar o modelo de VDI do servidor de origem, habilite áreas de trabalho virtuais no servidor de destino usando o Gerenciador do MultiPoint:

1. Vá para a guia **áreas de trabalho virtuais** no Gerenciador do MultiPoint.
2. Clique em **habilitado áreas de trabalho virtuais**. O servidor instalará a função Hyper-V e reiniciará.
3. Abra o Gerenciador do MultiPoint e navegue de volta para **áreas de trabalho virtuais**.
4. Clique em **Importar modelo de área de trabalho virtual**. Siga as instruções para importar o modelo do servidor de origem.

> [!NOTE]
> Quando você importa um modelo de área de trabalho virtual, qualquer personalização aplicada ao modelo será redefinida.

## <a name="next-step"></a>Próxima etapa
[Valide sua nova implantação de serviços do MultiPoint.](multipoint-services-post-migration-steps.md)