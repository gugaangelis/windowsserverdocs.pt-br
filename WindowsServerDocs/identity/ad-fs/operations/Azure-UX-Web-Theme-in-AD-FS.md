---
title: Tema de Web do Azure AD UX no AD FS
description: "Este documento descreve como alterar o AD FS formulários-entrar para que ele se parece com a experiência do usuário do Azure AD."
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7bac1db17eb4facc7643fe0db0ccf00c119a45d
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2018
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Usando um tema de Web do Azure AD UX nos serviços de Federação do Active Directory
Os formulários do AD FS login atualmente não espelhar a experiência de entrada do Azure/O365.  Para fornecer uma experiência mais uniforme e perfeita para os usuários finais, lançamos o seguir em cascata tema folha de estilos da web que pode ser aplicado aos servidores do AD FS.  Atualmente, o formulários entrar do AD FS no Windows Server 2016 é semelhante a seguir:

![Entrada atual](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Com a nova folha de estilos, a experiência do usuário terá uma aparência mais como as experiências entrar Azure e Office 365.

## <a name="download-the-css-style-sheet"></a>Baixe a folha de estilos CSS
Você pode baixar o tema da web do Github a seguir [local ](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi).


## <a name="enabling-the-new-web-theme"></a>Habilitando o novo tema da web
Para ativar o novo tema da web use o procedimento a seguir:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Para habilitar o novo tema web UX do Azure AD no AD FS
1.  Inicie o PowerShell como administrador
2.  Crie um novo tema da web usando o PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3.  Definir o novo tema como o tema de ativo usando o PowerShell: <ph x="1">'Conjunto AdfsWebConfig - ActiveThemeName custom'
! [</ph>PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4.  Teste a entrada acessando https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm ![logon](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

>! [OBSERVAÇÃO] Você precisa garantir que idpinitiatedsignon foi habilitada.  Ele não é habilitado por padrão.  Para habilitar idpinitiatedsignon usar o seguinte comando do PowerShell:  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recomendações de imagem
A seguir estão as recomendações de tamanho para a imagem de plano de fundo e a imagem do logotipo:

### <a name="logo"></a>Logotipo
- 24px altura, largura máxima 256px
- Não adicione qualquer preenchimento ao redor do logotipo dentro do ativo.  Certifique-se de que a tela de fundo ativo é transparente.

### <a name="background"></a>Em segundo plano
- Dimensione 1024 x 1080 pixels com um tamanho de arquivo de não mais que 200KB.  Você deve usar a melhor resolução possível com taxa de proporção 16:9 / 16:10 que mantém o tamanho da imagem sob a restrição.

## <a name="next-steps"></a>Próximas etapas
- [AD FS personalização no Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personalização avançada](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Temas da web personalizado](Custom-Web-Themes-in-AD-FS.md)
