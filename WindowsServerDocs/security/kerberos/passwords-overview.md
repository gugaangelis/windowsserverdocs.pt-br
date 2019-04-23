---
title: Visão geral de senhas
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-kerberos
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 6c1b8d56b5c0da738e7dae5c0072be81040f90d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869597"
---
# <a name="passwords-overview"></a>Visão geral de senhas

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico para profissionais de TI descreve as senhas como utilizado em sistemas de operacionais do Windows e links para documentação e discussões sobre o uso de senhas em uma estratégia de gerenciamento de credencial.

## <a name="BKMK_OVER"></a>Descrição do recurso
Sistemas operacionais e aplicativos de hoje são projetados em torno de senhas e, mesmo se você usar cartões inteligentes ou a biométricos sistemas, todas as contas ainda têm senhas e ainda pode ser usados em algumas circunstâncias. Algumas contas, especialmente as contas usadas para executar serviços, não é possível até mesmo usar cartões inteligentes e tokens biométricas e, portanto, devem usar uma senha para autenticar. Windows protege senhas usando os hashes criptográficos.

Para obter mais informações sobre as senhas do Windows, consulte [visão geral técnica de senhas](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Aplicativos práticos
No Windows e muitos outros sistemas operacionais, o método mais comum para autenticar a identidade do usuário é usar uma senha secreta. Protegendo seu ambiente de rede, é necessário que as senhas fortes sejam usados por todos os usuários. Isso ajuda a evitar a ameaça de um usuário mal-intencionado adivinhe uma senha fraca, seja por meio de métodos manuais ou usando as ferramentas, para adquirir as credenciais de uma conta de usuário comprometidas. Isso é especialmente verdadeiro para contas administrativas. Quando você altera uma senha complexa regularmente, ele reduz a probabilidade de um ataque de senha comprometer a essa conta.

## <a name="BKMK_NEW"></a>Funcionalidades novas e alteradas
No Windows Server 2012 e Windows 8, senhas com imagem são novas. Senhas com imagem são uma combinação de uma imagem selecionada do usuário juntamente com uma série de gestos. Funcionalidade de senha de imagem está desabilitada no domínio\-computadores ingressados no. Links para obter mais informações sobre senhas com imagem são listados em [Consulte também](#BKMK_LINKS) abaixo.

Não houve nenhuma alteração na funcionalidade de senha do Windows Server 2012 e Windows 8. Não há novas configurações de diretiva de grupo foram adicionadas. No entanto, aprimoramentos e melhorias feitas na credencial \(e a senha\) gerenciamento, como com senhas com imagem, o Cofre de credenciais e entrar no Windows 8 com uma conta da Microsoft, anteriormente conhecido como um Windows Live ID .

## <a name="BKMK_DEP"></a>Funcionalidade preterida
Nenhuma funcionalidade de senha foi preterida no Windows Server 2012 e Windows 8.

## <a name="BKMK_SOFT"></a>Requisitos de software
Em ambientes empresariais, as senhas são normalmente gerenciadas com serviços de domínio Active Directory. As senhas também podem ser gerenciadas no computador local usando as configurações na política de senha de configurações de segurança, diretivas de conta local.

## <a name="BKMK_LINKS"></a>Consulte também
Esta tabela lista recursos adicionais para os recursos de senha, gerenciamento de tecnologia e as credenciais.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Documentação do cenário**|[Protegendo sua identidade digital](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operações**|[Computadores e usuários do Active Directory](https://technet.microsoft.com/library/cc754217.aspx)|
|**Solução de problemas**|[Descubra quando sua senha expira \- Blog do PowerShell do Active Directory](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Segurança**| Windows Server 2008 R2 e Windows 7 [guia sobre ameaças e contramedidas: Diretivas de conta](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />Diretrizes para [alterar e criar senhas fortes](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Ferramentas e configurações**|[Referência de configurações de diretiva de grupo para Windows e Windows Server no Centro de Download da Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Recursos da comunidade**|[Protegendo sua identidade digital](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Entrando no Windows 8 com um Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Entrar com uma senha de imagem](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Otimizando a segurança de senha de imagem](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


