---
title: Visão geral da autenticação Kerberos
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 33712dc8502035bd9e47e1d2bdd4583eb8347dec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386311"
---
# <a name="kerberos-authentication-overview"></a>Visão geral da autenticação Kerberos

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Kerberos é um protocolo de autenticação usado para verificar a identidade de um usuário ou host. Este tópico contém informações sobre a autenticação Kerberos no Windows Server 2012 e no Windows 8.

## <a name="BKMK_OVER"></a>Descrição do recurso
Os sistemas operacionais Windows Server implementam o protocolo de autenticação Kerberos versão 5 e extensões para autenticação de chave pública, transporte de dados de autorização e delegação. O cliente de autenticação Kerberos é implementado como um provedor de suporte de segurança \(SSP @ no__t-1 e pode ser acessado por meio da interface do provedor de suporte de segurança \(SSPI @ no__t-3. A autenticação de usuário inicial é integrada com a arquitetura de logon único do Winlogon @ no__t-0on.

O Kerberos centro de distribuição de chaves \(KDC @ no__t-1 é integrado a outros serviços de segurança do Windows Server que são executados no controlador de domínio. O KDC usa o banco de dados de Active Directory Domain Services do domínio como seu banco de dados de conta de segurança. Os Serviços de Domínio do Active Directory são exigidos para implementações Kerberos padrão no domínio ou na floresta.

## <a name="kerb_tr_Kerb_Benefits"></a>Aplicativos práticos
Os benefícios obtidos com o uso do Kerberos para a autenticação do domínio @ no__t-0based são:

-   **Autenticação delegada.**

    Os serviços que são executados em sistemas operacionais Windows podem representar um computador cliente ao acessar recursos em nome do cliente. Em muitos casos, um serviço pode concluir seu trabalho para o cliente ao acessar recursos no computador local. Quando um computador cliente é autenticado para o serviço, o protocolo Kerberos e NTLM fornecem as informações de autorização de que um serviço precisa para representar o computador cliente local. No entanto, alguns aplicativos distribuídos são projetados de forma que um serviço de @ no__t-0end frontal deve usar a identidade do computador cliente quando ele se conecta aos serviços do @ no__t-1Fim em outros computadores. A autenticação Kerberos dá suporte a um mecanismo de delegação que permite que um serviço aja em nome de seu cliente ao se conectar a outros serviços.

-   **Logon único.**

    Usar a autenticação Kerberos em um domínio ou em uma floresta permite que o usuário ou o serviço acesse os recursos permitidos por administradores sem várias solicitações de credenciais. Depois do logon do domínio inicial por meio do Winlogon, o Kerberos gerencia as credenciais em toda a floresta sempre que há tentativa de acesso aos recursos.

-   **Interoperabilidade.**

    A implementação do protocolo Kerberos v5 pela Microsoft baseia-se nas especificações de padrões @ no__t-0track que são recomendadas para o Internet Engineering Task Force \(IETF @ no__t-2. Como resultado, em sistemas operacionais Windows, o protocolo Kerberos estabelece uma base para a interoperabilidade com outras redes em que o protocolo Kerberos é usado para autenticação. Além disso, a Microsoft publica documentação de protocolos do Windows para implementar o protocolo Kerberos. A documentação contém os requisitos técnicos, as limitações, as dependências e o comportamento do protocolo Windows @ no__t-0specific para a implementação do protocolo Kerberos da Microsoft.

-   **Autenticação mais eficiente para servidores.**

    Antes do Kerberos, a autenticação NTLM podia ser usada, que exige que um servidor de aplicativos conecte-se a um controlador de domínio para autenticar cada computador cliente ou serviço. Com o protocolo Kerberos, os tíquetes de sessão renováveis substituem a autenticação Pass @ no__t-0through. Não é necessário que o servidor vá para um controlador de domínio \(unless ele precisa validar um certificado de atributo de privilégio \(PAC @ no__t-2 @ no__t-3. Em vez disso, o servidor pode autenticar o computador cliente examinando as credenciais apresentadas pelo cliente. Computadores cliente podem obter credenciais para um servidor específico uma vez e então reutilizá-las durante uma sessão de logon de rede.

-   **Autenticação mútua.**

    Ao utilizar o protocolo Kerberos, uma parte na extremidade de uma conexão de rede pode verificar se a parte no outra extremidade é a entidade que ela diz ser. O NTLM não permite que os clientes verifiquem a identidade de um servidor ou habilitem um servidor para verificar a identidade de outro. A autenticação NTLM foi projetada para um ambiente de rede em que supõe-se que os servidores sejam genuínos. O protocolo Kerberos não faz essa suposição.

## <a name="see-also"></a>Consulte também
[Visão geral da autenticação do Windows](../windows-authentication/windows-authentication-overview.md)


