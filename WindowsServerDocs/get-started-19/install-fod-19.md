---
title: Recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD)
description: Como instalar recursos do Windows Server sob demanda
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 05/29/2019
ms.openlocfilehash: e76b7862549814d5453717c40cec45e341141d7a
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66308599"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>Recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD)

> Aplica-se a: Windows Server canal semestral do Windows Server de 2019

O **recurso de compatibilidade de aplicativo do Server Core sob demanda** é um pacote de recurso opcional que pode ser adicionado para instalações do Server Core do Windows Server 2019 ou canal semestral do Windows Server, a qualquer momento.

Para obter mais informações sobre os recursos na demanda FOD (), consulte [recursos sob demanda](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

## <a name="why-install-the-app-compatibility-fod"></a>Por que instalar o FOD de compatibilidade de aplicativo?

Compatibilidade de aplicativo, um recurso sob demanda para Server Core, melhora significativamente a compatibilidade de aplicativo da opção de instalação Server Core do Windows, incluindo um subconjunto de binários e pacotes do Windows Server com experiência Desktop, sem adicionar a Ambiente de gráfico de experiência de área de trabalho do Windows Server. Este pacote opcional está disponível em um ISO separado, ou do Windows Update, mas só pode ser adicionado às imagens e instalações Server Core do Windows.

Os dois valores principais que fornece o FOD de compatibilidade de aplicativo são:

- Aumenta a compatibilidade do Server Core para aplicativos de servidor que já estão no mercado ou já foram desenvolvidos por organizações e implantados.
- Ajuda com a prestação de compatibilidade de aplicativo aumentada de ferramentas de software usados em cenários de depuração e solução de problemas agudo e componentes do sistema operacional.

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

        -   Em uma sessão do PowerShell com privilégios elevados: 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   Para executar o Gerenciador de Cluster de Failover, digite **cluadmin** no prompt de comando.

Servidores que executam o Windows Server, versão 1903 e versões posterior também dão suporte os seguintes componentes:

- Gerenciador do Hyper-V (virtmgmt.msc)
- Agendador de tarefas (taskschd)

## <a name="installing-the-app-compatibility-fod"></a>Instalando a compatibilidade de aplicativo FOD

O FOD de compatibilidade de aplicativo só pode ser instalado no Server Core. Não tente adicionar o FOD de compatibilidade de aplicativo Server Core para uma instalação do Windows Server do Windows Server com experiência Desktop. Os pacotes opcionais FOD ISO mesmos podem ser usados para instalações do Server Core do Windows Server 2019 ou instalações de canal semestral do Windows Server.

1. Se o servidor pode se conectar ao Windows Update, tudo o que você precisa fazer é executar o comando a seguir em uma sessão do PowerShell com privilégios elevados e, em seguida, reiniciar o Windows Server após o comando a execução:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. Se o servidor não pode se conectar ao Windows Update, em vez disso baixar os pacotes opcionais do servidor FOD ISO e copie o ISO em uma pasta compartilhada em sua rede local:

   - Se você tiver uma licença de volume que você pode baixar o arquivo de imagem ISO do FOD Server no mesmo portal onde o arquivo de imagem ISO de sistema operacional é obtido: [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - O arquivo de imagem ISO do servidor FOD também está disponível na [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) ou nos [portal do Visual Studio](https://visualstudio.microsoft.com) para assinantes.

3. Entrar com uma conta de administrador no computador Server Core que está conectado à sua rede local e que você deseja adicionar o FOD para.

4. Use **net use**, ou algum outro método, para conectar-se para o local da ISO FOD.

5. Copie o ISO FOD para uma pasta local de sua escolha.

6. Monte o ISO FOD usando o seguinte comando em uma sessão do PowerShell com privilégios elevados:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. Execute o seguinte comando:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. Após a barra de progresso, reinicie o sistema operacional.

 Para obter mais informações sobre os comandos do DISM, consulte [usar DISM no Windows PowerShell](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Para adicionar, opcionalmente, o Internet Explorer 11 para Server Core (depois de adicionar o FOD de compatibilidade de aplicativo Server Core)

 >[!NOTE]  
   > O Server Core aplicativo compatibilidade FOD é necessária para a adição do Internet Explorer 11, mas o Internet Explorer 11 não é necessário adicionar o FOD de compatibilidade de aplicativo Server Core.

1. Entre como administrador em computador Server Core que tem o FOD de compatibilidade de aplicativo já adicionado e o pacote opcional Server FOD que ISO copiado localmente.

2. Inicie o PowerShell, inserindo **powershell.exe** em um prompt de comando.

3. Monte o ISO FOD usando o seguinte comando:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. Execute o seguinte comando, usando o `$package_path` variável inserir o caminho para o arquivo cab do Internet Explorer:

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. Após a barra de progresso, reinicie o sistema operacional.

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notas de versão e sugestões para o pacote opcional FOD de compatibilidade de aplicativo do Server Core e o Internet Explorer 11

> [!IMPORTANT]
> FODs instalados no Windows Server, versão 1809 não permanecerão em vigor após uma atualização in-loco para o Windows Server, versão 1903, portanto, é necessário instalá-los novamente após a atualização. Como alternativa, você pode adicionar FODs para a nova fonte de instalação do Windows Server antes da atualização. Isso garante que a nova versão de qualquer FODs estão presentes após a conclusão da atualização. Para obter mais informações, consulte o [adicionando recursos e pacotes opcionais em uma imagem offline do Server Core do WIM](install-fod-19.md#add-capabilities).

- **Importante:** Leia as notas de versão do Windows Server 2019 de problemas, considerações e diretrizes antes de prosseguir com a instalação e uso do pacote opcional FOD de compatibilidade de aplicativo do Server Core e o Internet Explorer 11.

- É possível encontrar cintilação com a experiência de console do Server Core, ao adicionar o FOD de compatibilidade do aplicativo depois de usar o Windows Update para instalar as atualizações cumulativas.  Esse problema é resolvido em dezembro de 2018 atualiza.  Para obter mais informações e etapas de resolução, consulte [4481610 de artigo da Base de dados de Conhecimento: Tela pisca depois de instalar FOD de compatibilidade de aplicativo do Server Core no Server Core do Windows Server 2019](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Após a instalação do FOD de compatibilidade do aplicativo e a reinicialização do servidor, a cor de quadro de janela de console de comando será alterado para uma tonalidade diferente de azul.

- Se você optar por instalar também o pacote opcional do Internet Explorer 11, observe que não é compatível clicando duas vezes para abrir arquivos. htm salvo localmente. No entanto, você pode **com o botão direito** e escolha **abrir com o IE**, ou você pode abri-lo diretamente do Internet Explorer **arquivo** -> **abrir**.

- Para melhorar ainda mais a compatibilidade de aplicativo do Server Core com o FOD de compatibilidade do aplicativo, o Console de gerenciamento do IIS foi adicionado ao Server Core como um componente opcional.  No entanto, é absolutamente necessário primeiro adicionar o FOD de compatibilidade de aplicativo para usar o Console de gerenciamento do IIS. Console de gerenciamento do IIS se baseia no Console de gerenciamento Microsoft (mmc.exe), que só está disponível em Server Core com a adição de FOD de compatibilidade do aplicativo.  Usar o Powershell [ **Install-WindowsFeature** ](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) para adicionar o Console de gerenciamento do IIS.

- Como um ponto geral de orientação, quando a instalação de aplicativos no servidor principal (com ou sem esses pacotes opcionais) ele às vezes, é necessário usar as instruções e opções de instalação silenciosa. 
    
 - Por exemplo, o SQL Server Management Studio para SQL Server 2016 e SQL Server 2017 podem ser instalado no Server Core e é totalmente funcional, quando o FOD de compatibilidade do aplicativo está presente.  Consulte, [instalar o SQL Server do Prompt de comando](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Se o SQL Server Management Studio não for desejado, em seguida, não é necessário instalar o Server Core aplicativo compatibilidade FOD.  Consulte, [instalar o SQL Server no Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> Adicionando recursos e pacotes opcionais em uma imagem do Server Core do WIM offline

1. Baixe os arquivos de imagem do Windows Server e o servidor FOD ISO em uma pasta local em um computador Windows.

   - Se você tiver uma licença de volume, você pode baixar os arquivos de imagem do Windows Server e o servidor FOD ISO do [Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - O arquivo de imagem ISO do servidor FOD também está disponível para versões de canal de manutenção em longo prazo na [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) ou nos [portal do Visual Studio](https://visualstudio.microsoft.com) para assinantes.

2. Abra uma sessão do PowerShell como administrador e, em seguida, use os seguintes comandos para montar os arquivos de imagem como unidades:

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. Cópia do conteúdo do arquivo ISO do Windows Server em uma pasta local (por exemplo, *C:\SetupFiles\WindowsServer*).

4. Obtenha o nome de imagem que você deseja modificar no arquivo Install. wim usando o comando a seguir.<br>
Use o `$install_wim_path` variável inserir o caminho para o arquivo Install. wim, localizado dentro da pasta \Sources do arquivo ISO.

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. Montar o arquivo Install. wim em uma nova pasta, usando o seguinte comando, substituindo os valores de variáveis de exemplo pelos seus próprios e reutilizar o `$install_wim_path` variável do comando anterior.<br>

   - `$image_name` -Insira o nome da imagem que você deseja montar.
   - `$mount_folder variable` -Especifique a pasta a ser usada ao acessar o conteúdo do arquivo Install. wim.

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. Adicione recursos e pacotes que você deseja para a imagem Install. wim montada usando os seguintes comandos, substituindo os valores de variáveis de exemplo pelos seus próprios.<br>

   - `$capability_name` -Especifique o nome da capacidade de instalar (nesse caso, a capacidade de AppCompatibility).
   - `$package_path` -Especifique o caminho para o pacote de instalação (nesse caso, Internet Explorer).
   - `$fod_drive` -Especifique a letra da unidade da imagem montada do FOD do servidor.

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. Desmontar e confirmar as alterações para o arquivo Install. wim usando o comando a seguir, que usa o `$mount_folder` variável dos dois comandos anteriores:

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

Agora você pode atualizar seu servidor executando setup.exe da pasta que você criou para os arquivos de instalação do Windows Server (neste exemplo: *C:\SetupFiles\WindowsServer*). Agora, essa pasta contém arquivos de instalação do Windows Server com os recursos adicionais e opcionais pacotes incluídos.