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
ms.openlocfilehash: d77dd6f6f310ae71a4d9e2d52b44cc9b1bef6401
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821927"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico de visão geral para profissionais de TI descreve as novas funcionalidades para a delegação restringido de Kerberos no Windows Server 2012 R2 e Windows Server 2012.

**Descrição do recurso**

A delegação restrita de Kerberos foi introduzida no Windows Server 2003 para proporcionar uma forma de delegação mais segura para ser usada em serviços. Quando configurada, a delegação restrita restringe os serviços nos quais um determinado servidor pode agir em nome de um usuário. Isso requer privilégios de administrador de domínio para configurar uma conta de domínio para um serviço e restringe a conta a um único domínio. Nas empresas de hoje, os serviços front-end não são projetados para se limitar à integração apenas com serviços em seu domínio.

Nos sistemas operacionais anteriores, em que o administrador de domínio configurava o serviço, o administrador de serviços não tinha muito como saber quais serviços front-end eram delegados aos serviços de recurso que eles tinham. Portanto, qualquer serviço front-end que pudesse ser delegado a um serviço de recurso representava um ponto de ataque potencial. Se um servidor que hospedasse um serviço front-end fosse comprometido, e estivesse configurado para delegar para serviços de recurso, os serviços de recurso também poderiam ser comprometidos.

No Windows Server 2012 R2 e Windows Server 2012, capacidade de configurar a delegação restrita para o serviço foi transferida do administrador de domínio para o administrador de serviço. Dessa forma, o administrador de serviços back-end pode permitir ou negar serviços front-end.

Para obter informações detalhadas sobre delegação restrita, conforme apresentada no Windows Server 2003, consulte [Transição de protocolo e delegação restrita de Kerberos](https://technet.microsoft.com/library/cc739587(v=ws.10)).

A implementação do Windows Server 2012 R2 e Windows Server 2012 do protocolo Kerberos inclui extensões especificamente para a delegação restrita.  O S4U2Proxy (Service for User to Proxy) permite que um serviço use seu respectivo tíquete de serviço Kerberos para que um usuário obtenha um tíquete de serviço do KDC (Centro de Distribuição de Chaves) para um serviço back-end. Essas extensões permitem que a delegação restrita ser configurado na conta do serviço de back-end, que pode ser em outro domínio. Para obter mais informações sobre essas extensões, consulte [ \[MS-SFU\]: Extensões do protocolo Kerberos: Serviço para usuário e especificação do protocolo de delegação restrita](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) na biblioteca MSDN.

**Aplicativos práticos**

A delegação restrita fornece a capacidade de especificar e impor limites de confiança do aplicativo restringindo o escopo em que os serviços de aplicativo podem atuar em nome de um usuário de administradores de serviço. Os administradores de serviços podem configurar quais contas de serviço front-end podem ser delegadas aos seus respectivos serviços back-end.

Por suporte a delegação restrita entre domínios no Windows Server 2012 R2 e Windows Server 2012, serviços front-end, como o Microsoft Internet Security and Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Web Access (OWA) e o Microsoft SharePoint Server podem ser configurado para usar a delegação restrita para autenticação em servidores em outros domínios. Isso dá suporte a soluções de serviços entre domínios usando uma infraestrutura Kerberos existente. A delegação restrita de Kerberos pode ser gerenciada por administradores de domínio ou administradores de serviços.

## <a name="resource-based-constrained-delegation-across-domains"></a>Delegação restrita entre domínios com base em recursos

A delegação restrita de Kerberos pode ser usada para fornecer delegação restrita quando o serviço front-end e os serviços de recurso não estão no mesmo domínio. Os administradores de serviços podem configurar a nova delegação especificando as contas de domínio dos serviços front-end que podem representar usuários nos objetos de conta dos serviços de recurso.

**Qual é o valor agregado desta alteração?**

Com o suporte à delegação restrita entre domínios, os serviços podem ser configurados para usar a delegação restrita para autenticação em servidores em outros domínios usando delegação não restrita. Isso dá suporte à autenticação para soluções de serviços entre domínios usando uma infraestrutura Kerberos existente sem a necessidade de confiar em serviços front-end para delegar a qualquer outro serviço.

Isso também muda a decisão de se um servidor deve confiar na origem de uma identidade delegada de delegação do administrador de domínio para o proprietário do recurso.

**O que passou a funcionar de maneira diferente?**

Uma alteração no protocolo subjacente permite a delegação restrita entre domínios. A implementação do Windows Server 2012 R2 e Windows Server 2012 do protocolo Kerberos inclui extensões para o serviço para usuário protocolo Proxy (S4U2Proxy). Trata-se de um conjunto de extensões para o protocolo Kerberos que permite que um serviço use seu respectivo tíquete de serviço Kerberos para que um usuário obtenha um tíquete de serviço do KDC (Centro de Distribuição de Chaves) para um serviço back-end.

Para obter informações sobre implementação sobre essas extensões, consulte [ \[MS-SFU\]: Extensões do protocolo Kerberos: Serviço para usuário e especificação do protocolo de delegação restrita](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) no MSDN.

Para obter mais informações sobre a sequência de mensagem básica para a delegação Kerberos com uma concessão de tíquete (TGT tíquete) encaminhado em comparação com o serviço para as extensões de usuário (S4U), consulte a seção [1.3.3 Visão geral do protocolo](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) no [MS-SFU]: Extensões do protocolo Kerberos: Serviço para usuário e especificação do protocolo de delegação restrita.

**Implicações de segurança de delegação restrita baseada em recursos**

Delegação de controle de puts de delegação nas mãos do administrador do proprietário do recurso que está sendo acessado restrita com base em recursos. Isso depende de atributos de serviço de recursos em vez do serviço sendo confiáveis para delegar. Como resultado, a delegação restrita baseada em recursos não é possível usar o bit confiável-para-autenticar-para-delegação que anteriormente controlados a transição de protocolo. O KDC sempre permite que a transição de protocolo ao executar com base no recurso de delegação restrita de como se o bit foi definido.

Porque o KDC não limita a transição de protocolo, dois novos SIDs bem conhecidos foram introduzidas para fornecer esse controle para o administrador de recursos.  Esses SIDs identificar se a transição de protocolo ocorreu e pode ser usada com listas de controle de acesso padrão para conceder ou limitar o acesso conforme necessário.

|SID|Descrição|
|-------|--------|
|AUTHENTICATION_AUTHORITY_ASSERTED_IDENTITY<br />S-1-18-1|Uma SID que significa que a identidade do cliente é declarada por uma autoridade de autenticação com base em uma prova de posse de credenciais de cliente.|
|SERVICE_ASSERTED_IDENTITY<br />S-1-18-2|Uma SID que significa que a identidade do cliente é declarada por um serviço.|

Um serviço de back-end pode usar expressões de ACL padrão para determinar como o usuário foi autenticado.

**Como você configurar a delegação restrita baseada em recursos?**

Para configurar um serviço de recurso para permitir acesso a um serviço front-end em nome de usuários, use cmdlets do Windows PowerShell.

-   Para recuperar uma lista de entidades de segurança, use o **Get-ADComputer**, **Get-ADServiceAccount**, e **Get-ADUser** cmdlets com o **propriedades PrincipalsAllowedToDelegateToAccount** parâmetro.

-   Para configurar o serviço de recursos, use o **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **Set-ADComputer**,  **Set-ADServiceAccount**, e **Set-ADUser** cmdlets com o **PrincipalsAllowedToDelegateToAccount** parâmetro.

## <a name="BKMK_SOFT"></a>Requisitos de software
Delegação restrita baseada em recurso só pode ser configurada em um controlador de domínio que executam o Windows Server 2012 R2 e Windows Server 2012, mas pode ser aplicada em uma floresta de modo misto.

Você deve aplicar o seguinte hotfix para todos os controladores de domínio executando o Windows Server 2012 no usuário domínios de conta no caminho de referência entre os domínios front-end e back-end que executam sistemas operacionais anteriores ao Windows Server:  Falha KDC_ERR_POLICY de delegação restrita baseada em recursos em ambientes que têm controladores de domínio baseados no Windows Server 2008 R2.
