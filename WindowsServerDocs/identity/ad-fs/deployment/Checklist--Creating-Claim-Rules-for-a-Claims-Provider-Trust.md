---
ms.assetid: 503733f8-be0c-429c-81f0-cd4205e8b118
title: "Lista de verificação - criação de regras de declaração para um provedor de declarações confiável"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6b0ece3274b0e0a2a0d5e18e3c0ebf10ded67ebe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="checklist-creating-claim-rules-for-a-claims-provider-trust"></a>Lista de verificação: Criação de regras de declaração para um provedor de declarações confiável

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta lista de verificação inclui tarefas para planejar, projetar e implantar regras de declaração associados uma relação de confiança do provedor de declarações em serviços de Federação do Active Directory \(AD FS\).  
  
> [!NOTE]  
> Conclua as tarefas nesta lista de verificação na ordem. Quando um link de referência leva você para um procedimento, retorne para este tópico depois de concluir as etapas no procedimento para que você pode continuar com as tarefas restantes desta lista de verificação.  
  
![Criando reivindicar regras](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**lista de verificação: criar uma regra de declaração definido para uma relação de confiança do provedor de declarações**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Criar regras de declaração](media/icon_checkboxo.gif)|Revise os conceitos sobre declarações, reivindicar regras, conjuntos de regra de declarações e reivindicar modelos de regra e como eles são associados a federado relações de confiança.|![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[a função de declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)<br /><br />![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[a função de reivindicar regras](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)|  
|![Criar regras de declaração](media/icon_checkboxo.gif)|Revise os conceitos sobre como uma reivindicação flui através de todos os estágios no pipeline de emissão de declarações e como as regras são processadas pelo mecanismo de emissão de declarações.|![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[a função do Pipeline requerimentos judiciais ou Extrajudiciais](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)<br /><br />![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[a função do mecanismo requerimentos judiciais ou Extrajudiciais](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)|  
|![Criar regras de declaração](media/icon_checkboxo.gif)|Para planejar e implementar as declarações de saída serão emitidas por esta relação de confiança do provedor de declarações eficazmente, determine se uma ou mais regras de declaração são necessários e que solicite as regras que você deve usar com esta relação de confiança do provedor de declarações.|![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[determinar o tipo de declaração regra modelo para uso](../../ad-fs/technical-reference/Determine-the-Type-of-Claim-Rule-Template-to-Use.md)|  
|![Criar regras de declaração](media/icon_checkboxo.gif)|Revise os conceitos sobre quando criar uma declaração regra sobre outro e como você pode usar a linguagem de regra de declaração para fornecer uma lógica mais complexa do que as regras padrão para proporcionar um resultado desejado na saída do ideal conjunto de declarações.|![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma passagem ou uma regra de declaração de filtro](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)<br /><br />![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma regra de declaração de transformação](../../ad-fs/technical-reference/When-to-Use-a-Transform-Claim-Rule.md)<br /><br />![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar um atributos de LDAP enviar como declarações de regra](../../ad-fs/technical-reference/When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)<br /><br />![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma associação de grupo Enviar como uma regra de declaração](../../ad-fs/technical-reference/When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)<br /><br />![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma regra de declaração personalizada](../../ad-fs/technical-reference/When-to-Use-a-Custom-Claim-Rule.md)<br /><br />![Criando reivindicar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[a função da linguagem de regra de declaração](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)|  
|![Criar regras de declaração](media/icon_checkboxo.gif)|Uma descrição de declaração deve ser criada se ainda não existir que atenderá às necessidades da sua organização. AD FS é fornecido com um conjunto padrão de descrições de declaração que são expostos no AD FS gerenciamento snap\-in.|![Criando reivindicar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[adicionar uma descrição de declaração](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![Criar regras de declaração](media/icon_checkboxo.gif)|Dependendo das necessidades da sua organização, crie uma ou mais regras de declaração para o conjunto de regras de transformação de aceitação que está associada essa relação de confiança do provedor de declarações para que requerimentos judiciais ou Extrajudiciais serão emitidos adequadamente.|![Criando reivindicar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[criar uma regra para passagem ou filtrar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)<br /><br />![Criando reivindicar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[criar uma regra para enviar LDAP atributos como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)<br /><br />![Criando reivindicar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[criar uma regra para enviar a associação de grupo como uma declaração](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)<br /><br />![Criando reivindicar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[criar uma regra para transformar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)<br /><br />![Criando reivindicar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[criar uma regra para enviar uma declaração de método de autenticação](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)<br /><br />![Criando reivindicar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[criar uma regra para enviar um AD FS 1. x reivindicar compatível](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)<br /><br />![Criando reivindicar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[criar uma regra para enviar requerimentos judiciais ou Extrajudiciais usando uma regra personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)|  
  

