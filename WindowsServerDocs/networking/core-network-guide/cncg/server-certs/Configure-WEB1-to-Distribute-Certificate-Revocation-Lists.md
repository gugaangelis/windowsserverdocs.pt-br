---
title: Configurar WEB1 para distribuir listas de revogação de certificados (CRLs)
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b96fad769638de9835c52137e5165fa32a6b9bd4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurar WEB1 para distribuir listas de revogação de certificados (CRLs)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para configurar o servidor web WEB1 para distribuir CRLs.  
  
Nas extensões da CA raiz, foi mencionado que CRL da CA raiz seria disponível via http://pki.corp.contoso.com/pki. Atualmente, não há um diretório PKI em WEB1, portanto, é necessário criar um.  
  
Para executar este procedimento, você deve ser um membro do **Admins. do domínio**.  
  
> [!NOTE]  
> No procedimento a seguir, substitua o nome da conta de usuário, o nome do servidor Web, nomes de pasta e locais e outros valores aqueles que são apropriados para a implantação.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Para configurar WEB1 para distribuir certificados e CRLs  
  
1.  Em WEB1, execute o Windows PowerShell como administrador, digite `explorer c:\`, e pressione ENTER. Windows Explorer abre a unidade C.   
  
2.  Crie uma nova pasta chamada PKI na unidade c:. Para fazer isso, clique em **Home**e clique em **nova pasta**. Uma nova pasta é criada com o nome temporário realçado. Tipo **pki** e pressione ENTER.  
  
3.  No Windows Explorer, clique com botão direito na pasta que você acabou de criar, passe o cursor do mouse sobre **compartilhar com**e clique em **pessoas específicas**. O **compartilhamento de arquivos** caixa de diálogo é aberta.  
  
4.  Em **compartilhamento de arquivos**, tipo **editores de certificados**e clique em **adicionar**. O grupo de editores de certificados é adicionado à lista. Na lista, em **nível de permissão**, clique na seta ao lado de **editores de certificados**e clique em **leitura/gravação**. Clique em **compartilhamento**e clique em **feito**.  
  
5.  Feche o Windows Explorer.  
  
6.  Abra o console do IIS. No Gerenciador do servidor, clique em **ferramentas**e clique em **Gerenciador de serviços de informações da Internet (IIS)**.  
  
7.  Na árvore de console do Gerenciador de serviços de informações da Internet (IIS), expanda **WEB1**. Se você está convidado para introdução ao Microsoft Web Platform, clique em **Cancelar**.  
  
8.  Expanda **Sites** e, em seguida, clique com botão direito do **site padrão** e, em seguida, clique em **Add Virtual Directory**.  
  
9. Em **Alias**, tipo **pki**. Em **caminho físico** tipo **C:\pki**, clique em **Okey**.  
  
10. Habilitar anônimos acessem o diretório virtual pki, para que qualquer cliente possa verificar a validade dos certificados de CA e CRLs. Para fazer isso:  
  
    1.  No **conexões** painel, certifique-se de que **pki** é selecionado.  
  
    2.  Em **pki Home** clique **autenticação**.  
  
    3.  No **ações** painel, clique em **editar permissões**.  
  
    4.  Sobre o **segurança**, clique em **editar**  
  
    5.  Sobre o **permissões para pki** caixa de diálogo, clique em **adicionar**.  
  
    6.  No **selecionar usuários, computadores, contas de serviço ou grupos**, tipo **LOGON anônimo; Todos** e, em seguida, clique em **verificar nomes**. Clique em **Okey**.  
  
    7.  Clique em **Okey** sobre o **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo.  
  
    8.  Clique em **Okey** sobre o **permissões para pki** caixa de diálogo.  
  
11. Clique em **Okey** sobre o **pki propriedades** caixa de diálogo.  
  
12. No **pki Home** painel, clique duas vezes em **solicitar filtragem**.  
  
13. O **extensões de nome de arquivo** guia é selecionada por padrão no **solicitar filtragem** painel. No **ações** painel, clique em **editar configurações de recurso**.  
  
14. Em **editar configurações da filtragem solicitar**, selecione **permitir escape double** e, em seguida, clique em **Okey**.  
  
15. No MMC do Gerenciador de serviços de informações da Internet (IIS), clique em nome do seu servidor Web. Por exemplo, se o servidor Web for denominado WEB1, clique em **WEB1**.  
  
16. Em **ações**, clique em **reiniciar**. Serviços de Internet são interrompidos e reiniciados.  
  

