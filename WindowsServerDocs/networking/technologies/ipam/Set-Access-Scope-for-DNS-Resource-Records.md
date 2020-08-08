---
title: Definir escopo de acesso para registros de recurso DNS
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: lizross
author: eross-msft
ms.openlocfilehash: cccb2da0e88ce2db6e92514d89644d548461f7ed
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944708"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Definir escopo de acesso para registros de recurso DNS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para definir o escopo de acesso para registros de recursos DNS usando o console do cliente IPAM.

A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.

### <a name="to-set-access-scope-for-dns-resource-records"></a>Para definir o escopo de acesso para registros de recursos de DNS

1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.

2.  No painel de navegação, clique em **zonas DNS**.  No painel de navegação inferior, expanda **pesquisa direta** e navegue até e selecione a zona que contém os registros de recursos cujo escopo de acesso você deseja alterar.

3.  No painel de exibição, localize e selecione os registros de recursos cujo escopo de acesso você deseja alterar.

    ![Selecionar os registros de recursos](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)

4.  Clique com o botão direito do mouse nos registros de recursos DNS selecionados e clique em **definir escopo de acesso**.

    ![Definir Escopo de Acesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)

5.  A caixa de diálogo **definir escopo de acesso** é aberta. Se necessário para sua implantação, clique para anular a seleção **de herdar o escopo de acesso do pai**. Em **selecionar o escopo de acesso**, selecione um item e clique em **OK**.

    ![Selecionar o escopo de acesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)

## <a name="see-also"></a>Consulte Também
Controle de acesso [baseado em função](Role-based-Access-Control.md) 
 [Gerenciar IPAM](Manage-IPAM.md)



