---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: "Determinar a topologia de implantação do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3300c16be6d516d7ec0bf4d0c3a025e59e6126b6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="determine-your-ad-fs-deployment-topology"></a>Determinar a topologia de implantação do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A primeira etapa no planejamento de uma implantação de serviços de Federação do Active Directory \(AD FS\) é para determinar a topologia de implantação certa para atender a \(SSO\) sign\-on único necessidades da sua organização. Os tópicos nesta seção descrevem as várias topologias de implantação que você pode usar com o AD FS. Eles também descrevem os benefícios e limitações associadas a cada topologia de implantação para que você pode selecionar a topologia mais adequada às suas necessidades específicas de negócios.  
  
Antes de ler este tópico de topologia de implantação, recomendamos concluir primeiro as tarefas na ordem mostrada na tabela a seguir.  
  
|Tarefa recomendada|Descrição|Referência|  
|--------------------|---------------|-------------|  
|Examine como os dados do AD FS são armazenados e replicados para outros servidores de Federação em um farm de servidores de Federação.|Entenda a finalidade e os métodos de replicação que podem ser usados para os dados subjacentes que são armazenados no banco de dados de configuração do AD FS. Este tópico apresenta os conceitos do banco de dados de configuração e descreve os tipos de banco de dois dados: \(WID\) banco de dados interno do Windows e Microsoft SQL Server.|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|Selecione o tipo de banco de dados de configuração de AD FS que você implantará em sua organização.|Examine os diversos benefícios e limitações associadas usando o trabalho ou SQL Server como o AD FS configuração banco de dados, juntamente com os diversos cenários de aplicativo que dão suporte a eles.|[Considerações de topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> Para implementar a redundância básico, balanceamento de carga e a opção para dimensionar o serviço de Federação \(if required\), recomendamos que você implantar pelo menos dois servidores de federação por farm de servidores de federação para todos os ambientes de produção, independentemente do tipo de banco de dados que você usará.  
  
Quando você tiver revisado o conteúdo na tabela anterior, vá para os seguintes tópicos nesta seção:  
  
-   [Servidor de Federação autônomo usando o trabalho](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [Farm de servidores de Federação usando o trabalho](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [Usando o trabalho e Proxies Farm do servidor de Federação](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [Farm de servidores de Federação usando o SQL Server](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
Depois que você terminar de selecionar a topologia de implantação do AD FS, recomendamos que você revise o tópico [planejamento de capacidade de servidor do AD FS](Planning-for-AD-FS-Server-Capacity.md) para determinar o número recomendado de servidores que você precisará implantar para dar suporte a essa topologia.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

