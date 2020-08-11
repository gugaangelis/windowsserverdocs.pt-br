---
title: Início rápido do Nano Server
description: Etapas para implantar rapidamente um Nano Server básico em máquinas virtuais ou físicas
manager: DonGill
ms.date: 09/05/2017
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 49166470cc5ef52cd46a9f92875099602a13acee
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87959564"
---
# <a name="nano-server-quick-start"></a>Início rápido do Nano Server

>Aplica-se a: Windows Server 2016

> [!IMPORTANT]
> A partir do Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem do sistema operacional de base de contêiner](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa.

Execute as etapas nesta seção ara começar rapidamente com uma implantação básica do Nano Server usando DHCP a fim de obter um endereço IP. Você pode executar um VHD do Nano Server em uma máquina virtual ou inicializar nele em um computador físico; as etapas são ligeiramente diferentes.

Depois de experimentar os conceitos básicos com estas etapas de início rápido, você poderá encontrar detalhes sobre a criação de suas próprias imagens personalizadas, gerenciamento de pacote por vários métodos, operações de domínio e muito mais em [Implantar o Nano Server](Deploy-Nano-Server.md).

**Nano Server em uma máquina virtual**

Execute estas etapas para criar um VHD do Nano Server que será executado em uma máquina virtual.

## <a name="to-quickly-deploy-nano-server-in-a-virtual-machine"></a>Para implantar rapidamente o Nano Server em uma máquina virtual

1. Copie a pasta *NanoServerImageGenerator* da pasta \NanoServer no Windows Server 2016 ISO para uma pasta em seu disco rígido.

2. Inicie o Windows PowerShell como administrador, altere o diretório para a pasta onde você colocou a pasta NanoServerImageGenerator e importe o módulo com `Import-Module .\NanoServerImageGenerator -Verbose`
   >[!NOTE]
   >Talvez seja necessário ajustar a política de execução do Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` deve funcionar bem.

3. Crie um VHD para a edição Standard que define um nome de computador e inclui os **drivers convidado** do Hyper-V executando o seguinte comando, que solicitará uma senha de administrador para o novo VHD:

   `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerVM\NanoServerVM.vhd -ComputerName <computer name>` em que

   -   **-MediaPath <caminho até a raiz da mídia\>** especifica um caminho até a raiz do conteúdo do Windows Server 2016 ISO. Por exemplo, se você tiver copiado o conteúdo do ISO em d:\TP5ISO, você usaria esse caminho.

   -   **-BasePath** (opcional) especifica uma pasta que será criada na qual copiar o WIM e os pacotes do Nano Server.

   -   **-TargetPath** especifica um caminho, incluindo o nome de arquivo e extensão, onde o VHD ou VHDX resultante será criado.

   -   **Nome_do_computador** especifica o nome do computador que terá a máquina virtual do Nano Server que você está criando.

   **Exemplo:** `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1`

   Este exemplo cria um VHD a partir de um ISO montado como f:\\. Ao criar o VHD, ele usará uma pasta chamada Base no mesmo diretório no qual você executou o New-NanoServerImage; ele colocará o VHD (chamado Nano.vhd) em uma pasta chamada Nano1 na pasta de onde o comando é executado. O nome do computador será Nano1. O VHD resultante conterá a edição Standard do Windows Server 2016 e será adequado para implantação de máquinas virtuais do Hyper-V. Se você quiser uma máquina virtual da 1ª Geração, crie uma imagem de VHD especificando uma extensão **.vhd** para -TargetPath. Para uma máquina virtual da 2ª Geração, crie uma imagem de VHDX especificando uma extensão **.vhdx** para -TargetPath. Você também pode gerar diretamente um arquivo WIM, especificando uma extensão **.wim** para -TargetPath.

   > [!NOTE]
   > New-NanoServerImage tem suporte no Windows 8.1, no Windows 10, no Windows Server 2012 R2 e no Windows Server 2016.

4. No Gerenciador do Hyper-V, crie uma nova máquina virtual e use o VHD criado na Etapa 3.

5. Reinicialize a máquina virtual e, no Gerenciador do Hyper-V, conecte-se à máquina virtual normalmente.

6. Faça logon no Console de Recuperação (confira a seção Console de Recuperação do Nano Server neste guia), usando o administrador e a senha que você forneceu ao executar o script na Etapa 3.
   > [!NOTE]
   > O Console de Recuperação só oferece suporte a funções básicas de teclado. Não há suporte para as luzes do teclado, seções de 10 teclas e alternância do layout do teclado, como caps lock e bloqueio de número.

7. Obtenha o endereço IP da máquina virtual do Nano Server e use o Windows PowerShell remotamente, ou outra ferramenta de gerenciamento remoto, para conectar-se e gerenciar remotamente a máquina virtual.

**Nano Server em um computador físico**

Você também pode criar um VHD que executará o Nano Server em um computador físico, usando os drivers de dispositivo pré-instalados. Se o seu hardware exigir um driver que ainda não tiver sido fornecido para inicializar ou conectar-se a uma rede, execute as etapas na seção Adicionar outros drivers deste guia.

## <a name="to-quickly-deploy-nano-server-on-a-physical-computer"></a>Para implantar rapidamente o Nano Server em um computador físico

1.  Copie a pasta *NanoServerImageGenerator* da pasta \NanoServer no Windows Server 2016 ISO para uma pasta em seu disco rígido.

2.  Inicie o Windows PowerShell como administrador, altere o diretório para a pasta onde você colocou a pasta NanoServerImageGenerator e importe o módulo com `Import-Module .\NanoServerImageGenerator -Verbose`

>[!NOTE]
>Talvez seja necessário ajustar a política de execução do Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` deve funcionar bem.

3. Crie um VHD que define um nome de computador e inclui os drivers OEM e o Hyper-V executando o seguinte comando, que solicitará uma senha de administrador para o novo VHD:

   `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.vhd -ComputerName <computer name> -OEMDrivers -Compute -Clustering` em que

   -   **-MediaPath <caminho até a raiz da mídia\>** especifica um caminho até a raiz do conteúdo do Windows Server 2016 ISO. Por exemplo, se você tiver copiado o conteúdo do ISO em d:\TP5ISO, você usaria esse caminho.

   -   **BasePath** especifica uma pasta que será criada na qual copiar o WIM e os pacotes do Nano Server. (Esse parâmetro é opcional).

   -   **TargetPath** especifica um caminho, incluindo o nome de arquivo e extensão, onde o VHD ou VHDX resultante será criado.

   -   **Nome_do_computador** é o nome do computador para o Nano Server que você está criando.

   **Exemplo:** `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath F:\ -BasePath .\Base -TargetPath .\Nano1\NanoServer.vhd -ComputerName Nano-srv1 -OEMDrivers -Compute -Clustering`

   Este exemplo cria um VHD a partir de um ISO montado como F:\\. Ao criar o VHD, ele usará uma pasta chamada Base no mesmo diretório no qual você executou o New-NanoServerImage; ele colocará o VHD em uma pasta chamada Nano1 na pasta de onde o comando é executado. O nome do computador será Nano-srv1 e terá drivers OEM instalados para o hardware mais comum, e tem a função Hyper-V e o recurso de cluster habilitados. A edição Nano Standard é usada.

4. Faça logon como administrador no servidor físico onde você deseja executar o Nano Server VHD.

5. Copie o VHD criado por esse script no computador físico e configure-o para inicializar a partir desse novo VHD. Para fazer isso, execute estas etapas:

   1.  Monte o VHD gerado. Neste exemplo, ele é montado em D:\\.

   2.  Execute **bcdboot d:\windows**.

   3.  Desmonte o VHD.

6. Inicialize o computador físico no Nano Server VHD.

7. Faça logon no Console de Recuperação (confira a seção Console de Recuperação do Nano Server neste guia), usando o administrador e a senha que você forneceu ao executar o script na Etapa 3.
   > [!NOTE]
   > O Console de Recuperação só oferece suporte a funções básicas de teclado. Não há suporte para as luzes do teclado, seções de 10 teclas e alternância do layout do teclado, como caps lock e bloqueio de número.

8. Obtenha o endereço IP do computador do Nano Server e use o Windows PowerShell remotamente, ou outra ferramenta de gerenciamento remoto, para conectar-se e gerenciar remotamente a máquina virtual.
