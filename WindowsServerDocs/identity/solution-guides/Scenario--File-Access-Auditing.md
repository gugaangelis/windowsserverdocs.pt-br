---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: "Auditoria de acesso de arquivo do cenário"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 93d78bbefce38173198f991543fb3a06d145b373
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-file-access-auditing"></a>Cenário: Auditoria de acesso de arquivos

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Auditoria de segurança é uma das ferramentas mais poderosas para ajudar a manter a segurança de uma empresa. Um dos principais objetivos da auditorias de segurança é conformidade regulatória. Padrões do setor, como Sarbanes Oxley, portabilidade de seguros de saúde e responsabilidade Act (HIPAA) e do setor de cartão de pagamento (PCI) exigem que as empresas a seguir um conjunto estrito de regras relacionadas a privacidade e segurança dos dados. Auditorias de segurança ajuda para estabelecer a presença dessas políticas e comprovar a conformidade com esses padrões. Além disso, auditorias de segurança ajudam a detectar comportamento anômalos, identificar e reduzir falhas em políticas de segurança e impedir o comportamento irresponsável através da criação de uma trilha de atividade do usuário que pode ser usada para perícia.  
  
Requisitos de política de auditoria normalmente são conduzidos nos seguintes níveis:  
  
-   **Segurança das informações.** Trilhas de auditoria de acesso de arquivo são geralmente usadas para a detecção de invasões e análise forense. As organizações a capacidade de se eventos segmentados sobre acesso a informações de alto valor vamos consideravelmente a melhorar sua precisão de investigação e tempo de resposta.  
  
-   **Políticas da organização.** Por exemplo, as organizações regulamentadas por padrões PCI poderiam ter uma política central para monitorar o acesso a todos os arquivos que são marcados como contendo informações de cartão de crédito e informações pessoalmente identificáveis (PII).  
  
-   **Política departamental.** Por exemplo, o departamento de Finanças pode exigir que a capacidade de modificar certos documentos de Finanças (por exemplo, um relatório de lucros trimestrais) ser restritas ao departamento de finanças e, portanto, o departamento de desejaria monitorar todos os outras tentativas de alterar esses documentos.  
  
-   **Política de negócios.** Por exemplo, empresários convém monitorar todas as tentativas não autorizadas para exibir dados que pertencem aos seus projetos.  
  
Além disso, o departamento de conformidade talvez queira monitorar as alterações todas as políticas de autorização central e construtores de política como usuário, computador e atributos de recursos.  
  
Uma das maiores considerações de auditoria de segurança é o custo de coletar, armazenar e analisar os eventos de auditoria. Se as políticas de auditoria são muito amplas, aumenta o volume de eventos de auditoria coletados, e isso aumenta os custos. Se as políticas de auditoria são muito estreitas, você corre o risco de perder eventos importantes.  
  
Com o Windows Server 2012, você pode criar políticas de auditoria usando declarações e propriedades do recurso. Isso leva a políticas de auditoria mais ricas, mais direcionada e mais fácil de gerenciar. Ele permite cenários que, até agora, impossíveis ou muito difíceis de serem executadas. Estes são exemplos de políticas de auditoria que os administradores podem criar:  
  
-   Auditoria de todas as pessoas que não tenha um vão de alta segurança e tenta acessar um documento AIN. Por exemplo, de auditoria | Todos | Acesso | Resource.BusinessImpact=HBI e User.SecurityClearance!=High.  
  
-   Auditar todos os fornecedores quando eles tentam acessar documentos que estão relacionados a projetos que não estão trabalhando. Por exemplo, de auditoria | Todos | Acesso | User.EmploymentStatus=Vendor e User.Project Not_AnyOf Resource.Project.  
  
Essas políticas ajudam a controlar o volume de eventos de auditoria e limitá-los a somente os dados mais relevantes ou os usuários.  
  
Depois que os administradores têm criado e aplicado as políticas de auditoria, a próxima consideração por eles é gleaning informações significativas dos eventos de auditoria que elas coletados. Eventos de auditoria baseadas em expressão ajudam a reduzir o volume de auditoria. No entanto, os usuários precisam uma maneira de consultar esses eventos para informações significativas e faça perguntas como, "quem está acessando meus dados AIN?" Ou "Tentativa lá não autorizado de acesso a dados confidenciais?"  
  
 Windows Server 2012 aprimora a eventos de acesso de dados existentes com declarações do usuário, o computador e o recurso. Esses eventos são gerados em cada servidor. Para fornecer uma visão completa dos eventos em toda a organização, a Microsoft está trabalhando com nossos parceiros para fornecer a coleta de eventos e ferramentas de análise, como os serviços de coleta de auditoria no System Center operação Manager.  
  
Figura 4 mostra uma visão geral de uma política de auditoria central.  
  
![guias de soluções](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**Figura 4** experiências de auditoria Central  
  
Configurando e consumindo auditorias de segurança normalmente envolvem as seguintes etapas gerais:  
  
1.  Identificar o conjunto correto de dados e os usuários monitorem  
  
2.  Criar e aplicar políticas de auditoria apropriado  
  
3.  Coletar e analisar os eventos de auditoria  
  
4.  Gerenciar e monitorar as políticas que foram criadas  
  
## <a name="in-this-scenario"></a>Neste cenário  
Os tópicos a seguir fornecem diretrizes adicionais para este cenário:  
  
-   [Planeje para o arquivo de auditoria de acesso](Plan-for-File-Access-Auditing.md)  
  
-   [Implantar a auditoria de segurança com políticas de auditoria Central #40 etapas de demonstração &; 41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e recursos que fazem parte desse cenário e descreve como suporte.  
  
|Função/recurso|Como ele dá suporte a esse cenário|  
|-----------------|---------------------------------|  
|Função de serviços de domínio Directory ativa|AD DS no Windows Server 2012 apresenta uma plataforma de autorização baseada em declarações que permite a criação de declarações do usuário e declarações de dispositivo, identidade composta, (usuário além declarações de dispositivo), novo modelo de acesso central (CAP) as políticas e o uso das informações de classificação de arquivo em decisões de autorização.|  
|Função Serviços de arquivo e armazenamento|Servidores de arquivos no Windows Server 2012 fornecem uma interface do usuário em que os administradores podem ver as permissões efetivas para usuários de um arquivo ou pasta e solucionar problemas de acesso e conceder acesso conforme necessário.|  
  


