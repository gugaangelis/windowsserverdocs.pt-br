---
title: Visão geral de senhas
description: Segurança do Windows Server
ms.topic: article
ms.assetid: f608960e-2039-4c91-9c8c-9b81053c675e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 1b917691f931836605cfe044c725f5200925d2ca
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621879"
---
# <a name="passwords-overview"></a>Visão geral de senhas

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico para o profissional de ti descreve as senhas conforme usadas nos sistemas operacionais Windows, além de links para documentação e discussões sobre o uso de senhas em uma estratégia de gerenciamento de credenciais.

## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descrição do recurso
Atualmente, os sistemas operacionais e os aplicativos são arquitetados em relação a senhas e, mesmo se você usar cartões inteligentes ou sistemas biométricos, todas as contas ainda terão senhas e ainda poderão ser usadas em algumas circunstâncias. Algumas contas, especialmente as usadas para executar serviços, não podem nem usar cartões inteligentes e tokens biométricos e, portanto, devem usar uma senha para autenticação. O Windows protege senhas usando hashes criptográficos.

Para obter mais informações sobre senhas do Windows, consulte [visão geral técnica de senhas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh994558(v=ws.10)).

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas
No Windows e em muitos outros sistemas operacionais, o método mais comum para autenticar a identidade de um usuário é usar uma senha secreta. Proteger seu ambiente de rede requer que senhas fortes sejam usadas por todos os usuários. Isso ajuda a evitar a ameaça de um usuário mal-intencionado adivinhar uma senha fraca, seja por meio de métodos manuais ou usando ferramentas, para adquirir as credenciais de uma conta de usuário comprometida. Isso é especialmente verdadeiro para contas administrativas. Quando você altera uma senha complexa regularmente, ela reduz a probabilidade de um ataque de senha comprometer essa conta.

## <a name="new-and-changed-functionality"></a><a name="BKMK_NEW"></a>Funcionalidade nova e alterada
No Windows Server 2012 e no Windows 8, as senhas de imagem são novas. As senhas de imagem são uma combinação de uma imagem selecionada pelo usuário, juntamente com uma série de gestos. A funcionalidade de senha de imagem está desabilitada em \- computadores ingressados no domínio. Links para obter mais informações sobre senhas de imagem estão listados em [Consulte também](#BKMK_LINKS) abaixo.

Não houve alteração na funcionalidade de senha no Windows Server 2012 e no Windows 8. Nenhuma nova configuração de Política de Grupo foi adicionada. No entanto, melhorias e aprimoramentos foram feitos no \( Gerenciamento de credenciais e senhas \) , como com as senhas de imagem, o armário de credenciais e a entrada no Windows 8 com um conta Microsoft, anteriormente conhecido como Windows Live ID.

## <a name="deprecated-functionality"></a><a name="BKMK_DEP"></a>Funcionalidades preteridas
Nenhuma funcionalidade de senha foi preterida no Windows Server 2012 e no Windows 8.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
Em ambientes corporativos, as senhas normalmente são gerenciadas com Active Directory Domain Services. As senhas também podem ser gerenciadas no computador local usando as configurações em configurações de segurança local, políticas de conta, política de senha.

## <a name="see-also"></a><a name="BKMK_LINKS"></a>Consulte também
Esta tabela lista recursos adicionais para recursos de senha, tecnologia e gerenciamento de credenciais.

|Tipo de conteúdo|Referências|
|--------|-------|
|**Documentação do cenário**|[Protegendo sua identidade digital](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)|
|**Operações**|[Usuários e computadores do Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754217(v=ws.11))|
|**Solução de problemas**|[Descubra quando sua senha expira \- Active Directory Blog do PowerShell](https://blogs.msdn.com/b/adpowershell/archive/2010/08/09/9970198.aspx)|
|**Segurança**| [Guia de ameaças e contramedidas](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125920(v=ws.10)) do windows Server 2008 R2 e do Windows 7: políticas de conta<p>Diretrizes para [alterar e criar senhas fortes](https://www.microsoft.com/security/online-privacy/passwords-create.aspx)|
|**Ferramentas e configurações**|[Referência de configurações de Política de Grupo para Windows e Windows Server no centro de download da Microsoft](https://www.microsoft.com/download/en/details.aspx?amp;displaylang=en&displaylang=en&id=25250)|
|**Recursos da comunidade**|[Protegendo sua identidade digital](https://blogs.msdn.com/b/b8/archive/2011/12/14/protecting-your-digital-identity.aspx)<p>[Entrando no Windows 8 com um Windows Live ID](https://blogs.msdn.com/b/b8/archive/2011/09/26/signing-in-to-windows-8-with-a-windows-live-id.aspx)<p>[Entrando com uma senha de imagem](/archive/blogs/b8/signing-in-with-a-picture-password)<p>[Otimizando a segurança de senha de imagem](/archive/blogs/b8/optimizing-picture-password-security)|