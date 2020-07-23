---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Adicionar um repositório de atributos
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 352c66bbbd2aade0f212605ca1416fb6581dab0d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962658"
---
# <a name="add-an-attribute-store"></a>Adicionar um repositório de atributos


Contas de usuário e contas de computador que exigem acesso a um recurso protegido pelo Serviços de Federação do Active Directory (AD FS) \( AD FS \) são armazenadas em um repositório de atributos, como Active Directory Domain Services \( AD DS \) . O mecanismo de emissão de declarações usa repositórios de atributos para coletar dados necessários para emitir declarações. Os dados dos repositórios de atributos são então projetados como declarações.  
  
Você pode usar o procedimento a seguir para adicionar um repositório de atributos ao Serviço de Federação.  
  
A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Para adicionar um repositório de atributos  
  
1.  Abra o **Gerenciamento de AD FS**.  
  
2.  Em **ações** , clique em **Adicionar um repositório de atributos**.  

![Adicionar repositório de atributos](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. Na caixa de diálogo **Adicionar um repositório de atributos** , configure as seguintes propriedades para o repositório de atributos que você deseja adicionar:  
  
   -   Em **nome de exibição**, digite o nome que você deseja usar para identificar o repositório de atributos.  
  
   -   Em **tipo de repositório de atributo**, selecione um tipo de repositório de atributo com suporte, **Active Directory**, **LDAP**ou **SQL**.  
  
   -   Em **cadeia de conexão**, se você tiver selecionado um armazenamento LDAP de protocolo de acesso de diretório Lightweight \( \) ou um linguagem SQL \( SQL \) Store, insira a cadeia de caracteres usada para estabelecer uma conexão com o repositório de atributos. Para Active Directory repositórios de atributo, nenhuma cadeia de conexão é necessária; Portanto, esse campo é desabilitado.  
  
       > [!NOTE]  
       > Por padrão, o AD FS cria automaticamente um repositório de atributos do Active Directory.  
 
![Adicionar repositório de atributos](media/Add-an-Attribute-Store/addstore2.PNG) 

4. Clique em **OK**.  
  
## <a name="additional-references"></a>Referências adicionais  

[Operações do AD FS](../ad-fs-operations.md)
  
[A função de repositórios de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
