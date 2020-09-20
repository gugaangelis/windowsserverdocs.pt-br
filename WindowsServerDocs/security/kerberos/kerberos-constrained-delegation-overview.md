---
title: Kerberos Constrained Delegation Overview
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 51923b0a-0c1a-47b2-93a0-d36f8e295589
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: cdee2aaecf8710b9801b689b141b16d0dbacc691
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766789"
---
# <a name="kerberos-constrained-delegation-overview"></a>Kerberos Constrained Delegation Overview

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico de visão geral do profissional de ti descreve os novos recursos para a delegação restrita de Kerberos no Windows Server 2012 R2 e no Windows Server 2012.

**Descrição do recurso**

A delegação restrita de Kerberos foi introduzida no Windows Server 2003 para proporcionar uma forma de delegação mais segura para ser usada em serviços. Quando configurada, a delegação restrita restringe os serviços nos quais um determinado servidor pode agir em nome de um usuário. Isso requer privilégios de administrador de domínio para configurar uma conta de domínio para um serviço e restringe a conta a um único domínio. Na empresa de hoje, os serviços front-end não são projetados para serem limitados à integração apenas com serviços em seu domínio.

Em sistemas operacionais mais antigos, nos quais o administrador de domínio configurava o serviço, o administrador do serviço não tinha uma maneira útil de saber quais serviços front-end eram delegados aos serviços de recursos eles possuíam. E qualquer serviço front-end que pudesse ser delegado a um serviço de recurso representava um possível ponto de ataque. Se um servidor que hospedava um serviço front-end fosse comprometido, e tivesse sido configurado para delegar aos serviços de recursos, os serviços de recurso também poderiam ser comprometidos.

No Windows Server 2012 R2 e no Windows Server 2012, a capacidade de configurar a delegação restrita para o serviço foi transferida do administrador de domínio para o administrador de serviços. Dessa forma, o administrador do serviço de back-end pode permitir ou negar os serviços front-end.

Para obter informações detalhadas sobre delegação restrita, conforme apresentada no Windows Server 2003, consulte [Transição de protocolo e delegação restrita de Kerberos](/previous-versions/windows/it-pro/windows-server-2003/cc739587(v=ws.10)).

A implementação do Windows Server 2012 R2 e do Windows Server 2012 do protocolo Kerberos inclui extensões específicas para delegação restrita.  O S4U2Proxy (Service for User to Proxy) permite que um serviço use seu respectivo tíquete de serviço Kerberos para que um usuário obtenha um tíquete de serviço do KDC (Centro de Distribuição de Chaves) para um serviço back-end. Essas extensões permitem que a delegação restrita seja configurada na conta do serviço de back-end, que pode estar em outro domínio. Para obter mais informações sobre essas extensões, consulte [ \[ MS-SFU \] : extensões de protocolo Kerberos: serviço para a especificação de protocolo de delegação restrita e de usuário](/openspecs/windows_protocols/ms-sfu/3bff5864-8135-400e-bdd9-33b552051d94) na biblioteca MSDN.

**Aplicações práticas**

A delegação restrita dá aos administradores de serviço a capacidade de especificar e impor limites de confiança do aplicativo, limitando o escopo em que os serviços de aplicativo podem agir em nome de um usuário. Os administradores de serviços podem configurar quais contas de serviço front-end podem ser delegadas aos seus respectivos serviços back-end.

Ao dar suporte à delegação restrita entre domínios no Windows Server 2012 R2 e no Windows Server 2012, serviços de front-end como o Microsoft ISA (Internet Security and Acceleration) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Acesso via Web (OWA) e Microsoft SharePoint Server podem ser configurados para usar a delegação restrita para autenticação em servidores em outros domínios. Isso dá suporte a soluções de serviços entre domínios usando uma infraestrutura Kerberos existente. A delegação restrita de Kerberos pode ser gerenciada por administradores de domínio ou administradores de serviços.

## <a name="resource-based-constrained-delegation-across-domains"></a>Delegação restrita entre domínios com base em recursos

A delegação restrita de Kerberos pode ser usada para fornecer delegação restrita quando o serviço front-end e os serviços de recurso não estão no mesmo domínio. Os administradores de serviços podem configurar a nova delegação especificando as contas de domínio dos serviços front-end que podem representar usuários nos objetos de conta dos serviços de recurso.

**Qual é o valor agregado desta alteração?**

Com o suporte à delegação restrita entre domínios, os serviços podem ser configurados para usar a delegação restrita para autenticação em servidores em outros domínios usando delegação não restrita. Isso dá suporte à autenticação para soluções de serviços entre domínios usando uma infraestrutura Kerberos existente sem a necessidade de confiar em serviços front-end para delegar a qualquer outro serviço.

Isso também muda a decisão de se um servidor deve confiar na origem de uma identidade delegada do administrador de domínio de delegação para o proprietário do recurso.

**O que passou a funcionar de maneira diferente?**

Uma alteração no protocolo subjacente permite a delegação restrita entre domínios. A implementação do Windows Server 2012 R2 e do Windows Server 2012 do protocolo Kerberos inclui extensões para serviço para o protocolo S4U2Proxy (user to proxy). Trata-se de um conjunto de extensões para o protocolo Kerberos que permite que um serviço use seu respectivo tíquete de serviço Kerberos para que um usuário obtenha um tíquete de serviço do KDC (Centro de Distribuição de Chaves) para um serviço back-end.

Para obter informações de implementação sobre essas extensões, consulte [ \[ MS-SFU \] : extensões de protocolo Kerberos: serviço para especificação de protocolo de delegação restrita e de usuário](/openspecs/windows_protocols/ms-sfu/3bff5864-8135-400e-bdd9-33b552051d94) no msdn.

Para obter mais informações sobre a sequência de mensagem básica para a delegação Kerberos com um TGT (tíquete de concessão de tíquete) encaminhado em comparação com o serviço para as extensões de usuário (S4U), consulte a seção [1.3.3 Visão geral do protocolo](/openspecs/windows_protocols/ms-sfu/1fb9caca-449f-4183-8f7a-1a5fc7e7290a) no [MS-SFU]: Extensões do protocolo Kerberos: Serviço para usuário e especificação do protocolo de delegação restrita.

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

-   Para configurar o serviço de recursos, use os cmdlets **New-ADComputer**, **New-ADServiceAccount**, **New-ADUser**, **set-ADComputer**, **set-ADServiceAccount**e **set-ADUser** com o parâmetro **PrincipalsAllowedToDelegateToAccount** .

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
A delegação restrita baseada em recursos só pode ser configurada em um controlador de domínio que executa o Windows Server 2012 R2 e o Windows Server 2012, mas pode ser aplicada em uma floresta de modo misto.

Você deve aplicar o hotfix a seguir a todos os controladores de domínio que executam o Windows Server 2012 em domínios de conta de usuário no caminho de referência entre os domínios de front-end e back-end que estão executando sistemas operacionais anteriores ao Windows Server: a delegação restrita baseada em recursos KDC_ERR_POLICY falha em ambientes que têm controladores de domínio baseados no Windows Server 2008 R2 ( https://support.microsoft.com/en-gb/help/2665790/resource-based-constrained-delegation-kdc-err-policy-failure-in-enviro) .