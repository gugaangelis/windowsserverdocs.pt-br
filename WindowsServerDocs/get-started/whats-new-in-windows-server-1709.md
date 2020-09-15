---
title: Novidades no Windows Server, versão 1709
description: Quais são os novos recursos de computação, identidade, gerenciamento, automação, rede, segurança, armazenamento.
ms.topic: article
ms.author: lizross
author: eross-msft
manager: mtillman
ms.localizationpriority: medium
ms.date: 06/03/2019
ms.openlocfilehash: 2202658da6d89d3a289e0fd1e6df07e9ba4e4544
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621996"
---
# <a name="whats-new-in-windows-server-version-1709"></a>Novidades no Windows Server, versão 1709

> Aplica-se a: Windows Server (Canal semestral)

<img src=../media/landing-icons/new.png style='float:left; padding:.5em;' alt=Icon showing a newspaper>&nbsp;Para saber mais sobre os recursos mais recentes do Windows, consulte [Novidades no Windows Server](whats-new-in-windows-server.md). O conteúdo desta seção descreve as novidades e as alterações no Windows Server, versão 1709. Os novos recursos e alterações listados aqui são os que têm maior probabilidade de ter um impacto maior ao trabalhar com esta versão. Consulte também [Windows Server, versão 1709](https://cloudblogs.microsoft.com/windowsserver/2017/08/24/sneak-peek-1-windows-server-version-1709/).

> [!IMPORTANT]
> O Windows Server versão 1709 está sem suporte desde 9 de abril de 2019.

## <a name="new-cadence-of-releases"></a>Nova cadência de versões

Desta versão em diante, você tem duas opções para receber atualizações de recursos do Windows Server:
- **LTSC (Canal de Manutenção em Longo Prazo)** : mantém a continuidade, com cinco anos de suporte base e cinco anos de suporte estendido. Você tem a opção de atualizar para a próxima versão do LTSC a cada dois ou três anos, com o mesmo suporte dos últimos 20 anos.
- **SAC (Canal Semestral)** : esse é um dos benefícios do Software Assurance e é totalmente compatível na produção. A diferença é que ele oferece suporte por 18 meses e há uma nova versão a cada seis meses.

Os canais de lançamento estão resumidos na tabela a seguir.

| Descrição | Canal Semestral | Canal de Manutenção em Longo Prazo |
| ------------- |--| -- |
| Cadência de versão | Duas vezes por ano (primavera e outono) | A cada 2-3 anos |
| Cronograma de suporte | Suporte base de produção de 18 meses | Suporte base de cinco anos + suporte estendido de cinco anos |
| Disponibilidade | Software Assurance ou Azure (hospedado em nuvem) | Todos os canais |
| Convenção de nomenclatura | Windows Server, versão AAMM | Windows Server AAAA |

Para obter mais informações, confira a [Comparação dos canais de manutenção](../get-started-19/servicing-channels-19.md).

## <a name="application-containers-and-micro-services"></a>Contêineres de aplicativo e microsserviços

- A imagem de contêiner do Server Core foi otimizada ainda mais para cenários de lift-and-shift, nos quais você pode migrar bases de código ou aplicativos em contêineres com poucas alterações, além de ser 60% menor.
- A imagem de contêiner do servidor Nano é aproximadamente 80% menor.
    - No Canal Semestral do Windows Server, o Nano Server como uma imagem de sistema operacional base do contêiner é reduzida de 390 MB para 80 MB.
- Contêineres do Linux com isolamento Hyper-V

Para obter mais informações, confira [Alterações ao Nano Server na próxima versão do Windows Server](./nano-in-semi-annual-channel.md) e [Windows Server, versão 1709 para desenvolvedores](https://cloudblogs.microsoft.com/windowsserver/2017/09/13/sneak-peek-3-windows-server-version-1709-for-developers/).

## <a name="modern-management"></a>Gerenciamento moderno

Confira o [Project Honolulu](../manage/windows-admin-center/overview.md) para obter uma experiência simplificada, integrada e segura a fim de ajudar os administradores do setor de TI a gerenciarem a solução de problemas, a configuração e os cenários de manutenção essenciais.  O Projeto Honolulu inclui a próxima geração de ferramenta com uma interface simplificada, integrada, segura e ampliável.
O projeto Honolulu inclui uma nova experiência de gerenciamento intuitiva para gerenciar computadores, servidores do Windows, clusters de failover, além da infraestrutura de hiperconvergência com base em Espaços de Armazenamento Diretos, reduzindo os custos operacionais.

## <a name="compute"></a>Computação

**Contêiner Nano e Contêiner do Server Core**: Primeiramente, essa versão é sobre impulsionar a inovação de aplicativo. O Servidor Nano ou Nano como host é preterido e substituído pelo contêiner Nano, que é o Nano funcionando como imagem de contêiner.

Para obter mais informações sobre contêineres, confira [Visão geral da rede de contêineres](../networking/sdn/technologies/containers/container-networking-overview.md).

**Server Core como um host de contêiner** (e infraestrutura), fornece mais flexibilidade, densidade e desempenho para aplicativos atuais sob um processo de modernização e marca os novos aplicativos desenvolvidos usando o modelo de nuvem.

A **Ordenação de Início de VM** também é melhorada com o reconhecimento de sistema operacional e de aplicativos, trazendo gatilhos avançados para quando uma VM é considerada como iniciada antes de iniciar a próxima.

O **Suporte de memória da classe de armazenamento para VMs** permite que os volumes de acesso direto formatados para NTFS sejam criados em DIMMs não voláteis e expostos às VMs Hyper-V. Isso permite que as VMs do Hyper-V aproveitem o desempenho de baixa latência de dispositivos de memória da classe de armazenamento.

A **Memória Persistente Virtualizada (vPMEM)** é habilitada ao criar um arquivo VHD (.vhdpmem) em um volume de acesso direto em um host, adicionando um Controlador vPMEM a uma VM, além de adicionar o dispositivo criado (.vhdpmem) a uma VM. O uso de arquivos vhdpmem nos volumes de acesso direto em um host para vPMEM proporciona flexibilidade de alocação e aproveita um modelo de gerenciamento conhecido para adicionar discos às VMs.

**Armazenamento de contêiner: volumes de dados persistentes nos volumes compartilhados do cluster (CSV)** . No Windows Server, versão 1709, bem como no Windows Server 2016 com as últimas atualizações, adicionamos suporte para que os contêineres acessem volumes de dados persistentes localizados em CSVs, incluindo os CSVs em Espaços de Armazenamento Diretos. Isso proporciona o acesso persistente do contêiner de aplicativo ao volume, independentemente de em qual nó de cluster a instância de contêiner está sendo executada. Para obter mais informações, confira [Suporte ao armazenamento de contêiner com Volumes Compartilhados do Cluster (CSV), Espaços de Armazenamento Diretos (S2D), Mapeamento Global de SMB](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering).

**Armazenamento de contêiner: volumes de dados persistentes com mapeamento global de SMB**. No Windows Server, versão 1709, adicionamos suporte para mapeamento de um compartilhamento de arquivo SMB para a letra da unidade em um contêiner; isso é chamado de mapeamento global de SMB. Essa unidade mapeada fica acessível para todos os usuários no servidor local, de modo que o contêiner de E/S no volume de dados pode passar da unidade montada para o compartilhamento de arquivo subjacente. Para obter mais informações, confira [Suporte ao armazenamento de contêiner com Volumes Compartilhados do Cluster (CSV), Espaços de Armazenamento Diretos (S2D), Mapeamento Global de SMB](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering).

**Formato de arquivo de configuração de máquina virtual (atualizado)** . Um outro arquivo (.vmgs) foi adicionado a máquinas virtuais com uma versão de configuração 8.2 e superior. VMGS significa o estado de convidado da VM e é um novo arquivo interno que inclui o estado do dispositivo que já fazia parte do arquivo de estado de runtime da VM.

## <a name="security-and-assurance"></a>Segurança e garantia

A **criptografia de rede** permite criptografar rapidamente os segmentos de rede na infraestrutura de rede definida pelo software a fim de atender às necessidades de segurança e de conformidade.

O **HGS (Serviço Guardião de Host)** como uma VM protegida está habilitado. Antes desta versão, a recomendação era implantar um cluster físico de três nós. Embora isso garanta que o ambiente de HGS não seja comprometido por um administrador, frequentemente o custo era alto.

Agora a opção de **Linux como uma VM protegida** é compatível.

Para obter mais informações, confira [Visão geral de malha e VMs protegidas](../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md).

**Vulnerabilidade SMBLoris** Um problema conhecido como "SMBLoris", que pode resultar em negação de serviço, foi resolvido.

## <a name="storage"></a>Armazenamento

**Réplica de Armazenamento**: A proteção de recuperação de desastres adicionada por Réplica de armazenamento no Windows Server 2016 foi agora expandida para incluir:
- **Failover de teste**: a opção para montar o armazenamento de destino agora é possível por meio do recurso de failover de teste. Você pode montar um instantâneo do armazenamento replicado em nós de destino temporariamente para fins de teste ou backup.  Para obter mais informações, consulte [Perguntas Frequentes sobre a Réplica de Armazenamento](https://aka.ms/srfaq).
- **Suporte ao Project Honolulu**: o suporte para gerenciamento gráfico de replicação de servidor para servidor agora está disponível no Project Honolulu. Isso elimina a necessidade de usar o PowerShell para gerenciar uma carga de trabalho de proteção contra desastres comuns.

**SMB**:
- **SMB1 e remoção de autenticação de convidado**: o Windows Server versão 1709 não instala mais o cliente SMB1 e o servidor por padrão. Além disso, a capacidade de autenticar como um convidado no SMB2 e posterior está desativada por padrão. Para obter mais informações, confira [o SMBv1 não é instalado por padrão no Windows 10 versão 1709 e no Windows Server versão 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server).

- **Compatibilidade e segurança de SMB2/SMB3**: foram adicionadas mais opções de compatibilidade e segurança do aplicativo, incluindo a capacidade de desabilitar os bloqueios em SMB2+ para aplicativos herdados, bem como exigir assinatura ou criptografia de um cliente para cada conexão. Para obter mais informações, confira a ajuda do módulo SMBShare do PowerShell.

**Eliminação de Duplicação de Dados**:
- **A eliminação de duplicação de dados agora dá suporte a ReFS**: você não precisa mais escolher entre as vantagens de um sistema de arquivos moderno com o ReFS e a eliminação de duplicação de dados: agora, você pode habilitar a eliminação de duplicação de dados, na qual você pode habilitar o ReFS. Aumente a eficiência do armazenamento em mais de 95% com o ReFS.
- **API de DataPort para entrada/saída otimizada para volumes com eliminação de duplicação**: os desenvolvedores agora podem aproveitar o conhecimento que a eliminação de duplicação de dados tem sobre como armazenar dados de modo eficaz para mover os dados entre volumes, servidores e clusters de forma eficiente.

## <a name="remote-desktop-services-rds"></a>RDS (Serviços de Área de Trabalho Remota)

**O RDS é integrado ao Azure AD**, portanto, os clientes podem aproveitar as políticas de Acesso condicional, Autenticação multifator, Autenticação integrada a outros aplicativos SaaS usando o Azure AD e muito mais. Para obter mais informações, confira [Integrar o Azure AD Domain Services à implantação do RDS](../remote/remote-desktop-services/rds-azure-adds.md).

>[!TIP]
>Para ver uma prévia de outras alterações incríveis do RDS, confira [Serviços de Área de Trabalho Remota: atualizações e inovações futuras](https://techcommunity.microsoft.com/t5/microsoft-security-and/first-look-at-updates-coming-to-remote-desktop-services/ba-p/250320)

## <a name="networking"></a>Rede

O **Roteamento de malha do Docker** é compatível. A malha de roteamento de ingresso faz parte do [modo Swarm](https://docs.docker.com/engine/swarm/), a solução de organização interna do Docker para contêineres. Para obter mais informações, confira [Malha de roteamento do Docker disponível com o Windows Server versão 1709](https://techcommunity.microsoft.com/t5/virtualization/bg-p/Virtualization).

**Novos recursos para o Docker** estão disponíveis. Para obter mais informações, confira [Novidades empolgantes do Docker com o Windows Server 1709](https://blog.docker.com/2017/09/docker-windows-server-1709/).

**Rede do Windows em paridade com o Linux para o Kubernetes**: O Windows agora está no mesmo nível do Linux em termos de rede. Os clientes podem implantar sistemas operacionais mistos, clusters de Kubernetes em qualquer ambiente incluindo o Azure, localmente e em pilhas de nuvem de terceiros com os mesmos primitivos e topologias de rede compatíveis com o Linux sem precisar de nenhuma solução alternativa nem extensões do switch.

**Pilha de rede principal**: diversos recursos da pilha principal da rede foram aprimorados. Para obter mais informações sobre esses recursos, confira [Recursos da pilha de rede principal na Atualização do Windows 10 para Criadores](https://techcommunity.microsoft.com/t5/networking-blog/bg-p/NetworkingBlog).
- **TFO (TCP Fast Open)** : o suporte para TFO foi adicionado a fim de otimizar o processo de handshake de três vias de TCP. O TFO estabelece um cookie TFO seguro na primeira conexão usando um handshake de três vias padrão.  As conexões subsequentes ao mesmo servidor usam o cookie TFO em vez de um handshake de três vias para se conectar sem afetar o tempo de ida e volta.
- **CUBIC**: uma implementação nativa experimental do Windows do CUBIC, um algoritmo de controle de congestionamento TCP, está disponível. Os comandos a seguir habilitam ou desabilitam o CUBIC, respectivamente.

    ```
    netsh int tcp set supplemental template=internet congestionprovider=cubic
    netsh int tcp set supplemental template=internet congestionprovider=compound
    ```

- **Ajuste automático da janela de recebimento**: a lógica de sintonia automática de TCP calcula o parâmetro "janela de recebimento" de uma conexão TCP.  As conexões de alta velocidade e/ou demoradas precisam desse algoritmo para alcançar características de bom desempenho.  Nesta versão, o algoritmo é modificado para usar uma função degrau para convergir no valor máximo de janela de recebimento para uma determinada conexão.
- **API de estatísticas de TCP**: uma nova API foi introduzida, chamada de SIO_TCP_INFO.  A SIO_TCP_INFO permite que os desenvolvedores consultem informações avançadas sobre conexões TCP individuais usando uma opção de soquete.
- **IPv6**: existem diversas melhorias no IPv6 nesta versão.
  - Suporte para **RFC 6106**: RFC 6106, que permite a configuração de DNS por meio de RAs (anúncios de rotas). Você pode usar o seguinte comando para habilitar ou desabilitar o suporte a RFC 6106:

    ```
    netsh int ipv6 set interface <ifindex> rabaseddnsconfig=<enabled | disabled>
    ```

  - **Rótulos de fluxo**: da Atualização para Criadores em diante, os pacotes TCP e UDP de saída por IPv6 tem este campo definido como um hash da tupla quíntupla (IP Src, IP Dst, Porta Src, Porta Dst).  Isso melhora a eficiência dos datacenters somente com IPv6 para balanceamento de carga ou classificação de fluxo. Para habilitar rótulos de fluxo:

    ```
    netsh int ipv6 set flowlabel=[disabled|enabled] (enabled by default)
    netsh int ipv6 set global flowlabel=<enabled | disabled>
    ```

  - **ISATAP e 6to4**: como uma etapa rumo à preterição futura, a Atualização para Criadores terá essas tecnologias desabilitadas por padrão.
- **DGD (Detecção de Gateway Inativo)** : o algoritmo de DGD faz a transição automática de conexões por outro gateway quando o atual não está acessível. Nesta versão, o algoritmo é melhorado para investigar novamente periodicamente o ambiente de rede.
- O [Test-NetConnection](/powershell/module/nettcpip/test-netconnection?view=win10-ps) é um cmdlet interno do Windows PowerShell que executa diversos diagnósticos de rede.  Nesta versão, aprimoramos o cmdlet para fornecer informações detalhadas sobre a seleção de rota, bem como a seleção do endereço de origem.

**Rede definida pelo software**

- A **Criptografia de Rede Virtual** é um novo recurso que oferece a capacidade de criptografar o tráfego de rede virtual entre Máquinas Virtuais que se comunicam entre si em sub-redes marcadas com Criptografia Habilitada. Esse recurso usa o Protocolo DTLS na sub-rede virtual para criptografar os pacotes.  O DTLS oferece proteção contra interceptações, adulteração e falsificação por qualquer pessoa com acesso à rede física.

**Windows 10 VPN**

- **Túneis de infraestrutura de pré-logon**. Por padrão, a VPN do Windows 10 não cria automaticamente Túneis de infraestrutura quando os usuários não estão conectados ao computador ou dispositivo. Você pode configurar a VPN do Windows 10 para criar automaticamente Túneis de infraestrutura de pré-logon usando o recurso de túnel de dispositivo (pré-logon) no perfil da VPN.
- **Gerenciamento de computadores e dispositivos remotos**.  Você pode gerenciar clientes VPN do Windows 10 ao configurar o recurso de túnel do dispositivo (pré-logon) no perfil da VPN. Além disso, você deve configurar a conexão VPN para registrar dinamicamente os endereços IP atribuídos à interface da VPN com serviços internos de DNS.
- **Especificar gateways de pré-logon**. Você pode especificar Gateways de pré-logon com o recurso de Túnel de dispositivo (pré-logon) no perfil da VPN, combinado com filtros de tráfego para controlar quais sistemas de gerenciamento na rede corporativa estão acessíveis pelo túnel de dispositivo.
