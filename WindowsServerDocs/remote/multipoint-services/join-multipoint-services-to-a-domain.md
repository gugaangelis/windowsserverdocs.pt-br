---
title: Ingressar o MultiPoint Services em um domínio (opcional)
Description: Fornece as etapas para ingressar o MultiPoint Services em seu domínio
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: af5dd1f16e011161bbcf72c21c21088721ac1243
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827037"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Ingressar o computador do MultiPoint Services em um domínio (opcional)
Se você for acessar o computador do MultiPoint Services em um domínio do Active Directory, a próxima etapa é adicionar o computador ao domínio.  
  
> [!IMPORTANT]  
> Antes de ingressar o computador em um domínio, você deve verificar seu fuso horário. Para obter instruções, consulte [defina a data, hora e fuso horário](Set-the-date--time--and-time-zone.md).  
   
1.  Na tela **Inicial** , abra o **Painel de Controle**. Clique em **sistema e segurança**e, em seguida, clique em **sistema**.  
  
2.  Em **configurações de grupo de trabalho, o domínio e o nome do computador**, clique em **alterar configurações**.  
  
3.  Sobre o **nome do computador** , clique em **alteração**.  
  
4.  No **alterações de nome/domínio do computador** caixa de diálogo, selecione **domínio**, insira o nome do domínio e clique em **Okey**e, em seguida, siga as etapas no Assistente para concluir o processo.  
  
5.  Depois que o computador for reiniciado, faça logon como administrador e aguarde o Gerenciador do MultiPoint abrir.  
  
> [!IMPORTANT]  
> Para garantir que sua implantação do domínio do MultiPoint Services funcione corretamente, você precisará configurar duas políticas de grupo e atualizar o registro. Para obter informações, consulte [configurar políticas de grupo para uma implantação do domínio](https://technet.microsoft.com/library/dn265982.aspx).  