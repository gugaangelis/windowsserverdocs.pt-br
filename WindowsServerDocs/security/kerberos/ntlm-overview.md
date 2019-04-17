---
title: "Visão geral NTLM"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ntlm-overview"></a>Visão geral NTLM

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI descreve NTLM, todas as alterações na funcionalidade e fornece links para recursos técnicos para autenticação do Windows e NTLM para o Windows Server 2012 e versões anteriores.

## <a name="BKMK_OVER"></a>Descrição dos recursos
A autenticação NTLM é uma família de protocolos de autenticação que englobada em Msv1\_0.dll do Windows. Os protocolos de autenticação NTLM incluem o LAN Manager versão 1 e 2 e NTLM versão 1 e 2. Os protocolos de autenticação NTLM autenticam usuários e computadores com base em um mecanismo de challenge\/resposta que prova para um servidor ou controlador de domínio que um usuário sabe a senha associada a uma conta. Quando o protocolo NTLM for usado, um servidor de recursos deve ter uma das seguintes ações para verificar a identidade de um computador ou usuário sempre que um novo token de acesso é necessário:

-   Fale com um serviço de autenticação de domínio no controlador de domínio para domínio da conta do computador ou do usuário, se a conta é uma conta de domínio.

-   Procure do computador ou da conta de usuário no banco de dados de conta local, se a conta é uma conta local.

## <a name="BKMK_APP"></a>Aplicativos atuais
A autenticação NTLM ainda é suportada e deve ser usada para autenticação do Windows com sistemas configurados como um membro de um grupo de trabalho. A autenticação NTLM também é usada para autenticação de logon local em controladores de domínio non\. Autenticação Kerberos versão 5 é o método de autenticação preferencial para ambientes do Active Directory, mas um non\ da Microsoft ou o aplicativo Microsoft talvez ainda usam NTLM.

Reduzir o uso do protocolo NTLM em um ambiente de TI requer conhecimento dos requisitos de aplicativo implantado no NTLM e estratégias e etapas necessárias para configurar os ambientes de computação usar outros protocolos. Novas ferramentas e configurações foram adicionadas para ajudá-lo a descobrir como NTLM é usado para restringir seletivamente o tráfego NTLM. Para obter informações sobre como analisar e restringir o uso NTLM em seus ambientes, consulte [apresentando a restrição de autenticação NTLM](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para acessar a auditoria e restringir o guia de uso NTLM.

## <a name="BKMK_NEW"></a>Funcionalidades novas e alteradas
Não há nenhuma alteração na funcionalidade de NTLM para o Windows Server 2012.

## <a name="BKMK_DEP"></a>Funcionalidade removida ou preterida
Não há nenhuma funcionalidade removida ou preterida para NTLM para o Windows Server 2012.

## <a name="BKMK_INSTALL"></a>Informações do Gerenciador do servidor
NTLM não pode ser configurado no Gerenciador de servidores. Você pode usar as configurações de política de segurança ou políticas de grupo para gerenciar o uso de autenticação NTLM entre os sistemas de computador. Em um domínio, Kerberos é o protocolo de autenticação padrão.

## <a name="BKMK_LINKS"></a>Consulte também
A tabela a seguir lista os recursos relevantes para NTLM e outras tecnologias de autenticação do Windows.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[Apresentando a restrição de autenticação NTLM](https://technet.microsoft.com/library/dd560653.aspx)<br /><br />[Alterações na autenticação NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planejamento**|[Guia de modelagem de ameaças de infraestrutura de TI](https://technet.microsoft.com/library/dd941826.aspx)<br /><br />[Ameaças e contramedidas: configurações de segurança no Windows Server 2003 e Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<br /><br />[Guia de ameaças e contramedidas: configurações de segurança no Windows Server 2008 e Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<br /><br />[Guia de ameaças e contramedidas: configurações de segurança no Windows Server 2008 R2 e Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Implantação**|[Proteção estendida para autenticação](https://support.microsoft.com/kb/968389)<br /><br />[Auditoria e restringir o guia de uso NTLM](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<br /><br />[Peça a equipe de serviços de diretório: bloqueando NTLM e você: análise de aplicativo e auditoria metodologias no Windows 7](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<br /><br />[Blog de autenticação do Windows](https://blogs.technet.com/authentication/)<br /><br />[Configurando MaxConcurrentAPI para a autenticação NTLM pass\-through](https://social.technet.microsoft.com/wiki/contents/articles/9759.configuring-maxconcurrentapi-for-ntlm-pass-through-authentication.aspx)|
|**Desenvolvimento**|[Microsoft NTLM \(Windows\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<br /><br />[\[MS\-NLMP\]: especificação de protocolo de autenticação do NT LAN Manager \(NTLM\)](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<br /><br />[\[MS\-NNTP\]: a autenticação \(NTLM\) NT LAN Manager: extensão de \(NNTP\) de protocolo de transferência de notícias de rede](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<br /><br />[\[MS\-NTHT\]: NTLM pela especificação do protocolo HTTP](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Solução de problemas**|Ainda não disponível|
|**Recursos da comunidade**|[É o cavalo já morreu?: NTLM afunilamentos e o tempo de execução RPC](http://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



