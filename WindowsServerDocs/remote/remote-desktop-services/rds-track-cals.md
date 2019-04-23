---
title: Acompanhar suas licenças de acesso de cliente dos serviços de área de trabalho remota (RDS CALs)
description: Saiba como controlar CALs em sua implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80d82d30-3ad0-4a8c-9a9b-2773c47eee19
author: lizap
ms.author: elizapo
ms.date: 05/11/2017
manager: dongill
ms.openlocfilehash: ed7490b2b61eb516d43b280b67fcefaeedb3e225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890617"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Acompanhar suas licenças de acesso de cliente dos serviços de área de trabalho remota (RDS CALs)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar a ferramenta Gerenciador de licenciamento de área de trabalho remota para criar relatórios para controlar o RDS por CALs de usuário que foram emitidos por um servidor de licenças de área de trabalho remota.

> [!NOTE]
>  Se você estiver usando o Azure AD Domain Services em seu ambiente, a ferramenta Gerenciador de licenciamento de área de trabalho remota não funcionará para obter CALs por usuário. Em vez disso, você precisa acompanhar o licenciamento manualmente, por meio de eventos de logon, a sondagem ativas conexões de área de trabalho remota por meio do agente de Conexão ou outro mecanismo que funciona para você. 

Use as seguintes etapas para gerar um relatório de CALs de usuário por:

1. No Gerenciador de licenciamento de área de trabalho remota com o botão direito no servidor de licença, clique em **criar relatório**e, em seguida, clique em **uso de CAL por usuário**.
2. Definir o escopo para o relatório – selecione um dos seguintes:
   - Domínio inteiro – o domínio no qual o servidor de licença é um membro.
   - Unidade organizacional - qualquer OU dentro do domínio no qual o servidor de licença é um membro.
   - Domínio inteiro e todos os domínios confiáveis - pode incluir domínios em outras florestas. Esta opção pode aumentar o tempo que leva para criar o relatório.

   A seleção feita determina quais contas de usuário no AD DS são pesquisadas para obter informações de CAL por usuário gerar o relatório.
3. Clique em **criar relatório**. O relatório é criado e será exibida uma mensagem para confirmar que o relatório foi criado com êxito. Clique em **OK** para fechar a mensagem.

O relatório que você criou aparece na seção de relatórios sob o nó para o servidor de licença. O relatório fornece as seguintes informações:

- Data e hora que o relatório foi criado
- O escopo do relatório (por exemplo, domínio, UO = Sales, ou todos os domínios confiáveis)
- O número de RDS por CALs de usuário que estão instalados no servidor de licença
- O número de RDS por CALs de usuário que foram emitidos pelo servidor de licenças específico para o escopo do relatório

Você também pode salvar o relatório como um arquivo CSV em uma pasta no computador. Para salvar o relatório, clique com botão direito no relatório que você deseja salvar, clique em Salvar como e, em seguida, especifique o nome do arquivo e o local para salvar o relatório.

Relatórios que você cria são listados no nó de relatórios sob o nó para o servidor de licença no Gerenciador de licenciamento de área de trabalho remota. Se você não precisa de um relatório, você pode excluí-lo.
