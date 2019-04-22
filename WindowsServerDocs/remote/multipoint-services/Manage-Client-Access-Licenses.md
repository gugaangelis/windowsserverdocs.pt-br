---
title: Gerenciar licenças de acesso para cliente
description: Saiba como trabalhar com as CALs no MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 744c2d7ff2965474b90686f88c21f7e6d87deced
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813657"
---
# <a name="manage-client-access-licenses"></a>Gerenciar licenças de acesso para cliente
Cada estação que se conecta a um sistema MultiPoint Services, incluindo o computador executando o MultiPoint Services que é usado como uma estação, deve ter uma área de trabalho remota por usuário válido *licença de acesso para cliente (CAL)*.

Se você estiver usando áreas de trabalho virtuais estação em vez de estações físicas, você deve instalar uma CAL para cada área de trabalho virtual estação.  
  
1.  Adquira uma licença de cliente para cada estação que está conectada ao seu servidor ou computador do MultiPoint Services. Para obter mais informações sobre como adquirir CALs, visite a documentação para o licenciamento de área de trabalho remota. <!--@Liza: add link to RDS licensing here-->

2.  Dos **inicie** tela, abra **MultiPoint Manager**.  
  
3.  Clique o **página inicial** guia e, em seguida, clique em **adicionar licenças de acesso para cliente**.  Isso abrirá a ferramenta de gerenciamento para o licenciamento CAL.

# <a name="set-the-licensing-mode-manually"></a>Definir o modo de licenciamento manualmente
Se não estiver configurado corretamente a configuração do MultiPoint Services solicitará que com uma notificação sobre o período de carência está expirando. Siga estas etapas para definir o modo de licenciamento:

1. Inicie **Editor de diretiva de Grupo Local** (gpedit. msc).

2. No painel esquerdo, navegue até **política do computador Local -> Configuração do computador - > modelos administrativos -> Windows Components -> remoto dos serviços de área de trabalho - > Host da sessão da área de trabalho remota -> licenciamento**.

3. No painel direito, clique com botão direito **usar os servidores de licenças de área de trabalho remota especificados** e selecione **editar**:
  - No diálogo de editor de diretiva de grupo, selecione **habilitado**
  - Insira o nome do computador local na **licenciar servidores para usar** campo.
  - Selecione **Okey**
  
4. No painel direito, clique com botão direito **definir o modo de licenciamento de área de trabalho remota** e selecione **editar**
 - No diálogo de editor de diretiva de grupo, selecione **habilitado**
 - Defina as **Licensing mode** para por dispositivo / usuário
 - Selecione **Okey** 

  
## <a name="see-also"></a>Consulte também  
[Gerenciar tarefas de sistema usando o MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)
