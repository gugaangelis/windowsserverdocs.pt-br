---
ms.assetid: 7b7ae389-5032-44f7-9c0a-94398c3e4d88
title: "Criar uma confiança de terceiros terceira ciente da não requerimentos judiciais ou Extrajudiciais"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f46675ff4c471af743fd8782c1e3036e7c546256
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-non-claims-aware-relying-party-trust"></a>Criar uma relação de confiança de terceiros sem reconhecimento de declarações de dependência

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

No AD FS gerenciamento snap\, non\ claims\-reconhecimento dependência relações de confiança de terceiros são objetos que são criados para representar a relação de confiança entre o serviço de Federação e um único com base em web\ aplicativo que não tem reconhecimento de claims\ e que é acessada por meio do Proxy de aplicativo Web.  
  
Um reconhecimento de claims\ non\ terceira parte relação de confiança for terceira terceiros que consiste em regras para autenticação e autorização, nomes e identificadores quando a terceira confiança de terceiros é acessada por meio do Proxy de aplicativo Web. Esses aplicativos com base em web\ que não dependem de declarações, em outras palavras, esses aplicativos baseados em Authentication\ integrado do Windows, pode ter regras de autorização que impõem acesso com base em declarações quando o acesso é externo à rede corporativa por meio do Proxy de aplicativo Web.  
  
Para adicionar uma nova non\ claims\ reconhecimento terceira parte confiança, usando o AD FS snap\-in Gerenciamento, execute o procedimento a seguir.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
##<a name="to-create-a-non-claims-aware-relying-party-trust-manually"></a>Criar um não-requerimentos judiciais ou Extrajudiciais ciente dependência terceiros relação de confiança manualmente 
1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Em **ações**, clique em **Adicionar dependência terceiros confiar **.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  No **boas-vindas** página, escolha **não requerimentos judiciais ou Extrajudiciais cientes** e clique em **iniciar**.  
![terceiro](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon1.PNG) 
  
4.  Sobre o **especificar o nome de exibição** página, digite um nome na **nome de exibição**, em **notas** digite uma descrição para essa terceira confiança de terceiros e, em seguida, clique em **próxima **.  
![terceiro](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon2.PNG)

5. No **configurar identificadores** de página, especifique um ou mais identificadores para esse terceiro, clique em **adicionar** para adicioná-los à lista e, em seguida, clique em **próxima**.  
![terceiro](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon3.PNG)

6.  Sobre o **escolher política de controle de acesso** selecione uma política e clique em **próxima**.  Para obter mais informações sobre políticas de controle de acesso, consulte [políticas de controle de acesso no AD FS](Access-Control-Policies-in-AD-FS.md). 
![terceiro](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon4.PNG)

7. No **pronto para adicionar confiança** página, examine as configurações e, em seguida, clique em **próxima** para salvar o terceiro confiar em informações.  
   ![terceiro](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon5.PNG) 

8. Sobre o **concluir** página, clique em **fechar**. Essa ação exibe automaticamente o **editar regras de declaração** caixa de diálogo.  
![terceiro](media/Create-a-Non-Claims-Aware-Relying-Party-Trust/addnon6.PNG)  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
