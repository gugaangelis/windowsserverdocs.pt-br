---
title: Solução de problemas do Windows Server
description: Obtenha links para artigos de solução de problemas do Windows Server
ms.date: 1/24/2020
author: kaushika-msft
ms.author: kaushika
ms.openlocfilehash: fbebb44d687bfeb5660eab3b81c164f8cdf6196d
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996724"
---
# <a name="troubleshooting-windows-server-components"></a>Solucionando problemas de componentes do Windows Server

> [!TIP]
> Está procurando informações sobre versões anteriores do Windows Server? Confira as outras [bibliotecas do Windows Server](/previous-versions/windows/) em docs.microsoft.com.
>
> Você também pode [pesquisar informações específicas neste site](/search/index?dataSource=previousVersions&search=Windows+Server).

A Microsoft lança regularmente as duas atualizações para o Windows Server. Para garantir que os servidores podem receber atualizações futuras, incluindo atualizações de segurança, é importante mantê-los atualizados. Confira [Histórico de atualizações do Windows 10 e Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history) para obter uma lista completa das atualizações lançadas.

Esta seção contém tópicos e links de solução de problemas avançados para ajudá-lo a resolver problemas com o Windows Server. Tópicos adicionais serão adicionados à medida que forem disponibilizados.

## <a name="troubleshoot-activation"></a>Solucionar problemas de ativação

- [Solução de problemas de ativação de volume do Windows](../get-started/activation-troubleshooting-guide.md)
- [Diretrizes para solução de problemas do KMS](../get-started/activation-troubleshoot-kms-general.md)
- [Opções Slmgr.vbs para obter informações de ativação de volume](../get-started/activation-slmgr-vbs-options.md)
- [Resolver códigos de erro de ativação do Windows](../get-started/activation-error-codes.md)
- [Problemas conhecidos da ativação do KMS](../get-started/activation-troubleshoot-kms-issues.md)
- [Problemas conhecidos de ativação da MAK](../get-started/activation-troubleshoot-mak-issues.md)
- [Diretrizes para solução de problemas de ativação relacionados ao DNS](../get-started/common-troubleshooting-procedures-kms-dns.md)
- [Recompilar o arquivo Tokens.dat](../get-started/activation-rebuild-tokens-dat-file.md)
- [Solucionar problemas de clientes ADBA](../get-started/activation-troubleshoot-adba-clients.md)

## <a name="troubleshoot-startup-and-restart"></a>Solucionar problemas de inicialização e reinicialização

- [Solução de problemas avançada de inicialização do Windows](/windows/client-management/troubleshoot-windows-startup)
- [Como determinar o tamanho do arquivo de paginação apropriado para versões de 64 bits do Windows](/windows/client-management/determine-appropriate-page-file-size)
- [Gerar um kernel ou um despejo de memória completo](/windows/client-management/generate-kernel-or-complete-crash-dump)
- [Introdução ao arquivo de paginação](/windows/client-management/introduction-page-file)
- [Configurar opções de falha e recuperação do sistema no Windows](/windows/client-management/system-failure-recovery-options)
- [Solução de problemas avançada para erros de inicialização do Windows](/windows/client-management/advanced-troubleshooting-boot-problems)
- [Solução de problemas avançada de congelamento de computador baseado no Windows](/windows/client-management/troubleshoot-windows-freeze)
- [Solução de problemas avançada para erro de tela azul ou erro de parada](/windows/client-management/troubleshoot-stop-errors)
- [Solução de problemas avançada para erro de parada 7B ou Inaccessible_Boot_Device](/windows/client-management/troubleshoot-inaccessible-boot-device)
- [Solução de problemas avançada para a ID de evento 41 "o sistema foi reinicializado sem desligar o primeiro"](/windows/client-management/troubleshoot-event-id-41-restart)
- [O erro de parada ocorre quando você atualiza o driver de adaptador de rede Broadcom in-box](/windows/client-management/troubleshoot-stop-error-on-broadcom-driver-update)

## <a name="troubleshoot-ad-forest-recovery"></a>Solucionar problemas de recuperação de floresta do AD

- [Recuperação de floresta do AD – Perguntas frequentes](../identity/ad-ds/manage/ad-forest-recovery-faq.md)

## <a name="troubleshoot-ad-replication"></a>Solucionar problemas de replicação do AD

- [Como solucionar problemas de replicação do Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Solução de problemas do controlador de domínio virtualizado](../identity/ad-ds/manage/virtual-dc/virtualized-domain-controller-troubleshooting.md)
- [Solução de problemas de implantação de controlador de domínio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Configurando um computador para solução de problemas](../identity/ad-ds/manage/troubleshoot/configuring-a-computer-for-troubleshooting.md)

## <a name="troubleshoot-ad-fs"></a>Solucionar problemas AD FS

- [Solução de problemas do AD FS](../identity/ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
- [AD FS solução de problemas-eventos e log de auditoria](../identity/ad-fs/troubleshooting/ad-fs-tshoot-logging.md)
- [Solução de problemas AD FS-conectividade do SQL](../identity/ad-fs/troubleshooting/ad-fs-tshoot-sql.md)
- [Solução de problemas de AD FS-emissão de declarações](../identity/ad-fs/troubleshooting/ad-fs-tshoot-claims-issuance.md)
- [Solução de problemas AD FS detecção de loop](../identity/ad-fs/troubleshooting/ad-fs-tshoot-loop.md)
- [Solução de problemas AD FS-certificados](../identity/ad-fs/troubleshooting/ad-fs-tshoot-certs.md)
- [Solução de problemas AD FS-Fiddler](../identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler.md)
- [Solução de problemas de AD FS-Fiddler-WS-Federation](../identity/ad-fs/troubleshooting/ad-fs-tshoot-fiddler-ws-fed.md)
- [AD FS solução de problemas – regras de declarações](../identity/ad-fs/troubleshooting/ad-fs-tshoot-claims-rules.md)
- [Solução de problemas AD FS-autenticação integrada do Windows](../identity/ad-fs/troubleshooting/ad-fs-tshoot-iwa.md)
- [Solução de problemas AD FS-Azure AD](../identity/ad-fs/troubleshooting/ad-fs-tshoot-azure.md)
- [Perguntas frequentes sobre o AD FS](../identity/ad-fs/overview/ad-fs-faq.md)
- [Analisador de diagnóstico de ajuda do AD FS](../identity/ad-fs/troubleshooting/ad-fs-diagnostics-analyzer.md)

## <a name="troubleshoot-aovpn"></a>Solucionar problemas de AoVPN

- [Solucionar problemas da VPN Always On](../remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy-troubleshooting.md)

## <a name="troubleshoot-converged-nic"></a>Solucionar problemas de NIC convergida

- [Solucionando problemas de configurações de NIC convergida](../networking/technologies/conv-nic/cnic-app-troubleshoot.md)

## <a name="troubleshoot-dfsr"></a>Solucionar problemas de DFSR

- [Replicação do DFS: perguntas frequentes (FAQ)](../storage/dfs-replication/dfsr-faq.md)

## <a name="troubleshoot-directaccess"></a>Solucionar problemas do DirectAccess

- [Como solucionar problemas do DirectAccess](../remote/remote-access/directaccess/troubleshooting-directaccess.md)

## <a name="troubleshoot-disk-management"></a>Solucionar problemas do gerenciamento de disco

- [Solução de problemas do Gerenciamento de disco](../storage/disk-management/troubleshooting-disk-management.md)

## <a name="troubleshoot-dns"></a>Solucionar problemas de DNS

- [Solucionando problemas de DNS (sistema de nomes de domínio)](../networking/dns/troubleshoot/troubleshoot-dns-data-collection.md)
- [Solução de problemas de clientes DNS](../networking/dns/troubleshoot/troubleshoot-dns-client.md)
- [Desabilitar o cache do DNS do lado do cliente em clientes DNS](../networking/dns/troubleshoot/disable-dns-client-side-caching.md)
- [Solucionando problemas de servidores DNS](../networking/dns/troubleshoot/troubleshoot-dns-server.md)

## <a name="troubleshoot-failover-cluster"></a>Solucionar problemas de cluster de failover

- [Solução de problemas de um Cluster de Failover usando o Relatório de Erros do Windows](../failover-clustering/troubleshooting-using-wer-reports.md)
- [Atualização com suporte a cluster-perguntas frequentes](../failover-clustering/cluster-aware-updating-faq.md)
- [Como solucionar o problema de cluster com a ID de Evento 1135](./troubleshooting-cluster-event-id-1135.md)
- [Tendo um problema com os nós sendo removidos da Associação de cluster de failover ativo](./problem-nodes-failover-cluster.md)
- [Nós sendo removidos da Associação de cluster de failover no VMWare ESX](./nodes-failover-cluster-vmware.md)
- [IaaS com SQL AlwaysOn - Ajustando Limites de Rede de Cluster de Failover](./iaas-sql-failover-cluster.md)

## <a name="troubleshoot-dhcp"></a>Solucionar problemas de DHCP

- [Guia de solução de problemas do DHCP (protocolo de configuração dinâmica de hosts)](./troubleshoot-dhcp-issue.md)
- [Noções básicas do DHCP (protocolo de configuração de host dinâmico)](./dynamic-host-configuration-protocol-basics.md)
- [Diretrizes gerais para solucionar problemas do DHCP](./general-guidance-to-troubleshoot-dhcp.md)
- [Como usar o endereçamento TCP/IP automático sem um servidor DHCP](./how-to-use-automatic-tcpip-addressing-without-a-dh.md)
- [Solucionar problemas no cliente DHCP](./troubleshoot-problems-on-dhcp-client.md)
- [Solucionar problemas no servidor DHCP](./troubleshoot-problems-on-dhcp-server.md)

## <a name="troubleshoot-fsrm"></a>Solucionar problemas de FSRM

- [Solução de problemas do Gerenciador de Recursos de Servidor de Arquivos](../storage/fsrm/troubleshooting-file-server-resource-manager.md)

## <a name="troubleshoot-guarded-fabric"></a>Solucionar problemas de malha protegida

- [Solução de problemas usando a ferramenta de diagnóstico de malha protegida](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-diagnostics.md)
- [Solucionando problemas do serviço guardião de host](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hgs.md)
- [Solucionando problemas do serviço guardião de host](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts.md)

## <a name="troubleshoot-multi-site-ras"></a>Solucionar problemas de RAS multissite

- [Como solucionar problemas da habilitação de multissite](../remote/remote-access/ras/multisite/troubleshoot/troubleshooting-enabling-multisite.md)
- [Solução de problemas de adição de pontos de entrada](../remote/remote-access/ras/multisite/troubleshoot/troubleshooting-adding-entry-points.md)
- [Como solucionar problemas de configuração do controlador de domínio de ponto de entrada](../remote/remote-access/ras/multisite/troubleshoot/troubleshooting-setting-the-entry-point-domain-controller.md)
- [Como solucionar problemas de URLs de investigação da Web](../remote/remote-access/ras/multisite/troubleshoot/troubleshooting-web-probe-urls.md)

## <a name="troubleshoot-nano-server"></a>Solução de problemas do nano Server

- [Solução de problemas do Nano Server](../get-started/troubleshooting-nano-server.md)

## <a name="troubleshoot-nic-teaming"></a>Solucionar problemas de agrupamento NIC

- [Como solucionar problemas de agrupamento NIC](../networking/technologies/nic-teaming/troubleshooting-nic-teaming.md)

## <a name="troubleshoot-otp-authentication"></a>Solucionar problemas de autenticação OTP

- [Como solucionar problemas de autenticação](../remote/remote-access/ras/otp/troubleshoot/troubleshooting-authentication-issues.md)
- [Como solucionar problemas de habilitação de OTP](../remote/remote-access/ras/otp/troubleshoot/troubleshooting-enabling-otp.md)

## <a name="troubleshoot-qos"></a>Solucionar problemas de QoS

- [Perguntas frequentes sobre QoS](../networking/technologies/qos/qos-policy-faq.md)

## <a name="troubleshoot-s2d"></a>Solucionar problemas de S2D

- [Solução de problemas Espaços de Armazenamento Diretos](../storage/storage-spaces/troubleshooting-storage-spaces.md)
- [Espaços de Armazenamento Diretos-perguntas frequentes](../storage/storage-spaces/storage-spaces-direct-faq.md)
- [Espaços de Armazenamento Diretos integridade e Estados operacionais](../storage/storage-spaces/storage-spaces-states.md)
- [Coletar dados de diagnóstico com Espaços de Armazenamento Diretos](../storage/storage-spaces/data-collection.md)
- [Gerenciamento de integridade de memória de classe de armazenamento (NVDIMM-N) no Windows](../storage/storage-spaces/storage-class-memory-health.md)

## <a name="troubleshoot-sdn"></a>Solucionar problemas de SDN

- [Solucionar problemas de SDN](../networking/sdn/troubleshoot/troubleshoot-software-defined-networking.md)
- [Solucionar problemas de pilha de rede definida pelo software do Windows Server](../networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack.md)

## <a name="troubleshoot-rds-session-connectivity"></a>Solucionar problemas de conectividade de sessão RDS

- [Solução de problemas gerais de conexão de Área de Trabalho Remota](../remote/remote-desktop-services/troubleshoot/rdp-error-general-troubleshooting.md)
- [Os clientes não podem se conectar e obter o erro de classe não registrada](../remote/remote-desktop-services/troubleshoot/rdp-error-class-not-registered.md)
- [Os clientes não podem se conectar e ver o erro não há licenças disponíveis](../remote/remote-desktop-services/troubleshoot/rdp-error-no-licenses-available.md)
- [O usuário não pode se autenticar ou deve se autenticar duas vezes](../remote/remote-desktop-services/troubleshoot/cannot-authenticate-or-must-authenticate-twice.md)
- [Na conexão, o usuário recebe a mensagem o serviço de Área de Trabalho Remota está atualmente ocupado](../remote/remote-desktop-services/troubleshoot/remote-desktop-service-currently-busy.md)
- [O cliente de Área de Trabalho Remota se desconecta e não pode se reconectar à mesma sessão](../remote/remote-desktop-services/troubleshoot/rdp-client-disconnects-cannot-reconnect-same-session.md)
- [Um laptop remoto se desconecta da rede sem fio](../remote/remote-desktop-services/troubleshoot/remote-laptop-disconnects-wireless-network.md)
- [Desempenho ruim ou problemas com o aplicativo durante a conexão de área de trabalho remota](../remote/remote-desktop-services/troubleshoot/poor-performance-or-application-problems.md)

## <a name="troubleshoot-shielded-vm"></a>Solucionar problemas de VM blindada

- [Solucionar problemas de VMs blindadas](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms.md)

## <a name="troubleshoot-software-restriction-policies"></a>Solucionar problemas de diretivas de restrição de software

- [Solucionar problemas de políticas de restrição de software](../identity/software-restriction-policies/troubleshoot-software-restriction-policies.md)

## <a name="troubleshoot-storage-migration"></a>Solucionar problemas de migração de armazenamento

- [Problemas conhecidos do serviço de migração de armazenamento](../storage/storage-migration-service/known-issues.md)
- [FAQ (perguntas frequentes) sobre o serviço de migração de armazenamento](../storage/storage-migration-service/faq.md)

## <a name="troubleshoot-storage-replica"></a>Solucionar problemas de réplica de armazenamento

- [Problemas conhecidos com a Réplica de Armazenamento](../storage/storage-replica/storage-replica-known-issues.md)
- [Perguntas frequentes sobre Réplica de Armazenamento](../storage/storage-replica/storage-replica-frequently-asked-questions.md)

## <a name="troubleshoot-user-profiles"></a>Solucionar problemas de perfis de usuário

- [Solucionar problemas de perfis de usuário com eventos](../storage/folder-redirection/troubleshoot-user-profiles-events.md)

## <a name="troubleshoot-vrss"></a>Solucionar problemas de vRSS

- [Perguntas frequentes sobre o vRSS](../networking/technologies/vrss/vrss-faq.md)

## <a name="troubleshoot-webproxy"></a>Solucionar problemas de WebProxy

- [Como solucionar problemas de proxy de aplicativo Web](../remote/remote-access/web-application-proxy/troubleshooting-web-application-proxy.md)

## <a name="troubleshoot-windows-admin-center"></a>Solucionar problemas do Windows Admin Center

- [Etapas de solução de problemas comuns do Windows Admin Center](../manage/windows-admin-center/support/troubleshooting.md)
- [Problemas conhecidos do Windows Admin Center](../manage/windows-admin-center/support/known-issues.md)
- [Perguntas frequentes sobre o Windows Admin Center](../manage/windows-admin-center/understand/faq.md)