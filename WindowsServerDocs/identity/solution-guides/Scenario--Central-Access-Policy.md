---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: "Política de acesso Central do cenário"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1ec4165209b726609b1f9b2caeab02fb5072c756
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-central-access-policy"></a>Cenário: Política de acesso Central

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Políticas de acesso central para arquivos permitem que as organizações a implantar e gerenciar políticas de autorização que incluem expressões condicionais que usam grupos de usuários, declarações do usuário, declarações de dispositivo e propriedades do recurso centralmente. (As declarações são declarações sobre os atributos do objeto com o qual eles estão associados). Por exemplo, para acessar os dados do alto impacto de negócios (AIN), um usuário deve ser um funcionário em tempo integral, obter acesso a partir de um dispositivo gerenciado e faça logon com um cartão inteligente. Essas políticas são definidas e hospedadas nos serviços de domínio do Active Directory (AD DS).  
  
Políticas de acesso organizacional são controladas pela conformidade e requisitos regulatórios de negócios. Por exemplo, se uma organização tem um requisito de negócios para restringir o acesso a informações pessoalmente identificáveis (PII) nos arquivos somente o proprietário do arquivo e membros do departamento de recursos humanos (RH) que têm permissão para exibir as informações PII, essa política se aplica aos arquivos PII onde quer que eles estão localizados em servidores de arquivos em toda a organização. Neste exemplo, você precisa ser capaz de:  
  
-   Identifique e marcar os arquivos que contêm informações de identificação pessoal.  
  
-   Identifique o grupo dos membros do RH quem tem permissão para exibir as informações PII.  
  
-   Crie uma política de acesso central que se aplica a todos os arquivos que contenham PII onde quer que eles estão localizados em servidores de arquivos em toda a organização.  
  
A iniciativa para implantar e impor uma política de autorização pode vir por vários motivos e aplique a vários níveis de organização. Estes são alguns tipos de política de exemplo:  
  
-   **Política de autorização de toda a organização.** Mais comumente iniciada a partir do escritório de segurança de informações, esta configuração de política de autorização é regida pela conformidade ou um requisitos de alto nível de organização, e ele é relevante em toda a organização. Por exemplo, arquivos AIN estão acessíveis a somente os funcionários em tempo integral.  
  
-   **Política de autorização departamentais.** Cada departamento em uma organização tem alguns requisitos especiais de manipulação de dados que deseja aplicar. Por exemplo, o departamento de finanças talvez queira limitar o acesso aos servidores de finanças para os funcionários de finanças.  
  
-   **Política de gerenciamento de dados específico.** Esta configuração de política geralmente está relacionada a conformidade e requisitos de negócios, e ele é direcionado para proteger o acesso às informações que estiver sendo gerenciados correto. Por exemplo, instituições financeiras podem implementar paredes de informações para que analistas não acessar as informações de corretagem e agentes não acessar as informações de análise.  
  
-   **Política precisa saber.** Esse tipo de política de autorização é normalmente usado em conjunto com os tipos de política anterior. Por exemplo, os fornecedores devem ser capazes de acessar e editar apenas os arquivos que pertencem a um projeto que estão trabalhando.  
  
Ambientes de vida real também nos ensinam que cada política de autorização precisa ter exceções para que as organizações rapidamente podem reagir quando surgem as necessidades de negócios importantes. Por exemplo, executivos que não é possível encontrar os cartões inteligentes e precisa de acesso rápido a informações AIN podem chamar o suporte técnico para obter uma exceção temporária para acessar essas informações.  
  
Políticas de acesso central atuam como guarda-Chuvas de segurança que uma organização aplica-se em seus servidores. Essas políticas aprimoram (mas não substituir) as políticas de acesso local ou listas de controle de acesso discricionário (DACL) que são aplicadas a arquivos e pastas. Por exemplo, se uma DACL em um arquivo permite o acesso a um usuário específico, mas uma política central que é aplicada ao arquivo restringe o acesso ao mesmo usuário, o usuário não poderá obter acesso ao arquivo. Se a política de acesso central permite o acesso, mas a DACL não permitir o acesso, o usuário não poderá obter acesso ao arquivo.  
  
Uma regra de política de acesso central possui as seguintes partes lógicas:  
  
-   **Capacidade de aplicação.** Uma condição que define quais dados a política se aplica, como Resource.BusinessImpact=High.  
  
-   **Condições de acesso.** Uma lista de um ou mais controle entradas de acesso (ACEs) que definem quem pode acessar os dados, como permitir | Controle total | User.EmployeeType=FTE.  
  
-   **Exceções.** Uma lista adicional de uma ou mais ACEs que definem uma exceção para a política, como MemberOf(HBIExceptionGroup).  
  
As figuras a seguir mostram o fluxo de trabalho de acesso central e políticas de auditoria.  
  
![guias de soluções](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**Figura 1** conceitos de política de acesso e auditoria Central  
  
![guias de soluções](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**Figura 2** fluxo de trabalho de política de acesso Central  
  
A política de autorização central combina os seguintes componentes:  
  
-   Uma lista de regras de acesso definidas centralmente destinados tipos específicos de informações, como AIN ou PII.  
  
-   Uma política definida centralmente que contém uma lista de regras.  
  
-   Um identificador de política que é atribuído a cada arquivo nos servidores de arquivos para apontar para uma política de acesso central específicos que deve ser aplicada durante a autorização de acesso.  
  
A figura a seguir demonstra como você pode combinar as políticas em listas de política para controlar centralmente o acesso a arquivos.  
  
![guias de soluções](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**Figura 3** combinando políticas  
  
## <a name="in-this-scenario"></a>Neste cenário  
A orientação a seguir está disponível para você para políticas de acesso central:  
  
-   [Planejar uma implantação de política de acesso Central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [Implantar uma política de acesso Central #40 etapas de demonstração &; 41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e recursos que fazem parte desse cenário e descreve como suporte.  
  
|Função/recurso|Como ele dá suporte a esse cenário|  
|-----------------|---------------------------------|  
|Função de serviços de domínio do diretório ativa|AD DS no Windows Server 2012 apresenta uma plataforma de autorização baseada em declarações que permite a criação de declarações do usuário e declarações de dispositivo, identidade composta, (usuário além declarações de dispositivo), novos modelos de política (CAP) de acesso central e o uso das informações de classificação de arquivos nas decisões de autorização.|  
|Função de arquivo e o servidor de serviços de armazenamento|Serviços de arquivo e armazenamento fornece tecnologias que ajudam você a configurar e gerenciar um ou mais servidores de arquivos que fornecem locais centrais em sua rede onde você pode armazenar arquivos e compartilhá-los com os usuários. Se os usuários da rede precisam acessar os mesmos arquivos e aplicativos, ou se o gerenciamento centralizado de backup e arquivo é importante para sua organização, você deve configurar um ou mais computadores como um servidor de arquivos, adicionando a função Serviços de arquivo e armazenamento e os serviços de função apropriada para os computadores.|  
|Computador cliente do Windows|Os usuários podem acessar arquivos e pastas na rede por meio do computador cliente.|  
  


