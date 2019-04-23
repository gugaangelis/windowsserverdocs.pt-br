---
title: Etapas para a migração do MultiPoint Services
description: Orienta você pelas etapas para migrar para o MultiPoint Services no Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: b63ef4bf63ce990aa0b0ba7624905ba8f14dde98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854807"
---
# <a name="migrate-to--multipoint-services-in-windows-server-2016"></a>Migrar para o MultiPoint Services no Windows Server 2016

>Aplica-se a: Windows Server 2016

Use as seguintes etapas e as informações coletadas na planilha de planejamento de migração para migrar para o MultiPoint Services no Windows Server 2016.

## <a name="transfer-server-settings"></a>Transferir as configurações do servidor
No servidor de destino, abra o MultiPoint Manager. Clique em **editar configurações de servidor**. Aplica as configurações de acordo com a planilha de planejamento de migração.

> [!NOTE]
> Se você precisar habilitar a proteção de disco no servidor de destino, espere até depois de configurar o MultiPoint Services.

## <a name="transfer-station-settings"></a>Configurações da estação de transferência
Certifique-se de que as estações estão conectadas ao servidor de destino e todos os mapeado antes de aplicar as configurações de estação. As estações serão detectadas automaticamente. Siga as instruções na tela cada estação para definir o mapeamento do servidor de estações de usuário e dispositivos USB conectados. Aplica suas configurações preferenciais de estação, conforme descrito na planilha de planejamento de migração.

## <a name="migrate-the-vdi-template"></a>Migrar o modelo VDI

Antes de importar o modelo VDI do servidor de origem, habilitado áreas de trabalho virtuais no servidor de destino usando o MultiPoint Manager:

1. Vá para o **áreas de trabalho virtuais** guia no Gerenciador do MultiPoint.
2. Clique em **habilitado áreas de trabalho virtuais**. O servidor irá instalar a função Hyper-V e reinicie.
3. Abra o MultiPoint Manager e navegue até **áreas de trabalho virtuais**.
4. Clique em **Importar modelo de área de trabalho virtual**. Siga as instruções para importar o modelo do servidor de origem.

> [!NOTE]
> Quando você importa um modelo de área de trabalho virtual, qualquer personalização aplicada ao modelo será redefinida. 

## <a name="next-step"></a>Próximas etapas
[Valide a nova implantação de MultiPoint Services.](multipoint-services-post-migration-steps.md)