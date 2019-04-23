---
title: Configurar o Objeto de Política de Grupo de publicação de hash BranchCache
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6e9a338dfebfb1dfadb258bcbdfcc8d75bd3ea86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862957"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurar o Objeto de Política de Grupo de publicação de hash BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para configurar a publicação de hash do BranchCache objeto de diretiva de grupo (GPO) para que todos os servidores de arquivo que você adicionou à sua UO têm a mesma configuração de diretiva de publicação de hash aplicada a eles.  
  
A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
> [!NOTE]  
> Antes de executar este procedimento, você deve criar unidade organizacional do servidores de arquivos BranchCache, mova os servidores de arquivos para a UO e crie o GPO de publicação de hash do BranchCache. Para obter mais informações, consulte [habilitar a publicação de Hash para servidores de arquivos do membro de domínio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Para configurar a publicação de hash do BranchCache objeto de diretiva de grupo  
  
1.  Executar o Windows PowerShell como administrador, digite **mmc**, e pressione ENTER. O Console de Gerenciamento Microsoft (MMC) é aberto.  
  
2.  No MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.  
  
3.  Em **Adicionar ou Remover Snap-ins**, em **Snap-ins disponíveis**, clique duas vezes em **Gerenciamento de Diretiva de Grupo** e clique em **OK**.  
  
4.  No MMC Gerenciamento de Diretiva de Grupo, expanda o caminho para o GPO de publicação de hash do BranchCache criado anteriormente. Por exemplo, se a sua floresta for denominada exemplo.com, seu domínio for denominado exemplo1.com e seu GPO for denominado **Publicação de Hash do BranchCache**, expanda o seguinte caminho: **Gerenciamento de diretiva de grupo**, **floresta: exemplo.com**, **domínios**, **exemplo1.com**, **objetos de diretiva de grupo**,  **Publicação de Hash do BranchCache**.  
  
5.  Clique com o botão direito do mouse no GPO **Publicação de Hash do BranchCache** e clique em **Editar**. O console do Editor de gerenciamento de diretiva de grupo é aberto.  
  
6.  No console do Editor de gerenciamento de diretiva de grupo, expanda o seguinte caminho: **Configuração do Computador**, **Diretivas**, **Modelos Administrativos**, **Rede**, **Servidor Lanman**.  
  
7.  No console do Editor de Gerenciamento de Diretiva de Grupo, clique em Servidor **Lanman**. No painel de detalhes, clique duas vezes em **Publicação de Hash para BranchCache**. A caixa de diálogo **Publicação de Hash para BranchCache** é aberta.  
  
8.  Na caixa de diálogo **Publicação de Hash para BranchCache**, clique em **Habilitado**.  
  
9. Na **opções**, clique em **permitir a publicação de hash para todas as pastas compartilhadas**e, em seguida, clique em um dos seguintes:  
  
    1.  Para habilitar a publicação de hash para todas as pastas compartilhadas para todos os servidores de arquivos que você adicionou à UO, clique em **permitir a publicação de hash para todas as pastas compartilhadas**.  
  
    2.  Para habilitar a publicação de hash somente para as pastas compartilhadas nas quais o BranchCache está habilitado, clique em **Permitir a publicação de hash somente em pastas compartilhadas nas quais o BranchCache está habilitado**.  
  
    3.  Para não permitir a publicação de hash para todas as pastas compartilhadas no computador, mesmo que o BranchCache esteja habilitado nos compartilhamentos de arquivos, clique em **Impedir a publicação de hash em todas as pastas compartilhadas**.  
  
10. Clique em **OK**.  
  
> [!NOTE]  
> Na maioria dos casos, é necessário salvar o console MMC e atualizar a exibição para exibir as alterações de configuração feitas.  
  


