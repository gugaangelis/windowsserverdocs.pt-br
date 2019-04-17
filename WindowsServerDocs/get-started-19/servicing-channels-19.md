---
title: Canais de manutenção
description: 'Explicação dos canais de serviço do Windows Server: LTSC e SAC'
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: high
ms.openlocfilehash: c4329339e37acbb0b589e767a570073f4ae1363f
ms.sourcegitcommit: e44bacf5bd4dbdd500dcfae803724b7888096582
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/07/2019
ms.locfileid: "9149762"
---
# Canais de manutenção do Windows Server: LTSC e SAC

>Aplicável a: Windows Server 2019, Windows Server 2016


**Há dois canais de versão principal disponível para clientes do Windows Server, o Canal de serviço em longo prazo e no Canal semestral.** 

Você pode manter os servidores no canal de serviço de Longo prazo (LTSC), transferi-los para o canal semestral ou ter alguns servidores em qualquer faixa, dependendo de qual funciona melhor para suas necessidades.


## Canal de Manutenção de Longo Prazo (LTSC)
Esse é o modelo de versão que você já está familiarizado com (anteriormente chamado de "*Branch* de Manutenção a Longo Prazo") em que uma nova versão principal do Windows Server é liberada a cada 2 ou 3 anos. Os usuários têm direito a cinco anos de suporte base e a cinco anos de suporte estendido. Esse canal é apropriado para sistemas que exigem um mais funcional estabilidade e a opção de manutenção. As implantações do Windows Server 2016 e versões anteriores do Windows Server não serão afetadas pelas novas versões de Canal semestral. O Canal de Manutenção a Longo Prazo continuará a receber atualizações não relacionadas à segurança e segurança, mas ele não receberá os novos recursos e funcionalidade.

> [!Note]  
> **O produto LTSC atual é o Windows Server 2019**. Se quiser ficar nesse canal, você deverá instalar (ou continuar a usar) o Windows Server 2019, que pode ser instalado na opção de instalação Server Core ou na opção de instalação Servidor com Experiência Desktop.

## Canal Semestral 
O Canal Semestral é perfeito para os clientes que estão inovando rapidamente aproveitarem os novos recursos do sistema operacional em um ritmo mais rápido, tanto em aplicativos, especialmente aqueles criados em contêineres e microsserviços, bem como no datacenter híbrido definido por software. Os produtos do Windows Server no Canal semestral receberão lançamentos duas vezes por ano, no primeiro e segundo semestres. Cada versão neste canal terá suporte por 18 meses a partir do lançamento inicial.

A maioria dos recursos introduzidos no canal semestral serão acumuladas na próxima versão do Canal de Manutenção a Longo Prazo do Windows Server. As edições, a funcionalidade e o conteúdo de suporte podem variar de versão para versão dependendo de comentários de clientes.

O Canal semestral estará disponível para clientes de licença de volume com [Software Assurance](https://www.microsoft.com/en-us/licensing/licensing-programs/software-assurance-default.aspx), bem como por meio do Azure Marketplace ou outro provedor de nuvem/serviços de hospedagem, bem como programas de lealdade Assinaturas do Visual Studio.

> [!Note]  
> **A versão atual do canal semestral é para o Windows Server, versão 1809**. Se quiser colocar servidores nesse canal, você deverá instalar o Windows Server, versão 1809, que pode ser instalado no modo Server Core ou no Nano Server executado em um contêiner. Os upgrades in-loco do Windows Server 2016 para Windows Server, versão 1809 não têm suporte porque estão em **canais de versão diferentes**. O Windows Server, a versão 1809 não é uma atualização para o Windows Server 2016, é a próxima versão do Windows Server no canal semestral.



Nesse modelo, as versões do Windows Server são identificadas por ano e mês após o lançamento: por exemplo, em 2017, uma versão do mês 9 (setembro) seria identificada como **versão 1709**. Novas versões do Windows Server no Canal Semestral ocorrerão duas vezes por ano. O ciclo de vida de suporte para cada versão é 18 meses.

## Você deve manter os servidores no LTSC ou movê-los para o Canal Semestral?
Estas são as principais diferenças para levar em consideração:

- Você precisa inovar com rapidez? Você precisa de acesso antecipado aos recursos mais recentes do Windows Server? Você precisa dar suporte a aplicativos híbridos de ritmo rápido, a operações de desenvolvimento e a malhas do Hyper-V? Nesse caso, você deve considerar **ingressar no Canal Semestral** ao instalar o **Windows Server, versão 1809**. Conforme descrito neste tópico, você receberá novas versões duas vezes por ano, com 18 meses de suporte base de produção por versão. Você pode obtê-lo por meio de licenciamento por volume, do Azure ou dos Serviços de Assinatura do Visual Studio. Atualmente, as versões no Canal Semestral exigem licenciamento por volume e Software Assurance se você pretende executar o produto em produção.
- Você precisa de estabilidade e de previsibilidade? Você precisa executar máquinas virtuais e cargas de trabalho tradicionais em servidores físicos? Nesse caso, você deve considerar **manter esses servidores no Canal de Manutenção a Longo Prazo**. A versão atual do LTSC é o **Windows Server 2019**. Conforme descrito neste tópico, você terá acesso às novas versões a cada dois ou três anos, com cinco anos de suporte base seguidos de cinco anos de suporte estendido por versão. As versões LTSC estão disponíveis por meio de todos os mecanismos de lançamento. As versões do LTSC estão disponíveis para qualquer pessoa, independentemente do modelo de licenciamento que esteja usando. 

A tabela a seguir resume as principais diferenças entre os canais:

|  | Canal de Manutenção de Longo Prazo (Windows Server 2019) |Canal semestral (Windows Server) |
| ------------------- | ------------------------------------ | ------------------------------------------------- |
|Cenários recomendados | Servidores de arquivos de finalidade geral, cargas de trabalho da Microsoft ou de terceiros, aplicativos tradicionais, funções de infraestrutura, Datacenter definidas por software e infraestrutura hiperconvergente | Aplicativos em contêiner, hosts do contêiner e cenários de aplicativos que se beneficiam da inovação mais rápida |
| Lançamentos | A cada 2 a 3 anos |Cada 6 meses |
| Suporte |5 anos de suporte base, além de 5 anos de suporte estendido | 18 meses |
| Edições | Todas as edições disponíveis do Windows Server | Edições Standard e Datacenter |
| Quem pode usar | Todos os clientes por meio de todos os canais | somente para clientes do Software Assurance e de nuvem |
| Opções de instalação | Server Core e servidor com Experiência Desktop | Server Core para imagem e host de contêiner e imagem de contêiner do Nano Server |                |


## Compatibilidade de dispositivos
A menos que seja comunicado, os requisitos mínimos de hardware para executar as versões de Canal semestral serão os mesmo da versão mais recente do Canal de Manutenção a Longo Prazo do Windows Server. Por exemplo, **a versão atual do Canal de Manutenção a Longo Prazo é o Windows Server 2019**. A maioria dos drivers de hardware continuam a funcionar nessas versões.

## Manutenção
O versões do Canal de Manutenção a Longo Prazo e do Canal Semestral são compatíveis com atualizações de segurança e atualizações não relacionadas à segurança. A diferença é o período de tempo que a versão é suportada, conforme descrito acima.

### Ferramentas de manutenção
Existem muitas ferramentas com as quais os profissionais de TI podem fazer a manutenção do Windows Server. Cada opção tem suas vantagens e desvantagens, desde funcionalidades e controle à simplicidade e baixos requisitos administrativos. Os exemplos a seguir são de ferramentas de manutenção disponíveis para gerenciar atualizações de manutenção:

- **Windows Update (autônomo)**: essa opção só está disponível para os servidores que estão conectados à Internet e tem a atualização do Windows habilitado.
- O **Windows Server Update Services (WSUS)** fornece controle abrangente sobre as atualizações do Windows 10 e do Windows Server e está nativamente disponível no sistema operacional Windows Server. Além da capacidade de adiar atualizações, as organizações podem adicionar uma camada de aprovação de atualizações e optar por implantá-las em computadores ou grupos de computadores específicos sempre que estiverem prontas.
- O **System Center Configuration Manager** fornece controle máximo sobre a manutenção. Os profissionais de TI podem adiar as atualizações e aprová-las, além de ter várias opções para direcionar as implantações e gerenciar o uso de largura de banda e os tempos de implantação.

Você provavelmente já escolheu usar pelo menos uma destas opções com base em seus recursos, a equipe e a experiência. Você pode continuar usando o mesmo processo para as versões de Canal semestral: por exemplo, se já usar o System Center Configuration Manager para gerenciar atualizações, você pode continuar a usá-lo. Da mesma forma, se estiver usando WSUS, você pode continuar a usá-lo.

## Obter versões prévias por meio do Programa Windows Insider
Testar os builds anteriores do Windows Server ajuda a Microsoft e seus clientes devido à oportunidade de descobrir possíveis problemas antes do lançamento. Ele também oferece aos clientes uma oportunidade única para influenciar diretamente a funcionalidade no produto. 

A Microsoft depende receber comentários durante o processo de desenvolvimento para que os ajustes podem ser feitos mais rápido possível. Os testes iniciais e os comentários são essenciais para o modelo de lançamento rápido.

Para obter mais informações sobre como se envolver no Programa Windows Insider, consulte os [documentos do Programa Windows Insider para servidor](https://docs.microsoft.com/windows-insider/at-work/).

## Para identificar o Windows Server 2019 e o Windows Server, versão 1809

>[!Note]  
> As diretrizes abaixo se destinam a ajudar a identificar e diferenciar entre o LTSC e o SAC apenas para fins de inventário geral somente e ciclo de vida.  Elas não servem para compatibilidade de aplicativos nem para representar uma superfície de API específica.  Os desenvolvedores de aplicativos devem usar as diretrizes para garantir a compatibilidade corretamente já que componentes, APIs e funcionalidades já podem ou não ter sido adicionados durante a vida útil de um sistema. [A versão do sistema operacional](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version) é um ponto de partida melhor para os desenvolvedores de aplicativos.

Abra o Powershell e use o Cmdlet Get-ItemProperty ou Get-ComputerInfo para verificar essas propriedades no Registro.  Juntamente com o número de compilação, isso indicará o LTSC ou o SAC pela presença ou ausência deles, do ano com marca, ou seja, 2019.  O LTSC apresenta essas indicações, mas o SAC não.  Isso também retornará a época da versão com ReleaseId ou WindowsVersion, ou seja, 1809, bem como se a instalação é Server Core ou Server com Experiência Desktop. 

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

# Tópicos relacionados
[Alterações no Nano Server na próxima versão do Canal semestral do Windows Server](../get-started/nano-in-semi-annual-channel.md)

[Ciclo de vida do suporte ao Windows Server](https://support.microsoft.com/lifecycle)

[Determinando se o Server Core está em execução](https://msdn.microsoft.com/library/hh846315%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)

[Função GetProductInfo](https://docs.microsoft.com/windows/desktop/api/sysinfoapi/nf-sysinfoapi-getproductinfo)

[Cmdlets de registro em log de inventário de software](https://docs.microsoft.com/powershell/module/softwareinventorylogging/?view=winserver2012R2-ps)
