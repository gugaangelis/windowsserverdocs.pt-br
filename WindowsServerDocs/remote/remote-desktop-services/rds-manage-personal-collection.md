---
title: Gerenciar uma coleção de sessão de área de trabalho pessoal no RDS
description: Saiba como adicionar um RDSH e programas RemoteApp na implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 286c7ba4bd4428669d135c35c825033d22b8f40e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743518"
---
## <a name="manage-your-personal-desktop-session-collections"></a>Gerenciar coleções de sessão de área de trabalho pessoal

Use as informações a seguir para gerenciar uma coleção de sessão de área de trabalho pessoal nos Serviços de Área de Trabalho Remota.

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>Atribuir manualmente um usuário a um host de sessão pessoal
Use o cmdlet **Set-RDPersonalSessionDesktopAssignment** para atribuir manualmente um usuário para um servidor de host de sessão pessoal na coleção. O cmdlet dá suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<string\> 

-User \<string\>

-Name \<string\>

- **–CollectionName** – especifica o nome da coleção de área de trabalho de sessão pessoal. Este parâmetro é necessário.
- **–ConnectionBroker** – especifica o servidor do Agente de Conexão RD (Agente de Conexão de Área de Trabalho Remota) para a implantação da Área de Trabalho Remota. Se você não fornecer um valor, o cmdlet usa o FQDN (Nome de Domínio Totalmente Qualificado) do computador local.
- **–User** – especifica a conta de usuário a ser associada com a área de trabalho de sessão pessoal no formato DOMÍNIO\Usuário. Este parâmetro é necessário.
- **–Name** – especifica o nome do servidor host da sessão. Este parâmetro é necessário. O host da sessão identificado aqui deve ser membro da coleção que o parâmetro **-CollectionName** especifica.

O cmdlet **Import-RDPersonalSessionDesktopAssignment** importa as associações entre contas de usuários e áreas de trabalho de sessão pessoal de um arquivo de texto. O cmdlet dá suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string>

**–Path** especifica o caminho e o nome do arquivo de importação.
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Remover uma atribuição de usuário de um host de sessão pessoal
Use o cmdlet **Remove-RDPersonalSessionDesktopAssignment** para remover a associação entre uma área de trabalho de sessão pessoal e um usuário. O cmdlet dá suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

-User \<string\>

**–Force** força o comando a ser executado sem solicitar a confirmação do usuário.

### <a name="query-user-assignments"></a>Consultar atribuições de usuário
Use o cmdlet **Get-RDPersonalSessionDesktopAssignment** para obter uma lista de áreas de trabalho de sessão pessoal e contas de usuário associadas. O cmdlet dá suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-User \<string\>

-Name \<string\>

É possível executar o cmdlet para consultar o nome da coleção, o nome de usuário, ou o nome da área de trabalho da sessão. Se você especificar apenas o parâmetro **–CollectionName**, o cmdlet retorna uma lista de hosts de sessão e usuários associados. Se você também especificar o parâmetro **–User**, o host de sessão associado a esse usuário é retornado. Se você fornecer o parâmetro **–Name**, o usuário associado a esse host de sessão é retornado. 


O cmdlet **Export-RDPersonalPersonalDesktopAssignment** exporta as associações atuais entre usuários e áreas de trabalho virtuais pessoais para um arquivo de texto. O cmdlet dá suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Path \<string\>


Todos os novos cmdlets oferecem suporte aos parâmetros comuns: -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer e -OutVariable. Para obter mais informações, consulte [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
