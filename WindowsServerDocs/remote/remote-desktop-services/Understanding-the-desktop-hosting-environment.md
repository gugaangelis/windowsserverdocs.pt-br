---
title: Noções básicas sobre o ambiente de hospedagem da área de trabalho
description: Visão geral de um deployhment RDS usando IaaS do Azure.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 880fc8f9fa2db5ec56d2117e02c069650c61584a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877917"
---
# <a name="understanding-the-desktop-hosting-environment"></a>Noções básicas sobre o ambiente de hospedagem da área de trabalho

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

As informações a seguir descrevem os componentes do serviço de hospedagem de área de trabalho.  
  
## <a name="tenant-environment"></a>Ambiente de locatário  
O serviço de hospedagem da área de trabalho do provedor é implementado como um conjunto de ambientes de locatários isoladas. Ambiente de cada locatário consiste em um contêiner de armazenamento, um conjunto de máquinas virtuais e uma combinação de serviços do Azure, todos os comunicando-se ao longo de uma rede virtual isolada. Cada máquina virtual contém um ou mais dos componentes que compõem o ambiente de área de trabalho hospedado do locatário. As subseções a seguir descrevem os componentes que compõem o ambiente de área de trabalho hospedado de cada locatário.

## <a name="remote-desktop-services"></a>Serviços da Área de Trabalho Remota
Em um ambiente de hospedagem da área de trabalho, as seguintes funções de serviços de área de trabalho remota são instaladas entre diversas máquinas virtuais:

  - Agente de Conexão de Área de Trabalho Remota
  - Gateway de Área de Trabalho Remota
  - Licenciamento de Área de Trabalho Remota
  - Host de Sessão de Área de Trabalho Remota
  - Acesso via Web da área de trabalho remota

Para obter uma descrição completa de cada uma dessas funções e como eles interagem entre si, examine os [funções de Noções básicas sobre RDS](Understanding-RDS-roles.md) documento.
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Serviços de domínio do Active Directory  
Há várias maneiras de conectar e gerenciar os serviços de domínio do Active Directory (AD DS) para um ambiente de hospedagem da área de trabalho no Azure:

1. Criar uma máquina virtual no ambiente do locatário executando a função AD DS
2. Criar uma conexão de VPN site a site com o no ambiente local do locatário para usar um existente do AD DS
3. Use a função de PaaS do Azure AD Domain Services, que cria um domínio na rede virtual do locatário, com base em do Azure Active Directory do locatário

Com os serviços de área de trabalho remota, o locatário deve ter um Active Directory para gerenciar o acesso ao ambiente, armazenamento de perfil do usuário e o monitoramento dentro da implantação. Ao usar o padrão (não Azure) AD DS, floresta do locatário não exige qualquer relação de confiança com a floresta de gerenciamento do provedor. Uma conta de administrador de domínio pode ser configurada para permitir que equipe de suporte técnico do provedor para executar tarefas administrativas no ambiente do locatário (como monitorar o status do sistema e aplicação de atualizações de software) e para ajudá-lo no domínio do locatário solução de problemas e configuração.  
    
Informações adicionais:  
[Documentação do Azure Active Directory Domain Services](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[Instalar uma nova floresta do Active Directory em uma rede virtual do Azure](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)  
[Criar um recurso de Gerenciador de rede virtual com uma conexão VPN Site a Site usando o Portal do Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  
  
## <a name="azure-sql-database"></a>Banco de Dados do SQL Azure  
Banco de dados SQL do Azure permite que hosters estender sua implantação de serviços de área de trabalho remota sem a necessidade de implantar e manter um cluster AlwaysOn do SQL Server completo. O banco de dados do SQL Azure é usado pelo agente de Conexão de área de trabalho remota para armazenar informações de implantação, como o mapeamento de conexões de usuários atuais com os servidores de host final. Como outros serviços do Azure, BD SQL do Azure segue um modelo de consumo, ideal para a implantação de qualquer tamanho.   
  
Informações adicionais:  
[O que é o banco de dados SQL?](https://azure.microsoft.com/documentation/articles/sql-database-technical-overview/)  
  
## <a name="azure-active-directory-application-proxy"></a>Proxy de aplicativo do Azure Active Directory  
Proxy de aplicativo do Active Directory do Azure é um serviço fornecido em pago-SKUs do Azure Active Directory que permitem que os usuários se conectem a aplicativos internos por meio do serviço de proxy reverso do Azure. Isso permite que os pontos de extremidade da Web da área de trabalho remota e Gateway de área de trabalho remota a ser escondido dentro da rede virtual, eliminando a necessidade de ser exposta à internet através de um endereço IP público. Isso permite que hosters condensar o número de máquinas virtuais no ambiente do locatário e ainda manter uma implantação completa ainda mais.
  
Informações adicionais:  
[Habilitar Proxy de aplicativo do Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-enable/)  
    
## <a name="file-server"></a>Servidor de arquivos  
O servidor de arquivos fornece pastas compartilhadas usando o protocolo SMB Server Message Block () 3.0. As pastas compartilhadas são usadas para criar e armazenar arquivos de disco de perfil do usuário (. vhdx), fazer backup de dados e para permitir aos usuários um lugar para compartilhar dados com outros usuários na rede virtual do locatário.
  
A VM usada para implantar o servidor de arquivos deve ter um disco de dados do Azure conectada e configurada com pastas compartilhadas. Discos de dados do Azure usam o cache de gravação que garante que as gravações no disco persistem entre as reinicializações da máquina virtual.  
  
Para locatários pequenos, o custo pode ser reduzido pela combinação de servidor de arquivos com a máquina virtual executando as funções do agente de Conexão de área de trabalho remota e licenciamento de área de trabalho remota em uma única máquina virtual no ambiente do locatário.  
  
Informações adicionais  
[Visão geral dos serviços de armazenamento e de arquivo](https://technet.microsoft.com/library/hh831487.aspx)  
[Como anexar um disco de dados a uma máquina Virtual](http://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>Discos de perfil do usuário  
Discos de perfil de usuário permitem que os usuários salvem arquivos e configurações pessoais quando estão conectados a uma sessão em um servidor de Host de sessão de área de trabalho remota em uma coleção e, em seguida, têm acesso aos mesmos arquivos e configurações ao entrar em um servidor de Host de sessão de área de trabalho remota diferente na coleção. Quando o usuário entra pela primeira vez, um disco de perfil do usuário é criado no servidor de arquivos do locatário e que o disco está montado no servidor de Host de sessão de área de trabalho remota ao qual o usuário está conectado. Para cada subsequentes ao entrar, o disco de perfil do usuário está montado para o servidor de host de sessão de área de trabalho remota apropriado, e com cada saída, é desmontado. O conteúdo do disco de perfil só pode ser acessado por esse usuário.  
  


