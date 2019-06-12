---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Criar um objeto de confiança de terceira parte confiável
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b000d4cfd4ded7ad37dbb235a9d33c83d8951707
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444364"
---
# <a name="create-a-relying-party-trust"></a>Criar um objeto de confiança de terceira parte confiável


O documento a seguir fornece informações sobre como criar uma terceira parte confiável manualmente e usando metadados de Federação.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Para criar um declarações com suporte a terceira parte confiável de confiança manualmente 

Para adicionar uma novo terceira parte confiável usando o snap de gerenciamento do AD FS\-além e manualmente, defina as configurações, execute o procedimento a seguir em um servidor de Federação.  

A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Sob **ações**, clique em **adicionar terceira parte confiável**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Sobre o **bem-vindo** , escolha **reconhecimento de declaração** e clique em **iniciar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Na página **Selecionar Fonte de Dados** , clique em **Inserir manualmente dados sobre a terceira parte confiável**e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  No **especificar nome para exibição** página, digite um nome na **nome de exibição**, em **notas** digite uma descrição para essa terceira parte confiável e, em seguida, clique em **Avançar** .  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. Sobre o **configurar certificado** página, se você tiver um certificado de criptografia de token opcional, clique em **procurar** para localizar um arquivo de certificado e, em seguida, clique em **próxima**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Sobre o **configurar URL** , siga um ou ambos os procedimentos a seguir, clique em **próxima**e, em seguida, vá para a etapa 8:  
  
    -   Selecione o **habilitar o suporte para o WS\-protocolo de federação passiva** caixa de seleção. Sob **WS de terceiros de terceira parte confiável\-URL do protocolo de federação passiva**, digite a URL para essa terceira parte confiável e, em seguida, clique em **próximo**.  
  
    -   Marque a caixa de seleção **Habilitar suporte para o protocolo WebSSO do SAML 2.0**. Sob **URL de serviço de SSO do SAML 2.0 de terceiros de terceira parte confiável**, digite o Security Assertion Markup Language \(SAML\) URL de ponto de extremidade para essa terceira parte confiável do serviço e, em seguida, clique em **próximo**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. Na página **Configurar Identificadores**, especifique um ou mais identificadores da terceira parte confiável, clique em **Adicionar** para adicioná-los à lista e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Sobre o **escolher política de controle de acesso** selecionar uma política e clique em **próxima**.  Para obter mais informações sobre as políticas de controle de acesso, consulte [políticas de controle de acesso no AD FS](Access-Control-Policies-in-AD-FS.md). 
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. Na página **Pronto para adicionar confiança** , revise as configurações e clique em **Avançar** para salvar as informações do objeto de confiança de terceira parte confiável.  
   ![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Na página **Concluir**, clique em **Fechar**. Essa ação exibe automaticamente a caixa de diálogo **Editar Regras de Declaração**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Para criar um declarações com suporte a terceira parte confiável de confiança usando metadados de Federação

Para adicionar uma novo terceira parte confiável, usando o snap-in de gerenciamento do AD FS, importando automaticamente dados de configuração sobre o parceiro de metadados de federação que o parceiro publicou em uma rede local ou à Internet, execute o seguinte procedimento em um servidor de federação na organização do parceiro de conta.

>[!NOTE]
>Embora sempre foi uma prática comum usar certificados com nomes de host não qualificados, como https://myserver, esses certificados não têm nenhum valor de segurança e pode permitir que um invasor represente um serviço de federação que está publicando metadados de Federação. Portanto, ao consultar metadados de federação, você deve apenas usar um nome de domínio totalmente qualificado, como https://myserver.contoso.com.

A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).


1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2. Sob **ações**, clique em **adicionar terceira parte confiável**.  
   ![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. Sobre o **bem-vindo** , escolha **reconhecimento de declaração** e clique em **iniciar**.  
   ![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. Sobre o **Selecionar fonte de dados** , clique em <strong>importar dados sobre a terceira parte confiável publicados online ou em uma rede local *. Em * * endereço de metadados de Federação (nome de host ou URL)</strong>, digite o nome de host ou URL da metadados de Federação do parceiro e, em seguida, clique em **próxima**.  
   ![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. Na página Especificar nome para exibição, digite um nome na **nome de exibição**, em observações, digite uma descrição para essa terceira parte confiável e, em seguida, clique em **próxima**.

6. Na página Escolher regras de autorização de emissão, selecione **permitir que todos os usuários acessem esta terceira** ou **negar acesso a todos os usuários a esta terceira**e, em seguida, clique em **Avançar**.

7. Na página pronto para adicionar relação de confiança, examine as configurações e, em seguida, clique em **próxima** para salvar sua terceira informações de confiança.

8. Na página concluir, clique em **fechar**. Essa ação exibe automaticamente a caixa de diálogo Editar regras de declaração. Para obter mais informações sobre como proceder ao adicionar regras de declaração a esse objeto de confiança de terceira parte confiável, consulte as Referências adicionais.




## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
