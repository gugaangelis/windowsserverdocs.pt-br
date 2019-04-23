---
title: Instalar licenças de acesso de cliente RDS
description: Saiba como instalar CALs para clientes de área de trabalho remota.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 2f283b51acc869704a52f09bebc228660cdfbc38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870567"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>Instalar licenças de acesso para cliente RDS no servidor de licenças de área de trabalho remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Use as informações a seguir para instalar licenças de acesso para cliente dos serviços de área de trabalho remota (CALs) no servidor de licença. Depois que as CALs são instaladas, o servidor de licença emitirá-los aos usuários conforme apropriado.

Observe que você precisa de conectividade com a Internet no computador executando o Gerenciador de licenciamento de área de trabalho remota, mas não no computador executando o servidor de licença.

1. No servidor de licença (geralmente a primeira Conexão agente RD), abra o Gerenciador de licenciamento de área de trabalho remota.
2. Clique com botão direito no servidor de licença e, em seguida, clique em **instalar licenças**.
3. Clique em **próxima** na página de boas-vinda.
4. Selecione o programa você adquiriu as RDS CALs do e, em seguida, clique em **próxima**. Se você for um provedor de serviços, selecione **contrato de licença de provedor de serviços**.
5. Insira as informações para o seu programa de licença. Na maioria dos casos, esse será o código de licença ou um número de contrato, mas isso varia de acordo com o programa de licença que você está usando.
6. Clique em **Avançar**.
7. Selecione a versão do produto, o tipo de licença e o número de licenças para o seu ambiente e, em seguida, clique em **próxima**. O Gerenciador de licenças entra em contato com a Microsoft Clearinghouse para validar e recuperar suas licenças.
8.  Clique em **Concluir** para concluir o processo.