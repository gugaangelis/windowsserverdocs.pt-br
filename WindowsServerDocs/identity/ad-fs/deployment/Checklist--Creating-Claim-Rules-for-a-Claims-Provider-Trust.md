---
ms.assetid: 503733f8-be0c-429c-81f0-cd4205e8b118
title: Lista de verificação – criando regras de declaração para uma confiança do provedor de declarações
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: fb1e0dba5921a2f49452cab5cb622aed3e7eb57d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359956"
---
# <a name="checklist-creating-claim-rules-for-a-claims-provider-trust"></a>Lista de verificação: criando regras de declaração para uma confiança do provedor de declarações


Esta lista de verificação inclui tarefas para planejamento, criação e implantação de regras de declaração associadas a uma relação de confiança do provedor de declarações no Serviços de Federação do Active Directory (AD FS) \(AD FS\).  
  
> [!NOTE]  
> Execute as tarefas desta lista de verificação na ordem indicada. Quando um link de referência levar você a um procedimento, volte para este tópico depois de executar as etapas nesse procedimento para que você possa prosseguir com as tarefas restantes na lista de verificação.  
  
![criar](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**a lista de verificação de regras de declaração: Criando um conjunto de regras de declaração para uma confiança do provedor de declarações**  
  
||Tarefa|Referência|  
|-|--------|-------------|  
|![Criando regras de declaração](media/icon_checkboxo.gif)|Examine os conceitos sobre declarações, regras de declaração, conjuntos de regras de declaração e modelos de regra de declaração e como eles estão associados a relações de confiança federadas.|![criar regras](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[de declaração da função de declarações](../../ad-fs/technical-reference/The-Role-of-Claims.md)<br /><br />![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[a função de regras de declaração](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)|  
|![Criando regras de declaração](media/icon_checkboxo.gif)|Examine os conceitos sobre como uma declaração flui em todos os estágios no pipeline de emissão de declarações e como as regras são processadas pelo mecanismo de emissão de declarações.|![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[da função do pipeline de declarações](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md)<br /><br />![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[da função do mecanismo de declarações](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md)|  
|![Criando regras de declaração](media/icon_checkboxo.gif)|Para planejar e implementar efetivamente as declarações de saída que serão emitidas por essa confiança do provedor de declarações, determine se uma ou mais regras de declaração são necessárias e quais regras de declaração você deve usar com essa confiança do provedor de declarações.|![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[determinam o tipo de modelo de regra de declaração a ser usado](../../ad-fs/technical-reference/Determine-the-Type-of-Claim-Rule-Template-to-Use.md)|  
|![Criando regras de declaração](media/icon_checkboxo.gif)|Examine os conceitos sobre quando criar uma regra de declaração sobre outra e como você pode usar o idioma da regra de declaração para fornecer uma lógica mais complexa do que as regras padrão, a fim de fornecer um resultado desejado no conjunto de declarações de saída ideal.|![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma regra de passagem ou de declaração de filtro](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)<br /><br />![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma regra de declaração de transformação](../../ad-fs/technical-reference/When-to-Use-a-Transform-Claim-Rule.md)<br /><br />![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma regra enviar atributos LDAP como declarações](../../ad-fs/technical-reference/When-to-Use-a-Send-LDAP-Attributes-as-Claims-Rule.md)<br /><br />![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma associação de grupo de envio como uma regra de declaração](../../ad-fs/technical-reference/When-to-Use-a-Send-Group-Membership-as-a-Claim-Rule.md)<br /><br />![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quando usar uma regra de declaração personalizada](../../ad-fs/technical-reference/When-to-Use-a-Custom-Claim-Rule.md)<br /><br />![criar regras de Declaração](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[a função do idioma da regra de declaração](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)|  
|![Criando regras de declaração](media/icon_checkboxo.gif)|Uma descrição de declaração deve ser criada se ainda não existir uma que atenda às necessidades da sua organização. O AD FS é fornecido com um conjunto padrão de descrições de declaração que são expostas no\-snap de gerenciamento de AD FS no.|![criar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[de declaração adicionar uma descrição de declaração](../../ad-fs/operations/Add-a-Claim-Description.md)|  
|![Criando regras de declaração](media/icon_checkboxo.gif)|Dependendo das necessidades da sua organização, crie uma ou mais regras de declaração para o conjunto de regras de transformação de aceitação associado a essa confiança do provedor de declarações para que as declarações sejam emitidas adequadamente.|![criar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[de declaração criar uma regra para passar ou filtrar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)<br /><br />![criar regras de Declaração](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[criar uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)<br /><br />![criar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[de declaração criar uma regra para enviar a associação de grupo como uma declaração](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)<br /><br />![criar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[de declaração criar uma regra para transformar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)<br /><br />![criar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[de declaração criar uma regra para enviar uma declaração de método de autenticação](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)<br /><br />![criar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[de declaração criar uma regra para enviar uma declaração compatível com o AD FS 1. x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)<br /><br />![criar regras](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[de declaração criar uma regra para enviar declarações usando uma regra personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)|  
  

