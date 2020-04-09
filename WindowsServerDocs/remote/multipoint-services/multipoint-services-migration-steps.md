---
title: Etapas para migrar serviços do MultiPoint
description: Orienta você pelas etapas para migrar para os serviços do MultiPoint no Windows Server 2016
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 3ee77efa-7cc5-4ddf-aaff-b5634a717014
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: f2e293fafb8d6f5d84e9ea5a4ad8ef3b7fe2ba7d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858689"
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

## <a name="next-step"></a>Próximas etapas
[Valide sua nova implantação de serviços do MultiPoint.](multipoint-services-post-migration-steps.md)