---
title: Criar o Objeto de Política de Grupo de publicação de hash BranchCache
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.topic: get-started-article
ms.assetid: c3d33bed-83ef-4eb8-acf9-0719ecb4a931
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ade42b3ff690e3c813c750609fb0b21b9999a85f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971873"
---
# <a name="create-the-branchcache-hash-publication-group-policy-object"></a>Criar o Objeto de Política de Grupo de publicação de hash BranchCache

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para criar a publicação de hash do BranchCache Política de Grupo objeto (GPO).

A associação a **Adminis. do Domínio** ou equivalente é o requisito mínimo para a execução deste procedimento.

> [!NOTE]
> Antes de executar este procedimento, crie a unidade organizacional de servidores de arquivos BranchCache e mova os servidores de arquivos para a UO. Para obter mais informações, consulte [habilitar a publicação de hash para servidores de arquivos do membro do domínio](../../branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md).

### <a name="to-create-the-branchcache-hash-publication-group-policy-object"></a>Para criar a publicação de hash do BranchCache Política de Grupo objeto

1.  Abra o Windows PowerShell, digite **mmc** e pressione ENTER. O Console de Gerenciamento Microsoft (MMC) é aberto.

2.  No MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.

3.  Em **Adicionar ou Remover Snap-ins**, em **Snap-ins disponíveis**, clique duas vezes em **Gerenciamento de Diretiva de Grupo** e clique em **OK**.

4.  No MMC do Gerenciamento de Diretiva de Grupo, expanda o caminho para a UO dos servidores de arquivos BranchCache criada anteriormente. Por exemplo, se sua floresta se chama example.com, seu domínio é denominado example1.com e sua UO é denominada servidores de arquivos do BranchCache, expanda o seguinte caminho: **política de grupo Management**, **Forest: example.com**, **domínios**, **example1.com**, **servidores de arquivos do BranchCache**.

5.  Clique com o botão direito do mouse em **servidores de arquivos do BranchCache**e clique em **criar um GPO nesse domínio e vincule-o aqui**. A caixa de diálogo **Novo GPO** é aberta. Em **nome**, digite um nome para o novo GPO. Por exemplo, se desejar denominar o objeto como Publicação de Hash do BranchCache, digite **Publicação de Hash do BranchCache**. Clique em **OK**.



