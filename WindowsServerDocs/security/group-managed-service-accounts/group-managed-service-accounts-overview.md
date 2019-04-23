---
title: Group Managed Service Accounts Overview
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 24e3e3c15544de2f3bed4a7ef177b659e8095385
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836967"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico para profissionais de TI apresenta a conta de serviço gerenciado de grupo, descrevendo aplicações práticas, as alterações na implementação e requisitos de hardware e software da Microsoft.


## <a name="BKMK_OVER"></a>Descrição do recurso
Uma conta de serviço gerenciado (sMSA) independente é uma conta de domínio gerenciado que fornece gerenciamento automático de senha, gerenciamento de nome principal (SPN) do serviço simplificado e a capacidade de delegar a outros administradores de gerenciamento. Esse tipo de conta de serviço gerenciado (MSA) foi introduzido no Windows Server 2008 R2 e Windows 7.

O grupo de conta de serviço gerenciado (gMSA) fornece a mesma funcionalidade dentro do domínio, mas também estende essa funcionalidade por vários servidores. Ao se conectar a um serviço hospedado em um farm de servidores, como solução de balanceamento de carga de rede, os protocolos de autenticação que dão suporte a autenticação mútua exigem que todas as instâncias dos serviços usam a mesma entidade. Quando uma gMSA é usada como entidades de serviço, o sistema operacional Windows gerencia a senha da conta em vez de depender do administrador para gerenciar a senha.

O serviço de distribuição de chaves do Microsoft \(kdssvc\) fornece o mecanismo para obter a chave mais recente ou uma chave específica com um identificador de chave para uma conta do Active Directory de forma segura. O Serviço de Distribuição de Chaves compartilha um segredo que é usado para criar chaves para a conta. Essas chaves são alteradas periodicamente. Para uma gMSA o controlador de domínio calcula a senha na chave fornecida pelos serviços de distribuição de chaves, além de outros atributos da gMSA.  Hosts membros podem obter os valores de senha atuais e precedentes entrando em um controlador de domínio.

## <a name="BKMK_APP"></a>Aplicativos práticos
gMSAs fornecem uma solução de identidade única para serviços em execução em um farm de servidores ou em sistemas por trás do balanceador de carga de rede. Ao fornecer uma solução de gMSA, serviços podem ser configurados para a gMSA nova entidade de segurança e o gerenciamento de senhas é tratado pelo Windows.

Usar uma gMSA, serviços ou administradores de serviço não precisa gerenciar a sincronização de senhas entre instâncias de serviço. A gMSA dá suporte a hosts que são mantidos offline por um longo período e o gerenciamento de hosts membros para todas as instâncias de um serviço. Isso significa que você pode implantar um farm de servidores que fornece suporte para uma única identidade na qual os computadores clientes atuais podem se autenticar sem saberem a qual instância do serviço eles estão se conectando.

Os clusters de failover não dão suporte a gMSAs. No entanto, os serviços que são executados sobre o Serviço de cluster poderão usar uma gMSA ou uma sMSA, se forem um serviço Windows, um pool de aplicativos, uma tarefa agendada ou oferecerem suporte nativo a gMSA ou sMSA.

## <a name="BKMK_SOFT"></a>Requisitos de software

Um 64\-arquitetura bits é necessário para executar os comandos do Windows PowerShell que são usados para administrar a gMSAs.

Uma conta de serviços gerenciados depende de tipos de criptografia Kerberos com suporte. Quando um computador cliente se autentica em um servidor usando Kerberos, o controlador de domínio cria um ticket de serviço Kerberos protegido com uma criptografia que é compatível tanto nesse controlador quanto no servidor. O controlador de domínio usa msDS da conta\-SupportedEncryptionTypes atributo para determinar o que dá suporte à criptografia do servidor e, se não houver nenhum atributo, ele pressupõe que o computador cliente não oferece suporte a tipos de criptografia mais fortes. Se o host está configurado para não dar suporte ao RC4, autenticação sempre irá falhar. Por esse motivo, o AES sempre deve ser configurado explicitamente para contas de serviços gerenciados.

> [!NOTE]
> A partir do Windows Server 2008 R2, o DES fica desabilitado por padrão. Para obter mais informações sobre os tipos de criptografia com suporte, consulte [Alterações da autenticação do Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

gMSAs não são aplicáveis a sistemas de operacionais Windows anteriores ao Windows Server 2012.

## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor
Não há nenhuma etapa de configuração necessárias para implementar MSA e gMSA usando o Gerenciador do servidor ou a instalação\-WindowsFeature cmdlet.

## <a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir fornece links para recursos adicionais relacionados às Contas de Serviço Gerenciado e Contas de Serviço Gerenciado de grupo.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[Quais são as novidades para contas de serviço gerenciado](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentação do Windows 7 e Windows Server 2008 R2 de contas de serviço gerenciado](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Etapa de contas de serviço\-por\-guia passo](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planejamento**|Não disponível ainda|
|**Implantação**|Não disponível ainda|
|**Operações**|[Contas de serviço no Active Directory gerenciado](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Solução de problemas**|Não disponível ainda|
|**Avaliação**|[Introdução ao grupo de contas de serviço gerenciado](getting-started-with-group-managed-service-accounts.md)|
|**Ferramentas e configurações**|[Contas de serviço nos serviços de domínio do Active Directory gerenciado](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Recursos da comunidade**|[Contas de serviço gerenciado: Compreendendo, Implementando, práticas recomendadas e solução de problemas](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Tecnologias relacionadas**|[Visão geral dos serviços de domínio do Active Directory](active-directory-domain-services-overview.md)|


