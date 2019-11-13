---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Quando criar um farm de servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fcfc7d640d3688bf0e23557af9bd56082418ef37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358944"
---
# <a name="when-to-create-a-federation-server-farm"></a>Quando criar um farm de servidor de federação

Considere criar um farm de servidores de Federação no Serviços de Federação do Active Directory (AD FS) \(AD FS\) quando você tiver uma implantação de AD FS maior e desejar fornecer tolerância a falhas, carregar\-balanceamento ou escalabilidade para o Serviço de Federação da sua organização. O ato de criar dois ou mais servidores de Federação na mesma rede, configurar cada um deles para usar o mesmo Serviço de Federação e adicionar a chave pública do token de cada servidor\-certificados de assinatura ao\-snap de gerenciamento de AD FS no cria um farm de servidores de Federação.  
  
Você pode criar um farm de servidores de Federação ou instalar servidores de Federação adicionais em um farm existente usando o assistente de configuração do servidor de Federação AD FS. Para obter mais informações, consulte [When to Create a Federation Server](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando você escolhe a opção para criar um **novo farm de servidores de Federação** usando o assistente de configuração do servidor de Federação AD FS, o assistente tentará criar um objeto de contêiner \(para compartilhar certificados\) no Active Directory. Portanto, é importante que você primeiro faça logon no computador em que está configurando a função do servidor de federação com uma conta que tenha permissões suficientes no Active Directory para criar o objeto de contêiner.  
  
Antes que os servidores de Federação possam ser agrupados como um farm, eles devem primeiro ser clusterizados para que as solicitações que chegam a um único nome de domínio totalmente qualificado \(FQDN\) sejam roteadas para os vários servidores de Federação no farm de servidores. Você pode criar o cluster de servidores implantando o balanceamento de carga de rede \(\) NLB dentro da rede corporativa. Este guia pressupõe que o NLB foi configurado adequadamente para clusterizar cada um dos servidores de Federação no farm.  
  
Para obter mais informações sobre como configurar um FQDN de cluster usando a tecnologia Microsoft NLB, consulte [especificando os parâmetros de cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Práticas recomendadas para implantação de um farm de servidores de federação  
Recomendamos as seguintes práticas recomendadas para implantar um servidor de Federação em um ambiente de produção:  
  
-   Se você estiver implantando vários servidores de Federação ao mesmo tempo ou se souber que adicionará mais servidores ao farm ao longo do tempo, considere criar uma imagem de servidor de um servidor de Federação existente no farm e, em seguida, instalar a partir dessa imagem quando precisar de CR Iar servidores de Federação adicionais rapidamente.  
  
    > [!NOTE]  
    > Se você decidir usar o método de imagem do servidor para implantar servidores de Federação adicionais, não será necessário concluir as tarefas na lista de [verificação: configurar um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) sempre que desejar adicionar um novo servidor ao farm.  
  
-   Use o NLB ou alguma outra forma de clustering para alocar um único endereço IP para muitos computadores do servidor de Federação.  
  
-   Reserve um endereço IP estático para cada servidor de Federação no farm e, dependendo de seu sistema de nome de domínio \(configuração de\) DNS, insira uma exclusão para cada endereço IP no protocolo de configuração dinâmica de hosts \(\)DHCP. A tecnologia NLB da Microsoft requer que a cada servidor que participa do cluster NLB seja atribuído um endereço IP estático.  
  
-   Se o banco de dados de configuração do AD FS for armazenado em um banco de dados SQL, evite editar o banco de dados SQL de vários servidores de Federação ao mesmo tempo.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configurando servidores de federação para um farm  
A tabela a seguir descreve as tarefas que devem ser concluídas para que cada servidor de federação possa participar de um ambiente de farm.  
  
|Tarefa|Descrição|  
|--------|---------------|  
|Se você estiver usando o SQL Server para armazenar o banco de dados de configuração do AD FS|Um farm de servidores de Federação consiste em dois ou mais servidores de Federação que compartilham o mesmo AD FS banco de dados de configuração e token\-certificados de assinatura. O banco de dados de configuração pode ser armazenado em um banco de dados interno do Windows ou em um banco de dados do SQL Server. Se você planeja armazenar o banco de dados de configuração em um banco de dados SQL, verifique se o banco de dados de configuração está acessível para que ele possa ser acessado por todos os novos servidores de Federação que participam do farm. **Observação:** Para cenários de farm, é importante que o banco de dados de configuração esteja localizado em um computador que não participe também de um servidor de Federação nesse farm. O NLB da Microsoft não permite que nenhum dos computadores que participam de um farm se comuniquem entre si. **Observação:** Verifique se a identidade do AD FS AppPool em Serviços de Informações da Internet \(IIS\)\) em todos os servidores de Federação que participam do farm têm acesso de leitura ao banco de dados de configuração.|  
|Obter e compartilhar certificados|Você pode obter um único certificado de autenticação de servidor de uma autoridade de certificação pública \(\)da AC, por exemplo, VeriSign. Em seguida, você pode configurar o certificado para que todos os servidores de Federação compartilhem a mesma parte de chave privada do certificado. Para obter mais informações sobre como compartilhar o mesmo certificado, consulte [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Observação:** O\-snap de gerenciamento de AD FS no refere-se aos certificados de autenticação de servidor para servidores de Federação como certificados de comunicação de serviço.<br /><br />Para obter mais informações, consulte [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).|  
|Apontar para a mesma instância do SQL Server|Se o banco de dados de configuração do AD FS for armazenado em um banco de dados SQL, o novo servidor de Federação deverá apontar para a mesma instância de SQL Server que é usada por outros servidores de Federação no farm para que o novo servidor possa participar do farm.|  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
