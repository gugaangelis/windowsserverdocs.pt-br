---
title: Usar as áreas de trabalho de sessão pessoal com os Serviços de Área de Trabalho Remota
description: Aprenda a compartilhar áreas de trabalho personalizadas e atribuídas por meio de RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 7429cd9cb87db310a716136c171de47cfe0892f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387363"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>Usar as áreas de trabalho de sessão pessoal com os Serviços de Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Você pode implantar áreas de trabalho pessoais baseadas em servidor em um ambiente de computação em nuvem usando áreas de trabalho de sessão pessoal.  Um ambiente de computação em nuvem tem uma separação entre os servidores de malha do Hyper-V e as máquinas virtuais convidadas, como a Plataforma de Nuvem da Microsoft ou a Microsoft Azure Cloud. O recurso de área de trabalho de sessão pessoal estende o cenário de implantação de área de trabalho baseada em sessão nos Serviços de Área de Trabalho Remota para criar um novo tipo de coleção de sessão em que cada usuário recebe seu próprio host de sessão pessoal com direitos administrativos. 

Use as informações a seguir para criar e gerenciar uma coleção de área de trabalho de sessão pessoal.

## <a name="create-a-personal-session-desktop-collection"></a>Criar uma coleção de área de trabalho de sessão pessoal

Use o cmdlet New-RDSessionCollection para criar uma coleção de sessão da área de trabalho pessoal. Os três parâmetros a seguir fornecem as informações de configuração necessárias para áreas de trabalho com sessão pessoal:

- **-PersonalUnmanaged** – especifica o tipo de coleção de sessão que permite atribuir usuários a um servidor de host de sessão pessoal. Se esse parâmetro não for especificado, a coleção é criada como uma coleção do host de sessão de área de trabalho remota tradicional, na qual os usuários são atribuídos para o próximo host de sessão disponível ao entrarem.
- **-GrantAdministrativePrivilege** – se você usar **-PersonalUnmanaged**, especifica que o usuário atribuído ao host de sessão receberá privilégios administrativos. Se você não usar esse parâmetro, os usuários recebem apenas os privilégios de usuário padrão.
- **-AutoAssignUser** – se você usar **-PersonalUnmanaged**, especifica que os novos usuários que se conectam por meio do agente de conexão de área de trabalho remota serão automaticamente atribuídos a um host de sessão não atribuído. Se não houver nenhum host de sessão não atribuído na coleção, o usuário verá uma mensagem de erro. Se você não usar esse parâmetro, precisará [atribuir manualmente os usuários a um host de sessão](#manually-assign-a-user-to-a-personal-session-host) antes que ele entre.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Atribuir manualmente um usuário a um host de sessão pessoal
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
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Remover uma atribuição de usuário de um host de sessão pessoal
Use o cmdlet **Remove-RDPersonalSessionDesktopAssignment** para remover a associação entre uma área de trabalho de sessão pessoal e um usuário. O cmdlet dá suporte aos seguintes parâmetros:

-CollectionName \<string\>

-ConnectionBroker \<string\>

-Force

-Name \<string\>

-User \<string\>

**–Force** força o comando a ser executado sem solicitar a confirmação do usuário.

## <a name="query-user-assignments"></a>Consultar atribuições de usuário
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


Todos os novos cmdlets dão suporte aos parâmetros comuns: -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer e -OutVariable. Para obter mais informações, consulte [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).

## <a name="hardware-accelerated-graphics"></a>Aceleração de gráficos por hardware
O Windows Server 2016 estende a tecnologia do adaptador gráfico RemoteFX 3D (vGPU) para ser compatível com OpenGL e dar suporte a VMs convidadas do Windows Server 2016 de usuário único. Você pode combinar as áreas de trabalho de sessão pessoal com os novos recursos vGPU para dar suporte aos aplicativos hospedados que exijam a aceleração de gráficos. Como alternativa, você pode combinar as áreas de trabalho de sessão pessoal com o novo recurso DDA (Atribuição de dispositivo discreto) para também dar suporte aos aplicativos hospedados que exijam a aceleração de gráficos.
