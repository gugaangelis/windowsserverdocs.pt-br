---
title: Group Managed Service Accounts Overview
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4e7f46739dd8def6ffc34c6cc50210c0e6999c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403751"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico para o profissional de ti apresenta a conta de serviço gerenciado de grupo descrevendo aplicativos práticos, alterações na implementação da Microsoft e requisitos de hardware e software.


## <a name="BKMK_OVER"></a>Descrição do recurso
Uma conta de serviço gerenciado autônoma (sMSA) é uma conta de domínio gerenciado que fornece gerenciamento automático de senhas, gerenciamento de SPN (nome da entidade de serviço) simplificado e a capacidade de delegar o gerenciamento a outros administradores. Esse tipo de MSA (conta de serviço gerenciado) foi introduzido no Windows Server 2008 R2 e no Windows 7.

A conta de serviço gerenciado de grupo (gMSA) fornece a mesma funcionalidade dentro do domínio, mas também estende essa funcionalidade em vários servidores. Ao se conectar a um serviço hospedado em um farm de servidores, como a solução de balanceamento de carga de rede, os protocolos de autenticação que dão suporte à autenticação mútua exigem que todas as instâncias dos serviços usem a mesma entidade de segurança. Quando um gMSA é usado como entidades de serviço, o sistema operacional Windows gerencia a senha da conta em vez de depender do administrador para gerenciar a senha.

O serviço de distribuição de chaves da Microsoft @no__t -0kdssvc. dll @ no__t-1 fornece o mecanismo para obter com segurança a chave mais recente ou uma chave específica com um identificador de chave para uma conta de Active Directory. O Serviço de Distribuição de Chaves compartilha um segredo que é usado para criar chaves para a conta. Essas chaves são alteradas periodicamente. Para um gMSA, o controlador de domínio computa a senha na chave fornecida pelos serviços de distribuição de chaves, além de outros atributos do gMSA.  Os hosts membros podem obter os valores de senha atuais e anteriores contatando um controlador de domínio.

## <a name="BKMK_APP"></a>Aplicativos práticos
o gMSAs fornece uma solução de identidade única para serviços em execução em um farm de servidores ou em sistemas por trás de Load Balancer de rede. Ao fornecer uma solução gMSA, os serviços podem ser configurados para a nova entidade gMSA e o gerenciamento de senhas é tratado pelo Windows.

Usando um gMSA, os serviços ou administradores de serviço não precisam gerenciar a sincronização de senha entre instâncias de serviço. O gMSA dá suporte a hosts que são mantidos offline por um período de tempo estendido e o gerenciamento de hosts membro para todas as instâncias de um serviço. Isso significa que você pode implantar um farm de servidores que fornece suporte para uma única identidade na qual os computadores clientes atuais podem se autenticar sem saberem a qual instância do serviço eles estão se conectando.

Os clusters de failover não dão suporte a gMSAs. No entanto, os serviços que são executados sobre o Serviço de cluster poderão usar uma gMSA ou uma sMSA, se forem um serviço Windows, um pool de aplicativos, uma tarefa agendada ou oferecerem suporte nativo a gMSA ou sMSA.

## <a name="BKMK_SOFT"></a>Requisitos de software

Uma arquitetura de 64 @ no__t-0bit é necessária para executar os comandos do Windows PowerShell que são usados para administrar o gMSAs.

Uma conta de serviços gerenciados depende de tipos de criptografia Kerberos com suporte. Quando um computador cliente se autentica em um servidor usando Kerberos, o controlador de domínio cria um ticket de serviço Kerberos protegido com uma criptografia que é compatível tanto nesse controlador quanto no servidor. O DC usa o atributo msDS @ no__t-0SupportedEncryptionTypes da conta para determinar a criptografia com a qual o servidor dá suporte e, se não houver nenhum atributo, ele assume que o computador cliente não oferece suporte a tipos de criptografia mais fortes. Se o host estiver configurado para não oferecer suporte a RC4, a autenticação sempre falhará. Por esse motivo, o AES sempre deve ser configurado explicitamente para contas de serviços gerenciados.

> [!NOTE]
> A partir do Windows Server 2008 R2, o DES fica desabilitado por padrão. Para obter mais informações sobre os tipos de criptografia com suporte, consulte [Alterações da autenticação do Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

gMSAs não são aplicáveis a sistemas operacionais Windows anteriores ao Windows Server 2012.

## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor
Não há nenhuma etapa de configuração necessária para implementar o MSA e o gMSA usando Gerenciador do Servidor ou o cmdlet Install @ no__t-0WindowsFeature.

## <a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir fornece links para recursos adicionais relacionados às Contas de Serviço Gerenciado e Contas de Serviço Gerenciado de grupo.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[O que há de novo para contas de serviço gerenciado](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentação das contas de serviço gerenciadas para Windows 7 e Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Etapa das contas de serviço @ no__t-1by @ no__t-guia de 2Step](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planejamento**|Não disponível ainda|
|**Implantação**|Não disponível ainda|
|**Operações**|[Contas de serviço gerenciado no Active Directory](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Solução de problemas**|Não disponível ainda|
|**Período**|[Introdução a contas de serviços gerenciados em grupo](getting-started-with-group-managed-service-accounts.md)|
|**Ferramentas e configurações**|[Contas de serviço gerenciado no Active Directory Domain Services](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Recursos da comunidade**|Contas de serviço [Managed: Noções básicas, implementação, práticas recomendadas e solução de problemas @ no__t-0|
|**Tecnologias relacionadas**|[Visão geral do Active Directory Domain Services](active-directory-domain-services-overview.md)|


