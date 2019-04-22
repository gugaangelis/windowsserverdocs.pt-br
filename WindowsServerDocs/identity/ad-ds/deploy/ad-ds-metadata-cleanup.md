---
title: Limpeza de metadados do servidor AD DS
description: Use as ferramentas internas para limpar os metadados de controladores de domínio removidos
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fbb6e720c9289c608d71d3c36695ba623a9df5f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818077"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>Limpar os metadados do servidor de controlador de domínio do Active Directory

Aplica-se a: Windows Server

Limpeza de metadados é um procedimento obrigatório após uma remoção forçada dos serviços de domínio Active Directory (AD DS). Você pode executar a limpeza de metadados em um controlador de domínio no domínio do controlador de domínio que você removeu à força. Limpeza de metadados remove dados do AD DS que identifica um controlador de domínio para o sistema de replicação. Limpeza de metadados também remove as conexões de replicação FRS (serviço) e a replicação de sistema de arquivos distribuído (DFS) e tenta executar ou transferir funções de mestre (também conhecidas como operações de mestre único ou FSMO) quaisquer operações que o domínio obsoleto controlador mantém.

Há três opções para limpar os metadados do servidor:

- Limpeza de metadados do servidor usando as ferramentas de GUI
- Limpeza de metadados do servidor usando a linha de comando
- Limpeza de metadados do servidor usando um script

> [!NOTE]
> Se você receber um erro "Acesso negado" quando você usa qualquer um desses métodos para executar a limpeza de metadados, certifique-se de que o objeto de computador e o objeto de configurações NTDS para o controlador de domínio não estão protegidos contra exclusão acidental. Para verificar se esse botão direito do mouse, o objeto de computador ou o objeto de configurações NTDS, clique em **propriedades**, clique em **objeto**e desmarque os **proteger objeto contra exclusão acidental** caixa de seleção. Em usuários do Active Directory e computadores, o **objeto** guia de um objeto será exibida se você clicar em **exibição** e, em seguida, clique em **recursos avançados**.

## <a name="clean-up-server-metadata-using-gui-tools"></a>Limpeza de metadados do servidor usando as ferramentas de GUI

Quando você usa ferramentas de administração de servidor remoto (RSAT) ou os usuários do Active Directory e o console de computadores (DSA. msc) que está incluído no Windows Server para excluir uma conta de computador do controlador de domínio da unidade organizacional (UO), controladores de domínio do Limpeza de metadados do servidor é executada automaticamente. Antes do Windows Server 2008, era necessário executar um procedimento de limpeza de metadados separados.

Você também pode usar o serviços e Sites do Active Directory console (Dssite. msc) para excluir a conta de computador do controlador de domínio, que também é automaticamente concluída a limpeza de metadados. No entanto, serviços e Sites do Active Directory remove os metadados automaticamente apenas quando você primeiro excluir o objeto de configurações NTDS abaixo a conta de computador no Dssite. msc.

Enquanto você estiver usando o Windows Server 2008 ou versões mais recentes do RSAT do DSA. msc ou Dssite. msc, você pode limpar metadados automaticamente para os controladores de domínio que executam versões anteriores dos sistemas de operacionais do Windows.

Associação na **Admins. do domínio**, ou equivalente, é o mínimo necessário para concluir esses procedimentos.

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>Limpeza de metadados do servidor usando o Active Directory Users and Computers

1. Abra **Usuários e Computadores do Active Directory**.
2. Se você tiver identificado os parceiros de replicação em preparação para esse procedimento e se você não estiver conectado a um parceiro de replicação do controlador de domínio removido cujos metadados que você está limpando, clique com botão direito **usuários do Active Directory e Computadores** nó e, em seguida, clique **Alterar controlador de domínio**. Clique no nome do controlador de domínio do qual você deseja remover os metadados e, em seguida, clique em **Okey**.
3. Expanda o domínio do controlador de domínio que foi removido de modo forçado e, em seguida, clique em **controladores de domínio**.
4. No painel de detalhes, clique com botão direito do objeto de computador do controlador de domínio cujos metadados você deseja limpar e, em seguida, clique em **excluir**.
5. No **serviços de domínio do Active Directory** diálogo caixa, confirme o nome do controlador de domínio que você deseja excluir é mostrado e clique em **Sim** para confirmar a exclusão do objeto de computador.
6. No **controlador de domínio excluindo** caixa de diálogo, selecione **este controlador de domínio está permanentemente offline e não pode ser rebaixado usando o Active Directory domínio serviços de Assistente para instalação (DCPROMO)** e, em seguida, clique em **excluir**.
7. Se o controlador de domínio é um servidor de catálogo global, na **excluir o controlador de domínio** caixa de diálogo, clique em **Sim** para continuar com a exclusão.
8. Se o controlador de domínio no momento, contém uma ou mais operações de funções de mestre, clique em **Okey** para mover a função ou funções para o controlador de domínio que é mostrado. Você não pode alterar esse controlador de domínio. Se você quiser mover a função para outro controlador de domínio, você deve mover a função depois de concluir o procedimento de limpeza de metadados do servidor.

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>Limpeza de metadados do servidor usando os serviços e Sites do Active Directory

1. Abra Serviços e sites do Active Directory.
2. Se você tiver identificado os parceiros de replicação em preparação para esse procedimento e se você não estiver conectado a um parceiro de replicação do controlador de domínio removido cujos metadados que você está limpando, clique com botão direito **serviços e Sites do Active Directory** e, em seguida, clique em **Alterar controlador de domínio**. Clique no nome do controlador de domínio do qual você deseja remover os metadados e, em seguida, clique em **Okey**.
3. Expanda o site do controlador de domínio que foi removido de modo forçado, expanda **servidores**, expanda o nome do controlador de domínio, clique com botão direito no objeto Configurações NTDS e, em seguida, clique em **excluir**.
4. No **serviços e Sites do Active Directory** caixa de diálogo, clique em **Sim** para confirmar a exclusão de configurações NTDS.
5. No **controlador de domínio excluindo** caixa de diálogo, selecione **este controlador de domínio está permanentemente offline e não pode ser rebaixado usando o Active Directory domínio serviços de Assistente para instalação (DCPROMO)** e, em seguida, clique em **excluir**.
6. Se o controlador de domínio é um servidor de catálogo global, na **excluir o controlador de domínio** caixa de diálogo, clique em **Sim** para continuar com a exclusão.
7. Se o controlador de domínio no momento, contém uma ou mais operações de funções de mestre, clique em **Okey** para mover a função ou funções para o controlador de domínio que é mostrado.
8. O controlador de domínio que foi removido à força com o botão direito e, em seguida, clique em Excluir.
9. No **serviços de domínio do Active Directory** caixa de diálogo, clique em **Sim** para confirmar a exclusão de controlador de domínio.

## <a name="clean-up-server-metadata-using-the-command-line"></a>Limpeza de metadados do servidor usando a linha de comando

Como alternativa, você pode limpar metadados usando Ntdsutil.exe, uma ferramenta de linha de comando que é instalada automaticamente em todos os controladores de domínio e servidores que têm o Active Directory Lightweight Directory Services (AD LDS) instalado. Ntdsutil.exe também está disponível em computadores que têm as RSAT instaladas.

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>A limpeza dos metadados do servidor usando o Ntdsutil

1. Abra um prompt de comando como administrador: No menu **Iniciar**, clique com o botão direito do mouse em **Prompt de Comando** e clique em **Executar como administrador**. Se o **User Account Control** caixa de diálogo for exibida, forneça as credenciais de administrador corporativo se for necessário e, em seguida, clique em **continuar**.
2. No prompt de comando, digite o seguinte comando e pressione ENTER:

   `ntdsutil`

3. No prompt `ntdsutil:`, digite o seguinte comando e pressione ENTER:

   `metadata cleanup`

4. No prompt `metadata cleanup:`, digite o seguinte comando e pressione ENTER:

   `remove selected server <ServerName>`

5. Na **caixa de diálogo do servidor remover configuração**, revise as informações e o aviso e, em seguida, clique em **Sim** para remover o objeto de servidor e os metadados.

   Neste ponto, o Ntdsutil confirma que o controlador de domínio foi removido com êxito. Se você receber uma mensagem de erro que indica que o objeto não pode ser encontrado, o controlador de domínio pode ter sido removido anteriormente.

6. No `metadata cleanup:` e `ntdsutil:` prompts, digite `quit`, e pressione ENTER.

7. Para confirmar a remoção do controlador de domínio:

   Abra Usuários e computadores do Active Directory. No domínio do controlador de domínio removido, clique em **controladores de domínio**. No painel de detalhes, um objeto para o controlador de domínio que você removeu não deve aparecer.

   Abra Serviços e Sites do Active Directory. Navegue até a **servidores** contêiner e confirme que o objeto de servidor do controlador de domínio que você removeu não contém um objeto Configurações NTDS. Se nenhum objeto filho aparecem abaixo do objeto de servidor, você pode excluir o objeto de servidor. Se um objeto filho for exibida, não exclua o objeto de servidor porque outro aplicativo está usando o objeto.

## <a name="see-also"></a>Consulte também

* [O rebaixamento de controladores de domínio](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Referência de comandos Ntdsutil](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
