---
ms.assetid: 4d002764-58b4-4137-9c86-1e55b02e07ce
title: Configurando organizações parceiras
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: 123968fdc9ce9e30b3cf78d25fe37b30e46fd473
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962970"
---
# <a name="configuring-partner-organizations"></a>Configurando organizações parceiras

Para implantar uma nova organização parceira no Serviços de Federação do Active Directory (AD FS) \( AD FS \) , conclua as tarefas em qualquer uma das [lista de verificação: Configurando a organização do parceiro de recursos](Checklist--Configuring-the-Resource-Partner-Organization.md) ou a [lista de verificação: Configurando a organização do parceiro de conta](Checklist--Configuring-the-Account-Partner-Organization.md), dependendo de seu design de AD FS.

> [!NOTE]
> Ao usar qualquer uma dessas listas de verificação, é altamente recomendável que você leia primeiro as referências às diretrizes de planejamento de parceiro de conta ou de parceiro de recurso no [Guia de design de AD FS no Windows Server 2012](../design/ad-fs-design-guide-in-windows-server-2012.md) antes de continuar com os procedimentos para configurar a nova organização parceira. Seguir a lista de verificação dessa forma ajudará a fornecer uma melhor compreensão do design completo de AD FS e da história de implantação para a organização parceiro de conta ou parceiro de recurso.

## <a name="about-account-partner-organizations"></a>Sobre organizações parceiras de contas
Um parceiro de conta é a organização na relação de confiança da Federação que armazena fisicamente as contas de usuário em um repositório de atributos com suporte AD FS. O parceiro de conta é responsável por coletar e autenticar as credenciais de um usuário, criar declarações para esse usuário e empacotar as declarações em tokens de segurança. Esses tokens podem ser apresentados em uma confiança de Federação para permitir o acesso a \- recursos baseados na Web que estão localizados na organização do parceiro de recurso.

Em outras palavras, um parceiro de conta representa a organização de cujos usuários o \- servidor de Federação do lado da conta emite tokens de segurança. O servidor de Federação na organização do parceiro de conta autentica usuários locais e cria tokens de segurança que o parceiro de recurso usa para tomar decisões de autorização.

Em relação a repositórios de atributos, o parceiro de conta no AD FS é conceitualmente equivalente a uma única floresta de Active Directory cujas contas precisam de acesso a recursos que estão fisicamente localizados em outra floresta. As contas nessa floresta podem acessar recursos na floresta de recursos somente quando existe uma relação de confiança externa ou de floresta entre as duas florestas e os recursos aos quais os usuários estão tentando obter acesso com as permissões de autorização adequadas.

## <a name="about-resource-partner-organizations"></a>Sobre organizações de parceiros de recursos
O parceiro de recurso é a organização em uma implantação de AD FS em que os servidores Web estão localizados. O parceiro de recurso confia no parceiro de conta para autenticar usuários. Portanto, para tomar decisões de autorização, o parceiro de recurso consome as declarações que são empacotadas em tokens de segurança provenientes de usuários no parceiro de conta.

Em outras palavras, um parceiro de recurso representa a organização cujos servidores Web são protegidos pelo servidor de Federação do lado do recurso \- . O servidor de Federação no parceiro de recurso usa os tokens de segurança que são produzidos pelo parceiro de conta para tomar decisões de autorização para servidores Web no parceiro de recurso.

Para funcionar como um recurso de AD FS, os servidores Web na organização do parceiro de recurso devem ter o Windows Identity Foundation \( WIF \) instalado ou ter o serviços de Federação do Active Directory (AD FS) \( AD FS \) 1. x \- serviços de função de agente Web com reconhecimento de declaração instalados. Os servidores Web que funcionam como um recurso de AD FS podem hospedar aplicativos baseados em navegador da Web \- \- ou \- em serviços Web \- .
