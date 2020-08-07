---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Determinar a topologia de implantação do AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: dfb863cf85f9e9bffadbe10ca017491341596dad
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972113"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinar a topologia de implantação do AD FS

A primeira etapa no planejamento de uma implantação do Serviços de Federação do Active Directory (AD FS) \( AD FS \) é determinar a topologia de implantação certa para atender às \- \( \) necessidades de SSO de logon único de sua organização. Os tópicos nesta seção descrevem as várias topologias de implantação que você pode usar com AD FS. Eles também descrevem os benefícios e as limitações associados a cada topologia de implantação para que você possa selecionar a topologia mais apropriada às necessidades específicas dos seus negócios.

Antes de ler este tópico sobre topologia de implantação, é recomendável concluir primeiro as tarefas na ordem mostrada na tabela a seguir.

|Tarefa recomendada|Descrição|Referência|
|--------------------|---------------|-------------|
|Examine como AD FS dados são armazenados e replicados em outros servidores de Federação em um farm de servidores de Federação.|Entenda o objetivo e os métodos de replicação que podem ser usados para os dados subjacentes que estão armazenados no banco de dados de configuração do AD FS. Este tópico apresenta os conceitos do banco de dados de configuração e descreve os dois tipos de banco de dados: \( wid e Microsoft SQL Server do banco de dados interno do Windows \) .|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|
|Selecione o tipo de banco de dados de configuração do AD FS que você implantará na sua organização.|Revise os vários benefícios e limitações associados ao uso do WID ou SQL Server como o banco de dados de configuração do AD FS, junto com os vários cenários de aplicativos compatíveis.|[Considerações sobre a topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md)|

> [!NOTE]
> Para implementar a redundância básica, o balanceamento de carga e a opção de dimensionar a Serviço de Federação \( se necessário \) , é recomendável implantar pelo menos dois servidores de Federação por farm de servidores de Federação para todos os ambientes de produção, independentemente do tipo de banco de dados que você usará.

Depois de revisar o conteúdo da tabela anterior, prossiga para os seguintes tópicos desta seção:

-   [Servidor de federação autônomo usando WID](Stand-Alone-Federation-Server-Using-WID.md)

-   [Farm de servidores de federação usando WID](Federation-Server-Farm-Using-WID-2012.md)

-   [Farm de servidores de federação usando WID e proxies](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)

-   [Farm de servidores de federação usando SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)

Depois de concluir a seleção da topologia de implantação do AD FS, recomendamos que você examine o tópico [planejando a capacidade do AD FS Server](Planning-for-AD-FS-Server-Capacity.md) para determinar o número recomendado de servidores que você precisará implantar para dar suporte a essa topologia.

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

