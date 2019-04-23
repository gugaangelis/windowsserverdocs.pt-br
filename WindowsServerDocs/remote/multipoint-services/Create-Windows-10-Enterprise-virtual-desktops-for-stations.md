---
title: Criar áreas de trabalho virtuais do Windows 10 Enterprise para estações
description: Aprenda a criar áreas de trabalho do Windows Server 2016 de estação
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 63f08b5b-c735-41f4-b6c8-411eff85a4ab
author: evaseydl
ms.author: evas
manager: scottman
ms.openlocfilehash: befd784f4a2179c121992057e298d4ea9068c11b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862077"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>Criar áreas de trabalho virtuais do Windows 10 Enterprise para estações
Essa configuração opcional no MultiPoint Services destina-se principalmente para situações em que um aplicativo essencial requer sua própria instância de um sistema operacional cliente para cada usuário. Exemplos incluem aplicativos que não podem ser instalados no Windows Server e aplicativos que não serão executados várias instâncias no mesmo computador host.  
  
> [!NOTE]  
> Essas áreas de trabalho virtuais, também conhecido como o VDI, são muito mais muitos recursos que as sessões de área de trabalho do MultiPoint Services do padrão, portanto, é recomendável que você use sessões do MultiPoint Services padrão quando possível.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Para se preparar para criar a estação de áreas de trabalho virtuais, certifique-se de que seus serviços do MultiPoint o sistema atende aos seguintes requisitos:      
  
|Hardware|Requisitos|         |
|------------|----------------|----------------| 
|CPU (multimídia)|1 núcleo ou thread por máquina virtual|  
|Unidade de estado sólido (SSD)|Capacidade de > = 20GB por estação + 40GB para sistema operacional de host do MultiPoint Services<br /><br />Leitura aleatória\/IOPS de gravação > = 3 mil por estação|  
|RAM|2GB por estação + 2GB para o sistema operacional do host Windows MultiPoint Server|  
|Gráficos|DX11|  
|BIOS|Configuração de CPU de BIOS configurada para habilitar a virtualização – conversão de endereço de segundo nível (SLAT)|  
  
-   **Estações** -configurar estações para seu sistema MultiPoint Services. Para obter mais informações, consulte [anexar estações adicionais ao MultiPoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
-   **Domínio** - o em um ambiente de domínio, o computador Windows MultiPoint Server foi adicionado ao domínio, e um usuário de domínio foi adicionado ao grupo Administradores local no sistema de operacional do host do MultiPoint Services.  
  
## <a name="procedures"></a>Procedimentos  
Use os procedimentos a seguir para:  
  
-   [Criar um modelo para áreas de trabalho virtuais](#a-namebkmkcreateatemplateacreate-a-template-for-virtual-desktops)  
  
-   [Criar áreas de trabalho virtuais a partir do modelo](#BKMK_CreateVirtualDesktopsfromTemplate)  
  
-   [Copiar um modelo de área de trabalho virtual existente](#BKMK_CopyExiistingVirtualDesktopTemplate)  
  
### <a name="BKMK_CreateaTemplate"></a>Criar um modelo para áreas de trabalho virtuais  
Antes de criar um modelo para áreas de trabalho virtuais, você deve habilitar o recurso de área de trabalho Virtual no servidor do MultiPoint.  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>Para habilitar o recurso de área de trabalho Virtual  
  
1.  Faça logon no sistema de operacional do host MultiPoint Server com uma conta de administrador local ou em um domínio, com uma conta de domínio que seja membro do grupo Administradores local.  
  
2.  Dos **iniciar** tela, abra o MultiPoint Manager.  
  
3.  Clique o **áreas de trabalho virtuais** , clique em **habilitar áreas de trabalho virtuais**e, em seguida, clique em **Okey**e aguarde até que o sistema seja reiniciado.  
  
A próxima etapa é criar um modelo de área de trabalho Virtual. Literalmente, você está criando um arquivo de disco rígido virtual (VHD) que você pode usar como modelo para criar áreas de trabalho virtuais para o MultiPoint Manager estação. Você pode usar a mídia de instalação físico para Windows ou um. Arquivo de imagem ISO para como origem para o modelo. Você também pode usar um. VHD da instalação do Windows. Observe que, para usar um disco de instalação física, você deve inserir o disco antes de iniciar o assistente.  
  
##### <a name="to-create-a-virtual-desktop-template"></a>Para criar um modelo de área de trabalho Virtual  
  
1.  Faça logon no sistema de operacional do host MultiPoint Server com uma conta de administrador local ou, no domínio, uma conta de domínio que seja membro do grupo Administradores local.  
  
2.  Dos **iniciar** tela, abra o MultiPoint Manager.  
  
3.  Clique o **áreas de trabalho virtuais** guia.  
  
4.   Copie um arquivo. ISO do Windows 10 Enterprise para o SSD local.  
  
5.  Na guia áreas de trabalho virtuais, clique em **criar modelo de área de trabalho virtual.**   
  
6.  Na **prefixo**, insira um prefixo a ser usado para identificar o modelo e das áreas de trabalho criadas com o modelo. O prefixo padrão é o nome do computador host.  
  
    O prefixo é usado para nomear o modelo e as estações da área de trabalho virtual. O modelo será <*prefixo*>-t. As estações da área de trabalho virtual serão nomeadas <*prefixo*>-*n*, onde *n* é o identificador da estação.  
  
7.  Insira um nome de usuário e senha a ser usada para a conta de administrador local para o modelo. Em um domínio, insira as credenciais para uma conta de domínio que será adicionada ao grupo Administradores local. Essa conta pode ser usada para fazer logon no modelo e todas as estações da área de trabalho virtuais é criada no modelo.  
  
8. Clique em **Okey**e aguarde a conclusão da criação do modelo.  
  
9. O novo modelo será listado na **áreas de trabalho virtuais** guia. O modelo será desativado.  
  
A próxima etapa é configurar o modelo com o software e configuração que deseja nas áreas de trabalho virtuais. Você deve fazer isso antes de criar qualquer áreas de trabalho virtuais do modelo.  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>Para personalizar um modelo de área de trabalho virtual  
  
1.  Faça logon no sistema de operacional do host MultiPoint server com uma conta de administrador local ou em um domínio, com uma conta de domínio no grupo de administradores local.  
  
2.  Dos **iniciar** tela, abra o MultiPoint Manager.  
  
3.  Clique o **áreas de trabalho virtuais** guia.  
  
4.  Selecione o modelo que você deseja personalizar, clique em **personalizar modelo**e, em seguida, clique em **Okey**.  
  
    > [!NOTE]  
    > Somente os modelos que não foram usados para criar estações de área de trabalho virtual estão disponíveis. Se você quiser atualizar um modelo que já está em uso, você deve fazer uma cópia do modelo usando o **Importar modelo** tarefa, descrita posteriormente, na [copiar um modelo de área de trabalho virtual existente](#BKMK_CopyExiistingVirtualDesktopTemplate).  
  
    O modelo é aberto em um Hyper-V **VM conectar** janela e logon automático é executada usando a conta de administrador interna.  
  
5.  Neste momento pode instalar aplicativos e atualizações de software, alterar as configurações e atualizar o perfil de administrador. Todas as alterações feitas ao perfil de administrador interna do modelo serão copiadas para o perfil de usuário padrão nas estações de área de trabalho virtuais que são criados a partir do modelo.  
  
    Se você estiver se conectando suas estações de um domínio, recomendamos que você crie uma conta de usuário local e adicioná-lo ao grupo Administradores local durante a personalização.  
  
    > [!NOTE]  
    > Se o sistema for reiniciado enquanto um modelo que está sendo personalizado, o logon automático usando a conta interna administrador poderá falhar após a reinicialização do sistema. Para contornar esse problema, manualmente, faça logon usando a conta de administrador local que você criou, alterar a senha da conta de administrador interna, faça logoff e, em seguida, fazer logon novamente usando a conta interna administrador e a nova senha. (Você precisará excluir o perfil que foi criado quando você logon usando a conta de administrador local.)  
  
6.  Depois de concluir a configuração de seu sistema, clique duas vezes o **CompleteCustomization** atalho na área de trabalho do administrador para executar o Sysprep e desligue o modelo. Durante a personalização, a ferramenta Sysprep remove todas as informações de sistema exclusivo para preparar a instalação do Windows a ser espelhado.  
  
### <a name="BKMK_CreateVirtualDesktopsfromTemplate"></a>Criar áreas de trabalho de máquina virtual do modelo  
Com o modelo de área de trabalho virtual configurado da maneira que você deseja que suas áreas de trabalho, você está pronto para começar a criar áreas de trabalho virtuais. Uma área de trabalho virtual será criada para cada estação que está anexada ao computador do MultiPoint Server. Na próxima vez que um usuário faz logon em uma estação, ele poderá ver a área de trabalho virtual em vez de com base em sessão de área de trabalho que foi exibida antes.  
  
> [!NOTE]  
> Esse procedimento só funciona quando o servidor do MultiPoint está no *modo de estação*. Se o sistema está no *modo de console*, você pode alternar para modo de estação do MultiPoint Manager. Se você estiver usando as configurações de MultiPoint de padrão, você também pode iniciar o modo de estação, reinicie o computador. Por padrão, o computador do MultiPoint Server sempre inicia no modo de estação  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>Para criar áreas de trabalho virtuais para suas estações  
  
1.  Faça logon no servidor de uma estação remota (por exemplo, de um computador Windows por meio de Conexão de área de trabalho remota) Windows MultiPoint usando um administrador local da conta ou, em um domínio, um domínio de conta no grupo de administradores local.  
  
    > [!NOTE]  
    > Como alternativa, você pode fazer logon no servidor usando uma estação de local. No entanto, quando você cria uma área de trabalho virtual estação, você precisará fazer logoff a estação que você usou para criar a área de trabalho virtual para se conectar a outra estação na nova área de trabalho virtual.  
  
2.  Dos **iniciar** tela, abra o MultiPoint Manager.  
  
3.  Se o computador estiver no modo de console, alterne para modo de estação:  
  
    1.  Sobre o **Home** , clique em **alterne para modo de estação**.  
  
    2.  Quando o computador for reiniciado, faça logon como administrador.  
  
4.  Clique o **áreas de trabalho virtuais** guia.  
  
5.  Selecione o modelo de área de trabalho virtual que você deseja usar com as estações, clique em **criar estações de área de trabalho virtual**e, em seguida, clique em **Okey**.  
  
Quando a tarefa for concluída, cada estação local se conectar a um desktop virtual com base em máquina virtual.  
  
> [!NOTE]  
> Se uma conta de usuário é conectada a qualquer uma das estações locais, você precisará fazer logoff da sessão para obter a estação de se conectar a uma das áreas de trabalho virtual estação recém-criado.  
  
### <a name="BKMK_CopyExiistingVirtualDesktopTemplate"></a>Copiar um modelo de área de trabalho virtual existente  
Use o procedimento a seguir para criar uma cópia de um modelo existente de área de trabalho virtual que você pode personalizar e usar. Isso pode ser útil nas seguintes situações:  
  
-   Para copiar um modelo de mestre de um compartilhamento de rede em um computador de host do MultiPoint Server, para que as estações da área de trabalho virtual podem ser criadas usando o modelo mestre.  
  
-   Para criar uma cópia de um modelo que está atualmente em uso, para que você possa fazer personalizações adicionais.  
  
##### <a name="to-import-a-virtual-desktop-template"></a>Para importar um modelo de área de trabalho virtual  
  
1.  Faça logon no MultiPoint server como um administrador.  
  
2.  Dos **iniciar** tela, abra o MultiPoint Manager.  
  
3.  Clique o **áreas de trabalho virtuais** guia.  
  
4.  Clique em **Importar modelo de área de trabalho virtual**e use **procurar** para selecionar o arquivo. vhd (modelo) que você deseja importar. Quando você importa um modelo, é feita uma cópia do VHD original. Por padrão, o MultiPoint Services armazena arquivos. vhd na unidade c:\\os usuários\\públicas\\documentos\\Hyper\-V\\discos rígidos virtuais\\ pasta.  
  
5.  Digite um prefixo para o novo modelo e, em seguida, clique em **Okey**.  
  
6.  Se você estiver fazendo mais personalizações para um modelo local, você pode alterar o nome do prefixo ao incrementar um número de versão no final do prefixo. Ou, se você estiver importando um modelo mestre, você talvez queira adicionar a versão do modelo mestre ao final do nome do prefixo do padrão.  
  
7.  Quando a tarefa for concluída, você pode personalizar o modelo ou usá-lo como ele é criar estações.  
