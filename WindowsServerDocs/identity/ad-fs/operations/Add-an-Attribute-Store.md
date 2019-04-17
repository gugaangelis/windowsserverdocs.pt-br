---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: "Adicione um repositório de atributo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-an-attribute-store"></a>Adicione um repositório de atributo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Contas de usuário e contas de computador que exigem acesso a um recurso que está protegido pelo \(AD FS\) serviços de Federação do Active Directory são armazenadas em um repositório de atributo, como serviços de domínio do Active Directory \(AD DS\). O mecanismo de emissão requerimentos judiciais ou Extrajudiciais usa armazenamentos de atributo para coletar dados que é necessários para emitir declarações. Dados das lojas de atributo, em seguida, são projetados como declarações.  
  
Você pode usar o procedimento a seguir para adicionar um repositório de atributo para o serviço de Federação.  
  
A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Para adicionar um repositório de atributo  
  
1.  Abrir **AD FS gerenciamento**.  
  
2.  Em **ações** clique **adicionar um repositório de atributo**.  

![Adicione o repositório de atributo](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  No **adicionar um repositório de atributo** caixa de diálogo caixa, configurar as seguintes propriedades para a loja de atributo que você deseja adicionar:  
  
    -   Em **nome de exibição**, digite o nome que você deseja usar para identificar o repositório de atributo.  
  
    -   Em **tipo de repositório do atributo**, selecione um tipo de repositório do atributo com suporte, ou **do Active Directory**, **LDAP**, ou **SQL**.  
  
    -   Em **cadeia de caracteres de Conexão**, se você tiver selecionado um repositório \(LDAP\) Lightweight Directory Access Protocol ou um repositório de linguagem de consulta estruturada \(SQL\), digite a cadeia de caracteres que você usou para estabelecer uma conexão para a loja de atributo. Para lojas de atributo do Active Directory, nenhuma cadeia de caracteres de conexão é necessária; Portanto, esse campo é desabilitado.  
  
        > [!NOTE]  
        > AD FS cria automaticamente um repositório de atributo do Active Directory, por padrão.  
 
![Adicione o repositório de atributo](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  Clique em **Okey**.  
  
## <a name="additional-references"></a>Referências adicionais  

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
  
[A função dos repositórios de atributo](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
