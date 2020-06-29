---
title: Conectar a um computador remoto
description: Este artigo descreve como se conectar a um computador remoto para gerenciar recursos de armazenamento do Gerenciador de Recursos de Servidor de Arquivos
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 562164b461b4cd5db939b116feeb1bf21f78bad4
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474123"
---
# <a name="connect-to-a-remote-computer"></a>Conectar a um computador remoto

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Para gerenciar recursos de armazenamento em um computador remoto, você pode conectar ao computador a partir do Gerenciador de Recursos de Servidor de Arquivos. Enquanto estiver conectado, o Gerenciador de Recursos de Servidor de Arquivos permite que você gerencie cotas, arquivos de triagem, gerencie classificações, agende tarefas de gerenciamento de arquivo e gere relatórios com esses recursos remotos.

> [!Note]
> O Gerenciador de recursos do servidor de arquivos pode gerenciar recursos no computador local ou em um computador remoto, mas não em ambos ao mesmo tempo.

## <a name="to-connect-to-a-remote-computer-from-file-server-resource-manager"></a>Para conectar a um computador remoto a partir de um Gerenciador de Recursos de Servidor de Arquivos

1.  Em **Ferramentas administrativas**, clique em **Gerenciador de Recursos de Servidor de Arquivos**.

2.  Na árvore de console, clique com o botão direito do mouse em **Gerenciador de Recursos de Servidor de Arquivos** e em **Conectar a outro computador**.

3.  Na caixa de diálogo **Conectar a outro computador**, clique em **Outro computador**. Digite o nome do servidor ao qual deseja se conectar (ou clique em **Procurar** para pesquisar um computador remoto).

4.  Clique em **OK**.

> [!Important]
> O comando **Conectar a outro computador** está disponível somente quando você abrir o Gerenciador de recursos do servidor de arquivos nas **Ferramentas administrativas**. Quando você acessar o Gerenciador de recursos do servidor de arquivos a partir do Gerenciador do servidor, o comando não estará disponível.

## <a name="additional-considerations"></a>Considerações adicionais

Para gerenciar recursos remotos com o Gerenciador de Recursos de Servidor de Arquivos:

-   Você deve estar conectado ao computador local com uma conta de domínio que é um membro do grupo **Administradores** no computador remoto.
-   O computador remoto deve estar executando o Windows Server e Gerenciador de recursos do servidor de arquivos deve estar instalado.
-   A exceção **Gerenciamento do Gerenciador de Recursos de Servidor de Arquivos remoto** no computador remoto deve ser habilitada. Habilite essa exceção usando o Firewall do Windows no Painel de Controle.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar recursos de armazenamento remoto](managing-remote-storage-resources.md)