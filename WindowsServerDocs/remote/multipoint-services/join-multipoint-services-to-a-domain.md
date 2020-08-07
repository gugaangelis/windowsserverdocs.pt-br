---
title: Unir serviços do MultiPoint a um domínio (opcional)
Description: Fornece as etapas para unir os serviços do MultiPoint ao seu domínio
ms.date: 07/22/2016
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 2956bd47bb2c4ac538b3b9828a6e5cdcef739eff
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970493"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Ingressar o computador dos serviços do MultiPoint em um domínio (opcional)
Se você for acessar o computador dos serviços do MultiPoint em um domínio Active Directory, a próxima etapa será adicionar o computador ao domínio.

> [!IMPORTANT]
> Você deve verificar seu fuso horário antes de ingressar o computador em um domínio. Para obter instruções, consulte [definir a data, a hora e o fuso horário](Set-the-date--time--and-time-zone.md).

1.  Na tela **Inicial**, abra o **Painel de Controle**. Clique em **sistema e segurança**e em **sistema**.

2.  Em **configurações de grupo de trabalho, o domínio e o nome do computador**, clique em **alterar configurações**.

3.  Na guia **nome do computador** , clique em **alterar**.

4.  Na caixa de diálogo **alterações no nome do computador/domínio** , selecione **domínio**, insira o nome do domínio e clique em **OK**e siga as etapas no Assistente para concluir o processo.

5.  Depois que o computador for reiniciado, faça logon como administrador e aguarde a abertura do Gerenciador do MultiPoint.

> [!IMPORTANT]
> Para garantir que a implantação de domínio dos serviços do MultiPoint funcione corretamente, você precisará configurar algumas políticas de grupo e atualizar o registro. Para obter informações, consulte [Configurar políticas de grupo para uma implantação de domínio](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265982(v=ws.11)).
