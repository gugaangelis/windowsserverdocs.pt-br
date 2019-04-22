---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Quando criar um farm de servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816227"
---
# <a name="when-to-create-a-federation-server-farm"></a>Quando criar um farm de servidor de federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere a criação de um farm de servidores de federação nos serviços de Federação do Active Directory \(do AD FS\) quando você tiver uma implantação maior do AD FS e você deseja fornecer tolerância a falhas, carregar\-balanceamento ou escalabilidade para seu Serviço de Federação da organização. O ato de criar dois ou mais servidores de federação na mesma rede, configurando cada um deles para usar o mesmo serviço de Federação e adicionando o token da chave pública de cada servidor\-certificados de assinatura para o snap de gerenciamento do AD FS\-em cria um farm de servidores de Federação.  
  
Você pode criar um farm de servidores de Federação ou instalar servidores de Federação adicionais a um farm existente, usando o Assistente de configuração do servidor de Federação do AD FS. Para obter mais informações, consulte [When to Create a Federation Server](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando você escolhe a opção de criar uma **novo farm de servidores de Federação** usando o Assistente de configuração do servidor de Federação do AD FS, o assistente tentará criar um objeto de contêiner \(para compartilhamento de certificados\) no Active Directory. Portanto, é importante que você primeiro faça logon no computador em que está configurando a função do servidor de federação com uma conta que tenha permissões suficientes no Active Directory para criar o objeto de contêiner.  
  
Antes de servidores de federação podem ser agrupados como um farm, eles devem primeiro ser clusterizados para que o nome de domínio de solicitações que chegam em uma única totalmente qualificado \(FQDN\) são roteadas para vários servidores de federação no farm de servidores. Você pode criar o cluster de servidor Implantando o balanceamento de carga de rede \(NLB\) dentro da rede corporativa. Este guia pressupõe que NLB foi configurado adequadamente para cluster cada um dos servidores de federação no farm.  
  
Para obter mais informações sobre como configurar um FQDN de cluster usando a tecnologia Microsoft NLB, consulte [especificando os parâmetros de Cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Práticas recomendadas para implantação de um farm de servidores de federação  
Recomendamos as seguintes práticas recomendadas para implantar um servidor de Federação em um ambiente de produção:  
  
-   Se você estiver implantando vários servidores de Federação ao mesmo tempo ou se você souber que você adicionar mais servidores ao farm ao longo do tempo, considere criar uma imagem de servidor de um servidor de Federação existente no farm e, em seguida, instalando a partir dessa imagem quando você precisar cr servidores de Federação adicionais iar rapidamente.  
  
    > [!NOTE]  
    > Se você decidir usar o método de imagem do servidor para a implantação de servidores de Federação adicionais, você não precisa concluir as tarefas no [lista de verificação: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) toda vez que você deseja adicionar um novo servidor ao farm.  
  
-   Use o NLB ou alguma outra forma de clustering para alocar um único endereço IP para diversos computadores de servidor de Federação.  
  
-   Reservar um endereço IP estático para cada servidor de federação no farm e, dependendo do seu sistema de nome de domínio \(DNS\) inserir uma exclusão para cada IP endereço no Dynamic Host Configuration Protocol, de configuração \(DHCP\). A tecnologia NLB da Microsoft requer que a cada servidor que participa do cluster NLB seja atribuído um endereço IP estático.  
  
-   Se o banco de dados de configuração do AD FS for armazenado em um banco de dados SQL, evite editar o banco de dados SQL de vários servidores de Federação ao mesmo tempo.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configurando servidores de federação para um farm  
A tabela a seguir descreve as tarefas que devem ser concluídas para que cada servidor de Federação pode participar de um ambiente de farm.  
  
|Tarefa|Descrição|  
|--------|---------------|  
|Se você estiver usando o SQL Server para armazenar o banco de dados de configuração do AD FS|Um farm de servidores de Federação consiste em dois ou mais servidores de federação que compartilham o mesmo banco de dados de configuração do AD FS e o token\-certificados de assinatura. O banco de dados de configuração pode ser armazenado em um banco de dados interno do Windows ou em um banco de dados do SQL Server. Se você planeja armazenar o banco de dados de configuração em um banco de dados SQL, certifique-se de que o banco de dados de configuração está acessível para que ele possa ser acessado por todos os novos servidores de federação que participam do farm. **Observação:** Para cenários de farm, é importante que o banco de dados estar localizado em um computador que não participe como um servidor de federação no farm. O NLB da Microsoft não permite que nenhum dos computadores que participam de um farm se comuniquem entre si. **Observação:** Certifique-se de que a identidade do AppPool de FS AD nos serviços de informações da Internet \(IIS\) \) em cada federação server que participa do farm tenha acesso de leitura no banco de dados de configuração.|  
|Obter e compartilhar certificados|Você pode obter o certificado de autenticação de um único servidor de uma autoridade de certificação pública \(autoridade de certificação\)— por exemplo, VeriSign. Em seguida, você pode configurar o certificado para que todos os servidores de Federação compartilham a mesma parte de chave privada do certificado. Para obter mais informações sobre como compartilhar o mesmo certificado, consulte [lista de verificação: Configurando um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Observação:** O snap de gerenciamento do AD FS\-refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação de serviço.<br /><br />Para obter mais informações, consulte [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).|  
|Apontar para a mesma instância do SQL Server|Se o banco de dados de configuração do AD FS for armazenado em um banco de dados SQL, o novo servidor de federação deve apontar para a mesma instância do SQL Server que é usada por outros servidores de federação no farm para que o novo servidor pode participar do farm.|  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
