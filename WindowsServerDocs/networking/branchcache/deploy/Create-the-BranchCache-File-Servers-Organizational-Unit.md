---
title: Criar a unidade organizacional de servidores de arquivos BranchCache
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b7b26ec5808f5b11141e81dc5e738c83c94ef6b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874077"
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Criar a unidade organizacional de servidores de arquivos BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para criar uma UO (unidade organizacional) no AD DS (Serviços de Domínio Active Directory) para servidores de arquivos BranchCache.  
  
A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Para criar a unidade organizacional de servidores de arquivos BranchCache  
  
1.  Em um computador onde AD DS está instalado, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Active Directory Users and Computers**. O console de Usuários e Computadores do Active Directory é aberto.  
  
2.  No console de Usuários e Computadores do Active Directory, clique com o botão direito do mouse no domínio ao qual você deseja adicionar uma UO. Por exemplo, se o domínio for denominado exemplo.com, clique com o botão direito do mouse em **exemplo.com**. Aponte para **Novo** e clique em **Unidade Organizacional**. O **novo objeto – unidade organizacional** caixa de diálogo é aberta.  
  
3.  No **novo objeto – unidade organizacional** na caixa **nome**, digite um nome para a nova UO. Por exemplo, se você deseja nomear os servidores de arquivos BranchCache UO, digite **servidores de arquivos BranchCache**e, em seguida, clique em **Okey**.  
  


