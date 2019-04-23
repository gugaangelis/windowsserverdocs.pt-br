---
ms.assetid: c60227a8-7b44-40f8-b807-a6532851a4a6
title: Adicionar um repositório de atributos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 11baba5bfdb699f120a506feb8361db21d26cff1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837857"
---
# <a name="add-an-attribute-store"></a>Adicionar um repositório de atributos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Contas de usuário e contas de computador que requerem acesso a um recurso que é protegido pelos serviços de Federação do Active Directory \(do AD FS\) são armazenados em um repositório de atributos, como o Active Directory Domain Services \(AD DS \). O mecanismo de emissão de declarações usa repositórios de atributos para coletar dados que é necessários emitir declarações. Dados de repositórios de atributos são então projetados como declarações.  
  
Você pode usar o procedimento a seguir para adicionar um repositório de atributos para o serviço de Federação.  
  
A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
#### <a name="to-add-an-attribute-store"></a>Para adicionar um repositório de atributos  
  
1.  Abra **gerenciamento do AD FS**.  
  
2.  Sob **ações** clique em **adicionar um repositório de atributos**.  

![Adicionar repositório de atributos](media/Add-an-Attribute-Store/addstore1.PNG)
  
3.  No **adicionar um repositório de atributos** caixa de diálogo caixa, configure as seguintes propriedades para o repositório de atributos que você deseja adicionar:  
  
    -   Na **nome de exibição**, digite o nome que você deseja usar para identificar o repositório de atributos.  
  
    -   Na **tipo de repositório de atributo**, selecione um tipo de repositório de atributos com suporte, ou **do Active Directory**, **LDAP**, ou **SQL**.  
  
    -   Na **cadeia de caracteres de Conexão**, se você tiver selecionado a um Lightweight Directory Access Protocol \(LDAP\) repositório ou uma linguagem de consulta estruturada \(SQL\) armazenar, insira a cadeia de caracteres que é usado para estabelecer uma conexão para o repositório de atributos. Para repositórios de atributos do Active Directory, nenhuma cadeia de conexão é necessária; Portanto, este campo será desabilitado.  
  
        > [!NOTE]  
        > Por padrão, o AD FS cria automaticamente um repositório de atributos do Active Directory.  
 
![Adicionar repositório de atributos](media/Add-an-Attribute-Store/addstore2.PNG) 

4.  Clique em **OK**.  
  
## <a name="additional-references"></a>Referências adicionais  

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
  
[A função dos repositórios de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)  
