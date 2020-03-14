---
ms.assetid: 5b9fc9c1-5d12-4ad4-8ddc-3b8a6d45b217
title: Criar um objeto de confiança de terceira parte confiável
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0d32edd7ebc23fa724439710c6511642d9c49a3
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323048"
---
# <a name="create-a-relying-party-trust"></a>Criar um objeto de confiança de terceira parte confiável


O documento a seguir fornece informações sobre como criar uma relação de confiança de terceira parte confiável manualmente e usando metadados de Federação.
  
## <a name="to-create-a-claims-aware-relying-party-trust-manually"></a>Para criar um confiança de terceira parte confiável com reconhecimento de declarações manualmente 

Para adicionar uma nova relação de confiança de terceira parte confiável usando o snap\-de gerenciamento de AD FS no e configurar manualmente as configurações, execute o procedimento a seguir em um servidor de Federação.  

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).
  
1. Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Em **ações**, clique em **Adicionar confiança de terceira parte confiável**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3.  Na página de **boas-vindas** , escolha **reconhecimento de declarações** e clique em **Iniciar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4.  Na página **Selecionar Fonte de Dados**, clique em **Inserir manualmente dados sobre a terceira parte confiável** e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust3.PNG) 
  
5.  Na página **especificar nome para exibição** , digite um nome em **nome para exibição**, em **observações** , digite uma descrição para a terceira parte confiável e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust4.PNG) 

6. Na página **Configurar certificado** , se você tiver um certificado de criptografia de token opcional, clique em **procurar** para localizar um arquivo de certificado e, em seguida, clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust5.PNG) 

7.  Na página **Configurar URL** , execute um ou ambos os procedimentos a seguir, clique em **Avançar**e vá para a etapa 8:  
  
    -   Marque a caixa de seleção **habilitar suporte para o protocolo passivo de Federação WS\-** . Em **URL do protocolo passivo de Federação WS\-** de terceira parte confiável, digite a URL para essa relação de confiança de terceira parte confiável e clique em **Avançar**.  
  
    -   Marque a caixa de seleção **Habilitar suporte para o protocolo WebSSO do SAML 2.0**. Em **URL do serviço de SSO do saml 2,0 de terceira parte confiável**, digite o Security ASSERTION MARKUP Language \(URL do ponto de extremidade do serviço\) SAML para essa terceira parte confiável e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust6.PNG)   

8. Na página **Configurar Identificadores**, especifique um ou mais identificadores da terceira parte confiável, clique em **Adicionar** para adicioná-los à lista e clique em **Avançar**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust8.PNG)
  
9.  Em **escolher política de controle de acesso** , selecione uma política e clique em **Avançar**.  Para obter mais informações sobre políticas de controle de acesso, consulte [políticas de controle de acesso em AD FS](Access-Control-Policies-in-AD-FS.md). 
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust9.PNG)

10. Na página **Pronto para adicionar confiança**, revise as configurações e clique em **Avançar** para salvar as informações do objeto de confiança de terceira parte confiável.  
   ![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust10.PNG) 
11. Na página **Concluir**, clique em **Fechar**. Essa ação exibe automaticamente a caixa de diálogo **Editar Regras de Declaração**.  
![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust11.PNG) 

## <a name="to-create-a-claims-aware-relying-party-trust-using-federation-metadata"></a>Para criar um confiança de terceira parte confiável com reconhecimento de declarações usando metadados de Federação

Para adicionar uma nova relação de confiança de terceira parte confiável, usando o snap-in de gerenciamento de AD FS, importando automaticamente os dados de configuração sobre o parceiro de metadados de Federação que o parceiro publicou em uma rede local ou com a Internet, execute o seguinte procedimento em um servidor de Federação na organização do parceiro de conta.

>[!NOTE]
>Embora tenha sido uma prática comum usar certificados com nomes de host não qualificados, como https://myserver, esses certificados não têm nenhum valor de segurança e podem permitir que um invasor represente um Serviço de Federação que esteja publicando metadados de Federação. Portanto, ao consultar metadados de Federação, você deve usar apenas um nome de domínio totalmente qualificado, como https://myserver.contoso.com.

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).


1. Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2. Em **ações**, clique em **Adicionar confiança de terceira parte confiável**.  
   ![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust1.PNG)   

3. Na página de **boas-vindas** , escolha **reconhecimento de declarações** e clique em **Iniciar**.  
   ![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust2.PNG) 
  
4. Na página **selecionar fonte de dados** , clique em <strong>importar dados sobre a terceira parte confiável publicada online ou em uma rede local *. Em * * endereço de metadados de Federação (nome do host ou URL)</strong>, digite a URL de metadados de Federação ou o nome do host do parceiro e clique em **Avançar**.  
   ![terceira parte confiável](media/Create-a-Relying-Party-Trust/addtrust12.PNG) 

5. Na página Especificar nome de exibição, digite um nome em **nome para exibição**, em observações, digite uma descrição para a terceira parte confiável e clique em **Avançar**.

6. Na página escolher regras de autorização de emissão, selecione **permitir que todos os usuários acessem essa** terceira parte confiável ou **negue a todos os usuários acesso a essa**terceira parte confiável e clique em **Avançar**.

7. Na página pronto para adicionar confiança, examine as configurações e clique em **Avançar** para salvar as informações de confiança de terceira parte confiável.

8. Na página concluir, clique em **fechar**. Essa ação exibe automaticamente a caixa de diálogo Editar Regras de Declaração. Para obter mais informações sobre como proceder ao adicionar regras de declaração a esse objeto de confiança de terceira parte confiável, consulte as Referências adicionais.




## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
