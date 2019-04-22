---
title: Visão geral da autenticação do Kerberos
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 646c6309-e865-4be2-b415-44dd125af5c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9f8878fb959c34663b1e1f3858fa7e0b6e7b6105
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824107"
---
# <a name="kerberos-authentication-overview"></a>Visão geral da autenticação do Kerberos

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Kerberos é um protocolo de autenticação usado para verificar a identidade de um usuário ou host. Este tópico contém informações sobre a autenticação Kerberos no Windows Server 2012 e Windows 8.

## <a name="BKMK_OVER"></a>Descrição do recurso
Os sistemas operacionais Windows Server implementam o protocolo de autenticação Kerberos versão 5 e extensões para autenticação de chave pública, transporte de dados de autorização e delegação. O cliente de autenticação Kerberos é implementado como um provedor de suporte de segurança \(SSP\), e ele pode ser acessado por meio da Interface de provedor de suporte de segurança \(SSPI\). Autenticação de usuário inicial é integrada com o logon único do Winlogon\-na arquitetura.

O Kerberos Key Distribution Center \(KDC\) é integrado com outros serviços de segurança do Windows Server que são executados no controlador de domínio. O KDC usa o banco de dados de serviços de domínio do Active Directory do domínio como seu banco de dados de conta de segurança. Os Serviços de Domínio do Active Directory são exigidos para implementações Kerberos padrão no domínio ou na floresta.

## <a name="kerb_tr_Kerb_Benefits"></a>Aplicativos práticos
Os benefícios obtidos usando o Kerberos para o domínio\-são autenticação baseada em:

-   **Autenticação delegada.**

    Serviços que são executados em sistemas de operacionais do Windows podem representar um computador cliente ao acessar recursos em nome do cliente. Em muitos casos, um serviço pode concluir seu trabalho para o cliente ao acessar recursos no computador local. Quando um computador cliente é autenticado para o serviço, o protocolo Kerberos e NTLM fornecem as informações de autorização de que um serviço precisa para representar o computador cliente local. No entanto, alguns aplicativos distribuídos são projetados para que um front\-serviço final deve usar a identidade do computador cliente quando ele se conecta para fazer\-terminar serviços em outros computadores. A autenticação Kerberos dá suporte a um mecanismo de delegação que permite que um serviço aja em nome de seu cliente ao se conectar a outros serviços.

-   **Logon único.**

    Usar a autenticação Kerberos em um domínio ou em uma floresta permite que o usuário ou o serviço acesse os recursos permitidos por administradores sem várias solicitações de credenciais. Depois do logon do domínio inicial por meio do Winlogon, o Kerberos gerencia as credenciais em toda a floresta sempre que há tentativa de acesso aos recursos.

-   **Interoperabilidade.**

    A implementação do protocolo Kerberos V5 pela Microsoft é baseada em padrões\-acompanhar especificações recomendadas para a Internet Engineering Task Force \(IETF\). Como resultado, em sistemas operacionais Windows, o protocolo Kerberos estabelece uma base para a interoperabilidade com outras redes em que o protocolo Kerberos é usado para autenticação. Além disso, a Microsoft publica documentação de protocolos do Windows para implementar o protocolo Kerberos. A documentação contém os requisitos técnicos, limitações, dependências e Windows\-comportamento de protocolo específico para a implementação da Microsoft do protocolo Kerberos.

-   **Autenticação mais eficiente para servidores.**

    Antes do Kerberos, a autenticação NTLM podia ser usada, que exige que um servidor de aplicativos conecte-se a um controlador de domínio para autenticar cada computador cliente ou serviço. Com o protocolo Kerberos, tíquetes de sessão renováveis substituem pass\-por meio da autenticação. O servidor não é necessário para ir para um controlador de domínio \(, a menos que precise validar um certificado de atributo privilégio \(PAC\)\). Em vez disso, o servidor pode autenticar o computador cliente examinando as credenciais apresentadas pelo cliente. Computadores cliente podem obter credenciais para um servidor específico uma vez e então reutilizá-las durante uma sessão de logon de rede.

-   **Autenticação mútua.**

    Ao utilizar o protocolo Kerberos, uma parte na extremidade de uma conexão de rede pode verificar se a parte no outra extremidade é a entidade que ela diz ser. NTLM não permite que os clientes verificar a identidade do servidor ou habilitem um servidor verificar a identidade de outro. A autenticação NTLM foi projetada para um ambiente de rede em que supõe-se que os servidores sejam genuínos. O protocolo Kerberos não faz essa suposição.

## <a name="see-also"></a>Consulte também
[Visão geral da autenticação do Windows](../windows-authentication/windows-authentication-overview.md)


