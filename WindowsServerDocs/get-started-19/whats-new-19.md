---
title: Novidades no Windows Server 2019
description: Uma visão geral dos novos recursos no Windows Server 2019, incluindo a Experiência Desktop, o Serviço de Migração do Armazenamento, os Insights do Sistema, o Adaptador de Rede do Azure, melhorias para Espaços de Armazenamento Diretos e outras alterações.
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: high
ms.date: 06/04/2019
ms.openlocfilehash: 6337a3812cb6e1ca838c463bc811f8959d1f0714
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972363"
---
# <a name="whats-new-in-windows-server-2019"></a>Novidades no Windows Server 2019

> Aplica-se a: Windows Server 2019

Este tópico descreve alguns dos novos recursos do Windows Server 2019. O Windows Server 2019 foi criado sobre a base sólida do Windows Server 2016 e traz diversas inovações em quatro temas chave: nuvem híbrida, segurança, plataforma de aplicativos e HCI (infraestrutura hiperconvergente).

Para conhecer as novidades nos lançamentos do Canal Semestral do Windows Server, consulte [Novidades do Windows Server](../get-started/whats-new-in-windows-server.md).

## <a name="general"></a>Geral

### <a name="windows-admin-center"></a>Windows Admin Center

O Windows Admin Center é um aplicativo baseado em navegador e implantado localmente destinado a gerenciar servidores, clusters, infraestrutura hiperconvergente e computadores Windows 10. Ele é fornecido sem custo adicional além do Windows e vem pronto para uso no ambiente de produção.

Você pode instalar o Windows Admin Center no Windows Server 2019, bem como no Windows 10 e em versões anteriores do Windows e Windows Server e usá-lo para gerenciar servidores e clusters que executam o Windows Server 2008 R2 e versões mais recentes.

Para obter mais informações, consulte [Windows Admin Center](../manage/windows-admin-center/overview.md).

### <a name="desktop-experience"></a>Experiência desktop

Como o Windows Server 2019 é uma versão do LTSC (Canal de Manutenção em Longo Prazo), ele inclui a <b>Experiência Desktop</b>. (Versões do SAC \(Canal Semestral\) não incluem a Experiência Desktop por padrão; elas são estritamente versões da imagem de contêiner Server Core e Nano Server.) Assim como acontece com o Windows Server 2016, durante a configuração do sistema operacional você pode escolher entre instalações do Server Core ou do Windows Server com Experiência Desktop.

### <a name="system-insights"></a>Insights do Sistema

O Insights do Sistema é um novo recurso disponível no Windows Server 2019 que reúne funcionalidades locais de análise de previsão de modo nativo no Windows Server. Essas funcionalidades de previsão, cada qual apoiada por um modelo de aprendizado de máquina, analisam localmente os dados de sistema do Windows Server (como contadores de desempenho e eventos), fornecendo insights sobre o funcionamento de seus servidores e ajudando a reduzir as despesas operacionais associadas ao gerenciamento reativo dos problemas em suas implantações do Windows Server.

## <a name="hybrid-cloud"></a>Nuvem Híbrida

### <a name="server-core-app-compatibility-feature-on-demand"></a>Recurso sob demanda de compatibilidade de aplicativos do Server Core

O [FOD (recurso sob demanda) de compatibilidade de aplicativos do Server Core](./install-fod-19.md) melhora significativamente a compatibilidade de aplicativos da opção de instalação do Windows Server Core, incluindo um subconjunto de binários e componentes do Windows Server com a Experiência Desktop, sem adicionar o ambiente gráfico da Experiência Desktop do Windows Server, em si.  Isso é feito para aumentar a funcionalidade e a compatibilidade do Server Core, mantendo-no o mais enxuto possível.

Esse recurso opcional sob demanda está disponível em um ISO separado e pode ser adicionado somente a imagens e instalações do Windows Server Core, usando o DISM.

## <a name="security"></a>Segurança

### <a name="windows-defender-advanced-threat-protection-atp"></a>Proteção Avançada contra Ameaças do Windows Defender (ATP)

Os sensores de plataforma avançada e as ações de resposta do ATP expõem ataques no nível do kernel e da memória e respondem suprimindo arquivos maliciosos e encerrando processos mal-intencionados.

-   Para obter mais informações sobre o Windows Defender ATP, consulte [Visão geral das funcionalidades do Windows Defender ATP](/windows/security/threat-protection/windows-defender-atp/overview).

-   Para obter mais informações sobre a integração de servidores, consulte [Integrar servidores ao serviço do Windows Defender ATP](/windows/security/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection).

O **Windows Defender ATP Exploit Guard** é um novo conjunto de funcionalidades para prevenção contra invasões de host. Os quatro componentes do Windows Defender Exploit Guard foram projetados para bloquear o dispositivo diante de uma ampla variedade de vetores de ataque e bloquear comportamentos usados com frequência em ataques de malware, ao mesmo tempo permitindo que você mantenha o equilíbrio entre os riscos de segurança e os requisitos de produtividade.

-   A [ASR (Redução da Superfície de Ataque)](/windows/security/threat-protection/windows-defender-exploit-guard/attack-surface-reduction-exploit-guard?ocid=cx-blog-mmpc) é um conjunto de controles que as empresas podem habilitar para impedir que malwares alcancem o computador, bloqueando arquivos suspeitos de serem mal-intencionados (por exemplo, arquivos do Office), scripts, movimentos laterais, comportamentos de ransomware e ameaças baseadas em email.

-   A [Proteção de rede](/windows/security/threat-protection/microsoft-defender-atp/network-protection) protege o ponto de extremidade contra ameaças baseadas na Web, bloqueando qualquer processo de saída no dispositivo para endereços IP/hosts não confiáveis por meio do Windows Defender SmartScreen.

-   O [Acesso controlado a pastas](https://cloudblogs.microsoft.com/microsoftsecure/2017/10/23/stopping-ransomware-where-it-counts-protecting-your-data-with-controlled-folder-access/?ocid=cx-blog-mmpc?source=mmpc) protege dados confidenciais contra ransomware impedindo que processos não confiáveis acessem suas pastas protegidas.

-   O [Exploit Protection](/windows/security/threat-protection/windows-defender-exploit-guard/exploit-protection-exploit-guard) é um conjunto de mitigações para explorações de vulnerabilidade (em substituição ao EMET) que pode ser facilmente configurado para proteger o sistema e seus aplicativos.

O [Controle de Aplicativos do Windows Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) (também conhecido como a política de CI [integridade de código]) foi lançado no Windows Server 2016.
De acordo com os comentários dos clientes, este é um excelente conceito, mas difícil de implantar.
Para resolver isso, criamos políticas de CI padrão que aceitam todos os arquivos de caixa de entrada do Windows e aplicativos da Microsoft, como o SQL Server, e bloqueiam executáveis conhecidos que podem ignorar a CI. 

### <a name="security-with-software-defined-networking-sdn"></a>Segurança com SDN (Rede Definida pelo Software)

A [Segurança com SDN](../networking/sdn/security/sdn-security-top.md) oferece vários recursos para aumentar a confiança do cliente na execução de cargas de trabalho, seja localmente ou como um provedor de serviços na nuvem.

Essas melhorias de segurança estão integradas à plataforma SDN abrangente introduzida no Windows Server 2016.

Para ver uma lista completa das novidades na SDN, confira [Novidades na SDN para o Windows Server 2019](../networking/sdn/sdn-whats-new.md).

### <a name="shielded-virtual-machines-improvements"></a>Melhorias nas máquinas virtuais blindadas

- **Melhorias nas filiais**

    Agora você pode executar máquinas virtuais blindadas em computadores com conectividade intermitente ao Serviço Guardião de Host, aproveitando os novos recursos de [HGS de fallback](../security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md#fallback-configuration) e [modo offline](../security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md#offline-mode) . O HGS de fallback permite que você configure um segundo conjunto de URLs para o Hyper-V tentar caso não consiga acessar seu servidor HGS principal.

    O modo offline permite que você continue a iniciar suas VMs blindadas, mesmo se o HGS não puder ser alcançado, contato que a VM tenha sido iniciada com êxito pelo menos uma vez e as configurações de segurança do host não tenham sido alteradas desde então.

- **Melhorias na solução de problemas**

    Também facilitamos o processo para [solucionar problemas de suas máquinas virtuais blindadas](../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms.md) habilitando o suporte para o modo de sessão avançado VMConnect e o PowerShell Direct. Essas ferramentas são particularmente úteis se você tiver perdido a conectividade de rede com sua VM e precisar atualizar a configuração dela para restaurar o acesso.

    Esses recursos não precisam ser configurados e são disponibilizados automaticamente quando uma VM blindada é posta em um host Hyper-V que executa o Windows Server versão 1803 ou mais recente.

- **Suporte a Linux**

    Se você executa ambientes com múltiplos sistemas operacionais, agora o Windows Server 2019 oferece suporte para executar o Ubuntu, o Red Hat Enterprise Linux e o SUSE Linux Enterprise Server em máquinas virtuais blindadas.

### <a name="http2-for-a-faster-and-safer-web"></a>HTTP/2 para uma Web mais rápida e mais segura

- Melhorada a concentração de conexões para oferecer uma experiência de navegação sem interrupções e devidamente criptografada.

- Negociação do conjunto de criptografia do lado do servidor do HTTP/2 atualizado para mitigação automática de falhas de conexão e facilidade de implantação.

- Alterado nosso provedor de congestionamento TCP padrão para cúbico para oferecer maior taxa de transferência!

## <a name="storage"></a>Armazenamento

Eis algumas das alterações que fizemos no armazenamento do Windows Server 2019. Para obter detalhes, consulte [Novidades no armazenamento](../storage/whats-new-in-storage.md).

### <a name="storage-migration-service"></a>Serviço de Migração de Armazenamento

O Serviço de Migração de Armazenamento é uma nova tecnologia que facilita a migração dos servidores para uma versão mais recente do Windows Server. Ele fornece uma ferramenta gráfica que faz o inventário dos dados em servidores, transfere os dados e a configuração para servidores mais recentes e, em seguida, opcionalmente, move as identidades dos servidores antigos para os novos servidores para que os aplicativos e usuários não precisem alterar nada. Para obter mais informações, consulte [Serviço de Migração de Armazenamento](../storage/storage-migration-service/overview.md).

### <a name="storage-spaces-direct"></a>Espaços de Armazenamento Direct

Eis uma lista das novidades nos Espaços de Armazenamento Diretos. Para obter detalhes, consulte [Novidades nos Espaços de Armazenamento Diretos](../storage/whats-new-in-storage.md#storage-spaces-direct). Consulte também o [Azure Stack HCI](/azure-stack/operator/azure-stack-hci-overview) para obter informações sobre a aquisição de sistemas validados de Espaços de Armazenamento Diretos.

- **Eliminação de duplicação e compactação para volumes do ReFS**
- **Suporte nativo para memória persistente**
- **Resiliência aninhada para a infraestrutura hiperconvergente de dois nós na borda**
- **Clusters de dois servidores usando um pen drive como testemunha**
- **Suporte para o Windows Admin Center**
- **Histórico de desempenho**
- **Dimensionar até 4 PB por cluster**
- **A paridade acelerada por espelho é duas vezes mais rápida**
- **Detecção de exceções de latência da unidade**
- **Delimitar manualmente a alocação de volumes para aumentar a tolerância a falhas**

### <a name="storage-replica"></a>Réplica de Armazenamento

Eis as novidades na Réplica de Armazenamento. Para obter detalhes, consulte [Novidades na Réplica de Armazenamento](../storage/whats-new-in-storage.md#storage-replica).

- A Réplica de Armazenamento agora está disponível no Windows Server 2019 Standard Edition.
- O Failover de teste é um novo recurso que permite a montagem de armazenamento de destino para validar a replicação ou os dados de backup. Para obter mais informações, consulte [Perguntas Frequentes sobre a Réplica de Armazenamento](../storage/storage-replica/storage-replica-frequently-asked-questions.md).
- Melhorias de desempenho do log da Réplica de Armazenamento
- Suporte para o Windows Admin Center

## <a name="failover-clustering"></a>Clustering de failover

Eis a lista de Novidades no Clustering de Failover. Para obter detalhes, consulte [Novidades no Clustering de Failover](../failover-clustering/whats-new-in-failover-clustering.md).

- **Conjuntos de cluster**
- **Clusters com suporte ao Azure**
- **Migração de cluster entre domínios**
- **Testemunha USB**
- **Melhorias de infraestrutura de cluster**
- **A atualização com suporte a cluster é compatível com os Espaços de Armazenamento Diretos**
- **Melhorias da testemunha do compartilhamento de arquivo**
- **Proteção de cluster**
- **O Cluster de Failover não usa mais a autenticação NTLM**

## <a name="application-platform"></a>Plataformas de aplicativos

### <a name="linux-containers-on-windows"></a>Contêineres do Linux no Windows

Agora é possível executar contêineres baseados no Windows e no Linux no mesmo host de contêineres, usando o mesmo daemon do docker. Isso permite que você tenha um ambiente heterogêneo de host de contêineres, fornecendo flexibilidade aos desenvolvedores de aplicativos.

### <a name="built-in-support-for-kubernetes"></a>Suporte interno para Kubernetes

O Windows Server 2019 dá continuidade às melhorias de computação, redes e armazenamento das versões de canal semestral necessárias para dar suporte a Kubernetes no Windows. Mais detalhes estarão disponíveis em versões futuras do Kubernetes.

- A [Rede de contêineres](../networking/sdn/technologies/containers/container-networking-overview.md) do Windows Server 2019 melhora significativamente a usabilidade do Kubernetes no Windows por meio da melhoria da resiliência de rede da plataforma e do suporte a plug-ins de rede de contêineres.

- As cargas de trabalho implantadas em Kubernetes são capazes de usar a segurança de rede para proteger serviços do Linux e do Windows usando ferramentas inseridas.

### <a name="container-improvements"></a>Melhorias de contêiner

- **Identidade integrada melhorada**

    Tornamos a autenticação integrada do Windows em contêineres mais fácil e mais confiáveis, corrigindo várias limitações de versões anteriores do Windows Server.

- **Melhor compatibilidade do aplicativo**

    Colocar aplicativos baseados no Windows em contêineres acaba de ficar ainda mais fácil: A compatibilidade com aplicativos da imagem existente do *windowsservercore* foi aumentada. Para aplicativos com dependências adicionais de API, agora há uma terceira imagem de base: *Windows*.

- **Tamanho reduzido e melhor desempenho**

    Os tamanhos de download da imagem de contêiner de base, o tamanho em disco e os horários de inicialização foram melhorados. Isso acelera os fluxos de trabalho do contêiner

- **Experiência de gerenciamento usando o Windows Admin Center \(versão prévia\)**

    Nós tornamos mais fácil que nunca ver quais contêineres estão em execução em seu computador e gerenciar contêineres individuais com uma nova extensão para o Windows Admin Center. Procure a extensão "Contêineres" no [feed público do Windows Admin Center](../manage/windows-admin-center/configure/using-extensions.md).

### <a name="encrypted-networks"></a>Redes Criptografadas

[Redes Criptografadas](../networking/sdn/sdn-whats-new.md) – A criptografia de rede Virtual permite a criptografia do tráfego de rede virtual entre máquinas virtuais que se comunicam entre si dentro de sub-redes marcadas como **Criptografia Habilitada**.
Ela também utiliza o DTLS (Datagrama do protocolo TLS) na sub-rede virtual para criptografar os pacotes. O DTLS protege contra interceptações, falsificação e adulteração por qualquer pessoa com acesso à rede física.

### <a name="network-performance-improvements-for-virtual-workloads"></a>Melhorias no desempenho de rede para cargas de trabalho virtuais

As [Melhorias no desempenho de rede para cargas de trabalho virtuais](../networking/technologies/hpn/hpn-insider-preview.md) maximizam a taxa de transferência de rede para as máquinas virtuais, sem exigir que você ajuste constantemente ou faça um provisionamento excessivo em seu host. Isso reduz os custos de manutenção e das operações, aumentando a densidade disponível de seus hosts. Esses novos recursos são:

* Receber concentração de segmentos no vSwitch

* Fila com várias máquinas virtuais dinâmicas (d.VMMQ)

### <a name="low-extra-delay-background-transport"></a>Transporte em segundo plano com baixo atraso extra

O LEDBAT (Transporte em segundo plano com baixo atraso extra) é um provedor de controle de congestionamento de rede com latência otimizada projetado para conceder automaticamente largura de banda para usuários e aplicativos, consumindo toda a largura de banda disponível quando a rede não estiver em uso.
Essa tecnologia destina-se ao uso na implantação de grandes atualizações críticas em um ambiente de TI, sem afetar os serviços diretamente ligados ao cliente e a largura de banda associada.

### <a name="windows-time-service"></a>Serviço de Tempo do Windows

O [Serviço de Tempo do Windows](../networking/windows-time-service/insider-preview.md) inclui suporte real compatível com UTC a frações de segundos, um novo protocolo de tempo chamado de Protocolo de Tempo de Precisão e rastreabilidade de ponta a ponta.


### <a name="high-performance-sdn-gateways"></a>Gateways SDN de alto desempenho

Os [Gateways SDN de alto desempenho](../networking/sdn/gateway-performance.md) no Windows Server 2019 melhoram significativamente o desempenho das conexões IPsec e GRE, fornecendo uma taxa de transferência de altíssimo desempenho com muito menos utilização da CPU.
<br/>

### <a name="new-deployment-ui-and-windows-admin-center-extension-for-sdn"></a>Nova extensão da interface do usuário de implantação e do Windows Admin Center para SDN

Agora, com o Windows Server 2019, ficou fácil realizar implantação e gerenciamento por meio de uma nova extensão da interface do usuário de implantação e do Windows Admin Center que permitem a qualquer pessoa aproveitar o poder da SDN.

### <a name="persistent-memory-support-for-hyper-v-vms"></a>Suporte de Memória Persistente para VMs do Hyper-V

Para aproveitar a alta taxa de transferência e a baixa latência da memória persistente (também conhecida como memória de classe de armazenamento) nas máquinas virtuais, agora ela pode ser projetada diretamente nas VMs. Isso pode ajudar a reduzir drasticamente a latência das transações de banco de dados ou reduzir o tempo de recuperação para bancos de dados em memória de baixa latência em caso de falha.
