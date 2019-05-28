---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: Planejar a topologia de implantação do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00c43a56d9b57a2ae2c8b9aeca56807fe1d1841f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191182"
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planejar a topologia de implantação do AD FS

A primeira etapa do planejamento da implantação dos serviços de Federação do Active Directory \(do AD FS\) é determinar a topologia de implantação certa para atender às necessidades da sua organização.  
  
Antes de ler este tópico, examine como os dados do AD FS são armazenados e replicados para outros servidores de Federação em um farm de servidores de Federação e certifique-se de entender a finalidade e os métodos de replicação que podem ser usados para os dados subjacentes que são armazenados no AD FS, con banco de dados figuração.  
  
Há dois tipos de banco de dados que você pode usar para armazenar dados de configuração do AD FS: Banco de dados interno do Windows \(WID\) e Microsoft SQL Server. Para saber mais, confira [A função do banco de dados de configuração de AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Examine os vários benefícios e limitações que estão associadas usando o WID ou SQL Server como banco de configuração do AD FS, junto com os vários cenários de aplicativo que eles dão suporte e, em seguida, faça sua seleção.  
  
> [!IMPORTANT]  
> Para implementar a redundância básica, balanceamento de carga e a opção de dimensionar o serviço de Federação \(se for necessário\), recomendamos que você implante pelo menos dois servidores de federação por farm de servidores de federação para todos os ambientes de produção, independentemente do tipo de banco de dados que você usará.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>A determinação de qual tipo de banco de dados de configuração AD FS deve ser usado para  
O AD FS usa um banco de dados para armazenar a configuração e — em alguns casos — dados transacionais relacionados ao serviço de Federação. Você pode usar o software AD FS para selecionar qualquer um dos internos\-no banco de dados interno do Windows \(WID\) ou Microsoft SQL Server 2008 ou mais recente para armazenar os dados no serviço de Federação.  
  
Para a maioria das finalidades, os dois tipos de bancos de dados são relativamente equivalentes. No entanto, há algumas diferenças a serem consideradas antes de começar a ler mais sobre as diversas topologias de implantação que você pode usar com o AD FS. A tabela a seguir descreve as diferenças nos recursos compatíveis entre um banco de dados WID e um banco de dados SQL Server.  
  
||Recurso|Compatível com WID?|Compatível com SQL Server?
| --- | --- | --- |--- |
|Recursos do AD FS|Implantação do farm do servidor de federação|Sim. Um farm de WID tem um limite de 30 servidores de federação, se você tiver 100 ou menos objetos de confiança.</br></br>Um farm de WID não oferece suporte a reprodução de token de detecção ou artefato resolução (parte do protocolo marcação linguagem SAML (Security Assertion)). |Sim. Não há um limite imposto para o número de servidores de federação que podem ser implantados em um único farm  
|Recursos do AD FS|Resolução do artefato SAML </br></br>**Observação:** Esse recurso não é necessário para os cenários do Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Não|Sim  
|Recursos do AD FS|SAML\/WS\-detecção de reprodução de token de Federação|Não|Sim  
|Recursos do banco de dados|Redundância de banco de dados básico usando a replicação, efetuar pull em que um ou mais servidores que hospedam uma leitura\-única cópia das alterações de solicitação do banco de dados que são feitas em um servidor de origem que hospeda uma leitura\/escrever a cópia do banco de dados|Sim|Não 
|Recursos do banco de dados|Redundância de banco de dados usando alta\-soluções de disponibilidade, como o clustering de failover ou espelhamento \(na camada de banco de dados só\) **Observação:** Todas as topologias de implantação do AD FS dá suporte a clustering na camada de serviço do AD FS.|Não|Sim  

  
## <a name="sql-server-considerations"></a>Considerações do SQL Server  
Você deve considerar os seguintes fatos de implantação se selecionar o SQL Server como o banco de dados de configuração para sua implantação do AD FS.  
  
-   **Recursos de SAML e seu efeito no tamanho e crescimento do banco de dados**. Quando os recursos de resolução de SAML ou de detecção de reprodução do token de SAML são habilitados, o AD FS armazena informações no banco de dados de configuração do SQL Server para cada token do AD FS que é emitido. O crescimento do banco de dados do SQL Server como resultado dessa atividade não é significativa e isso depende do período de retenção de reprodução do token configurado. Cada registro de artefato tem um tamanho de aproximadamente 30 kilobytes \(KB\).  
  
-   **Número de servidores necessários para sua implantação**. Será necessário adicionar pelo menos um servidor adicional \(para o número total de servidores necessários para implantar a infraestrutura do AD FS\) que atuará como um host dedicado da instância do SQL Server. Se você planejar usar o clustering de failover ou espelhamento para fornecer escalabilidade e tolerância padrão para o banco de dados de configuração do SQL Server, será necessário um mínimo de dois SQL Servers.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Como o tipo do banco de dados de configuração selecionado por você pode impactar os recursos de hardware  
O impacto nos recursos de hardware de um servidor de federação que é implantado em um farm usando o WID como oposto a um servidor de federação que é implantado em um farm usando o banco de dados do SQL Server não é significativo. No entanto, é importante considerar que ao usar o WID para o farm, cada servidor de federação no farm deve armazenar, gerenciar e manter as alterações de replicação para sua cópia local do banco de dados de configuração do AD FS enquanto ainda continua fornecendo as operações normais que o Serviço de Federação requer.  
  
Em comparação, os servidores de federação que são implantados em um farm que usa o banco de dados do SQL Server não contém necessariamente uma instância local do banco de dados de configuração do AD FS. Portanto, eles podem fazer um pouco menos de exigências em recursos de hardware.  
  
## <a name="BKMK_1"></a>Onde colocar um servidor de Federação  
Como uma segurança de práticas recomendadas, colocar servidores de Federação do AD FS na frente de um firewall e conecte-se à sua rede corporativa para evitar a exposição da Internet. Isso é importante porque os servidores de Federação têm autorização total para conceder tokens de segurança. Portanto, eles devem ter a mesma proteção que um controlador de domínio. Se um servidor de Federação estiver comprometido, um usuário mal-intencionado tem a capacidade de emitir tokens de acesso completo para todos os aplicativos Web e servidores de federação que são protegidos pelo AD FS.  
  
> [!NOTE]  
> Como uma segurança melhor prática, evite ter servidores de Federação diretamente acessíveis na Internet. É recomendável atribuir os servidores de Federação acesso direto à Internet somente quando você está configurando um ambiente de laboratório de teste ou quando sua organização não tiver uma rede de perímetro.  
  
Para redes corporativas típicas, uma intranet\-voltados para o firewall é estabelecido entre a rede corporativa e a rede de perímetro e um Internet\-voltados para o firewall geralmente é estabelecido entre a rede de perímetro e o Na Internet. Nessa situação, o servidor de Federação fica dentro da rede corporativa, e não é diretamente acessível pelos clientes de Internet.  
  
> [!NOTE]  
> Computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de federação por meio da autenticação integrada do Windows.  
  
Um proxy do servidor de federação deve ser colocado na rede de perímetro antes de configurar os servidores de firewall para uso com o AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologias de implantação com suporte  
Os tópicos a seguir descrevem as várias topologias de implantação que você pode usar com o AD FS. Eles também descrevem os benefícios e as limitações associados a cada topologia de implantação para que você possa selecionar a topologia mais apropriada às necessidades específicas dos seus negócios.  
  
-   [Farm de servidores de federação usando WID](Federation-Server-Farm-Using-WID.md)  
  
-   [Farm de servidores de federação usando WID e proxies](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Farm de servidores de federação usando SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Consulte também  
[Guia de design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

