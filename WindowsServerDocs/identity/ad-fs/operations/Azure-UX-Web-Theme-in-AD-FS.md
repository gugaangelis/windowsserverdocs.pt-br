---
title: Tema da Web do Azure AD UX no AD FS
description: O documento a seguir descreve como alterar a entrada do AD FS Forms para que ele se assemelhe à experiência do usuário do Azure AD.
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.openlocfilehash: a7cb3a037d074fc4a61e6c805bca181316643bb3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956673"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Usando um tema da Web do Azure AD UX no Serviços de Federação do Active Directory (AD FS)
Atualmente, a entrada de formulários AD FS não espelha a experiência de entrada do Azure/O365.  Para fornecer uma experiência mais uniforme e simples para os usuários finais, lançamos o seguinte tema da Web de folha de estilos em cascata que pode ser aplicado aos seus servidores de AD FS.  Atualmente, a entrada de formulários para AD FS no Windows Server 2016 é semelhante ao seguinte:

![Entrada atual](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Com a nova folha de estilos, a experiência do usuário se parecerá mais com as experiências de entrada do Azure e do Office 365.

## <a name="download-the-css-style-sheet"></a>Baixar a folha de estilos CSS
Você pode baixar o tema da Web no seguinte [local](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)do github.


## <a name="enabling-the-new-web-theme"></a>Habilitando o novo tema da Web
Para habilitar o novo tema da Web, use o seguinte procedimento:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Para habilitar o novo tema da Web do Azure AD UX no AD FS
1. Iniciar o PowerShell como administrador
2. Criar um novo tema da Web usando o PowerShell:`New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3. Definir o novo tema como o tema ativo usando o PowerShell: `Set-AdfsWebConfig -ActiveThemeName custom` 
    ![ PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4. Teste a entrada acessando https:// <AD FS name.domain> /adfs/ls/idpinitiatedsignon.htm ![ logon](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ! ANOTAÇÕES Você precisa garantir que o idpinitiatedsignon tenha sido habilitado.  Ele não está habilitado por padrão.  Para habilitar o idpinitiatedsignon, use o seguinte comando do PowerShell:`Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recomendações de imagem
Habilitar a interface do usuário centralizada permite que você use as mesmas imagens para o plano de fundo e o logotipo que você já tem para Azure Active Directory identidade visual da empresa. Em geral, as mesmas recomendações para tamanho, proporção e formato se aplicam.

### <a name="logo"></a>Logotipo

Descrição | Restrições | Recomendações
------- | ------- | ----------
O logotipo é exibido na parte superior do painel de logon. | JPG ou PNG transparente<br>Altura máxima: 36 px<br>Largura máxima: 245 px | Use o logotipo da sua organização aqui.<br>Use uma imagem transparente. Não presuma que o plano de fundo será branco.<br>Não adicione preenchimento ao redor de seu logotipo na imagem ou seu logotipo parecerá desproporcionalmente pequeno.

### <a name="background"></a>Segundo plano

Descrição | Restrições | Recomendações
------- | ------- | ----------
Essa opção é exibida na tela de fundo da página de entrada, é ancorada no centro do espaço visível e é dimensionada e cortada para preencher a janela do navegador.    <br>Em telas estreitas, como celulares, a imagem não é mostrada.<br>Uma máscara preta com opacidade de 0,55 é aplicada sobre essa imagem quando a página é carregada. | JPG ou PNG<br>Dimensões da imagem: 1920 x 1080 px<br>Tamanho do arquivo: &lt; 300 KB | <br>Use imagens que não possuam um foco forte no assunto. O formulário de entrada opaco é exibido no centro dessa imagem e pode abranger qualquer parte da imagem, dependendo do tamanho da janela do navegador.<br>Mantenha o tamanho do arquivo pequeno para garantir tempos de carregamento rápidos.

## <a name="next-steps"></a>Próximas etapas
- [AD FS personalização no Windows Server 2016](./ad-fs-customization-in-windows-server.md)
- [Personalização avançada](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Temas da Web personalizados](Custom-Web-Themes-in-AD-FS.md)
