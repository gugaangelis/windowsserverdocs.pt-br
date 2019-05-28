---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configurando a organizações de parceiros
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 3d7389ce806a5e3aebf4fe166b10e5262df0be8a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192242"
---
# <a name="configuring-partner-organizations"></a>Configurando a organizações de parceiros

Para implantar uma nova organização do parceiro nos serviços de Federação do Active Directory \(do AD FS\), conclua as tarefas em uma [lista de verificação: Configurando a organização do parceiro de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md) ou [lista de verificação: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md), dependendo do seu design do AD FS.  
  
> [!NOTE]  
> Ao usar uma dessas listas de verificação, é altamente recomendável que você leia primeiro as referências ao parceiro de conta ou parceiro de recurso diretrizes de planejamento a [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de passar para o procedimentos para configurar a nova organização do parceiro. A lista de verificação dessa maneira a seguir o ajudará a fornecer uma melhor compreensão sobre o AD FS design e implantação história completa para a organização de parceiro de conta parceiro ou recurso.  
  
## <a name="about-account-partner-organizations"></a>Sobre organizações do parceiro de conta  
Um parceiro de conta é a organização na relação de confiança de federação que armazena fisicamente as contas de usuário em um repositório do AD FS – suporte de atributo. O parceiro de conta é responsável por coletar e autenticar as credenciais do usuário, criação de declarações para o usuário e empacotamento das declarações em tokens de segurança. Esses tokens podem ser apresentados em uma relação de confiança de federação para habilitar o acesso à Web\-com base em recursos que estão localizados na organização do parceiro de recurso.  
  
Em outras palavras, um parceiro de conta representa a organização para aqueles usuários que a conta\-servidor de Federação lado emite tokens de segurança. O servidor de federação na organização do parceiro de conta autentica os usuários locais e cria tokens de segurança que usa o parceiro de recurso na tomada de decisões de autorização.  
  
Em relação a repositórios de atributos, o parceiro de conta no AD FS é conceitualmente equivalente a uma única floresta do Active Directory cujas contas precisam acessar recursos que estão fisicamente localizados em outra floresta. Contas nesta floresta podem acessar recursos na floresta de recursos somente quando uma relação de confiança externa ou relação existe entre as duas florestas e os recursos aos quais os usuários estão tentando obter acesso foram definidos com a autorização adequada de confiança de floresta permissões.  
  
## <a name="about-resource-partner-organizations"></a>Sobre organizações parceiras  
O parceiro de recurso é a organização em uma implantação do AD FS onde servidores Web estão localizados. O parceiro de recurso confia no parceiro de conta para autenticar usuários. Portanto, para tomar decisões de autorização, o parceiro de recurso consome as declarações que são empacotadas em tokens de segurança que vêm de usuários no parceiro de conta.  
  
Em outras palavras, um parceiro de recurso representa a organização cujos servidores Web são protegidos pelo recurso\-servidor de federação de lado. O servidor de federação no parceiro de recurso usa os tokens de segurança que são produzidos pelo parceiro de conta para tomar decisões de autorização para servidores Web no parceiro de recurso.  
  
Para funcionar como um recurso do AD FS, servidores Web na organização do parceiro de recurso devem possuir o Windows Identity Foundation \(WIF\) instalado ou tem os serviços de Federação do Active Directory \(AD FS\) 1.x Declarações\-serviços de função Agente Web com reconhecimento instalados. Servidores que funcionam como um recurso do AD FS pode hospedar qualquer Web Web\-navegador\-com base ou da Web\-service\-com base em aplicativos.  
