---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Criar uma relação de confiança do provedor de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 909be9e4bbcd12b00fd60ff061b1f4e2ede34546
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308534"
---
# <a name="create-a-claims-provider-trust"></a>Criar uma relação de confiança do provedor de declarações

Para adicionar uma nova relação de confiança de provedor de declarações usando o snap de gerenciamento do AD FS\-além e manualmente, defina as configurações, execute o procedimento a seguir em um servidor de Federação do parceiro de recurso na organização do parceiro de recurso.  
  
Associação na **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Para criar manualmente uma relação de confiança do provedor de declarações  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Sob **ações**, clique em **adicionar confiança do provedor de declarações**.  
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Na página **Bem-vindo**, clique em **Iniciar**. 
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Na página **Selecionar Fonte de Dados**, clique em **Inserir manualmente dados da relação de confiança do provedor de declarações** e depois clique em **Avançar**.  
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  Na página **Especificar Nome para Exibição**, digite um **Nome para exibição**; em **Observações**, digite uma descrição para essa relação de confiança do provedor de declarações e clique em **Avançar**.  
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  Sobre o **configurar URL do** , especifique a **URL passivo do WS-Federation** se aplicável e clique em **próxima**.
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. Na página **Configurar Identificador**, em **Identificador da confiança do provedor de declarações**, digite o identificador apropriado e clique em **Avançar**.  
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. Na página **Configurar Certificados**, clique em **Adicionar** para localizar um arquivo de certificado e adicioná-lo à lista de certificados. Em seguida, clique em **Avançar**.  
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. Na página **Pronto para Adicionar Confiança**, clique em **Avançar** para salvar as informações sobre confianças do provedor de declarações.  
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. Na página **Concluir**, clique em **Fechar**. Essa ação exibe automaticamente a caixa de diálogo **Editar Regras de Declaração**. Para obter mais informações sobre como proceder com a adição de regras para essa relação de confiança do provedor de declarações de declaração, consulte as seguintes referências adicionais.  
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Para criar uma relação de confiança do provedor de declarações usando metadados de Federação
Para adicionar uma nova relação de confiança do provedor de declarações, usando o snap-in de gerenciamento do AD FS, importando automaticamente dados de configuração sobre o parceiro de metadados de federação que o parceiro publicou em uma rede local ou à Internet, execute o procedimento a seguir em um servidor de federação na organização do parceiro de recurso.

>[!NOTE]
>Embora sempre foi uma prática comum usar certificados com nomes de host não qualificado como https:\//myserver, esses certificados não têm nenhum valor de segurança e pode permitir que um invasor represente um serviço de federação que está publicando a federação metadados. Portanto, ao consultar metadados de federação, você deve apenas usar um nome de domínio totalmente qualificado, como `https://myserver.contoso.com`.

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**.  
  
2.  Sob **ações**, clique em **adicionar confiança do provedor de declarações**.  
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Na página **Bem-vindo**, clique em **Iniciar**. 
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Na página **Selecionar Fonte de Dados**, clique em **Importar dados sobre o provedor de declarações publicados online ou em uma rede local**. Na federação endereço de metadados (nome de host ou URL), digite o **URL de metadados de Federação** ou o host de nomes para o parceiro e, em seguida, clique em **próxima**.
![Relação de confiança de provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Tipo de página Especificar nome de exibição um **nome de exibição**, em observações, digite uma descrição para essa relação de confiança do provedor de declarações e, em seguida, clique em **próxima**.

6.  Na página pronto para adicionar relação de confiança, clique em **próxima** para salvar suas declarações informações de confiança do provedor.

7.  Na página concluir, clique em **fechar**. Isso exibirá a caixa de diálogo Editar regras de declaração automaticamente. Para obter mais informações sobre como proceder com a adição de regras para essa relação de confiança do provedor de declarações de declaração, consulte a seção Referências adicionais a seguir.



    
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando a organização do parceiro de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Lista de verificação: Como criar regras de declaração para uma relação de confiança do provedor de declarações](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
