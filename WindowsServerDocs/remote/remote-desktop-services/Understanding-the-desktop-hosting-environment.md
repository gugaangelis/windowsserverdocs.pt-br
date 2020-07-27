---
title: Noções básicas sobre o ambiente de hospedagem de área de trabalho
description: Visão geral de uma implantação de RDS usando a IaaS do Azure.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 29d7417ff82be77efeb02d16093c88c53777d1fa
ms.sourcegitcommit: f305bc5f1c5a44dac62f4288450af19f351f9576
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87118636"
---
# <a name="understanding-the-desktop-hosting-environment"></a>Noções básicas sobre o ambiente de hospedagem de área de trabalho

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

As informações a seguir descrevem os componentes do serviço de hospedagem de área de trabalho.  
  
## <a name="tenant-environment"></a>Ambiente de locatário  
O serviço de hospedagem de área de trabalho do provedor é implementado como um conjunto de ambientes de locatários isolados. O ambiente de cada locatário consiste em um contêiner de armazenamento, um conjunto de máquinas virtuais e uma combinação de serviços do Azure, tudo se comunicando sobre uma rede virtual isolada. Cada máquina virtual contém um ou mais dos componentes que compõem o ambiente de área de trabalho hospedada do locatário. As subseções a seguir descrevem os componentes que compõem o ambiente de área de trabalho hospedada de cada locatário.

## <a name="remote-desktop-services"></a>Serviços da área de trabalho Remota
Em um ambiente de hospedagem de área de trabalho, as seguintes funções de Serviços de Área de Trabalho Remota são instaladas entre diversas máquinas virtuais:

  - Agente de Conexão de Área de Trabalho Remota
  - Gateway de Área de Trabalho Remota
  - Licenciamento de Área de Trabalho Remota
  - Host de Sessão de Área de Trabalho Remota
  - Acesso via Web à Área de Trabalho Remota

Para obter uma descrição completa de cada uma dessas funções e saber como elas interagem entre si, examine o documento [Noções básicas sobre funções de RDS](Understanding-RDS-roles.md).
  
##  <a name="azure-active-directory-domain-services"></a>(Azure) Active Directory Domain Services  
Há várias maneiras de conectar e gerenciar o AD DS (Active Directory Domain Services) para um ambiente de hospedagem de área de trabalho no Azure:

1. Criar uma máquina virtual no ambiente do locatário executando a função AD DS
2. Criar uma conexão de VPN site a site com o ambiente local do locatário para usar um AD DS existente
3. Usar a função PaaS do Azure AD Domain Services, a qual cria um domínio na rede virtual do locatário com base no Azure Active Directory do locatário

Com Serviços de Área de Trabalho Remota, o locatário deve ter um Active Directory para gerenciar o acesso ao ambiente, o armazenamento de perfis do usuário e o monitoramento dentro da implantação. Ao usar o AD DS padrão (não Azure), a floresta do locatário não exige qualquer relação de confiança com a floresta de gerenciamento do provedor. Uma conta de administrador de domínio pode ser configurada no domínio do locatário para permitir que a equipe de suporte técnico do provedor execute tarefas administrativas no ambiente do locatário (tais como monitorar o status do sistema e aplicar atualizações de software) e para ajudar com solução de problemas e configuração.  
    
Informações adicionais:  
[Documentação do Azure Active Directory Domain Services](https://azure.microsoft.com/documentation/services/active-directory-ds/)  
[Instalar uma nova floresta do Active Directory em uma rede virtual do Azure](../../identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100.md)  
[Criar um uma VNet do gerenciador de recursos com uma conexão VPN Site a Site usando o portal do Azure](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)  
  
## <a name="azure-sql-database"></a>Banco de Dados do SQL Azure  
O Banco de Dados SQL do Azure permite que hosters estendam a implantação de Serviços de Área de Trabalho Remota sem a necessidade de implantar e manter um cluster AlwaysOn do SQL Server completo. O Banco de Dados SQL do Azure é usado pelo Agente de Conexão da Área de Trabalho Remota para armazenar informações de implantação, como o mapeamento das conexões dos usuários atuais com os servidores host final. Como outros serviços do Azure, o Azure SQL DB segue um modelo de consumo ideal para a implantação de qualquer tamanho.   
  
Informações adicionais:  
[O que é o Banco de Dados SQL?](/azure/azure-sql/database/sql-database-paas-overview)  
  
## <a name="azure-active-directory-application-proxy"></a>Proxy de Aplicativo do Azure Active Directory  
Proxy de Aplicativo do Azure Active Directory é um serviço fornecido em SKUs pagas do Azure Active Directory que permite aos usuários conectarem a aplicativos internos por meio do próprio serviço de proxy reverso do Azure. Isso permite que os pontos de extremidade da Web da Área de Trabalho Remota e do Gateway de Área de Trabalho Remota sejam ocultados dentro da rede virtual, eliminando a necessidade de serem expostos à internet por meio de um endereço IP público. Isso ainda permite que hosters condensem o número de máquinas virtuais no ambiente do locatário, enquanto continuam mantendo uma implantação completa.
  
Informações adicionais:  
[Habilitar o Proxy de Aplicativo do Azure AD](/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)  
    
## <a name="file-server"></a>Servidor de arquivos  
O servidor de arquivos fornece pastas compartilhadas usando o protocolo SMB (Server Message Block) 3.0. As pastas compartilhadas são usadas para criar e armazenar arquivos de disco de perfil do usuário (.vhdx), fazer backup de dados e disponibilizar aos usuários um lugar para compartilhar dados com outros usuários na rede virtual do locatário.
  
A VM usada para implantar o servidor de arquivos deve ter um disco de dados do Azure conectada e configurada com pastas compartilhadas. Discos de dados do Azure usam o cache com write-through que garante a persistência das gravações no disco entre as reinicializações da VM.  
  
Para locatários pequenos, o custo pode ser reduzido pela combinação do servidor de arquivos com a máquina virtual executando as funções de Agente de Conexão de Área de Trabalho Remota e de Licenciamento de Área de Trabalho Remota em uma única máquina virtual no ambiente do locatário.  
  
Informações adicionais  
[Visão geral dos Serviços de Arquivo e Armazenamento](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831487(v=ws.11))  
[Como anexar um disco de dados a uma Máquina Virtual](https://www.windowsazure.com/manage/windows/how-to-guides/attach-a-disk/)  
  
### <a name="user-profile-disks"></a>Discos de Perfil de Usuário  
Discos de perfil de usuário permitem que os usuários salvem arquivos e configurações pessoais quando estão conectados a uma sessão em um servidor Host da Sessão da Área de Trabalho Remota em uma coleção e, em seguida, têm acesso aos mesmos arquivos e configurações ao entrar em um servidor Host da Sessão da Área de Trabalho Remota diferente na coleção. Quando o usuário entra pela primeira vez, um disco de perfil de usuário é criado no servidor de arquivos do locatário e esse disco é montado no servidor Host da Sessão da Área de Trabalho Remota ao qual o usuário está conectado. Para cada entrada subsequente, o disco de perfil de usuário é montado no servidor Host da Sessão da Área de Trabalho Remota apropriado e, com cada saída, o disco é desmontado. O conteúdo do disco de perfil somente pode ser acessado por esse usuário.  
  
