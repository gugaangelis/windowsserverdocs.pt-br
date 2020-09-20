---
title: NTLM Overview
description: Segurança do Windows Server
ms.topic: article
ms.assetid: 773909fd-c0bc-498a-95fc-bb452ec04d90
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: d12e61fac3ba6c44dbcac35ea3097e95db54752a
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766779"
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

Reduzir o uso do protocolo NTLM em um ambiente de TI exige o conhecimento dos requisitos do aplicativo implantado em NTLM, bem como das estratégias e das etapas necessárias para configurar ambientes de computação para usar outros protocolos. Novas ferramentas e configurações foram adicionadas para ajudá-lo a descobrir como o NTLM é usado para restringir de maneira seletiva o tráfego NTLM. Para obter informações sobre como analisar e restringir o uso do NTLM em seus ambientes, consulte [Introducing the Restriction of NTLM Authentication (Apresentando a restrição da autenticação NTLM)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560653(v=ws.10)) para acessar o guia de uso do NTLM para auditoria e restrição.

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
|**Avaliação do produto**|[Introducing the Restriction of NTLM Authentication (Apresentando a restrição da autenticação NTLM)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560653(v=ws.10))<p>[Alterações na autenticação NTLM](/previous-versions/windows/it-pro/windows-7/dd566199(v=ws.10))|
|**Planejamento**|[IT Infrastructure Threat Modeling Guide (Guia de modelagem de ameaças de infraestrutura de TI)](/previous-versions/tn-archive/dd941826(v=technet.10))<p>[Ameaças e contramedidas: configurações de segurança no Windows Server 2003 e no Windows XP](/previous-versions/tn-archive/dd162275(v=technet.10))<p>[Guia de ameaças e contramedidas: configurações de segurança no Windows Server 2008 e no Windows Vista](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349791(v=ws.10))<p>[Guia de ameaças e contramedidas: configurações de segurança no Windows Server 2008 R2 e no Windows 7](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125921(v=ws.10))|
|**Implantação**|[Proteção Estendida para Autenticação](https://support.microsoft.com/kb/968389)<p>[Auditing and restricting NTLM usage guide (Guia de uso do NTLM para auditoria e restrição)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/jj865674(v=ws.10))<p>[Ask the Directory Services Team: NTLM Blocking and You: Application Analysis and Auditing Methodologies in Windows 7 (Pergunte à equipe dos Serviços de Diretório: Bloqueio NTLM e você: análise de aplicativo e metodologias de auditoria no Windows 7)](https://blogs.technet.com/askds/archive/2009/10/08/ntlm-blocking-and-you-application-analysis-and-auditing-methodologies-in-windows-7.aspx)<p>[Windows Authentication Blog (Blog de autenticação do Windows)](https://blogs.technet.com/authentication/)<p>[Configuring MaxConcurrentAPI for NTLM pass-through authentication (Configurando MaxConcurrentAPI para autenticação de passagem de NTLM)](https://support.microsoft.com/help/2688798/how-to-do-performance-tuning-for-ntlm-authentication-by-using-the-maxc)|
|**Desenvolvimento**|[Windows NTLM da Microsoft \(\)](/windows/win32/secauthn/microsoft-ntlm)<p>[\[MS \- NLMP \] : \( especificação do protocolo de autenticação NTLM do NT LAN Manager \)](/openspecs/windows_protocols/ms-nlmp/b38c36ed-2804-4868-a9ff-8dd3182128e4)<p>[\[MS \- NNTP \] : autenticação NTLM do Gerenciador de LAN NT \( \) : \( extensão NNTP de protocolo de transferência de notícias de rede \)](/openspecs/windows_protocols/ms-nntp/73ae7d96-30fe-4750-807c-bfe7c38b3a0a)<p>[\[MS \- NTHT \] : especificação de protocolo NTLM sobre http](/openspecs/windows_protocols/ms-ntht/f09cf6e1-529e-403b-a8a5-7368ee096a6a)|
|**Solução de problemas**|Ainda não está disponível|
|**Recursos da comunidade**|[Is this horse dead yet: NTLM Bottlenecks and the RPC runtime (Este assunto ainda não acabou: afunilamentos do NTLM e o tempo de execução do RPC)](https://blogs.technet.com/b/askds/archive/2011/09/15/is-this-horse-dead-yet-ntlm-bottlenecks-and-the-rpc-runtime.aspx)|