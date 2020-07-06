---
title: Canais de manutenção
description: Explicação de canais de serviço do Windows Server – LTSC e SAC
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 05/21/2019
ms.openlocfilehash: 2bf56e69d1a28007c35c320d1d5cc73c2ba9fa53
ms.sourcegitcommit: 643a9916efb95ad0bb5cc0a9b115ac29af4cb076
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85586693"
---
# <a name="windows-server-servicing-channels-ltsc-and-sac"></a>Canais de manutenção do Windows Server: LTSC e SAC

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Há dois canais de versão principal disponíveis para clientes do Windows Server, o Canal de Manutenção em Longo Prazo e no Canal Semestral.

É possível manter os servidores no LTSC (Canal de Manutenção em Longo Prazo), transferi-los para o Canal Semestral ou ter alguns servidores em qualquer faixa, dependendo de qual funciona melhor para suas necessidades.

## <a name="long-term-servicing-channel-ltsc"></a>LTSC (Canal de Manutenção em Longo Prazo)

É o modelo de versão que você já conhece (anteriormente chamado de "*Branch* de Manutenção de Longo Prazo") em que uma nova versão principal do Windows Server é liberada a cada dois ou três anos. Os usuários têm direito a cinco anos de suporte base e a cinco anos de suporte estendido. O canal é apropriado para sistemas que exigem uma opção de manutenção mais longa e estabilidade funcional. As implantações do Windows Server 2019 e versões anteriores do Windows Server não serão afetadas pelas novas versões do Canal Semestral. O Canal de Manutenção em Longo Prazo continuará a receber atualizações de segurança e não relacionadas à segurança, mas não receberá os novos recursos e funcionalidade.

> [!Note]
> **O produto LTSC atual é o Windows Server 2019**. Se quiser ficar nesse canal, você deverá instalar (ou continuar a usar) o Windows Server 2019, que pode ser instalado na opção de instalação Server Core ou na opção de instalação Servidor com Experiência Desktop.

## <a name="semi-annual-channel"></a>Canal Semestral

O Canal Semestral é perfeito para que clientes que estão inovando rapidamente aproveitem as novas funcionalidades do sistema operacional em um ritmo mais rápido, com foco em contêineres e microsserviços. Os produtos do Windows Server no Canal Semestral disponibilizarão novos lançamentos duas vezes por ano, no primeiro e segundo semestres. Cada versão neste canal será compatível por 18 meses a partir do lançamento inicial.

A maioria dos recursos introduzidos no Canal Semestral será acumulada na próxima versão do Canal de Manutenção em Longo Prazo do Windows Server. As edições, a funcionalidade e o conteúdo de suporte podem variar de versão para versão dependendo de comentários de clientes.

O Canal Semestral estará disponível para clientes de licença de volume com [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx), bem como por meio do Azure Marketplace ou outro provedor de nuvem/serviços de hospedagem e programas de fidelidade como as Assinaturas do Visual Studio.

> [!Note]  
> **A versão atual do Canal Semestral é a versão 1909 do Windows Server**. Se você deseja colocar servidores nesse canal, instale o Windows Server, versão 1909, que pode ser instalado no modo Server Core ou como um Nano Server executado em um contêiner. As atualizações in-loco de uma versão do Canal de Manutenção em Longo Prazo não são compatíveis, porque estão em **canais de versões diferentes**. As versões do Canal Semestral não são atualizações – são a próxima versão do Windows Server no Canal Semestral.

Nesse modelo, as versões do Windows Server são identificadas por ano e mês de lançamento: por exemplo, em 2017, uma versão do nono mês (setembro) seria identificada como **versão 1709**. Novas versões do Windows Server no Canal Semestral ocorrerão duas vezes por ano. O ciclo de vida de suporte para cada versão é 18 meses.

## <a name="should-you-keep-servers-on-the-ltsc-or-move-them-to-the-semi-annual-channel"></a>É necessário manter os servidores no LTSC ou movê-los para o Canal Semestral?

Estas são as principais diferenças para levar em consideração:

- Você precisa fazer upgrade para uma nova tecnologia no DevOps, nos contêineres e nos microsserviços? Nesse caso, considere **ingressar no Canal Semestral** ao instalar o **Windows Server, versão 1909**. Conforme descrito neste tópico, você receberá novas versões duas vezes por ano, com 18 meses de suporte base de produção por versão. Você pode obtê-lo por meio de licenciamento por volume, do Azure ou dos Serviços de Assinatura do Visual Studio. Atualmente, as versões no Canal Semestral exigem licenciamento por volume e Software Assurance se você pretende executar o produto na produção.
- Você precisa de estabilidade e de previsibilidade? Você precisa executar máquinas virtuais e cargas de trabalho tradicionais em servidores físicos? Nesse caso, considere **manter esses servidores no Canal de Manutenção em Longo Prazo**. A versão atual do LTSC é o **Windows Server 2019**. Conforme descrito neste tópico, você terá acesso às novas versões a cada dois ou três anos, com cinco anos de suporte base seguidos de cinco anos de suporte estendido por versão. As versões LTSC estão disponíveis em todos os mecanismos de lançamento. As versões do LTSC estão disponíveis para qualquer pessoa, independentemente do modelo de licenciamento usado. 

A tabela a seguir resume as principais diferenças entre os canais:


|                       |                                                              Canal de Manutenção em Longo Prazo (Windows Server 2019)                                                               |                                   Canal Semestral (Windows Server)                                   |
|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| Cenários recomendados | Servidores de arquivos de uso geral, cargas de trabalho da Microsoft ou de terceiros, aplicativos tradicionais, funções de infraestrutura, Datacenter definido por software e infraestrutura hiperconvergente | Aplicativos em contêineres, hosts do contêiner e cenários de aplicativos se beneficiando da inovação mais rápida |
|     Lançamentos      |                                                                               A cada 2 a 3 anos                                                                                |                                              A cada 6 meses                                              |
|        Suporte        |                                                       5 anos de suporte base, além de 5 anos de suporte estendido                                                        |                                                18 meses                                                 |
|       Edições        |                                                                    Todas as edições disponíveis do Windows Server                                                                     |                                     Edições Standard e Datacenter                                     |
|      Quem pode usar      |                                                                      Todos os clientes por meio de todos os canais                                                                      |                               Somente para clientes do Software Assurance e de nuvem                                |
| Opções de instalação  |                                                                Server Core e Server com Experiência Desktop                                                                |                 Server Core para imagem e host de contêiner e imagem de contêiner do Nano Server                 |

## <a name="device-compatibility"></a>Compatibilidade de dispositivo

A menos que seja comunicado, os requisitos mínimos de hardware para executar as versões de Canal Semestral serão os mesmos da versão mais recente do Canal de Manutenção em Longo Prazo do Windows Server. Por exemplo, **a versão atual do Canal de Manutenção em Longo Prazo é o Windows Server 2019**. A maioria dos drivers de hardware continuará funcionando nessas versões.

## <a name="servicing"></a>Manutenção

O versões do Canal de Manutenção em Longo Prazo e do Canal Semestral são compatíveis com atualizações de segurança e atualizações não relacionadas à segurança. A diferença é o período de tempo em que a versão é compatível, conforme descrito acima.

### <a name="servicing-tools"></a>Ferramentas de manutenção

Existem muitas ferramentas com as quais os profissionais de TI podem fazer a manutenção do Windows Server. Cada opção tem seus prós e contras, variando de funcionalidades e controle até a simplicidade e os poucos requisitos administrativos. Os exemplos a seguir são de ferramentas de manutenção disponíveis para gerenciar atualizações de manutenção:

- **Windows Update (autônomo)** : essa opção só está disponível para os servidores conectados à Internet e que têm o Windows Update habilitado.
- O **WSUS (Windows Server Update Services)** fornece controle abrangente sobre as atualizações do Windows 10 e do Windows Server e está nativamente disponível no sistema operacional Windows Server. Além da capacidade de adiar atualizações, as organizações podem adicionar uma camada de aprovação de atualizações e optar por implantá-las em computadores ou grupos de computadores específicos quando estiverem prontas.
- O **Microsoft Endpoint Configuration Manager** fornece controle máximo sobre a manutenção. Os profissionais de TI podem adiar e aprovar as atualizações, além de contar com várias opções para direcionar as implantações e gerenciar o uso de largura de banda e os tempos de implantação.

Você provavelmente já escolheu usar pelo menos uma dessas opções com base em seus recursos, equipe e experiência. É possível continuar a usar o mesmo processo para as versões do Canal Semestral. Por exemplo, se você já usar o Configuration Manager para gerenciar atualizações, poderá continuar a usá-lo. Da mesma forma, se estiver usando o WSUS, você poderá continuar a usá-lo.

## <a name="where-to-obtain-semi-annual-channel-releases"></a>Onde obter as versões do Canal Semestral

As versões do Canal Semestral devem ser instaladas como uma instalação limpa.

- VLSC (Centro de Serviços de Licenciamento por Volume): os clientes de licenciamento por volume com o [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx) podem obter essa versão ao acessar o [Centro de Serviços de Licenciamento por Volume](https://www.microsoft.com/Licensing/servicecenter/default.aspx) e clicar em **Entrar**. Em seguida, clique em **Downloads e Chaves** e procure por esta versão.

- As versões do Canal Semestral também estão disponíveis no [Microsoft Azure](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview).

- Assinaturas do Visual Studio: os assinantes do Visual Studio podem obter as versões do Canal Semestral baixando-as na [página de download do Assinante do Visual Studio](https://my.visualstudio.com/downloads?pid=2347). Caso ainda não seja um assinante, acesse [Assinaturas do Visual Studio](https://www.visualstudio.com/subscriptions/) para se inscrever e, em seguida, acesse a [página de download do Assinante do Visual Studio](https://my.visualstudio.com/downloads?pid=2347) como mostrado acima. As versões obtidas por meio de Assinaturas do Visual Studio destinam-se somente a desenvolvimento e teste.

- Obter versões prévias por meio do Programa Windows Insider: Testar os builds anteriores do Windows Server ajuda a Microsoft e seus clientes devido à oportunidade de descobrir possíveis problemas antes do lançamento. Também oferece aos clientes uma oportunidade única de influenciar diretamente a funcionalidade no produto.
A Microsoft depende dos comentários recebidos durante o processo de desenvolvimento para que os ajustes possam ser feitos o mais rápido possível. Os testes iniciais e os comentários são essenciais para o modelo de lançamento rápido. Para saber mais sobre como se envolver no Programa Windows Insider, consulte os [documentos do Programa Windows Insider para servidor](https://docs.microsoft.com/windows-insider/at-work/).

## <a name="activating-semi-annual-channel-releases"></a>Ativar as versões do Canal Semestral

- Se estiver usando o Microsoft Azure, esta versão deverá ser ativada automaticamente.
- Se obteve esta versão pelo Centro de Serviços de Licenciamento por Volume ou pelas Assinaturas do Visual Studio, você poderá ativá-la usando o CSVLK do Windows Server 2019 com seu ambiente de KMS (Sistema de Gerenciamento de Chaves). Para saber mais, confira as [chaves de configuração do cliente do KMS](../get-started/kmsclientkeys.md).

As versões do Canal Semestral lançadas antes do Windows Server 2019 usam o CSVLK do Windows Server 2016.

## <a name="why-do-semi-annual-channel-releases-offer-only-the-server-core-installation-option"></a>Por que as versões do Canal Semestral só oferecem a opção de instalação Server Core?

Uma das etapas mais importantes no planejamento de cada versão do Windows Server é ouvir os comentários dos clientes, ou seja, como você está usando o Windows Server? Quais novos recursos afetarão mais as implantações do Windows Server e, consequentemente, suas atividades diárias? Seus comentários nos dizem que a disponibilização de inovações do modo mais rápido e eficiente possível é uma prioridade. Ao mesmo tempo, para os clientes que inovam mais rapidamente, fomos informados que você está usando principalmente scripts de linha de comando com o PowerShell para gerenciar seus data centers e, dessa forma, não há uma grande necessidade de GUI de área de trabalho na instalação do Windows Server com Experiência Desktop, especialmente agora que o [Windows Admin Center](../manage/windows-admin-center/overview.md) está disponível para gerenciar remotamente os servidores.

Ao se concentrar na opção de instalação Server Core, é possível dedicar mais recursos para inovações e manter a funcionalidade e a compatibilidade do aplicativo tradicional da plataforma Windows Server. Caso tenha comentários sobre este ou outros problemas referentes ao Windows Server e às versões futuras, você pode fazer sugestões e comentários por meio do [Hub de Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app).

## <a name="what-about-nano-server"></a>E quanto ao Nano Server?

O Nano Server está disponível como um sistema operacional de contêiner no Canal Semestral. Confira [Mudanças no Nano Server no Canal Semestral do Windows Server](../get-started/nano-in-semi-annual-channel.md) para saber mais detalhes.

## <a name="how-to-tell-whether-a-server-is-running-an-ltsc-or-sac-release"></a>Como saber se um servidor está executando uma versão do LTSC ou do SAC

Em termos gerais, as versões do Canal de Manutenção em Longo Prazo, como o Windows Server 2019, são lançadas ao mesmo tempo que uma nova versão do Canal Semestral, por exemplo, o Windows Server, versão 1809. Desse modo, pode ficar mais difícil para determinar se um servidor está executando uma versão do Canal Semestral. Em vez de examinar o número de build, confira o nome do produto: As versões do Canal Semestral usam o nome de produto Windows Server Standard ou Windows Server Datacenter, sem o número da versão. Já as versões do Canal de Manutenção em Longo Prazo incluem o número da versão, por exemplo, Windows Server 2019 Datacenter.

> [!Note]
> As diretrizes abaixo se destinam a ajudar a identificar e diferenciar entre o LTSC e o SAC apenas para fins de inventário geral e ciclo de vida.  Não servem para compatibilidade do aplicativo nem para representar uma superfície de API específica.  Os desenvolvedores de aplicativos devem usar as diretrizes para garantir a compatibilidade corretamente já que componentes, APIs e funcionalidades podem ou não ter sido adicionados durante a vida útil de um sistema. A [versão do sistema operacional](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) é um ponto de partida melhor para os desenvolvedores de aplicativos.

Abra o Powershell e use o Cmdlet Get-ItemProperty ou Get-ComputerInfo para verificar essas propriedades no Registro.  Juntamente com o número de build, isso indicará o LTSC ou o SAC pela presença ou ausência do ano com marca, ou seja, 2019.  O LTSC apresenta essas indicações, mas o SAC não.  Isso também retornará a época da versão com ReleaseId ou WindowsVersion, ou seja, 1809, bem como se a instalação é Server Core ou Server com Experiência Desktop.

**Exemplo do Windows Server 2019 Datacenter Edition (LTSC) com Experiência Desktop:**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server 2019 Datacenter
ReleaseId                 : 1809
InstallationType          : Server
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Exemplo do Windows Server, versão 1809 (SAC) Standard Edition Server Core:**

````PowerShell
Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows NT\CurrentVersion" | Select ProductName, ReleaseId, InstallationType, CurrentMajorVersionNumber,CurrentMinorVersionNumber,CurrentBuild
````

````
ProductName               : Windows Server Standard
ReleaseId                 : 1809
InstallationType          : Server Core
CurrentMajorVersionNumber : 10
CurrentMinorVersionNumber : 0
CurrentBuild              : 17763
````

**Exemplo do Windows Server 2019 Standard Edition (LTSC) Server Core:**


````PowerShell
Get-ComputerInfo | Select WindowsProductName, WindowsVersion, WindowsInstallationType, OsServerLevel, OsVersion, OsHardwareAbstractionLayer
````

````
WindowsProductName            : Windows Server 2019 Standard
WindowsVersion                : 1809
WindowsInstallationType       : Server Core
OsServerLevel                 : ServerCore
OsVersion                     : 10.0.17763
OsHardwareAbstractionLayer    : 10.0.17763.107
````

Para consultar se o novo [FOD de compatibilidade de aplicativo do Server Core](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) está presente em um servidor, use o Cmdlet [Get-WindowsCapability](https://docs.microsoft.com/powershell/module/dism/get-windowscapability?view=win10-ps) e procure por:
````
Name    :     ServerCore.AppCompatibility~~~~0.0.1.0
State   :     Installed
````

## <a name="additional-references"></a>Referências adicionais

[Alterações no Nano Server no Canal Semestral do Windows Server](../get-started/nano-in-semi-annual-channel.md)

[Ciclo de vida do suporte ao Windows Server](https://support.microsoft.com/lifecycle)

[Determinar se o Server Core está em execução](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[Função GetProductInfo](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Cmdlets de registro em log de inventário de software](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
