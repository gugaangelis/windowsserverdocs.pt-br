---
title: Desenvolvimento de cmdlets do PowerShell para Nano Server
description: portabilidade CIM, cmdlets do .NET, C++
ms.prod: windows-server
manager: DonGill
ms.technology: server-nano
ms.topic: article
ms.assetid: 7b4267f0-1c91-4a40-9262-5daf4659f686
author: jaimeo
ms.author: jaimeo
ms.date: 09/06/2017
ms.localizationpriority: medium
ms.openlocfilehash: 3965e453483b3515e4957ecfaba39cf9a0b8104f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827069"
---
# <a name="developing-powershell-cmdlets-for-nano-server"></a>Desenvolvimento de cmdlets do PowerShell para Nano Server

>Aplica-se a: Windows Server 2016

> [!IMPORTANT]
> A partir do Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem do sistema operacional de base de contêiner](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa. 
  
## <a name="overview"></a>Visão geral  
O Nano Server inclui o PowerShell Core por padrão em todas as instalações do Nano Server. O PowerShell Core é uma edição de volume reduzido do PowerShell baseada no .NET Core e executada em edições de volume reduzido do Windows, como Nano Server e Windows IoT Core. O PowerShell Core funciona da mesma maneira que outras edições do PowerShell, como o Windows PowerShell em execução no Windows Server 2016. No entanto, o volume reduzido do Nano Server significa que nem todos os recursos do PowerShell do Windows Server 2016 estão disponíveis no PowerShell Core no Nano Server.  
  
Se você tiver cmdlets do PowerShell que queira executar no Nano Server, ou se estiver desenvolvendo novos cmdlets para essa finalidade, este tópico inclui dicas e sugestões que devem ajudar a tornar isso mais fácil.  

## <a name="powershell-editions"></a>Edições do PowerShell  
  
  
A partir da versão 5.1, o PowerShell está disponível em diferentes edições que denotam os vários conjuntos de recursos e compatibilidades de plataforma.  
  
- **Desktop Edition:** Criado no .NET Framework; fornece compatibilidade com scripts e módulos destinados a versões do PowerShell em execução em edições de volume completo do Windows, como Server Core e Windows Desktop.  
- **Core Edition:** Criado no .NET Core; fornece compatibilidade com scripts e módulos destinados a versões do PowerShell em execução em edições de volume reduzido do Windows, como Nano Server e Windows IoT.  
  
A edição do PowerShell em execução é exibida na propriedade PSEdition de $PSVersionTable.  
```powershell  
$PSVersionTable  
  
Name                           Value  
----                           -----  
PSVersion                      5.1.14300.1000  
PSEdition                      Desktop  
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}  
CLRVersion                     4.0.30319.42000  
BuildVersion                   10.0.14300.1000  
WSManStackVersion              3.0  
PSRemotingProtocolVersion      2.3  
SerializationVersion           1.1.0.1  
```  
  
Os autores de módulo podem declarar seus módulos como compatíveis com uma ou mais edições do PowerShell usando a chave de manifesto do módulo CompatiblePSEditions. Essa chave só é compatível com o PowerShell 5.1 ou posterior.  
```powershell  
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1  
$moduleInfo = Test-ModuleManifest -Path \TestModuleWithEdition.psd1  
$moduleInfo.CompatiblePSEditions  
Desktop  
Core  
  
$moduleInfo | Get-Member CompatiblePSEditions  
  
   TypeName: System.Management.Automation.PSModuleInfo  
  
Name                 MemberType Definition  
----                 ---------- ----------  
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}  
  
```  
Ao obter uma lista de módulos disponíveis, é possível filtrá-la por edição do PowerShell.  
```powershell  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Desktop  
  
    Directory: C:\Program Files\WindowsPowerShell\Modules  
  
  
ModuleType Version    Name                                ExportedCommands  
---------- -------    ----                                ----------------  
Manifest   1.0        ModuleWithPSEditions  
  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Core | % CompatiblePSEditions  
Desktop  
Core  
  
```  
Os autores de script podem impedir que um script seja executado, a menos que seja executado em uma edição compatível do PowerShell, usando o parâmetro PSEdition em uma instrução #requires.  
```powershell  
Set-Content C:\script.ps1 -Value #requires -PSEdition Core  
Get-Process -Name PowerShell  
Get-Content C:\script.ps1  
#requires -PSEdition Core  
Get-Process -Name PowerShell  
  
C:\script.ps1  
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a #requires statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.  
At line:1 char:1  
+ C:\script.ps1  
+ ~~~~~~~~~~~~~  
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException  
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition  
```  
  
  
## <a name="installing-nano-server"></a>Instalar o Nano Server  
Etapas detalhadas e de início rápido para instalar o Nano Server em máquinas virtuais ou físicas são apresentadas em [Install Nano Server](Getting-Started-with-Nano-Server.md) (Instalar o Nano Server), que é o tópico pai deste.  
  
> [!NOTE]  
> Para o trabalho de desenvolvimento no Nano Server, talvez seja útil instalar o Nano Server usando o parâmetro -Development de New-NanoServerImage. Isso habilitará a instalação de drivers não assinados, cópia de binários do depurador, abertura de uma porta de depuração, habilitação da assinatura de teste e habilitação da instalação de pacotes AppX sem uma licença de desenvolvedor. Por exemplo:  
>  
>`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`  
  
## <a name="determining-the-type-of-cmdlet-implementation"></a>Determinar o tipo de implementação do cmdlet  
O PowerShell dá suporte a vários tipos de implementação para cmdlets, e o que você usou determina os processos e ferramentas envolvidos na criação ou na portabilidade para funcionar no Nano Server. Os tipos de implementação com suporte são:  
* CIM - composto por arquivos CDXML disposto em camada sobre provedores CIM (WMIv2)   
* .NET - composto por assemblies .NET que implementam interfaces de cmdlet gerenciadas, geralmente escritas em C#   
* Script do PowerShell - composto por módulos de script (.psm1) ou scripts (.ps1) escritos na linguagem do PowerShell   
  
Se você não tiver certeza sobre a implementação usada para os cmdlets existentes que você quer portar, instale o produto ou recurso e procure a pasta de módulo do PowerShell em um dos seguintes locais:   
  
* %windir%\system32\WindowsPowerShell\v1.0\Modules   
* %ProgramFiles%\WindowsPowerShell\Modules   
* %UserProfile%\Documents\WindowsPowerShell\Modules   
* \<local de instalação do produto>   
    
  Verifique os seguintes detalhes nesses locais:  
  * Cmdlets do CIM têm extensões de arquivo .cdxml.  
  * Cmdlets de .NET têm extensões de arquivo .dll, ou têm assemblies instalados no GAC e listados no arquivo .psd1 nos campos RootModule, ModuleToProcess ou NestedModules.  
* Cmdlets de script do PowerShell têm extensões de arquivo .psm1 ou .ps1.   
  
## <a name="porting-cim-cmdlets"></a>Portabilidade de cmdlets do CIM  
Em geral, esses cmdlets devem funcionar no Nano Server sem a necessidade de qualquer conversão. No entanto, você deve transferir o provedor subjacente da WMI v2 para executar no Nano Server, caso ainda não tenha feito isso.  
  
### <a name="building-c-for-nano-server"></a>Compilação em C++ para Nano Server  
Para fazer as DLLs de C++ funcionarem no Nano Server, compile-as para Nano Server em vez de para uma edição específica.  
  
Para conferir os pré-requisitos e uma explicação passo a passo sobre o desenvolvimento em C++ no Nano Server, confira [Developing Native Apps on Nano Server (Desenvolver aplicativos nativos no Nano Server)](https://blogs.technet.com/b/nanoserver/archive/2016/04/27/developing-native-apps-on-nano-server.aspx).  
  
  
## <a name="porting-net-cmdlets"></a>Portabilidade de cmdlets do .NET  
Há suporte para a maioria dos códigos em C# no Nano Server. Você pode usar [ApiPort](https://github.com/Microsoft/dotnet-apiport) para verificar se há APIs incompatíveis.  
  
### <a name="powershell-core-sdk"></a>SDK do PowerShell Core  
O módulo Microsoft.PowerShell.NanoServer.SDK está disponível na [Galeria do PowerShell](https://www.powershellgallery.com/packages/Microsoft.PowerShell.NanoServer.SDK/) para facilitar o desenvolvimento de cmdlets em .NET usando a Atualização 2 do Visual Studio 2015 direcionados a versões do CoreCLR e do PowerShell Core disponíveis no Nano Server. Você pode instalar o módulo usando PowerShellGet com este comando:  
  
`Find-Module Microsoft.PowerShell.NanoServer.SDK -Repository PSGallery | Install-Module -Scope <scope>`  
  
O módulo do SDK do PowerShell Core expõe cmdlets para configurar os assemblies de referência corretos do CoreCLR e do PowerShell Core, crie um projeto em C# no Visual Studio 2015 abrangendo os assemblies de referência, e configure o depurador remoto em uma máquina do Nano Server para que os desenvolvedores possam depurar seus cmdlets .NET em execução remota no Nano Server no Visual Studio 2015.  
  
O módulo do SDK do PowerShell Core exige a Atualização 2 do Visual Studio 2015. Se você não tiver instalado o Visual Studio 2015, instale o [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx).  
  
O módulo do SDK também depende do seguinte recurso que deve estar instalado no Visual Studio 2015:  
  
- Desenvolvimento no Windows e na Web -> Ferramentas de Desenvolvimento de Aplicativo Universal do Windows -> Ferramentas (1.3.1) e SDK do Windows 10  
  
Revise a instalação do Visual Studio antes de usar o módulo do SDK para garantir que esses pré-requisitos tenham sido atendidos. Selecione a instalação dos recursos acima durante a instalação do Visual Studio, ou modifique sua instalação existente do Visual Studio 2015 para instalá-los.  
  
O módulo do SDK do PowerShell Core inclui os seguintes cmdlets:  
- New-NanoCSharpProject: Cria um novo projeto em C# do Visual Studio destinado ao CoreCLR e ao PowerShell Core incluídos na versão do Nano Server para Windows Server 2016.  
- Show-SdkSetupReadMe: Abre a pasta raiz do SDK no Explorador de Arquivos e abre o arquivo README.txt para configuração manual.  
- Install-RemoteDebugger: Instala e configura o depurador remoto do Visual Studio em uma máquina do Nano Server.  
- Start-RemoteDebugger: Inicia o depurador remoto em um computador remoto que esteja executando o Nano Server.  
- Stop-RemoteDebugger: Inicia o depurador remoto em um computador remoto que esteja executando o Nano Server.  
  
Para obter informações detalhadas sobre como usar esses cmdlets, execute Get-Help em cada cmdlet depois de instalar e importar o módulo, da seguinte maneira:  
  
`Get-Command -Module Microsoft.PowerShell.NanoServer.SDK | Get-Help -Full`   
  
  
### <a name="searching-for-compatible-apis"></a>Pesquisar APIs compatíveis  
  
Você pode pesquisar no catálogo de APIs por .NET Core ou desmontar assemblies de referência de Core CLR. Para saber mais sobre portabilidade de plataforma de APIs do .NET, confira [Portabilidade de plataforma](https://github.com/Microsoft/dotnet-apiport/blob/master/docs/HowTo/PlatformPortability.md)  
  
### <a name="pinvoke"></a>PInvoke  
No Core CLR usado pelo Nano Server, algumas DLLs fundamentais, como Kernel32.dll e Advapi32.dll, foram divididas em vários conjuntos de API, portanto, você precisará garantir que seu PInvokes faça referência à API correta. Qualquer incompatibilidade será manifestada como um erro de runtime.  
  
Para obter uma lista de APIs nativas com suporte no Nano Server, confira [APIs do Nano Server](https://msdn.microsoft.com/library/mt588480(v=vs.85).aspx).  
  
### <a name="building-c-for-nano-server"></a>Compilação em C# para Nano Server  
  
Depois de criar um projeto em C# no Visual Studio 2015 usando `New-NanoCSharpProject`, você pode simplesmente criá-lo no Visual Studio clicando no menu **Compilação** e selecionando **Compilar Projeto** ou **Compilar Solução**. Os assemblies gerados visarão o CoreCLR e PowerShell Core corretos que acompanham com o Nano Server, e basta copiar os assemblies em um computador que esteja executando o Nano Server e usá-los.  
  
### <a name="building-managed-c-cppcli-for-nano-server"></a>Compilar C++ (CPP/CLI) gerenciado para Nano Server  
Não há suporte para C++ gerenciado para CoreCLR. Ao portar para CoreCLR, reescreva o código C++ gerenciado em C# e faça todas as chamadas nativas por meio do PInvoke.  
  
## <a name="porting-powershell-script-cmdlets"></a>Portabilidade de cmdlets de script do PowerShell  
  
O PowerShell Core tem paridade completa de linguagem do PowerShell com outras edições do PowerShell, incluindo a edição em execução no Windows Server 2016 e no Windows 10. No entanto, ao portar cmdlets de script do PowerShell para o Nano Server, lembre-se destes fatores:  
* Há dependências de outros cmdlets? Se houver, esses cmdlets estarão disponíveis no Nano Server? Veja [PowerShell no Nano Server](PowerShell-on-Nano-Server.md) para saber mais sobre o que não está disponível.  
* Se houver dependências de assemblies carregados em runtime, eles ainda funcionarão?   
* Como você pode depurar o script remotamente?   
* Como você pode migrar do .Net do WMI para .Net de MI?  
  
### <a name="dependency-on-built-in-cmdlets"></a>Dependência de cmdlets internos  
Nem todos os cmdlets no Windows Server 2016 estão disponíveis no Nano Server (veja [PowerShell no Nano Server](PowerShell-on-Nano-Server.md)). A melhor abordagem é configurar uma máquina virtual do Nano Server e descobrir se os cmdlets que você precisa estão disponíveis. Para fazer isso, execute `Enter-PSSession` para se conectar ao Nano Server de destino e execute `Get-Command -CommandType Cmdlet, Function` para obter a lista de cmdlets disponíveis.  
  
### <a name="consider-using-powershell-classes"></a>Considere o uso de classes do PowerShell  
Add-Type tem suporte no Nano Server para compilação de código C# embutido. Se você estiver escrevendo um código novo ou se estiver portando um código existente, convém usar também as classes do PowerShell para definir tipos personalizados. Você pode usar as classes do PowerShell para cenários de recipiente do propriedades, bem como para Enums. Se você precisar executar um PInvoke, faça isso por meio de C# usando Add-Type, ou em um assembly previamente compilado.  
Veja um exemplo que mostra o uso de Add-Type:  
  
```  
Add-Type -ReferencedAssemblies ([Microsoft.Management.Infrastructure.Ciminstance].Assembly.Location) -TypeDefinition @'  
public class TestNetConnectionResult  
{  
   // The compute name  
   public string ComputerName = null;  
   // The Remote IP address used for connectivity  
   public System.Net.IPAddress RemoteAddress = null;  
}  
'@  
# Create object and set properties  
$result = New-Object TestNetConnectionResult  
$result.ComputerName = Foo  
$result.RemoteAddress = 1.1.1.1  
  
```  
 Esse exemplo mostra como usar classes do PowerShell no Nano Server:  
   
```  
class TestNetConnectionResult    
{    
   # The compute name  
  [string] $ComputerName    
  
  #The Remote IP address used for connectivity    
  [System.Net.IPAddress] $RemoteAddress  
}  
# Create object and set properties  
$result = [TestNetConnectionResult]::new()  
$result.ComputerName = Foo  
$result.RemoteAddress = 1.1.1.1  
  
```  
  
### <a name="remotely-debugging-scripts"></a>Depuração remota de scripts  
  
Para depurar remotamente um script, conecte-se ao computador remoto usando `Enter-PSsession` do ISE do PowerShell. Uma vez na sessão, você pode executar `psedit <file_path>` e uma cópia do arquivo será aberta em seu ISE do PowerShell local. Em seguida, você pode depurar o script como se estivesse sendo executado localmente por meio da definição de pontos de interrupção. Além disso, quaisquer alterações feitas neste arquivo serão salvas na versão remota.   
  
### <a name="migrating-from-wmi-net-to-mi-net"></a>Migrar do .NET de WMI para .NET de MI  
  
Não há suporte para [.NET de WMI](https://msdn.microsoft.com/library/mt481551(v=vs.110).aspx), portanto, todos os cmdlets que usam a API antiga deve migrar para a API de WMI com suporte: [MI. NET](https://msdn.microsoft.com/library/dn387184(v=vs.85).aspx). Você pode acessar o .NET de MI diretamente por meio C# ou os cmdlets no módulo CimCmdlets.   
  
### <a name="cimcmdlets-module"></a>Módulo de CimCmdlets  
  
Os cmdlets do WMI v1 (por exemplo, Get-WmiObject) não têm suporte no Nano Server. No entanto, há suporte para os cmdlets de CIM (por exemplo, Get-CimInstance) no módulo CimCmdlets. Os cmdlets de CIM mapeiam de forma próxima aos cmdlets do WMI v1. Por exemplo, Get-WmiObject correlaciona com Get-CimInstance usando parâmetros muito semelhantes. A sintaxe de invocação do método é ligeiramente diferente, mas é bem documentada via Invoke-CimMethod. Tenha cuidado com a digitação do parâmetro. .NET de MI possui requisitos mais estritos com relação aos tipos de parâmetro do método.  
  
### <a name="c-api"></a>API do C#  
  
.NET de WMI encapsula a interface WMIv1, enquanto o .NET de MI encapsula a interface WMIv2 (CIM). As classes expostas podem ser diferentes, mas as operações subjacentes são muito semelhantes. Enumere ou obtenha instâncias de objetos e invoque operações nelas para realizar tarefas.   
  
  


