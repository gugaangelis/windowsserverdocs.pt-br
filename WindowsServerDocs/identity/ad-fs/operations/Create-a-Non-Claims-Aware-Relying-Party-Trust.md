---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Criar um objeto de confiança da terceira parte confiável sem reconhecimento de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cdd0b32b50f676007a6cc922bc15b95bb61323be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189666"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Criar uma confiança de terceira parte confiável sem reconhecimento de declarações


No snap do gerenciamento do AD FS\-em um\-declarações\-com suporte a relações de confiança de terceira parte confiável são objetos que são criados para representar a relação de confiança entre o serviço de Federação e uma única web\-aplicativo que não é baseado em declarações\-ciente e que é acessado por meio do Proxy de aplicativo Web.  
  
Não\-declarações\-ciente de terceira parte confiável é uma terceira parte confiável que consiste em identificadores, nomes e regras de autenticação e autorização, quando a terceira parte confiável é acessada por meio do Proxy de aplicativo Web. Esses web\-com base em aplicativos que não dependem de declarações, em outras palavras, essas autenticação integrada do Windows\-aplicativos baseados no, pode ter regras de autorização que impõem o acesso baseado em declarações quando o acesso é externo à rede corporativa por meio do Proxy de aplicativo Web.  
  
Para adicionar uma nova não\-declarações\-ciente de terceira parte confiável, usando o gerenciamento do AD FS snap\-, execute o procedimento a seguir.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Criar um não é baseado em declarações com suporte a terceira parte confiável manualmente 
1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Sob **ações**, clique em **adicionar terceira parte confiável**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sobre o **bem-vindo** , escolha **com reconhecimento de declaração não** e clique em **iniciar**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  No **especificar nome para exibição** página, digite um nome na **nome de exibição**, em **notas** digite uma descrição para essa terceira parte confiável e, em seguida, clique em **Avançar** .  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. Na página **Configurar Identificadores**, especifique um ou mais identificadores da terceira parte confiável, clique em **Adicionar** para adicioná-los à lista e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Sobre o **escolher política de controle de acesso** selecionar uma política e clique em **próxima**.  Para obter mais informações sobre as políticas de controle de acesso, consulte [políticas de controle de acesso no AD FS](Access-Control-Policies-in-AD-FS.md). 
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. Na página **Pronto para adicionar confiança** , revise as configurações e clique em **Avançar** para salvar as informações do objeto de confiança de terceira parte confiável.  
   ![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Na página **Concluir**, clique em **Fechar**. Essa ação exibe automaticamente a caixa de diálogo **Editar Regras de Declaração**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
