---
title: Editar uma zona DNS
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b632203289c3affd16735026e0c553be09c5e9e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847437"
---
# <a name="edit-a-dns-zone"></a>Editar uma zona DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para editar uma zona DNS no console de cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-edit-a-dns-zone"></a>Para editar uma zona DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**. Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  No painel de navegação inferior, faça uma das seguintes seleções:  
  
    -   Pesquisa direta  
  
    -   Pesquisa inversa IPv4  
  
    -   Pesquisa inversa IPv6  
  
4.  Por exemplo, selecione uma pesquisa inversa de IPv4.  
  
    ![Selecione um tipo de zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  No painel de exibição, clique com botão direito a zona que você deseja editar e, em seguida, clique em **zona de DNS de edição**.  
  
    ![Zona de DNS de edição](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  O **zona de DNS de edição** caixa de diálogo é aberta com o **geral** página selecionada. Se necessário, edite as propriedades de zona gerais: **Servidor DNS**, **categoria zona**, e **tipo de zona**e, em seguida, clique em **aplicar** ou, se as edições estiverem concluídas, **Okey**.  
  
    ![Editar propriedades de zona e salvar](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  No **zona de DNS de edição** caixa de diálogo, clique em **avançado**. O **avançado** página Propriedades de zona será aberta. Se necessário, edite as propriedades que você deseja alterar e, em seguida, clique em **Apply** ou, se as edições estiverem concluídas, **Okey**.  
  
    ![Editar propriedades de zona avançado](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Se necessário, selecione os nomes de página de propriedades de zona adicional (servidores de nomes, SOA, transferências de zona), faça as edições e clique em **Apply** ou **Okey**. Para examinar todas as suas edições de zona, clique em **resumo**e, em seguida, clique em **Okey**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de zonas DNS](DNS-Zone-Management.md)  
[Gerenciar o IPAM](Manage-IPAM.md)  
  


