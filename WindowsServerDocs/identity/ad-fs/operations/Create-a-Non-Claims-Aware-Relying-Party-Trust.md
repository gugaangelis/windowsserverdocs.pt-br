---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Criar um objeto de confiança da terceira parte confiável sem reconhecimento de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0ea877170a07db6abe9ac82e72d1722600ec933
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358115"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Criar uma relação de confiança de terceira parte confiável sem reconhecimento de declaração


Nas AD FS snap @ no__t-0in, não @ no__t-1claims @ no__t-2aware de terceira parte confiável são objetos que são criados para representar a relação de confiança entre o serviço de Federação e um único aplicativo Web @ no__t-3based que não é Claims @ no__t-4aware e que é acessado por meio do proxy de aplicativo Web.  
  
Uma relação de confiança de terceira parte confiável não @ no__t-0claims @ no__t-1aware é uma terceira parte confiável que consiste em identificadores, nomes e regras para autenticação e autorização quando a confiança de terceira parte confiável é acessada por meio do proxy de aplicativo Web. Esses aplicativos Web @ no__t-0based que não dependem de declarações, em outras palavras, esses aplicativos integrados de autenticação do Windows @ no__t-1based podem ter regras de autorização que impõem o acesso baseado em declarações quando o acesso é externo ao rede corporativa por meio do proxy de aplicativo Web.  
  
Para adicionar uma nova relação de confiança de terceira parte confiável não @ no__t-0claims @ no__t-1aware, usando o snap de gerenciamento de AD FS @ no__t-2in, execute o procedimento a seguir.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Para criar uma relação de confiança de terceira parte confiável que não reconhece declarações manualmente 
1. Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Em **ações**, clique em **Adicionar confiança de terceira parte confiável**.  
parte ![relying @ no__t-1   

3.  Na página de **boas-vindas** , escolha **não reconhecimento de declarações** e clique em **Iniciar**.  
parte ![relying @ no__t-1 
  
4.  Na página **especificar nome para exibição** , digite um nome em **nome para exibição**, em **observações** , digite uma descrição para a terceira parte confiável e clique em **Avançar**.  
parte ![relying @ no__t-1

5. Na página **Configurar Identificadores**, especifique um ou mais identificadores da terceira parte confiável, clique em **Adicionar** para adicioná-los à lista e clique em **Avançar**.  
parte ![relying @ no__t-1

6.  Em **escolher política de controle de acesso** , selecione uma política e clique em **Avançar**.  Para obter mais informações sobre políticas de controle de acesso, consulte [políticas de controle de acesso em AD FS](Access-Control-Policies-in-AD-FS.md). 
parte ![relying @ no__t-1

7. Na página **Pronto para adicionar confiança** , revise as configurações e clique em **Avançar** para salvar as informações do objeto de confiança de terceira parte confiável.  
   parte ![relying @ no__t-1 

8. Na página **Concluir**, clique em **Fechar**. Essa ação exibe automaticamente a caixa de diálogo **Editar Regras de Declaração**.  
parte ![relying @ no__t-1  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
