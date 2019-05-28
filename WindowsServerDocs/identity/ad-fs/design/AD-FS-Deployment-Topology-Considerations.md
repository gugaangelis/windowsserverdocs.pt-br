---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Considerações sobre a topologia de implantação do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c5a3c85d40baee137ecdf7a1a5507b25361cac6d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191767"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considerações sobre a topologia de implantação do AD FS

Este tópico descreve considerações importantes para ajudá-lo a planejar e projetar qual serviços de Federação do Active Directory \(do AD FS\) topologia de implantação para usar em seu ambiente de produção. Este tópico é um ponto de partida para revisar e avaliar considerações que afetam quais recursos ou capacidades estarão disponíveis para você após a implantação do AD FS. Por exemplo, dependendo de qual banco de dados tipo você optar por armazenar o banco de dados de configuração do AD FS, por fim, determinará se é possível implementar determinados Security Assertion Markup Language \(SAML\) recursos que exigem o SQL Servidor.  
  
## <a name="determining-which-type-of-adfs-configuration-database-to-use"></a>A determinação de qual tipo de banco de dados de configuração AD FS deve ser usado para  
O AD FS usa um banco de dados para armazenar a configuração e — em alguns casos — dados transacionais relacionados ao serviço de Federação. Você pode usar o software AD FS para selecionar qualquer um dos internos\-no banco de dados interno do Windows \(WID\) ou Microsoft SQL Server 2005 ou mais recente para armazenar os dados no serviço de Federação.  
  
Para a maioria das finalidades, os dois tipos de bancos de dados são relativamente equivalentes. No entanto, há algumas diferenças a serem consideradas antes de começar a ler mais sobre as diversas topologias de implantação que você pode usar com o AD FS. A tabela a seguir descreve as diferenças nos recursos compatíveis entre um banco de dados WID e um banco de dados SQL Server.  
  
Recursos do AD FS  
  
|Recurso|Compatível com WID?|Compatível com SQL Server?|Mais informações sobre esse recurso|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Implantação do farm do servidor de federação|Sim, com um limite de 30 servidores de federação para cada farm|Sim. Não há um limite imposto para o número de servidores de federação que podem ser implantados em um único farm|[Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Resolução do artefato SAML **Observação:** Esse recurso não é necessário para os cenários do Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Melhores práticas para o planejamento e a implantação seguros do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|SAML\/WS\-detecção de reprodução de token de Federação|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Melhores práticas para o planejamento e a implantação seguros do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
Recursos do banco de dados  
  
|Recurso|Compatível com WID?|Compatível com SQL Server?|Mais informações sobre esse recurso|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redundância de banco de dados básico usando a replicação, efetuar pull em que um ou mais servidores que hospedam uma leitura\-única cópia das alterações de solicitação do banco de dados que são feitas em um servidor de origem que hospeda uma leitura\/escrever a cópia do banco de dados|Sim|Não|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redundância de banco de dados usando alta\-soluções de disponibilidade, como o clustering de failover ou espelhamento \(na camada de banco de dados só\) **Observação:** Todas as topologias de implantação do AD FS dá suporte a clustering na camada de serviço do AD FS.|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Visão geral das soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>Considerações do SQL Server  
Você deve considerar os seguintes fatos de implantação se selecionar o SQL Server como o banco de dados de configuração para sua implantação do AD FS.  
  
-   **Recursos de SAML e seu efeito no tamanho e crescimento do banco de dados**. Quando os recursos de resolução de SAML ou de detecção de reprodução do token de SAML são habilitados, o AD FS armazena informações no banco de dados de configuração do SQL Server para cada token do AD FS que é emitido. O crescimento do banco de dados do SQL Server como resultado dessa atividade não é significativa e isso depende do período de retenção de reprodução do token configurado. Cada registro de artefato tem um tamanho de aproximadamente 30 kilobytes \(KB\).  
  
-   **Número de servidores necessários para sua implantação**. Será necessário adicionar pelo menos um servidor adicional \(para o número total de servidores necessários para implantar a infraestrutura do AD FS\) que atuará como um host dedicado da instância do SQL Server. Se você planejar usar o clustering de failover ou espelhamento para fornecer escalabilidade e tolerância padrão para o banco de dados de configuração do SQL Server, será necessário um mínimo de dois SQL Servers.  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Como o tipo do banco de dados de configuração selecionado por você pode impactar os recursos de hardware  
O impacto nos recursos de hardware de um servidor de federação que é implantado em um farm usando o WID como oposto a um servidor de federação que é implantado em um farm usando o banco de dados do SQL Server não é significativo. No entanto, é importante considerar que ao usar o WID para o farm, cada servidor de federação no farm deve armazenar, gerenciar e manter as alterações de replicação para sua cópia local do banco de dados de configuração do AD FS enquanto ainda continua fornecendo as operações normais que o Serviço de Federação requer.  
  
Em comparação, os servidores de federação que são implantados em um farm que usa o banco de dados do SQL Server não contém necessariamente uma instância local do banco de dados de configuração do AD FS. Portanto, eles podem fazer um pouco menos de exigências em recursos de hardware.  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Verificar se seu ambiente de produção pode dar suporte a uma implantação do AD FS  
Além dos servidores de federação que você implantará e, dependendo de como seu ambiente de produção existente é configurada, os seguintes servidores adicionais deverão fornecer a infraestrutura necessária para dar suporte à nova implantação do AD FS:  
  
-   Controlador de domínio do Active Directory  
  
-   Autoridade de certificação \(autoridade de certificação\)  
  
-   Servidor Web para hospedar metadados de federação  
  
-   Balanceamento de carga de rede \(NLB\)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
