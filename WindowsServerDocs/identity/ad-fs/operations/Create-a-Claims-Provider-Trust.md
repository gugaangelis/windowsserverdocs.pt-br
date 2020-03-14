---
ms.assetid: a4f7842c-cfca-4d78-916e-023d12a9cdf0
title: Criar uma relação de confiança do provedor de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4539e8abd1af1eca7bacb51971e6d355bb0aab28
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323078"
---
# <a name="create-a-claims-provider-trust"></a>Criar uma relação de confiança do provedor de declarações

Para adicionar uma nova confiança do provedor de declarações usando o snap\-de gerenciamento de AD FS no e configurar manualmente as configurações, execute o procedimento a seguir em um servidor de Federação de parceiro de recurso na organização do parceiro de recurso.  
  
A associação em **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
## <a name="to-create-a-claims-provider-trust-manually"></a>Para criar manualmente uma relação de confiança do provedor de declarações  
  
1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Em **ações**, clique em **Adicionar confiança do provedor de declarações**.  
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Na página **Bem-vindo**, clique em **Iniciar**. 
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Na página **Selecionar Fonte de Dados**, clique em **Inserir manualmente dados da relação de confiança do provedor de declarações** e depois clique em **Avançar**.  
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim3.PNG)     

5.  Na página **Especificar Nome para Exibição**, digite um **Nome para exibição**; em **Observações**, digite uma descrição para essa relação de confiança do provedor de declarações e clique em **Avançar**.  
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim4.PNG)     

6.  Na página **Configurar URL** , ESPECIFIQUE a **URL passiva do WS-Federation** , se aplicável, e clique em **Avançar**.
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim5.PNG)     

8. Na página **Configurar Identificador**, em **Identificador da confiança do provedor de declarações**, digite o identificador apropriado e clique em **Avançar**.  
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim6.PNG)    

9. Na página **Configurar Certificados**, clique em **Adicionar** para localizar um arquivo de certificado e adicioná-lo à lista de certificados. Em seguida, clique em **Avançar**.  
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim7.PNG)    

10. Na página **Pronto para Adicionar Confiança**, clique em **Avançar** para salvar as informações sobre confianças do provedor de declarações.  
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim8.PNG)    

11. Na página **Concluir**, clique em **Fechar**. Essa ação exibe automaticamente a caixa de diálogo **Editar Regras de Declaração**. Para obter mais informações sobre como continuar com a adição de regras de declaração para essa confiança do provedor de declarações, consulte as referências adicionais a seguir.  
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim9.PNG)

## <a name="to-create-a-claims-provider-trust-using-federation-metadata"></a>Para criar uma confiança do provedor de declarações usando metadados de Federação
Para adicionar uma nova confiança do provedor de declarações, usando o snap-in de gerenciamento de AD FS, importando automaticamente os dados de configuração sobre o parceiro de metadados de Federação que o parceiro publicou em uma rede local ou na Internet, execute o seguinte procedimento em um servidor de Federação na organização do parceiro de recurso.

>[!NOTE]
>Embora tenha sido uma prática comum usar certificados com nomes de host não qualificados, como https:\//MyServer, esses certificados não têm nenhum valor de segurança e podem permitir que um invasor represente um Serviço de Federação que esteja publicando metadados de Federação. Portanto, ao consultar metadados de Federação, você deve usar apenas um nome de domínio totalmente qualificado, como `https://myserver.contoso.com`.

1.  Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**.  
  
2.  Em **ações**, clique em **Adicionar confiança do provedor de declarações**.  
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim1.PNG)   
  
3.  Na página **Bem-vindo**, clique em **Iniciar**. 
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim2.PNG)    
  
4.  Na página **Selecionar Fonte de Dados**, clique em **Importar dados sobre o provedor de declarações publicados online ou em uma rede local**. Em endereço de metadados de Federação (nome do host ou URL), digite a **URL de metadados de Federação** ou o nome do host do parceiro e clique em **Avançar**.
![confiança do provedor de declarações](media/Create-a-Claims-Provider-Trust/addclaim10.PNG)    

5.  Na página Especificar nome de exibição, digite um **nome para exibição**, em observações, digite uma descrição para a confiança do provedor de declarações e clique em **Avançar**.

6.  Na página pronto para adicionar confiança, clique em **Avançar** para salvar as informações de confiança do provedor de declarações.

7.  Na página concluir, clique em **fechar**. Essa ação exibirá automaticamente a caixa de diálogo Editar regras de declaração. Para obter mais informações sobre como continuar com a adição de regras de declaração para essa confiança do provedor de declarações, consulte a seção referências adicionais abaixo.



    
## <a name="additional-references"></a>Referências adicionais  
[Lista de verificação: Configurando a organização do parceiro de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md)  
  
[Lista de verificação: Criando regras de declaração para uma confiança do provedor de declarações](../../ad-fs/deployment/Checklist--Creating-Claim-Rules-for-a-Claims-Provider-Trust.md)  
  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
  
