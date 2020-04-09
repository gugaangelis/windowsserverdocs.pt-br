---
title: Implantar o acesso remoto em um cluster
description: Este tópico faz parte do guia implantar o acesso remoto em um cluster no Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7b9ab144c19b81d2229ea0618aebc9a94b9fdccf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861459"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Implantar o acesso remoto em um cluster

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O Windows Server 2016 e o Windows Server 2012 combinam o DirectAccess e o serviço de acesso remoto \(RAS\) VPN em uma única função de acesso remoto. Você pode implantar o acesso remoto em vários cenários empresariais. Esta visão geral fornece uma introdução ao cenário empresarial para a implantação de vários servidores de acesso remoto em uma carga de cluster balanceada com o balanceamento de carga de rede do Windows \(NLB\) ou com um balanceador de carga externo \(ELB\), como F5 Big\-IP.  

## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrição do cenário  
Uma implantação de cluster reúne vários servidores de acesso remoto em uma única unidade, que atua como um único ponto de contato para computadores cliente remotos que se conectam via DirectAccess ou VPN à rede corporativa interna usando o endereço IP virtual externo \(VIP\) endereços do cluster de acesso remoto.  O tráfego para o cluster tem balanceamento de carga usando o NLB do Windows ou com um balanceador de carga externo \(como F5 Big\-IP\).  

## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}  
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:  

-   Balanceamento de carga padrão por meio do Windows NLB.  

-   Há suporte para balanceadores de carga externos.  

-   O modo unicast é o padrão e o modo recomendado para NLB.  

-   Não há suporte para alteração de políticas fora do console de gerenciamento do DirectAccess ou dos cmdlets do PowerShell.  

-   Quando o NLB ou um balanceador de carga externo é usado, o prefixo IPHTTPS não pode ser alterado para algo diferente de \/59.  

-   Os nós balanceados para carga devem estar na mesma sub-rede do IPv4.  

-   Em implantações do ELB, se o gerenciamento for necessário, os clientes do DirectAccess não poderão usar&nbsp;Teredo. Somente o IPHTTPS pode ser usado para\-end para\-a comunicação final.  

-   Verifique se todos os hotfixes do NLB\/ELB estão instalados.  

-   Não há suporte para ISATAP na rede corporativa. Se você estiver usando ISATAP, remova-o e use o IPv6 nativo.  

## <a name="in-this-scenario"></a>Neste cenário  
O cenário de implantação de cluster inclui diversas etapas:  

1.  [Implante um servidor VPN AlwaysOn com opções avançadas](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Um único servidor de acesso remoto com configurações avançadas deve ser implantado antes de configurar uma implantação de cluster.  

2.  [Planejar uma implantação de cluster de acesso remoto](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Para criar um cluster a partir de uma implantação de servidor único, é necessária uma série de etapas adicionais, incluindo a preparação de certificados para a implantação do cluster.  

3.  [Configure um cluster de acesso remoto](configure/Configure-a-Remote-Access-Cluster.md). Isso consiste em várias etapas de configuração, incluindo a preparação do servidor único para o NLB do Windows ou o balanceador externo de carga, a preparação de servidores adicionais para ingressar no cluster e a habilitação do balanceamento de carga.  

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicativos práticos  
Reunir diversos servidores em um cluster de servidor fornece:  

-   Escalabilidade. Um único servidor de acesso remoto fornece um nível limitado de confiabilidade do servidor e desempenho escalonável. Ao agrupar os recursos de dois ou mais servidores em um único cluster, você aumenta a capacidade para número de usuários e taxa de transferência.  

-   Alta disponibilidade. Um cluster fornece alta disponibilidade para sempre\-no acesso. Se um servidor no cluster falhar, os usuários remotos podem continuar acessando a rede corporativa por meio de um servidor diferente no cluster. Todos os servidores no cluster têm o mesmo conjunto de IP virtual de cluster \(endereços de\) VIP, mantendo, ao mesmo tempo, um endereço IP dedicado e exclusivo para cada servidor.  

-   Facilite\-de gerenciamento de\-. Um cluster permite o gerenciamento de vários servidores como uma única entidade. As configurações compartilhadas podem ser facilmente definidas no servidor de cluster. As configurações de acesso remoto podem ser gerenciadas de qualquer um dos servidores no cluster ou remotamente usando o Ferramentas de Administração de Servidor Remoto \(RSAT\). Além disso, todo o cluster pode ser monitorado a partir de um único console de Gerenciamento de Acesso Remoto.  

## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista funções e recursos necessários para o cenário:  

|Recurso de\/de função|Como este cenário tem suporte|  
|---------|-----------------|  
|Função Acesso Remoto|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Ele abrange o DirectAccess, que anteriormente era um recurso no Windows Server 2008 R2, e serviços de roteamento e acesso remoto \(\)RRAS, que era anteriormente um serviço de função na diretiva de rede e serviços de acesso \(função de servidor NPAS\). A função Acesso Remoto consiste em dois componentes:<p>-Always On VPN e serviços de roteamento e acesso remoto \(RRAS\) VPN-DirectAccess e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />-Os recursos de roteamento RRAS de roteamento RRAS são gerenciados no console de roteamento e acesso remoto herdado.<p>As dependências são as seguintes:<p>-Serviços de Informações da Internet \(IIS\) servidor Web-esse recurso é necessário para configurar o servidor de local de rede e a investigação da Web padrão.<br />-Banco de dados interno do Windows-usado para contabilização local no servidor de acesso remoto.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<p>-Ele é instalado por padrão em um servidor de acesso remoto quando a função de acesso remoto é instalada e dá suporte à interface do usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não está executando a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<p>O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<p>-GUI de acesso remoto e ferramentas de linha de comando<br />-Módulo de acesso remoto para Windows PowerShell<p>As dependências incluem:<p>-Console de Gerenciamento de Política de Grupo<br />-Kit de administração do Gerenciador de conexões RAS \(CMAK\)<br />-Windows PowerShell 3,0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|  
|Balanceamento de carga de rede|Este recurso fornece o balanceamento de carga em um cluster utilizando o NLB do Windows.|  

## <a name="hardware-requirements"></a><a name="BKMK_HARD"></a>Requisitos de hardware  
Os requisitos de hardware para este cenário incluem o seguinte:  

-   Pelo menos dois computadores que atendem aos requisitos de hardware do Windows Server 2012.  

-   Para o cenário de Load Balancer externo, é necessário um hardware dedicado \(por exemplo, F5 BigIP\).  

-   Para testar o cenário, você deve ter pelo menos um computador executando o Windows 10 configurado como um cliente VPN Always On.   

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software  
Há diversos requisitos para este cenário:  

-   Requisitos de software para implantação de servidor único. Para obter mais informações, consulte [implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Um único acesso remoto).  

-   Além dos requisitos de software para um único servidor, há um número de clusters\-requisitos específicos:  

    -   Em cada servidor de cluster, o nome da entidade do certificado IP\-HTTPS deve corresponder ao endereço connectto. Uma implantação de cluster dá suporte a uma combinação de certificados curinga e não\-curinga em servidores de cluster.  

    -   Se o servidor de local de rede estiver instalado no servidor de Acesso Remoto, em cada servidor de cluster, o certificado do servidor de local de rede deve ter o mesmo nome de assunto. Além disso, o nome do certificado do servidor de local de rede não deve ter o mesmo nome de nenhum servidor na implantação do DirectAccess.  

    -   Os certificados IP\-HTTPS e de servidor de local de rede devem ser emitidos usando o mesmo método com o qual o certificado para o único servidor foi emitido. Por exemplo, se o servidor único usar uma autoridade de certificação pública \(AC\), todos os servidores no cluster deverão ter um certificado emitido por uma autoridade de certificação pública. Ou, se o servidor único usar um certificado auto\-assinado para IP\-HTTPS, todos os servidores no cluster deverão fazer a mesma.  

    -   O prefixo de IPv6 atribuído a computadores cliente do DirectAccess em clusters de servidores deve ser de 59 bits. Se a VPN estiver habilitada, o prefixo da VPN também deve ser de 59 bits.  

## <a name="known-issues"></a><a name="KnownIssues"></a>Problemas conhecidos  
Os problemas a seguir são conhecidos quando se configura um cenário de cluster:  

-   Depois de configurar o DirectAccess em um IPv4\-apenas implantação com um único adaptador de rede e, depois do DNS64 padrão \(o endereço IPv6 que contém ": 3333::"\) é automaticamente configurado no adaptador de rede, a tentativa de habilitar o balanceamento de carga\-por meio do console de gerenciamento de acesso remoto faz com que o usuário forneça um DIP IPv6. Se for fornecido um DIP IPv6, ocorrerá falha na configuração depois de clicar em **Confirmar** com o erro: O parâmetro está incorreto.  

    Para resolver esse problema:  

    1.  Baixe os scripts de backup e restauração em [Fazer Backup e Restauração da Configuração de Acesso Remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  

    2.  Faça backup de seus GPOs de acesso remoto usando o script de backup baixado\-RemoteAccess. ps1  

    3.  Tente habilitar o balanceamento de carga até a etapa em que ocorre a falha. Na caixa de diálogo habilitar balanceamento de carga, expanda a área de detalhes, à direita\-clique na área de detalhes e clique em **copiar script**.  

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

    8.  Se o cmdlet falhar enquanto estiver em execução \(não devido a valores de entrada incorretos\), execute o comando Restore\-RemoteAccess. ps1 e siga as instruções para garantir que a integridade da configuração original seja mantida.  

    9. Agora é possível abrir o console de Gerenciamento de Acesso Remoto novamente.  
