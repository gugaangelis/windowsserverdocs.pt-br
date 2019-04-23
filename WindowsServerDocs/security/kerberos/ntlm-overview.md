---
title: Visão Geral do NTLM
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 523fb71304ae55d17203cab4d1c5a17551bf8fdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890637"
---
# <a name="ntlm-overview"></a>Visão Geral do NTLM

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico para profissionais de TI descreve o NTLM, todas as alterações na funcionalidade e fornece links para recursos técnicos para a autenticação do Windows e NTLM para o Windows Server 2012 e versões anteriores.

## <a name="BKMK_OVER"></a>Descrição do recurso
A autenticação NTLM é uma família de protocolos de autenticação que são englobados no Windows Msv1\_0.dll. Os protocolos de autenticação NTLM incluem LAN Manager versão 1 e 2 e NTLM versão 1 e 2. Os protocolos de autenticação NTLM autenticam usuários e computadores com base em um desafio\/mecanismo de resposta que prova a um servidor ou controlador de domínio que um usuário sabe a senha associada a uma conta. Quando o protocolo NTLM é usado, um servidor de recursos deve tomar uma das seguintes ações para verificar a identidade de um computador ou usuário sempre que um novo token de acesso for necessário:

-   Entra em contato com um serviço de autenticação de domínio no controlador de domínio para o domínio da conta do computador ou do usuário, se a conta for uma conta de domínio.

-   Procura a conta do computador ou do usuário no banco de dados de contas locais, se a conta for uma conta local.

## <a name="BKMK_APP"></a>Aplicativos atuais
Ainda há suporte para a autenticação NTLM e ela deve ser usada para autenticação do Windows com sistemas configurados como um membro de um grupo de trabalho. A autenticação NTLM também é usada para autenticação de logon local no não\-controladores de domínio. Autenticação Kerberos versão 5 é o método de autenticação preferido para ambientes do Active Directory, mas não\-Microsoft ou Microsoft aplicativo ainda pode usar o NTLM.

Reduzir o uso do protocolo NTLM em um ambiente de TI exige o conhecimento dos requisitos do aplicativo implantado em NTLM, bem como das estratégias e das etapas necessárias para configurar ambientes de computação para usar outros protocolos. Novas ferramentas e configurações foram adicionadas para ajudá-lo a descobrir como o NTLM é usado para restringir de maneira seletiva o tráfego NTLM. Para obter informações sobre como analisar e restringir o uso do NTLM em seus ambientes, consulte [Introducing the Restriction of NTLM Authentication (Apresentando a restrição da autenticação NTLM)](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para acessar o guia de uso do NTLM para auditoria e restrição.

## <a name="BKMK_NEW"></a>Funcionalidades novas e alteradas
Não há nenhuma alteração na funcionalidade para NTLM para o Windows Server 2012.

## <a name="BKMK_DEP"></a>Funcionalidade removida ou preterida
Não há nenhuma funcionalidade removida ou preterida para NTLM para o Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Informações do Gerenciador do servidor
O NTLM não pode ser configurado a partir do Gerenciador do Servidor. Você pode usar as configurações de Política de Segurança ou Políticas de Grupo para gerenciar o uso da autenticação NTLM entre sistemas de computadores. Em um domínio, o Kerberos é o protocolo de autenticação padrão.

## <a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir lista os recursos relevantes para NTLM e outras tecnologias de autenticação do Windows.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[Apresentando a restrição da autenticação NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Alterações na autenticação NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planejamento**|[Guia de modelagem de ameaças de infraestrutura de TI](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Ameaças e contramedidas: Configurações de segurança no Windows Server 2003 e Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Ameaças e contramedidas: Configurações de segurança no Windows Server 2008 e Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Ameaças e contramedidas: Configurações de segurança no Windows Server 2008 R2 e Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Implantação**|[Proteção estendida para autenticação](https://support.microsoft.com/kb/968389)<br /><br />[Auditoria e restringir o guia de uso do NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Pergunte à equipe de serviços de diretório: Bloqueio de NTLM e você: No Windows 7 as metodologias de auditoria e análise de aplicativos](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog de autenticação do Windows](https://blogs.technet.com/authentication/)<br /><br />[Configurando MaxConcurrentAPI para passagem NTLM\-por meio da autenticação](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Desenvolvimento**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: NT LAN Manager \(NTLM\) especificação do protocolo de autenticação](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: NT LAN Manager \(NTLM\) autenticação: Network News Transfer Protocol \(NNTP\) extensão](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: NTLM sobre a especificação do protocolo HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Solução de problemas**|Não disponível ainda|
|**Recursos da comunidade**|[É esse cavalo dead ainda: Afunilamentos do NTLM e o tempo de execução RPC](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



