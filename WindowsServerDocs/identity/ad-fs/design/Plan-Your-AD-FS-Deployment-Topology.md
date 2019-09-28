---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Planejar a topologia de implantação do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9cd036e9dd0b249197fb475504c9cad532ead0ea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408030"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planejar a topologia de implantação do AD FS

A primeira etapa no planejamento de uma implantação do Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 é determinar a topologia de implantação certa para atender às necessidades da sua organização.  
  
Antes de ler este tópico, examine como AD FS dados são armazenados e replicados para outros servidores de Federação em um farm de servidores de Federação e certifique-se de entender a finalidade do e os métodos de replicação que podem ser usados para os dados subjacentes armazenados no AD FS con banco de dados figuração.  
  
Há dois tipos de banco de dados que você pode usar para armazenar AD FS dados de configuração: Banco de dados interno do Windows \(WID @ no__t-1 e Microsoft SQL Server. Para saber mais, confira [A função do banco de dados de configuração de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Examine os vários benefícios e limitações associados ao uso do WID ou SQL Server como o banco de dados de configuração do AD FS, juntamente com os vários cenários de aplicativos aos quais eles dão suporte e faça sua seleção.  
  
> [!IMPORTANT]  
> Para implementar a redundância básica, o balanceamento de carga e a opção de dimensionar o Serviço de Federação \(if necessário @ no__t-1, recomendamos que você implante pelo menos dois servidores de Federação por farm de servidores de Federação para todos os ambientes de produção, independentemente do tipo de banco de dados que será usado.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>A determinação de qual tipo de banco de dados de configuração AD FS deve ser usado para  
O AD FS usa um banco de dados para armazenar a configuração e, em alguns casos, os dados transacionais relacionados ao Serviço de Federação. Você pode usar o software AD FS para selecionar o banco de dados interno do Windows @ no__t-0in interno \(WID @ no__t-2 ou Microsoft SQL Server 2008 ou mais recente para armazenar os dados no Serviço de Federação.  
  
Para a maioria das finalidades, os dois tipos de bancos de dados são relativamente equivalentes. No entanto, há algumas diferenças a serem consideradas antes de começar a ler mais sobre as várias topologias de implantação que você pode usar com AD FS. A tabela a seguir descreve as diferenças nos recursos compatíveis entre um banco de dados WID e um banco de dados SQL Server.  
  
||Recurso|Compatível com WID?|Compatível com SQL Server?
| --- | --- | --- |--- |
|Recursos de AD FS|Implantação do farm do servidor de federação|Sim. Um farm WID tem um limite de 30 servidores de Federação se você tiver 100 ou menos confianças de terceira parte confiável.</br></br>Um farm WID não oferece suporte à detecção de reprodução de token ou resolução de artefatos (parte do protocolo Security Assertion Markup Language (SAML)). |Sim. Não há um limite imposto para o número de servidores de federação que podem ser implantados em um único farm  
|Recursos de AD FS|Resolução do artefato SAML </br></br>**Observação:** Esse recurso não é necessário para os cenários do Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Não|Sim  
|Recursos de AD FS|Detecção de reprodução de token do SAML @ no__t-0WS @ no__t-1Federation|Não|Sim  
|Recursos do banco de dados|Redundância básica de banco de dados usando replicação de recepção, em que um ou mais servidores que hospedam uma cópia de leitura @ no__t-0only das alterações de solicitação de banco de dados são feitas em um servidor de origem que hospeda uma cópia de leitura @ no__t-1write do banco de dados|Sim|Não 
|Recursos do banco de dados|Redundância de banco de dados usando soluções High @ no__t-0availability, como clustering de failover ou espelhamento \(AT a camada do banco de dados somente @ no__t-2 **Observação:** Todas as AD FS topologias de implantação dão suporte ao clustering na camada de serviço do AD FS.|Não|Sim  

  
## <a name="sql-server-considerations"></a>Considerações do SQL Server  
Você deve considerar os seguintes fatos de implantação se selecionar o SQL Server como o banco de dados de configuração para sua implantação do AD FS.  
  
-   **Recursos de SAML e seu efeito no tamanho e crescimento do banco de dados**. Quando os recursos de resolução de SAML ou de detecção de reprodução do token de SAML são habilitados, o AD FS armazena informações no banco de dados de configuração do SQL Server para cada token do AD FS que é emitido. O crescimento do banco de dados do SQL Server como resultado dessa atividade não é significativa e isso depende do período de retenção de reprodução do token configurado. Cada registro de artefato tem um tamanho de aproximadamente 30 kilobytes \(KB @ no__t-1.  
  
-   **Número de servidores necessários para sua implantação**. Você precisará adicionar pelo menos um servidor adicional \(to o número total de servidores necessários para implantar sua infraestrutura de AD FS @ no__t-1 que atuará como um host dedicado da instância de SQL Server. Se você planejar usar o clustering de failover ou espelhamento para fornecer escalabilidade e tolerância padrão para o banco de dados de configuração do SQL Server, será necessário um mínimo de dois SQL Servers.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Como o tipo do banco de dados de configuração selecionado por você pode impactar os recursos de hardware  
O impacto nos recursos de hardware de um servidor de federação que é implantado em um farm usando o WID como oposto a um servidor de federação que é implantado em um farm usando o banco de dados do SQL Server não é significativo. No entanto, é importante considerar que ao usar o WID para o farm, cada servidor de federação no farm deve armazenar, gerenciar e manter as alterações de replicação para sua cópia local do banco de dados de configuração do AD FS enquanto ainda continua fornecendo as operações normais que o Serviço de Federação requer.  
  
Em comparação, os servidores de federação que são implantados em um farm que usa o banco de dados do SQL Server não contém necessariamente uma instância local do banco de dados de configuração do AD FS. Portanto, eles podem fazer um pouco menos de exigências em recursos de hardware.  
  
## <a name="BKMK_1"></a>Onde posicionar um servidor de Federação  
Como prática recomendada de segurança, coloque AD FS servidores de Federação na frente de um firewall e conecte-os à sua rede corporativa para evitar a exposição da Internet. Isso é importante porque os servidores de Federação têm autorização total para conceder tokens de segurança. Portanto, eles devem ter a mesma proteção que um controlador de domínio. Se um servidor de Federação estiver comprometido, um usuário mal-intencionado terá a capacidade de emitir tokens de acesso completo para todos os aplicativos Web e para servidores de Federação protegidos pelo AD FS.  
  
> [!NOTE]  
> Como prática recomendada de segurança, evite ter seus servidores de Federação diretamente acessíveis na Internet. Considere dar aos seus servidores de Federação acesso direto à Internet somente quando você estiver configurando um ambiente de laboratório de teste ou quando sua organização não tiver uma rede de perímetro.  
  
Para redes corporativas típicas, um firewall de intranet @ no__t-0facing é estabelecido entre a rede corporativa e a rede de perímetro, e um firewall de Internet @ no__t-1facing é geralmente estabelecido entre a rede de perímetro e a Internet. Nessa situação, o servidor de Federação fica dentro da rede corporativa e não pode ser acessado diretamente pelos clientes da Internet.  
  
> [!NOTE]  
> Os computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de Federação por meio da autenticação integrada do Windows.  
  
Um proxy de servidor de Federação deve ser colocado na rede de perímetro antes de configurar os servidores de firewall para uso com o AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologias de implantação com suporte  
Os tópicos a seguir descrevem as várias topologias de implantação que você pode usar com AD FS. Eles também descrevem os benefícios e as limitações associados a cada topologia de implantação para que você possa selecionar a topologia mais apropriada às necessidades específicas dos seus negócios.  
  
-   [Farm de servidores de federação usando WID](Federation-Server-Farm-Using-WID.md)  
  
-   [Farm de servidores de federação usando WID e proxies](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Farm de servidores de federação usando SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Consulte também  
[Guia de design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

