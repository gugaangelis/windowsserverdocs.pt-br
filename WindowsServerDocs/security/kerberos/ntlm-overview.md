---
title: NTLM Overview
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-kerberos
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 972b5b8eb5e25382c2c9b7841cf0d0fe4db6e647
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181872"
---
# <a name="ntlm-overview"></a>NTLM Overview

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico para o profissional de ti descreve o NTLM, quaisquer alterações na funcionalidade e fornece links para recursos técnicos para autenticação do Windows e NTLM para Windows Server 2012 e versões anteriores.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descrição do recurso
A autenticação NTLM é uma família de protocolos de autenticação que são incluídos no0.dll Msv1 do Windows \_ . Os protocolos de autenticação NTLM incluem LAN Manager versão 1 e 2 e NTLM versão 1 e 2. Os protocolos de autenticação NTLM autenticam usuários e computadores com base em um mecanismo de resposta de desafio \/ que comprova para um servidor ou controlador de domínio que um usuário conhece a senha associada a uma conta. Quando o protocolo NTLM é usado, um servidor de recursos deve tomar uma das seguintes ações para verificar a identidade de um computador ou usuário sempre que um novo token de acesso for necessário:

-   Entra em contato com um serviço de autenticação de domínio no controlador de domínio para o domínio da conta do computador ou do usuário, se a conta for uma conta de domínio.

-   Procura a conta do computador ou do usuário no banco de dados de contas locais, se a conta for uma conta local.

## <a name="current-applications"></a><a name="BKMK_APP"></a>Aplicativos atuais
Ainda há suporte para a autenticação NTLM e ela deve ser usada para autenticação do Windows com sistemas configurados como um membro de um grupo de trabalho. A autenticação NTLM também é usada para autenticação de logon local em controladores que não são de \- domínio. A autenticação Kerberos versão 5 é o método de autenticação preferencial para ambientes Active Directory, mas um aplicativo que não seja da \- Microsoft ou da Microsoft ainda pode usar NTLM.

Reduzir o uso do protocolo NTLM em um ambiente de TI exige o conhecimento dos requisitos do aplicativo implantado em NTLM, bem como das estratégias e das etapas necessárias para configurar ambientes de computação para usar outros protocolos. Novas ferramentas e configurações foram adicionadas para ajudá-lo a descobrir como o NTLM é usado para restringir de maneira seletiva o tráfego NTLM. Para obter informações sobre como analisar e restringir o uso do NTLM em seus ambientes, consulte [Introducing the Restriction of NTLM Authentication (Apresentando a restrição da autenticação NTLM)](https://technet.microsoft.com/library/dd560653(v=ws.10).aspx) para acessar o guia de uso do NTLM para auditoria e restrição.

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funcionalidade nova e alterada
Não há nenhuma alteração na funcionalidade para NTLM para o Windows Server 2012.

## <a name="removed-or-deprecated-functionality"></a><a name="BKMK_DEP"></a>Funcionalidades removidas ou reprovadas
Não há nenhuma funcionalidade removida ou preterida para NTLM para o Windows Server 2012.

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>Informações sobre o Gerenciador do Servidor
O NTLM não pode ser configurado a partir do Gerenciador do Servidor. Você pode usar as configurações de Política de Segurança ou Políticas de Grupo para gerenciar o uso da autenticação NTLM entre sistemas de computadores. Em um domínio, o Kerberos é o protocolo de autenticação padrão.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Confira também
A tabela a seguir lista os recursos relevantes para NTLM e outras tecnologias de autenticação do Windows.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Avaliação do produto**|[Introducing the Restriction of NTLM Authentication (Apresentando a restrição da autenticação NTLM)](https://technet.microsoft.com/library/dd560653.aspx)<p>[Alterações na autenticação NTLM](https://technet.microsoft.com/library/dd566199.aspx)|
|**Planejamento**|[IT Infrastructure Threat Modeling Guide (Guia de modelagem de ameaças de infraestrutura de TI)](https://technet.microsoft.com/library/dd941826.aspx)<p>[Ameaças e contramedidas: configurações de segurança no Windows Server 2003 e no Windows XP](https://technet.microsoft.com/library/dd162275.aspx)<p>[Guia de ameaças e contramedidas: configurações de segurança no Windows Server 2008 e no Windows Vista](https://technet.microsoft.com/library/dd349791.aspx)<p>[Guia de ameaças e contramedidas: configurações de segurança no Windows Server 2008 R2 e no Windows 7](https://technet.microsoft.com/library/hh125921.aspx)|
|**Implantação**|[Proteção Estendida para Autenticação](https://support.microsoft.com/kb/968389)<p>[Auditing and restricting NTLM usage guide (Guia de uso do NTLM para auditoria e restrição)](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)<p>[Ask the Directory Services Team: NTLM Blocking and You: Application Analysis and Auditing Methodologies in Windows 7 (Pergunte à equipe dos Serviços de Diretório: Bloqueio NTLM e você: análise de aplicativo e metodologias de auditoria no Windows 7)](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<p>[Windows Authentication Blog (Blog de autenticação do Windows)](https://blogs.technet.com/authentication/)<p>[Configuring MaxConcurrentAPI for NTLM pass-through authentication (Configurando MaxConcurrentAPI para autenticação de passagem de NTLM)](https://support.microsoft.com/help/2688798/how-to-do-performance-tuning-for-ntlm-authentication-by-using-the-maxc)|
|**Desenvolvimento**|[Windows NTLM da Microsoft \(\)](https://msdn.microsoft.com/library/aa378749(VS.85).aspx)<p>[\[MS \- NLMP \] : \( especificação do protocolo de autenticação NTLM do NT LAN Manager \)](https://msdn.microsoft.com/library/cc236621(PROT.10).aspx)<p>[\[MS \- NNTP \] : autenticação NTLM do Gerenciador de LAN NT \( \) : \( extensão NNTP de protocolo de transferência de notícias de rede \)](https://msdn.microsoft.com/library/cc236774(PROT.10).aspx)<p>[\[MS \- NTHT \] : especificação de protocolo NTLM sobre http](https://msdn.microsoft.com/library/cc237488(PROT.10).aspx)|
|**Solução de problemas**|Ainda não está disponível|
|**Recursos da comunidade**|[Is this horse dead yet: NTLM Bottlenecks and the RPC runtime (Este assunto ainda não acabou: afunilamentos do NTLM e o tempo de execução do RPC)](https://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|



