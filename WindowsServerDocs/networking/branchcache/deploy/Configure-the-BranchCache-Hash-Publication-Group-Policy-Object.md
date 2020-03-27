---
title: Configurar o Objeto de Política de Grupo de publicação de hash BranchCache
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da74fea7-52b2-4d6d-9d21-19184eedbe3c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 660a4979aa5e29a6dd22cb2e0e6659966d077a8e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319336"
---
# <a name="configure-the-branchcache-hash-publication-group-policy-object"></a>Configurar o Objeto de Política de Grupo de publicação de hash BranchCache

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para configurar a publicação de hash do BranchCache Política de Grupo objeto (GPO) para que todos os servidores de arquivos que você adicionou à sua UO tenham a mesma configuração de política de publicação de hash aplicada a eles.  
  
A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
> [!NOTE]  
> Antes de executar esse procedimento, você deve criar a unidade organizacional de servidores de arquivos do BranchCache, mover os servidores de arquivos para a UO e criar o GPO de publicação de hash do BranchCache. Para obter mais informações, consulte [habilitar a publicação de hash para servidores de arquivos do membro do domínio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).  
  
### <a name="to-configure-the-branchcache-hash-publication-group-policy-object"></a>Para configurar a publicação de hash do BranchCache Política de Grupo objeto  
  
1.  Execute o Windows PowerShell como administrador, digite **MMC**e pressione Enter. O Console de Gerenciamento Microsoft (MMC) é aberto.  
  
2.  No MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.  
  
3.  Em **Adicionar ou Remover Snap-ins**, em **Snap-ins disponíveis**, clique duas vezes em **Gerenciamento de Diretiva de Grupo** e clique em **OK**.  
  
4.  No MMC Gerenciamento de Diretiva de Grupo, expanda o caminho para o GPO de publicação de hash do BranchCache criado anteriormente. Por exemplo, se sua floresta se chama example.com, seu domínio é denominado example1.com e seu GPO é chamado de **publicação de hash do BranchCache**, expanda o seguinte caminho: **Gerenciamento de política de grupo**, **floresta: example.com**, **domínios**, **example1.com**, **política de grupo objetos**, **publicação de hash do BranchCache**.  
  
5.  Clique com o botão direito do mouse no GPO **Publicação de Hash do BranchCache** e clique em **Editar**. O console do Editor de Gerenciamento de Política de Grupo é aberto.  
  
6.  No console do Editor de Gerenciamento de Política de Grupo, expanda o seguinte caminho **: configuração do computador**, **políticas**, **modelos administrativos**, **rede**, **servidor do LanMan**.  
  
7.  No console do Editor de Gerenciamento de Diretiva de Grupo, clique em Servidor **Lanman**. No painel de detalhes, clique duas vezes em **Publicação de Hash para BranchCache**. A caixa de diálogo **Publicação de Hash para BranchCache** é aberta.  
  
8.  Na caixa de diálogo **Publicação de Hash para BranchCache**, clique em **Habilitado**.  
  
9. Em **Opções**, clique em **Permitir publicação de hash para todas as pastas compartilhadas**e clique em uma das opções a seguir:  
  
    1.  Para habilitar a publicação de hash para todas as pastas compartilhadas de todos os servidores de arquivos que você adicionou à UO, clique em **Permitir publicação de hash para todas as pastas compartilhadas**.  
  
    2.  Para habilitar a publicação de hash somente para as pastas compartilhadas nas quais o BranchCache está habilitado, clique em **Permitir a publicação de hash somente em pastas compartilhadas nas quais o BranchCache está habilitado**.  
  
    3.  Para não permitir a publicação de hash para todas as pastas compartilhadas no computador, mesmo que o BranchCache esteja habilitado nos compartilhamentos de arquivos, clique em **Impedir a publicação de hash em todas as pastas compartilhadas**.  
  
10. Clique em **OK**.  
  
> [!NOTE]  
> Na maioria dos casos, é necessário salvar o console MMC e atualizar a exibição para exibir as alterações de configuração feitas.  
  


