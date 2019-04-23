---
title: Gerenciar uma coleção de sessão de área de trabalho pessoal no RDS
description: Saiba como adicionar e programas de RDSH e RemoteApp para sua implantação do RDS.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865717"
---
## <a name="manage-your-personal-desktop-session-collections"></a>Gerenciar as coleções de sessão de área de trabalho pessoal

Use as informações a seguir para gerenciar uma coleção de sessão de área de trabalho pessoal nos serviços de área de trabalho remota.

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>Atribuir manualmente um usuário a um host de sessão pessoal
Use o **RDPersonalSessionDesktopAssignment conjunto** cmdlet para atribuir manualmente um usuário para um servidor de host de sessão pessoal na coleção. O cmdlet oferece suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<cadeia de caracteres\> 

-Usuário \<cadeia de caracteres\>

-Name \<string\>

- **– CollectionName** -Especifica o nome da coleção de área de trabalho de sessão pessoal. Este parâmetro é necessário.
- **– ConnectionBroker** -Especifica o servidor do agente de Conexão de área de trabalho remota (agente de Conexão de área de trabalho remota) para sua implantação de área de trabalho remota. Se você não fornecer um valor, o cmdlet usa o nome de domínio totalmente qualificado (FQDN) do computador local.
- **– Usuário** -Especifica a conta de usuário para associar com a área de trabalho de sessão pessoal, no formato domínio \ usuário. Este parâmetro é necessário.
- **– Nome** -Especifica o nome do servidor de host de sessão. Este parâmetro é necessário. O host de sessão identificado aqui deve ser um membro da coleção que o **- CollectionName** parâmetro especifica.

O **importação RDPersonalSessionDesktopAssignment** cmdlet importa as associações entre contas de usuário e áreas de trabalho de sessão pessoal de um arquivo de texto. O cmdlet oferece suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<cadeia de caracteres\>

-Path \<string>

**– Caminho** Especifica o nome de arquivo e caminho de um arquivo para importar.
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Remover uma atribuição de usuário de um Host de sessão pessoal
Use o **RDPersonalSessionDesktopAssignment remover** cmdlet para remover a associação entre uma área de trabalho de sessão pessoal e um usuário. O cmdlet oferece suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<cadeia de caracteres\>

-Force

-Name \<string\>

-Usuário \<cadeia de caracteres\>

**– Forçar** força o comando a ser executado sem solicitar confirmação do usuário.

### <a name="query-user-assignments"></a>Atribuições de usuário de consulta
Use o **Get-RDPersonalSessionDesktopAssignment** para obter uma lista de áreas de trabalho de sessão pessoal e contas de usuário associado. O cmdlet oferece suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<cadeia de caracteres\>

-Usuário \<cadeia de caracteres\>

-Name \<string\>

Você pode executar o cmdlet a consulta pelo nome da coleção, nome de usuário, ou pelo nome da sessão da área de trabalho. Se você especificar apenas o **CollectionName –** parâmetro, o cmdlet retorna uma lista de hosts de sessão e usuários associados. Se você também especificar o **– usuário** parâmetro, o host de sessão associado a esse usuário será retornado. Se você fornecer a **– nome** parâmetro, o usuário associado a esse host de sessão é retornado. 


O **RDPersonalPersonalDesktopAssignment exportação** cmdlet exporta as associações entre usuários e áreas de trabalho virtuais pessoais de atuais para um arquivo de texto. O cmdlet oferece suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<cadeia de caracteres\>

-Path \<string\>


Todos os novos cmdlets do dão suporte a parâmetros comuns:-Verbose,-Debug, - ErrorAction, - ErrorVariable,-OutBuffer e - OutVariable. Para obter mais informações, consulte [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
