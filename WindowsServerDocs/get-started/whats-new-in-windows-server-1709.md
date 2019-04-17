---
title: Novidades no Windows Server, versão 1709
description: Quais são os novos recursos de computação, identidade, gerenciamento, automação, rede, segurança, armazenamento.
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: 1d63721dde484756e67b68bcff078257c130ae36
ms.sourcegitcommit: 2ffd664a771421231a583d0b90827e922ed47ae3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2018
ms.locfileid: "4646123"
---
# Novidades no Windows Server versão 1709

>Aplicável a: Windows Server (canal semestral)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;O conteúdo desta seção descreve as novidades e as alterações no Windows Server, versão 1709. Os novos recursos e alterações listados aqui são os que têm maior probabilidade de ter um impacto maior ao trabalhar com esta versão. Consulte também [Windows Server, versão 1709](https://blogs.technet.microsoft.com/windowsserver/2017/08/24/sneak-peek-1-windows-server-version-1709/).
   

## Nova cadência de versões

A partir desta versão, você tem duas opções para receber atualizações de recursos do Windows Server:
- **Canal de manutenção a longo prazo (LTSC)**: abrange cinco anos de suporte base e cinco anos de suporte estendido. Você tem a opção de fazer upgrade para a próxima versão do LTSC a cada dois ou três anos, com o mesmo suporte dos últimos 20 anos.
- **Canal semestral (SAC)**: esse é um benefício do Software Assurance e é totalmente compatível na produção. A diferença é que ele oferece suporte por 18 meses e há uma nova versão a cada seis meses.

Os canais de lançamento estão resumidos na tabela a seguir.

|   | Canal Semestral | Canal de Manutenção a Longo Prazo |
| ------------- | ------------- | ------------ |
| Cadência de lançamento  | Duas vezes por ano (primavera e outono)  | A cada dois a três anos |
| Cronograma de suporte  | Suporte base de produção de 18 meses  | Suporte base de cinco anos + suporte estendido de cinco anos |
| Disponibilidade  | Software Assurance ou Azure (hospedado em nuvem)  | Todos os canais |
| Convenção de nomenclatura  | Windows Server, versão MMAA  | YYYY do Windows Server |

Para obter mais informações, consulte a [Visão geral do canal semestral do Windows Server](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview).

## Contêineres de aplicativo e microsserviços

- A imagem de contêiner do Server Core foi otimizada ainda mais para cenários de lift-and-shift, nos quais você pode migrar bases de código ou aplicativos em contêineres com alterações poucas alterações, além de ser 60% menor. 
- A imagem de contêiner do Nano servidor é aproximadamente 80% menor.
    - No Canal semestral do Windows Server, o servidor Nano como uma imagem de sistema operacional base do contêiner é reduzida de 390 MB para 80 MB.
- Contêineres do Linux com isolamento Hyper-V 

Para obter mais informações, consulte [Alterações no servidor nano na próxima versão do Windows Server](https://docs.microsoft.com/windows-server/get-started/nano-in-semi-annual-channel) e [Windows Server, versão 1709 para desenvolvedores](https://blogs.technet.microsoft.com/windowsserver/2017/09/13/sneak-peek-3-windows-server-version-1709-for-developers/).

## Gerenciamento moderno

Consulte o [Projeto Honolulu](https://docs.microsoft.com/windows-server/manage/honolulu/honolulu) para obter uma experiência simplificada, integrada e segura a fim de ajudar os administradores do setor de TI a gerenciarem a solução de problemas, a configuração e os cenários de manutenção essenciais.  O Projeto Honolulu inclui a próxima geração de ferramenta com uma interface simplificada, integrada, segura e ampliável.
O projeto Honolulu inclui uma nova experiência de gerenciamento intuitiva para gerenciar computadores, servidores do Windows, Clusters de failover, além da infraestrutura de hiperconvergência com base nos Espaços de Armazenamento Diretos, reduzindo os custos operacionais.

## Computação

**Nano contêiner e contêiner do Server Core**: em primeiro lugar, esta versão promove a inovação dos aplicativos. O Servidor Nano ou Nano como host é preterido e substituído pelo contêiner Nano, que é o Nano funcionando como imagem do contêiner. 

Para obter mais informações sobre contêineres, consulte [Visão geral da rede de contêineres](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview).

**Server Core como um host de contêiner** (e infraestrutura), fornece mais flexibilidade, densidade e desempenho para aplicativos atuais sob um processo de modernização e marca os novos aplicativos desenvolvidos usando o modelo de nuvem.

**Ordenação de Início de VM** também é melhorado com o reconhecimento de sistema operacional e de aplicativos, trazendo gatilhos avançados para quando uma VM é considerada como iniciada antes de iniciar a próxima.

O **Suporte de memória da classe de armazenamento para VMs** permite que os volumes de acesso direto formatados para NTFS sejam criados em DIMMs não voláteis e expostos às VMs Hyper-V. Isso permite que as VMs do Hyper-V aproveitem o desempenho de baixa latência de dispositivos de memória da classe de armazenamento.

A **Memória Persistente Virtualizada (vPMEM)** é habilitada ao criar um arquivo VHD (.vhdpmem) em um volume de acesso direto em um host, adicionando um Controlador vPMEM a uma VM, além de adicionar o dispositivo criado (.vhdpmem) a uma VM. O uso de arquivos vhdpmem nos volumes de acesso direto em um host para vPMEM proporciona a flexibilidade de alocação e aproveita um modelo de gerenciamento conhecido para adicionar discos às VMs.

**Armazenamento de contêiner: volumes de dados persistentes nos volumes compartilhados do cluster (CSV)**. No Windows Server, versão 1709, bem como no Windows Server 2016 com as últimas atualizações, adicionamos suporte para que os contêineres acesse volumes de dados persistentes localizados em CSVs, incluindo os CSVs em Espaços de Armazenamento Diretos. Isso proporciona o acesso persistente do contêiner de aplicativo ao volume, independentemente do nó de cluster no qual a instância do contêiner está em execução. Para obter mais informações, consulte [Suporte ao armazenamento de contêiner com Volumes Compartilhados do Cluster (CSV), Espaços de Armazenamento Diretos (S2D), Mapeamento Global de SMB](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Armazenamento de contêiner: volumes de dados persistentes com mapeamento global de SMB**. No Windows Server, versão 1709, adicionamos suporte para mapeamento de um compartilhamento de arquivo SMB para uma letra de unidade em um contêiner; isso é chamado de mapeamento global de SMB. Essa unidade mapeada fica acessível para todos os usuários no servidor local, para que o contêiner de E/S no volume de dados pode passar da unidade montada para o compartilhamento de arquivos subjacente. Para obter mais informações, consulte [Suporte ao armazenamento de contêiner com Volumes Compartilhados do Cluster (CSV), Espaços de Armazenamento Diretos (S2D), Mapeamento Global de SMB](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Formato de arquivo de configuração de máquina virtual (atualizado)**. Um outro arquivo (.vmgs) foi adicionado a máquinas virtuais com uma versão de configuração 8.2 e superior. VMGS significa o estado de convidado da VM e é um novo arquivo interno que inclui o estado do dispositivo que já fazia parte do arquivo de estado de tempo de execução da VM.

## Segurança e garantia

A **Criptografia de rede** permite criptografar rapidamente os segmentos de rede na infraestrutura de rede definida pelo software a fim de atender às necessidades de segurança e conformidade.

O **Serviço Guardião de Host (HGS)** como uma VM protegida está habilitado. Antes desta versão, a recomendação era implantar um cluster físico de três nós. Embora isso garanta que o ambiente de HGS não seja comprometido por um administrador, frequentemente o custo era alto.

Agora há suporte para **Linux como VM protegida**.

Para obter mais informações, consulte [Visão geral de malha e VMs protegidas](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms).

**Vulnerabilidade SMBLoris** Um problema, conhecido como "SMBLoris", que pode resultar em negação de serviço, foi resolvido.

## Armazenamento

**Réplica de armazenamento**: a proteção de recuperação de desastres adicionada por Réplica de armazenamento no Windows Server 2016 agora é expandida para incluir:
- **Failover de teste**: a opção para montar o armazenamento de destino agora é possível por meio do recurso de failover de teste. Você pode montar um instantâneo do armazenamento replicado em nós de destino temporariamente para fins de teste ou backup.  Para obter mais informações, consulte [Perguntas Frequentes sobre a Réplica de armazenamento](https://aka.ms/srfaq). 
- **Suporte do Projeto Honolulu**: o suporte para gerenciamento gráfico de replicação de servidor para servidor agora está disponível no projeto Honolulu. Isso elimina a necessidade de usar o PowerShell para gerenciar uma carga de trabalho de proteção contra desastres comuns.

**SMB**: 
- **SMB1 e remoção de autenticação de convidado**: o Windows Server, versão 1709, não instala mais o cliente SMB1 e o servidor por padrão. Além disso, a capacidade de autenticar como um convidado no SMB2 e posterior está desativada por padrão. Para obter mais informações, consulte [o SMBv1 não é instalado por padrão no Windows 10, versão 1709, e no Windows Server, versão 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Segurança e compatibilidade de SMB2/SMB3**: foram adicionadas mais opções de compatibilidade e segurança de aplicativo, incluindo a capacidade de desabilitar os bloqueios em SMB2+ para aplicativos herdados, bem como exigir assinatura ou criptografia com base em conexão de um cliente. Para obter mais informações, consulte a ajuda do módulo SMBShare PowerShell.

**Eliminação de duplicação de dados**: 
- **Eliminação da duplicação de dados agora oferece suporte a ReFS**: você não deve escolher entre as vantagens de um sistema de arquivos moderno com ReFS e a eliminação da duplicação de dados: agora, você pode habilitar a eliminação da duplicação de dados, na qual você pode habilitar ReFS. Aumente a eficiência do armazenamento em mais de 95% com ReFS.
- **API de DataPort para entrada/saída otimizada para volumes com eliminação de duplicação**: os desenvolvedores agora podem aproveitar o conhecimento que a Eliminação da duplicação de dados tem sobre como armazenar dados de modo eficaz para mover os dados entre volumes, servidores e clusters de forma eficiente.

## RDS (Serviços de Área de Trabalho Remota)

**RDS é integrado ao Azure AD**, portanto, os clientes podem aproveitar as políticas de Acesso condicional, Autenticação multifator, Autenticação integrada a outros aplicativos SaaS usando o Azure AD e muito mais. Para obter mais informações, consulte [Integrar o Azure AD Domain Services à implantação do RDS](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).

>[!TIP]
>Para ver uma prévia de outras alterações incríveis do RDS, consulte [Serviços de área de trabalho remota: atualizações e inovações futuras](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/20/first-look-at-updates-coming-to-remote-desktop-services/)

## Rede

Suporte para **Roteamento de malha do Docker**. Malha de roteamento de ingresso faz parte do [modo nuvem](https://docs.docker.com/engine/swarm/), a solução de organização nativa do Docker para contêineres. Para obter mais informações, consulte [Malha de roteamento do Docker com Windows Server versão 1709](https://blogs.technet.microsoft.com/virtualization/2017/09/26/dockers-ingress-routing-mesh-available-with-windows-server-version-1709/).

**Novos recursos para Docker** estão disponíveis. Para obter mais informações, consulte [Novidades do Docker com o Windows Server 1709](https://blog.docker.com/2017/09/docker-windows-server-1709/).

**Rede do Windows em paridade com Linux para Kubernetes**: o Windows agora está no mesmo nível que o Linux em termos de rede. Os clientes podem implantar sistemas operacionais mistos, clusters de Kubernetes em qualquer ambiente incluindo o Azure, no local e em pilhas de nuvem de terceiros com os mesmos primitivos e topologias de rede compatíveis com o Linux sem precisar de qualquer solução alternativa ou extensões do switch.

**Pilha de rede principal**: diversos recursos da pilha principal da rede são aprimorados. Para obter mais informações sobre esses recursos, consulte [Principais recursos da pilha de rede na Atualização do Windows 10 para Criadores](https://blogs.technet.microsoft.com/networking/2017/07/13/core-network-stack-features-in-the-creators-update-for-windows-10/).
- **TCP Fast Open (TFO)**: o suporte para TFO foi adicionado a fim de otimizar o processo de handshake de três vias de TCP. O TFO estabelece um cookie TFO seguro na primeira conexão usando um handshake de três vias.  As conexões subsequentes ao mesmo servidor usam o cookie TFO em vez de um handshake de três vias para se conectar sem afetar o tempo de ida e volta.
- **CUBIC**: implementação nativa experimental do Windows do CUBIC, um algoritmo de controle de congestionamento TCP, está disponível. Os seguintes comandos habilitam ou desabilitam o CUBIC, respectivamente.

    ```
    netsh int tcp set supplemental template=internet congestionprovider=cubic
    netsh int tcp set supplemental template=internet congestionprovider=compound
    ```

- **Receber sintonia automática de janela**: a lógica de sintonia automática de TCP calcula o parâmetro "janela de recepção" de uma conexão TCP.  As conexões de alta velocidade e/ou demoradas precisam desse algoritmo para alcançar as características de bom desempenho.  Nesta versão, o algoritmo é modificado para usar uma função degrau para conversão no valor máximo de janela de recepção para uma determinada conexão.
- **API de estatísticas de TCP**: uma nova API é apresentada chamada de SIO_TCP_INFO.  SIO_TCP_INFO permite que os desenvolvedores consultem informações sobre conexões TCP individuais usando a opção de soquete.
- **IPv6**: existem diversas melhorias no IPv6 nesta versão.
    - Suporte para **RFC 6106**: RFC 6106 que permite a configuração de DNS por meio de anúncio de rotas (RAs). Você pode usar o seguinte comando para habilitar ou desabilitar o suporte de RFC 6106:

    ```
    netsh int ipv6 set interface <ifindex> rabaseddnsconfig=<enabled | disabled>
    ```

    - **Rótulos de fluxo**: a partir da Atualização para Criadores, os pacotes TCP e UDP de saída por IPv6 tem este campo definido como um hash quíntuplo (IP Src, IP Dst, Porta Src, Porta Dst).  Isso melhora a eficiência dos datacenters somente com IPv6 para balanceamento de carga os classificação de fluxo. Para habilitar flowlabels:

    ```
    netsh int ipv6 set flowlabel=[disabled|enabled] (enabled by default)
    netsh int ipv6 set global flowlabel=<enabled | disabled>
    ```

    - **ISATAP e 6to4**: como uma etapa para a futura reprovação, a Atualização para Criadores desabilitará essas tecnologias por padrão.
- **Detecção de gateway inativo (DGD)**: o algoritmo de DGD faz a transição automática de conexões por outro gateway quando o atual não está acessível. Nesta versão, o algoritmo é melhorado para testar periodicamente o ambiente de rede.
- O [Test-NetConnection](https://technet.microsoft.com/itpro/powershell/windows/nettcpip/test-netconnection) é um cmdlet nativo do Windows PowerShell que executa diversos diagnósticos de rede.  Nesta versão, aprimoraram o cmdlet para fornecer informações detalhadas sobre a seleção de rota, bem como a seleção do endereço de origem.

**Rede definida pelo software**

- A **Criptografia de rede virtual** é um novo recurso que oferece a capacidade de criptografar o tráfego de rede virtual entre Máquinas virtuais que se comunicam entre si em sub-redes marcadas com "Criptografia habilitada". Esse recurso usa o Protocolo DTLS na sub-rede virtual para criptografar os pacotes.  O DTLS oferece proteção contra interceptações, falsificação e falsificação por qualquer pessoa com acesso à rede física.
 
**Windows 10 VPN**

- **Túneis de infraestrutura de pré-logon**. Por padrão, a VPN do Windows 10 não cria automaticamente Túneis de infraestrutura quando os usuários não estão conectados ao computador ou dispositivo. Você pode configurar a VPN do Windows 10 para criar automaticamente Túneis de infraestrutura de pré-login usando o recurso de encapsulamento do dispositivo (pré-login) no perfil da VPN.
- **Gerenciamento de computadores e dispositivos remotos**.  Você pode gerenciar clientes VPN do Windows 10 ao configurar o recurso de Encapsulamento do dispositivo (pré-login) no perfil da VPN. Além disso, você deve configurar a conexão da VPN para registrar dinamicamente os endereços IP atribuídos à interface da VPN com serviços internos de DNS.
- **Especificar gateways de pré-login**. Você pode especificar Gateways de pré-login com o recurso de Encapsulamento do dispositivo (pré-login) no perfil da VPN, combinado com filtros de tráfego para controlar quais sistemas de gerenciamento na rede corporativa estão acessíveis pelo túnel de dispositivo.

