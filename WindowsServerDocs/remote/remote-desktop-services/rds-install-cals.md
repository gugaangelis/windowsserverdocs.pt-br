---
title: Instalar licenças de acesso para cliente RDS
description: Saiba como instalar CALs para clientes de Área de Trabalho Remota.
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 848ca4ae9edd414173bbfd5822011b93d044e394
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961660"
---
# <a name="install-rds-client-access-licenses-on-the-remote-desktop-license-server"></a>Instalar licenças de acesso para cliente RDS no servidor de licenças de Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Use as informações a seguir para instalar CALs (Licenças de Acesso para Cliente) de Serviços de Área de Trabalho Remota no servidor de licenças. Quando as CALs forem instaladas, o servidor de licença emitirá elas aos usuários conforme apropriado.

Observe que é necessário ter conectividade com a Internet no computador executando o Gerenciador de Licenciamento de Área de Trabalho Remota, mas não no computador executando o servidor de licença.

1. No servidor de licença (geralmente o primeiro Agente de Conexão de Área de Trabalho Remota), abra o Gerenciador de Licenciamento de Área de Trabalho Remota.
2. Clique com o botão direito do mouse no servidor de licença e, em seguida, clique em **Instalar licenças**.
3. Clique em **Avançar** na página inicial.
4. Selecione o programa do qual você adquiriu as CALs para Serviços de Área de Trabalho Remota e, em seguida, clique em **Avançar**. Se você for um provedor de serviços, selecione **Contrato de Licença de Provedor de Serviços**.
5. Insira as informações do programa de licença. Na maioria dos casos, será o código de licença ou um número de contrato, mas isso varia de acordo com o programa de licença que você está usando.
6. Clique em **Avançar**.
7. Selecione a versão do produto, o tipo de licença e o número de licenças para o ambiente e, em seguida, clique em **Avançar**. O gerenciador de licenças entra em contato com a Microsoft Clearinghouse para validar e recuperar suas licenças.
8.  Clique em **Concluir** para concluir o processo.