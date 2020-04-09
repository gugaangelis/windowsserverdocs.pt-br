---
ms.assetid: 4ef052f0-61a9-4912-b780-5c96187c850f
title: Considerações sobre a topologia de implantação do AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 42db104691bce75c37adf19eb97abba3bf579cf8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855869"
---
# <a name="ad-fs-deployment-topology-considerations"></a>Considerações sobre a topologia de implantação do AD FS

Este tópico descreve considerações importantes para ajudá-lo a planejar e projetar quais Serviços de Federação do Active Directory (AD FS) \(AD FS\) topologia de implantação para usar em seu ambiente de produção. Este tópico é um ponto de partida para revisar e avaliar as considerações que afetam quais recursos ou funcionalidades estarão disponíveis para você depois de implantar o AD FS. Por exemplo, dependendo de qual tipo de banco de dados você escolher para armazenar o banco de dados de configuração de AD FS, finalmente determinará se você pode implementar determinados Security Assertion Markup Language \(recursos de\) SAML que exigem SQL Server.  

## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinando qual tipo de banco de dados de configuração AD FS usar  
O AD FS usa um banco de dados para armazenar a configuração e, em alguns casos, os dados transacionais relacionados ao Serviço de Federação. Você pode usar o software AD FS para selecionar o\-interno no banco de dados interno do Windows \(WID\) ou Microsoft SQL Server 2005 ou mais recente para armazenar os dados no Serviço de Federação.  

Para a maioria das finalidades, os dois tipos de bancos de dados são relativamente equivalentes. No entanto, há algumas diferenças a serem consideradas antes de começar a ler mais sobre as várias topologias de implantação que você pode usar com AD FS. A tabela a seguir descreve as diferenças nos recursos com suporte entre um banco de dados WID e um SQL Server banco de dados.  

Recursos de AD FS  

|Recurso|Compatível com WID?|Compatível com SQL Server?|Mais informações sobre esse recurso|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Implantação do farm do servidor de federação|Sim, com um limite de 30 servidores de Federação para cada farm|Sim. Não há um limite imposto para o número de servidores de federação que podem ser implantados em um único farm|[Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Observação de resolução do artefato SAML **:** esse recurso não é necessário para cenários do Microsoft Online Services, Microsoft Office 365, Microsoft Exchange ou Microsoft Office SharePoint.|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[Melhores práticas para o planejamento e a implantação seguros do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  
|Detecção de reprodução de token de Federação de\/de WS\-SAML|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[Melhores práticas para o planejamento e a implantação seguros do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)|  

Recursos do banco de dados  

|Recurso|Compatível com WID?|Compatível com SQL Server?|Mais informações sobre esse recurso|  
|-----------|---------------------|----------------------------|---------------------------------------|  
|Redundância básica de banco de dados usando replicação pull, em que um ou mais servidores que hospedam uma leitura\-apenas cópia das alterações de solicitação de banco de dados feitas em um servidor de origem que hospeda uma cópia de leitura\/gravação do banco de dados|Sim|Não|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Redundância de banco de dados usando soluções de alta\-disponibilidade, como clustering de failover ou espelhamento \(na camada do banco de dados somente\) **Observação:** todas as topologias de implantação do AD FS dão suporte ao clustering na camada de serviço AD FS.|Não|Sim|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)<p>[Visão geral das soluções de alta disponibilidade](https://go.microsoft.com/fwlink/?LinkId=179853)|  

### <a name="sql-server-considerations"></a>Considerações do SQL Server  
Você deve considerar os seguintes fatos de implantação se selecionar SQL Server como o banco de dados de configuração para sua implantação de AD FS.  

-   **Recursos de SAML e seu efeito no tamanho e crescimento do banco de dados**. Quando os recursos resolução de artefato SAML ou detecção de repetição de token SAML estão habilitados, o AD FS armazena informações no banco de dados de configuração de SQL Server para cada token de AD FS que é emitido. O crescimento do banco de dados SQL Server como resultado dessa atividade não é considerado significativo, e depende do período de retenção do token reproduzido configurado. Cada registro de artefato tem um tamanho de aproximadamente 30 kilobytes \(KB\).  

-   **Número de servidores necessários para sua implantação**. Você precisará adicionar pelo menos um servidor adicional \(ao número total de servidores necessários para implantar sua infraestrutura de AD FS\) que atuará como um host dedicado da instância de SQL Server. Se você planeja usar clustering de failover ou espelhamento para fornecer tolerância a falhas e escalabilidade para o banco de dados de configuração do SQL Server, é necessário um mínimo de dois SQL Servers.  

### <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Como o tipo do banco de dados de configuração selecionado por você pode impactar os recursos de hardware  
O impacto nos recursos de hardware em um servidor de Federação implantado em um farm usando o WID, em oposição a um servidor de Federação implantado em um farm usando o banco de dados SQL Server não é significativo. No entanto, é importante considerar que quando você usa o WID para o farm, cada servidor de Federação nesse farm deve armazenar, gerenciar e manter as alterações de replicação para sua cópia local do banco de dados de configuração AD FS, enquanto também continua a fornecer as operações normais que o Serviço de Federação exige.  

Em comparação, os servidores de Federação que são implantados em um farm que usa o banco de dados SQL Server não contêm necessariamente uma instância local do banco de dados de configuração do AD FS. Portanto, eles podem fazer um pouco menos de exigências em recursos de hardware.  

## <a name="verifying-that-your-production-environment-can-support-an-ad-fs-deployment"></a>Verificar se seu ambiente de produção pode dar suporte a uma implantação do AD FS  
Além dos servidores de Federação que você irá implantar e, dependendo de como o ambiente de produção existente está configurado, os seguintes servidores adicionais podem ser necessários para fornecer a infraestrutura necessária para dar suporte à nova implantação de AD FS:  

-   Controlador de domínio do Active Directory  

-   Autoridade de certificação \(\) da AC  

-   Servidor Web para hospedar metadados de federação  

-   Balanceamento de carga de rede \(NLB\)  

## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
