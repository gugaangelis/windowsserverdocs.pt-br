---
title: Configurar o WEB1 para distribuir listas de certificados revogados (CRLs)
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 29c9cfb0a229200e1a62f0187e0bf277dbb99718
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969643"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurar o WEB1 para distribuir listas de certificados revogados (CRLs)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para configurar o servidor Web WEB1 para distribuir CRLs.

Nas extensões da CA raiz, foi mencionado que a CRL da CA raiz estaria disponível via https://pki.corp.contoso.com/pki . Atualmente, não há um diretório virtual PKI em WEB1, portanto, é necessário criá-lo.

Para executar esse procedimento, você deve ser membro de **Admins**. do domínio.

> [!NOTE]
> No procedimento abaixo, substitua o nome da conta de usuário, o nome do servidor Web, os nomes de pasta e os locais e outros valores que são apropriados para sua implantação.

#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Para configurar o WEB1 para distribuir certificados e CRLs

1.  No WEB1, execute o Windows PowerShell como administrador, digite `explorer c:\` e pressione Enter. O Windows Explorer é aberto para a unidade C.

2.  Crie uma nova pasta chamada PKI na unidade C:. Para fazer isso, clique em **início**e, em seguida, clique em **nova pasta**. Uma nova pasta é criada com o nome temporário realçado. Digite **PKI** e pressione Enter.

3.  No Windows Explorer, clique com o botão direito do mouse na pasta que você acabou de criar, passe o cursor do mouse sobre **compartilhar com**e clique em **pessoas específicas**. A caixa de diálogo **Compartilhamento de Arquivos** será aberta.

4.  Em **compartilhamento de arquivos**, digite **editores de certificado**e clique em **Adicionar**. O grupo Publicadores de certificados é adicionado à lista. Na lista, em **nível de permissão**, clique na seta ao lado de **editores de certificado**e, em seguida, clique em **leitura/gravação**. Clique em **compartilhar**e, em seguida, clique em **concluído**.

5.  Feche o Windows Explorer.

6.  Abra o console do IIS. No Gerenciador do Servidor, clique em **Ferramentas** e depois em **Gerenciador de Serviços de Informações da Internet (IIS)**.

7.  Na árvore de console do Gerenciador do Serviços de Informações da Internet (IIS), expanda **WEB1**. Se for convidado a começar a usar o Microsoft Web Platform, clique em **Cancelar**.

8.  Expanda **Sites**, clique com o botão direito do mouse no **Default Web Site** e clique em **Adicionar Diretório Virtual**.

9. Em **alias**, digite **PKI**. Em **caminho físico** , digite **C:\pki**e clique em **OK**.

10. Habilite o acesso anônimo ao diretório virtual PKI, para que qualquer cliente possa verificar a validade dos certificados e das CRLs da AC. Para fazer isso:

    1.  No painel **Conexões**, verifique se **pki** está selecionado.

    2.  Na **Página Inicial da pki** clique em **Autenticação**.

    3.  No painel **Ações**, clique em **Editar Permissões**.

    4.  Na guia **Segurança**, clique em **Editar**.

    5.  Na caixa de diálogo **Permissões para pki**, clique em **Adicionar**.

    6.  Em **Selecionar usuários, computadores, contas de serviço ou grupos**, digite **logon anônimo; Todos** e, em seguida, clique em **verificar nomes**. Clique em **OK**.

    7.  Clique em **OK** na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** .

    8.  Clique em **OK** na caixa de diálogo **permissões para PKI** .

11. Clique em **OK** na caixa de diálogo **Propriedades PKI** .

12. No painel **Página Inicial da pki**, clique duas vezes em **Filtragem de Solicitações**.

13. A guia **Extensões de Nome de Arquivo** é selecionada por padrão no painel **Filtragem de Solicitações**. No painel **Ações**, clique em **Editar Configurações de Recurso**.

14. Em **Editar Configurações de Filtragem de Solicitações**, selecione **Permitir saída dupla** e clique em **OK**.

15. No MMC do Gerenciador de Serviços de Informações da Internet (IIS), clique no nome do servidor Web. Por exemplo, se o servidor Web for denominado WEB1, clique em **WEB1**.

16. Em **ações**, clique em **reiniciar**. Os serviços de Internet são interrompidos e reiniciados.


