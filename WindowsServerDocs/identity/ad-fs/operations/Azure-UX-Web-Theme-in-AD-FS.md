---
title: Tema da Web de experiência do usuário do Azure AD no AD FS
description: O documento a seguir descreve como alterar o sign-in de formulários do AD FS para que ele fique parecido com a experiência do usuário do Azure AD.
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8d6afd7829c92382815e95b8c43a054b000359e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887877"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>Usando um tema da Web de experiência do usuário do Azure AD nos serviços de Federação do Active Directory
Logon de formulários do AD FS no atualmente não refletem a experiência de logon do Azure/O365.  Para fornecer uma experiência mais uniforme e perfeita para os usuários finais, lançamos o seguir em cascata tema folha de estilos da web que pode ser aplicado aos servidores do AD FS.  Atualmente, formulários na entrada do AD FS no Windows Server 2016 aparência a seguir:

![Entrar atual](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


Com a nova folha de estilos, a experiência do usuário ficará mais parecido com os Azure e Office 365 experiências de conexão.

## <a name="download-the-css-style-sheet"></a>Baixe a folha de estilos CSS
Você pode baixar o tema da web do Github a seguir [local](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi).


## <a name="enabling-the-new-web-theme"></a>Habilitando o novo tema da web
Para habilitar o novo tema da web use o procedimento a seguir:

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>Para habilitar o novo tema da web do Azure AD experiência do usuário no AD FS
1.  Inicie o PowerShell como administrador
2.  Crie um novo tema da web usando o PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3.  Defina o novo tema como o tema ativo usando o PowerShell:  `Set-AdfsWebConfig -ActiveThemeName custom`
![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4.  Testar a entrar, vá para https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm ![Sign-on](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ! [OBSERVAÇÃO] Você precisa garantir que idpinitiatedsignon tiver sido habilitado.  Ele não está habilitado por padrão.  Para habilitar idpinitiatedsignon use o seguinte comando do PowerShell:  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>Recomendações de imagem
Habilitando a interface do usuário centralizado permite que você use as mesmas imagens de plano de fundo e logotipo que você já pode ter para a marca da empresa do Active Directory do Azure. Em geral, as mesmas recomendações de tamanho, taxa e formato se aplicam.

### <a name="logo"></a>Logotipo
Descrição | Restrições | Recomendações
------- | ------- | ----------
O logotipo é exibido na parte superior do painel de logon. | JPG ou PNG transparente<br>Altura máxima: 36 px<br>Largura máxima: 245 px | Use o logotipo da sua organização aqui.<br>Use uma imagem transparente. Não presuma que a tela de fundo será branca.<br>Não adicione preenchimento ao redor de seu logotipo na imagem ou seu logotipo parecerá desproporcionalmente pequeno.

### <a name="background"></a>Histórico
Descrição | Restrições | Recomendações
------- | ------- | ----------
Essa opção é exibida na tela de fundo da página de entrada, é ancorada no centro do espaço visível e escalas e cortada para preencher a janela do navegador.    <br>Em telas estreitas, como celulares, essa imagem não é mostrada.<br>Uma máscara preta com opacidade de 0,55 é aplicada sobre essa imagem quando a página é carregada. | JPG ou PNG<br>Dimensões da imagem: 1920x1080 px<br>Tamanho do arquivo: &lt; 300 KB | <br>Use imagens que não tem um foco forte no assunto. O formulário de entrada opaco aparece sobre o centro da imagem e pode abranger qualquer parte da imagem, dependendo do tamanho da janela do navegador.<br>Mantenha o tamanho de arquivo pequeno para garantir tempos de carregamento rápido.

## <a name="next-steps"></a>Próximas etapas
- [Personalização do AD FS no Windows Server 2016](AD-FS-Customization-in-Windows-Server-2016.md)
- [Personalização avançada](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [Temas da web personalizado](Custom-Web-Themes-in-AD-FS.md)
