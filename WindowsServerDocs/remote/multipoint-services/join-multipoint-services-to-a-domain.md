---
title: Unir serviços do MultiPoint a um domínio (opcional)
Description: Fornece as etapas para unir os serviços do MultiPoint ao seu domínio
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 79bd9e594f94c7b3acd06265891dd646b3853b50
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394736"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Ingressar o computador dos serviços do MultiPoint em um domínio (opcional)
Se você for acessar o computador dos serviços do MultiPoint em um domínio Active Directory, a próxima etapa será adicionar o computador ao domínio.  
  
> [!IMPORTANT]  
> Você deve verificar seu fuso horário antes de ingressar o computador em um domínio. Para obter instruções, consulte [definir a data, a hora e o fuso horário](Set-the-date--time--and-time-zone.md).  
   
1.  Na tela **Inicial** , abra o **Painel de Controle**. Clique em **sistema e segurança**e em **sistema**.  
  
2.  Em **configurações de grupo de trabalho, o domínio e o nome do computador**, clique em **alterar configurações**.  
  
3.  Na guia **nome do computador** , clique em **alterar**.  
  
4.  Na caixa de diálogo **alterações no nome do computador/domínio** , selecione **domínio**, insira o nome do domínio e clique em **OK**e siga as etapas no Assistente para concluir o processo.  
  
5.  Depois que o computador for reiniciado, faça logon como administrador e aguarde a abertura do Gerenciador do MultiPoint.  
  
> [!IMPORTANT]  
> Para garantir que a implantação de domínio dos serviços do MultiPoint funcione corretamente, você precisará configurar algumas políticas de grupo e atualizar o registro. Para obter informações, consulte [Configurar políticas de grupo para uma implantação de domínio](https://technet.microsoft.com/library/dn265982.aspx).  