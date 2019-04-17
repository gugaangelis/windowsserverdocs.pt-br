---
title: Recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD)
description: Como instalar o Windows Server recursos sob demanda
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
ms.sourcegitcommit: dbb4738fdac3b7911952ff11f1eaed9649d6567a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149896"
---
# Recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD)

> Aplica-se ao Windows Server 2019 e Windows Server, versão 1809

O **Recurso de compatibilidade de aplicativo do Server Core sob demanda** é um pacote de recurso opcional que pode ser adicionado para instalações do Server Core do Windows Server 2019 ou Windows Server, versão 1809, a qualquer momento.

Para obter mais informações sobre recursos sob demanda (FOD), consulte [Recursos sob demanda](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).


## Por que instalar o FOD de compatibilidade de aplicativo? 

Compatibilidade de aplicativo, um recurso sob demanda para Server Core, melhora consideravelmente a compatibilidade de aplicativo da opção de instalação Server Core do Windows, incluindo um subconjunto de binários e pacotes do Windows Server com experiência Desktop, sem adicionar o Ambiente de gráfico de experiência de área de trabalho do Windows Server. Esse pacote opcional está disponível em um ISO separado ou do Windows Update, mas só pode ser adicionado a imagens e instalações Server Core do Windows.

Os dois valores primários que fornece o FOD de compatibilidade de aplicativo são:

1.  Aumenta a compatibilidade do Server Core para aplicativos de servidor que já estão no mercado ou já foram desenvolvidos por organizações e implantado.

2.  Ajuda com o fornecimento de componentes do sistema operacional e compatibilidade maior de ferramentas de software usadas na solução de problemas agudo e cenários de depuração.

Componentes do sistema operacional que estão disponíveis como parte do FOD de compatibilidade do Server Core aplicativo incluem:

-   Console de gerenciamento Microsoft (mmc.exe)

-   Visualizador de eventos (eventvwr. msc)

-   Monitor de desempenho (PerfMon.exe)

-   Monitor de recursos (Resmon.exe)

-   Gerenciador de dispositivos (devmgmt. msc)

-   Explorador de arquivos (Explorer.exe)

-   Windows PowerShell (Powershell_ISE.exe)

-   Gerenciamento de disco (Diskmgmt. msc)

-   Gerenciador de Cluster de failover (CluAdmin)

    -   Requer a adição do recurso de servidor Windows Clustering de Failover do primeiro.

        -   Use o Cmdlet do Powershell para adicionar, `Install-WindowsFeature -NameFailover-Clustering -IncludeManagementTools`

        -   Para executar o Gerenciador de Cluster de Failover, insira **cluadmin** no prompt de comando.

## Instalando a compatibilidade de aplicativo FOD

 >[!IMPORTANT] 
   >O FOD de compatibilidade de aplicativo só pode ser instalado no Server Core. Não tente adicionar o FOD de compatibilidade de aplicativo Server Core para uma instalação do Windows Server do Windows Server com experiência Desktop.

### Para adicionar o recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD) a uma instância em execução do Server Core

 >[!NOTE] 
   > Este procedimento usa o gerenciamento (DISM.exe), uma ferramenta de linha de comando e manutenção de imagens de implantação. Para obter mais informações sobre os comandos do DISM, consulte as [Opções de linha de comando DISM recursos de pacote de manutenção](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-capabilities-package-servicing-command-line-options).

>[!NOTE] 
   > Os pacotes opcionais FOD ISO mesmo podem ser usados para instalações Server Core do Windows Server 2019 ou Windows Server, versão 1809, instalações.

>[!NOTE] 
   > Se seu computador ou uma máquina virtual que está executando o Server Core é capaz de se conectar ao Windows Update, as etapas 1 a 7 abaixo pode ser ignorada. Mas não se esqueça de deixar desativar /Source e /LimitAccess do comando DISM na etapa 8.

1. Baixe os pacotes opcionais do servidor FOD ISO e copiar o ISO para uma pasta compartilhada na rede local:

 - Se você tiver uma licença de volume, você pode baixar o arquivo de imagem ISO de FOD do servidor do portal do mesmo onde o arquivo de imagem ISO de sistema operacional é obtido: [Centro de serviço de licenciamento por Volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx).
 - O arquivo de imagem ISO do servidor FOD também está disponível no [Centro de avaliação do Microsoft](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) ou no [portal do Visual Studio](https://visualstudio.microsoft.com) para assinantes.


2. Entre como administrador no computador do Server Core, que está conectado à sua rede local e que você deseja adicionar o FOD para.

3. Use **net use**ou algum outro método para conectar-se para o local do FOD ISO.

4. Copie o ISO FOD para uma pasta local de sua preferência.

5. Inicie o PowerShell inserindo **powershell.exe** em um prompt de comando.

6. Monte o ISO FoD usando o seguinte comando:

        Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

7. Digite **Sair** para sair do PowerShell.

8.  Execute o seguinte comando:

        DISM /Online /Add-Capability /CapabilityName:"ServerCore.AppCompatibility~~~~0.0.1.0" /Source:drive_letter_of_mounted_ISO: /LimitAccess

9.  Depois que a barra de progresso é concluída, reinicie o sistema operacional.

### Opcionalmente, adicionar o Internet Explorer 11 ao Server Core (depois de adicionar o Server Core aplicativo compatibilidade FOD)

 >[!NOTE]  
   > O Server Core aplicativo compatibilidade FOD é necessário para a adição do Internet Explorer 11, mas o Internet Explorer 11 não é necessário adicionar o FOD de compatibilidade de aplicativo Server Core.

1.  Entre como administrador no computador do Server Core que tem o FOD de compatibilidade de aplicativo já adicionado e o pacote opcional do servidor FOD que ISO copiado localmente.

2.  Inicie o PowerShell inserindo **powershell.exe** em um prompt de comando.

3.  Monte o ISO FoD usando o seguinte comando:

         Mount-DiskImage -ImagePath drive_letter:\folder_where_ISO_is_saved\ISO_filename.iso

4.  Digite **Sair** para sair do PowerShell.


5.  Execute o seguinte comando:

        Dism /online /add-package:drive_letter_of_mounted_iso:"Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~.cab"

6.  Depois que a barra de progresso é concluída, reinicie o sistema operacional.

 
#### Notas de versão e sugestões para o pacote opcional FOD de compatibilidade de aplicativo do Server Core e o Internet Explorer 11

- **Importante:** leia as notas de versão do Windows Server 2019 para quaisquer problemas, considerações ou diretrizes antes de prosseguir com a instalação e uso do pacote opcional FOD de compatibilidade de aplicativo do Server Core e o Internet Explorer 11.
 
 >[!NOTE] 
   > É possível encontrar oscilação com a experiência de console do Server Core ao adicionar o FOD de compatibilidade de aplicativo após usando o Windows Update para instalar atualizações cumulativas.  Esse problema é resolvido com dezembro de 2018 atualizações.  Para obter mais etapas de resolução e de informações, consulte [artigo da Base de Conhecimento 4481610: tela treme depois de instalar FOD de compatibilidade de aplicativo do Server Core no Server Core do Windows Server 2019](https://support.microsoft.com/help/4481610/screen-flickers-after-fod-installation-windows2019-server-core).

- Após a instalação do FOD de compatibilidade de aplicativo e reinicialização do servidor, a cor de quadro de janela de console de comando será alterado para uma tonalidade diferente de azul.

- Se você optar por instalar também o pacote opcional do Internet Explorer 11, observe que não é suportado clicando duas vezes para abrir arquivos. htm salvos localmente. No entanto, você pode **clique com botão direito** e escolha **Abrir com o IE**, ou você pode abri-lo diretamente do Internet Explorer **arquivo** -> **Abrir**. 

- Para aprimorar ainda mais a compatibilidade de aplicativo do Server Core com o FOD de compatibilidade de aplicativo, o Console de gerenciamento do IIS foi adicionado ao Server Core como um componente opcional.  No entanto, é absolutamente necessário primeiro adicionar o FOD de compatibilidade de aplicativo para usar o Console de gerenciamento do IIS. Console de gerenciamento do IIS se baseia no Console de gerenciamento Microsoft (mmc.exe), que só está disponível no Server Core com a adição de FOD de compatibilidade do aplicativo.  Use o Powershell [**Instalação-WindowsFeature**](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature?view=win10-ps) para adicionar o Console de gerenciamento do IIS.

- Como um ponto geral de orientação, quando a instalação de aplicativos no Server Core (com ou sem esses pacotes opcionais)-às vezes, é necessário usar instruções e opções de instalação silenciosa. 
    
 - Por exemplo, SQL Server Management Studio para SQL Server 2016 e 2017 do SQL Server pode ser instalado no Server Core e é totalmente funcional quando o FOD de compatibilidade de aplicativo está presente.  Consulte [instalar o SQL Server no Prompt de comando](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-from-the-command-prompt?view=sql-server-2017).
 - Se não desejar o SQL Server Management Studio, é instalar o Server Core aplicativo compatibilidade FOD desnecessária.  Consulte [instalar o SQL Server no Server Core](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-on-server-core?view=sql-server-2017).

