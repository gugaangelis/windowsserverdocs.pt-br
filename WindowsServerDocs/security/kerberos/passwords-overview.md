---
title: Visão geral de senhas
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-kerberos
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 19003d5dcfdaa0f9d6dafcc31bab31a5cb50efa0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858829"
---
# <a name="passwords-overview"></a>Visão geral de senhas

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico para o profissional de ti descreve as senhas conforme usadas nos sistemas operacionais Windows, além de links para documentação e discussões sobre o uso de senhas em uma estratégia de gerenciamento de credenciais.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descrição do recurso
Atualmente, os sistemas operacionais e os aplicativos são arquitetados em relação a senhas e, mesmo se você usar cartões inteligentes ou sistemas biométricos, todas as contas ainda terão senhas e ainda poderão ser usadas em algumas circunstâncias. Algumas contas, especialmente as usadas para executar serviços, não podem nem usar cartões inteligentes e tokens biométricos e, portanto, devem usar uma senha para autenticação. O Windows protege senhas usando hashes criptográficos.

Para obter mais informações sobre senhas do Windows, consulte [visão geral técnica de senhas](https://technet.microsoft.com/library/hh994558(WS.10).aspx).

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicativos práticos
No Windows e em muitos outros sistemas operacionais, o método mais comum para autenticar a identidade de um usuário é usar uma senha secreta. Proteger seu ambiente de rede requer que senhas fortes sejam usadas por todos os usuários. Isso ajuda a evitar a ameaça de um usuário mal-intencionado adivinhar uma senha fraca, seja por meio de métodos manuais ou usando ferramentas, para adquirir as credenciais de uma conta de usuário comprometida. Isso é especialmente verdadeiro para contas administrativas. Quando você altera uma senha complexa regularmente, ela reduz a probabilidade de um ataque de senha comprometer essa conta.

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funcionalidade nova e alterada
No Windows Server 2012 e no Windows 8, as senhas de imagem são novas. As senhas de imagem são uma combinação de uma imagem selecionada pelo usuário, juntamente com uma série de gestos. A funcionalidade de senha de imagem está desabilitada no domínio\-computadores associados. Links para obter mais informações sobre senhas de imagem estão listados em [Consulte também](#BKMK_LINKS) abaixo.

Não houve alteração na funcionalidade de senha no Windows Server 2012 e no Windows 8. Nenhuma nova configuração de Política de Grupo foi adicionada. No entanto, melhorias e aprimoramentos foram feitos em \(de credencial e gerenciamento de\) de senha, como com senhas de imagem, o armário de credenciais e a entrada no Windows 8 com um conta Microsoft, anteriormente conhecido como Windows Live ID.

## <a name="deprecated-functionality"></a><a name="BKMK_DEP"></a>Funcionalidade preterida
Nenhuma funcionalidade de senha foi preterida no Windows Server 2012 e no Windows 8.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
Em ambientes corporativos, as senhas normalmente são gerenciadas com Active Directory Domain Services. As senhas também podem ser gerenciadas no computador local usando as configurações em configurações de segurança local, políticas de conta, política de senha.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Consulte também
Esta tabela lista recursos adicionais para recursos de senha, tecnologia e gerenciamento de credenciais.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Documentação do cenário**|[Protegendo sua identidade digital](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operações**|[Active Directory usuários e computadores](https://technet.microsoft.com/library/cc754217.aspx)|
|**Solução de problemas**|[Descubra quando sua senha expira \- Active Directory Blog do PowerShell](https://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Segurança**| [Guia de ameaças e contramedidas](https://technet.microsoft.com/library/hh125920(v=ws.10).aspx) do windows Server 2008 R2 e do Windows 7: políticas de conta<p>Diretrizes para [alterar e criar senhas fortes](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Ferramentas e configurações**|[Referência de configurações de Política de Grupo para Windows e Windows Server no centro de download da Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Recursos da comunidade**|[Protegendo sua identidade digital](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<p>[Entrar no Windows 8 com um Windows Live ID](https://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<p>[Entrando com uma senha de imagem](https://blogs.msdn.com/b/b8/archive/2011/12/16/signing-in-with-a-picture-password.aspx)<p>[Otimizando a segurança de senha de imagem](https://blogs.msdn.com/b/b8/archive/2011/12/19/optimizing-picture-password-security.aspx)|


