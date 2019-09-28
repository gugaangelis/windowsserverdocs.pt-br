---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configurando organizações parceiras
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 575d7e3fc97496c3f7c147220fe342add66517c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408393"
---
# <a name="configuring-partner-organizations"></a>Configurando organizações parceiras

Para implantar uma nova organização parceira no Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1, conclua as tarefas em [Checklist: Configurando a organização do parceiro de recurso @ no__t-0 ou [Checklist: Configurando a organização do parceiro de conta @ no__t-0, dependendo de seu design de AD FS.  
  
> [!NOTE]  
> Ao usar qualquer uma dessas listas de verificação, é altamente recomendável que você leia primeiro as referências às diretrizes de planejamento de parceiro de conta ou de parceiro de recurso no [Guia de design de AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de continuar com os procedimentos para configurar o nova organização parceira. Seguir a lista de verificação dessa forma ajudará a fornecer uma melhor compreensão do design completo de AD FS e da história de implantação para a organização parceiro de conta ou parceiro de recurso.  
  
## <a name="about-account-partner-organizations"></a>Sobre organizações parceiras de contas  
Um parceiro de conta é a organização na relação de confiança da Federação que armazena fisicamente as contas de usuário em um repositório de atributos com suporte AD FS. O parceiro de conta é responsável por coletar e autenticar as credenciais de um usuário, criar declarações para esse usuário e empacotar as declarações em tokens de segurança. Esses tokens podem ser apresentados em uma confiança de Federação para habilitar o acesso aos recursos Web @ no__t-0based localizados na organização do parceiro de recurso.  
  
Em outras palavras, um parceiro de conta representa a organização de cujos usuários o servidor de Federação da conta @ no__t-0side emite tokens de segurança. O servidor de Federação na organização do parceiro de conta autentica usuários locais e cria tokens de segurança que o parceiro de recurso usa para tomar decisões de autorização.  
  
Em relação a repositórios de atributos, o parceiro de conta no AD FS é conceitualmente equivalente a uma única floresta de Active Directory cujas contas precisam de acesso a recursos que estão fisicamente localizados em outra floresta. As contas nessa floresta podem acessar recursos na floresta de recursos somente quando existe uma relação de confiança externa ou de floresta entre as duas florestas e os recursos aos quais os usuários estão tentando obter acesso com a autorização apropriada permissões.  
  
## <a name="about-resource-partner-organizations"></a>Sobre organizações de parceiros de recursos  
O parceiro de recurso é a organização em uma implantação de AD FS em que os servidores Web estão localizados. O parceiro de recurso confia no parceiro de conta para autenticar usuários. Portanto, para tomar decisões de autorização, o parceiro de recurso consome as declarações que são empacotadas em tokens de segurança provenientes de usuários no parceiro de conta.  
  
Em outras palavras, um parceiro de recurso representa a organização cujos servidores Web são protegidos pelo servidor de Federação do Resource @ no__t-0side. O servidor de Federação no parceiro de recurso usa os tokens de segurança que são produzidos pelo parceiro de conta para tomar decisões de autorização para servidores Web no parceiro de recurso.  
  
Para funcionar como um recurso AD FS, os servidores Web na organização do parceiro de recurso devem ter o Windows Identity Foundation \(WIF @ no__t-1 instalado ou ter o Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-3 1. x Claims @ no__t-4Aware Web Serviços de função do agente instalados. Os servidores Web que funcionam como um recurso AD FS podem hospedar aplicativos Web @ no__t-0browser @ no__t-1based ou Web @ no__t-2service @ no__t-3based.  
