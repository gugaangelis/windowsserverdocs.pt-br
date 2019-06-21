---
title: Definir escopo de acesso para registros de recurso DNS
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c79e1f63b9bcb43520a57defca8228b76db68a31
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283867"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Definir escopo de acesso para registros de recurso DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para definir o escopo de acesso para registros de recursos DNS usando o console de cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Definir escopo de acesso para registros de recurso DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, clique em **zonas DNS**.  No painel de navegação inferior, expanda **pesquisa direta** e navegue até e selecione a zona que contém os registros de recursos cujo escopo de acesso que você deseja alterar.  
  
3.  No painel de exibição, localize e selecione os registros de recursos cujo escopo de acesso que você deseja alterar.  
  
    ![Selecione os registros de recursos](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Os registros de recursos DNS selecionados com o botão direito e, em seguida, clique em **definir escopo de acesso**.  
  
    ![Definir Escopo de Acesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  O **definir escopo de acesso** caixa de diálogo é aberta. Se for necessário para sua implantação, clique para desmarcar **escopo de acesso herdado do pai**. Na **selecionar o escopo de acesso**, selecione um item e, em seguida, clique em **Okey**.  
  
    ![Selecione o escopo de acesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


