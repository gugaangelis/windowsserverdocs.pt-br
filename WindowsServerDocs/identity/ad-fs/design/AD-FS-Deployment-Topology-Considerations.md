---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: "Considerações de topologia de implantação do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: eee14ee7bb50e1a82f35caf9fbacda0b86d3a1ad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considerações de topologia de implantação do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve as considerações importantes para ajudá-lo a planejar e projetar a topologia de implantação de \(AD FS\) quais serviços de Federação do Active Directory para usar em seu ambiente de produção. Este tópico é um ponto de partida para examinar e avaliar considerações que afetam quais recursos ou funcionalidades estarão disponíveis para você depois de implantar o AD FS. Por exemplo, dependendo de qual banco de dados que escolhe armazenar o banco de dados de configuração do AD FS do tipo em última análise, determinará se você pode implementar determinados recursos de linguagem de marcação de asserção de segurança \(SAML\) que exigem o SQL Server.  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinando qual tipo de banco de dados de configuração do AD FS usar  
AD FS usa um banco de dados para armazenar a configuração e, em alguns casos, dados transacionais relacionados ao serviço de Federação. Você pode usar o software do AD FS para selecionar o built\ no banco de dados interno do Windows \(WID\) ou Microsoft SQL Server 2005 ou mais recente para armazenar os dados no serviço de Federação.  
  
Em geral, os tipos de banco de dois dados são relativamente equivalentes. No entanto, existem algumas diferenças devem ser lembradas antes de começar a ler mais sobre as diversas topologias de implantação que você pode usar com o AD FS. A tabela a seguir descreve as diferenças nos recursos com suporte entre um banco de dados de trabalho e um banco de dados do SQL Server.  
  
Recursos do AD FS  
  
|Recurso|Suporte ao trabalho?|Suporte pelo SQL Server?|Para obter mais informações sobre esse recurso|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Implantação de farm de servidor de Federação|Sim, com um limite de cinco servidores de federação para cada farm|Sim. Não há nenhum limite imposta para o número de servidores de federação que você pode implantar em um único farm|[Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Resolução de artefato SAML **Observação:** esse recurso não é necessário para cenários de serviços Online da Microsoft, Microsoft Office 365, Microsoft Exchange ou do Microsoft Office SharePoint.|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Práticas recomendadas para segura de planejamento e implantação do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|Detecção de repetição token WS\/SAML\-federação|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Práticas recomendadas para segura de planejamento e implantação do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
  
Recursos de banco de dados  
  
|Recurso|Suporte ao trabalho?|Suporte pelo SQL Server?|Para obter mais informações sobre esse recurso|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redundância básicas de banco de dados usando a replicação de recepção, onde um ou mais servidores que hospedam uma cópia somente read\ das alterações de solicitação de banco de dados que são feitas em um servidor de origem que hospeda uma cópia read\/gravação do banco de dados|Sim|Não|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redundância do banco de dados usando soluções de disponibilidade de high\, como o failover clustering ou espelhamento \ (em only\ de camada do banco de dados) **Observação:** compatíveis com todas as topologias de implantação do AD FS clusters na camada de serviço do AD FS.|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<br /><br />[Visão geral das soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853)|  
  
### <a name="sql-server-considerations"></a>Considerações do SQL Server  
Caso você selecione SQL Server como o banco de dados de configuração para a implantação do AD FS, você deve considerar os seguintes fatos de implantação.  
  
-   **SAML recursos e seus efeitos sobre tamanho do banco de dados e crescimento**. Quando a resolução de artefato SAML ou recursos de detecção de repetição token SAML forem habilitados, o AD FS armazena informações no banco de dados SQL Server configuration para cada token do AD FS emitido. O crescimento do banco de dados SQL Server como resultado dessa atividade não é considerado significativo e depende do período de retenção de repetição token configurado. Cada registro artefato tem um tamanho de aproximadamente 30 kilobytes \(KB\).  
  
-   **Número de servidores necessários para a implantação**. Você precisará adicionar pelo menos um servidor adicional \ (para o número total de servidores necessários para implantar seu infrastructure\ AD FS) que vai atuar como um host dedicado da instância do SQL Server. Se você pretende usar failover clustering ou espelhamento para fornecer a tolerância a e escalabilidade do banco de dados de configuração do SQL Server, é necessário um mínimo de dois servidores SQL.  
  
### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Como o tipo de banco de dados de configuração selecionado pode ter impacto em recursos de hardware  
O impacto nos recursos de hardware em um servidor de federação que é implantado em um farm usando o trabalho em oposição a um servidor de federação que é implantado em um farm usando o banco de dados do SQL Server não é significativo. No entanto, é importante considerar que quando você usa o trabalho para o farm, cada servidor de Federação nesse farm deve armazenar, gerenciar e manter as alterações de replicação para sua cópia local do banco de dados de configuração do AD FS enquanto também continua a fornecer as operações normais que exige que o serviço de Federação.  
  
Em comparação, servidores de federação que são implantados em um farm que usa o banco de dados do SQL Server não incluem necessariamente uma instância local do banco de dados de configuração do AD FS. Portanto, eles podem fazer um pouco menos demandas sobre os recursos de hardware.  
  
## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Verificando que seu ambiente de produção pode dar suporte a uma implantação do AD FS  
Além dos servidores de federação que você implantará e dependendo de como seu ambiente de produção existentes é configurado, os seguintes servidores adicionais podem ser necessário fornecer a infraestrutura necessária para dar suporte a nova implantação AD FS:  
  
-   Controlador de domínio do Active Directory  
  
-   Autoridade de certificação \(CA\)  
  
-   Servidor Web para metadados de Federação do host  
  
-   \(NLB\) balanceamento de carga de rede  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
