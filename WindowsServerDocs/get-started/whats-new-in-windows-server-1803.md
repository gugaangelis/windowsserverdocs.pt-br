---
title: Novidades no Windows Server, versão 1803
description: Quais são os novos recursos de computação, identidade, gerenciamento, automação, rede, segurança, armazenamento.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: greg-lindsay
ms.author: greg-lindsay
ms.localizationpriority: high
ms.date: 05/07/2018
ms.openlocfilehash: a489d3f8958304d685116186f5db9e1c854114bf
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "65976536"
---
# <a name="whats-new-in-windows-server-version-1803"></a>Novidades no Windows Server, versão 1803

>Aplica-se a: Windows Server (Canal semestral)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;Para saber mais sobre os recursos mais recentes do Windows, consulte [Novidades no Windows Server](whats-new-in-windows-server.md). O conteúdo desta seção descreve as novidades e as alterações no Windows Server, versão 1803. Os novos recursos e alterações listados aqui são os que têm maior probabilidade de ter um impacto maior ao trabalhar com esta versão. Consulte também a [atualização do Canal semestral do Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/).

## <a name="windows-admin-center"></a>Windows Admin Center

O Project Honolulu agora é o **Windows Admin Center**.
<br>&nbsp;
> [!video https://www.youtube.com/embed/WCWxAp27ERk?autoplay=false]

O [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) consolida todos os aspectos do gerenciamento de servidores locais e remotos. O Windows Admin Center é uma experiência de gerenciamento com base em navegador e implantada localmente que não exige uma conexão com a Internet, proporcionando controle completo de todos os aspectos da implantação do Windows Server.

## <a name="windows-server-release-strategy"></a>Estratégia de lançamento do Windows Server

O Windows Server versão 1709 foi lançado em setembro de 2017 como a primeira versão no Canal semestral. O Canal semestral tem um ritmo de lançamento mais rápido e considera os comentários daqueles que desejam inovação rápida em intervalos de poucos meses. Isso complementa o canal de manutenção em longo prazo, em que o ritmo de lançamento é de a cada dois ou três anos.

Com base nos comentários e na telemetria, esses canais demonstraram que estão em boa conformidade com a seguinte estratégia geral:
- O canal semestral é ideal para aplicativos modernos e cenários de inovação, tais como contêineres e microsserviços.
- O canal de manutenção em longo prazo é a versão preferencial para cenários de infraestrutura de núcleo, tais como datacenter definido por software e HCI (infraestrutura hiperconvergente). 

Os cenários específicos para o canal semestral e o canal de manutenção em longo prazo são:

|   | Canal de Manutenção em Longo Prazo |  Canal Semestral |
| ------------- | ------------- | ------------ |
| Cenários recomendados     | Servidores de arquivos de uso geral, cargas de trabalho primárias e de terceiros, aplicativos tradicionais, funções de infraestrutura, datacenter definido por software e infraestrutura hiperconvergente  | Aplicativos em contêineres, hosts do contêiner e cenários de aplicativos se beneficiando de inovação mais rápida |
| Lançamentos  | A cada 2 a 3 anos  | A cada 6 meses |
| Suporte  | Suporte de 5 anos + 5 anos de suporte estendido  | 18 meses |
| Edições  | Todas as edições disponíveis do Windows Server  | Edições Standard e Datacenter |
| Quem pode usar  | Todos os clientes por meio de todos os canais | Somente para clientes do Software Assurance e de nuvem |
| Opções de instalação  | Server Core e Server com Experiência Desktop  | Server Core para o host do contêiner, imagem de contêiner e imagem de contêiner do Nano Server |

## <a name="application-platform-and-containers"></a>Plataforma de aplicativos e contêineres

- Otimização
    - A imagem de contêiner base do Server Core é reduzida em 30% em relação ao Windows Server, versão 1709. 
    - A compatibilidade do aplicativo também é aprimorada para ajudar com a colocação de aplicativos tradicionais em contêineres.
    - O desempenho de inicialização do contêiner e o desempenho de tempo de execução foram aprimorados também devido a várias correções e otimizações.
- Rede de contêineres: O suporte a proxy localhost e http foi adicionado, além de melhorar a escalabilidade e o tempo de inicialização dos contêineres.
- Ferramentas: O suporte a Curl.exe, Tar.exe e SSH foi aprimorado a fim de complementar o PowerShell para cenários de compilação e depuração.

### <a name="server-core-container-image"></a>Imagem de contêiner do Server Core

Um contêiner do Server Core menor com melhor compatibilidade do aplicativo agora está disponível. Informações detalhadas estão disponíveis [aqui](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/).

- Funções e recursos opcionais não utilizados foram removidos. Para obter mais informações, veja [Funções, serviços de função e recursos não pertencentes a contêineres do Server Core](../administration/server-core/server-core-container-removed-roles.md).
    - Tamanho de download reduzido para 1,58 GB, com redução de 30% em relação ao Windows Server, versão 1709.
    - Tamanho em disco reduzido para 3,61 GB, com 20% de redução em relação ao Windows Server, versão 1709.
- A imagem de contêiner do Nano Server é inferior a 100 MB

### <a name="windows-subsystem-for-linux-wsl"></a>WSL (Subsistema do Windows para Linux)

O WSL permite que os administradores do servidor usem as ferramentas e scripts existentes do Linux no Windows Server. Vários aprimoramentos apresentados no [blog de linha de comando](https://blogs.msdn.microsoft.com/commandline/tag/wsl/) agora fazem parte do Windows Server, incluindo tarefas em segundo plano, DriveFS, WSLPath e muito mais.

### <a name="kubernetes"></a>Kubernetes 

O Kubernetes (geralmente conhecido como K8s) é um sistema de software livre para automatização da implantação, do dimensionamento e do gerenciamento de aplicativos em contêineres, desenvolvido sob a administração da [Cloud Native Computing Foundation](https://www.cncf.io). 

No Windows Server versão 1709, os usuários puderam aproveitar o Kubernetes em recursos de rede do Windows, incluindo:
- Compartimentos de pod compartilhados: Pods de infraestrutura e de trabalho agora compartilham um compartimento de rede (semelhante a um namespace do Linux).
- Otimização de ponto de extremidade: Devido ao compartilhamento do compartimento, os serviços de contêiner precisam controlar apenas metade dos pontos de extremidade.
- Otimização do caminho de dados: Melhorias na Plataforma de Filtragem Virtual e no Serviço de Rede Host permitem balanceamento de carga baseado em kernel.

Com o lançamento do Windows Server, versão 1803, mais recursos estarão disponíveis na versões futuras de Kubernetes: 
- [Plug-ins de armazenamento](https://github.com/Microsoft/K8s-Storage-Plugins) para contêineres do Windows coordenados por Kubernetes.
- Redes em escala de nuvem por meio de iniciativas como nossa parceria com suporte da [Tigera no projeto Calico](https://cloudblogs.microsoft.com/windowsserver/2017/12/07/securing-modernized-apps-and-simplified-networking-on-windows-with-calico/).
- Suporte de plataforma do Windows para Pods isolados do Hyper-V com vários contêineres por Pod.

### <a name="application-compatibility-and-feature-parity-issues-fixed"></a>Problemas de paridade de recursos e de compatibilidade do aplicativo corrigidos

- O MSMQ (Enfileiramento de Mensagens da Microsoft) agora é instalado em um contêiner do Server Core.
- Um problema que interrompe os contadores de desempenho do ASP.NET foi corrigido.
- Um problema em que serviços em execução nos contêineres não recebiam a notificação de desligamento foi corrigido.
    - Especificamente, a notificação é alterada para CTRL_SHUTDOWN_EVENT para imagens com base em contêiner do Server Core e do Nano Server. Além disso, ele estende a notificação nas imagens com base em contêiner do Server Core para afetar todos os processos em execução no contêiner, incluindo o envio de notificações de desligamento do serviço para serviços em execução no contêiner.
- Uma incompatibilidade de docker pull e carga do docker com a configuração de política que determina se a proteção BitLocker é necessária para que as unidades de dados fixas sejam graváveis (FDVDenyWriteAccess) foi corrigida. 

## <a name="storage"></a>Armazenamento

Com essa versão, é possível impedir que o serviço do Gerenciador de Recursos de Servidor de Arquivos crie um diário de alteração (também conhecido como um diário USN) em todos os volumes quando o serviço é iniciado. Isso pode economizar espaço em cada volume, mas desabilita a classificação de arquivos em tempo real. Para obter mais informações, consulte [Visão geral do Gerenciador de Recursos de Servidor de Arquivos](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview).

## <a name="features-added-to-server-core"></a>Recursos adicionados ao Server Core

A função de servidor de transporte na função dos WDS (Serviços de Implantação do Windows) foi adicionada ao Server Core.

O Servidor de transporte contém somente as partes de rede principais do WDS. Você pode usar o servidor de transporte para criar namespaces multicast que transmitem dados (inclusive imagens do sistema operacional) de um servidor autônomo. Também será possível usá-lo se você desejar ter um servidor PXE que permita aos clientes inicializar o PXE e baixar seu próprio aplicativo de instalação personalizado. Você deverá usar essa opção se desejar usar um desses dois cenários.

Você pode usar o comando do Windows PowerShell a seguir para habilitar o serviço do Servidor de transporte no Server Core:

```
Install-WindowsFeature -Name WDS
```

## <a name="see-also"></a>Consulte também

[Informações sobre versões do Windows Server](https://docs.microsoft.com/windows-server/get-started/windows-server-release-info)<br>
[Novidades no conteúdo para profissionais de TI do Windows 10, versão 1803](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1803)
