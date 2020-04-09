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
ms.openlocfilehash: c6886145e910b76edbe99549266d651cdd7c3edf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816919"
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Criar uma relação de confiança de terceira parte confiável sem reconhecimento de declaração


No snap\-de gerenciamento de AD FS no, as declarações não\-\-confianças de terceira parte confiável são objetos que são criados para representar a relação de confiança entre o serviço de Federação e um único aplicativo baseado na Web\-que não é declarações\-ciente e que é acessado por meio do proxy de aplicativo Web.  
  
Uma declaração não\-\-confiança de terceira parte confiável é uma relação de confiança de terceira parte confiável que consiste em identificadores, nomes e regras para autenticação e autorização quando a confiança de terceira parte confiável é acessada por meio do proxy de aplicativo Web. Esses aplicativos baseados na Web\-que não dependem de declarações, ou seja, esses aplicativos integrados\-de autenticação do Windows, podem ter regras de autorização que impõem o acesso baseado em declarações quando o acesso é externo à rede corporativa por meio do proxy de aplicativo Web.  
  
Para adicionar uma nova declaração não\-\-confiança de terceira parte confiável, usando o\-de snap AD FS de gerenciamento no, execute o procedimento a seguir.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Para criar uma relação de confiança de terceira parte confiável que não reconhece declarações manualmente 
1. Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Em **ações**, clique em **Adicionar confiança de terceira parte confiável**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Na página de **boas-vindas** , escolha **não reconhecimento de declarações** e clique em **Iniciar**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  Na página **especificar nome para exibição** , digite um nome em **nome para exibição**, em **observações** , digite uma descrição para a terceira parte confiável e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. Na página **Configurar Identificadores**, especifique um ou mais identificadores da terceira parte confiável, clique em **Adicionar** para adicioná-los à lista e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Em **escolher política de controle de acesso** , selecione uma política e clique em **Avançar**.  Para obter mais informações sobre políticas de controle de acesso, consulte [políticas de controle de acesso em AD FS](Access-Control-Policies-in-AD-FS.md). 
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. Na página **Pronto para adicionar confiança**, revise as configurações e clique em **Avançar** para salvar as informações do objeto de confiança de terceira parte confiável.  
   ![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Na página **Concluir**, clique em **Fechar**. Essa ação exibe automaticamente a caixa de diálogo **Editar Regras de Declaração**.  
![terceira parte confiável](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
