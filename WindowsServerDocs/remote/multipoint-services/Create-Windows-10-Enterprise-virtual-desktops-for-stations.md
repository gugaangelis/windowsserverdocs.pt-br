---
title: Criar áreas de trabalho virtuais do Windows 10 Enterprise para estações
description: Saiba como criar desktops do Windows Server 2016 para estação
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
ms.openlocfilehash: e68412808e037b788d5b25c1c2c7b14253e40ea6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871734"
---
# <a name="create-windows-10-enterprise-virtual-desktops-for-stations"></a>Criar áreas de trabalho virtuais do Windows 10 Enterprise para estações
Essa configuração opcional nos serviços do MultiPoint destina-se principalmente a situações em que um aplicativo essencial requer sua própria instância de um sistema operacional cliente para cada usuário. Os exemplos incluem aplicativos que não podem ser instalados no Windows Server e aplicativos que não executarão várias instâncias no mesmo computador host.  
  
> [!NOTE]  
> Essas áreas de trabalho virtuais, também conhecidas como VDI, são muito mais intensivas em recursos do que as sessões de área de trabalho dos serviços do MultiPoint padrão, portanto, recomendamos que você use sessões padrão de serviços do MultiPoint quando possível.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Para se preparar para criar áreas de trabalho virtuais de estação, verifique se o sistema de serviços do MultiPoint atende aos seguintes requisitos:      
  
|Hardware|Requisitos|         |
|------------|----------------|----------------| 
|CPU (multimídia)|1 núcleo ou thread por máquina virtual|  
|Unidade de estado sólido (SSD)|Capacidade > = 20 GB por estação + 40 GB para o sistema operacional do host de serviços do MultiPoint<br /><br />IOPS de\/leitura e gravação aleatória > = 3K por estação|  
|RAM|2GB por estação + 2GB para o sistema operacional Windows MultiPoint Server host|  
|Gráficos|DX11|  
|BIOS|Configuração de CPU do BIOS configurada para habilitar a virtualização – SLAT (conversão de endereços de segundo nível)|  
  
-   **Estações** – configure as estações para o sistema MultiPoint Services. Para obter mais informações, consulte [anexar estações adicionais aos serviços do MultiPoint](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
-   **Domínio** -em um ambiente de domínio, o computador Windows MultiPoint Server foi adicionado ao domínio e um usuário de domínio foi adicionado ao grupo local de administradores no sistema operacional host dos serviços do MultiPoint.  
  
## <a name="procedures"></a>Procedimentos  
Use os procedimentos a seguir para:  
  
-   [Criar um modelo para áreas de trabalho virtuais](#create-a-template-for-virtual-desktops)  
  
-   [Criar áreas de trabalho virtuais do modelo](#create-virtual-machine-desktops-from-the-template)  
  
-   [Copiar um modelo de área de trabalho virtual existente](#copy-an-existing-virtual-desktop-template)  
  
### <a name="create-a-template-for-virtual-desktops"></a>Criar um modelo para áreas de trabalho virtuais  
Antes de criar um modelo para suas áreas de trabalho virtuais, você deve habilitar o recurso de área de trabalho virtual no MultiPoint Server.  
  
##### <a name="to-enable-the-virtual-desktop-feature"></a>Para habilitar o recurso de área de trabalho virtual  
  
1.  Faça logon no sistema operacional do host do MultiPoint Server com uma conta de administrador local ou, em um domínio, com uma conta de domínio que seja membro do grupo Administradores local.  
  
2.  Na tela **Iniciar** , abra o Gerenciador do MultiPoint.  
  
3.  Clique na guia **áreas de trabalho virtuais** , clique em **habilitar áreas de trabalho virtuais**e, em seguida, clique em **OK**e aguarde a reinicialização do sistema.  
  
A próxima etapa é criar um modelo de área de trabalho virtual. Literalmente, você está criando um arquivo de VHD (disco rígido virtual) que pode ser usado como modelo para criar áreas de trabalho virtuais de estação para o Gerenciador do MultiPoint. Você pode usar a mídia de instalação física para Windows ou um. Arquivo de imagem ISO para como fonte para o modelo. Você também pode usar um. VHD da instalação do Windows. Observe que, para usar um disco de instalação física, você deve inserir o disco antes de iniciar o assistente.  
  
##### <a name="to-create-a-virtual-desktop-template"></a>Para criar um modelo de área de trabalho virtual  
  
1.  Faça logon no sistema operacional do host do MultiPoint Server com uma conta de administrador local ou, no domínio, uma conta de domínio que seja membro do grupo Administradores local.  
  
2.  Na tela **Iniciar** , abra o Gerenciador do MultiPoint.  
  
3.  Clique na guia **áreas de trabalho virtuais** .  
  
4.   Copie um arquivo. ISO do Windows 10 Enterprise para o SSD local.  
  
5.  Na guia áreas de trabalho virtuais, clique em **criar modelo de área de trabalho virtual.**   
  
6.  Em **prefixo**, insira um prefixo a ser usado para identificar o modelo e as áreas de trabalho virtuais criadas com o modelo. O prefixo padrão é o nome do computador host.  
  
    O prefixo é usado para nomear o modelo e as estações da área de trabalho virtual. O modelo será <*prefixo*>-t. As estações da área de trabalho virtual serão nomeadas <*prefixo*>-*n*, em que *n* é o identificador da estação.  
  
7.  Insira um nome de usuário e senha a serem usados para a conta de administrador local para o modelo. Em um domínio, insira as credenciais para uma conta de domínio que será adicionada ao grupo de administradores locais. Essa conta pode ser usada para fazer logon no modelo e em todas as estações de área de trabalho virtual criadas com base no modelo.  
  
8. Clique em **OK**e aguarde a conclusão da criação do modelo.  
  
9. O novo modelo será listado na guia **áreas de trabalho virtuais** . O modelo será desativado.  
  
A próxima etapa é configurar o modelo com o software e a configuração que você deseja nas áreas de trabalho virtuais. Você deve fazer isso antes de criar qualquer área de trabalho virtual a partir do modelo.  
  
##### <a name="to-customize-a-virtual-desktop-template"></a>Para personalizar um modelo de área de trabalho virtual  
  
1.  Faça logon no sistema operacional do host do MultiPoint Server com uma conta de administrador local ou, em um domínio, com uma conta de domínio no grupo Administradores local.  
  
2.  Na tela **Iniciar** , abra o Gerenciador do MultiPoint.  
  
3.  Clique na guia **áreas de trabalho virtuais** .  
  
4.  Selecione o modelo que você deseja personalizar, clique em **Personalizar modelo**e, em seguida, clique em **OK**.  
  
    > [!NOTE]  
    > Somente os modelos que não foram usados para criar estações de área de trabalho virtuais estão disponíveis. Se desejar atualizar um modelo que já está em uso, você deverá fazer uma cópia do modelo usando a tarefa **Importar modelo** , descrita posteriormente, em [copiar um modelo de área de trabalho virtual existente](#copy-an-existing-virtual-desktop-template).  
  
    O modelo é aberto em uma janela de **conexão de VM** do Hyper-V e o logon automático é executado usando a conta de administrador interno.  
  
5.  Neste ponto, você pode instalar aplicativos e atualizações de software, alterar as configurações e atualizar o perfil de administrador. Todas as alterações feitas no perfil de administrador interno do modelo serão copiadas para o perfil de usuário padrão nas estações de área de trabalho virtual criadas com base no modelo.  
  
    Se você estiver conectando suas estações em um domínio, recomendamos que você crie uma conta de usuário local e adicione-a ao grupo local de administradores durante a personalização.  
  
    > [!NOTE]  
    > Se o sistema for reiniciado enquanto um modelo estiver sendo personalizado, o logon automático usando a conta de administrador interno poderá falhar após a reinicialização do sistema. Para resolver esse problema, faça logon manualmente usando a conta de administrador local que você criou, altere a senha da conta de administrador interna, faça logoff e, em seguida, faça logon novamente usando a conta de administrador interna e a nova senha. (Será necessário excluir o perfil que foi criado quando você fez logon usando a conta de administrador local.)  
  
6.  Depois de concluir a configuração do sistema, clique duas vezes no atalho **CompleteCustomization** na área de trabalho do administrador para executar o Sysprep e, em seguida, desligue o modelo. Durante a personalização, a ferramenta Sysprep remove todas as informações exclusivas do sistema para preparar a imagem da instalação do Windows.  
  
### <a name="create-virtual-machine-desktops-from-the-template"></a>Criar áreas de trabalho de máquina virtual do modelo  
Com o modelo de área de trabalho virtual configurado da maneira como você deseja que seus desktops sejam, você estará pronto para começar a criar áreas de trabalho virtuais. Uma área de trabalho virtual será criada para cada estação anexada ao computador do MultiPoint Server. Na próxima vez que um usuário fizer logon em uma estação, ele verá a área de trabalho virtual em vez da área de trabalho baseada em sessão que foi exibida antes.  
  
> [!NOTE]  
> Este procedimento só funciona quando o MultiPoint Server está no *modo de estação*. Se o sistema estiver no *modo de console*, você poderá alternar para o modo de estação do Gerenciador do MultiPoint. Se você estiver usando as configurações padrão do MultiPoint, também poderá iniciar o modo de estação reiniciando o computador. Por padrão, o computador do MultiPoint Server sempre inicia no modo de estação  
  
##### <a name="to-create-virtual-desktops-for-your-stations"></a>Para criar áreas de trabalho virtuais para suas estações  
  
1.  Faça logon no Windows MultiPoint Server de uma estação remota (por exemplo, de um computador com Windows usando Conexão de Área de Trabalho Remota) usando uma conta de administrador local ou, em um domínio, uma conta de domínio no grupo local de administradores.  
  
    > [!NOTE]  
    > Como alternativa, você pode fazer logon no servidor usando uma estação local. No entanto, ao criar uma área de trabalho virtual de estação, você precisará fazer logoff da estação usada para criar a área de trabalho virtual a fim de conectar a outra estação à nova área de trabalho virtual.  
  
2.  Na tela **Iniciar** , abra o Gerenciador do MultiPoint.  
  
3.  Se o computador estiver no modo de console, alterne para o modo de estação:  
  
    1.  Na guia **início** , clique em **alternar para o modo de estação**.  
  
    2.  Quando o computador for reiniciado, faça logon como administrador.  
  
4.  Clique na guia **áreas de trabalho virtuais** .  
  
5.  Selecione o modelo de área de trabalho virtual que você deseja usar com as estações, clique em **criar estações de área de trabalho virtual**e em **OK**.  
  
Quando a tarefa for concluída, cada estação local será conectada a uma área de trabalho virtual baseada em máquina virtual.  
  
> [!NOTE]  
> Se uma conta de usuário estiver conectada a qualquer uma das estações locais, você precisará fazer logoff da sessão para obter a estação para se conectar a uma das áreas de trabalho virtuais da estação recém-criada.  
  
### <a name="copy-an-existing-virtual-desktop-template"></a>Copiar um modelo de área de trabalho virtual existente  
Use o procedimento a seguir para criar uma cópia de um modelo de área de trabalho virtual existente que você pode personalizar e usar. Isso pode ser útil nas seguintes situações:  
  
-   Para copiar um modelo mestre de um compartilhamento de rede em um computador host do MultiPoint Server para que as estações da área de trabalho virtual possam ser criadas a partir do modelo mestre.  
  
-   Para criar uma cópia de um modelo que está em uso no momento para que você possa fazer personalizações adicionais.  
  
##### <a name="to-import-a-virtual-desktop-template"></a>Para importar um modelo de área de trabalho virtual  
  
1.  Faça logon no MultiPoint Server como administrador.  
  
2.  Na tela **Iniciar** , abra o Gerenciador do MultiPoint.  
  
3.  Clique na guia **áreas de trabalho virtuais** .  
  
4.  Clique em **Importar modelo de área de trabalho virtual**e use **procurar** para selecionar o arquivo. VHD (modelo) que você deseja importar. Quando você importa um modelo, é feita uma cópia do. vhd original. Por padrão, os serviços do MultiPoint armazenam arquivos. vhd na\\pasta\\C\\: Users\\Public Document\\discos\\ rígidos virtuais do Hyper\-V.  
  
5.  Insira um prefixo para o novo modelo e clique em **OK**.  
  
6.  Se você estiver fazendo mais personalizações em um modelo local, poderá alterar o nome do prefixo incrementando um número de versão no final do prefixo. Ou, se você estiver importando um modelo mestre, talvez queira adicionar a versão do modelo mestre ao final do nome do prefixo padrão.  
  
7.  Quando a tarefa for concluída, você poderá personalizar o modelo ou usá-lo como é para criar estações.  
