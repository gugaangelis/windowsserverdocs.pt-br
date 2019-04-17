---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: "Criar uma terceira confiança de terceiros"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14e1cc732ed60b7f05a9a4a9aac9037c48b702f2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-relying-party-trust"></a>Criar uma terceira confiança de terceiros

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Este documento fornece informações sobre como criar uma relação de confiança de terceiros terceira manualmente e usando metadados de Federação.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Para criar um requerimentos judiciais ou Extrajudiciais ciente terceiros Relying confiar manualmente 

Para adicionar uma nova confiança de terceiros terceira usando o AD FS snap\-in Gerenciamento e definir as configurações manualmente, execute o seguinte procedimento em um servidor de Federação.  

A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Em **ações**, clique em **Adicionar dependência terceiros confiar **.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  No **boas-vindas** página, escolha **reconhecer declarações** e clique em **iniciar **.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Sobre o **Selecionar fonte de dados** página, clique em **inserir manualmente os dados sobre o terceiro**e, em seguida, clique em **próxima **.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  Sobre o **especificar o nome de exibição** página, digite um nome na **nome de exibição**, em **notas** digite uma descrição para essa terceira confiança de terceiros e, em seguida, clique em **próxima **.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. No **certificado configurar** página, se você tiver um certificado de criptografia do token opcional, clique em **procurar** para localizar um arquivo de certificado e clique em **próxima **.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Sobre o **configurar URL** de página, siga um ou ambos os procedimentos, clique em **próxima**e, em seguida, vá para a etapa 8:  
  
    -   Selecione o **habilitar o suporte para o protocolo WS\ federação passiva** caixa de seleção. Em **Relying terceiros WS\ federação passiva protocolo URL**, digite a URL para essa terceira confiança de terceiros e, em seguida, clique em **próxima **.  
  
    -   Selecione o **habilitar o suporte para o protocolo SAML 2.0 WebSSO** caixa de seleção. Em **Relying terceiros SAML 2.0 SSO service URL**, digite a URL de ponto de extremidade do serviço \(SAML\) linguagem de marcação de asserção de segurança para essa terceira confiança de terceiros e, em seguida, clique em **próxima**.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. No **configurar identificadores** de página, especifique um ou mais identificadores para esse terceiro, clique em **adicionar** para adicioná-los à lista e, em seguida, clique em **próxima**.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Sobre o **escolher política de controle de acesso** selecione uma política e clique em **próxima**.  Para obter mais informações sobre políticas de controle de acesso, consulte [políticas de controle de acesso no AD FS](Access-Control-Policies-in-AD-FS.md). 
![terceiro](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. No **pronto para adicionar confiança** página, examine as configurações e, em seguida, clique em **próxima** para salvar o terceiro confiar em informações.  
   ![terceiro](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Sobre o **concluir** página, clique em **fechar**. Essa ação exibe automaticamente o **editar regras de declaração** caixa de diálogo.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Para criar um requerimentos judiciais ou Extrajudiciais ciente terceiros Relying confiar usando metadados de Federação

Para adicionar uma nova confiante de confiança de terceiros, usando o snap-in de gerenciamento do AD FS importando automaticamente os dados de configuração sobre o parceiro de metadados de federação que o parceiro publicado para uma rede local ou à Internet, execute o procedimento a seguir em um servidor de federação na organização do parceiro de conta.

>[!NOTE]
>Embora ele foi longa prática comum usar certificados com nomes de host não qualificado, como https://myserver, esses certificados não têm nenhum valor de segurança e podem permitir que um invasor representar um serviço de federação que publica federação metadados. Portanto, ao consultar metadados de federação, você só deve usar um nome de domínio totalmente qualificado, como https://myserver.contoso.com.

A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).


1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Em **ações**, clique em **Adicionar dependência terceiros confiar **.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  No **boas-vindas** página, escolha **reconhecer declarações** e clique em **iniciar **.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Sobre o **Selecionar fonte de dados** página, clique em **importar dados sobre o terceiro publicados online ou em uma rede local*. Em **endereço de metadados de Federação (nome de host ou URL)**, digite o nome de host ou URL da metadados de Federação do parceiro e, em seguida, clique em **próxima**.  
![terceiro](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5.  Na página especificar o nome de exibição, digite um nome na **nome de exibição**, em notas, digite uma descrição para esta relação de confiança de terceiros confiante e clique **próxima**.

6.  Na página Escolha as regras de autorização de emissão, selecione **permitir que todos os usuários acessem esse terceiro** ou **negar acesso de todos os usuários a esse terceiro**e clique em **próxima**.

7.  Na página pronto para adicionar confiança, examine as configurações e, em seguida, clique em **próxima** para salvar o terceiro confiar em informações.

8.  Na página Finish, clique em **fechar**. Essa ação exibe a caixa de diálogo Editar reivindicação regras automaticamente. Para obter mais informações sobre como proceder com a adição de regras de declaração para essa terceira confiança de terceiros, consulte as referências adicionais.




## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
