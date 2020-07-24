---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: Criar um objeto de confiança da terceira parte confiável sem reconhecimento de declarações
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2bc08d2797812d6001f2a792037bcfb8bab4fc0a
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965638"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Criar uma relação de confiança de terceira parte confiável sem reconhecimento de declaração


No snap in de gerenciamento de AD FS \- , as relações de confiança de terceira \- \- parte confiável não reconhecem declarações são objetos criados para representar a confiança entre o serviço de Federação e um único \- aplicativo baseado na Web que não \- reconhece declarações e que é acessado por meio do proxy de aplicativo Web.  
  
Uma confiança de terceira parte \- confiável sem reconhecimento de Declaração \- é uma relação de confiança de terceira parte confiável que consiste em identificadores, nomes e regras para autenticação e autorização quando a confiança de terceira parte confiável é acessada por meio do proxy de aplicativo Web. Esses \- aplicativos baseados na Web que não dependem de declarações, ou seja, esses aplicativos integrados \- baseados na autenticação do Windows, podem ter regras de autorização que impõem acesso com base em declarações quando o acesso é externo à rede corporativa por meio do proxy de aplicativo Web.  
  
Para adicionar uma nova relação de confiança de terceira \- \- parte confiável sem reconhecimento de declaração, usando o snap de gerenciamento de AD FS \- no, execute o procedimento a seguir.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Para criar uma relação de confiança de terceira parte confiável que não reconhece declarações manualmente 
1. No Gerenciador do Servidor, clique em **Ferramentas** e depois selecione **Gerenciamento do AD FS**.  
  
2.  Em **ações**, clique em **Adicionar confiança de terceira parte confiável**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Na página de **boas-vindas** , escolha **não reconhecimento de declarações** e clique em **Iniciar**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  Na página **Especificar Nome de Exibição**, digite um nome em **Nome de exibição**, em **Notas** digite uma descrição para essa terceira parte confiável e, em seguida, clique em **Avançar **.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. Na página **Configurar Identificadores**, especifique um ou mais identificadores da terceira parte confiável, clique em **Adicionar** para adicioná-los à lista e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Em **Escolher Política de Controle de Acesso**, escolha uma política e clique em **Avançar**.  Para obter mais informações sobre políticas de controle de acesso, consulte [políticas de controle de acesso em AD FS](Access-Control-Policies-in-AD-FS.md). 
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. Na página **Pronto para adicionar confiança**, revise as configurações e clique em **Avançar** para salvar as informações do objeto de confiança de terceira parte confiável.  
   ![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Na página **Concluir**, clique em **Fechar**. Essa ação exibe automaticamente a caixa de diálogo **Editar Regras de Declaração**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Consulte Também  
[Operações do AD FS](../ad-fs-operations.md) 
