---
title: O que há de novo no Windows Server, versão 1903
description: Este tópico descreve alguns dos novos recursos no Windows Server, versão 1903, que é uma versão do canal semestral.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 2aa84df1ca162cb6e2dfa580b4b0744ff08cc95a
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983436"
---
# <a name="whats-new-in-windows-server-version-1903"></a>O que há de novo no Windows Server, versão 1903

>Aplica-se a: Windows Server (canal semestral)

Este tópico descreve alguns dos novos recursos no Windows Server, versão 1903, que é uma versão do canal semestral. Esses recursos incluem melhorias para executar e gerenciar contêineres, ferramentas para trabalhar em instalações Server Core e a capacidade de migrar o armazenamento de dispositivos do Linux.

Para descobrir em vez disso, o que há de novo na última versão do canal de manutenção em longo prazo (LTSC) do Windows Server, consulte ver [o que há de novo no Windows Server 2019](../get-started-19/whats-new-19.md). Consulte também [o que há de novo no Windows 10, o conteúdo para profissionais de TI versão 1903](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903).

Os requisitos de sistema para esta versão são da mesma maneira que o Windows Server 2019 – consulte [requisitos de sistema](../get-started-19/sys-reqs-19.md) para obter mais informações. Para ver o que é removido recentemente, consulte [recursos removidos ou planejado para substituição, começando com o Windows Server, versão 1903](../get-started-19/removed-features-1903.md)

> [!NOTE]
> Contêineres do Windows devem usar a mesma versão do Windows como o servidor host, ou um *anteriores* versão. Por exemplo, um servidor de host executando a versão de lançamento do Windows Server, versão 1903 (build 18342) pode executar contêineres do Windows Server com versão igual ou anterior e número de build (mesmo se o contêiner usa uma versão do Insider Preview do Windows). Para obter mais informações, consulte [compatibilidade de versão de contêiner do Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

## <a name="enhanced-support-for-non-microsoft-container-services"></a>Suporte aprimorado para serviços de contêiner não-Microsoft

Aprimoramos funcionalidades de plataforma para dar suporte a serviços de contêiner do Azure e serviços de contêiner não são da Microsoft.

- Integramos o CRI containerd com o Host de computação Service (HCS) para dar suporte a compartimentos de contêineres do Windows e Linux no Windows (LCOW) no Azure.
- Trabalhamos com a comunidade do Kubernetes para habilitar o suporte de contêiner do Windows. Com o lançamento do v 1.14 do Kubernetes, suporte de nó do Windows Server oficialmente formou-se na versão beta para estável. Para obter mais informações, consulte [contêineres do Windows agora tem suportados no Kubernetes](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/).
- Malhado Tigera para Windows já está disponível como parte da assinatura Tigera Essentials e ofertas misto de diretiva de rede e a rede de não-sobreposição interoperável entre ambientes Linux/Windows.
- Enviamos aprimoramentos de escalabilidade, aprimorando a sobreposição de rede de suporte para contêineres do Windows, incluindo integração com o Kubernetes na versão mais recente do v 1.14 Flannel e Kubernetes. Para obter mais informações, consulte [Introdução ao suporte do Windows no Kubernetes](https://kubernetes.io/docs/setup/windows/).

## <a name="directx-hardware-acceleration-in-containers"></a>Aceleração de hardware do DirectX em contêineres

Habilitamos o suporte para aceleração de hardware de APIs do DirectX em cenários de suporte do Windows contêineres tp como inferência de Machine Learning (ML) usando o hardware GPU (unidade) do local de processamento gráfico. Para obter mais informações, consulte [aceleração de GPU trazendo para contêineres do Windows](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939).

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>Identidade de contêiner atualizada e o grupo gerenciado documentação de conta de serviço

Adicionamos mais exemplos e informações de compatibilidade para o [grupo de contas de serviço gerenciado](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts) documentação e fez a [módulo do PowerShell de especificação de credencial](https://www.powershellgallery.com/packages/CredentialSpec) disponíveis na Galeria do PowerShell. Para obter mais informações, consulte o [o que há de novo para a identidade do contêiner](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151) postagem de blog.

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>Adicionar o Agendador de tarefas e o Gerenciador do Hyper-V em instalações Server Core

Como você deve saber, é recomendável usar a opção de instalação Server Core, ao usar o Windows Server, o canal semestral em produção. No entanto, Server Core por padrão omite uma série de ferramentas de gerenciamento bastante úteis. Você pode adicionar muitos dos mais comumente usados ferramentas instalando o recurso de compatibilidade de aplicativos, mas ainda ter havido algumas ferramentas ausentes.

Portanto, com base nos comentários dos clientes, adicionamos duas ou mais ferramentas para o recurso de compatibilidade de aplicativos nesta versão: O Agendador de tarefas (taskschd) e o Gerenciador do Hyper-V (virtmgmt.msc).

Para obter mais informações, consulte [recurso de compatibilidade de aplicativo do Server Core](../get-started-19/install-fod-19.md).

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Serviço de migração de armazenamento agora migra contas locais, clusters e servidores Linux

Serviço de migração de armazenamento facilita a migração de servidores para uma versão mais recente do Windows Server. Ele fornece uma ferramenta gráfica que faz o inventário de dados em servidores e, em seguida, transfere os dados e a configuração para servidores mais recentes — tudo sem aplicativos ou usuários que têm alterar nada.

Ao usar esta versão do Windows Server para orquestrar as migrações, adicionamos as seguintes capacidades:

- Migrar usuários e grupos locais para o novo servidor
- Migrar o armazenamento de clusters de failover
- Migrar o armazenamento de um servidor Linux que usa o Samba
- Mais facilmente migrados compartilhamentos de sincronização no Azure usando a sincronização de arquivos do Azure
- Migrar para novas redes, como o Azure

Para obter mais informações sobre o serviço de migração de armazenamento, consulte [visão geral do serviço de migração de armazenamento](../storage/storage-migration-service/overview.md).

## <a name="system-insights-disk-anomaly-detection"></a>Detecção de anomalias de disco do sistema Insights

[Insights de sistema](../manage/system-insights/overview.md) é um recurso de análise preditiva que analisa dados de sistema do Windows Server localmente e fornece informações sobre o funcionamento do servidor. Ele vem com um número de recursos internos, mas nós adicionamos a capacidade de instalar recursos adicionais por meio do Windows Admin Center, começando com a detecção de anomalias de disco.

Detecção de anomalias de disco é um novo recurso que destaca quando discos estão se comportando *diferentemente* que o normal. Embora diferentes não necessariamente uma coisa ruim, vendo esses momentos anormais pode ser útil ao solucionar problemas em seus sistemas.

Essa funcionalidade também está disponível para servidores que executam o Windows Server 2019.

## <a name="windows-admin-center-enhancements"></a>Aprimoramentos do Windows Admin Center

Uma nova versão do Windows Admin Center é horizontalmente, adicionando novas funcionalidades para o Windows Server. Para obter informações sobre os recursos mais recentes, consulte [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Linha de base de segurança para Windows 10 e Windows Server

A versão de rascunho do [definições de linha de base de configuração de segurança](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/) para Windows 10 versão 1903 e para o Windows Server versão 1903 está disponível.

## <a name="setupdiag"></a>SetupDiag
[SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) versão 1.4.1 está disponível.

SetupDiag é uma ferramenta de linha de comando que pode ajudar a diagnosticar por que uma atualização do Windows falha. O SetupDiag funciona pesquisando os arquivos de registro da Instalação do Windows. Ao pesquisar os arquivos de registro, o SetupDiag usa um conjunto de regras para corresponder aos problemas conhecidos. Na versão atual do SetupDiag 53 regras contidas no arquivo rules.xml, qual é extraído quando SetupDiag é executado. O arquivo rules.xml será atualizado conforme novas versões do SetupDiag forem disponibilizadas.

## <a name="update-rollback-improvements"></a>Aprimoramentos de reversão de atualização

Servidores agora podem recuperar automaticamente de falhas de inicialização, removendo as atualizações se a falha de inicialização foi introduzida após a instalação das atualizações de driver ou qualidade recentes. Quando um dispositivo não puder ser iniciado corretamente após a instalação recente de qualidade de atualizações de driver, Windows agora desinstalará automaticamente as atualizações para instalar o dispositivo de backup e funcionando normalmente.

Essa funcionalidade requer que o servidor usando a opção de instalação Server Core com um [ambiente de recuperação do Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference) partição.

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Melhorias do Microsoft Defender Advanced Threat ATP (proteção)

O Windows Server inclui o Microsoft Defender Advanced Thread Protection (para obter mais informações, consulte [Windows Defender antivírus no Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)). Esta versão inclui os seguintes aprimoramentos:

- [Redução da área de superfície de ataque](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction) – os administradores de TI podem configurar dispositivos com proteção web avançada que permite definir permitir e negar listas para endereços IP e a URL específica.
- [Proteção de próxima geração](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) – controles foram estendidos para a proteção contra ransomware, uso indevido de credenciais e ataques que são transmitidas por meio de armazenamento removível.
    - Recursos de imposição de integridade – habilitar atestado de tempo de execução remota.
    - À prova de adulteração recursos – a segurança baseada em virtualização usa para isolar recursos de segurança de ATP críticos do sistema operacional e os invasores.
- Tecnologias de proteção de próxima geração da Microsoft Defender ATP:
    - **Avançadas de aprendizado de máquina**: Aprimorado com o aprendizado de máquina avançadas e modelos de IA para habilitá-lo a proteger contra invasores apex usando malware, ferramentas e técnicas de exploração inovadoras de vulnerabilidade.
    - **Proteção contra epidemias de emergência**: Fornece proteção de ataque de emergência que atualizará automaticamente dispositivos nova inteligência quando foi detectada uma epidemia de novo.
    - **Certificação ISO 27001 conformidade**: Garante que o serviço de nuvem analisou as ameaças, vulnerabilidades e impactos e que controles de segurança e gerenciamento de risco estão em vigor.
    - **Suporte de localização geográfica**: Suporte a localização geográfica e a Soberania de dados de exemplo, bem como as políticas de retenção configurável.