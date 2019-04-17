---
title: "Visão geral de delegação restrita de Kerberos"
description: "Segurança do Windows Server"
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
ms.openlocfilehash: e90aa5e395fa43a3f94cccb13444fd6991ac1074
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-constrained-delegation-overview"></a>Visão geral de delegação restrita de Kerberos

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico de visão geral para o profissional de TI descreve os novos recursos para delegação restringida de Kerberos no Windows Server 2012 R2 e Windows Server 2012.

## <a name="feature-description"></a>Descrição dos recursos
Delegação restringida de Kerberos foi introduzida no Windows Server 2003 para fornecer uma forma mais segura de delegação que poderia ser usada pelos serviços. Quando ele é configurado, a delegação restrita restringe os serviços aos quais o servidor especificado pode atuar em nome de um usuário. Isso requer privilégios de administrador do domínio para configurar uma conta de domínio para um serviço e é restringe a conta em um único domínio. Na empresa de hoje, serviços front-end não são projetados para ser limitado a integração com serviços em seu domínio.

Em sistemas operacionais anteriores em que o administrador de domínio configurado o serviço, o administrador de serviço não tinha úteis como saber quais serviços front-end recebidos para os serviços de recursos que eles pertencem. E qualquer serviço front-end que pode delegar a um serviço de recurso representado um ponto de ataque potencial. Se um servidor que um serviço front-end hospedado foi comprometido, e ele foi configurado para delegar aos serviços de recurso, os serviços de recurso também poderiam ser comprometidos.

No Windows Server 2012 R2 e Windows Server 2012, capacidade de configurar delegação restrita para o serviço foi transferida do administrador do domínio para o administrador de serviço. Dessa forma, o administrador de serviço de back-end pode permitir ou negar serviços front-end.

Para obter informações detalhadas sobre a delegação restrita conforme apresentado no Windows Server 2003, consulte [transição de protocolo Kerberos e delegação restrita](https://technet.microsoft.com/library/cc739587(v=ws.10)).

A implementação do Windows Server 2012 R2 e Windows Server 2012 do protocolo Kerberos inclui extensões especificamente para delegação restrita.  Serviço de usuário Proxy (S4U2Proxy) permite que um serviço para usar o tíquete de serviço Kerberos para um usuário para obter um tíquete de serviço do Centro de distribuição de chaves (KDC) para um serviço de back-end. Essas extensões permitem delegação restrita para ser configurado na conta do serviço de back-end, que pode ser em outro domínio. Para obter mais informações sobre essas extensões, consulte [\[MS-SFU\]: extensões do protocolo Kerberos: serviço de usuário e a especificação de protocolo de delegação restrita](https://msdn.microsoft.com/library/cc246071(PROT.13).aspx) na biblioteca MSDN.

## <a name="practical-applications"></a>Aplicativos práticos
Delegação restrita oferece aos administradores de serviço a capacidade de especificar e impor limites de confiança do aplicativo, limitando o escopo em que os serviços de aplicativo podem atuar em nome do usuário. Os administradores de serviço podem definir quais contas de serviço front-end podem delegar para seus serviços de back-end.

Dando suporte a delegação restrita entre domínios no Windows Server 2012 R2 e Windows Server 2012, serviços front-end, como o Microsoft Internet Security and Acceleration (ISA) Server, Microsoft Forefront Threat Management Gateway, Microsoft Exchange Outlook Web Access (OWA) e Microsoft SharePoint Server podem ser configurados para usar a delegação restrita para se autenticar em servidores em outros domínios. Isso dá suporte entre domínios soluções de serviço usando uma infraestrutura existente de Kerberos. Delegação restringida de Kerberos pode ser gerenciada por administradores de domínio ou administradores de serviços.

## <a name="new-and-changed-functionality"></a>Funcionalidades novas e alteradas
**Delegação restrita com base em recursos entre domínios**

Delegação restringida de Kerberos pode ser usada para fornecer delegação restrita quando o serviço front-end e os serviços de recursos não estão no mesmo domínio. Administradores de serviços são capazes de configurar a delegação de novo, especificando as contas de domínio dos serviços front-end que podem representar os usuários sobre os objetos de conta dos serviços de recurso.

**O valor adicione essa alteração?**

Dando suporte a delegação restrita entre domínios, serviços podem ser configurados para usar a delegação restrita para se autenticar em servidores em outros domínios em vez de usar delegação irrestrita. Isso fornece suporte para autenticação em soluções de serviços de domínio usando uma infraestrutura de Kerberos existente sem a necessidade de confiar em serviços front-end de delegate para qualquer serviço.

**O que funciona de modo diferente?**

Uma alteração no protocolo subjacente permite que a delegação restrita entre domínios. A implementação do Windows Server 2012 R2 e Windows Server 2012 do protocolo Kerberos inclui extensões ao serviço de usuário para o protocolo de Proxy (S4U2Proxy). Este é um conjunto de extensões para o protocolo Kerberos que permite que um serviço usar o tíquete de serviço Kerberos para um usuário para obter um tíquete de serviço do Centro de distribuição de chaves (KDC) para um serviço de back-end.

Para obter informações de implementação sobre essas extensões, consulte [\[MS-SFU\]: extensões do protocolo Kerberos: serviço de usuário e a especificação de protocolo de delegação restrita](https://msdn.microsoft.com/library/cc246071(PROT.10).aspx) no MSDN.

Para saber mais sobre a sequência de mensagem básica para delegação Kerberos com um encaminhadas tíquete de concessão (TGT) comparação com o serviço para as extensões do usuário (S4U), consulte a seção [1.3.3 Visão geral do protocolo](https://msdn.microsoft.com/library/cc246080(v=prot.10).aspx) do [MS-SFU]: extensões do protocolo Kerberos: serviço de usuário e a especificação de protocolo de delegação restrita.

Para configurar um serviço de recurso para permitir o acesso um front-end do serviço em nome dos usuários, use cmdlets do Windows PowerShell.

-   Para recuperar uma lista dos objetos, use o **Get-ADComputer**, **Get-ADServiceAccount**, e **Get-ADUser** cmdlets com o **propriedades PrincipalsAllowedToDelegateToAccount** parâmetro.

-   Para configurar o serviço de recursos, use o **nova ADComputer**, **nova ADServiceAccount**, **New-ADUser**, **conjunto ADComputer**, **ADServiceAccount conjunto**, e **Set-ADUser** cmdlets com o **PrincipalsAllowedToDelegateToAccount** parâmetro.

## <a name="BKMK_SOFT"></a>Requisitos de software
Delegação restrita com base em recursos só pode ser configurada em um controlador de domínio que executam o Windows Server 2012 R2 e Windows Server 2012, mas pode ser aplicada em uma floresta de modo misto.

Você deverá aplicar o seguinte hotfix para todos os controladores de domínio executando o Windows Server 2012 em usuário domínios de conta no caminho de referência entre os domínios front-end e back-end que executam sistemas operacionais anteriores ao Windows Server: baseado em recursos restritos delegação falha KDC_ERR_POLICY em ambientes com controladores de domínio baseados no Windows Server 2008 R2.
