---
title: Kerberos Constrained Delegation Overview
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51923b0a-0c1a-47b2-93a0-d36f8e295589
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 02677c8d9db4129ebbd7edd79027e0a6348372b5
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544612"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico de visão geral do profissional de ti descreve os novos recursos para a delegação restrita de Kerberos no Windows Server 2012 R2 e no Windows Server 2012.

**Descrição do recurso**

A delegação restrita de Kerberos foi introduzida no Windows Server 2003 para proporcionar uma forma de delegação mais segura para ser usada em serviços. Quando configurada, a delegação restrita restringe os serviços nos quais um determinado servidor pode agir em nome de um usuário. Isso requer privilégios de administrador de domínio para configurar uma conta de domínio para um serviço e restringe a conta a um único domínio. Na empresa de hoje, os serviços front-end não são projetados para serem limitados à integração apenas com serviços em seu domínio.

Nos sistemas operacionais anteriores, em que o administrador de domínio configurava o serviço, o administrador de serviços não tinha muito como saber quais serviços front-end eram delegados aos serviços de recurso que eles tinham. Portanto, qualquer serviço front-end que pudesse ser delegado a um serviço de recurso representava um ponto de ataque potencial. Se um servidor que hospedasse um serviço front-end fosse comprometido, e estivesse configurado para delegar para serviços de recurso, os serviços de recurso também poderiam ser comprometidos.

No Windows Server 2012 R2 e no Windows Server 2012, a capacidade de configurar a delegação restrita para o serviço foi transferida do administrador de domínio para o administrador de serviços. Dessa forma, o administrador de serviços back-end pode permitir ou negar serviços front-end.

Para obter informações detalhadas sobre delegação restrita, conforme apresentada no Windows Server 2003, consulte [Transição de protocolo e delegação restrita de Kerberos](https://technet.microsoft.com/library/cc739587(v=ws.10)).

A implementação do Windows Server 2012 R2 e do Windows Server 2012 do protocolo Kerberos inclui extensões específicas para delegação restrita.  O S4U2Proxy (Service for User to Proxy) permite que um serviço use seu respectivo tíquete de serviço Kerberos para que um usuário obtenha um tíquete de serviço do KDC (Centro de Distribuição de Chaves) para um serviço back-end. Essas extensões permitem que a delegação restrita seja configurada na conta do serviço de back-end, que pode estar em outro domínio. Para obter mais informações sobre essas extensões, [consulte \[MS-\]SFU: Extensões do protocolo Kerberos: Serviço para especificação](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) de protocolo de delegação restrita e de usuário na biblioteca MSDN.

**Aplicativos práticos**

A delegação restrita dá aos administradores de serviço a capacidade de especificar e impor limites de confiança do aplicativo, limitando o escopo em que os serviços de aplicativo podem agir em nome de um usuário. Os administradores de serviços podem configurar quais contas de serviço front-end podem ser delegadas aos seus respectivos serviços back-end.

Ao dar suporte à delegação restrita entre domínios no Windows Server 2012 R2 e no Windows Server 2012, serviços de front-end como o Microsoft ISA (Internet Security and Acceleration) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange O Outlook Acesso via Web (OWA) e Microsoft SharePoint Server podem ser configurados para usar a delegação restrita para autenticar servidores em outros domínios. Isso dá suporte a soluções de serviços entre domínios usando uma infraestrutura Kerberos existente. A delegação restrita de Kerberos pode ser gerenciada por administradores de domínio ou administradores de serviços.

## <a name="resource-based-constrained-delegation-across-domains"></a>Delegação restrita entre domínios com base em recursos

A delegação restrita de Kerberos pode ser usada para fornecer delegação restrita quando o serviço front-end e os serviços de recurso não estão no mesmo domínio. Os administradores de serviços podem configurar a nova delegação especificando as contas de domínio dos serviços front-end que podem representar usuários nos objetos de conta dos serviços de recurso.

**Qual é o valor agregado desta alteração?**

Com o suporte à delegação restrita entre domínios, os serviços podem ser configurados para usar a delegação restrita para autenticação em servidores em outros domínios usando delegação não restrita. Isso dá suporte à autenticação para soluções de serviços entre domínios usando uma infraestrutura Kerberos existente sem a necessidade de confiar em serviços front-end para delegar a qualquer outro serviço.

Isso também muda a decisão de se um servidor deve confiar na origem de uma identidade delegada do administrador de domínio de delegação para o proprietário do recurso.

**O que passou a funcionar de maneira diferente?**

Uma alteração no protocolo subjacente permite a delegação restrita entre domínios. A implementação do Windows Server 2012 R2 e do Windows Server 2012 do protocolo Kerberos inclui extensões para serviço para o protocolo S4U2Proxy (user to proxy). Trata-se de um conjunto de extensões para o protocolo Kerberos que permite que um serviço use seu respectivo tíquete de serviço Kerberos para que um usuário obtenha um tíquete de serviço do KDC (Centro de Distribuição de Chaves) para um serviço back-end.

Para obter informações de implementação sobre essas extensões [, consulte \[MS\]-SFU: Extensões do protocolo Kerberos: Serviço para especificação](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) de protocolo de delegação restrita e de usuário no msdn.

Para obter mais informações sobre a sequência de mensagens básica para delegação de Kerberos com um tíquete de concessão de tíquete (TGT) encaminhado em comparação com as extensões de serviço para usuário (S4U), consulte a seção [visão geral do protocolo 1.3.3](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) no [MS-SFU]: Extensões do protocolo Kerberos: Serviço para usuário e especificação do protocolo de delegação restrita.

**Implicações de segurança da delegação restrita baseada em recursos**

A delegação restrita baseada em recursos coloca o controle da delegação nas mãos do administrador proprietário do recurso que está sendo acessado. Depende de atributos do serviço de recursos em vez do serviço que está sendo confiável para delegar. Como resultado, a delegação restrita baseada em recursos não pode usar o bit confiável para autenticar para delegação que anteriormente controlava a transição de protocolo. O KDC sempre permite a transição de protocolo ao executar a delegação restrita baseada em recursos como se o bit estivesse definido.

Como o KDC não limita a transição de protocolo, foram introduzidos dois novos SIDs conhecidos para dar esse controle ao administrador de recursos.  Esses SIDs identificam se a transição de protocolo ocorreu e pode ser usada com listas de controle de acesso padrão para conceder ou limitar o acesso conforme necessário.

|SID|Descrição|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|Um SID que significa que a identidade do cliente é declarada por uma autoridade de autenticação com base na prova de posse das credenciais do cliente.|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|Um SID que significa que a identidade do cliente é declarada por um serviço.|

Um serviço de back-end pode usar expressões ACL padrão para determinar como o usuário foi autenticado.

**Como configurar a delegação restrita baseada em recursos?**

Para configurar um serviço de recurso para permitir acesso a um serviço front-end em nome de usuários, use cmdlets do Windows PowerShell.

-   Para recuperar uma lista de entidades, use os cmdlets **Get-ADComputer**, **Get-ADServiceAccount**e **Get-ADUser** com o parâmetro **PrincipalsAllowedToDelegateToAccount Properties** .

-   Para configurar o serviço de recursos, use os cmdlets **novo-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **set-ADComputer**, **set-ADServiceAccount**e **set-ADUser** com o  **Parâmetro PrincipalsAllowedToDelegateToAccount** .

## <a name="BKMK_SOFT"></a>Requisitos de software
A delegação restrita baseada em recursos só pode ser configurada em um controlador de domínio que executa o Windows Server 2012 R2 e o Windows Server 2012, mas pode ser aplicada em uma floresta de modo misto.

Você deve aplicar o hotfix a seguir a todos os controladores de domínio que executam o Windows Server 2012 em domínios de conta de usuário no caminho de referência entre os domínios de front-end e back-end que estão executando sistemas operacionais anteriores ao Windows Server:  A delegação restrita baseada em recursos KDC_ERR_POLICY falha em ambientes que têm controladores de domínio baseados no Windows Server 2008 R2 (https://support.microsoft.com/en-gb/help/2665790/resource-based-constrained-delegation-kdc-err-policy-failure-in-enviro).
