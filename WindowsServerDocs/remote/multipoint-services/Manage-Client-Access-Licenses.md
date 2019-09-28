---
title: Gerenciar licenças de acesso para cliente
description: Saiba como trabalhar com CALs nos serviços do MultiPoint
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 675e089e-d841-401e-bba7-69f3929ef609
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 0ca951c5e4c4fcdba06d0b475a7d7536a9c7f91f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395459"
---
# <a name="manage-client-access-licenses"></a>Gerenciar licenças de acesso para cliente
Cada estação que se conecta a um sistema de serviços do MultiPoint, incluindo o computador que executa os serviços do MultiPoint que é usado como uma estação, deve ter uma *Cal (licença de acesso para cliente)* válida por usuário área de trabalho remota.

Se você estiver usando áreas de trabalho virtuais de estação em vez de estações físicas, deverá instalar uma CAL para cada área de trabalho virtual de estação.  
  
1.  Adquira uma licença de cliente para cada estação conectada ao computador ou servidor do MultiPoint Services. Para obter mais informações sobre como comprar CALs, visite a documentação para Área de Trabalho Remota licenciamento. 

2.  Na tela **Iniciar** , abra o **Gerenciador do MultiPoint**.  
  
3.  Clique na guia **início** e, em seguida, clique em **adicionar licenças de acesso para cliente**.  Isso abrirá a ferramenta de gerenciamento para licenciamento CAL.

# <a name="set-the-licensing-mode-manually"></a>Definir o modo de licenciamento manualmente
Se não estiver configurado corretamente, a configuração dos serviços do MultiPoint solicitará uma notificação sobre o período de carência expirado. Siga estas etapas para definir o modo de licenciamento:

1. Inicie o **Editor de política de grupo local** (gpedit. msc).

2. No painel esquerdo, navegue até **política de computador local – configuração do computador >-> modelos administrativos-> componentes do Windows-> serviços de área de trabalho remota-> licenciamento**host da sessão da área de trabalho remota.

3. No painel direito, clique com o botão direito do mouse em **usar os servidores de licença de área de trabalho remota especificados** e selecione **Editar**:
   - Na caixa de diálogo Editor de diretiva de grupo, selecione **habilitado**
   - Insira o nome do computador local no campo **servidores de licença a serem usados** .
   - Selecione **OK**
  
4. No painel direito, clique com o botão direito do mouse em **definir o modo de licenciamento área de trabalho remota** e selecione **Editar**
   - Na caixa de diálogo Editor de diretiva de grupo, selecione **habilitado**
   - Definir o **modo de licenciamento** como por dispositivo/por usuário
   - Selecione **OK** 

  
## <a name="see-also"></a>Consulte também  
[Gerenciar tarefas de sistema usando o MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)
