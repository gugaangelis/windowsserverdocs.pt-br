---
title: Configurar o WEB1 para distribuir as listas de certificados revogados (CRLs)
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 57fa45eff87a1f0cdaae8b780d7f605e54ff6871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839187"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurar o WEB1 para distribuir as listas de certificados revogados (CRLs)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para configurar o WEB1 para distribuir as CRLs de servidor web.  
  
Nas extensões da AC raiz, foi mencionado que a CRL da AC raiz deve estar disponível por meio de https://pki.corp.contoso.com/pki. Atualmente, não há um diretório virtual da PKI no WEB1, portanto, deve-se criar.  
  
Para executar este procedimento, você deve ser um membro da **Admins. do domínio**.  
  
> [!NOTE]  
> No procedimento a seguir, substitua o nome da conta de usuário, o nome do servidor Web, nomes de pastas e locais e outros valores por aqueles que são apropriadas para sua implantação.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Para configurar o WEB1 para distribuir certificados e CRLs  
  
1.  No WEB1, execute o Windows PowerShell como administrador, digite `explorer c:\`, e pressione ENTER. Windows Explorer é aberto para a unidade C.   
  
2.  Crie uma nova pasta chamada PKI na unidade c:. Para fazer isso, clique em **página inicial**e, em seguida, clique em **nova pasta**. Uma nova pasta é criada com o nome temporário realçado. Tipo de **pki** e, em seguida, pressione ENTER.  
  
3.  No Windows Explorer, clique com botão direito na pasta que você acabou de criar, focalize o cursor do mouse **compartilhar com**e, em seguida, clique em **pessoas específicas**. A caixa de diálogo **Compartilhamento de Arquivos** será aberta.  
  
4.  Na **compartilhamento de arquivos**, digite **editores de certificados**e, em seguida, clique em **adicionar**. O grupo de editores de certificados é adicionado à lista. Na lista, na **nível de permissão**, clique na seta ao lado de **editores de certificados**e, em seguida, clique em **leitura/gravação**. Clique em **compartilhamento**e, em seguida, clique em **feito**.  
  
5.  Feche o Windows Explorer.  
  
6.  Abra o console do IIS. No Gerenciador do Servidor, clique em **Ferramentas** e depois em **Gerenciador dos Serviços de Informações da Internet (IIS)**.  
  
7.  Na árvore de console do Gerenciador de serviços de informações da Internet (IIS), expanda **WEB1**. Se for convidado a começar a usar o Microsoft Web Platform, clique em **Cancelar**.  
  
8.  Expanda **Sites**, clique com o botão direito do mouse no **Site Padrão** e clique em **Adicionar Diretório Virtual**.  
  
9. Na **Alias**, digite **pki**. Na **caminho físico** tipo **C:\pki**, em seguida, clique em **Okey**.  
  
10. Habilitar anônimo acesso ao diretório virtual da pki, para que qualquer cliente possa verificar a validade dos certificados de autoridade de certificação e CRLs. Para fazer isso:  
  
    1.  No painel **Conexões**, verifique se **pki** está selecionado.  
  
    2.  Na **Página Inicial da pki** clique em **Autenticação**.  
  
    3.  No painel **Ações**, clique em **Editar Permissões**.  
  
    4.  Na guia **Segurança**, clique em **Editar**.  
  
    5.  Na caixa de diálogo **Permissões para pki** , clique em **Adicionar**.  
  
    6.  No **selecionar usuários, computadores, contas de serviço ou grupos**, tipo **LOGON anônimo; Todas as pessoas** e, em seguida, clique em **verificar nomes**. Clique em **OK**.  
  
    7.  Clique em **Okey** sobre o **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo.  
  
    8.  Clique em **Okey** sobre o **permissões para pki** caixa de diálogo.  
  
11. Clique em **Okey** sobre o **pki propriedades** caixa de diálogo.  
  
12. No painel **Página Inicial da pki** , clique duas vezes em **Filtragem de Solicitações**.  
  
13. A guia **Extensões de Nome de Arquivo** é selecionada por padrão no painel **Filtragem de Solicitações**. No painel **Ações** , clique em **Editar Configurações de Recurso**.  
  
14. Em **Editar Configurações de Filtragem de Solicitações**, selecione **Permitir saída dupla** e clique em **OK**.  
  
15. No MMC do Gerenciador de serviços de informações da Internet (IIS), clique no nome do seu servidor Web. Por exemplo, se seu servidor Web for denominado WEB1, clique em **WEB1**.  
  
16. Na **ações**, clique em **reiniciar**. Serviços de Internet estão parados e reiniciados.  
  

