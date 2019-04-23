---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: Determinar a topologia de implantação do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3300c16be6d516d7ec0bf4d0c3a025e59e6126b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834517"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinar a topologia de implantação do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa do planejamento da implantação dos serviços de Federação do Active Directory \(do AD FS\) é determinar a topologia de implantação certa para atender o logon único\-na \(SSO\) das necessidades de seu organização. Os tópicos nesta seção descrevem as várias topologias de implantação que você pode usar com o AD FS. Eles também descrevem os benefícios e as limitações associados a cada topologia de implantação para que você possa selecionar a topologia mais apropriada às necessidades específicas dos seus negócios.  
  
Antes de ler este tópico sobre topologia de implantação, é recomendável concluir primeiro as tarefas na ordem mostrada na tabela a seguir.  
  
|Tarefa recomendada|Descrição|Referência|  
|--------------------|---------------|-------------|  
|Examine como os dados do AD FS são armazenados e replicados para outros servidores de Federação em um farm de servidores de Federação.|Entenda o objetivo e os métodos de replicação que podem ser usados para os dados subjacentes que estão armazenados no banco de dados de configuração do AD FS. Este tópico apresenta os conceitos do banco de dados de configuração e descreve os dois tipos de bancos de dados: Banco de dados interno do Windows \(WID\) e Microsoft SQL Server.|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Selecione o tipo de banco de dados de configuração do AD FS que você implantará na sua organização.|Revise os vários benefícios e limitações associados ao uso do WID ou SQL Server como o banco de dados de configuração do AD FS, junto com os vários cenários de aplicativos compatíveis.|[Considerações de topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Para implementar a redundância básica, balanceamento de carga e a opção de dimensionar o serviço de Federação \(se for necessário\), recomendamos que você implante pelo menos dois servidores de federação por farm de servidores de federação para todos os ambientes de produção, independentemente do tipo de banco de dados que você usará.  
  
Depois de revisar o conteúdo da tabela anterior, prossiga para os seguintes tópicos desta seção:  
  
-   [Servidor de Federação autônomo usando WID](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Farm de servidores de Federação usando WID](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Farm de servidores de Federação usando WID e Proxies](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Farm de servidores de Federação usando o SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Depois de terminar de selecionar a topologia de implantação do AD FS, é recomendável que você examine o tópico [planejamento de capacidade do servidor do AD FS](Planning-for-AD-FS-Server-Capacity.md) para determinar o número recomendado de servidores que você precisará implantar para dar suporte a essa topologia.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

