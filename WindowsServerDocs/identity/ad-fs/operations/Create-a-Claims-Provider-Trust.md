---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: "Criar uma relação de confiança do provedor de declarações"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4dc20713fdd137b019a072037e35e9219e02fa9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-claims-provider-trust"></a>Criar uma relação de confiança do provedor de declarações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Para adicionar uma nova confiança provedor requerimentos judiciais ou Extrajudiciais usando o AD FS snap\-in Gerenciamento e definir as configurações manualmente, execute o seguinte procedimento em um servidor de Federação do parceiro de recurso na organização do parceiro de recurso.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Para criar uma relação de confiança do provedor de declarações manualmente  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Em **ações**, clique em **adicionar a confiança do provedor de declarações **.  
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Sobre o **boas-vindas** página, clique em **iniciar **. 
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  No **Selecionar fonte de dados** página, clique em **Enter declarações dados de confiança do provedor manualmente**e clique em **próxima **.  
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  No **especificar o nome de exibição** página, digite um **nome de exibição**, em **notas**, digite uma descrição para isso diz confiança de provedor e, em seguida, clique em **próxima **.  
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  No **configurar URL** página, especifique o **WS-Federation passivo URL** se aplicável e clique em **próxima **.
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. Sobre o **identificador configurar** página, em **identificador de confiança do provedor de declarações**, digite o identificador apropriado e, em seguida, clique em **próxima **.  
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. No **configurar certificados** página, clique em **adicionar** para localizar um arquivo de certificado e adicioná-lo à lista de certificados e, em seguida, clique em **próxima **.  
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. Sobre o **pronto para adicionar confiança** página, clique em **próxima** salvar suas declarações informações de confiança do provedor.  
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. Sobre o **concluir** página, clique em **fechar**. Essa ação exibe automaticamente o **editar regras de declaração** caixa de diálogo. Para obter mais informações sobre como proceder com adicionando reivindicar regras para esta relação de confiança do provedor de declarações, consulte as seguintes referências adicionais.  
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Para criar uma relação de confiança de provedor de declarações usando metadados de Federação
Para adicionar um novo provedor de declarações a relação de confiança, usando o snap-in de gerenciamento do AD FS importando automaticamente os dados de configuração sobre o parceiro de metadados de federação que o parceiro tenha publicado para uma rede local ou à Internet, execute o seguinte procedimento em um servidor de federação na organização do parceiro de recurso.

>[!NOTE]
>Embora ele foi longa prática comum usar certificados com nomes de host não qualificado, como https://myserver, esses certificados não têm nenhum valor de segurança e podem permitir que um invasor representar um serviço de federação que publica federação metadados. Portanto, ao consultar metadados de federação, você só deve usar um nome de domínio totalmente qualificado, como https://myserver.contoso.com.

1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **.  
  
2.  Em **ações**, clique em **Adicionar dependência terceiros confiar **.  
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Sobre o **boas-vindas** página, clique em **iniciar **. 
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Sobre o **Selecionar fonte de dados** página, clique em **importar dados sobre o provedor de declarações publicados online ou em uma rede local **. Na federação endereço dos metadados (nome de host ou URL), digite o **URL de metadados de Federação** ou host dê um nome para o parceiro e clique em **próxima **.
![Requerimentos judiciais ou Extrajudiciais provedor confiança](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Tipo de página no nome de exibição de especificar uma **nome de exibição**, em notas, digite uma descrição para isso diz confiança de provedor e, em seguida, clique em **próxima **.

6.  Na página pronto para adicionar confiança, clique em **próxima** salvar suas declarações informações de confiança do provedor.

7.  Na página Finish, clique em **fechar**. Isso exibirá a caixa de diálogo Editar reivindicação regras automaticamente. Para obter mais informações sobre como proceder com adicionando reivindicar regras para esta relação de confiança do provedor de declarações, consulte a seção de referências adicionais abaixo.



    
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando a organização do parceiro de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Lista de verificação: Criação de regras de declaração para um provedor de declarações confiável](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
