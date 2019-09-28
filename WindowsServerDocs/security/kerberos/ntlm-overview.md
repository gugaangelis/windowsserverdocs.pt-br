---
title: Visão Geral do NTLM
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: b8dec2877646fd2bfe00da9d5c9047e8edfd6f1d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386263"
---
# <a name="ntlm-overview"></a>Visão Geral do NTLM

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico para o profissional de ti descreve o NTLM, quaisquer alterações na funcionalidade e fornece links para recursos técnicos para autenticação do Windows e NTLM para Windows Server 2012 e versões anteriores.

## <a name="BKMK_OVER"></a>Descrição do recurso
A autenticação NTLM é uma família de protocolos de autenticação que é englobada no Msv1\_0.dll do Windows. Os protocolos de autenticação NTLM incluem LAN Manager versão 1 e 2 e NTLM versão 1 e 2. Os protocolos de autenticação NTLM autenticam usuários e computadores com base em um mecanismo de desafio @ no__t-0response que comprova para um servidor ou controlador de domínio que um usuário conhece a senha associada a uma conta. Quando o protocolo NTLM é usado, um servidor de recursos deve tomar uma das seguintes ações para verificar a identidade de um computador ou usuário sempre que um novo token de acesso for necessário:

-   Entra em contato com um serviço de autenticação de domínio no controlador de domínio para o domínio da conta do computador ou do usuário, se a conta for uma conta de domínio.

-   Procura a conta do computador ou do usuário no banco de dados de contas locais, se a conta for uma conta local.

## <a name="BKMK_APP"></a>Aplicativos atuais
Ainda há suporte para a autenticação NTLM e ela deve ser usada para autenticação do Windows com sistemas configurados como um membro de um grupo de trabalho. A autenticação NTLM também é usada para autenticação de logon local em controladores que não são @ no__t-0domain. A autenticação Kerberos versão 5 é o método de autenticação preferencial para ambientes Active Directory, mas um aplicativo que não é @ no__t-0Microsoft ou Microsoft ainda pode usar NTLM.

Reduzir o uso do protocolo NTLM em um ambiente de TI exige o conhecimento dos requisitos do aplicativo implantado em NTLM, bem como das estratégias e das etapas necessárias para configurar ambientes de computação para usar outros protocolos. Novas ferramentas e configurações foram adicionadas para ajudá-lo a descobrir como o NTLM é usado para restringir de maneira seletiva o tráfego NTLM. Para obter informações sobre como analisar e restringir o uso do NTLM em seus ambientes, consulte [Introducing the Restriction of NTLM Authentication (Apresentando a restrição da autenticação NTLM)](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para acessar o guia de uso do NTLM para auditoria e restrição.

## <a name="BKMK_NEW"></a>Funcionalidade nova e alterada
Não há nenhuma alteração na funcionalidade para NTLM para o Windows Server 2012.

## <a name="BKMK_DEP"></a>Funcionalidade removida ou preterida
Não há nenhuma funcionalidade removida ou preterida para NTLM para o Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Informações de Gerenciador do Servidor
O NTLM não pode ser configurado a partir do Gerenciador do Servidor. Você pode usar as configurações de Política de Segurança ou Políticas de Grupo para gerenciar o uso da autenticação NTLM entre sistemas de computadores. Em um domínio, o Kerberos é o protocolo de autenticação padrão.

## <a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir lista os recursos relevantes para NTLM e outras tecnologias de autenticação do Windows.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[Apresentando a restrição de autenticação NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Alterações na autenticação NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planejamento**|[Guia de modelagem de ameaças de infraestrutura de ti](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Threats e contramedidas: Configurações de segurança no Windows Server 2003 e no Windows XP @ no__t-0<br /><br />Guia de [Threats e contramedidas: Configurações de segurança no Windows Server 2008 e no Windows Vista @ no__t-0<br /><br />Guia de [Threats e contramedidas: Configurações de segurança no Windows Server 2008 R2 e no Windows 7 @ no__t-0|
|**Implantação**|[Proteção Estendida para Autenticação](https://support.microsoft.com/kb/968389)<br /><br />[Auditando e restringindo o guia de uso do NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Ask a equipe de serviços de diretório: Bloqueio de NTLM e você: Metodologias de análise e auditoria de aplicativos no Windows 7 @ no__t-0<br /><br />[Blog de autenticação do Windows](https://blogs.technet.com/authentication/)<br /><br />[Configurando o MaxConcurrentAPI para autenticação NTLM Pass @ no__t-1through](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Desenvolver**|[Microsoft NTLM \(Windows @ no__t-2](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[ @ NO__T-1 MS @ NO__T-2NLMP @ NO__T-3: NT LAN Manager \(NTLM @ no__t-1 especificação do protocolo de autenticação @ no__t-2<br /><br />[ @ NO__T-1 MS @ NO__T-2NNTP @ NO__T-3: Autenticação do NT LAN Manager \(NTLM @ no__t-1: Protocolo de transferência de notícias de rede \(NNTP @ no__t-1 extensão @ no__t-2<br /><br />[ @ NO__T-1 MS @ NO__T-2NTHT @ NO__T-3: Especificação de protocolo NTLM sobre HTTP @ no__t-0|
|**Solução de problemas**|Não disponível ainda|
|**Recursos da comunidade**|[Is este cavalo ainda inativo: Afunilamentos de NTLM e o tempo de execução RPC @ no__t-0|



