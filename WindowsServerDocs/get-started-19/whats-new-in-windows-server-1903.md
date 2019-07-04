---
title: Novidades no Windows Server, versão 1903
description: Este tópico descreve alguns dos novos recursos do Windows Server, versão 1903, que é uma versão do Canal Semestral.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.date: 05/21/2019
ms.openlocfilehash: 0ec6a7ec624818b92fb306089f3dea3c786c0827
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280313"
---
# <a name="whats-new-in-windows-server-version-1903"></a>Novidades no Windows Server, versão 1903

>Aplica-se a: Windows Server (Canal Semestral)

Este tópico descreve alguns dos novos recursos do Windows Server, versão 1903, que é uma versão do Canal Semestral. Esses recursos incluem melhorias para executar e gerenciar contêineres, ferramentas para trabalhar em instalações Server Core e a capacidade de migrar o armazenamento de dispositivos Linux.

Para, em vez disso, descobrir as novidades na última versão do LTSC (Canal de Manutenção em Longo Prazo) do Windows Server, confira [Novidades no Windows Server 2019](../get-started-19/whats-new-19.md). Confira também [Novidades no conteúdo para profissionais de TI do Windows 10, versão 1903](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1903).

Os requisitos de sistema para esta versão são iguais aos do Windows Server 2019 — confira [Requisitos do sistema](../get-started-19/sys-reqs-19.md) para obter mais informações. Para ver o que foi removido recentemente, confira [Recursos removidos ou com substituição planejada começando no Windows Server, versão 1903](../get-started-19/removed-features-1903.md)

> [!NOTE]
> Os contêineres do Windows devem usar a mesma versão do Windows que o servidor host ou uma versão *mais recente*. Por exemplo, um servidor de host executando a versão de lançamento do Windows Server, versão 1903 (build 18342), pode executar contêineres do Windows Server com versão igual ou anterior e número de build (mesmo se o contêiner usar uma versão de prévia do Insider do Windows). Para obter mais informações, confira [Compatibilidade da versão do contêiner do Windows](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

## <a name="enhanced-support-for-non-microsoft-container-services"></a>Suporte aprimorado para serviços de contêiner não Microsoft

Aprimoramos funcionalidades de plataforma para a compatibilidade de serviços de contêiner do Azure e serviços de contêiner que não são da Microsoft.

- Integramos o contêiner CRI com o HCS (Serviço de Computação de Host) para compatibilidade com pods de contêineres do Windows e LCOW (contêineres do Linux no Windows) no Azure.
- Trabalhamos com a comunidade do Kubernetes para possibilitar a compatibilidade com o contêiner do Windows. Com o lançamento do Kubernetes v1.14, a compatibilidade do nó do Windows Server oficialmente foi promovido de beta para estável. Para obter mais informações, confira [Contêineres do Windows agora compatíveis com o Kubernetes](https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/).
- Tigera Calico para Windows agora está em disponibilidade geral como parte da assinatura do Tigera Essentials e oferece política de rede e rede sem sobreposição interoperáveis entre ambientes Linux/Windows mistos.
- Fornecemos aprimoramentos de escalabilidade, melhorando a compatibilidade de rede de sobreposição para contêineres do Windows, incluindo a integração com o Kubernetes por meio da versão mais recente do Flannel e Kubernetes v1.14. Para obter mais informações, confira [Introdução à compatibilidade do Windows no Kubernetes](https://kubernetes.io/docs/setup/windows/).

## <a name="directx-hardware-acceleration-in-containers"></a>Aceleração de hardware do DirectX em contêineres

Habilitamos a compatibilidade da aceleração de hardware de APIs do DirectX em cenários compatibilidade de tp de contêineres do Windows, como a inferência ML (Machine Learning) usando o hardware de GPU (unidade de processamento gráfico) local. Para obter mais informações, confira [Trazendo a aceleração de GPU para contêineres do Windows](https://techcommunity.microsoft.com/t5/Containers/Bringing-GPU-acceleration-to-Windows-containers/ba-p/393939).

## <a name="updated-container-identity-and-group-managed-service-account-documentation"></a>Documentação da conta de serviço gerenciado do grupo e identidade do contêiner atualizada

Adicionamos mais exemplos e informações de compatibilidade à documentação de [Contas de serviço gerenciado de grupo](https://docs.microsoft.com/virtualization/windowscontainers/manage-containers/manage-serviceaccounts) e disponibilizamos o [Módulo do PowerShell de especificação de credencial](https://www.powershellgallery.com/packages/CredentialSpec) na Galeria do PowerShell. Para obter mais informações, confira a postagem de blog [Novidades para a identidade de contêineres](https://techcommunity.microsoft.com/t5/Containers/What-s-new-for-container-identity/ba-p/389151).

## <a name="add-task-scheduler-and-hyper-v-manager-to-server-core-installations"></a>Adicionar o Agendador de Tarefas e o Gerenciador de Hyper-V em instalações do Server Core

Como você deve saber, é recomendável usar a opção de instalação Server Core ao usar o Windows Server, Canal Semestral, na produção. No entanto, o Server Core por padrão omite uma série de ferramentas de gerenciamento úteis. Você pode adicionar muitas das ferramentas normalmente mais usadas instalando o recurso de compatibilidade de aplicativos, mas há algumas ferramentas ausentes.

Portanto, com base nos comentários dos clientes, adicionamos mais duas ferramentas ao recurso de compatibilidade de aplicativos nesta versão: Agendador de Tarefas (taskschd.msc) e Gerenciador do Hyper-V (virtmgmt.msc).

Para obter mais informações, confira [Recurso de compatibilidade de aplicativos do Server Core](../get-started-19/install-fod-19.md).

## <a name="storage-migration-service-now-migrates-local-accounts-clusters-and-linux-servers"></a>Agora, o serviço de migração de armazenamento migra contas locais, clusters e servidores Linux

O Serviço de Migração de Armazenamento facilita a migração dos servidores para uma versão mais recente do Windows Server. Ele fornece uma ferramenta gráfica que faz o inventário de dados nos servidores e, em seguida, transfere os dados e a configuração para servidores mais recentes — tudo sem aplicativos ou usuários precisando mudar tudo.

Ao usar essa versão do Windows Server para orquestrar as migrações, adicionamos as seguintes capacidades:

- Migrar usuários e grupos locais para o novo servidor
- Migrar o armazenamento dos clusters de failover
- Migrar o armazenamento de um servidor Linux que usa o Samba
- Sincronização mais fácil de compartilhamentos integrados no Azure usando a Sincronização de Arquivos do Azure
- Migrar para novas redes, como o Azure

Para obter mais informações sobre o serviço de migração de armazenamento, confira [Visão geral do Serviço de Migração de Armazenamento](../storage/storage-migration-service/overview.md).

## <a name="system-insights-disk-anomaly-detection"></a>Detecção de anomalias de disco de insights do sistema

Os [insights do sistema](../manage/system-insights/overview.md) são um recurso de análise preditiva que analisa localmente os dados do sistema do Windows Server e fornece insights sobre o funcionamento do servidor. Eles vêm com vários recursos internos, mas adicionamos a capacidade de instalar funcionalidades por meio do Windows Admin Center, começando com a detecção de anomalias de disco.

A detecção de anomalias de disco é uma nova funcionalidade que destaca quando discos estão se comportando *de forma diferente* da normal. Embora diferente não necessariamente uma seja uma coisa ruim, ver esses momentos anormais pode ser útil ao solucionar problemas em seus sistemas.

Essa funcionalidade também está disponível para servidores que executam o Windows Server 2019.

## <a name="windows-admin-center-enhancements"></a>Aprimoramentos do Windows Admin Center

Uma nova versão do Windows Admin Center foi lançada, adicionando novas funcionalidades ao Windows Server. Para obter informações sobre os recursos mais recentes, confira [Windows Admin Center](../manage/windows-admin-center/understand/windows-admin-center.md).

## <a name="security-baseline-for-windows-10-and-windows-server"></a>Linha de base de segurança para Windows 10 e Windows Server

A versão de rascunho das [definições de linha de base de configuração de segurança](https://blogs.technet.microsoft.com/secguide/2019/04/24/security-baseline-draft-for-windows-10-v1903-and-windows-server-v1903/) para Windows 10 versão 1903 e para o Windows Server versão 1903 está disponível.

## <a name="setupdiag"></a>SetupDiag
O [SetupDiag](https://docs.microsoft.com/windows/deployment/upgrade/setupdiag) versão 1.4.1 está disponível.

O SetupDiag é uma nova ferramenta de linha de comando que pode ajudar a diagnosticar o motivo da falha de uma atualização do Windows. O SetupDiag funciona pesquisando os arquivos de log da Instalação do Windows. Ao pesquisar os arquivos de log, o SetupDiag usa um conjunto de regras para corresponder aos problemas conhecidos. Existem 53 regras contidas na versão atual do SetupDiag no arquivo rules.xml, que é extraído quando SetupDiag é executado. O arquivo rules.xml será atualizado conforme novas versões do SetupDiag forem disponibilizadas.

## <a name="update-rollback-improvements"></a>Aprimoramentos de reversão de atualização

Agora, os servidores podem se recuperar automaticamente de falhas de inicialização removendo as atualizações se a falha de inicialização foi introduzida após a instalação das atualizações de driver ou qualidade recentes. Quando um dispositivo não puder ser iniciado corretamente após a instalação recente de qualidade de atualizações de driver, o Windows agora desinstalará automaticamente as atualizações para instalar o dispositivo de backup e funcionando normalmente.

Essa funcionalidade requer que o servidor usando a opção de instalação Server Core com uma partição do [Ambiente de Recuperação do Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

## <a name="microsoft-defender-advanced-threat-protection-atp-improvements"></a>Aprimoramentos de ATP (Proteção Avançada contra Ameaças) do Microsoft Defender

O Windows Server inclui a Proteção Avançada Contra Ameaças do Microsoft Defender Advanced (para obter mais informações, confira [Windows Defender Antivírus no Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-on-windows-server-2016)). Esta versão inclui os seguintes aprimoramentos:

- [Redução da área de superfície de ataque](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview-attack-surface-reduction) – os administradores de TI podem configurar dispositivos com proteção da Web avançada que permite que eles definam listas de permissão e negação para endereços IP e URLs específicas.
- [Proteção de próxima geração](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10) – os controles foram estendidos para a proteção contra ransomware, uso indevido de credenciais e ataques que são transmitidos por meio de armazenamento removível.
    - Funcionalidades de imposição de integridade – permite o atestado de tempo de execução remota.
    - Funcionalidades à prova de adulteração – usa a segurança baseada em virtualização para isolar funcionalidades de segurança de ATP críticas do sistema operacional e de invasores.
- Tecnologias de proteção de próxima geração da ATP do Microsoft Defender:
    - **Aprendizado de máquina avançado**: Aprimorado com o aprendizado de máquina avançado e modelos de IA que permitem que ele proteja contra invasores apex usando técnicas, ferramentas e malware de exploração de vulnerabilidades avançados.
    - **Proteção contra ataques de emergência**: Fornece proteção contra ataques de emergência que atualizará automaticamente dispositivos nova inteligência quando um novo ataque for detectado.
    - **Conformidade com a certificação ISO 27001**: Garante que o serviço de nuvem tenha realizado a análise quanto a ameaças, vulnerabilidades e impactos e que os controles de segurança e gerenciamento de riscos estão em vigor.
    - **Compatibilidade com geolocalização**: Compatibilidade com a geolocalização e a soberania de dados de exemplo, bem como as políticas de retenção configuráveis.