---
title: Limpar metadados do AD DS Server
description: Usar ferramentas internas para limpar metadados de controladores de domínio removidos
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 622ff33437a3aef14a185c9a4157dba68db0a2ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824779"
---
# <a name="clean-up-active-directory-domain-controller-server-metadata"></a>Limpar os metadados do servidor do controlador Domínio do Active Directory

Aplica-se a: Windows Server

A limpeza de metadados é um procedimento necessário após uma remoção forçada de Active Directory Domain Services (AD DS). Você executa a limpeza de metadados em um controlador de domínio no domínio do controlador de domínio que você removeu forçosamente. A limpeza de metadados remove dados de AD DS que identificam um controlador de domínio para o sistema de replicação. A limpeza de metadados também remove as conexões de replicação de FRS (serviço de replicação de arquivo) e de Sistema de Arquivos Distribuído (DFS) e tenta transferir ou executar todas as funções de mestre de operações (também conhecidas como operações de mestre único flexíveis ou FSMO) mantidas pelo controlador de domínio desativado.

Há três opções para limpar os metadados do servidor:

- Limpar os metadados do servidor usando ferramentas de GUI
- Limpar os metadados do servidor usando a linha de comando
- Limpar os metadados do servidor usando um script

> [!NOTE]
> Se você receber um erro "acesso negado" ao usar qualquer um desses métodos para executar a limpeza de metadados, verifique se o objeto de computador e o objeto de configurações NTDS do controlador de domínio não estão protegidos contra exclusão acidental. Para verificar isso, clique com o botão direito do mouse no objeto computador ou no objeto Configurações NTDS, clique em **Propriedades**, em **objeto**e desmarque a caixa de seleção **proteger objeto contra exclusão acidental** . Em Active Directory usuários e computadores, a guia **objeto** de um objeto será exibida se você clicar em **Exibir** e, em seguida, em **recursos avançados**.

## <a name="clean-up-server-metadata-using-gui-tools"></a>Limpar metadados do servidor usando ferramentas de GUI

Quando você usa o Ferramentas de Administração de Servidor Remoto (RSAT) ou o console de usuários e computadores Active Directory (DSA. msc) que está incluído no Windows Server para excluir uma conta de computador do controlador de domínio da unidade organizacional dos controladores de domínio (UO), a limpeza dos metadados do servidor é executada automaticamente. Antes do Windows Server 2008, era necessário executar um procedimento de limpeza de metadados separado.

Você também pode usar o console Active Directory sites e serviços (dssite. msc) para excluir uma conta de computador do controlador de domínio, o que também conclui a limpeza de metadados automaticamente. No entanto, Active Directory sites e serviços remove os metadados automaticamente somente quando você exclui primeiro o objeto de configurações NTDS abaixo da conta de computador em dssite. msc.

Contanto que você esteja usando o Windows Server 2008 ou versões mais recentes do RSAT do DSA. msc ou dssite. msc, você pode limpar os metadados automaticamente para controladores de domínio que executam versões anteriores dos sistemas operacionais Windows.

A associação em **Admins**. do domínio, ou equivalente, é o mínimo necessário para concluir esses procedimentos.

## <a name="clean-up-server-metadata-using-activedirectory-users-and-computers"></a>Limpar os metadados do servidor usando Active Directory usuários e computadores

1. Abra **Usuários e Computadores do Active Directory**.
2. Se você tiver identificado parceiros de replicação em preparação para este procedimento e se não estiver conectado a um parceiro de replicação do controlador de domínio removido cujos metadados você está limpando, clique com o botão direito do mouse **Active Directory nó usuários e computadores** e clique em **alterar controlador de domínio**. Clique no nome do controlador de domínio do qual você deseja remover os metadados e, em seguida, clique em **OK**.
3. Expanda o domínio do controlador de domínio que foi removida forçosamente e clique em **controladores de domínio**.
4. No painel de detalhes, clique com o botão direito do mouse no objeto de computador do controlador de domínio cujos metadados você deseja limpar e clique em **excluir**.
5. Na caixa de diálogo **Active Directory Domain Services** , confirme se o nome do controlador de domínio que você deseja excluir é mostrado e clique em **Sim** para confirmar a exclusão do objeto do computador.
6. Na caixa de diálogo **excluindo o controlador de domínio** , selecione **este controlador de domínio está permanentemente offline e não pode mais ser rebaixado usando o assistente para instalação do Active Directory Domain Services (Dcpromo)** e, em seguida, clique em **excluir**.
7. Se o controlador de domínio for um servidor de catálogo global, na caixa de diálogo **excluir controlador de domínio** , clique em **Sim** para continuar com a exclusão.
8. Se o controlador de domínio atualmente mantiver uma ou mais funções de mestre de operações, clique em **OK** para mover a função ou funções para o controlador de domínio que é mostrado. Você não pode alterar esse controlador de domínio. Se desejar mover a função para um controlador de domínio diferente, você deverá mover a função depois de concluir o procedimento de limpeza de metadados do servidor.

## <a name="clean-up-server-metadata-using-activedirectory-sites-and-services"></a>Limpar os metadados do servidor usando Active Directory sites e serviços

1. Abra Sites e Serviços do Active Directory.
2. Se você tiver identificado parceiros de replicação em preparação para esse procedimento e se não estiver conectado a um parceiro de replicação do controlador de domínio removido cujos metadados você está limpando, clique com o botão direito do mouse em **Active Directory sites e serviços**e clique em **alterar controlador de domínio**. Clique no nome do controlador de domínio do qual você deseja remover os metadados e, em seguida, clique em **OK**.
3. Expanda o site do controlador de domínio que foi removida forçosamente, expanda **servidores**, expanda o nome do controlador de domínio, clique com o botão direito do mouse no objeto Configurações NTDS e clique em **excluir**.
4. Na caixa de diálogo **Active Directory sites e serviços** , clique em **Sim** para confirmar a exclusão das configurações NTDS.
5. Na caixa de diálogo **excluindo o controlador de domínio** , selecione **este controlador de domínio está permanentemente offline e não pode mais ser rebaixado usando o assistente para instalação do Active Directory Domain Services (Dcpromo)** e, em seguida, clique em **excluir**.
6. Se o controlador de domínio for um servidor de catálogo global, na caixa de diálogo **excluir controlador de domínio** , clique em **Sim** para continuar com a exclusão.
7. Se o controlador de domínio atualmente mantiver uma ou mais funções de mestre de operações, clique em **OK** para mover a função ou funções para o controlador de domínio que é mostrado.
8. Clique com o botão direito do mouse no controlador de domínio que foi removido forçosamente e clique em excluir.
9. Na caixa de diálogo **Active Directory Domain Services** , clique em **Sim** para confirmar a exclusão do controlador de domínio.

## <a name="clean-up-server-metadata-using-the-command-line"></a>Limpar os metadados do servidor usando a linha de comando

Como alternativa, você pode limpar os metadados usando ntdsutil. exe, uma ferramenta de linha de comando que é instalada automaticamente em todos os controladores de domínio e servidores que têm o Active Directory Lightweight Directory Services (AD LDS) instalado. O Ntdsutil. exe também está disponível em computadores que têm o RSAT instalado.

## <a name="to-clean-up-server-metadata-by-using-ntdsutil"></a>Para limpar os metadados do servidor usando ntdsutil

1. Abra um prompt de comando como administrador: no menu **Iniciar** , clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**. Se a caixa de diálogo **controle de conta de usuário** for exibida, forneça credenciais de um administrador corporativo, se necessário, e clique em **continuar**.
2. No prompt de comando, digite o seguinte comando e pressione ENTER:

   `ntdsutil`

3. No prompt `ntdsutil:`, digite o seguinte comando e pressione ENTER:

   `metadata cleanup`

4. No prompt `metadata cleanup:`, digite o seguinte comando e pressione ENTER:

   `remove selected server <ServerName>`

5. Na **caixa de diálogo remover configuração do servidor**, examine as informações e o aviso e clique em **Sim** para remover o objeto de servidor e os metadados.

   Neste ponto, o Ntdsutil confirma que o controlador de domínio foi removido com êxito. Se você receber uma mensagem de erro indicando que o objeto não pode ser encontrado, o controlador de domínio poderá ter sido removido anteriormente.

6. No `metadata cleanup:` e `ntdsutil:` prompts, digite `quit`e pressione ENTER.

7. Para confirmar a remoção do controlador de domínio:

   Abra Active Directory usuários e computadores. No domínio do controlador de domínio removido, clique em **controladores de domínio**. No painel de detalhes, um objeto para o controlador de domínio que você removeu não deve aparecer.

   Abra Active Directory sites e serviços. Navegue até o contêiner **servidores** e confirme se o objeto de servidor para o controlador de domínio que você removeu não contém um objeto de configurações NTDS. Se nenhum dos objetos filho aparecer abaixo do objeto de servidor, você poderá excluir o objeto de servidor. Se um objeto filho for exibido, não exclua o objeto de servidor porque outro aplicativo está usando o objeto.

## <a name="see-also"></a>Consulte também

* [Como rebaixar controladores de domínio](Demoting-Domain-Controllers-and-Domains--Level-200-.md)
* [Referência de comando Ntdsutil](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753343(v=ws.10))
