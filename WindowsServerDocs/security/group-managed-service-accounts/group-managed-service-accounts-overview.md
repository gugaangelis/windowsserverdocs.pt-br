---
title: Group Managed Service Accounts Overview
description: Segurança do Windows Server
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 09405b940e9fd862372fe80c4a5194caa205e5ea
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991498"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico para o profissional de ti apresenta a conta de serviço gerenciado de grupo descrevendo aplicativos práticos, alterações na implementação da Microsoft e requisitos de hardware e software.


## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descrição do recurso
Uma conta de serviço gerenciado autônoma (sMSA) é uma conta de domínio gerenciado que fornece gerenciamento automático de senhas, gerenciamento de SPN (nome da entidade de serviço) simplificado e a capacidade de delegar o gerenciamento a outros administradores. Esse tipo de MSA (conta de serviço gerenciado) foi introduzido no Windows Server 2008 R2 e no Windows 7.

A conta de serviço gerenciado de grupo (gMSA) fornece a mesma funcionalidade dentro do domínio, mas também estende essa funcionalidade em vários servidores. Ao se conectar a um serviço hospedado em um farm de servidores, como a solução de balanceamento de carga de rede, os protocolos de autenticação que dão suporte à autenticação mútua exigem que todas as instâncias dos serviços usem a mesma entidade de segurança. Quando um gMSA é usado como entidades de serviço, o sistema operacional Windows gerencia a senha da conta em vez de depender do administrador para gerenciar a senha.

O serviço de distribuição de chaves da Microsoft \(kdssvc.dll\) fornece o mecanismo para obter com segurança a chave mais recente ou uma chave específica com um identificador de chave para uma conta de Active Directory. O Serviço de Distribuição de Chaves compartilha um segredo que é usado para criar chaves para a conta. Essas chaves são alteradas periodicamente. Para um gMSA, o controlador de domínio computa a senha na chave fornecida pelos serviços de distribuição de chaves, além de outros atributos do gMSA.  Os hosts membros podem obter os valores de senha atuais e anteriores contatando um controlador de domínio.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas
o gMSAs fornece uma solução de identidade única para serviços em execução em um farm de servidores ou em sistemas por trás de Load Balancer de rede. Ao fornecer uma solução gMSA, os serviços podem ser configurados para a nova entidade gMSA e o gerenciamento de senhas é tratado pelo Windows.

Usando um gMSA, os serviços ou administradores de serviço não precisam gerenciar a sincronização de senha entre instâncias de serviço. O gMSA dá suporte a hosts que são mantidos offline por um período de tempo estendido e o gerenciamento de hosts membro para todas as instâncias de um serviço. Isso significa que você pode implantar um farm de servidores que fornece suporte para uma única identidade na qual os computadores clientes atuais podem se autenticar sem saberem a qual instância do serviço eles estão se conectando.

Os clusters de failover não dão suporte a gMSAs. No entanto, os serviços que são executados sobre o Serviço de cluster poderão usar uma gMSA ou uma sMSA, se forem um serviço Windows, um pool de aplicativos, uma tarefa agendada ou oferecerem suporte nativo a gMSA ou sMSA.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software

Uma arquitetura de 64 \- bits é necessária para executar os comandos do Windows PowerShell que são usados para administrar o gMSAs.

Uma conta de serviços gerenciados depende de tipos de criptografia Kerberos com suporte. Quando um computador cliente se autentica em um servidor usando Kerberos, o controlador de domínio cria um ticket de serviço Kerberos protegido com uma criptografia que é compatível tanto nesse controlador quanto no servidor. O DC usa o atributo msDS SupportedEncryptionTypes da conta \- para determinar a criptografia com a qual o servidor dá suporte e, se não houver nenhum atributo, ele assume que o computador cliente não oferece suporte a tipos de criptografia mais fortes. Se o host estiver configurado para não oferecer suporte a RC4, a autenticação sempre falhará. Por esse motivo, o AES sempre deve ser configurado explicitamente para contas de serviços gerenciados.

> [!NOTE]
> A partir do Windows Server 2008 R2, o DES fica desabilitado por padrão. Para obter mais informações sobre os tipos de criptografia com suporte, consulte [Alterações da autenticação do Kerberos](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560670(v=ws.10)).

gMSAs não são aplicáveis a sistemas operacionais Windows anteriores ao Windows Server 2012.

## <a name="server-manager-information"></a>Informações sobre o Gerenciador do Servidor
Não há nenhuma etapa de configuração necessária para implementar o MSA e o gMSA usando Gerenciador do Servidor ou o \- cmdlet Install WindowsFeature.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir fornece links para recursos adicionais relacionados às Contas de Serviço Gerenciado e Contas de Serviço Gerenciado de grupo.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[What's New for Managed Service Accounts](what-s-new-for-managed-service-accounts.md)<p>[Documentação de Contas de Serviço Gerenciado para Windows 7 e Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff641731(v=ws.10))<p>[Guia passo a passo das contas \- de serviço \-](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd548356(v=ws.10))|
|**Planejamento**|Ainda não está disponível|
|**Implantação**|Ainda não está disponível|
|**Operações**|[Contas de Serviço Gerenciado no Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378925(v=ws.10))|
|**Solução de problemas**|Ainda não está disponível|
|**Evaluation**|[Introdução com contas de serviço gerenciado de grupo](getting-started-with-group-managed-service-accounts.md)|
|**Ferramentas e configurações**|[Contas de Serviço Gerenciado nos Serviços de Domínio Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd378925(v=ws.10))|
|**Recursos da comunidade**|[Contas de Serviços Gerenciados: compreendendo, implementando, práticas recomendadas e solução de problemas](/archive/blogs/askds/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting)|
|**Tecnologias relacionadas**|[Visão geral do Active Directory Domain Services](active-directory-domain-services-overview.md)|