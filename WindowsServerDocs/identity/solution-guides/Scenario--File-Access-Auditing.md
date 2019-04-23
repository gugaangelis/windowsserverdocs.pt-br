---
ms.assetid: 7be1f2cb-02d5-4209-ba79-edf496a88f47
title: Cenário de auditoria de acesso de arquivo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 93d78bbefce38173198f991543fb3a06d145b373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867677"
---
# <a name="scenario-file-access-auditing"></a>Cenário: Auditoria de acesso a arquivo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A Auditoria de Segurança é uma das ferramentas mais avançadas para ajudar a manter a segurança de uma empresa. Uma das metas principais das auditorias de segurança é a conformidade regulatória. Os padrões do setor, como Sarbanes Oxley, lei americana HIPAA (Health Insurance Portability Accountability Act) e Payment Card Industry (PCI), exigem que as empresas sigam um conjunto restrito de regras relacionadas à segurança e privacidade de dados. Auditorias de segurança ajudam a estabelecer a presença dessas políticas e comprovar a conformidade com essas normas. Além disso, elas ajudam a detectar comportamentos anormais, identificar e eliminar lacunas em políticas de segurança e impedir comportamentos irresponsáveis por meio da criação de um registro de atividades do usuário, que pode ser usado para análises periciais.  
  
Os requisitos de política de auditoria são normalmente direcionados aos seguintes níveis:  
  
-   **Segurança das informações.** As trilhas de auditoria de acesso ao arquivo são frequentemente usadas na análise forense e detecção de intrusão. Ser capaz de obter eventos específicos sobre o acesso às informações de alto valor permite que as organizações melhorem consideravelmente seu tempo de resposta e sua precisão de investigação.  
  
-   **Política organizacional.** Por exemplo, as organizações regulamentadas por normas PCI poderiam ter uma política central para monitorar o acesso a todos os arquivos marcados como contendo informações de cartão de crédito e informações de identificação pessoal (PII).  
  
-   **Política departamental.** Por exemplo, o departamento financeiro pode exigir que a capacidade de modificar determinados documentos financeiros (como um relatório de ganhos trimestrais) esteja restrita ao departamento financeiro e, assim, o departamento poderia monitorar todas as outras tentativas de alterar esses documentos.  
  
-   **Política de negócios.** Por exemplo, os proprietários de empresas podem monitorar todas as tentativas não autorizadas de exibir os dados que pertencem a seus projetos.  
  
Além disso, o departamento de conformidade pode monitorar todas as alterações nas políticas de autorização central e construtores de política, como usuário, computador e atributos de recursos.  
  
Uma das maiores considerações das auditorias de segurança é o custo de coleta, armazenamento e análise de eventos de auditoria. Se as políticas de auditoria forem muito amplas, o volume de eventos de auditoria coletado será maior, e isso aumentará os custos. Se as políticas de auditoria forem muito limitadas, você poderá perder eventos importantes.  
  
Com o Windows Server 2012, você pode criar políticas de auditoria usando declarações e propriedades do recurso. Isso leva a políticas de auditoria mais elaboradas, direcionadas e fáceis de gerenciar. Isso possibilita cenários cuja execução era impossível ou muito difícil até agora. Estes são exemplos de políticas de auditoria que os administradores podem criar:  
  
-   Realizar auditoria de todos que não têm autorização de segurança elevada e tentam acessar um documento HBI. Por exemplo, Audit | Everyone | All-Access | Resource.BusinessImpact=HBI AND User.SecurityClearance!=High.  
  
-   Realizar auditoria de todos os fornecedores quando tentam acessar documentos relacionados a projetos em que não estão trabalhando. Por exemplo, Audit | Everyone | All-Access | User.EmploymentStatus=Vendor AND User.Project Not_AnyOf Resource.Project.  
  
Essas políticas ajudam a regular o volume de eventos de auditoria e a limitá-los para apenas os dados ou usuários mais relevantes.  
  
Depois que os administradores criam e aplicam as políticas de auditoria, a próxima questão que eles devem considerar é a coleta de informações significativas dos eventos de auditoria coletados. Os eventos de auditoria com base em expressões ajudam a reduzir o volume de auditorias. No entanto, os usuários precisam um modo para consultar esses eventos para obter informações significativas e fazer perguntas, como "quem está acessando meus dados HBI?" ou "Houve uma tentativa não autorizada de acessar dados confidenciais?"  
  
 Windows Server 2012 aprimora os eventos de acesso de dados existentes com declarações de usuário, computador e recurso. Esses eventos são gerados por servidor. Para fornecer uma exibição completa dos eventos em toda a organização, a Microsoft está trabalhando com parceiros para fornecer ferramentas de coleta e análise de eventos, como os Serviços de Coleta de Auditoria no System Center Operation Manager.  
  
A Figura 4 mostra uma visão geral de uma política de auditoria central.  
  
![guias de soluções](media/Scenario--File-Access-Auditing/DynamicAccessControl_RevGuide_4.JPG)  
  
**Figura 4** Experiências de auditoria centrais  
  
A configuração e o consumo de auditorias de segurança normalmente envolvem as seguintes etapas gerais:  
  
1.  Identificar o conjunto correto de dados e usuários a ser monitorado  
  
2.  Criar e aplicar as políticas de auditoria apropriadas  
  
3.  Coletar e analisar os eventos de auditoria  
  
4.  Gerenciar e monitorar as políticas que foram criadas  
  
## <a name="in-this-scenario"></a>Neste cenário  
Os seguintes tópicos fornecem orientação adicional para este cenário:  
  
-   [Planejar para o arquivo de auditoria de acesso](Plan-for-File-Access-Auditing.md)  
  
-   [Implantar a auditoria de segurança com as políticas de auditoria Central &#40;etapas de demonstração&#41;](Deploy-Security-Auditing-with-Central-Audit-Policies--Demonstration-Steps-.md)  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista as funções e os recursos que fazem parte deste cenário e descreve como dar suporte a ele.  
  
|Função/recurso|Como este cenário tem suporte|  
|-----------------|---------------------------------|  
|Função dos Serviços de Domínio Active Directory|AD DS no Windows Server 2012 apresenta uma plataforma de autorização baseada em declarações que permite a criação de declarações de usuário e declarações de dispositivo, identidades compostas, (declarações de usuário + dispositivo), novo modelo de CAP (políticas) de acesso central e o uso de classificação de arquivos informações em decisões de autorização.|  
|Função dos Serviços de Arquivo e Armazenamento|Servidores de arquivos no Windows Server 2012 fornecem uma interface do usuário em que os administradores podem exibir as permissões efetivas para usuários para um arquivo ou pasta e solucionar problemas de acesso e conceder acesso conforme necessário.|  
  


