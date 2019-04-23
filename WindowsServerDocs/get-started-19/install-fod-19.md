---
title: Recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD)
description: Como instalar recursos do Windows Server sob demanda
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: b8211ace56aa6565295a15adce26a8dfbc98e1e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866107"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD)

> Aplica-se ao Windows Server 2019 e Windows Server, versão 1809

O **recurso de compatibilidade de aplicativo do Server Core sob demanda** é um pacote de recurso opcional que pode ser adicionado para instalações de 2019 Server Core do Windows Server ou Windows Server, versão 1809, a qualquer momento.

Para obter mais informações sobre os recursos na demanda FOD (), consulte [recursos sob demanda](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).


## <a name="why-install-the-app-compatibility-fod"></a>Por que instalar o FOD de compatibilidade de aplicativo? 

Compatibilidade de aplicativo, um recurso sob demanda para Server Core, melhora significativamente a compatibilidade de aplicativo da opção de instalação Server Core do Windows, incluindo um subconjunto de binários e pacotes do Windows Server com experiência Desktop, sem adicionar a Ambiente de gráfico de experiência de área de trabalho do Windows Server. Este pacote opcional está disponível em um ISO separado, ou do Windows Update, mas só pode ser adicionado às imagens e instalações Server Core do Windows.

Os dois valores principais que fornece o FOD de compatibilidade de aplicativo são:

1.  Aumenta a compatibilidade do Server Core para aplicativos de servidor que já estão no mercado ou já foram desenvolvidos por organizações e implantados.

2.  Ajuda com a prestação de compatibilidade de aplicativo aumentada de ferramentas de software usados em cenários de depuração e solução de problemas agudo e componentes do sistema operacional.

Componentes do sistema operacional que estão disponíveis como parte do que o Server Core aplicativo compatibilidade FOD incluem:

-   Microsoft Management Console (mmc.exe)

-   Visualizador de eventos (eventvwr. msc)

-   Monitor de desempenho (PerfMon.exe)

-   Monitor de recursos (Resmon.exe)

-   Gerenciador de dispositivos (devmgmt. msc)

-   Explorador de arquivos (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gerenciamento de disco (Diskmgmt. msc)

-   Gerenciador de Cluster de failover (CluAdmin)

    -   Requer a adição do recurso servidor do Windows de Clustering de Failover pela primeira vez.

        -   Use o Cmdlet do Powershell para adicionar, `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   Para executar o Gerenciador de Cluster de Failover, digite **cluadmin** no prompt de comando.

## <a name="installing-the-app-compatibility-fod"></a>Instalando a compatibilidade de aplicativo FOD

 >[!IMPORTANT] 
   >O FOD de compatibilidade de aplicativo só pode ser instalado no Server Core. Não tente adicionar o FOD de compatibilidade de aplicativo Server Core para uma instalação do Windows Server do Windows Server com experiência Desktop.

### <a name="to-add-the-server-core-app-compatibility-feature-on-demand-fod-to-a-running-instance-of-server-core"></a>Para adicionar o recurso de compatibilidade de aplicativos do núcleo de servidor sob demanda (FOD) para executar uma instância do Server Core

 >[!NOTE] 
   > Este procedimento usa Deployment Image Servicing and Management (DISM.exe), uma ferramenta de linha de comando. Para obter mais informações sobre os comandos do DISM, consulte [opções de linha de comando DISM recursos de pacote de manutenção](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options).

>[!NOTE] 
   > Os pacotes opcionais FOD ISO mesmos podem ser usados para instalações do Server Core do Windows Server de 2019, ou Windows Server, versão 1809, instalações.

>[!NOTE] 
   > Se seu computador ou máquina virtual que executa o Server Core é capaz de se conectar ao Windows Update, as etapas 1 a 7 abaixo pode ser ignorada. Mas não se esqueça de deixar de fora /Source e /LimitAccess do comando DISM na etapa 8.

1. Baixar os pacotes opcionais do servidor FOD ISO e copie o ISO em uma pasta compartilhada em sua rede local:

 - Se você tiver uma licença de volume que você pode baixar o arquivo de imagem ISO do FOD Server no mesmo portal onde o arquivo de imagem ISO de sistema operacional é obtido: [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
 - O arquivo de imagem ISO do servidor FOD também está disponível na [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) ou nos [portal do Visual Studio](https://visualstudio.microsoft.com) para assinantes.


2. Entre como administrador no computador Server Core que está conectado à sua rede local e que você deseja adicionar o FOD para.

3. Use **net use**, ou algum outro método, para conectar-se para o local da ISO FOD.

4. Copie o ISO FOD para uma pasta local de sua escolha.

5. Inicie o PowerShell, inserindo **powershell.exe** em um prompt de comando.

6. Monte o ISO FoD usando o seguinte comando:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. Tipo de **sair** para sair do PowerShell.

8.  Execute o seguinte comando:

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  Após a barra de progresso, reinicie o sistema operacional.

### <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Para adicionar, opcionalmente, o Internet Explorer 11 para Server Core (depois de adicionar o FOD de compatibilidade de aplicativo Server Core)

 >[!NOTE]  
   > O Server Core aplicativo compatibilidade FOD é necessária para a adição do Internet Explorer 11, mas o Internet Explorer 11 não é necessário adicionar o FOD de compatibilidade de aplicativo Server Core.

1.  Entre como administrador em computador Server Core que tem o FOD de compatibilidade de aplicativo já adicionado e o pacote opcional Server FOD que ISO copiado localmente.

2.  Inicie o PowerShell, inserindo **powershell.exe** em um prompt de comando.

3.  Monte o ISO FoD usando o seguinte comando:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  Tipo de **sair** para sair do PowerShell.


5.  Execute o seguinte comando:

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  Após a barra de progresso, reinicie o sistema operacional.

 
#### <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notas de versão e sugestões para o pacote opcional FOD de compatibilidade de aplicativo do Server Core e o Internet Explorer 11

- **Importante:** , leia as notas de versão do Windows Server 2019 de problemas, considerações e diretrizes antes de prosseguir com a instalação e uso do pacote opcional FOD de compatibilidade de aplicativo do Server Core e o Internet Explorer 11.
 
 >[!NOTE] 
   > É possível encontrar cintilação com a experiência de console do Server Core, ao adicionar o FOD de compatibilidade do aplicativo depois de usar o Windows Update para instalar as atualizações cumulativas.  Esse problema é resolvido em dezembro de 2018 atualiza.  Para obter mais informações e etapas de resolução, consulte [4481610 de artigo da Base de dados de Conhecimento: Tela pisca depois de instalar FOD de compatibilidade de aplicativo do Server Core no Server Core do Windows Server 2019](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Após a instalação do FOD de compatibilidade do aplicativo e a reinicialização do servidor, a cor de quadro de janela de console de comando será alterado para uma tonalidade diferente de azul.

- Se você optar por instalar também o pacote opcional do Internet Explorer 11, observe que não é compatível clicando duas vezes para abrir arquivos. htm salvo localmente. No entanto, você pode **com o botão direito** e escolha **abrir com o IE**, ou você pode abri-lo diretamente do Internet Explorer **arquivo** -> **abrir**. 

- Para melhorar ainda mais a compatibilidade de aplicativo do Server Core com o FOD de compatibilidade do aplicativo, o Console de gerenciamento do IIS foi adicionado ao Server Core como um componente opcional.  No entanto, é absolutamente necessário primeiro adicionar o FOD de compatibilidade de aplicativo para usar o Console de gerenciamento do IIS. Console de gerenciamento do IIS se baseia no Console de gerenciamento Microsoft (mmc.exe), que só está disponível em Server Core com a adição de FOD de compatibilidade do aplicativo.  Usar o Powershell [ **Install-WindowsFeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) para adicionar o Console de gerenciamento do IIS.

- Como um ponto geral de orientação, quando a instalação de aplicativos no servidor principal (com ou sem esses pacotes opcionais) ele às vezes, é necessário usar as instruções e opções de instalação silenciosa. 
    
 - Por exemplo, o SQL Server Management Studio para SQL Server 2016 e SQL Server 2017 podem ser instalado no Server Core e é totalmente funcional, quando o FOD de compatibilidade do aplicativo está presente.  Consulte, [instalar o SQL Server do Prompt de comando](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Se o SQL Server Management Studio não for desejado, em seguida, não é necessário instalar o Server Core aplicativo compatibilidade FOD.  Consulte, [instalar o SQL Server no Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

