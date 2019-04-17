---
title: Habilitar o BranchCache em um compartilhamento de arquivos (opcional)
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-bc
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9c465a9e-c504-44ec-9ebc-4e06ba54db30
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33ed40ef91d9389bb7940dcf928cba43f0c9dbd2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Habilitar o BranchCache em um compartilhamento de arquivos (opcional)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para habilitar o BranchCache em um compartilhamento de arquivos.  
  
> [!IMPORTANT]  
> Você não precisa executar este procedimento se você definir a configuração de publicação de hash com o valor **permitir a publicação de hash para todas as pastas compartilhadas**.  
  
A associação ao grupo **administradores**, ou equivalente é o requisito mínimo para executar este procedimento.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Para habilitar o BranchCache em um compartilhamento de arquivos  
  
1.  Abra o Windows PowerShell, tipo **mmc**, e pressione ENTER. O Console de gerenciamento Microsoft (MMC) é aberta.  
  
2.  No MMC, no **arquivo** menu, clique em **Adicionar/Remover Snap-in**. O **adicionar ou Remover Snap-ins** caixa de diálogo é aberta.  
  
3.  Em **adicionar ou Remover Snap-ins**, na **snap-ins disponíveis**, clique duas vezes em **pastas compartilhadas**. Abre o Assistente de pastas compartilhadas com o objeto de computador Local selecionado. Configurar o modo de exibição que você preferir, clique em **concluir**e clique em **Okey**.  
  
4.  Clique duas vezes em **pastas compartilhadas (Local)**e clique em **compartilhamentos**.  
  
5.  No painel de detalhes, clique com botão direito a um compartilhamento e clique em **propriedades**. O compartilhamento **propriedades** caixa de diálogo é aberta.  
  
6.  No **propriedades** caixa de diálogo, pela **geral**, clique em **configurações Offline**. O **configurações Offline** caixa de diálogo é aberta.  
  
7.  Certifique-se de que **somente os arquivos e programas que especificam os usuários estão disponíveis offline** está selecionado e clique em **BranchCache habilitar**.  
  
8.  Clique em **Okey** duas vezes.  
  

