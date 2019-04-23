---
ms.assetid: 7f285c9f-c3e8-4aae-9ff4-a9123815114e
title: Política de acesso Central do cenário
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1ec4165209b726609b1f9b2caeab02fb5072c756
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873407"
---
# <a name="scenario-central-access-policy"></a>Cenário: Política de Acesso Central

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Políticas de acesso central a arquivos permitem que as organizações implantem e gerenciem, de maneira centralizada, políticas de autorização que incluem expressões condicionais usando declarações de usuário, declarações de dispositivo e propriedades de recursos. (Declarações são afirmações sobre os atributos do objeto às quais eles estão associados). Por exemplo, para acessar dados HBI (Alto Impacto nos Negócios), um usuário deve ser um funcionário em tempo integral, obter acesso por meio de um dispositivo gerenciado e fazer logon com um cartão inteligente. Essas políticas são definidas e hospedadas no AD DS (Serviços de Domínio do Active Directory).  
  
Políticas de acesso organizacional orientadas por requisitos de conformidade e negócios. Por exemplo, se uma organização tiver um requisito de negócios para restringir o acesso a PII (Informações de identificação Pessoal) em arquivos a apenas o proprietário do arquivo e membros do departamento de RH (Recursos Humanos) que podem ver as informações PII, essa política se aplicará a arquivos PII, independentemente de onde eles estejam localizados nos servidores de arquivos em toda a organização. Neste exemplo, você precisa ser capaz de:  
  
-   Identificar e marcar os arquivos que contêm PII.  
  
-   Identificar o grupo de membros de RH que têm permissão para exibir informações PII.  
  
-   Criar uma política de acesso central que se aplique a todos os arquivos que contêm PII onde quer que eles estejam localizados em servidores de arquivos em toda a organização.  
  
A iniciativa de implantar e reforçar uma política de autorização pode surgir por diversas razões e se aplicam a vários níveis da organização. Veja a seguir alguns exemplos de tipos de política:  
  
-   **Política de autorização para toda a organização.** Geralmente iniciada no departamento de segurança das informações, essa política de autorização é orientada pelos requisitos de conformidade e alto nível da organização, sendo relevante para toda a organização. Por exemplo, arquivos de alto impacto nos negócios são acessíveis apenas a funcionários em tempo integral.  
  
-   **Política de autorização de departamento.** Cada departamento em uma organização tem certos requisitos de tratamento de dados especiais que devem ser impostos. O departamento financeiro, por exemplo, pode querer limitar o acesso dos funcionários de finanças ao servidores financeiros.  
  
-   **Política específica de gerenciamento de dados.** Essa política geralmente está relacionada a requisitos de conformidade e de negócios, com o objetivo de proteger o acesso devido às informações gerenciadas. Por exemplo, instituições financeiras podem implementar bloqueios de informações para que os analistas não acessam informações de ações e os corretores não acessem informações de análise.  
  
-   **Política de divulgação restrita aos diretamente interessados.** Esse tipo de política de autorização é geralmente usado em conjunto com os tipos de política anteriores. Por exemplo, os fornecedores devem poder acessar e editar somente arquivos que pertencem a um projeto no qual estejam trabalhando.  
  
Ambientes reais também nos ensinam que cada política de autorização deve ter exceções para que as organizações possam reagir rapidamente quando surgirem necessidades de negócios importantes. Por exemplo, executivos que não é possível localizar seus cartões inteligentes e precisam de acesso rápido a informações de alto impacto nos negócios podem chamar o suporte técnico para obter uma exceção temporária para acessar essas informações.  
  
As políticas de acesso central atuam como protetores de segurança que uma organização aplica em seus servidores. Essas políticas complementam (mas não substituem) as políticas de acesso local ou DACLs (Listas de Controle de Acesso Discricionário) aplicadas a arquivos e pastas. Por exemplo, se uma DACL em um arquivo permitir o acesso a um usuário específico, mas uma política central aplicada ao arquivo restringir o acesso ao mesmo usuário, o usuário não poderá obter acesso ao arquivo. Se a política de acesso central permitir o acesso, mas a DACL não permitir, o usuário não poderá obter acesso ao arquivo.  
  
Uma regra de política de acesso central tem as seguintes partes lógicas:  
  
-   **Aplicabilidade.** uma condição que define a quais dados as políticas devem ser aplicadas, por exemplo, Resource.BusinessImpact=High.  
  
-   **Condições de acesso.** Uma lista de uma ou mais ACEs (Entradas de Controle de Acesso) que definem quem pode acessar os dados, como permitir | Controle Total | User.EmployeeType=FTE.  
  
-   **Exceções.** Uma lista adicional contendo uma ou mais ACEs que define muma exceção para a política, por exemplo, MemberOf(HBIExceptionGroup).  
  
As duas figuras a seguir mostram o fluxo de trabalho de acesso central e políticas de auditoria.  
  
![guias de soluções](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide.JPG)  
  
**Figura 1** Acesso central e conceitos de política de auditoria  
  
![guias de soluções](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide_2.JPG)  
  
**Figura 2** Fluxo de trabalho da política de acesso central  
  
A política de autorização central combina os seguintes componentes:  
  
-   Uma lista de regras de acesso definidas centralmente que tem como alvo tipos específicos de informações, como HBI ou PII.  
  
-   Uma política definida centralmente que contém uma lista de regras.  
  
-   Um identificador de política que é atribuído a cada arquivo nos servidores de arquivos para apontar para uma política de acesso central específica que deve ser aplicada durante a autorização de acesso.  
  
A figura a seguir demonstra como combinar políticas em listas de política para controlar centralmente o acesso aos arquivos.  
  
![guias de soluções](media/Scenario--Central-Access-Policy/DynamicAccessControl_RevGuide3.JPG)  
  
**Figura 3** Combinando políticas  
  
## <a name="in-this-scenario"></a>Neste cenário  
As políticas a seguir estão disponíveis para políticas de acesso central:  
  
-   [Planejar uma implantação de política de acesso Central](assetId:///0311a76d-d66c-4ddb-ade6-af586a2ad82f)  
  
-   [Implantar uma política de acesso Central &#40;etapas de demonstração&#41;](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md)  
  
-   [Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e os recursos que fazem parte deste cenário e descreve como dar suporte a ele.  
  
|Função/recurso|Como este cenário tem suporte|  
|-----------------|---------------------------------|  
|Função de Serviços de Domínio do Active Directory|AD DS no Windows Server 2012 apresenta uma plataforma de autorização baseada em declarações que permite a criação de declarações de usuário e declarações de dispositivo, identidades compostas, (declarações de usuário + dispositivo), novos modelos de CAP (política) de acesso central e o uso de classificação de arquivos informações em decisões de autorização.|  
|Função Servidor de Serviços de Arquivo e Armazenamento|Serviços de Arquivo e Armazenamento oferecem tecnologias que ajudam a configurar e gerenciar um ou mais servidores de arquivos, os quais são servidores que fornecem localizações centrais na rede na qual é possível armazenar arquivos e compartilhá-los com os usuários. Se os usuários da rede precisam ter acesso aos mesmos arquivos e aplicativos, ou se o backup centralizado e gerenciamento de arquivos são importantes para sua organização, você deve configurar um ou mais computadores como servidor de arquivos, adicionando a função de Serviços de Arquivo e Armazenamento e os serviços de função apropriados aos computadores.|  
|Computador cliente Windows|Os usuários podem acessar arquivos e pastas da rede por meio do computador cliente.|  
  


