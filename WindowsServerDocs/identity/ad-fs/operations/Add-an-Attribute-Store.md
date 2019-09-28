---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Adicionar um repositório de atributos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0f5c9d3b0f856ab72a16930ddb5c50686d747ecc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358392"
---
# <a name="add-an-attribute-store"></a>Adicionar um repositório de atributos


Contas de usuário e contas de computador que exigem acesso a um recurso protegido por Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 são armazenadas em um repositório de atributos, como Active Directory Domain Services \(AD DS @ no__t-3. O mecanismo de emissão de declarações usa repositórios de atributos para coletar dados necessários para emitir declarações. Os dados dos repositórios de atributos são então projetados como declarações.  
  
Você pode usar o procedimento a seguir para adicionar um repositório de atributos ao Serviço de Federação.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Para adicionar um repositório de atributos  
  
1.  Abra o **Gerenciamento de AD FS**.  
  
2.  Em **ações** , clique em **Adicionar um repositório de atributos**.  

![Adicionar repositório de atributos](media/Add-an-Attribute-Store/addstore1.PNG)
  
3. Na caixa de diálogo **Adicionar um repositório de atributos** , configure as seguintes propriedades para o repositório de atributos que você deseja adicionar:  
  
   -   Em **nome de exibição**, digite o nome que você deseja usar para identificar o repositório de atributos.  
  
   -   Em **tipo de repositório de atributo**, selecione um tipo de repositório de atributo com suporte, **Active Directory**, **LDAP**ou **SQL**.  
  
   -   Em **cadeia de conexão**, se você tiver selecionado um protocolo de acesso de diretório leve \(LDAP @ no__t-2 ou um repositório linguagem SQL \(SQL @ no__t-4, insira a cadeia de caracteres que você usou para estabelecer uma conexão com o atributo armazenadas. Para Active Directory repositórios de atributo, nenhuma cadeia de conexão é necessária; Portanto, esse campo é desabilitado.  
  
       > [!NOTE]  
       > O AD FS cria automaticamente um repositório de atributos de Active Directory, por padrão.  
 
![Adicionar repositório de atributos](media/Add-an-Attribute-Store/addstore2.PNG) 

4. Clique em **OK**.  
  
## <a name="additional-references"></a>Referências adicionais  

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
  
[A função de repositórios de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
