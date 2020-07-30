---
title: Alterar o Esquema de Cores do Dashboard e da Barra Inicial
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: b2913e51-7979-4d48-a431-d2ec5f1042be
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 47d29fd8960de3f85031e59552219166946474f7
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181462"
---
# <a name="change-the-color-scheme-of-the-dashboard-and-launchpad"></a>Alterar o Esquema de Cores do Dashboard e da Barra Inicial

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode alterar o esquema de cores do Dashboard e da Barra Inicial definindo as cores que deseja usar em um arquivo com formatação XML, instalando o arquivo .xml em uma pasta no servidor e especificando o nome do arquivo .xml em uma entrada do Registro.

## <a name="create-the-xml-file"></a>Criar o arquivo xml
 O exemplo a seguir mostra o possível conteúdo do arquivo. xml:

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
    <HomePageBackColor     HexValue="FFFFFFFF"/>
    <CategoryNotExpandedBackColor HexValue="FFFFB343"/>
    <CategoryExpandedBackColor HexValue="FFF26522"/>
    <CategoryNotExpandedForeColor HexValue="FF2A2A2A"/>
    <CategoryExpandedForeColor HexValue="FFFFFFFF"/>
    <HomePageTaskListForeColor    HexValue="FF2A2A2A"/>
    <HomePageTaskListBackColor HexValue="FFEAEAEA"/>
    <HomePageTaskListHoverForeColor      HexValue="FF2A2A2A"/>
    <HomePageTaskListItemBorderColor     HexValue="FF999999"/>
    <HomePageTaskListSelectedForeColor   HexValue="FFFFFFFF"/>
    <HomePageTaskListSelectedBackColor   HexValue="FFF26522"/>
    <HomePageTaskDetailStatusForeColor   HexValue="FFF26522"/>
    <HomePageTaskDetailDescriptionForeColor     HexValue="FF2A2A2A"/>
    <HomePageLinkForeColor HexValue="FF0072BC"/>
    <HomePageLinkSelectedForeColor HexValue="FF0054A6"/>
    <HomePageLinkHoverForeColor   HexValue="FF0072BC"/>
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
>  Todos os valores do atributo HexValue são exemplos de valores hexadecimais para cores. Você pode inserir qualquer valor hexadecimal de cor que deseja usar.

 Use o Notepad ou o Visual Studio 2010 para criar o arquivo .xml que contém as marcas das áreas que deseja personalizar. O arquivo pode receber qualquer nome, desde que tenha uma extensão .xml. Para obter uma descrição das áreas que podem ser personalizadas no Dashboard e na Barra Inicial, consulte [Áreas do Dashboard e da Barra Inicial que podem ser alteradas](Change-the-Color-Scheme-of-the-Dashboard-and-Launchpad.md#BKMK_Dashboard).

#### <a name="to-install-the-xml-file"></a>Para instalar o arquivo .xml

1.  No servidor, mova o mouse para o canto superior direito da tela e clique em **Pesquisar**.

2.  Na caixa de Pesquisa, digite **regedit** e, em seguida, clique no aplicativo **Regedit**.

3.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Se a chave **OEM** não existir, é necessário concluir as seguintes etapas para criá-la:

    1.  Clique com o botão direito do mouse em **Windows Server**, aponte para **Novo** e clique em **Chave**.

    2.  Digite **OEM** para o nome da chave.

4.  Clique com o botão direito do mouse em **OEM**, aponte para **Novo** e clique em **Valor da Cadeia de Caracteres**.

5.  Digite **CustomColorScheme** como nome da cadeia de caracteres e pressione **Enter**.

6.  Clique com o botão direito do mouse em **CustomColorScheme** no painel direito e, em seguida, clique em **Modificar**.

7.  Digite o nome do arquivo e depois clique em **OK**.

8.  Copie o arquivo para %programFiles%\Windows Server\Bin\OEM. Se o diretório OEM não existir, crie-o.

##  <a name="dashboard-and-launchpad-areas-that-can-be-changed"></a><a name="BKMK_Dashboard"></a>Áreas de painel e Launchpad que podem ser alteradas
 Esta seção inclui exemplos das áreas do Dashboard e da Barra Inicial que podem ser personalizadas.

### <a name="examples"></a>Exemplos

####  <a name="figure-1-sign-in-page-of-the-dashboard"></a><a name="BKMK_Figure1"></a>Figura 1: página de entrada do painel
 ![Painel do Windows Server Essentials](media/SBS8_ADK_Dashboard_Signin_RC.png "SBS8_ADK_Dashboard_Signin_RC")

####  <a name="figure-2-launchpad"></a><a name="BKMK_Figure2"></a>Figura 2: Launchpad
 ![&#45;de entrada do Launchpad do Windows SBS](media/SBS8_ADK_LaunchpadSignin2.png "SBS8_ADK_LaunchpadSignin2")

####  <a name="figure-3-sign-in-page-of-the-launchpad"></a><a name="BKMK_Figure3"></a>Figura 3: página de entrada do Launchpad
 ![Windows Server Essentials Launchpad](media/SBS8_ADK_Launchpad_Signin_RC.png "SBS8_ADK_Launchpad_Signin_RC")

####  <a name="figure-4-dashboard-text"></a><a name="BKMK_Figure4"></a>Figura 4: texto do painel
 ![Painel de navegação do Windows Server Essentials](media/SBS8_ADK_Navigation_RC.png "SBS8_ADK_Navigation_RC")

####  <a name="figure-5-subtab-border"></a><a name="BKMK_Figure5"></a>Figura 5: borda subtab
 ![Borda de subguia do Windows SBS Dashboard](media/SBS8_ADK_DashboardSubtabborder.png "SBS8_ADK_DashboardSubtabborder")

####  <a name="figure-6-task-pane"></a><a name="BKMK_Figure6"></a>Figura 6: painel de tarefas
 ![Painel de Tarefas do Windows SBS Dashboard](media/SBS8_ADK_DashboardTaskPane.png "SBS8_ADK_DashboardTaskPane")

####  <a name="figure-7a-product-splash-screen"></a><a name="BKMK_Figure9"></a>Figura 7A: tela inicial do produto
 ![Tela inicial do Windows Server Essentials](media/SBS8_ADK_productspalshscreen_RC.png "SBS8_ADK_productspalshscreen_RC")

#### <a name="figure-7b-home-page"></a>Figura 7b: Home page
 ![Home Page do Windows Server Essentials](media/SBS8_ADK_Dashboard_HomePage_RC.png "SBS8_ADK_Dashboard_HomePage_RC")

## <a name="see-also"></a>Consulte Também
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)