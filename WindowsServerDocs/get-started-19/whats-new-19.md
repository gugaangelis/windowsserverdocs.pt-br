---
title: Novidades no Windows Server 2019
description: Uma visão geral dos novos recursos no Windows Server 2019, incluindo a Experiência Desktop, o Serviço de Migração do Armazenamento, os Insights do Sistema, o Adaptador de Rede do Azure, aprimoramentos para Espaços de Armazenamento Diretos e outras alterações.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: high
ms.openlocfilehash: 4c454fc397b662e313d5cfb7ed02a83dc7059207
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992539"
---
# Novidades no Windows Server 2019

Este tópico descreve alguns dos novos recursos no Windows Server 2019. O Windows Server 2019 aproveita a base forte do Windows Server 2016 e apresenta várias inovações abrangendo quatro temas principais: nuvem híbrida, segurança, plataforma de aplicativos e infraestrutura hiperconvergente (HCI). Para conhecer as novidades no Windows Server, versão 1809, veja [as novidades no Windows Server, versão 1809](../get-started/whats-new-in-windows-server-1809.md).

## Geral

### Experiência Desktop

Como o Windows Server 2019 é uma versão do Canal de Manutenção a Longo Prazo (LTSC), ele inclui a <b>Experiência Desktop</b>. (Ela não está incluída no Windows Server, versão 1709, no Windows Server, versão 1803 nem no Windows Server, versão 1809, porque versões do Canal Semestral \(SAC\) não incluem a Experiência Desktop por design; elas são estritamente versões de imagem de contêiner do Server Core e do Nano.) Assim como acontece com o Windows Server 2016, durante a configuração do sistema operacional, você pode escolher entre as instalações do Server Core ou Server com Experiência Desktop.

### Insights do Sistema

Insights do sistema é um novo recurso disponível no Windows Server 2019 que reúne recursos locais de análise de previsão nativamente para o Windows Server. Esses recursos de previsão, cada apoiadas por um modelo de aprendizado de máquina, localmente analisar dados de sistema do Windows Server, como contadores de desempenho e eventos, fornecendo percepção o funcionamento de seus servidores e ajudando você reduzir as despesas operacionais associado ao gerenciamento reativamente problemas em suas implantações do Windows Server.

## Nuvem híbrida

### Recurso de compatibilidade de aplicativo do Server Core sob demanda

O [recurso de compatibilidade de aplicativo do Server Core sob demanda (FOD)](https://docs.microsoft.com/windows-server/get-started-19/install-fod-19) melhora significativamente a compatibilidade de aplicativo a opção de instalação Server Core do Windows, incluindo um subconjunto de binários e componentes do Windows Server com experiência Desktop, sem adicionar o ambiente de gráfico de experiência de área de trabalho do Windows Server em si.  Isso é feito para aumentar a funcionalidade e a compatibilidade do Server Core, o mantendo o mais enxuto possível.  

Esse recurso opcional sob demanda está disponível em um ISO separado e pode ser adicionado a imagens somente e instalações Server Core do Windows usando o DISM. 

## Segurança

### Proteção Avançada contra Ameaças do Windows Defender (ATP)

Os sensores de plataforma profunda e as ações de resposta do ATP expõem ataques no nível do kernel e de memória e respondem ao eliminar arquivos mal-intencionados e encerrar processos mal-intencionados.

-   Para obter mais informações sobre o Windows Defender ATP, consulte [Visão geral dos recursos do Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/overview).

-   Para obter mais informações em servidores de integração, ver [servidores integrados ao serviço Windows Defender ATP](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection).

**Windows Defender ATP Exploit Guard** é um novo conjunto de recursos de prevenção contra invasões de host. Os quatro componentes do Windows Defender Exploit Guard são projetados para bloquear o dispositivo em relação a uma ampla variedade de vetores de ataque e comportamentos de bloco costuma ser usados em ataques de malware, enquanto você a riscos de segurança de equilíbrio e requisitos de produtividade.

-   [Redução da Superfície de Ataque (ASR)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/attack-surface-reduction-exploit-guard?ocid=cx-blog-mmpc) é um conjunto de controles que as empresas podem habilitar para impedir que malware atinja o computador, bloqueando arquivos suspeitos mal-intencionados (por exemplo, arquivos do Office), scripts, movimento lateral, comportamento de ransomware e ameaças baseadas em email.

-   [Proteção de rede](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/network-protection-exploit-guard?ocid=cx-blog-mmpc) protege o ponto de extremidade contra ameaças baseadas na web, bloqueando qualquer processo de saída no dispositivo para endereços IP/hosts não confiáveis por meio do Windows Defender SmartScreen.

-   [Acesso controlado a pastas](https://cloudblogs.microsoft.com/microsoftsecure/2017/10/23/stopping-ransomware-where-it-counts-protecting-your-data-with-controlled-folder-access/?ocid=cx-blog-mmpc?source=mmpc) protege dados confidenciais contra ransomware bloqueando processos não confiáveis acessem suas pastas protegidas.

-   [Exploit protection](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-exploit-guard/exploit-protection-exploit-guard) é um conjunto de mitigações de explorações de vulnerabilidade (substituindo EMET) que podem ser facilmente configuradas para proteger seu sistema e aplicativos.



[Controle de Aplicativos do Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) (também conhecido como a política de integridade de código (CI)) foi lançada no Windows Server 2016.
De acordo com os comentários dos clientes, este é um excelente conceito, mas difícil de implantar.
Para resolver isso, criamos políticas de CI padrão, que aceitam todos os arquivos de caixa de entrada do Windows e aplicativos da Microsoft, como o SQL Server, e bloqueiam arquivos executáveis conhecidos que podem ignorar a CI. 

### Segurança com o Software de rede definida (SDN)

[Segurança com SDN](https://docs.microsoft.com/windows-server/networking/sdn/security/sdn-security-top) oferece vários recursos para aumentar a confiança do cliente em execução cargas de trabalho, localmente, ou como um provedor de serviços na nuvem. 

Esses aprimoramentos de segurança estão integrados à plataforma SDN abrangente introduzida no Windows Server 2016.

Para obter uma lista completa do que há de nova na, consulte SDN [Novidades no SDN para o Windows Server 2019](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new).

### Melhorias de máquinas virtuais blindadas

- **Melhorias do Branch office**

    Agora você pode executar máquinas virtuais blindadas em computadores com conectividade intermitente para o serviço de guardião de Host, aproveitando os novos recursos de [fallback HGS](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#fallback-configuration) e [modo off-line](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office#offline-mode) . HGS fallback permite que você configure um segundo conjunto de URLs do Hyper-V tentar se ele não puder acessar o servidor principal do HGS.

    Modo offline permite que você continue a iniciar suas VMs blindadas, mesmo se HGS não puder ser atingido, desde que a VM foi iniciado com êxito uma vez, e a configuração de segurança do host não foi alterada.

- **Melhorias de solução de problemas**

    Podemos tiver também facilitou o processo para [Solucionar problemas de suas máquinas virtuais protegidas](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms) habilitando suporte para o modo de sessão avançado VMConnect e PowerShell Direct. Essas ferramentas são particularmente úteis se você tiver perdido a conectividade de rede com a VM e precisar atualizar a configuração dela para restaurar o acesso. 

    Esses recursos não precisam ser configurados e são disponibilizados automaticamente quando uma VM protegida é colocada em um host Hyper-V que estiver executando o Windows Server versão 1803 ou posterior.

- **Suporte a Linux**

    Se você executar ambientes Mistos, Windows Server 2019 agora oferece suporte ao Ubuntu, Red Hat Enterprise Linux e SUSE Linux Enterprise Server em execução dentro de máquinas virtuais blindadas.

### HTTP/2 para uma Web mais rápida e mais segura

- Aprimorada a concentração de conexões para oferecer uma experiência de navegação sem interrupções e criptografada corretamente.

- Atualizado negociação do conjunto de codificação do lado do servidor do HTTP/2 para redução automática de falhas de conexão e a facilidade de implantação.

- Alterado nosso provedor de congestionamento TCP padrão para cúbica para oferecer a você mais taxa de transferência!

## Armazenamento

Aqui estão algumas das alterações que fizemos para armazenamento no Windows Server 2019. Para obter detalhes, consulte [o que há de novo no armazenamento](../storage/whats-new-in-storage.md).

### Serviço de Migração do Armazenamento

O Serviço de Migração do Armazenamento é uma nova tecnologia que facilita migrar servidores para uma versão mais recente do Windows Server. Ele fornece uma ferramenta gráfica que faz o inventário de dados em servidores, transfere os dados e a configuração para servidores mais recentes e, em seguida, opcionalmente, move as identidades dos servidores antigos para os novos servidores para que aplicativos e usuários não precisem alterar nada. Para obter mais informações, consulte o [Serviço de Migração do Armazenamento](../storage/storage-migration-service/overview.md).

### Espaços de Armazenamento Diretos

Aqui está uma lista das novidades nos Espaços de Armazenamento Diretos. Para obter detalhes, consulte [o que há de novo nos Espaços de Armazenamento Diretos](../storage/whats-new-in-storage.md#storage-spaces-direct).

- **Eliminação de duplicação e compactação para volumes de ReFS**
- **Suporte nativo para a memória persistente**
- **Resiliência aninhada para a infraestrutura hiperconvergente dois nós na borda**
- **Clusters de dois servidores usando uma unidade flash USB como uma testemunha**
- **Suporte do Windows Admin Center**
- **Histórico de desempenho**
- **Dimensionar até 4 PB por cluster**
- **A paridade acelerada por espelho é duas vezes mais rápida**
- **Detecção de exceções de latência de unidade de disco**
- **Delimitar manualmente a alocação de volumes para aumentar a tolerância a falhas**

### Réplica de Armazenamento

Aqui está o que há de novo na Réplica de Armazenamento. Para obter detalhes, consulte [o que há de novo na Réplica de Armazenamento](../storage/whats-new-in-storage.md#storage-replica).

- A Réplica de Armazenamento agora está disponível no Windows Server 2019 Standard Edition.
- O Failover de teste é um novo recurso que permite a montagem de armazenamento de destino para validar os dados de backup ou a replicação. Para obter mais informações, consulte [Perguntas Frequentes sobre a Réplica de Armazenamento](../storage/storage-replica/storage-replica-frequently-asked-questions.md).
- Melhorias de desempenho de log da Réplica de Armazenamento
- Suporte do Windows Admin Center

## Clustering de Failover

Veja a seguir uma lista das novidades no Clustering de Failover. Para obter detalhes, consulte [o que há de novo no Clustering de Failover](../failover-clustering/whats-new-in-failover-clustering.md).

- **Conjuntos de cluster**
- **Clusters com reconhecimento do Azure**
- **Migração de cluster entre domínios**
- **Testemunha USB**
- **Melhorias de infraestrutura de cluster**
- **Atualização com Suporte a Cluster é compatível com Espaços de Armazenamento Diretos**
- **Aprimoramentos de testemunha de compartilhamento de arquivos**
- **Proteção do cluster**
- **Clustering de Failover deixa de usar a autenticação NTLM**

## Plataforma de aplicativos

### Contêineres do Linux no Windows

Agora é possível executar o Windows e contêineres do Linux no mesmo host do contêiner, usando o mesmo daemon do docker. Isso permite que você tenha um ambiente de host do contêiner heterogêneos enquanto fornece flexibilidade para desenvolvedores de aplicativos.

### Suporte de construção para Kubernetes

O Windows Server 2019 mantém as melhorias de computação, rede e armazenamento das versões de canal semestral necessárias para dar suporte a plataforma Kubernetes no Windows. Mais detalhes estarão disponíveis em versões futuras da Kubernetes.

- [Rede de contêineres](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview) no Windows Server 2019 melhora significativamente a usabilidade do Kubernetes no Windows ao aprimorar a resiliência de rede de plataforma e oferecer suporte a plug-ins de rede de contêineres.

- As cargas de trabalho implantadas na Kubernetes são capazes de usar a segurança de rede para proteger serviços Linux e Windows usando ferramentas incorporada.

### Melhorias de contêiner
    
- **Identidade integrada aprimorada**

    Fizemos a autenticação integrada do Windows em contêineres mais fácil e mais confiáveis, endereçamento várias limitações de versões anteriores do Windows Server.

- **Melhor compatibilidade de aplicativos**

    Os contêineres de aplicativos baseados em Windows apenas ficou mais fácil: aumentou a compatibilidade de aplicativo para a imagem *windowsservercore* existente. Para aplicativos com dependências adicionais de API, agora há uma imagem de base do terceiro: *windows*.

- **Tamanho reduzido e melhor desempenho**

    Os tamanhos de download de imagem do contêiner base, tamanho em disco e inicialização foram aprimorados. Isso acelera a fluxos de trabalho do contêiner

- **Experiência de gerenciamento usando o Windows Admin Center \(preview\)**

    Que fizemos mais fácil do que nunca para ver quais contêineres estão em execução em seu computador e gerenciar contêineres individuais com uma nova extensão para o Windows Admin Center. Procure a extensão "Contêineres" no [feed público do Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/using-extensions).

### Redes criptografadas

[Redes criptografados](https://docs.microsoft.com/windows-server/networking/sdn/sdn-whats-new) - criptografia de rede Virtual permite a criptografia de tráfego de rede virtual entre máquinas virtuais que se comunicam entre si em sub-redes marcadas como **Criptografia ativada**. Ele também utiliza datagrama Transport Layer Security (DTLS) na sub-rede virtual para criptografar pacotes. O DTLS oferece proteção contra interceptações, falsificação e falsificação por qualquer pessoa com acesso à rede física.

### Melhorias de desempenho de rede para cargas de trabalho virtuais

As [Melhorias de desempenho de rede para cargas de trabalho virtuais](https://docs.microsoft.com/windows-server/networking/technologies/hpn/hpn-insider-preview) maximizam a taxa de transferência de rede para máquinas virtuais sem a necessidade de você ajustar constantemente ou fazer um provisionamento excessivo de seu host. Isso reduz o custo de manutenção e operações aumentando a densidade disponível dos hosts seu. Esses novos recursos são:

* Receber o segmento concentração no vSwitch

* Fila de vários de máquina Virtual dinâmica (d.VMMQ)

### Atraso Extra baixa em segundo plano transporte

Baixa Extra atraso em segundo plano transporte (LEDBAT) é uma latência otimizada, provedor de controle de congestionamento de rede projetado para gerar automaticamente largura de banda para usuários e aplicativos, consumindo a toda largura de banda disponível quando a rede não estiver em uso.   
Essa tecnologia destina-se para uso na implantação de atualizações críticas de grandes em um ambiente de TI sem afetar o atendimento ao cliente frontal e largura de banda associada.

### Serviço Horário do Windows

O [Serviço Horário do Windows](https://docs.microsoft.com/windows-server/networking/windows-time-service/insider-preview) inclui true suporte compatível com UTC fração de segundo, um protocolo de novo horário chamado protocolo de tempo de precisão e o rastreamento de ponta a ponta.


### Gateways SDN de alto desempenho

[Gateways SDN de alto desempenho](https://docs.microsoft.com/windows-server/networking/sdn/gateway-performance) no Windows Server 2019 melhora significativamente o desempenho para conexões IPsec e GRE, fornecendo a taxa de transferência de alto desempenho com muito menos utilização da CPU.
<br/>

### Nova extensão de interface do usuário de implantação e o Windows Admin Center para SDN

Agora, com o Windows Server 2019, é fácil realizar a implantação e o gerenciamento por meio de uma nova interface do usuário da implantação e da extensão do Windows Admin Center que permitem a qualquer pessoa aproveitar o poder do SDN. 

### Suporte de Memória Persistente para VMs Hyper-V

Para aproveitar a alta taxa de transferência e baixa latência de memória persistente (ou seja memória de classe de armazenamento) em máquinas virtuais, ele agora pode ser projetado diretamente nas VMs. Isso pode ajudar a reduzir drasticamente a latência de transação do banco de dados ou reduzir o tempo de recuperação para bancos de dados de baixa latência de memória em caso de falha.

