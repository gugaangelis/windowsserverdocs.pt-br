---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: "Quando criar um Farm de servidores de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-farm"></a>Quando criar um Farm de servidores de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere criar um farm de servidores de Federação em serviços de Federação do Active Directory \(AD FS\) quando você tiver uma implantação do AD FS maior e você deseja fornecer tolerância, balanceamento load\ ou escalabilidade ao serviço de Federação da sua organização. O ato de criar dois ou mais servidores de federação na mesma rede, configurando a cada um para usar o mesmo serviço de Federação e adicionando a chave pública de certificados de assinatura de token\ cada servidor para o AD FS snap\-in Gerenciamento criam um farm de servidores de Federação.  
  
Você pode criar um farm de servidores de Federação ou instalar servidores de Federação adicional a um farm existente usando o Assistente para configuração do servidor de Federação do AD FS. Para obter mais informações, consulte [quando criar um servidor de Federação](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Quando você escolhe a opção para criar um **novo farm de servidores de Federação** usando o Assistente para configuração do servidor de Federação do AD FS, o assistente tentará criar um objeto contêiner \(for sharing certificates\) no Active Directory. Portanto, é importante que você primeiro logon no computador, onde você estiver configurando a função de servidor de federação, com uma conta que tenha permissões suficientes no Active Directory para criar esse objeto de contêiner.  
  
Antes de servidores de federação podem ser agrupados como um farm, ele devem primeiro clusterizados para que as solicitações que chegarem a um único totalmente qualificados \(FQDN\) são roteadas para os vários servidores de federação no farm de servidores de nome de domínio. Você pode criar o cluster server Implantando \(NLB\) balanceamento de carga de rede dentro da rede corporativa. Este guia considera que NLB foi configurado adequadamente para cada um dos servidores de federação no farm do cluster.  
  
Para obter mais informações sobre como configurar um cluster FQDN usando a tecnologia Microsoft NLB, consulte [especificando os parâmetros de Cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Práticas recomendadas para implantar um farm de servidores de Federação  
Recomendamos as seguintes práticas recomendadas para implantar um servidor de Federação em um ambiente de produção:  
  
-   Se você estiver implantando vários servidores de Federação ao mesmo tempo ou se você sabe que você adicionará mais servidores ao farm ao longo do tempo, considere a criação de uma imagem do servidor de um servidor de Federação existente no farm e instalando a partir dessa imagem quando você precisa criar rapidamente servidores de Federação adicionais.  
  
    > [!NOTE]  
    > Se você decidir usar o método de imagem do servidor para implantar servidores de Federação adicionais, você não precisa concluir as tarefas no [lista de verificação: configuração de um servidor federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) toda vez que você deseja adicionar um novo servidor ao farm.  
  
-   Use NLB ou alguma outra forma de clustering para alocar um único endereço IP para vários computadores de servidor de Federação.  
  
-   Reservar um endereço IP estático para cada servidor de federação no farm e, dependendo da configuração \(DNS\) Domain Name System, insira uma exclusão para cada endereço IP em \(DHCP\) Dynamic Host Configuration Protocol. A tecnologia Microsoft NLB requer que cada servidor que participam do cluster NLB ser atribuído um endereço IP estático.  
  
-   Se o banco de dados de configuração do AD FS será armazenado em um banco de dados SQL, evite editando o banco de dados SQL de vários servidores de Federação ao mesmo tempo.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configurando um farm de servidores federação  
A tabela a seguir descreve as tarefas que devem ser concluídas para que cada servidor de Federação pode participar em um ambiente de farm.  
  
|Tarefa|Descrição|  
|--------|---------------|  
|Se você estiver usando o SQL Server para armazenar o banco de dados de configuração do AD FS|Um farm de servidores de Federação consiste em dois ou mais serviços de federação que compartilham a mesma configuração do AD FS banco de dados e certificados de autenticação de token\. O banco de dados de configuração pode ser armazenado em um banco de dados interno do Windows ou em um banco de dados do SQL Server. Se você pretende armazenar o banco de dados de configuração em um banco de dados SQL, certifique-se de que o banco de dados de configuração seja acessível para que ele possa ser acessado por todos os servidores de Federação novo que participam do farm. **Observação:** para cenários de farm, é importante que o banco de dados de configuração estar localizada em um computador que não participe como um servidor de federação no farm. Microsoft NLB não permite que qualquer um dos computadores que participam um farm para se comunicar uns com os outros. **Observação:** Certifique-se de que a identidade do AppPool FS de anúncios na Internet Information Services \(IIS\)\) em cada servidor de federação que participam sítio tem acesso de leitura no banco de dados de configuração.|  
|Obter e compartilhar certificados|Você pode obter o certificado de autenticação de um único servidor de uma autoridade de certificação pública \ (CA\) — por exemplo, VeriSign. Em seguida, você pode configurar o certificado para que todos os servidores de Federação compartilham o mesmo parte da chave privada do certificado. Para saber mais sobre como compartilhar o mesmo certificado, consulte [lista de verificação: configuração de um servidor federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Observação:** o AD FS snap\-in Gerenciamento refere-se aos certificados de autenticação de servidor para servidores de federação como certificados de comunicação do serviço.<br /><br />Para obter mais informações, consulte [requisitos de certificado para servidores de Federação](Certificate-Requirements-for-Federation-Servers.md).|  
|Aponte para a mesma instância do SQL Server|Se o banco de dados de configuração do AD FS será armazenado em um banco de dados SQL, o servidor de Federação novo deve apontar para a mesma instância do SQL Server que é usada por outros servidores de federação no farm para que o novo servidor pode participar no farm.|  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
