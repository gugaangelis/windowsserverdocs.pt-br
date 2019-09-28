---
title: FOD (recurso sob demanda) de compatibilidade de aplicativos do Server Core
description: Como instalar recursos sob demanda do Windows Server
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
ms.assetid: 99f7daa4-30ce-4d13-be65-0a4d55cca754
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 9ba0303d1eebc2138db7c5f1428ce920b0cb8af0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360824"
---
# <a name="server-core-app-compatibility-feature-on-demand-fod"></a>FOD (recurso sob demanda) de compatibilidade de aplicativos do Server Core

> Aplica-se a: Windows Server 2019, Canal Semestral do Windows Server

O **Recurso sob demanda de compatibilidade de aplicativos do Server Core** é um pacote de recursos opcional que pode ser adicionado a instalações Server Core do Windows Server 2019 ou ao Canal Semestral do Windows Server a qualquer momento.

Para obter mais informações sobre o FOD (Recursos sob demanda), confira [Recursos sob demanda](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

## <a name="why-install-the-app-compatibility-fod"></a>Por que instalar o FOD de compatibilidade de aplicativos?

A compatibilidade de aplicativos, um recurso sob demanda para Server Core, melhora significativamente a compatibilidade de aplicativos da opção de instalação do Windows Server Core incluindo um subconjunto de binários e pacotes do Windows Server com a Experiência Desktop, sem adicionar o ambiente gráfico da Experiência Desktop do Windows Server. Esse pacote opcional está disponível em um ISO separado ou no Windows Update, mas pode ser adicionado somente às imagens e instalações do Windows Server Core.

Os dois valores principais que o FOD de compatibilidade de aplicativos fornece são:

- Aumenta a compatibilidade do Server Core para aplicativos de servidor que já estão no mercado ou já foram desenvolvidos por organizações e implantados.
- Ajuda fornecendo componentes de sistema operacional e compatibilidade de aplicativos aumentada de ferramentas de software usadas em cenários de depuração e solução de problemas críticos.

Os componentes do sistema operacional que estão disponíveis como parte do FOD de compatibilidade de aplicativos do Server Core incluem:

-   Console de Gerenciamento Microsoft (mmc.exe)

-   Visualizador de Eventos (Eventvwr.msc)

-   Monitor de Desempenho (PerfMon.exe)

-   Monitor de Recursos (Resmon.exe)

-   Gerenciador de Dispositivos (Devmgmt.msc)

-   Explorador de Arquivos (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gerenciamento de Disco (Diskmgmt.msc)

-   Gerenciador de Cluster de Failover (CluAdmin.msc)

    -   Requer a adição do recurso do Windows Server de Clustering de Failover primeiro.

        -   Em uma sessão do PowerShell elevada: 

            ```PowerShell
            Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools
            ```

        -   Para executar o Gerenciador de Cluster de Failover, digite **cluadmin** no prompt de comando.

Servidores que executam o Windows Server, versão 1903 e versões posteriores também são compatíveis com os seguintes componentes (ao usar a mesma versão do FOD de compatibilidade de aplicativos):

- Gerenciador do Hyper-V (virtmgmt.msc)
- Agendador de Tarefas (taskschd.msc)

## <a name="installing-the-app-compatibility-fod"></a>Instalando o FOD de compatibilidade de aplicativos

O FOD de compatibilidade de aplicativos pode ser instalado apenas no Server Core. Não tente adicionar o FOD de compatibilidade de aplicativos do Server Core a uma instalação do Windows Server com experiência Desktop. O mesmo ISO de pacotes opcionais de FOD pode ser usado para instalações Server Core do Windows Server 2019 ou instalações do Canal Semestral do Windows Server.

1. Se o servidor puder se conectar ao Windows Update, tudo o que você precisará fazer é executar o comando a seguir em uma sessão do PowerShell com privilégios elevados e reiniciar o Windows Server após o término da execução do comando:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0
    ```

2. Se o servidor não puder se conectar ao Windows Update, baixe o ISO de pacotes opcionais de FOD do Server e copie o ISO em uma pasta compartilhada em sua rede local:

   - Se você tiver uma licença de volume, poderá baixar o arquivo de imagem ISO de FOD do Server no mesmo portal em que o arquivo de imagem ISO do sistema operacional é obtido: [Centro de Serviços de Licenciamento por Volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - O arquivo de imagem ISO de FOD do Server também está disponível no [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) ou no [portal do Visual Studio](https://visualstudio.microsoft.com) para assinantes.

3. Entre com uma conta de administrador no computador do Server Core que está conectado à sua rede local e ao qual deseja adicionar o FOD.

4. Use **net use**, ou algum outro método, para se conectar à localização do ISO de FOD.

5. Copie o ISO de FOD para uma pasta local de sua escolha.

6. Monte o ISO de FOD usando o seguinte comando em uma sessão do PowerShell com privilégios elevados:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

7. Execute o seguinte comando:

    ```PowerShell
    Add-WindowsCapability -Online -Name ServerCore.AppCompatibility~~~~0.0.1.0 -Source <Mounted_Server_FOD_Drive> -LimitAccess
     ```

8. Após a barra de progresso ser concluída, reinicie o sistema operacional.

   Para obter mais informações sobre os comandos DISM, confira [Usar DISM no Windows PowerShell](https://docs.microsoft.com/windows-hardware/manufacture/desktop/use-dism-in-windows-powershell-s14)

## <a name="to-optionally-add-internet-explorer-11-to-server-core-after-adding-the-server-core-app-compatibility-fod"></a>Para adicionar, opcionalmente, o Internet Explorer 11 ao Server Core (depois de adicionar o FOD de compatibilidade de aplicativos do Server Core)

 >[!NOTE]  
   > O FOD de compatibilidade de aplicativos do Server Core é necessário para a adição do Internet Explorer 11, mas a adição do Internet Explorer 11 ao FOD de compatibilidade de aplicativos do Server Core não é obrigatória.

1. Entre como administrador no computador do Server Core que tem o FOD de compatibilidade de aplicativos já adicionado e o ISO de pacote opcional de FOD do Server copiado localmente.

2. Inicie o PowerShell inserindo **powershell.exe** em um prompt de comando.

3. Monte o ISO de FOD usando o seguinte comando:

    ```PowerShell
    Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso
    ```

4. Execute o seguinte comando, usando a variável `$package_path` para inserir o caminho para o arquivo CAB do Internet Explorer:

    ```PowerShell
    $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

    Add-WindowsPackage -Online -PackagePath $package_path
    ```

5. Após a barra de progresso ser concluída, reinicie o sistema operacional.

## <a name="release-notes-and-suggestions-for-the-server-core-app-compatibility-fod-and-internet-explorer-11-optional-package"></a>Notas sobre a versão e sugestões para o pacote opcional do Internet Explorer 11 e do FOD de compatibilidade de aplicativos do Server Core

> [!IMPORTANT]
> Os FODs instalados no Windows Server, versão 1809 não permanecerão em vigor após uma atualização in-loco para o Windows Server, versão 1903, portanto, é necessário instalá-los novamente após a atualização. Como alternativa, você pode adicionar os FODs à nova fonte de instalação do Windows Server antes da atualização. Isso garante que a nova versão de qualquer FOD esteja presente após a conclusão da atualização. Para obter mais informações, confira [Adicionando funcionalidades e pacotes opcionais a uma imagem do Server Core do WIM offline](install-fod-19.md#add-capabilities).

- **Importante:** Leia as notas sobre a versão do Windows Server 2019 para encontrar informações sobre problemas, considerações ou orientações antes de prosseguir com a instalação e uso do pacote opcional do Internet Explorer 11 e o FOD de compatibilidade de aplicativos do Server Core.

- É possível encontrar cintilação com a experiência de console do Server Core ao adicionar o FOD de compatibilidade do aplicativos depois de usar o Windows Update para instalar as atualizações cumulativas.  Esse problema é resolvido com as atualizações de dezembro de 2018.  Para obter mais informações e encontrar as etapas de resolução, confira [Artigo 4481610 da Base de Dados de Conhecimento: A tela pisca depois de instalar FOD de compatibilidade de aplicativos do Server Core no Windows Server 2019 Server Core](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Após a instalação do FOD de compatibilidade de aplicativos e a reinicialização do servidor, a cor do quadro de janela do console de comando mudará para uma tonalidade de azul diferente.

- Se você optar por também instalar pacote opcional do Internet Explorer 11, observe que não há compatibilidade para clicar duas vezes para abrir arquivos. htm salvos localmente. No entanto, você pode **clicar com o botão direito do mouse** e escolher **Abrir com o IE** ou pode abri-lo diretamente de **Arquivo** -> **Abrir** do Internet Explorer.

- Para melhorar ainda mais a compatibilidade do Server Core com o FOD de compatibilidade de aplicativos, o Console de Gerenciamento de IIS foi adicionado ao Server Core como um componente opcional.  No entanto, é absolutamente necessário primeiro adicionar o FOD de compatibilidade de aplicativos para usar o Console de Gerenciamento de IIS. O Console de Gerenciamento de IIS se baseia no Console de Gerenciamento Microsoft (mmc.exe), que está disponível no Server Core apenas com a adição do FOD de compatibilidade do aplicativos.  Use o PowerShell [**Install-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) para adicionar o Console de Gerenciamento do IIS.

- Como um ponto geral de orientação, ao instalar aplicativos no Server Core (com ou sem esses pacotes adicionais), às vezes é necessário usar instruções e opções de instalação silenciosas. 
    
  - Por exemplo, o SQL Server Management Studio para SQL Server 2016 e SQL Server 2017 podem ser instalado no Server Core e é totalmente funcional, quando o FOD de compatibilidade de aplicativos está presente.  Confira [Instalar o SQL Server do prompt de comando](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
  - Se o SQL Server Management Studio não for desejado, não será necessário instalar o FOD de compatibilidade de aplicativos do Server Core.  Confira [Instalar o SQL Server no Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

## <a name="a-idadd-capabilities-adding-capabilities-and-optional-packages-to-an-offline-wim-server-core-image"></a><a id="add-capabilities"> Adicionando funcionalidades e pacotes opcionais a uma imagem do Server Core do WIM offline

1. Baixe os arquivos de imagem ISO do FOD do Server e Windows Server em uma pasta local em um computador Windows.

   - Se você tiver uma licença de volume, poderá baixar os arquivos de imagem ISO do FOD do Server e Windows Server do [Centro de Serviços de Licenciamento por Volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
   - O arquivo de imagem ISO de FOD do Server também está disponível para versões do Canal de Manutenção em Longo Prazo no [Microsoft Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server) ou no [portal do Visual Studio](https://visualstudio.microsoft.com) para assinantes.

2. Abra uma sessão do PowerShell como administrador e use os seguintes comandos para montar os arquivos de imagem como unidades:

   ```PowerShell
   Mount-DiskImage -ImagePath Path_To_ServerFOD_ISO
   Mount-DiskImage -ImagePath Path_To_Windows_Server_ISO
   ```

3. Copie o conteúdo do arquivo ISO do Windows Server em uma pasta local (por exemplo, *C:\SetupFiles\WindowsServer*).

4. Obtenha o nome de imagem que você deseja modificar no arquivo Install.wim usando o comando a seguir.<br>
Use a variável `$install_wim_path` para inserir o caminho para o arquivo Install.wim, localizado dentro da pasta \Sources do arquivo ISO.

   ```PowerShell
   $install_wim_path = "C:\SetupFiles\WindowsServer\sources\install.wim"

   Get-WindowsImage -ImagePath $install_wim_path
   ```

5. Monte o arquivo Install.wim em uma nova pasta usando o comando a seguir, substituindo os valores de variáveis de exemplo pelos seus e reutilizando a variável `$install_wim_path` do comando anterior.<br>

   - `$image_name` – Insira o nome da imagem que você deseja montar.
   - `$mount_folder variable` – Especifique a pasta a ser usada ao acessar o conteúdo do arquivo Install.wim.

   ```PowerShell
   $image_name = "Windows Server Datacenter"
   $mount_folder = "c:\test\offline"

   Mount-WindowsImage -ImagePath $install_wim_path -Name $image_name -path $mount_folder
   ```

6. Adicione funcionalidades e pacotes que você deseja à imagem de Install.wim montada usando os comandos a seguir, substituindo os valores de variável de exemplo pelos seus.<br>

   - `$capability_name` – Especifique o nome da funcionalidade a ser instalada (nesse caso, a funcionalidade AppCompatibility).
   - `$package_path` – Especifique o caminho para o pacote a ser instalado (nesse caso, Internet Explorer).
   - `$fod_drive` – Especifique a letra da unidade da imagem de FOD do Server montada.

   ```PowerShell
   $capability_name = "ServerCore.AppCompatibility~~~~0.0.1.0"
   $package_path = "D:\Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"
   $fod_drive = "d:\"

   Add-WindowsCapability -Path $mount_folder -Name $capability_name -Source $fod_drive -LimitAccess
   Add-WindowsPackage -Path $mount_folder -PackagePath $package_path
   ```

7. Desmonte e confirme as alterações no arquivo Install.wim usando o comando a seguir, que usa a variável `$mount_folder` de comandos anteriores:

   ```PowerShell
   Dismount-WindowsImage -Path $mount_folder -Save
   ```

Agora você pode atualizar seu servidor executando setup.exe da pasta que criou para os arquivos de instalação do Windows Server (nesse exemplo: *C:\SetupFiles\WindowsServer*). Agora, essa pasta contém arquivos de instalação do Windows Server com os recursos adicionais e pacotes opcionais incluídos.