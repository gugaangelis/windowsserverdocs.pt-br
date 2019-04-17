---
title: Criar a unidade organizacional do arquivo BranchCache servidores
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 2cda192f-6b45-4e6c-88d9-70ca179ddb94
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a92cb3110e4aecb1ef09a45ed14173305722c655
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-the-branchcache-file-servers-organizational-unit"></a>Criar a unidade organizacional do arquivo BranchCache servidores

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para criar uma unidade organizacional (UO) nos serviços de domínio Active Directory (AD DS) BranchCache para servidores de arquivos.  
  
A associação ao grupo **Admins. do domínio**, ou equivalente é o requisito mínimo para executar este procedimento.  
  
### <a name="to-create-the-branchcache-file-servers-organizational-unit"></a>Para criar a unidade organizacional do BranchCache arquivo servidores  
  
1.  Em um computador onde AD DS é instalado, no Gerenciador do servidor, clique em **ferramentas**e clique em **usuários e Active Directory computadores**. O console de usuários e Active Directory computadores é aberto.  
  
2.  No console do Active Directory Users and Computers, clique com botão direito no domínio ao qual você deseja adicionar uma unidade Organizacional. Por exemplo, se o domínio for denominado example.com, clique com botão direito **example.com**. Aponte para **nova**e clique em **unidade organizacional**. O **novo objeto - unidade organizacional** caixa de diálogo é aberta.  
  
3.  No **novo objeto - unidade organizacional** na caixa **nome**, digite um nome para a nova UO. Por exemplo, se você quiser nomear os servidores de arquivos de UO BranchCache, digite **servidores de arquivos BranchCache**e clique em **Okey**.  
  


