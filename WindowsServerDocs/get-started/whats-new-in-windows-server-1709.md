---
title: Novidades no Windows Server, versão 1709
description: Quais são os novos recursos de computação, identidade, gerenciamento, automação, rede, segurança, armazenamento.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.date: 06/03/2019
ms.openlocfilehash: e17a636c5bf06d194abd1bfe9b6d20970773e993
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501398"
---
# <a name="whats-new-in-windows-server-version-1709"></a>Novidades no Windows Server versão 1709

>Aplica-se a: Windows Server (canal semestral)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;Para saber mais sobre os recursos mais recentes do Windows, consulte [o que há de novo no Windows Server](whats-new-in-windows-server.md). O conteúdo desta seção descreve as novidades e as alterações no Windows Server, versão 1709. Os novos recursos e alterações listados aqui são os que têm maior probabilidade de ter um impacto maior ao trabalhar com esta versão. Consulte também [Windows Server, versão 1709](https://blogs.technet.microsoft.com/windowsserver/2017/08/24/sneak-peek-1-windows-server-version-1709/).

> [!IMPORTANT]
> Windows Server, versão 1709 está fora do suporte a partir de 9 de abril de 2019.


## <a name="new-cadence-of-releases"></a>Nova cadência de versões

A partir desta versão, você tem duas opções para receber atualizações de recursos do Windows Server:
- **Canal (LTSC) de manutenção de longo prazo**: Esse é o negócio como de costume, com 5 anos de suporte base e 5 anos de suporte estendido. Você tem a opção de fazer upgrade para a próxima versão do LTSC a cada dois ou três anos, com o mesmo suporte dos últimos 20 anos.
- **Canal semestral (SAC)** : Isso é um benefício do Software Assurance e é totalmente suportado em produção. A diferença é que ele oferece suporte por 18 meses e há uma nova versão a cada seis meses.

Os canais de lançamento estão resumidos na tabela a seguir.

|   | Canal Semestral | Canal de Manutenção a Longo Prazo |
| ------------- | ------------- | ------------ |
| Cadência de lançamento  | Duas vezes por ano (primavera e outono)  | A cada dois a três anos |
| Cronograma de suporte  | Suporte base de produção de 18 meses  | Suporte base de cinco anos + suporte estendido de cinco anos |
| Disponibilidade  | Software Assurance ou Azure (hospedado em nuvem)  | Todos os canais |
| Convenção de nomenclatura  | Windows Server, versão MMAA  | YYYY do Windows Server |

Para obter mais informações, consulte [comparação dos canais de manutenção](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview).

## <a name="application-containers-and-micro-services"></a>Contêineres de aplicativo e microsserviços

- A imagem de contêiner do Server Core foi otimizada ainda mais para cenários de lift-and-shift, nos quais você pode migrar bases de código ou aplicativos em contêineres com alterações poucas alterações, além de ser 60% menor. 
- A imagem de contêiner do Nano servidor é aproximadamente 80% menor.
    - No Canal semestral do Windows Server, o servidor Nano como uma imagem de sistema operacional base do contêiner é reduzida de 390 MB para 80 MB.
- Contêineres do Linux com isolamento Hyper-V 

Para obter mais informações, consulte [Alterações no servidor nano na próxima versão do Windows Server](https://docs.microsoft.com/windows-server/get-started/nano-in-semi-annual-channel) e [Windows Server, versão 1709 para desenvolvedores](https://blogs.technet.microsoft.com/windowsserver/2017/09/13/sneak-peek-3-windows-server-version-1709-for-developers/).

## <a name="modern-management"></a>Gerenciamento moderno

Consulte o [Projeto Honolulu](https://docs.microsoft.com/windows-server/manage/honolulu/honolulu) para obter uma experiência simplificada, integrada e segura a fim de ajudar os administradores do setor de TI a gerenciarem a solução de problemas, a configuração e os cenários de manutenção essenciais.  O Projeto Honolulu inclui a próxima geração de ferramenta com uma interface simplificada, integrada, segura e ampliável.
O projeto Honolulu inclui uma nova experiência de gerenciamento intuitiva para gerenciar computadores, servidores do Windows, Clusters de failover, além da infraestrutura de hiperconvergência com base nos Espaços de Armazenamento Diretos, reduzindo os custos operacionais.

## <a name="compute"></a>Computação

**Contêineres do Nano e Server Core**: Primeiramente, essa versão é sobre levando a inovação de aplicativo. O Servidor Nano ou Nano como host é preterido e substituído pelo contêiner Nano, que é o Nano funcionando como imagem do contêiner. 

Para obter mais informações sobre contêineres, consulte [Visão geral da rede de contêineres](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview).

**Server Core como um host de contêiner** (e infraestrutura), fornece mais flexibilidade, densidade e desempenho para aplicativos atuais sob um processo de modernização e marca os novos aplicativos desenvolvidos usando o modelo de nuvem.

**Ordenação de Início de VM** também é melhorado com o reconhecimento de sistema operacional e de aplicativos, trazendo gatilhos avançados para quando uma VM é considerada como iniciada antes de iniciar a próxima.

O **Suporte de memória da classe de armazenamento para VMs** permite que os volumes de acesso direto formatados para NTFS sejam criados em DIMMs não voláteis e expostos às VMs Hyper-V. Isso permite que as VMs do Hyper-V aproveitem o desempenho de baixa latência de dispositivos de memória da classe de armazenamento.

A **Memória Persistente Virtualizada (vPMEM)** é habilitada ao criar um arquivo VHD (.vhdpmem) em um volume de acesso direto em um host, adicionando um Controlador vPMEM a uma VM, além de adicionar o dispositivo criado (.vhdpmem) a uma VM. O uso de arquivos vhdpmem nos volumes de acesso direto em um host para vPMEM proporciona a flexibilidade de alocação e aproveita um modelo de gerenciamento conhecido para adicionar discos às VMs.

**Armazenamento de contêiner: volumes de dados persistentes nos volumes compartilhados do cluster (CSV)** . No Windows Server, versão 1709, bem como no Windows Server 2016 com as últimas atualizações, adicionamos suporte para que os contêineres acesse volumes de dados persistentes localizados em CSVs, incluindo os CSVs em Espaços de Armazenamento Diretos. Isso proporciona o acesso persistente do contêiner de aplicativo ao volume, independentemente do nó de cluster no qual a instância do contêiner está em execução. Para obter mais informações, consulte [Suporte ao armazenamento de contêiner com Volumes Compartilhados do Cluster (CSV), Espaços de Armazenamento Diretos (S2D), Mapeamento Global de SMB](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Armazenamento de contêiner: volumes de dados persistentes com mapeamento global de SMB**. No Windows Server, versão 1709, adicionamos suporte para mapeamento de um compartilhamento de arquivo SMB para uma letra de unidade em um contêiner; isso é chamado de mapeamento global de SMB. Essa unidade mapeada fica acessível para todos os usuários no servidor local, para que o contêiner de E/S no volume de dados pode passar da unidade montada para o compartilhamento de arquivos subjacente. Para obter mais informações, consulte [Suporte ao armazenamento de contêiner com Volumes Compartilhados do Cluster (CSV), Espaços de Armazenamento Diretos (S2D), Mapeamento Global de SMB](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Formato de arquivo de configuração de máquina virtual (atualizado)** . Um outro arquivo (.vmgs) foi adicionado a máquinas virtuais com uma versão de configuração 8.2 e superior. VMGS significa o estado de convidado da VM e é um novo arquivo interno que inclui o estado do dispositivo que já fazia parte do arquivo de estado de tempo de execução da VM.

## <a name="security-and-assurance"></a>Segurança e garantia

A **Criptografia de rede** permite criptografar rapidamente os segmentos de rede na infraestrutura de rede definida pelo software a fim de atender às necessidades de segurança e conformidade.

O **Serviço Guardião de Host (HGS)** como uma VM protegida está habilitado. Antes desta versão, a recomendação era implantar um cluster físico de três nós. Embora isso garanta que o ambiente de HGS não seja comprometido por um administrador, frequentemente o custo era alto.

Agora há suporte para **Linux como VM protegida**.

Para obter mais informações, consulte [Visão geral de malha e VMs protegidas](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms).

**Vulnerabilidade SMBLoris** Um problema, conhecido como "SMBLoris", que pode resultar em negação de serviço, foi resolvido.

## <a name="storage"></a>Armazenamento

**A réplica de armazenamento**: Agora, a proteção de recuperação de desastres adicionada pela réplica de armazenamento no Windows Server 2016 é expandida para incluir:
- **Failover de teste**: a opção para montar o armazenamento de destino agora é possível por meio do recurso de failover de teste. Você pode montar um instantâneo do armazenamento replicado em nós de destino temporariamente para fins de teste ou backup.  Para obter mais informações, consulte [Perguntas Frequentes sobre a Réplica de Armazenamento](https://aka.ms/srfaq). 
- **Suporte de Paulo projetos**: Suporte para gráficas de gerenciamento de replicação de servidor para servidor agora está disponível em são Paulo do projeto. Isso elimina a necessidade de usar o PowerShell para gerenciar uma carga de trabalho de proteção contra desastres comuns.

**SMB**: 
- **Remoção de autenticação SMB1 e convidado**: Windows Server, versão 1709 não instala o cliente do SMB1 e o servidor por padrão. Além disso, a capacidade de autenticar como um convidado no SMB2 e posterior está desativada por padrão. Para obter mais informações, consulte [o SMBv1 não é instalado por padrão no Windows 10, versão 1709, e no Windows Server, versão 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilidade e segurança SMB2/SMB3**: Opções adicionais para segurança e compatibilidade de aplicativos foram adicionadas, incluindo a capacidade de desabilitar oplocks em SMB2 + para aplicativos herdados, bem como exigir a assinatura ou criptografia cada per-conexão de um cliente. Para obter mais informações, consulte a ajuda do módulo SMBShare PowerShell.

**Eliminação de duplicação de dados**: 
- **Eliminação de duplicação de dados agora dá suporte a ReFS**: Você deve escolher entre as vantagens de um sistema de arquivos modernos com o ReFS e eliminação de duplicação de dados não é mais: agora, você pode habilitar a eliminação de duplicação sempre que você pode habilitar o ReFS. Aumente a eficiência do armazenamento em mais de 95% com ReFS.
- **Da API para entrada/saída otimizada para volumes com eliminação de duplicação**: Os desenvolvedores podem aproveitar o conhecimento de eliminação de duplicação de dados tem sobre como armazenar dados com eficiência para mover dados entre servidores, volumes e clusters com eficiência.

## <a name="remote-desktop-services-rds"></a>RDS (Serviços de Área de Trabalho Remota)

**RDS é integrado ao Azure AD**, portanto, os clientes podem aproveitar as políticas de Acesso condicional, Autenticação multifator, Autenticação integrada a outros aplicativos SaaS usando o Azure AD e muito mais. Para obter mais informações, consulte [Integrar o Azure AD Domain Services à implantação do RDS](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).

>[!TIP]
>Para uma espiada no outras alterações empolgantes chegando ao RDS, consulte [dos serviços de área de trabalho remota: Atualizações e inovações futuras](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/20/first-look-at-updates-coming-to-remote-desktop-services/)

## <a name="networking"></a>Rede

Suporte para **Roteamento de malha do Docker**. Malha de roteamento de ingresso faz parte do [modo nuvem](https://docs.docker.com/engine/swarm/), a solução de organização nativa do Docker para contêineres. Para obter mais informações, consulte [Malha de roteamento do Docker com Windows Server versão 1709](https://blogs.technet.microsoft.com/virtualization/2017/09/26/dockers-ingress-routing-mesh-available-with-windows-server-version-1709/).

**Novos recursos para Docker** estão disponíveis. Para obter mais informações, consulte [Novidades do Docker com o Windows Server 1709](https://blog.docker.com/2017/09/docker-windows-server-1709/).

**Windows Networking em paridade com o Linux para o Kubernetes**: Windows agora está no Linux em termos de rede. Os clientes podem implantar sistemas operacionais mistos, clusters de Kubernetes em qualquer ambiente incluindo o Azure, no local e em pilhas de nuvem de terceiros com os mesmos primitivos e topologias de rede compatíveis com o Linux sem precisar de qualquer solução alternativa ou extensões do switch.

**Pilha de rede de núcleo**: Vários recursos da pilha de rede de núcleo estão aprimorados. Para obter mais informações sobre esses recursos, consulte [Principais recursos da pilha de rede na Atualização do Windows 10 para Criadores](https://blogs.technet.microsoft.com/networking/2017/07/13/core-network-stack-features-in-the-creators-update-for-windows-10/).
- **Abrir rápida de TCP (TFO)** : Foi adicionado suporte para a TFO para otimizar o processo de handshake de 3 vias TCP. O TFO estabelece um cookie TFO seguro na primeira conexão usando um handshake de três vias.  As conexões subsequentes ao mesmo servidor usam o cookie TFO em vez de um handshake de três vias para se conectar sem afetar o tempo de ida e volta.
- **CÚBICA**: Experimental Windows implementação nativa CÚBICA, um algoritmo de controle de congestionamento de TCP está disponível. Os seguintes comandos habilitam ou desabilitam o CUBIC, respectivamente.

    ```
    netsh int tcp set supplemental template=internet congestionprovider=cubic
    netsh int tcp set supplemental template=internet congestionprovider=compound
    ```

- **Receber janela Autotuning**: Lógica de autotuning TCP calcula o parâmetro "janela de recebimento" de uma conexão TCP.  As conexões de alta velocidade e/ou demoradas precisam desse algoritmo para alcançar as características de bom desempenho.  Nesta versão, o algoritmo é modificado para usar uma função degrau para conversão no valor máximo de janela de recepção para uma determinada conexão.
- **Status do TCP API**: Uma nova API é introduzida chamado SIO_TCP_INFO.  SIO_TCP_INFO permite que os desenvolvedores consultem informações sobre conexões TCP individuais usando a opção de soquete.
- **IPv6**: Há vários aprimoramentos no IPv6 nesta versão.
  - **RFC 6106** dar suporte a: 6106 RFC que permite a configuração de DNS por meio de anúncios de roteador (RAs). Você pode usar o seguinte comando para habilitar ou desabilitar o suporte de RFC 6106:

    ```
    netsh int ipv6 set interface <ifindex> rabaseddnsconfig=<enabled | disabled>
    ```

  - **Fluxo de rótulos**: Começando com a atualização de criadores de saída TCP e UDP pacotes via IPv6 têm esse campo definido como um hash de 5-tupla (IP Src, Dst IP, porta de origem, porta de destino).  Isso melhora a eficiência dos datacenters somente com IPv6 para balanceamento de carga os classificação de fluxo. Para habilitar flowlabels:

    ```
    netsh int ipv6 set flowlabel=[disabled|enabled] (enabled by default)
    netsh int ipv6 set global flowlabel=<enabled | disabled>
    ```

  - **ISATAP e 6to4**: Como uma etapa em direção a substituição futura, a atualização para criadores terá essas tecnologias desabilitadas por padrão.
- **Detecção de gateways inativos (DGD)** : O algoritmo DGD automaticamente faz a transição conexões na outro gateway quando o gateway atual está inacessível. Nesta versão, o algoritmo é melhorado para testar periodicamente o ambiente de rede.
- O [Test-NetConnection](https://technet.microsoft.com/itpro/powershell/windows/nettcpip/test-netconnection) é um cmdlet nativo do Windows PowerShell que executa diversos diagnósticos de rede.  Nesta versão, aprimoraram o cmdlet para fornecer informações detalhadas sobre a seleção de rota, bem como a seleção do endereço de origem.

**Rede definida pelo software**

- A **Criptografia de rede virtual** é um novo recurso que oferece a capacidade de criptografar o tráfego de rede virtual entre Máquinas virtuais que se comunicam entre si em sub-redes marcadas com "Criptografia habilitada". Esse recurso usa o Protocolo DTLS na sub-rede virtual para criptografar os pacotes.  O DTLS oferece proteção contra interceptações, falsificação e falsificação por qualquer pessoa com acesso à rede física.
 
**Windows 10 VPN**

- **Túneis de infraestrutura de pré-logon**. Por padrão, a VPN do Windows 10 não cria automaticamente Túneis de infraestrutura quando os usuários não estão conectados ao computador ou dispositivo. Você pode configurar a VPN do Windows 10 para criar automaticamente Túneis de infraestrutura de pré-login usando o recurso de encapsulamento do dispositivo (pré-login) no perfil da VPN.
- **Gerenciamento de computadores e dispositivos remotos**.  Você pode gerenciar clientes VPN do Windows 10 ao configurar o recurso de Encapsulamento do dispositivo (pré-login) no perfil da VPN. Além disso, você deve configurar a conexão da VPN para registrar dinamicamente os endereços IP atribuídos à interface da VPN com serviços internos de DNS.
- **Especificar gateways de pré-login**. Você pode especificar Gateways de pré-login com o recurso de Encapsulamento do dispositivo (pré-login) no perfil da VPN, combinado com filtros de tráfego para controlar quais sistemas de gerenciamento na rede corporativa estão acessíveis pelo túnel de dispositivo.

