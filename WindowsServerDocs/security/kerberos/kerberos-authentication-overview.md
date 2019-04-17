---
title: "Visão geral de autenticação Kerberos"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="kerberos-authentication-overview"></a>Visão geral de autenticação Kerberos

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Kerberos é um protocolo de autenticação que é usado para verificar a identidade de um usuário ou host. Este tópico contém informações sobre autenticação Kerberos no Windows Server 2012 e Windows 8.

## <a name="BKMK_OVER"></a>Descrição dos recursos
Os sistemas operacionais Windows Server implemente o protocolo de autenticação Kerberos versão 5 e extensões para autenticação de chave pública, transportar dados de autorização e delegação. O cliente de autenticação Kerberos é implementado como um provedor de suporte de segurança \(SSP\) e elas podem ser acessados por meio de \(SSPI\) a Interface do provedor de suporte de segurança. Autenticação de usuário inicial é integrada com a único Winlogon sign\ na arquitetura.

O Centro de distribuição de chaves Kerberos \(KDC\) é integrado com outros serviços de segurança do Windows Server que são executados no controlador de domínio. O KDC usa o banco de dados de serviços de domínio do Active Directory do domínio como seu banco de dados de conta de segurança. Serviços de domínio do Active Directory é necessário para implementações de Kerberos padrão dentro do domínio ou floresta.

## <a name="kerb_tr_Kerb_Benefits"></a>Aplicativos práticos
Os benefícios obtidos usando Kerberos para autenticação baseada em domain\ são:

-   **Autenticação delegada.**

    Serviços que são executados em sistemas operacionais Windows podem representar um computador cliente ao acessar recursos em nome do cliente. Em muitos casos, um serviço possa concluir seu trabalho para o cliente acessando recursos no computador local. Quando um computador cliente autentica o serviço, o protocolo Kerberos e NTLM fornecer as informações de autorização que precisa de um serviço representar o computador cliente localmente. No entanto, alguns aplicativos distribuídos são projetados para que um serviço front\-end deve usar a identidade do computador cliente quando ele se conecta aos serviços back\-end em outros computadores. Autenticação Kerberos dá suporte a um mecanismo de delegação que permite que um serviço para agir em nome de seu cliente ao se conectar a outros serviços.

-   **Logon único.**

    Usando Kerberos autenticação em um domínio ou em uma floresta permite que o usuário ou o acesso ao serviço de recursos permitidos por administradores sem várias solicitações de credenciais. Depois de domínio inicial de logon por meio do Winlogon, Kerberos gerencia as credenciais em toda a floresta sempre que o acesso aos recursos é tentado.

-   **Interoperabilidade.**

    A implementação do protocolo Kerberos V5 pela Microsoft baseia-se em especificações de faixa de standards\ que são recomendadas para o \(IETF\) Internet Engineering Task Force. Como resultado, em sistemas operacionais Windows, o protocolo Kerberos estabelece uma base para interoperabilidade com outras redes em que o protocolo Kerberos é usado para autenticação. Além disso, a Microsoft publica documentação de protocolos do Windows para implementar o protocolo Kerberos. A documentação contém os requisitos técnicos, limitações, as dependências e comportamento de protocolo do Windows \ específico para a implementação da Microsoft do protocolo Kerberos.

-   **Autenticação mais eficiente para servidores.**

    Antes de Kerberos, a autenticação NTLM poderia ser usada, que requer um servidor de aplicativos para se conectar a um controlador de domínio para autenticar a cada computador cliente ou serviço. Com o protocolo Kerberos, tíquetes de sessão renováveis substituem pass\-por meio de autenticação. O servidor não é necessário para ir para um controlador de domínio \ (a menos que ele precisa validar um certificado de atributo de privilégio \(PAC\)\). Em vez disso, o servidor pode autenticar o computador cliente examinando credenciais apresentadas pelo cliente. Computadores cliente podem obter credenciais para um determinado servidor uma vez e, em seguida, reutilizar essas credenciais durante uma sessão de logon de rede.

-   **Autenticação mútua.**

    Usando o protocolo Kerberos, das partes de uma conexão de rede podem verificar a identidade do outro lado é a entidade que diz ser. NTLM não permitir que os clientes verificar a identidade do servidor ou permitir que um servidor para verificar a identidade de outro. A autenticação NTLM foi projetada para um ambiente de rede em que os servidores foram presumidos como sendo original. O protocolo Kerberos não faz essa suposição.

## <a name="see-also"></a>Consulte também
[Visão geral de autenticação do Windows](../windows-authentication/windows-authentication-overview.md)


