---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: "Apêndice A Glossário de controle de acesso dinâmico"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Apêndice a: Glossário de controle de acesso dinâmico

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A seguir estão a lista de termos e as definições que estão incluídas no cenário de controle de acesso dinâmico.  
  
|Termo|Definição|  
|--------|--------------|  
|Classificação automática|Classificação que ocorre com base nas propriedades de classificação que são determinadas pelas regras de classificação configuradas por um administrador.|  
|CAPID|ID de política de acesso central. Essa ID faz referência a uma política de acesso central específico, e ele é usado para fazer referência a política do descritor de segurança de arquivos e pastas.|  
|Regras de acesso central|Uma regra que inclui uma condição e uma expressão de acesso.|  
|Política de acesso central|Políticas que são criadas e hospedadas no Active Directory.|  
|Controle de acesso baseada em declarações|Um paradigma que utiliza declarações para tomar decisões de controle de acesso aos recursos.|  
|Classificação|O processo de determinar as propriedades de classificação de recursos e atribuindo essas propriedades para os metadados que está associado com os recursos. Consulte também REF AutomaticClassification \h \ \ * classificação MERGEFORMAT automática, REF InheritedClassification \h \ \ \ * MERGEFORMAT herdados classificação e REF ManualClassification \h \ \ \ * classification MERGEFORMAT Manual.|  
|Declaração de dispositivo|Uma declaração que está associada com o sistema.  Com as declarações do usuário, ela está incluída no token de um usuário tentar acessar um recurso.|  
|Lista de controle de acesso discricionário (DACL)|Uma lista de controle de acesso que identifica objetos de confiança que são acesso permitidos ou negados a um recurso protegível. Ele pode ser modificado a critério do proprietário do recurso.|  
|Propriedade Resource|Propriedades (como rótulos) que descrevem um arquivo e são atribuídas a arquivos usando a classificação automática ou manual classificação. Os exemplos incluem: período de retenção de sensibilidade e projeto.|  
|Gerenciador de recursos do servidor de arquivos|Um recurso no sistema operacional Windows Server que oferece gerenciamento de cotas de pasta, triagem de arquivo, relatórios de armazenamento, classificação de arquivos e trabalhos de gerenciamento de arquivo em um servidor de arquivos.|  
|Rótulos e propriedades da pasta|Propriedades e rótulos que descrevem uma pasta e são atribuídos manualmente por administradores e proprietários da pasta. Essas propriedades atribuem valores de propriedade padrão para os arquivos dentro dessas pastas, por exemplo, sigilo ou departamento.|  
|Política de grupo|Um conjunto de regras e políticas que controla o ambiente de trabalho de usuários e computadores em um ambiente do Active Directory.|  
|Perto de classificação de tempo real|Classificação automática que é executada logo depois que um arquivo é criado ou modificado.|  
|Perto de tarefas de gerenciamento de arquivo em tempo real|Tarefas de gerenciamento que são executadas logo depois do arquivo (um arquivo é criado ou modificado. Essas tarefas são acionadas pela classificação quase em tempo real.|  
|Unidade organizacional (UO)|Um contêiner do Active Directory que representa as estruturas hierárquicas e lógicas dentro de uma organização. É o menor escopo para qual política de grupo configurações são aplicadas.|  
|Propriedade Secure|Uma propriedade de classificação que o tempo de execução de autorização pode confiável para ser uma declaração válida sobre o recurso em um determinado ponto-in-time. Em controle de acesso baseado em declarações, uma propriedade segura que é atribuída a um recurso é tratada como uma declaração de recurso.|  
|Descritor de segurança|Uma estrutura de dados que contém informações de segurança associadas a um recurso protegível, como listas de controle de acesso.|  
|Linguagem de definição do descritor de segurança|Uma especificação que descreve as informações em um descritor de segurança como uma cadeia de caracteres de texto.|  
|Política de preparo|Uma política de acesso central que não ainda chegou em vigor.|  
|Lista de controle de acesso do sistema (SACL)|Uma lista de controle de acesso que especifica os tipos de tentativas de acesso de objetos de confiança específicos para o qual os registros de auditoria precisam ser geradas.|  
|Declaração de usuário|Atributos de um usuário que são fornecidos no token de segurança do usuário. Os exemplos incluem: liberação departamento, empresa, projeto e segurança.  Informações no token do usuário de sistemas anteriores ao Windows Server 2012, como os grupos de segurança que o usuário faz parte do, também podem ser consideradas declarações do usuário. Algumas declarações do usuário são fornecidas por meio do Active Directory e outros são calculados dinamicamente, como se o usuário conectado com um cartão inteligente.|  
|Token de usuário|Um objeto de dados que identificam um usuário e as declarações do usuário e declarações de dispositivo que estão associadas esse usuário. Ele é usado para autorizar o acesso do usuário aos recursos.|  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  


