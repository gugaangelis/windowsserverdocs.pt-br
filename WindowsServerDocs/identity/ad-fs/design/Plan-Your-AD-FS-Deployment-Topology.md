---
ms.assetid: 5c8c6cc0-0d22-4f27-a111-0aa90db7d6c8
title: "Planejar a topologia de implantação do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e41f7728c42912ec6ce680e1ed0c6a906a33392
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="plan-your-ad-fs-deployment-topology"></a>Planejar a topologia de implantação do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

A primeira etapa no planejamento de uma implantação de serviços de Federação do Active Directory \(AD FS\) é determinar a topologia de implantação certa para atender às necessidades da sua organização.  
  
Antes de ler este tópico, examine como os dados do AD FS são armazenados e replicados para outros servidores de Federação em um farm de servidores de Federação e certifique-se de que você entende a finalidade e os métodos de replicação que podem ser usados para os dados subjacentes que são armazenados no banco de dados de configuração do AD FS.  
  
Existem dois tipos de banco de dados que você pode usar para armazenar dados de configuração do AD FS: \(WID\) banco de dados interno do Windows e Microsoft SQL Server. Para obter mais informações, consulte [a função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md). Examine os diversos benefícios e limitações associadas usando o trabalho ou SQL Server como o AD FS configuração banco de dados, juntamente com os diversos cenários de aplicativo que eles dão suporte e, em seguida, faça sua seleção.  
  
> [!IMPORTANT]  
> Para implementar a redundância básico, balanceamento de carga e a opção para dimensionar o serviço de Federação \(if required\), recomendamos que você implantar pelo menos dois servidores de federação por farm de servidores de federação para todos os ambientes de produção, independentemente do tipo de banco de dados que você usará.  
  
## <a name="determining-which-type-of-ad-fs-configuration-database-to-use"></a>Determinando qual tipo de banco de dados de configuração do AD FS usar  
AD FS usa um banco de dados para armazenar a configuração e, em alguns casos, dados transacionais relacionados ao serviço de Federação. Você pode usar o software do AD FS para selecionar o built\ no banco de dados interno do Windows \(WID\) ou Microsoft SQL Server 2008 ou mais recente para armazenar os dados no serviço de Federação.  
  
Em geral, os tipos de banco de dois dados são relativamente equivalentes. No entanto, existem algumas diferenças devem ser lembradas antes de começar a ler mais sobre as diversas topologias de implantação que você pode usar com o AD FS. A tabela a seguir descreve as diferenças nos recursos com suporte entre um banco de dados de trabalho e um banco de dados do SQL Server.  
  
||Recurso|Suporte ao trabalho?|Suporte pelo SQL Server?
| --- | --- | --- |--- |
|Recursos do AD FS|Implantação de farm de servidor de Federação|Sim. Um farm de trabalho tem um limite de 30 servidores de federação se você tiver 100 ou menos terceira parte relações de confiança.</br></br>Um farm de trabalho não suporta reprodução token detecção ou artefato resolução (parte do protocolo asserção SAML Security Markup Language ()). |Sim. Não há nenhum limite imposta para o número de servidores de federação que você pode implantar em um único farm  
|Recursos do AD FS|Resolução de artefato SAML </br></br>**Observação:** esse recurso não é necessário para cenários de serviços Online da Microsoft, Microsoft Office 365, Microsoft Exchange ou do Microsoft Office SharePoint.|Não|Sim  
|Recursos do AD FS|Detecção de repetição token WS\/SAML\-federação|Não|Sim  
|Recursos de banco de dados|Redundância básicas de banco de dados usando a replicação de recepção, onde um ou mais servidores que hospedam uma cópia somente read\ das alterações de solicitação de banco de dados que são feitas em um servidor de origem que hospeda uma cópia read\/gravação do banco de dados|Sim|Não 
|Recursos de banco de dados|Redundância do banco de dados usando soluções de disponibilidade de high\, como o failover clustering ou espelhamento \ (em only\ de camada do banco de dados) **Observação:** compatíveis com todas as topologias de implantação do AD FS clusters na camada de serviço do AD FS.|Não|Sim  

  
## <a name="sql-server-considerations"></a>Considerações do SQL Server  
Caso você selecione SQL Server como o banco de dados de configuração para a implantação do AD FS, você deve considerar os seguintes fatos de implantação.  
  
-   **SAML recursos e seus efeitos sobre tamanho do banco de dados e crescimento**. Quando a resolução de artefato SAML ou recursos de detecção de repetição token SAML forem habilitados, o AD FS armazena informações no banco de dados SQL Server configuration para cada token do AD FS emitido. O crescimento do banco de dados SQL Server como resultado dessa atividade não é considerado significativo e depende do período de retenção de repetição token configurado. Cada registro artefato tem um tamanho de aproximadamente 30 kilobytes \(KB\).  
  
-   **Número de servidores necessários para a implantação**. Você precisará adicionar pelo menos um servidor adicional \ (para o número total de servidores necessários para implantar seu infrastructure\ AD FS) que vai atuar como um host dedicado da instância do SQL Server. Se você pretende usar failover clustering ou espelhamento para fornecer a tolerância a e escalabilidade do banco de dados de configuração do SQL Server, é necessário um mínimo de dois servidores SQL.  
  
## <a name="how-the-configuration-database-type-you-select-may-impact-hardware-resources"></a>Como o tipo de banco de dados de configuração selecionado pode ter impacto em recursos de hardware  
O impacto nos recursos de hardware em um servidor de federação que é implantado em um farm usando o trabalho em oposição a um servidor de federação que é implantado em um farm usando o banco de dados do SQL Server não é significativo. No entanto, é importante considerar que quando você usa o trabalho para o farm, cada servidor de Federação nesse farm deve armazenar, gerenciar e manter as alterações de replicação para sua cópia local do banco de dados de configuração do AD FS enquanto também continua a fornecer as operações normais que exige que o serviço de Federação.  
  
Em comparação, servidores de federação que são implantados em um farm que usa o banco de dados do SQL Server não incluem necessariamente uma instância local do banco de dados de configuração do AD FS. Portanto, eles podem fazer um pouco menos demandas sobre os recursos de hardware.  
  
## <a name="BKMK_1"></a>Onde colocar um servidor de Federação  
Como segurança práticas recomendadas, coloque os servidores de Federação do AD FS na frente de um firewall e conectá-las à sua rede corporativa para evitar a exposição da Internet. Isso é importante porque os servidores de federação tem autorização completa para conceder tokens de segurança. Portanto, eles devem ter a mesma proteção que um controlador de domínio. Se um servidor de Federação está comprometido, um usuário mal-intencionado tem a capacidade de emitir tokens de acesso completo a todos os aplicativos da Web e servidores de federação que estão protegidos pelo AD FS.  
  
> [!NOTE]  
> Como uma segurança prática recomendada, evite ter seus servidores de Federação diretamente acessíveis na Internet. Talvez seja conveniente dar acesso direto à Internet de seus servidores de Federação somente quando você está configurando um ambiente de laboratório de teste ou quando sua organização não tem uma rede do perímetro.  
  
Para redes corporativas típicas, um firewall intranet\ voltados é estabelecido entre a rede corporativa e a rede do perímetro e um firewall Internet\ voltados geralmente é estabelecido entre a rede do perímetro e a Internet. Nessa situação, o servidor de Federação fica dentro da rede corporativa e não está diretamente acessível por clientes na Internet.  
  
> [!NOTE]  
> Computadores cliente que estão conectados à rede corporativa podem se comunicar diretamente com o servidor de federação por meio de autenticação integrada do Windows.  
  
Um proxy de servidor de federação deve ser colocado na rede de perímetro antes de você configurar os servidores de firewall para uso com o AD FS.  
  
## <a name="supported-deployment-topologies"></a>Topologias de implantação com suporte  
Os tópicos a seguir descrevem as vários topologias de implantação que você pode usar com o AD FS. Eles também descrevem os benefícios e limitações associadas a cada topologia de implantação para que você pode selecionar a topologia mais adequada às suas necessidades específicas de negócios.  
  
-   [Farm de servidores de Federação usando o trabalho](Federation-Server-Farm-Using-WID.md)  
  
-   [Usando o trabalho e Proxies Farm do servidor de Federação](Federation-Server-Farm-Using-WID-and-Proxies.md)  
  
-   [Farm de servidores de Federação usando o SQL Server](Federation-Server-Farm-Using-SQL-Server.md)  
  
## <a name="see-also"></a>Consulte também  
[Guia de Design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

