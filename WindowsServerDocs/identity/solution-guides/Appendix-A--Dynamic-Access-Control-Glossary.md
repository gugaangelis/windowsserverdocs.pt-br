---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: Apêndice A um glossário de controle de acesso dinâmico
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 328d8c74e57150c94d0a6032ff88a3600fe0baf3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859279"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Apêndice A: Glossário de Controle de Acesso Dinâmico

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A seguir estão a lista de termos e definições que estão incluídos no cenário de controle de acesso dinâmico.  
  
|Termo|Definição|  
|--------|--------------|  
|Classificação automática|Classificação que ocorre com base nas propriedades de classificação que são determinadas pelas regras de classificação configuradas por um administrador.|  
|CAPId|ID da política de acesso central. Essa ID faz referência a uma política de acesso central específica e é usada para fazer referência à política do descritor de segurança de arquivos e pastas.|  
|Regra de acesso central|Uma regra que inclui uma condição e uma expressão de acesso.|  
|Política de acesso central|Políticas que são criadas e hospedadas no Active Directory.|  
|Controle de acesso baseado em declarações|Um paradigma que utiliza declarações para tomar decisões de controle de acesso a recursos.|  
|Classificação|O processo de determinar as propriedades de classificação de recursos e atribuir essas propriedades aos metadados associados aos recursos. Consulte também REF AutomaticClassification \h \\* MERGEFORMAT classificação automática, REF InheritedClassification \h \\\* a classificação herdada de MERGEFORMAT e REF ManualClassification \h \\\* MERGEFORMAT classificação manual.|  
|Declaração do dispositivo|Uma declaração associada ao sistema.  Com as declarações do usuário, ele é incluído no token de um usuário que está tentando acessar um recurso.|  
|DACL (lista de controle de acesso discricionário)|Uma lista de controle de acesso que identifica os confiáveis que têm permissão ou acesso negado a um recurso protegível. Ele pode ser modificado a critério do proprietário do recurso.|  
|Propriedade de recurso|Propriedades (como rótulos) que descrevem um arquivo e são atribuídos aos arquivos usando a classificação automática ou a classificação manual. Os exemplos incluem: sensibilidade, projeto e período de retenção.|  
|Gerenciador de Recursos de Servidor de Arquivos|Um recurso no sistema operacional Windows Server que oferece gerenciamento de cotas de pastas, triagens de arquivos, relatórios de armazenamento, classificação de arquivos e trabalhos de gerenciamento de arquivos em um servidor de arquivos.|  
|Rótulos e propriedades da pasta|Propriedades e rótulos que descrevem uma pasta e são atribuídos manualmente por administradores e proprietários de pasta. Essas propriedades atribuem valores de propriedade padrão aos arquivos dentro dessas pastas, por exemplo, Sigilor ou departamento.|  
|Diretiva de grupos|Um conjunto de regras e políticas que controla o ambiente de trabalho de usuários e computadores em um ambiente de Active Directory.|  
|Classificação quase em tempo real|Classificação automática executada logo depois que um arquivo é criado ou modificado.|  
|Tarefas de gerenciamento de arquivos quase em tempo real|Tarefas de gerenciamento de arquivos que são executadas logo após (um arquivo é criado ou modificado. Essas tarefas são disparadas pela classificação quase em tempo real.|  
|OU (Unidade Organizacional)|Um contêiner Active Directory que representa estruturas hierárquicas e lógicas dentro de uma organização. É o menor escopo ao qual Política de Grupo configurações são aplicadas.|  
|Propriedade segura|Uma propriedade de classificação que o tempo de execução de autorização pode confiar para ser uma asserção válida sobre o recurso em um determinado ponto no tempo. No controle de acesso baseado em declarações, uma propriedade segura que é atribuída a um recurso é tratada como uma declaração de recurso.|  
|Descritor de segurança|Uma estrutura de dados que contém informações de segurança associadas a um recurso protegível, como listas de controle de acesso.|  
|Linguagem de definição do descritor de segurança|Uma especificação que descreve as informações em um descritor de segurança como uma cadeia de texto.|  
|Política de preparo|Uma política de acesso central que ainda não está em vigor.|  
|Lista de controle de acesso do sistema (SACL)|Uma lista de controle de acesso que especifica os tipos de tentativas de acesso por determinados confiáveis para os quais os registros de auditoria precisam ser gerados.|  
|Declaração do usuário|Atributos de um usuário que são fornecidos dentro do token de segurança do usuário. Os exemplos incluem: departamento, empresa, projeto e espaço livre de segurança.  As informações no token do usuário de sistemas anteriores ao Windows Server 2012, como os grupos de segurança dos quais o usuário faz parte, também podem ser consideradas declarações do usuário. Algumas declarações de usuário são fornecidas por meio de Active Directory e outras são calculadas dinamicamente, como se o usuário fez logon com um cartão inteligente.|  
|Token de usuário|Um objeto de dados que identifica um usuário e as declarações do usuário e as declarações do dispositivo que estão associadas a esse usuário. Ele é usado para autorizar o acesso do usuário aos recursos.|  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso dinâmico: visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  


