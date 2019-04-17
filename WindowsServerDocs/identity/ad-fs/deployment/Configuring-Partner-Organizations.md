---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: "Configurando organizações parceiras"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5494f3bd8d012bf1ecc240439ff880d1bb52c280
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="configuring-partner-organizations"></a>Configurando organizações parceiras

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implantar uma nova organização de parceiro no \(AD FS\) serviços de Federação do Active Directory, conclua as tarefas em um [lista de verificação: Configurando a organização do parceiro de recurso](Checklist--Configuring-the-Resource-Partner-Organization.md) ou [lista de verificação: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md), dependendo de seu design do AD FS.  
  
> [!NOTE]  
> Quando você usa qualquer uma dessas listas de verificação, é altamente recomendável que você leia primeiro as referências ao parceiro de conta ou recurso diretrizes de planejamento do [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de continuar os procedimentos para configurar a nova organização de parceiro. Após a lista de verificação dessa forma ajudará a fornecer uma melhor compreensão do total do AD FS design e implantação texto para a organização conta de parceiro de parceiro ou recurso.  
  
## <a name="about-account-partner-organizations"></a>Sobre as organizações de parceiros de conta  
Um parceiro de conta é a organização na relação de confiança de federação que armazena contas de usuário fisicamente em um repositório do AD FS – suporte atributo. O parceiro de conta é responsável por coletar e autenticar as credenciais do usuário, criação de declarações para esse usuário e empacotamento das declarações em tokens de segurança. Esses tokens podem ser apresentados em uma relação de confiança de federação para permitir o acesso aos recursos com base em Web\ que estão localizados na organização do parceiro de recurso.  
  
Em outras palavras, um parceiro de conta representa a organização para o qual os usuários o servidor de Federação do lado do account\ emite tokens de segurança. O servidor de federação na organização do parceiro de conta autentica usuários locais e cria tokens de segurança que usa o parceiro de recurso na tomada de decisões de autorização.  
  
Em relação a repositórios de atributo, o parceiro de conta no AD FS é conceitualmente equivalente a uma única floresta do Active Directory cujas contas precisam de acesso a recursos fisicamente localizados em outra floresta. Contas essa floresta podem acessar recursos na floresta de recursos somente quando uma relação de confiança externa ou de floresta existe relação entre as duas florestas e os recursos aos quais os usuários estão tentando obter acesso foram definidos com as permissões de autorização adequada.  
  
## <a name="about-resource-partner-organizations"></a>Sobre as organizações parceiras de recurso  
O parceiro de recurso é a organização em uma implantação do AD FS onde servidores Web estão localizados. O parceiro de recurso confia o parceiro de conta para autenticar os usuários. Portanto, para tomar decisões de autorização, o parceiro de recurso consome as declarações que são empacotadas em tokens de segurança que vêm de usuários no parceiro de conta.  
  
Em outras palavras, um parceiro de recurso representa a organização cujos servidores Web são protegidas pelo servidor de Federação resource\ lado. O servidor de federação no parceiro de recurso usa os tokens de segurança que são gerados pelo parceiro de conta para tomar decisões de autorização para servidores Web no parceiro de recurso.  
  
Para funcionar como um recurso do AD FS, servidores Web na organização do parceiro de recurso deve ter o Windows Identity Foundation \(WIF\) instalado ou instalou os serviços de função de reconhecimento de Claims\ agente de Web do serviços de Federação do Active Directory \(AD FS\) 1. x. Servidores Web que funcionam como um recurso do AD FS podem hospedar aplicativos com base em browser\ Web\ tanto com base em intelegente Web\.  
