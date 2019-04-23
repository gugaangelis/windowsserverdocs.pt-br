---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: Glossário de controle de acesso dinâmico do Apêndice A
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856697"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>Apêndice a: Glossário de controle de acesso dinâmico

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A seguir estão a lista de termos e definições que são incluídas no cenário de controle de acesso dinâmico.  
  
|Termo|Definição|  
|--------|--------------|  
|Classificação automática|Classificação que ocorre com base nas propriedades de classificação que são determinadas pelas regras de classificação configuradas por um administrador.|  
|CAPID|ID de política de acesso central. Essa ID faz referência a uma política de acesso central específicas, e ele é usado para referenciar a política do descritor de segurança de arquivos e pastas.|  
|Regra de acesso central|Uma regra que inclui uma condição e uma expressão de acesso.|  
|Política de acesso central|Políticas que são criadas e hospedadas no Active Directory.|  
|Controle de acesso baseado em declarações|Um paradigma que utiliza as declarações para tomar decisões de controle de acesso aos recursos.|  
|Classificação|O processo de determinar as propriedades de classificação de recursos e atribuir essas propriedades para os metadados que está associado com os recursos. Consulte também \h REF AutomaticClassification \\* a classificação automática MERGEFORMAT, REF InheritedClassification \h \\ \* classificação MERGEFORMAT herdadas e REF ManualClassification \h \\ \* Classificação Manual MERGEFORMAT.|  
|Declaração do dispositivo|Uma declaração que está associada com o sistema.  Com declarações de usuário, ele está incluído no token de um usuário tenta acessar um recurso.|  
|Lista de controle de acesso discricionário (DACL)|Uma lista de controle de acesso que identifica objetos de confiança que estão permitidos ou negados acesso a um recurso protegível. Ele pode ser modificado a critério do proprietário do recurso.|  
|Propriedade de recurso|Propriedades (como rótulos) que descrevem um arquivo e são atribuídas aos arquivos usando a classificação automática ou manual na classificação. Os exemplos incluem: Período de retenção, projeto e sensibilidade.|  
|Gerenciador de Recursos de Servidor de Arquivos|Um recurso do sistema operacional Windows Server que oferece gerenciamento de cotas de pasta, triagem de arquivo, relatórios de armazenamento, a classificação de arquivos e os trabalhos de gerenciamento de arquivo em um servidor de arquivos.|  
|Rótulos e as propriedades da pasta|As propriedades e os rótulos que descrevem uma pasta e são atribuídos manualmente por administradores e proprietários de pasta. Essas propriedades atribuem valores de propriedade padrão para os arquivos dentro dessas pastas, por exemplo, sigilo ou departamento.|  
|Política de Grupo|Um conjunto de regras e políticas que controla o ambiente de trabalho de usuários e computadores em um ambiente do Active Directory.|  
|Quase em tempo real de classificação|Classificação automática é executada logo após um arquivo é criada ou modificada.|  
|Perto de tarefas de gerenciamento de arquivos em tempo real|Tarefas de gerenciamento que são realizadas logo depois do arquivo (um arquivo é criado ou modificado. Essas tarefas são disparadas por classificação quase em tempo real.|  
|OU (Unidade Organizacional)|Um contêiner do Active Directory que representa as estruturas hierárquicas e lógicas dentro de uma organização. É o menor escopo ao qual política de grupo de configurações são aplicadas.|  
|Propriedade segura|Uma propriedade de classificação que o tempo de execução de autorização pode confiar para ser uma asserção válida sobre o recurso em um determinado point-in-time. No controle de acesso baseado em declarações, uma propriedade segura que é atribuída a um recurso é tratada como uma declaração de recurso.|  
|descritor de segurança|Uma estrutura de dados que contém informações de segurança associadas a um recurso protegível, como listas de controle de acesso.|  
|Linguagem de definição do descritor de segurança|Uma especificação que descreve as informações em um descritor de segurança como uma cadeia de caracteres de texto.|  
|Política de preparo|Uma política de acesso central que não ainda está em vigor.|  
|Lista de controle de acesso do sistema (SACL)|Uma lista de controle de acesso que especifica os tipos de tentativas de acesso por objetos de confiança específicos para o qual os registros de auditoria precisam ser gerado.|  
|Declaração de usuário|Atributos de um usuário que são fornecidos dentro do token de segurança do usuário. Os exemplos incluem: Espaço livre de departamento, empresa, projeto e segurança.  Informações no token de usuário de sistemas anteriores ao Windows Server 2012, como grupos de segurança que o usuário faz parte, também podem ser consideradas declarações de usuário. Algumas declarações do usuário são fornecidas por meio do Active Directory e outros são calculados dinamicamente, como se o usuário conectado com um cartão inteligente.|  
|Token de usuário|Um objeto de dados que identifica um usuário e as declarações de usuário e declarações de dispositivo que estão associadas esse usuário. Ele é usado para autorizar o acesso do usuário aos recursos.|  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso dinâmico: Visão geral do cenário](Dynamic-Access-Control--Scenario-Overview.md)  
  


