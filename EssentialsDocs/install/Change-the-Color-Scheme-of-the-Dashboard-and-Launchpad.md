---
title: Alterar o esquema de cores do painel e da barra inicial
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b2913e51-7979-4d48-a431-d2ec5f1042be
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f7079c9e59c44907fa203db48ce366c2b5a1102b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-color-scheme-of-the-dashboard-and-launchpad"></a>Alterar o esquema de cores do painel e da barra inicial

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode alterar o esquema de cores do painel e a barra inicial, definindo as cores que você deseja usar em XML formatadas arquivo, instalar o arquivo. XML em uma pasta no servidor e especificando o nome do arquivo. XML em uma entrada do registro.  
  
## <a name="create-the-xml-file"></a>Criar o arquivo xml  
 O exemplo a seguir mostra os possíveis tipos de conteúdo do arquivo. XML:  
  
```xml  
<DashboardTheme xmlns="https://www.microsoft.com/HSBS/Dashboard/Branding/2010">  
  
  <!-- Hex color values overwriting default SKU theme colors -->  
  
    <SplashScreenBackgroundColor HexValue="FFFFFFFF"/>  
    <SplashScreenBorderColor HexValue="FF000000"/>  
    <MainHeaderBackgroundColor HexValue="FF414141"/>  
    <MainTabBackgroundColor HexValue="FFFFFFFF"/>  
    <MainTabFontColor HexValue="FF999999"/>  
    <MainTabHoverFontColor HexValue="FF0072BC"/>  
    <MainTabSelectedFontColor HexValue="FF0072BC"/>  
    <MainButtonPressedBackgroundColor HexValue="FF0072BC"/>  
    <MainButtonFontColor HexValue="FFFFFFFF"/>  
    <MainButtonBorderColor HexValue="FF6E6E6E"/>  
    <ScrollButtonBackgroundColor HexValue="FFF0F0F0"/>  
    <ScrollButtonArrowColor HexValue="FF999999"/>  
    <ScrollButtonHoverBackgroundColor HexValue="FF999999"/>  
    <ScrollButtonHoverArrowColor HexValue="FF6E6E6E"/>  
    <ScrollButtonSelectedBackgroundColor HexValue="FF6E6E6E"/>  
    <ScrollButtonSelectedArrowColor HexValue="FFFFFFFF"/>  
    <ScrollButtonDisabledBackgroundColor HexValue="FFF8F8F8"/>  
    <ScrollButtonDisabledArrowColor HexValue="FFCCCCCC"/>  
    <AlertTextBlockBackground HexValue="FFFFFFFF"/>  
    <AlertTextBlockFont HexValue="FF000000"/>  
    <FontColor HexValue="FF000000"/>  
    <SubTabBarBackgroundColor HexValue="FFFFFFFF"/>  
    <SubTabBackgroundColor HexValue="FFFFFFFF"/>  
    <SubTabSelectedBackgroundColor HexValue="FF414141"/>  
    <SubTabBorderColor HexValue="FF787878"/>  
    <SubTabFontColor HexValue="FF787878"/>  
    <SubTabHoverFontColor HexValue="FF0072BC"/>  
    <SubTabPressedFontColor HexValue="FFFFFFFF"/>  
    <ListViewColor HexValue="FFFFFFFF"/>  
    <PageBorderColor HexValue="FF999999"/>      
    <LaunchpadButtonHoverBorderColor HexValue="FF6BA0B4"/>  
    <LaunchpadButtonHoverBackgroundColor HexValue="FF41788F"/>  
    <ClientArrowColor HexValue="FFFFFFFF"/>  
    <ClientGlyphColor HexValue="FFFFFFFF"/>  
    <SplitterColor HexValue="FF83C6E2"/>  
    <HomePageBackColor     HexValue="FFFFFFFF"/>  
    <CategoryNotExpandedBackColor HexValue="FFFFB343"/>  
    <CategoryExpandedBackColor HexValue="FFF26522"/>  
    <CategoryNotExpandedForeColor HexValue="FF2A2A2A"/>  
    <CategoryExpandedForeColor HexValue="FFFFFFFF"/>  
    <HomePageTaskListForeColor    HexValue="FF2A2A2A"/>  
    <HomePageTaskListBackColor HexValue="FFEAEAEA"/>  
    <HomePageTaskListHoverForeColor      HexValue="FF2A2A2A"/>  
    <HomePageTaskListItemBorderColor     HexValue="FF999999"/>  
    <HomePageTaskListSelectedForeColor   HexValue="FFFFFFFF"/>  
    <HomePageTaskListSelectedBackColor   HexValue="FFF26522"/>  
    <HomePageTaskDetailStatusForeColor   HexValue="FFF26522"/>  
    <HomePageTaskDetailDescriptionForeColor     HexValue="FF2A2A2A"/>  
    <HomePageLinkForeColor HexValue="FF0072BC"/>  
    <HomePageLinkSelectedForeColor HexValue="FF0054A6"/>  
    <HomePageLinkHoverForeColor   HexValue="FF0072BC"/>  
    <PropertyFormForeColor HexValue="FF2A2A2A"/>  
    <PropertyFormTabHoverColor HexValue="FF0072BC"/>  
    <PropertyFormTabSelectedColor HexValue="FFFFFFFF"/>  
    <PropertyFormTabSelectedBackColor HexValue="FF414141"/>  
    <TaskPanelBackColor HexValue="FFFFFFFF"/>  
    <TaskPanelItemForeColor HexValue="FF2A2A2A"/>  
    <TaskPanelGroupTitleForeColor HexValue="FF2A2A2A"/>  
    <TaskPanelGroupTitleBackColor HexValue="FFCCCCCC"/>  
    <DashboardClientBackColor HexValue="FF004050"/>  
    <DashboardClientTitleColor HexValue="FFFFFFFF"/>  
    <DashboardClientOptionsForeColor HexValue="FFFFFFFF"/>  
    <DashboardClientOptionsItemForeColor HexValue="FF2A2A2A"/>  
    <DashboardClientHelpForeColor HexValue="FF0054A6"/>  
    <ClientSignInForeColor HexValue="FFFFFFFF"/>  
    <ClientSignInWaterMarkForeColor HexValue="FF999999"/>  
    <ClientSignInUserNameForeColor HexValue="FF2A2A2A"/>  
    <ClientSignInPassWordSpecificBackColor HexValue="FFCCCCCC"/>  
    <ClientSignInButtonBackColor HexValue="FF004050"/>  
    <ClientSignInPassWordForeColor HexValue="FF2A2A2A"/>  
    <LaunchPadBackColor HexValue="FF004050"/>  
    <LaunchPadPageTitleColor HexValue="FFFFFFFF"/>  
    <LaunchPadItemForeColor HexValue="FFFFFFFF"/>  
  <LaunchPadItemHoverColor HexValue="33FFFFFF"/>  
  <LaunchPadItemIconBackgroundColor HexValue="F2004050"/>  
</DashboardTheme >  
  
```  
  
> [!IMPORTANT]
>  Os elementos xml devem ser especificados na ordem listada no exemplo anterior.  
  
> [!NOTE]
>  Todos os valores do atributo HexValue são exemplos de valores hexadecimais para cores. Você pode inserir qualquer valor de cor hexadecimal que você deseja usar.  
  
 Use o bloco de notas ou o Visual Studio 2010 para criar o arquivo. XML que contém as marcas para as áreas que você deseja personalizar. O arquivo pode ter qualquer nome, mas ele deve ter uma extensão. XML. Para obter uma descrição das áreas que podem ser personalizadas no painel e na barra inicial, consulte [áreas do painel e na barra inicial que podem ser alteradas](Change-the-Color-Scheme-of-the-Dashboard-and-Launchpad.md#BKMK_Dashboard).  
  
#### <a name="to-install-the-xml-file"></a>Para instalar o arquivo. XML  
  
1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **pesquisa **.  
  
2.  Na caixa de pesquisa, digite **regedit**e, em seguida, clique no **Regedit** aplicativo.  
  
3.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Se o **OEM** chave não existir, você deve concluir as seguintes etapas para criá-lo:  
  
    1.  Clique com botão direito **Windows Server**, aponte para **nova**e clique em **chave**.  
  
    2.  Tipo **OEM** para o nome da chave.  
  
4.  Clique com botão direito **OEM**, aponte para **nova**e clique em **valor de cadeia de caracteres**.  
  
5.  ENTER **CustomColorScheme** como o nome da cadeia de caracteres e pressione **Enter**.  
  
6.  Clique com botão direito **CustomColorScheme** no painel direito e clique em **modificar**.  
  
7.  Insira o nome do arquivo e clique em **Okey**.  
  
8.  Copie o arquivo para %programFiles%\Windows server\bin\oem.. Se o diretório OEM não existir, crie-o.  
  
##  <a name="BKMK_Dashboard"></a>Áreas do painel e na barra inicial que podem ser alteradas  
 Esta seção inclui exemplos de áreas do painel e na barra inicial que podem ser personalizadas.  
  
### <a name="examples"></a>Exemplos  
  
####  <a name="BKMK_Figure1"></a>Figura 1: Página de entrada do painel  
 ![Windows Server Essentials Dashboard](media/SBS8_ADK_Dashboard_Signin_RC.png "SBS8_ADK_Dashboard_Signin_RC")  
  
####  <a name="BKMK_Figure2"></a>Figura 2: barra inicial  
 ![Entrada do Windows SBS Launchpad & #45; em](media/SBS8_ADK_LaunchpadSignin2.png "SBS8_ADK_LaunchpadSignin2")  
  
####  <a name="BKMK_Figure3"></a>Figura 3: Página de entrada da barra inicial  
 ![Windows Server Essentials Launchpad](media/SBS8_ADK_Launchpad_Signin_RC.png "SBS8_ADK_Launchpad_Signin_RC")  
  
####  <a name="BKMK_Figure4"></a>Figura 4: Texto do painel  
 ![Painel de navegação do Windows Server Essentials](media/SBS8_ADK_Navigation_RC.png "SBS8_ADK_Navigation_RC")  
  
####  <a name="BKMK_Figure5"></a>Figura 5: Borda da subguia  
 ![Borda de subguia do Windows SBS Dashboard](media/SBS8_ADK_DashboardSubtabborder.png "SBS8_ADK_DashboardSubtabborder")  
  
####  <a name="BKMK_Figure6"></a>Figura 6: Painel de tarefas  
 ![Painel de tarefas do Windows SBS Dashboard](media/SBS8_ADK_DashboardTaskPane.png "SBS8_ADK_DashboardTaskPane")  
  
####  <a name="BKMK_Figure9"></a>Figura 7a: tela inicial de produtos  
 ![Tela inicial do Windows Server Essentials](media/SBS8_ADK_productspalshscreen_RC.png "SBS8_ADK_productspalshscreen_RC")  
  
#### <a name="figure-7b-home-page"></a>Figura 7b: página inicial  
 ![Página inicial do Windows Server Essentials](media/SBS8_ADK_Dashboard_HomePage_RC.png "SBS8_ADK_Dashboard_HomePage_RC")  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)