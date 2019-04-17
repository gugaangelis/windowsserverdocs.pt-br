---
title: "Visão geral de senhas"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="passwords-overview"></a>Visão geral de senhas

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico para o profissional de TI descreve as senhas como usada em sistemas operacionais Windows e links para documentação e discussões sobre o uso de senhas em uma estratégia de gerenciamento de credenciais.

## <a name="BKMK_OVER"></a>Descrição dos recursos
Sistemas operacionais e aplicativos hoje são pensados no senhas e mesmo se você usar cartões inteligentes ou sistemas biométricos, todas as contas ainda tenham senhas e ainda pode ser usadas em algumas circunstâncias. Algumas contas, especialmente contas usadas para executar serviços, não é possível até mesmo usar cartões inteligentes e tokens biométricos e, portanto, devem usar uma senha para autenticar. Windows protege senhas usando hashes criptográficos.

Para saber mais sobre senhas do Windows, consulte [visão geral técnica das senhas](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="BKMK_APP"></a>Aplicativos práticos
No Windows e muitos outros sistemas operacionais, o método mais comum para autenticar a identidade do usuário é usar uma frase secreta ou senha. Protegendo seu ambiente de rede requer que senhas fortes seja usada por todos os usuários. Isso ajuda a evitar a ameaça de um usuário mal-intencionado adivinhar uma senha fraca, seja por meio de métodos manuais ou usando ferramentas, para adquirir as credenciais de uma conta de usuário comprometido. Isso é especialmente verdadeiro para contas de administrador. Quando você alterar uma senha complexa regularmente, ele reduz a probabilidade de um ataque de senha comprometer essa conta.

## <a name="BKMK_NEW"></a>Funcionalidades novas e alteradas
No Windows Server 2012 e Windows 8, senhas com imagem são novas. Senhas com imagem são uma combinação de uma imagem selecionada do usuário juntamente com uma série de gestos. Funcionalidade de senha de imagem está desabilitada em computadores ingressados em domain\. Links para saber mais sobre senhas com imagem são listados em [Consulte também](#BKMK_LINKS) abaixo.

Não houve nenhuma alteração na funcionalidade de senha do Windows Server 2012 e Windows 8. Não há novas configurações de política de grupo foram adicionadas. No entanto, melhorias e aprimoramentos foram feitos no gerenciamento de \(and password\) de credenciais, como com senhas com imagem, o Cofre de credenciais e fazer logon no Windows 8 com uma conta da Microsoft, anteriormente conhecido como um Windows Live ID.

## <a name="BKMK_DEP"></a>Funcionalidade substituída
Nenhuma funcionalidade de senha foi preterida no Windows Server 2012 e Windows 8.

## <a name="BKMK_SOFT"></a>Requisitos de software
Em ambientes corporativos, senhas normalmente são gerenciadas com os serviços de domínio do Active Directory. Senhas também podem ser gerenciadas no computador local usando as configurações em política de senha de configurações de segurança, políticas de conta local.

## <a name="BKMK_LINKS"></a>Consulte também
Esta tabela lista os recursos adicionais para recursos de senha, gerenciamento de tecnologia e credencial.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Documentação do cenário**|[Protegendo sua identidade digital](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operações**|[Usuários e computadores active Directory](https://technet.microsoft.com/library/cc754217.aspx)|
|**Solução de problemas**|[Descubra quando sua senha expira \-Blog do Active Directory do PowerShell](http://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Segurança**| Windows Server 2008 R2 e Windows 7 [guia de ameaças e contramedidas: políticas de conta](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx)<br /><br />Orientações para [alterar e criar senhas fortes](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Ferramentas e configurações**|[Referência de configurações de política de grupo para Windows e Windows Server no Centro de Download da Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Recursos da comunidade**|[Protegendo sua identidade digital](http://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<br /><br />[Fazer logon no Windows 8 com um Windows Live ID](http://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<br /><br />[Logon com uma senha com imagem](http://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<br /><br />[Otimizando a segurança de senha de imagem](http://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


