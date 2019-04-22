---
title: Habilitar BranchCache em um compartilhamento de arquivos (opcional)
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
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
ms.openlocfilehash: 36d8379378529a94874c82e0aa90a6440f0281b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822227"
---
# <a name="enable-branchcache-on-a-file-share-optional"></a>Habilitar BranchCache em um compartilhamento de arquivos (opcional)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para habilitar o BranchCache em um compartilhamento de arquivos.  
  
> [!IMPORTANT]  
> Você não precisará executar este procedimento se você definir a configuração de publicação de hash com o valor **permitir a publicação de hash para todas as pastas compartilhadas**.  
  
A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
### <a name="to-enable-branchcache-on-a-file-share"></a>Para habilitar o BranchCache em um compartilhamento de arquivos  
  
1.  Abra o Windows PowerShell, digite **mmc**e pressione ENTER. O Console de Gerenciamento Microsoft (MMC) é aberto.  
  
2.  No MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.  
  
3.  Na **adicionar ou Remover Snap-ins**, na **snap-ins disponíveis**, clique duas vezes em **pastas compartilhadas**. O Assistente de pastas compartilhadas é aberto com o objeto de computador Local selecionado. Configurar o modo de exibição que você preferir, clique em **terminar**e, em seguida, clique em **Okey**.  
  
4.  Clique duas vezes em **(Local) de pastas compartilhadas**e, em seguida, clique em **compartilhamentos**.  
  
5.  No painel de detalhes, clique com botão direito um compartilhamento e, em seguida, clique em **propriedades**. O compartilhamento **propriedades** caixa de diálogo é aberta.  
  
6.  No **propriedades** caixa de diálogo do **gerais** , clique em **configurações off-line**. O **configurações off-line** caixa de diálogo é aberta.  
  
7.  Certifique-se de que **somente os arquivos e programas especificados pelos usuários ficam disponíveis offline** está selecionado e, em seguida, clique em **habilitar BranchCache**.  
  
8.  Clique em **OK** duas vezes.  
  

