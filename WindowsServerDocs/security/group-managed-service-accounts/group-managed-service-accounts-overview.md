---
title: Visão geral de contas de serviço gerenciado grupo
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
ms.openlocfilehash: 4912ae273e603b4a3362c4984da710f780c3e8b3
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/12/2018
---
# <a name="group-managed-service-accounts-overview"></a>Visão geral de contas de serviço gerenciado grupo

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI apresenta o grupo de conta de serviço gerenciado descrevendo aplicativos práticos, muda na implementação e requisitos de hardware e software da Microsoft.


## <a name="BKMK_OVER"></a>Descrição dos recursos
Autônomo gerenciado contas de serviço que foram introduzidas no Windows Server 2008 R2 e Windows 7, são contas de domínio gerenciado que fornecem gerenciamento automático de senha e gerenciamento simplificado de SPN, incluindo delegação de gerenciamento para outros administradores.

O grupo de conta de serviço gerenciado fornece a mesma funcionalidade dentro do domínio, mas também se estende essa funcionalidade ao longo de vários servidores. Ao se conectar a um serviço hospedado em um farm de servidores, como balanceamento de carga de rede, os protocolos de autenticação dando suporte à autenticação mútua exigem que todas as instâncias dos serviços usem a mesma entidade de segurança. Quando a conta de serviço gerenciado do grupo são usados como entidades de serviço, o sistema operacional Windows gerencia a senha da conta em vez de depender do administrador para gerenciar a senha.

O serviço de distribuição de chaves do Microsoft \(kdssvc.dll\) fornece o mecanismo para obter com segurança a chave mais recente ou uma chave específica com um identificador da chave para uma conta do Active Directory. O serviço de distribuição de chaves compartilha um segredo que é usado para criar chaves para a conta. Essas chaves são alteradas periodicamente. Para um grupo de conta de serviço gerenciado do controlador de domínio calcula a senha na chave fornecida pelos serviços de distribuição de chave, além para outros atributos do grupo de conta de serviço gerenciado.  Os hosts de membro poderá obter os valores atuais e anteriores senha entrando em contato com um controlador de domínio.

## <a name="BKMK_APP"></a>Aplicativos práticos
Contas de serviço gerenciado do grupo fornecem uma solução de identidade única para os serviços executados em um farm de servidores ou em sistemas atrás de balanceamento de carga de rede. Ao fornecer uma solução MSA do grupo, serviços podem ser configurados para o novo grupo MSA principal e o gerenciamento de senhas é manipulado pelo Windows.

Usar um grupo de conta de serviço de gerenciada, serviços ou administradores de serviços não precisa gerenciar a sincronização de senhas entre as instâncias de serviço. O grupo de conta de serviço gerenciado dá suporte a hosts que são mantidos offline de um período de tempo estendido e gerenciamento dos hosts de membro para todas as instâncias de um serviço. Isso significa que você pode implantar um farm de servidores que dá suporte a uma identidade única aos quais computadores clientes existentes podem autenticar sem saber a instância do serviço ao qual ele estão se conectando.

Clusters de failover não dão suporte a gMSAs. No entanto, os serviços que são executados sobre o serviço de Cluster podem usar um gMSA ou um sMSA se eles são um serviço do Windows, um pool de aplicativo, uma tarefa agendada ou suporte nativo para gMSA ou sMSA.

## <a name="BKMK_SOFT"></a>Requisitos de software

Uma arquitetura 64\ bits é necessária para executar os comandos do Windows PowerShell que são usados para administrar as contas de serviço gerenciado do grupo.

Uma conta de serviço gerenciado depende de tipos de criptografia Kerberos com suporte. Quando um computador cliente é autenticado para um servidor usando Kerberos o controlador de domínio cria um tíquete de serviço Kerberos protegidos com criptografia suporta DC e servidor. O controlador de domínio usa o atributo de msDS\ SupportedEncryptionTypes da conta para determinar o que dá suporte a criptografia do servidor e, se não houver nenhum atributo, ele presume que o computador cliente não dá suporte a tipos de criptografia mais fortes. Se o host estiver configurado para não suporte RC4, autenticação sempre irá falhar. Por esse motivo, AES deve sempre ser configurado explicitamente para MSAs.

> [!NOTE]
> A partir do Windows Server 2008 R2, DES é desabilitada por padrão. Para saber mais sobre tipos de criptografia com suporte, consulte [alterações na autenticação Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

Contas de serviço gerenciado do grupo não são aplicáveis aos sistemas de operacionais Windows anteriores ao Windows Server 2012.

## <a name="server-manager-information"></a>Informações do Gerenciador do servidor
Não há nenhuma etapa de configuração necessárias para implementar o MSA e agrupar MSA usando o Gerenciador do servidor ou o cmdlet Install\-WindowsFeature.

## <a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir fornece links para recursos adicionais relacionados a contas de serviço gerenciado e contas de serviço gerenciado do grupo.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[O que há de novo para contas de serviço gerenciado](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentação do Windows 7 e Windows Server 2008 R2 de contas de serviço gerenciado](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Guia passo a Step\-by\ de contas de serviço](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planejamento**|Ainda não disponível|
|**Implantação**|Ainda não disponível|
|**Operações**|[Gerenciado contas de serviço no Active Directory](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Solução de problemas**|Ainda não disponível|
|**Avaliação**|[Introdução ao grupo de contas de serviço gerenciado](getting-started-with-group-managed-service-accounts.md)|
|**Ferramentas e configurações**|[Gerenciado contas de serviço nos serviços de domínio do Active Directory](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Recursos da comunidade**|[Noções básicas sobre contas de serviço gerenciado:, Implementação de práticas recomendadas e solução de problemas](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Tecnologias relacionadas**|[Visão geral dos serviços de domínio do Active Directory](active-directory-domain-services-overview.md)|


