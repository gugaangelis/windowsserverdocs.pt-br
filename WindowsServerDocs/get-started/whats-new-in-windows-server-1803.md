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
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976536"
---
# <a name="whats-new-in-windows-server-version-1803"></a>Novidades no Windows Server versão 1803

>Aplica-se a: Windows Server (canal semestral)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;Para saber mais sobre os recursos mais recentes do Windows, consulte [o que há de novo no Windows Server](whats-new-in-windows-server.md). O conteúdo desta seção descreve as novidades e mudanças no Windows Server, versão 1803. Os novos recursos e alterações listados aqui são os que têm maior probabilidade de ter um impacto maior ao trabalhar com esta versão. Consulte também a [atualização do Canal semestral do Windows Server](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update/).

## <a name="windows-admin-center"></a>Windows Admin Center

O Project Honolulu agora é o **Windows Admin Center**.
<br>&nbsp;
> [!video https://www.youtube.com/embed/WCWxAp27ERk?autoplay=false]

O [Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/overview) consolida todos os aspectos do gerenciamento de servidores locais e remotos. O Windows Admin Center é uma experiência de gerenciamento com base em navegador e implantado localmente, que não exige uma conexão de Internet, proporcionando controle completo de todos os aspectos da implantação do Windows Server.

## <a name="windows-server-release-strategy"></a>Estratégia de lançamento do Windows Server

O Windows Server versão 1709 foi lançado em setembro de 2017 como a primeira versão no Canal semestral. O Canal semestral tem um ritmo de lançamento mais rápido e considera os comentários daqueles que desejam inovação rápida em intervalos de poucos meses. Isso complementa o canal de serviço do Long-Term onde o seu ritmo é a cada 2-3 anos.

Com base nos comentários e telemetria, esses canais demonstraram que eles estão de acordo com bom desempenho a seguinte estratégia geral:
- O canal semestral é ideal para aplicativos modernos e cenários de inovação, como contêineres e microsserviços.
- O canal de serviço de longo prazo é a versão preferencial para cenários de infraestrutura de núcleo, como datacenter definido por software e infraestrutura hiperconvergente (HCI). 

Os cenários específicos para o canal semestral e o canal de serviço de longo prazo são:

|   | Canal de Manutenção a Longo Prazo |  Canal Semestral |
| ------------- | ------------- | ------------ |
| Cenários recomendados     | Servidores de arquivos de finalidade geral, cargas de trabalho primárias e de terceiros, aplicativos tradicionais, funções de infraestrutura, Datacenter definido por software e infraestrutura hiperconvergente  | Aplicativos em recipientes, hosts do contêiner e cenários de aplicativos se beneficiando da inovação mais rápida |
| Lançamentos  | A cada 2 a 3 anos  | Cada 6 meses |
| Suporte  | Suporte de 5 anos + 5 anos de suporte estendido  | 18 meses |
| Edições  | Todas as edições disponíveis do Windows Server  | Edições Standard e Datacenter |
| Quem pode usar  | Todos os clientes por meio de todos os canais | somente para clientes do Software Assurance e de nuvem |
| Opções de instalação  | Server Core e Server com Experiência Desktop  | Server Core para o host do contêiner, imagem de contêiner e imagem de contêiner do Nano Server |

## <a name="application-platform-and-containers"></a>Plataforma de aplicativo e contêineres

- Otimização
    - A imagem de contêiner base do Server Core é reduzida em 30% do Windows Server, versão 1709. 
    - A compatibilidade de aplicativos também é aprimorada para ajudar você com os contêineres de aplicativos tradicionais.
    - O desempenho de inicialização do contêiner e o desempenho de tempo de execução foram aprimorados também devido a várias correções e otimizações.
- Rede de contêiner: Foi adicionado suporte de proxy do localhost e http e tempo de inicialização e a escalabilidade do contêiner é aprimorado.
- Ferramentas: Suporte para Curl.exe, Tar.exe e SSH foi aprimorado para complementar o PowerShell para compilar e cenários de depuração.

### <a name="server-core-container-image"></a>Imagem de contêiner do Server Core

Um contêiner do Server Core menor com melhor compatibilidade de aplicativo agora está disponível. Veja informações detalhadas [aqui](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/).

- Funções e recursos opcionais não utilizados foram removidas. Para obter mais informações, veja [Funções, serviços de função e recursos não pertencentes aos contêineres do Server Core](../administration/server-core/server-core-container-removed-roles.md).
    - Tamanho de download reduzido para 1,58 GB, com redução de 30% do Windows Server, versão 1709.
    - Tamanho em disco reduzido para 3,61 GB, com 20% de redução do Windows Server, versão 1709.
- Imagem de contêiner do Nano Server inferior a 100 MB

### <a name="windows-subsystem-for-linux-wsl"></a>Subsistema do Windows para Linux (WSL)

O WSL permite que os administradores do servidor usem as ferramentas existentes e scripts do Linux no Windows Server. Vários aprimoramentos apresentados no [blog de linha de comando](https://blogs.msdn.microsoft.com/commandline/tag/wsl/) agora fazem parte do Windows Server, incluindo tarefas em segundo plano, DriveFS, WSLPath e muito mais.

### <a name="kubernetes"></a>Kubernetes 

Kubernetes (geralmente conhecido como K8s) é um sistema de fonte aberta para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos em contêineres, desenvolvido sob a administração da [Cloud Native Computing Foundation](https://www.cncf.io). 

No Windows Server, os usuários da versão 1709 puderam aproveitar Kubernetes em recursos de rede do Windows, incluindo:
- Compartilhado compartimentos de pod: Os pods de infraestrutura e de trabalho agora compartilham um compartimento de rede (análogo a um namespace do Linux).
- Otimização de ponto de extremidade: Graças ao compartilhamento de compartimento, serviços de contêiner precisam controlar quantos pontos de extremidade pelo menos metade.
- Otimização de caminho de dados: Melhorias para a plataforma de filtragem Virtual e o serviço de rede do Host permitem baseada em kernel balanceamento de carga.

Com o lançamento do Windows Server, versão 1803, mais recursos estarão disponíveis na versões futuras de Kubernetes: 
- [Plug-ins de armazenamento](https://github.com/Microsoft/K8s-Storage-Plugins) para contêineres Windows coordenados por Kubernetes.
- Redes em escala de nuvem por meio de iniciativas como nossa parceria com suporte da [Tigera no projeto Calico](https://cloudblogs.microsoft.com/windowsserver/2017/12/07/securing-modernized-apps-and-simplified-networking-on-windows-with-calico/).
- Suporte de plataforma do Windows para Pods isolados do Hyper-V com vários contêineres por Pod.

### <a name="application-compatibility-and-feature-parity-issues-fixed"></a>Problemas de compatibilidade e recursos de aplicativo corrigidos

- O Enfileiramento de Mensagens da Microsoft (MSMQ) agora é instalado em um contêiner do Server Core.
- Um problema que interrompe os contadores de desempenho do ASP.net foi corrigido.
- Um problema em que serviços em execução nos contêineres não recebem a notificação de desligamento foi corrigido.
    - Especificamente, a notificação é alterada para CTRL_SHUTDOWN_EVENT para imagens com base em contêiner do Server Core e Nano Server. Além disso, ele estende a notificação nas imagens com base em contêiner do Server Core para afetar todos os processos em execução, incluindo o envio de notificações de desligamento do serviço para serviços em execução no contêiner.
- Uma incompatibilidade de docker pull e carga do docker com a configuração de política que determina se a proteção BitLocker é necessária para que as unidades de dados fixas sejam graváveis (FDVDenyWriteAccess) foi corrigida. 

## <a name="storage"></a>Armazenamento

Com essa versão, é possível impedir que o serviço do Gerenciador de Recursos de Servidor de Arquivos crie um diário de alteração (também conhecido como um diário USN) em todos os volumes quando o serviço é iniciado. Isso pode economizar espaço em cada volume, mas desabilitará a classificação de arquivos em tempo real. Para obter mais informações, consulte a [visão geral do Gerenciador de Recursos de Servidor de Arquivos](https://docs.microsoft.com/windows-server/storage/fsrm/fsrm-overview).

## <a name="features-added-to-server-core"></a>Recursos adicionados ao Server Core

A função de servidor de transporte na função dos Serviços de Implantação do Windows (WDS) foi adicionada ao Server Core.

O Servidor de transporte contém somente as principais partes de rede do WDS. Você pode usar o servidor de transporte para criar namespaces multicast que transmitem dados (inclusive imagens do sistema operacional) de um servidor autônomo. Também é possível usá-lo se você deseja um servidor PXE que permite aos clientes inicializar o PXE e baixar seu próprio aplicativo de instalação personalizada. Você deve usar essa opção se desejar usar um destes cenários.

Você pode usar o seguinte comando do Windows PowerShell para habilitar o serviço do Servidor de transporte no Server Core:

```
Install-WindowsFeature -Name WDS
```

## <a name="see-also"></a>Consulte também

[Informações sobre versões do Windows Server](https://docs.microsoft.com/windows-server/get-started/windows-server-release-info)<br>
[O que há de novo no Windows 10, o conteúdo para profissionais de TI da versão 1803](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1803)
