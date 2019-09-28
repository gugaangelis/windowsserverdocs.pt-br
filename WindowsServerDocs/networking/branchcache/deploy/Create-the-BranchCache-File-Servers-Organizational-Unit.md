---
title: Criar a unidade organizacional de servidores de arquivos BranchCache
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f058dbec42997f22106666b014508191a8861694
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406489"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Criar a unidade organizacional de servidores de arquivos BranchCache

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para criar uma UO (unidade organizacional) no AD DS (Serviços de Domínio Active Directory) para servidores de arquivos BranchCache.  
  
A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Para criar a unidade organizacional de servidores de arquivos BranchCache  
  
1.  Em um computador em que o AD DS está instalado, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **Active Directory usuários e computadores**. O console de Usuários e Computadores do Active Directory é aberto.  
  
2.  No console de Usuários e Computadores do Active Directory, clique com o botão direito do mouse no domínio ao qual você deseja adicionar uma UO. Por exemplo, se o domínio for denominado exemplo.com, clique com o botão direito do mouse em **exemplo.com**. Aponte para **Novo** e clique em **Unidade Organizacional**. A caixa de diálogo **novo objeto – unidade organizacional** é aberta.  
  
3.  Na caixa de diálogo **novo objeto – unidade organizacional** , em **nome**, digite um nome para a nova UO. Por exemplo, se você quiser nomear os servidores de arquivos do OU BranchCache de UO, digite **servidores de arquivos do BranchCache**e clique em **OK**.  
  


