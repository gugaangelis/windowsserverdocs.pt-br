---
title: Implantar o Nano Server
description: Explica a criação e a implantação de imagens, pacotes, drivers, domínios, funções e recursos personalizados
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 9f109c91-7c2e-4065-856c-ce9e2e9ce558
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: e61844cfb04f95723fe9d08b9bd2e8b481714eea
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442237"
---
# <a name="deploy-nano-server"></a>Implantar o Nano Server

>Aplica-se a: Windows Server 2016

> [!IMPORTANT]
> A partir do Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem de sistema operacional base do contêiner](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa. 

Este tópico aborda as informações necessárias para implantar imagens do Nano Server mais personalizadas para suas necessidades em comparação aos exemplos simples no Tópico de início rápido do Nano Server. Você encontrará informações sobre como criar uma imagem personalizada do Nano Server com os recursos que você deseja, como instalar imagens do Nano Server de VHD ou WIM, como editar arquivos, como trabalhar com domínios, lidar com pacotes usando vários métodos e trabalhar com funções de servidor.

## <a name="nano-server-image-builder"></a>Construtor de Imagens do Nano Server

O Construtor de Imagens do Nano Server é uma ferramenta que ajuda você a criar uma imagem personalizada do Nano Server e uma mídia USB inicializável com o auxílio de uma interface gráfica. Com base nas entradas fornecidas por você, ele gera scripts reutilizáveis do PowerShell que permitem que você automatize com facilidade instalações consistentes do Nano Server que executam o Windows Server 2016 Datacenter ou edições Standard.

Obtenha a ferramenta do [Centro de Download](https://www.microsoft.com/en-us/download/details.aspx?id=54065). 

A ferramenta também exige o [ADK (Kit de Avaliação e Implantação do Windows)](https://developer.microsoft.com/en-us/windows/hardware/windows-assessment-deployment-kit).


O Construtor de Imagens do Nano Server cria imagens personalizadas do Nano Server nos formatos VHD, VHDX ou ISO e pode criar uma mídia USB inicializável para implantar o Nano Server ou detectar a configuração de hardware de um servidor. Também pode fazer o seguinte:

- Aceitar os termos de licença 
- Criar formatos VHD, VHDX ou ISO
- Adicionar funções de servidor
- Adicionar drivers de dispositivo
- Definir o nome do computador, a senha do administrador, o caminho do arquivo de log e o fuso horário
- Ingressar em um domínio usando uma conta existente do Active Directory ou um blob coletado ingressado em domínio
- Habilitar o WinRM para comunicação fora da sub-rede local
- Habilitar IDs de LAN Virtual e configurar endereços IP estáticos
- Injetar novos pacotes de serviço em tempo real
- Adicionar um setupcomplete.cmd ou outros scripts de cliente para execução após o processamento de unattend.xml
- Habilitar o EMS (Serviços de gerenciamento de emergência) para acesso de console de porta serial
- Habilitar serviços de desenvolvimento para permitir o teste de drivers assinados e aplicativos não assinados, o shell padrão do PowerShell
- Habilitar a depuração em protocolos em série, USB, TCP/IP ou IEEE 1394
- Criar uma mídia USB usando o WinPE que particionará o servidor e instalará a imagem Nano
- Criar uma mídia USB usando o WinPE que detectará sua configuração de hardware existente do Nano Server e reportará todos os detalhes em um log e na tela. Isso inclui os adaptadores de rede, endereços MAC e firmware do Tipo (BIOS ou UEFI). O processo de detecção também listará todos os volumes no sistema e os dispositivos que não têm um driver incluído no pacote de drivers do Server Core.

Se você não conhecer qualquer um desses, revise o restante deste tópico e os outros tópicos sobre Nano Server para se preparar para fornecer à ferramenta as informações necessárias.

## <a name="BKMK_CreateImage"></a>Criando uma imagem personalizada do Nano Server  
Para o Windows Server 2016, o Nano Server é distribuído em mídia física, onde você encontrará uma pasta **NanoServer**; essa pasta contém uma imagem .wim e uma subpasta chamada **Pacotes**. São esses arquivos de pacote que você usa para adicionar funções de servidor e recursos para a imagem VHD, na qual você inicializa.  

Você também pode localizar e instalar esses pacotes com o provedor NanoServerPackage do módulo do PowerShell PackageManagement (OneGet). Confira a seção "Instalar funções e recursos online" deste tópico.  

Esta tabela mostra as funções e recursos disponíveis nesta versão do Nano Server, juntamente com as opções do Windows PowerShell que instalarão os pacotes. Alguns pacotes são instalados diretamente com suas próprias opções do Windows PowerShell (como -Compute); outros você instala passando nomes de pacote para o parâmetro -Package, que você pode combinar em uma lista separada por vírgulas. Você pode listar dinamicamente os pacotes disponíveis usando o cmdlet Get-NanoServerPackage.  


|                                                                             Função ou recurso                                                                             |                                                                                                                                                                                                          Opção                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                     Função Hyper-V (incluindo NetQoS)                                                                     |                                                                                                                                                                                                         -Compute                                                                                                                                                                                                          |
|                                                   Clustering de Failover e outros componentes, detalhados após esta tabela                                                   |                                                                                                                                                                                                        -Clustering                                                                                                                                                                                                        |
| Drivers básicos para vários adaptadores de rede e controladores de armazenamento. Esse é o mesmo conjunto de drivers incluído em uma instalação do Server Core do Windows Server 2016. |                                                                                                                                                                                                        -OEMDrivers                                                                                                                                                                                                        |
|                                                Função Servidor de Arquivos e outros componentes de armazenamento, detalhados após esta tabela                                                 |                                                                                                                                                                                                         -Storage                                                                                                                                                                                                          |
|                                                          Windows Defender, incluindo um arquivo de assinatura padrão                                                           |                                                                                                                                                                                                         -Defender                                                                                                                                                                                                         |
|                         Reverta os encaminhadores para compatibilidade de aplicativos, por exemplo, estruturas de aplicativo comuns, como Ruby, Node.js etc.                         |                                                                                                                                                                                                  Agora incluído por padrão                                                                                                                                                                                                  |
|                                                                             Função Servidor DNS                                                                             |                                                                                                                                                                                         -Package Microsoft-NanoServer-DNS-Package                                                                                                                                                                                         |
|                                                              DSC (Configuração de Estado Desejado) do PowerShell                                                               |                                                                                                                               -Package Microsoft-NanoServer-DSC-Package<br />**Observação:** Para obter mais detalhes, confira [Usar DSC no Nano Server](https://msdn.microsoft.com/powershell/dsc/nanoDsc).                                                                                                                               |
|                                                                    Internet Information Server (IIS)                                                                    |                                                                                                                                       -Package Microsoft-NanoServer-IIS-Package<br />**Observação:** Ver [IIS no Nano Server](IIS-on-Nano-Server.md) para obter detalhes sobre como trabalhar com o IIS.                                                                                                                                        |
|                                                                   Suporte de host para contêineres do Windows                                                                   |                                                                                                                                                                                                        -Containers                                                                                                                                                                                                        |
|                                                               Agente do System Center Virtual Machine Manager                                                               | -Package Microsoft-NanoServer-SCVMM-Package<br />-Package Microsoft-NanoServer-SCVMM-Compute-Package<br />**Observação:** Use o SCVMM Compute package somente se você estiver monitorando o Hyper-V. Para implantações hiperconvergidas em VMM, você também deve especificar o -parâmetro de armazenamento. Para obter mais detalhes, consulte a [documentação VMM](https://technet.microsoft.com/system-center-docs/vmm/manage/manage-compute-add-nano-hyper-v). |
|                                                                 Agente do System Center Operations Manager                                                                  |                                                                                                                 Instalado separadamente. Consulte a documentação do System Center Operations Manager para obter mais detalhes em https://technet.microsoft.com/system-center-docs/om/manage/install-agent-on-nano-server.                                                                                                                 |
|                                                                 Data Center Bridging (incluindo DCBQoS)                                                                 |                                                                                                                                                                                         -Package Microsoft-NanoServer-DCB-Package                                                                                                                                                                                         |
|                                                                     Implantar em uma máquina virtual                                                                      |                                                                                                                                                                                        -Package Microsoft-NanoServer-Guest-Package                                                                                                                                                                                        |
|                                                                     Implantar em uma máquina física                                                                     |                                                                                                                                                                                        - Package Microsoft-NanoServer-Host-Package                                                                                                                                                                                        |
|     O BitLocker, o TPM (Trusted Platform Module), criptografia de volume, identificação da plataforma, provedores de criptografia e outras funcionalidades relacionadas à inicialização segura     |                                                                                                                                                                                    -Package Microsoft-NanoServer-SecureStartup-Package                                                                                                                                                                                    |
|                                                                    Suporte do Hyper-V para VMs Blindadas                                                                     |                                                                                                                                         -Package Microsoft-NanoServer-ShieldedVM-Package<br />**Observação:** Esse pacote só está disponível para o Datacenter edition do Nano Server.                                                                                                                                         |
|                                                             Agente do protocolo SNMP (Simple Network Management)                                                             |                                   -Pacote Microsoft-NanoServer-SNMP-Agent-Package.cab<br />**Observação:** Não incluído na mídia de instalação do Windows Server 2016. Disponível somente online. Consulte [Instalar funções e recursos online](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online) para obter detalhes.                                    |
|               Serviço de IPHelper que fornece conectividade de túnel usando tecnologias de transição IPv6 (6to4, ISATAP, Proxy de Porta e Teredo) e IP-HTTPS               |                                -Pacote Microsoft-NanoServer-IPHelper-Service-Package.cab<br />**Observação:** Não incluído na mídia de instalação do Windows Server 2016. Disponível somente online. Consulte [Instalar funções e recursos online](https://technet.microsoft.com/windows-server-docs/get-started/deploy-nano-server#a-namebkmkonlineainstalling-roles-and-features-online) para obter detalhes.                                 |

> [!NOTE]  
> Quando você instala pacotes com essas opções, um pacote de idiomas correspondente também é instalado com base na localidade da mídia do servidor selecionado. Você pode encontrar os pacotes de idiomas disponíveis e suas abreviações de localidade na mídia de instalação, em subpastas nomeadas com a localidade da imagem.  

> [!NOTE]  
> Quando você usa o parâmetro -Storage para instalar os Serviços de arquivo, eles não são, na verdade, habilitados. Habilite esse recurso de um computador remoto com o Gerenciador do Servidor. 

### <a name="failover-clustering-items-installed-by-the--clustering-parameter"></a>Itens de Clustering de Failover instalados pelo parâmetro -Clustering

- Função Clustering de Failover
- Clustering de Failover de VM
- Espaços de Armazenamento Diretos (S2D)
- Qualidade de Serviço do Armazenamento
- Clustering de Replicação de Volume
- Serviço Testemunha SMB


### <a name="file-and-storage-items-installed-by-the--storage-parameter"></a>Itens de arquivo e armazenamento instalados pelo parâmetro -Storage

- Função Servidor de Arquivos
- Eliminação de Duplicação de Dados
- Multipath I/O, incluindo um driver para o MSDSM (Módulo Específico de Dispositivo Microsoft)
- ReFS (v1 e v2)
- Iniciador iSCSI (mas não o Destino iSCSI)
- Réplica de Armazenamento
- Serviço de Gerenciamento de Armazenamento com suporte SMI-S
- Serviço Testemunha SMB
- Volumes Dinâmicos
- Provedores de armazenamento básicos do Windows (para Gerenciamento de Armazenamento do Windows)




### <a name="installing-a-nano-server-vhd"></a>Instalar um VHD do Nano Server  
Este exemplo cria uma imagem VHDX com base em GPT com o nome de um certo computador e incluindo drivers de convidado do Hyper-V, começando com a mídia de instalação do Nano Server em um compartilhamento de rede. Em uma janela do Windows PowerShell com privilégios elevados, comece com este cmdlet:  

`Import-Module <Server media location>\NanoServer\NanoServerImageGenerator; New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\server_en-us -BasePath .\Base -TargetPath .\FirstStepsNano.vhdx -ComputerName FirstStepsNano`  

O cmdlet realizará todas essas tarefas:  

1.  Selecionar Standard como uma edição de base  

2.  Solicitar a senha de Administrador  

3.  Copiar a mídia de instalação de \\\Path\To\Media\server_en-us para .\Base  

4.  Converter a imagem WIM a um VHD. (A extensão de arquivo do argumento de caminho de destino determina se ele cria um VHD com base em MBR para máquinas virtuais da 1ª geração versus um VHDX baseado em GPT para máquinas virtuais da 2ª geração)  

5.  Copie o VHD resultante em .\FirstStepsNano.vhdx  

6.  Defina a senha de Administrador para a imagem conforme especificado  

7.  Defina o nome do computador da imagem como FirstStepsNano  

8.  Instale os drivers convidados do Hyper-V  

Tudo isso resulta em uma imagem de .\FirstStepsNano.vhdx.  

O cmdlet gera um log conforme é executado e informará a você a localização desse após a conclusão. A conversão de WIM para VHD realizada pelo script complementar gera seu próprio log em %TEMP%\Convert-WindowsImage\\\<GUID > (onde \<GUID > é um identificador exclusivo por sessão de conversão).  

Contanto que você use o mesmo caminho de base, poderá omitir o parâmetro de caminho de mídia sempre que você executar esse cmdlet, já que ele usará os arquivos armazenados em cache do caminho de base. Se você não especificar um caminho de base, o cmdlet gerará um padrão na pasta TEMP. Porém, se você quiser usar uma mídia de origem diferentes, mas com o mesmo caminho de base, especifique o parâmetro de caminho de mídia.  

>[!NOTE]  
>Agora você tem a opção de especificar a edição do Nano Server para compilar a edição Standard ou Datacenter. Use o parâmetro -Edition para especificar as edições *Standard* ou *Datacenter*.  

Quando você tiver uma imagem existente, poderá modificá-la conforme o necessário usando o cmdlet *Edit-NanoServerImage*.  

Se você não especificar um nome de computador, será gerado um nome aleatório.  

### <a name="installing-a-nano-server-wim"></a>Instalar um WIM do Nano Server  

1. Copie a pasta *NanoServerImageGenerator* da pasta \NanoServer na pasta local do Windows Server 2016 ISO em seu computador.  
2. Inicie o Windows PowerShell como administrador, altere o diretório para a pasta onde você colocou a pasta NanoServerImageGenerator e, em seguida, importe o módulo com `Import-Module .\NanoServerImageGenerator -Verbose`.  

   >[!NOTE]  
   >Talvez seja necessário ajustar a política de execução do Windows PowerShell. `Set-ExecutionPolicy RemoteSigned` deve funcionar bem.  

Para criar uma imagem do Nano Server para servir como um host do Hyper-V, execute o seguinte:  

`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.wim -ComputerName <computer name> -OEMDrivers -Compute -Clustering`  

Em que  
-   -MediaPath é a raiz da mídia de DVD ou imagem ISO que contém o Windows Server 2016.  
-   -BasePath conterá uma cópia dos binários do Nano Server, então você pode usar o novo -BasePath do NanoServerImage sem precisar especificar -MediaPath em execuções futuras.  
-   -TargetPath conterá o arquivo .wim resultante que contém as funções e recursos que você selecionou. Especifique a extensão .wim.  
-   -Compute adiciona a função Hyper-V.  
-   -OemDrivers adiciona um número de drivers comuns.  

Você receberá uma solicitação para inserir uma senha de Administrador.  

Para obter mais informações, execute `Get-Help New-NanoServerImage -Full`.  

Inicialize no WinPE e verifique se o arquivo .wim recém-criado pode ser acessado a partir do WinPE. (Você pode, por exemplo, copiar o arquivo .wim em uma imagem inicializável do WinPE em uma unidade flash USB).  

Após a inicialização do WinPE, use o Diskpart.exe para preparar o disco rígido do computador de destino. Execute os seguintes comandos de Diskpart (modifique adequadamente, se você não estiver usando UEFI & GPT):  

> [!WARNING]  
> Estes comandos excluirão todos os dados do disco rígido.  

**Tamanho da partição GPT limpa converter, criar efi Diskpart.exe Select disk 0 = 100 FS rápido de formato = FAT32 rótulo = "System" Assign letter = "s" Criar partição msr size = 128 criar partição primário FS rápido de formato = rótulo do NTFS = "NanoServer" Assign letter = "n" List volume Sair**  

Aplique a imagem do Nano Server (ajuste o caminho do arquivo .wim):  

**Dism.exe /apply-image /imagefile:.\NanoServer.wim /index:1 /applydir:n:\ Bcdboot.exe n:\Windows /s s:**  

Remova a mídia de DVD ou unidade USB e reinicialize o sistema com **Wpeutil.exe Reboot**  


### <a name="editing-files-on-nano-server-locally-and-remotely"></a>Edição de arquivos no Nano Server localmente e remotamente  
 Em ambos os casos, conecte-se ao Nano Server, por exemplo, com a comunicação remota do Windows PowerShell.  

 Depois de se conectar ao Nano Server, você pode editar um arquivo que reside no computador local passando o caminho do arquivo relativo ou absoluto para o comando psEdit, por exemplo:   
`psEdit C:\Windows\Logs\DISM\dism.log` ou `psEdit .\myScript.ps1`  

Editar um arquivo que reside no Nano Server remoto iniciando uma sessão remota com `Enter-PSSession -ComputerName "192.168.0.100" -Credential ~\Administrator` e, em seguida, passando o caminho do arquivo relativo ou absoluto para o comando psEdit, da seguinte forma:   
`psEdit C:\Windows\Logs\DISM\dism.log`  

## <a name="BKMK_online"></a>Instalando funções e recursos online  
> [!NOTE]
> Se você instalar um pacote do Nano Server opcional a partir de uma mídia ou um repositório online, ele não terá correções de segurança recentes incluídas. Para evitar uma incompatibilidade de versão entre os pacotes opcionais e o sistema operacional básico, você deve instalar a [atualização cumulativa mais recente](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) imediatamente após a instalação de todos os pacotes opcionais e **antes** de reiniciar o servidor.

### <a name="installing-roles-and-features-from-a-package-repository"></a>Instalar funções e recursos de um repositório de pacotes  
Você pode localizar e instalar pacotes do Nano Server do repositório online de pacotes usando o provedor NanoServerPackage do módulo PackageManagement do PowerShell. Para instalar esse provedor, use estes cmdlets:

```powershell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
```

>[!NOTE]
>Se ocorrerem erros durante a execução do Install-PackageProvider, verifique se você instalou a [atualização cumulativa mais recente](https://technet.microsoft.com/windows-server-docs/get-started/update-nano-server) ([KB3206632](https://support.microsoft.com/kb/3206632) ou posterior) ou use Save-Module, da seguinte maneira: 

```powershell
Save-Module -Path "$Env:ProgramFiles\WindowsPowerShell\Modules\" -Name NanoServerPackage -MinimumVersion 1.0.1.0
Import-PackageProvider NanoServerPackage
```

Depois que esse provedor for instalado e importado, você poderá procurar, baixar e instalar pacotes do Nano Server usando os cmdlets projetados especificamente para funcionar com pacotes do Nano Server:

```powershell
Find-NanoServerPackage  
Save-NanoServerPackage  
Install-NanoServerPackage
```  

Você também pode usar os cmdlets PackageManagement genéricos e especificar o provedor do NanoServerPackage:

```powershell
Find-Package -ProviderName NanoServerPackage
Save-Package -ProviderName NanoServerPackage
Install-Package -ProviderName NanoServerPackage
Get-Package -ProviderName NanoServerPackage
```

Para usar esses cmdlets com pacotes do Nano Server no Nano Server, adicione `-ProviderName NanoServerPackage`. Se você não adicionar o parâmetro -ProviderName, o PackageManagement iterará todos os provedores. Para obter mais detalhes sobre esses cmdlets, execute `Get-Help <cmdlet>`. Estes são alguns exemplos de uso comuns:

### <a name="searching-for-nano-server-packages"></a>Pesquisar pacotes do Nano Server  
Você pode usar um `Find-NanoServerPackage` ou `Find-Package -ProviderName NanoServerPackage` para pesquisar e retornar uma lista de pacotes do Nano Server que estão disponíveis no repositório online. Por exemplo, você pode obter uma lista de todos os pacotes mais recentes:

```powershell
Find-NanoServerPackage
```

Executar `Find-Package -ProviderName NanoServerPackage -DisplayCulture` exibe todas as culturas disponíveis.

Se você precisa de uma versão de localidade específica, como inglês EUA, use `Find-NanoServerPackage -Culture en-us` ou  
`Find-Package -ProviderName NanoServerPackage -Culture en-us` ou `Find-Package -Culture en-us -DisplayCulture`

Para localizar um pacote específico pelo nome do pacote, use o parâmetro -Name. Esse parâmetro também aceita caracteres curinga. Por exemplo, para localizar todos os pacotes com VMM no nome, use `Find-NanoServerPackage -Name *VMM*` ou `Find-Package -ProviderName NanoServerPackage -Name *VMM*`.

Você pode encontrar uma versão específica com os parâmetros -RequiredVersion, -MinimumVersion ou -MaximumVersion. Para localizar todas as versões disponíveis, use -AllVersions. Caso contrário, apenas a versão mais recente será retornada. Por exemplo: `Find-NanoServerPackage -Name *VMM* -RequiredVersion 10.0.14393.0`. Ou, para todas as versões: `Find-Package -ProviderName NanoServerPackage -Name *VMM* -AllVersions`

### <a name="installing-nano-server-packages"></a>Instalar pacotes do Nano Server  
Você pode instalar um pacote do Nano Server (incluindo seus pacotes de dependência, se houver algum) no Nano Server, localmente ou em uma imagem offline com `Install-NanoServerPackage` ou `Install-Package -ProviderName NanoServerPackage`. Ambos aceitam entrada do pipeline.

Para instalar a versão mais recente de um pacote do Nano Server em um Nano Server online, use `Install-NanoServerPackage -Name Microsoft-NanoServer-Containers-Package` ou `Install-Package -Name Microsoft-NanoServer-Containers-Package`. PackageManagement usará a cultura do Nano Server.

Você pode instalar um pacote do Nano Server em uma imagem offline, especificando uma versão e cultura específicas, da seguinte maneira:

`Install-NanoServerPackage -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

ou:

`Install-Package -Name Microsoft-NanoServer-DCB-Package -Culture de-de -RequiredVersion 10.0.14393.0 -ToVhd C:\MyNanoVhd.vhd`

Veja alguns exemplos de pipelining de resultados de pesquisa de pacote para o cmdlet de instalação:  

`Find-NanoServerPackage *dcb* | Install-NanoServerPackage` Localiza todos os pacotes com "dcb" no nome e, em seguida, instala-os.

`Find-Package *nanoserver-compute-* | Install-Package` Localiza pacotes com "nanoserver - compute-" no nome e os instala.

`Find-NanoServerPackage -Name *nanoserver-compute* | Install-NanoServerPackage -ToVhd C:\MyNanoVhd.vhd` Localiza pacotes com "compute" no nome e instala-os em uma imagem offline.

`Find-Package -ProviderName NanoserverPackage *nanoserver-compute-* | Install-Package -ToVhd C:\MyNanoVhd.vhd` faz a mesma coisa com qualquer pacote que tenha "nanoserver - compute-" no nome.

### <a name="downloading-nano-server-packages"></a>Baixar pacotes do Nano Server  

`Save-NanoServerPackage` ou `Save-Package` permitem que você baixe os pacotes e salvá-los sem instalá-los. Ambos os cmdlets aceitam entrada do pipeline.

Por exemplo, para baixar e salvar um pacote do Nano Server em um diretório que corresponde ao caminho curinga, use `Save-NanoServerPackage -Name Microsoft-NanoServer-DNS-Package -Path C:\`. Neste exemplo, -Culture não foi especificado, então a cultura do computador local será usada. Nenhuma versão foi especificada, então a versão mais recente será salva.

`Save-Package -ProviderName NanoServerPackage -Name Microsoft-NanoServer-IIS-Package -Path C:\ -Culture it-IT -MinimumVersion 10.0.14393.0` salva uma versão específica e para a localidade e idioma italiano.

Você pode enviar os resultados de pesquisa pelo pipeline, como nestes exemplos:

`Find-NanoServerPackage -Name *containers* -MaximumVersion 10.2 -MinimumVersion 1.0 -Culture es-ES | Save-NanoServerPackage -Path C:\`

ou

`Find-Package -ProviderName NanoServerPackage -Name *shield* -Culture es-ES | Save-Package -Path`

### <a name="inventory-installed-packages"></a>Pacotes de inventário instalados
Você pode descobrir quais pacotes do Nano Server são instalados com o `Get-Package`. Por exemplo, veja quais pacotes estão no Nano Server com `Get-Package -ProviderName NanoserverPackage`.

Para verificar os pacotes do Nano Server que estão instalados em uma imagem offline, execute `Get-Package -ProviderName NanoserverPackage -FromVhd C:\MyNanoVhd.vhd`.


### <a name="installing-roles-and-features-from-local-source"></a>Instalar funções e recursos de uma fonte local  
Embora seja recomendada a instalação offline de funções de servidor e de outros pacotes, talvez seja necessário instalá-los online (com o Nano Server) em cenários de contêiner. Para fazer isso, execute estas etapas:  

1.  Copie a pasta Pacotes da mídia de instalação localmente no Nano Server em execução (por exemplo, em C:\packages.).  

2.  Crie um novo arquivo Unattend.xml em outro computador e, em seguida, copie-o no Nano Server. Você pode copiar e colar esse conteúdo XML no arquivo XML que você criou (este exemplo mostra a instalação do pacote do IIS):  



```  
<?xml version="1.0" encoding="utf-8"?>
    <unattend xmlns="urn:schemas-microsoft-com:unattend">  
    <servicing>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" />  
            <source location="c:\packages\Microsoft-NanoServer-IIS-Package.cab" />  
        </package>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Feature-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="en-US" />  
            <source location="c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab" />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source="" xmlns:cpi="urn:schemas-microsoft-com:cpi" />  
</unattend>  
```  




3. No novo arquivo XML criado (ou copiado), edite C:\packages para o diretório no qual você copiou o conteúdo de Pacotes.  

4. Alternar para o diretório com o arquivo XML criado recentemente e executar  

   **dism /online /apply-unattend:.\unattend.xml**  



5. Confirme se o pacote e o pacote de idiomas associado está instalado corretamente, executando:  

   **dism /online /get-packages**  

   Você deve ver "Package Identity: Microsoft-NanoServer-IIS-Package ~ 31bf3856ad364e35 ~ amd64 ~ en-US ~ 10.0.10586.0" listado duas vezes, uma vez para o tipo de versão: Pacote de idiomas e uma vez para o tipo de versão: Feature Pack.  

## <a name="customizing-an-existing-nano-server-vhd"></a>Personalizar um VHD existente do Nano Server  
Você pode alterar os detalhes de um VHD existente usando o cmdlet Edit-NanoServerImage, como neste exemplo:  

`Edit-NanoServerImage   -BasePath .\Base   -TargetPath .\BYOVHD.vhd`  

Esse cmdlet faz a mesma coisa que New-NanoServerImage, mas altera a imagem existente em vez de converter um WIM em um VHD. Ele oferece suporte aos mesmos parâmetros que New-NanoServerImage, com a exceção de -MediaPath e -MaxSize, portanto, o VHD inicial deve ter sido criado com esses parâmetros antes de ser possível fazer alterações com Edit-NanoServerImage.  

## <a name="additional-tasks-you-can-accomplish-with-new-nanoserverimage-and-edit-nanoserverimage"></a>Tarefas adicionais que você pode realizar com New-NanoServerImage e Edit-NanoServerImage  

### <a name="joining-domains"></a>Ingressar em domínios  
New-NanoServerImage oferece dois métodos para ingressar em um domínio; ambos dependem do provisionamento de domínio offline, mas uma coleta um blob para fazer o ingresso. Neste exemplo, o cmdlet coleta um blob de domínio para o domínio Contoso do computador local (que, obviamente, precisa fazer parte do domínio Contoso) e executa o provisionamento offline da imagem usando o blob:  

`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomHarvest.vhdx -ComputerName JoinDomHarvest -DomainName Contoso`  

Após a conclusão desse cmdlet, você deve encontrar um computador chamado "JoinDomHarvest" na lista de computadores do Active Directory.  

Você também pode usar esse cmdlet em um computador que não ingressou em um domínio. Para fazer isso, colete um blob de qualquer computador que tenha ingressado no domínio e, em seguida, forneça o blob para o cmdlet por conta própria. Observe que, quando você coleta um blob de outro computador, o blob já inclui o nome do computador. Portanto, se você tentar adicionar o parâmetro *-ComputerName*, ocorrerá um erro.  

Você pode coletar o blob com este comando:  

**djoin**  
 **/Provision**  
 **/ A Contoso domínio**  
 **/ JoiningDomainsNoHarvest computador**  
 **/SaveFile JoiningDomainsNoHarvest.djoin**  

Execute New-NanoServerImage usando o blob coletado:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\JoinDomNoHrvest.vhd -DomainBlobPath .\Path\To\Domain\Blob\JoinDomNoHrvestContoso.djoin`  

Se você já tiver um nó no domínio com o mesmo nome de computador que seu futuro Nano Server, reutilize o nome do computador, adicionando o parâmetro `-ReuseDomainNode`.  

### <a name="adding-additional-drivers"></a>Adicionar outros drivers
O Nano Server oferece um pacote que inclui um conjunto de drivers básicos vários adaptadores de rede e controladores de armazenamento; é possível que os drivers para os adaptadores de rede não estejam incluídos. Você pode usar estas etapas para localizar os drivers em um sistema em funcionamento, extraí-los e, em seguida, adicioná-los à imagem do Nano Server.

1. Instale o Windows Server 2016 no computador físico onde você executará o Nano Server.
2. Abra o Gerenciador de Dispositivos e identifique os dispositivos nas seguintes categorias:
3. Adaptadores de Rede
4. Controladores de armazenamento
5. Unidades de disco
6. Para cada dispositivo nessas categorias, clique no nome do dispositivo e clique em **Propriedades**. Na caixa de diálogo aberta, clique na guia **Driver** e, em seguida, clique em **Detalhes do Driver**.
7. Anote o nome e o caminho do arquivo do driver que é exibido. Por exemplo, vamos supor que o arquivo de driver seja e1i63x64.sys, que está em C:\Windows\System32\Drivers.
8. Em um prompt de comando, procure o arquivo de driver e procure por todas as instâncias com dir e1i*.sys /s /b. Neste exemplo, o arquivo de driver também está presente no caminho C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd\e1i63x64.sys.
9. Em um prompt de comando elevado, navegue até o diretório no qual o VHD do Nano Server está e execute os seguintes comandos: **md mountdir**

    **dism\dism /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**

    **dism\dism /Add-Driver /image:.\mountdir /driver: C:\Windows\System32\DriverStore\FileRepository\net1ic64.inf_amd64_fafa7441408bbecd**

    **dism\dism /Unmount-Image /MountDir:.\MountDir /Commit**
10. Repita estas etapas para cada arquivo de driver que você precisar.

> [!NOTE]  
> Na pasta onde você mantém seus drivers, os arquivos de Sistema e os arquivos INF correspondentes devem estar presentes. Além disso, o Nano Server oferece suporte apenas aos drivers de 64\-bits assinados. 

### <a name="injecting-drivers"></a>Injetar drivers  
O Nano Server oferece um pacote que inclui um conjunto de drivers básicos vários adaptadores de rede e controladores de armazenamento; é possível que os drivers para os adaptadores de rede não estejam incluídos. Você pode usar esta sintaxe para que New-NanoServerImage pesquise no diretório por drivers disponíveis e injete-os na imagem do Nano Server:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers`  

> [!NOTE]
> Na pasta onde você mantém seus drivers, os arquivos de Sistema e os arquivos INF correspondentes devem estar presentes. Além disso, o Nano Server oferece suporte apenas aos drivers de 64 bits assinados.

Usando o parâmetro -DriverPath, você também pode passar uma matriz de caminhos para arquivos .inf de driver:

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\InjectingDrivers.vhdx -DriverPath .\Extra\Drivers\netcard64.inf`

### <a name="connecting-with-winrm"></a>Conectar-se ao WinRM  
Para ser capaz de se conectar a um computador Nano Server usando o WinRM (Windows Remote Management) (de outro computador que não esteja na mesma sub-rede), abra a porta 5985 para tráfego de entrada TCP na imagem do Nano Server. Use este cmdlet:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\ConnectingOverWinRM.vhd -EnableRemoteManagementPort`  

### <a name="setting-static-ip-addresses"></a>Configurar endereços IP estáticos  
Para configurar uma imagem do Nano Server a fim de usar endereços IP estáticos, primeiro localize o nome ou índice da interface que você quer modificar usando o Console de Recuperação do Nano Server, netsh ou Get-NetAdapter. Use os parâmetros -Ipv6Address, -Ipv6Dns, -Ipv4Address, -Ipv4SubnetMask, -Ipv4Gateway e -Ipv4Dns para especificar a configuração, como neste exemplo:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\StaticIpv4.vhd -InterfaceNameOrIndex Ethernet -Ipv4Address 192.168.1.2 -Ipv4SubnetMask 255.255.255.0 -Ipv4Gateway 192.168.1.1 -Ipv4Dns 192.168.1.1`  

### <a name="custom-image-size"></a>Tamanho de imagem personalizado  
Você pode configurar a imagem do Nano Server para ser um VHD ou VHDX de expansão dinâmica com o parâmetro -MaxSize, como neste exemplo:  

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -MaxSize 100GB`  

### <a name="embedding-custom-data"></a>Inserir dados personalizados  
Para inserir seu próprio script ou binários na imagem do Nano Server, use o parâmetro -CopyPath para passar uma matriz de arquivos e diretórios a serem copiados. O parâmetro -CopyPath também pode aceitar uma tabela de hash para especificar o caminho de destino para arquivos e diretórios.

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\BigBoss.vhd -CopyPath .\tools`

### <a name="running-custom-commands-after-the-first-boot"></a>Executar comandos personalizados após a primeira inicialização
Para executar comandos personalizados como parte do setupcomplete.cmd, use o parâmetro -SetupCompleteCommand para passar uma matriz de comandos:

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -SetupCompleteCommand @("echo foo", "echo bar")`


### <a name="running-custom-powershell-scripts-as-part-of-image-creation"></a>Executar scripts personalizados do PowerShell como parte da criação de imagem
Para executar scripts personalizados do PowerShell como parte do processo de criação de imagem, use o parâmetro -OfflineScriptPath para passar uma matriz de caminhos para scripts .ps1. Se esses scripts usam argumentos, use -OfflineScriptArgument para passar uma tabela de hash de argumentos adicionais para os scripts.

`New-NanoServerImage -DeploymentType Host -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -OfflineScriptPath C:\MyScripts\custom.ps1 -OfflineScriptArgument @{Param1="Value1"; Param2="Value2"}`


### <a name="support-for-development-scenarios"></a>Suporte para cenários de desenvolvimento
Se você quiser desenvolver e testar no Nano Server, use o parâmetro -Development. Isso habilitará o PowerShell como o shell local padrão, a instalação de drivers não assinados, a cópia de binários do depurador, a abertura de uma porta de depuração, a assinatura de teste e a instalação de pacotes AppX sem uma licença de desenvolvedor:

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`


### <a name="custom-unattend-file"></a>Arquivo autônomo personalizado  
Se você quiser usar seu próprio arquivo autônomo, use o parâmetro -UnattendPath:  

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -UnattendPath \\path\to\unattend.xml`  

A especificação de um nome de computador ou senha de administrador nesse arquivo autônomo substituirá os valores definidos por -AdministratorPassword e -ComputerName. 

> [!NOTE]
> O Nano Server não é compatível com configurações de TCP/IP por meio de arquivos autônomos. Você pode usar o Setupcomplete.cmd para definir as configurações de TCP/IP.

### <a name="collecting-log-files"></a>Coletar arquivos de log
Se você quiser coletar os arquivos de log durante a criação de imagem, use o parâmetro -LogPath para especificar um diretório onde todos os arquivos de log são copiados.

`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -LogPath C:\Logs`


> [!NOTE]
> Alguns parâmetros em New-NanoServerImage e Edit-NanoServerImage são somente para uso interno e podem ser ignorados com segurança. Isso inclui os parâmetros -SetupUI e -Internal.


## <a name="installing-apps-and-drivers"></a>Instalar aplicativos e drivers
[comment]: # (De Xumin Sun, bugs 68620 #.)  

### <a name="windows-server-app-installer"></a>Instalador do Windows Server App
O instalador do WSA (Windows Server App) fornece uma opção de instalação confiável para o Nano Server. Como não há suporte para MSI (Windows Installer) no Nano Server, o WSA também é a única tecnologia de instalação disponível para produtos que não são da Microsoft. O WSA aproveita a tecnologia de pacote de aplicativos do Windows, criada para instalar e fazer manutenção de aplicativos de serviço com segurança e confiabilidade, usando um manifesto declarativo. Ele estende o instalador de pacotes de aplicativo do Windows para oferecer suporte a extensões específicas do Windows Server, com a limitação que o WSA não oferece suporte à instalação de drivers.

A criação e instalação de um pacote WSA no Nano Server envolve etapas para o publicador e o consumidor do pacote.

O editor do pacote deve fazer o seguinte:

1. Instale [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk), que inclui as ferramentas necessárias para criar um pacote WSA: MakeAppx, MakeCert, Pvk2Pfx, SignTool.
2. Declare um manifesto: Siga as [esquema de extensão de manifesto do WSA](https://msdn.microsoft.com/library/windows/apps/mt670653.aspx) para criar o arquivo de manifesto, appxmanifest. XML.
3. Use a ferramenta **MakeAppx** para criar um pacote WSA.
4. Use as ferramentas **MakeCert** e **Pvk2Pfx** para criar o certificado e, em seguida, use **Signtool** para assinar o pacote.

Em seguida, o consumidor do pacote deverá executar estas etapas:

1. Execute o cmdlet do PowerShell [*Import-Certificate*](https://technet.microsoft.com/library/hh848630) para importar o certificado do editor da Etapa 4 acima para o Nano Server com certStoreLocation em "Cert: \LocalMachine\TrustedPeople". Por exemplo: `Import-Certificate -FilePath ".\xyz.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedPeople"`
2. Instale o aplicativo no Nano Server executando o cmdlet do PowerShell [**Add-AppxPackage**](https://technet.microsoft.com/library/mt575516(v=wps.620).aspx) para instalar um pacote WSA no Nano Server. Por exemplo: `Add-AppxPackage wsaSample.appx`

#### <a name="additional-resources-for-creating-apps"></a>Recursos adicionais para criação de aplicativos
WSA é a extensão do servidor da tecnologia de pacote de aplicativos do Windows (embora não esteja hospedado na Microsoft Store). Se você quiser publicar aplicativos com WSA, estes tópicos ajudarão você a se familiarizar com o pipeline de pacote de aplicativo:

- [Como criar um manifesto de pacote básico](https://msdn.microsoft.com/library/windows/desktop/br211475.aspx)
- [App Packager (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)
- [Como criar um certificado de assinatura de pacote do aplicativo](https://msdn.microsoft.com/library/windows/desktop/jj835832(v=vs.85).aspx)
- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)

### <a name="installing-drivers-on-nano-server"></a>Instalar drivers no Nano Server
Você pode instalar drivers de terceiros no Nano Server usando pacotes de driver INF. Entre eles estão pacotes de driver PnP (Plug-and-Play) e pacotes de driver do Filtros de Sistema de Arquivos. Não há suporte no momento para drivers de filtro de rede no Nano Server.

Os pacotes de driver PnP e do Filtro de Sistema de Arquivos devem seguir os requisitos de driver Universal e o processo de instalação, bem como diretrizes gerais de pacote de driver, como assinatura. Essas informações estão documentadas nestes locais:

- [Assinatura de driver](https://msdn.microsoft.com/windows/hardware/drivers/install/driver-signing)
- [Usando um arquivo INF Universal](https://msdn.microsoft.com/windows/hardware/drivers/install/using-a-configurable-inf-file)

#### <a name="installing-driver-packages-offline"></a>Instalar pacotes de driver offline

Os pacotes de driver com suporte podem ser instalados offline no Nano Server por meio dos cmdlets [DISM.exe](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/dism-driver-servicing-command-line-options-s14) ou [DISM PowerShell](https://technet.microsoft.com/library/dn376497.aspx).

#### <a name="installing-driver-packages-online"></a>Instalar pacotes de driver online
Pacotes de driver PnP podem ser instalados online no Nano Server usando [PnpUtil](https://msdn.microsoft.com/library/windows/hardware/ff550419(v=vs.85).aspx). Não há suporte no momento para a instalação de pacotes de driver online não PnP no Nano Server.






--------------------------------------------------  


## <a name="BKMK_JoinDomain"></a>Adicionar um Nano Server em um domínio  

### <a name="to-add-nano-server-to-a-domain-online"></a>Para adicionar o Nano Server a um domínio online  

1.  Colete um blob de dados de um computador no domínio que já está executando o Windows Threshold Server usando este comando:  

    `djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  

    Isso salva o blob de dados em um arquivo chamado "odjblob".  

2.  Copie o arquivo "odjblob" no computador do Nano Server com estes comandos:  

    **NET use z: \\ \\ \<endereço ip do Nano Server > \c$**  

    > [!NOTE]  
    > Se o comando net use falhar, você provavelmente precisa ajustar as regras de Firewall do Windows. Para fazer isso, primeiro abra um prompt de comandos com privilégios elevados, inicie o Windows PowerShell e conecte-se ao computador do Nano Server com o Windows PowerShell Remoting usando estes comandos:  
    >   
    > `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
    >   
    > `$ip = "<ip address of Nano Server>"`  
    >   
    > `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  
    >   
    > Ao receber uma solicitação, forneça a senha de Administrador e execute este comando para definir a regra de firewall:  
    >   
    > **grupo de regras de conjunto de netsh advfirewall firewall = "Compartilhamento de arquivos e impressora" Habilitar nova = Sim**  
    >   
    > Saia do Windows PowerShell com `Exit-PSSession` e repita o comando net use. Se for bem-sucedido, continue copiando o conteúdo do arquivo "odjblob" no Nano Server.  

    **md z:\Temp**  

    **copy odjblob z:\Temp**  

3.  Verifique o domínio no qual você deseja ingressar o Nano Server e verifique se o DNS está configurado. Além disso, verifique se essa resolução de nome do domínio, ou um controlador de domínio, funciona conforme o esperado. Para fazer isso, abra um prompt de comandos com privilégios elevados, inicie o Windows PowerShell e conecte-se ao computador do Nano Server com o Windows PowerShell Remoting usando estes comandos:  

    `Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  

    `$ip = "<ip address of Nano Server>"`  

    `Enter-PSSession -ComputerName $ip -Credential $ip\Administrator`  

    Quando receber uma solicitação, forneça a senha de Administrador. O Nslookup não está disponível no Nano Server, mas você pode verificar a resolução de nomes com Resolve-DNSName.

4. Se a resolução de nome for bem-sucedida, na mesma sessão do Windows PowerShell, execute este comando para ingressar no domínio:  

    `djoin /requestodj /loadfile c:\Temp\odjblob /windowspath c:\windows /localos`  

5.  Reinicie o computador do Nano Server e, em seguida, encerre a sessão do Windows PowerShell:  

    `shutdown /r /t 5`  

    `Exit-PSSession`  

6.  Após ingressar o Nano Server em um domínio, adicione a conta de usuário de domínio ao grupo de Administradores no Nano Server.

7. Para segurança, remova o Nano Server da lista de hosts confiáveis com este comando: `Set-Item WSMan:\localhost\client\TrustedHosts ""` 

**Método alternativo para ingressar em um domínio em uma única etapa**  

Primeiro, colete o blob de dados de outro computador que esteja executando o Windows Threshold Server, e que já esteja em seu domínio, usando este comando:  

`djoin.exe /provision /domain <domain-name> /machine <machine-name> /savefile .\odjblob`  

Abra o arquivo "odjblob" (talvez no Bloco de notas), copie o conteúdo e cole-o na seção \<AccountData> do arquivo Unattend.xml abaixo.  

Coloque o arquivo Unattend.xml na pasta C:\NanoServer e use os seguintes comandos para montar o VHD e aplicar as configurações na seção `offlineServicing`:  

**dism\dism /Mount-ImagemediaFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir**  

**dism\dismmedia:.\mountdir /Apply-Unattend:.\unattend.xml**  

Crie uma pasta "Panther" (usada pelos sistemas Windows para armazenar arquivos durante a instalação; confira [Locais de arquivos de log de instalação do Windows 7, Windows Server 2008 R2 e Windows Vista](https://support.microsoft.com/en-us/kb/927521) se você estiver curioso), copie o arquivo Unattend.xml nela e, em seguida, desmonte o VHD com estes comandos:  

**md .\mountdir\windows\panther**  

**copy .\unattend.xml .\mountdir\windows\panther**  

**dism\dism /Unmount-Image /MountDir:.\mountdir /Commit**  

Na primeira vez que você inicializa o Nano Server nesse VHD, as outras configurações serão aplicadas.  

Após ingressar o Nano Server em um domínio, adicione a conta de usuário de domínio ao grupo de Administradores no Nano Server.  

## <a name="working-with-server-roles-on-nano-server"></a>Trabalhar com funções de servidor no Nano Server

### <a name="using-hyper-v-on-nano-server"></a>Usar o Hyper-V no Nano Server  
O Hyper-V funciona da mesma maneira no Nano Server como no Windows Server no modo Server Core, com duas exceções:  

-   Você deve executar todo o gerenciamento remotamente, e o computador de gerenciamento deve estar executado a mesma compilação do Windows Server que o Nano Server. Versões mais antigas do Gerenciador do Hyper-V ou dos cmdlets do Windows PowerShell Hyper-V não funcionarão.  

-   RemoteFX não está disponível.  

Nesta versão, esses recursos do Hyper-V foram verificados:  

-   Habilitar o Hyper-V  

-   Criação de máquinas virtuais de 1ª e 2ª geração  

-   Criação de comutadores virtuais  

-   Iniciar as máquinas virtuais e executar sistemas operacionais convidados do Windows  
- Réplica do Hyper-V  



Se você quiser executar uma migração ao vivo das máquinas virtuais, criar uma máquina virtual em um compartilhamento SMB ou conectar recursos em um compartilhamento SMB existente em uma máquina virtual existente, é fundamental que você configure a autenticação corretamente. Você tem duas opções para fazer isso:  

**Delegação restrita**  

A delegação restrita funciona exatamente como nas versões anteriores. confira estes artigos para saber mais:  

-   [Habilitar o gerenciamento remoto do Hyper-V - configurar a delegação restrita para SMB e SMB altamente disponível](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-smb-and-highly-available-smb.aspx)  

-   [Habilitar o gerenciamento remoto do Hyper-V - configurar a delegação restrita para migração ao vivo não clusterizado](http://blogs.msdn.com/b/taylorb/archive/2012/03/20/enabling-hyper-v-remote-management-configuring-constrained-delegation-for-non-clustered-live-migration.aspx)  

**CredSSP**  

Primeiro, confira a seção "Usar a comunicação remota do Windows PowerShell" deste tópico para ativar e testar CredSSP. Em seguida, no computador de gerenciamento, você pode usar o Gerenciador do Hyper-V e selecionar a opção "conectar-se como outro usuário". O Gerenciador do Hyper-V usará CredSSP. Você deve fazer isso mesmo se estiver usando sua conta atual.  

Os cmdlets do Windows PowerShell para Hyper-V podem usar parâmetros CimSession ou Credential, que funcionam com o CredSSP.  

### <a name="BKMK_Failover"></a>Usando Clustering de Failover no Nano Server  
O clustering de failover funciona da mesma forma no Nano Server que no Windows Server no modo Server Core, mas lembre-se dessas advertências:  

-   Os clusters devem ser gerenciados remotamente com o Gerenciador de Cluster de Failover ou o Windows PowerShell.  

-   Todos os nós de cluster do Nano Server devem estar ingressados no mesmo domínio, semelhante aos nós de cluster no Windows Server.  

-   A conta de domínio deve ter privilégios de Administrador em todos os nós do Nano Server, assim como acontece com nós de cluster no Windows Server.  

-   Todos os comandos devem ser executados em um prompt de comandos com privilégios elevados.  

> [!NOTE]  
> Além disso, determinados recursos não recebem suporte nesta versão:  
>   
> -   Você não pode executar cmdlets de clustering de failover em um Nano Server local por meio do Windows PowerShell.  
> -   Funções de clustering diferentes do Hyper-V e Servidor de arquivos.  

Estes cmdlets do Windows PowerShell são úteis no gerenciamento de clusters de Failover:  

Você pode criar um novo cluster com `New-Cluster -Name <clustername> -Node <comma-separated cluster node list>`  

Depois de estabelecer um novo cluster, você deve executar `Set-StorageSetting -NewDiskPolicy OfflineShared` em todos os nós.  

Adicionar um nó adicional no cluster com `Add-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  

Remover um nó de cluster com  `Remove-ClusterNode -Name <comma-separated cluster node list>  -Cluster <clustername>`  

Criar um servidor de arquivos de escalabilidade horizontal com `Add-ClusterScaleoutFileServerRole -name <sofsname> -cluster <clustername>`  

Você pode encontrar outros cmdlets para clustering de failover em [Microsoft.FailoverClusters.PowerShell](https://technet.microsoft.com/library/ee461009.aspx).  

### <a name="BKMK_DNS"></a>Usando o servidor DNS no Nano Server  
Para fornecer ao Nano Server a função de Servidor DNS, adicione o Microsoft-NanoServer-DNS-Package à imagem (confira a seção "Criar uma imagem personalizada do Nano Server" deste tópico. Quando o Nano Server estiver em execução, conecte-se a ele e execute esse comando em um console do Windows PowerShell com privilégios elevados para habilitar o recurso:  

`Enable-WindowsOptionalFeature -Online -FeatureName DNS-Server-Full-Role`  

### <a name="BKMK_IIS"></a>Usando o IIS no Nano Server  
Para obter as etapas para usar a função de IIS (Serviços de Informações da Internet), confira [IIS no Nano Server](IIS-on-Nano-Server.md). 

### <a name="using-mpio-on-nano-server"></a>Usar o MPIO no Nano Server
Para obter as etapas para usar o MPIO, confira [MPIO no Nano Server](MPIO-on-Nano-Server.md) 

### <a name="BKMK_SSH"></a>Usando o SSH no Nano Server
Para obter instruções sobre como instalar e usar o SSH no Nano Server com o projeto OpenSSH, confira o [Wiki Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/wiki).

## <a name="appendix-sample-unattendxml-file-that-joins-nano-server-to-a-domain"></a>Apêndice: Arquivo Unattend. XML de exemplo que ingressa o Nano Server em um domínio  

> [!NOTE]  
> Exclua o espaço à direita no conteúdo de "odjblob" quando você colar no arquivo Unattend.  

```  
<?xml version='1.0' encoding='utf-8'?>  
<unattend xmlns="urn:schemas-microsoft-com:unattend" xmlns:wcm="https://schemas.microsoft.com/WMIConfig/2002/State" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  

  <settings pass="offlineServicing">  
    <component name="Microsoft-Windows-UnattendedJoin" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
        <OfflineIdentification>                
           <Provisioning>    
             <AccountData> AAAAAAARUABLEABLEABAoAAAAAAAMABSUABLEABLEABAwAAAAAAAAABbMAAdYABc8ABYkABLAABbMAAEAAAAMAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABAsAAAAAAAQAAZoABNUABOYABZYAANQABMoAAOEAAMIAAOkAANoAAMAAAXwAAJAAAAYAAA0ABY4ABZ8ABbIABa0AAcIABY4ABb8ABZUABLEAALMABLQABU0AATMABXAAAAAAAKdf/mhfXoAAUAAAQAAAAb8ABLQABbMABcMABb4ABc8ABAIAAAAAAb8ABLQABbMABcMABb4ABc8ABLQABb0ABZIAAGAAAAsAAR4ABTQABUAAAAAAACAAAQwABZMAAZcAAUgABVcAAegAARcABKkABVIAASwAAY4ABbcABW8ABQoAAT0ABN8AAO8ABekAAJMAAVkAAZUABckABXEABJUAAQ8AAJ4AAIsABZMABdoAAOsABIsABKkABQEABUEABIwABKoAAaAABXgABNwAAegAAAkAAAAABAMABLIABdIABc8ABY4AADAAAA4AAZ4ABbQABcAAAAAAACAAkKBW0ID8nJDWYAHnBAXE77j7BAEWEkl+lKB98XC2G0/9+Wd1DJQW4IYAkKBAADhAnKBWEwhiDAAAM2zzDCEAM6IAAAgAAAAAAAQAAAAAAAAAAAABwzzAAA  
</AccountData>  
           </Provisioning>    
         </OfflineIdentification>    
    </component>  
  </settings>  

  <settings pass="oobeSystem">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <UserAccounts>  
        <AdministratorPassword>  
           <Value>Tuva</Value>  
           <PlainText>true</PlainText>  
        </AdministratorPassword>  
      </UserAccounts>  
      <TimeZone>Pacific Standard Time</TimeZone>  
    </component>  
  </settings>  

  <settings pass="specialize">  
    <component name="Microsoft-Windows-Shell-Setup" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" versionScope="nonSxS">  
      <RegisteredOwner>My Team</RegisteredOwner>  
      <RegisteredOrganization>My Corporation</RegisteredOrganization>  
    </component>  
  </settings>  
</unattend>  
```  

