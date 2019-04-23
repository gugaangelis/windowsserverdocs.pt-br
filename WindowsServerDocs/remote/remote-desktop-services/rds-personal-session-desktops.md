---
title: Use as áreas de trabalho de sessão pessoal com os serviços de área de trabalho remota
description: Aprenda a compartilhar personalizada, atribuído a áreas de trabalho por meio de RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 41f6a28c1b754a5277a0966a87dae08a6aa34e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875947"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>Use as áreas de trabalho de sessão pessoal com os serviços de área de trabalho remota

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode implantar áreas de trabalho pessoais baseada em servidor em um ambiente de computação em nuvem usando áreas de trabalho de sessão pessoal.  (Um ambiente de computação em nuvem tem uma separação entre os servidores de malha do Hyper-V e máquinas virtuais convidadas, como a plataforma de nuvem da Microsoft ou de nuvem do Microsoft Azure.) O recurso de área de trabalho de sessão pessoal estende o cenário de implantação de área de trabalho baseadas em sessão nos serviços de área de trabalho remota para criar um novo tipo de coleção de sessão em que cada usuário está atribuído ao seu próprio host da sessão pessoal com direitos administrativos. 

Use as informações a seguir para criar e gerenciar uma coleção de sessão pessoal da área de trabalho.

## <a name="create-a-personal-session-desktop-collection"></a>Criar uma coleção de área de trabalho de sessão pessoal

Use o cmdlet New-RDSessionCollection para criar uma coleção de sessão pessoal da área de trabalho. Os três parâmetros a seguir fornecem as informações de configuração necessárias para áreas de trabalho de sessão pessoal:

- **-PersonalUnmanaged** -Especifica o tipo de coleção de sessão que permite que você atribuir usuários a um servidor de host de sessão pessoal. Se você não especificar esse parâmetro, a coleção é criada como uma coleção de Host de sessão de área de trabalho remota tradicional, onde os usuários são atribuídos para o próximo host de sessão disponíveis quando eles entrarem.
- **-GrantAdministrativePrivilege** – se você usar **- PersonalUnmanaged**, especifica que o usuário atribuído ao host de sessão receber privilégios administrativos. Se você não usar esse parâmetro, os usuários recebem apenas os privilégios de usuário padrão.
- **-AutoAssignUser** – se você usar **- PersonalUnmanaged**, especifica que os novos usuários se conectam por meio do agente de Conexão de área de trabalho são automaticamente atribuídos a um host de sessão não atribuídos. Se não houver nenhum host de sessão não atribuídos na coleção, o usuário verá uma mensagem de erro. Se você não usar esse parâmetro, você precisará [atribuir manualmente os usuários a um host de sessão](#manually-assign-a-user-to-a-personal-session-host) antes que ele entrar.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Atribuir manualmente um usuário a um host de sessão pessoal
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
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Remover uma atribuição de usuário de um Host de sessão pessoal
Use o **RDPersonalSessionDesktopAssignment remover** cmdlet para remover a associação entre uma área de trabalho de sessão pessoal e um usuário. O cmdlet oferece suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<cadeia de caracteres\>

-Force

-Name \<string\>

-Usuário \<cadeia de caracteres\>

**– Forçar** força o comando a ser executado sem solicitar confirmação do usuário.

## <a name="query-user-assignments"></a>Atribuições de usuário de consulta
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

## <a name="hardware-accelerated-graphics"></a>Elementos gráficos aceleração por hardware
Windows Server 2016 estende a tecnologia de (vGPU) do adaptador RemoteFX 3D gráficos para dar suporte a OpenGL e dá suporte a VMs de convidado do Windows Server 2016 de usuário único. Você pode combinar as áreas de trabalho de sessão pessoal com os novos recursos de vGPU para fornecer suporte para aplicativos hospedados que exigem acelerada de elementos gráficos. Como alternativa, você pode combinar as áreas de trabalho de sessão pessoal com o novo recurso de atribuição de dispositivo discretos (DDA) também dar suporte aos aplicativos hospedados que exigem acelerada de elementos gráficos.
