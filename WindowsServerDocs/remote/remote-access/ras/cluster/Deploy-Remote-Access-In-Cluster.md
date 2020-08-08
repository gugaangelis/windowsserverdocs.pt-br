---
title: Implantar o acesso remoto em um cluster
description: Este tópico faz parte do guia implantar o acesso remoto em um cluster no Windows Server 2016.
manager: dougkim
ms.topic: article
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c1d0a9490a2b2ef6efbd15f39ec95f1b482efd5e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944166"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Implantar o acesso remoto em um cluster

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O Windows Server 2016 e o Windows Server 2012 combinam o DirectAccess e o serviço de acesso remoto \( RAS \) VPN em uma única função de acesso remoto. Você pode implantar o acesso remoto em vários cenários empresariais. Esta visão geral fornece uma introdução ao cenário empresarial para implantar vários servidores de acesso remoto em uma carga de cluster balanceada com balanceamento de carga de rede do Windows \( NLB \) ou com um ELB de balanceador de carga externo \( , como \) F5 Big \- IP.

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrição do cenário
Uma implantação de cluster reúne vários servidores de acesso remoto em uma única unidade, que atua como um único ponto de contato para computadores cliente remotos que se conectam via DirectAccess ou VPN à rede corporativa interna usando o \( endereço VIP de IP virtual externo \) do cluster de acesso remoto.  O tráfego para o cluster tem balanceamento de carga usando o NLB do Windows ou com um balanceador de carga externo \( , como F5 Big \- IP \) .

## <a name="prerequisites"></a>Pré-requisitos
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:

-   Balanceamento de carga padrão por meio do Windows NLB.

-   Há suporte para balanceadores de carga externos.

-   O modo unicast é o padrão e o modo recomendado para NLB.

-   Não há suporte para alteração de políticas fora do console de gerenciamento do DirectAccess ou dos cmdlets do PowerShell.

-   Quando o NLB ou um balanceador de carga externo é usado, o prefixo IPHTTPS não pode ser alterado para algo diferente de \/ 59.

-   Os nós balanceados para carga devem estar na mesma sub-rede do IPv4.

-   Em implantações do ELB, se o gerenciamento for necessário, os clientes do DirectAccess não poderão usar o &nbsp; Teredo. Somente o IPHTTPS pode ser usado para \- comunicação de ponta a \- ponta.

-   Verifique se todos os \/ hotfixes NLB ELB conhecidos estão instalados.

-   Não há suporte para ISATAP na rede corporativa. Se você estiver usando ISATAP, remova-o e use o IPv6 nativo.

## <a name="in-this-scenario"></a>Neste cenário
O cenário de implantação de cluster inclui diversas etapas:

1.  [Implante um servidor VPN AlwaysOn com opções avançadas](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Um único servidor de acesso remoto com configurações avançadas deve ser implantado antes de configurar uma implantação de cluster.

2.  [Planejar uma implantação de cluster de acesso remoto](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Para criar um cluster a partir de uma implantação de servidor único, é necessária uma série de etapas adicionais, incluindo a preparação de certificados para a implantação do cluster.

3.  [Configure um cluster de acesso remoto](configure/Configure-a-Remote-Access-Cluster.md). Isso consiste em várias etapas de configuração, incluindo a preparação do servidor único para o NLB do Windows ou o balanceador externo de carga, a preparação de servidores adicionais para ingressar no cluster e a habilitação do balanceamento de carga.

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas
Reunir diversos servidores em um cluster de servidor fornece:

-   Escalabilidade. Um único servidor de acesso remoto fornece um nível limitado de confiabilidade do servidor e desempenho escalonável. Ao agrupar os recursos de dois ou mais servidores em um único cluster, você aumenta a capacidade para número de usuários e taxa de transferência.

-   Alta disponibilidade. Um cluster fornece alta disponibilidade para \- acesso AlwaysOn. Se um servidor no cluster falhar, os usuários remotos podem continuar acessando a rede corporativa por meio de um servidor diferente no cluster. Todos os servidores no cluster têm o mesmo conjunto de endereços VIP de IP virtual de cluster \( \) , mantendo, ao mesmo tempo, um endereço IP dedicado e exclusivo para cada servidor.

-   Facilidade \- de \- Gerenciamento. Um cluster permite o gerenciamento de vários servidores como uma única entidade. As configurações compartilhadas podem ser facilmente definidas no servidor de cluster. As configurações de acesso remoto podem ser gerenciadas de qualquer um dos servidores no cluster ou remotamente usando o Ferramentas de Administração de Servidor Remoto \( rsat \) . Além disso, todo o cluster pode ser monitorado a partir de um único console de Gerenciamento de Acesso Remoto.

## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário
A tabela a seguir lista funções e recursos necessários para o cenário:

|Recurso de função \/|Como este cenário tem suporte|
|---------|-----------------|
|Função Acesso Remoto|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Ele abrange o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2, e serviços de roteamento e acesso remoto \( RRAS \) , que era um serviço de função na função de servidor de serviços de acesso e diretiva de rede \( \) . A função Acesso Remoto consiste em dois componentes:<p>-Always On VPN e roteamento e serviços de acesso remoto \( RRAS \) VPN-DirectAccess e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />-Os recursos de roteamento RRAS de roteamento RRAS são gerenciados no console de roteamento e acesso remoto herdado.<p>As dependências são as seguintes:<p>-Serviços de Informações da Internet \( \) servidor Web do IIS-esse recurso é necessário para configurar o servidor de local de rede e a investigação da Web padrão.<br />-Banco de dados interno do Windows-usado para contabilização local no servidor de acesso remoto.|
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<p>-Ele é instalado por padrão em um servidor de acesso remoto quando a função de acesso remoto é instalada e dá suporte à interface do usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não está executando a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<p>O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<p>-GUI de acesso remoto e ferramentas de linha de comando<br />-Módulo de acesso remoto para Windows PowerShell<p>As dependências incluem:<p>-Console de Gerenciamento de Política de Grupo<br />-Kit de administração do Gerenciador de conexões RAS \( CMAK\)<br />-Windows PowerShell 3,0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|
|Network Load Balancing|Este recurso fornece o balanceamento de carga em um cluster utilizando o NLB do Windows.|

## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware
Os requisitos de hardware para este cenário incluem o seguinte:

-   Pelo menos dois computadores que atendem aos requisitos de hardware do Windows Server 2012.

-   Para o cenário de Load Balancer externo, é necessário um hardware dedicado, \( ou seja, F5 BigIP \) .

-   Para testar o cenário, você deve ter pelo menos um computador executando o Windows 10 configurado como um cliente VPN Always On.

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
Há diversos requisitos para este cenário:

-   Requisitos de software para implantação de servidor único. Para obter mais informações, consulte [implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Um único acesso remoto).

-   Além dos requisitos de software para um único servidor, há vários \- requisitos específicos do cluster:

    -   Em cada servidor de cluster, o \- nome da entidade do certificado HTTPS de IP deve corresponder ao endereço connectto. Uma implantação de cluster dá suporte a uma combinação de certificados curinga e não \- curinga em servidores de cluster.

    -   Se o servidor de local de rede estiver instalado no servidor de Acesso Remoto, em cada servidor de cluster, o certificado do servidor de local de rede deve ter o mesmo nome de assunto. Além disso, o nome do certificado do servidor de local de rede não deve ter o mesmo nome de nenhum servidor na implantação do DirectAccess.

    -   Os \- certificados IP HTTPS e servidor de local de rede devem ser emitidos usando o mesmo método com o qual o certificado para o único servidor foi emitido. Por exemplo, se o servidor único usar uma AC de autoridade de certificação pública \( \) , todos os servidores no cluster deverão ter um certificado emitido por uma autoridade de certificação pública. Ou, se o servidor único usar um \- certificado autoassinado para \- https de IP, todos os servidores no cluster deverão fazer a mesma configuração.

    -   O prefixo de IPv6 atribuído a computadores cliente do DirectAccess em clusters de servidores deve ser de 59 bits. Se a VPN estiver habilitada, o prefixo da VPN também deve ser de 59 bits.

## <a name="known-issues"></a><a name="KnownIssues"></a>Problemas conhecidos
Os problemas a seguir são conhecidos quando se configura um cenário de cluster:

-   Depois de configurar o DirectAccess em uma \- implantação somente IPv4 com um único adaptador de rede e, depois do DNS64 padrão \( , o endereço IPv6 que contém ": 3333::" \) é configurado automaticamente no adaptador de rede, a tentativa de habilitar o balanceamento de carga \- por meio do console de gerenciamento de acesso remoto faz com que o usuário forneça um DIP IPv6. Se for fornecido um DIP IPv6, ocorrerá falha na configuração depois de clicar em **Confirmar** com o erro: O parâmetro está incorreto.

    Para resolver o problema:

    1.  Baixe os scripts de backup e restauração em [Fazer Backup e Restauração da Configuração de Acesso Remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).

    2.  Faça backup de seus GPOs de acesso remoto usando o backup de script baixado \-RemoteAccess.ps1

    3.  Tente habilitar o balanceamento de carga até a etapa em que ocorre a falha. Na caixa de diálogo habilitar balanceamento de carga, expanda a área detalhes, clique com o botão direito do mouse \- na área detalhes e clique em **copiar script**.

    4.  Abra o Bloco de Notas e cole o conteúdo da área de transferência. Por exemplo:

        ```
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose
        ```

    5.  Feche todas as caixas de diálogo de Acesso Remoto abertas e feche o console de Gerenciamento de Acesso Remoto.

    6.  Edite o texto colado e remova os endereços IPv6. Por exemplo:

        ```
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19 /255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21 /255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose
        ```

    7.  Em uma janela elevada do PowerShell, execute o comando da etapa anterior.

    8.  Se o cmdlet falhar enquanto está em execução \( não devido a valores de entrada incorretos \) , execute o comando restore \-RemoteAccess.ps1 e siga as instruções para garantir que a integridade da configuração original seja mantida.

    9. Agora é possível abrir o console de Gerenciamento de Acesso Remoto novamente.
