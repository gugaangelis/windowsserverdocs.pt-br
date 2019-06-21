---
title: Implantar o acesso remoto em um cluster
description: Este tópico faz parte do guia de implantação de acesso remoto em um Cluster no Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 853788f20c452391c802f0681fa23978b4892c6a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281227"
---
# <a name="deploy-remote-access-in-a-cluster"></a>Implantar o acesso remoto em um cluster

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Windows Server 2016 e Windows Server 2012 combinam o DirectAccess e o serviço de acesso remoto \(RAS\) VPN em uma única função de acesso remoto. Você pode implantar o acesso remoto em um número de cenários empresariais. Esta visão geral fornece uma introdução ao cenário corporativo para implantar diversos servidores de acesso remoto em uma carga de cluster balanceada com balanceamento de carga de rede do Windows \(NLB\) ou com um balanceador de carga externo \(ELB \), como F5 Big\-IP.  

## <a name="BKMK_OVER"></a>Descrição do cenário  
Implantação de um cluster reúne diversos servidores de acesso remoto em uma única unidade, que então atua como um ponto único de contato para computadores cliente remotos se conectam por meio do DirectAccess ou VPN à rede corporativa interna usando o IP virtual externo \(VIP\) endereço do cluster de acesso remoto.  Tráfego para o cluster é balanceada utilizando o NLB do Windows ou balanceador de carga externo \(como o F5 Big\-IP\).  

## <a name="prerequisites"></a>Pré-requisitos  
Antes de começar a implantar este cenário, examine esta lista de requisitos importantes:  

-   Balanceamento de carga padrão por meio do Windows NLB.  

-   Há suporte para balanceadores de carga externos.  

-   O modo unicast é o padrão e o modo recomendado para NLB.  

-   Não há suporte para alteração de políticas fora do console de gerenciamento do DirectAccess ou dos cmdlets do PowerShell.  

-   Quando é usado NLB ou um balanceador de carga externo, o prefixo IPHTTPS não pode ser alterado para algo diferente de \/59.  

-   Os nós balanceados para carga devem estar na mesma sub-rede do IPv4.  

-   Em implantações ELB, se gerenciar for necessária e, em seguida, os clientes DirectAccess não é possível usar&nbsp;Teredo. Apenas IPHTTPS pode ser usado para o final\-para\-terminar a comunicação.  

-   Certifique-se de NLB todos conhecido\/ELB hotfixes são instalados.  

-   Não há suporte para ISATAP na rede corporativa. Se você estiver usando ISATAP, remova-o e use o IPv6 nativo.  

## <a name="in-this-scenario"></a>Neste cenário  
O cenário de implantação de cluster inclui diversas etapas:  

1.  [Implantar um servidor VPN com opções avançadas AlwaysOn](../../vpn/always-on-vpn/deploy/always-on-vpn-adv-options.md). Um único servidor de acesso remoto com configurações avançadas deve ser implantado antes de configurar uma implantação de cluster.  

2.  [Planejar uma implantação de Cluster de acesso remoto](plan/Plan-a-Remote-Access-Cluster-Deployment.md). Para criar um cluster de uma implantação de servidor único de um número de mais etapas são necessárias, incluindo preparar certificados para a implantação de cluster.  

3.  [Configurar um Cluster de acesso remoto](configure/Configure-a-Remote-Access-Cluster.md). Isso consiste em um número de etapas de configuração, incluindo preparar o servidor único para NLB do Windows ou balanceador externo de carga, preparar servidores adicionais para ingressar no cluster e habilitar o balanceamento de carga.  

## <a name="BKMK_APP"></a>Aplicativos práticos  
Reunir diversos servidores em um cluster de servidor fornece:  

-   Escalabilidade. Um único servidor de acesso remoto fornece um nível limitado de confiabilidade de servidor e de desempenho escalável. Ao agrupar os recursos de dois ou mais servidores em um único cluster, você aumenta a capacidade para número de usuários e taxa de transferência.  

-   Alta disponibilidade. Um cluster fornece alta disponibilidade para sempre\-no acesso. Se um servidor no cluster falhar, os usuários remotos podem continuar acessando a rede corporativa por meio de um servidor diferente no cluster. Todos os servidores no cluster têm o mesmo conjunto de IP virtual do cluster \(VIP\) endereços, e ainda manter uma única, dedicado a endereços IP para cada servidor.  

-   Facilidade\-de\-gerenciamento. Um cluster permite o gerenciamento de vários servidores como uma única entidade. As configurações compartilhadas podem ser facilmente definidas no servidor de cluster. Configurações de acesso remoto podem ser gerenciadas do qualquer um dos servidores no cluster ou remotamente usando ferramentas de administração de servidor remoto \(RSAT\). Além disso, todo o cluster pode ser monitorado a partir de um único console de Gerenciamento de Acesso Remoto.  

## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista funções e recursos necessários para o cenário:  

|Função\/recurso|Como este cenário tem suporte|  
|---------|-----------------|  
|Função Acesso Remoto|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Ele engloba o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2 e o roteamento e serviços de acesso remoto \(RRAS\), que eram anteriormente um serviço de função sob a política de rede e serviços de acesso \(NPAS\) função de servidor. A função Acesso Remoto consiste em dois componentes:<br /><br />-Sempre em VPN e de roteamento e serviços de acesso remoto \(RRAS\) VPN do DirectAccess e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />-Recursos de roteamento de RRAS Roteamento-RRAS são gerenciados no console de roteamento e acesso remoto legado.<br /><br />As dependências são as seguintes:<br /><br />-Serviços de informações da Internet \(IIS\) servidor Web - esse recurso é necessário para configurar a rede local padrão e o servidor de investigação da web.<br />-Windows Database-Used interno para contabilidade local no servidor de acesso remoto.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<br /><br />– Ele é instalado por padrão em um servidor de acesso remoto quando a função acesso remoto está instalada e oferece suporte a interface de usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não executa a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<br /><br />O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<br /><br />-Ferramentas de linha de comando e GUI de acesso remoto<br />-Módulo de acesso remoto para o Windows PowerShell<br /><br />As dependências incluem:<br /><br />-Console de gerenciamento de diretiva de grupo<br />-Kit de administração do Gerenciador de Conexão de RAS \(CMAK\)<br />-   Windows PowerShell 3.0<br />-Infraestrutura e ferramentas de gerenciamento gráfico|  
|Balanceamento de Carga de Rede|Este recurso fornece o balanceamento de carga em um cluster utilizando o NLB do Windows.|  

## <a name="BKMK_HARD"></a>Requisitos de hardware  
Os requisitos de hardware para este cenário incluem o seguinte:  

-   Pelo menos dois computadores que atendem aos requisitos de hardware do Windows Server 2012.  

-   Para o cenário de Balanceador de carga externo, é necessário hardware dedicado \(ou seja, o BigIP F5\).  

-   Para testar o cenário, você deve ter pelo menos um computador executando o Windows 10, configurado como um cliente de VPN Always On.   

## <a name="BKMK_SOFT"></a>Requisitos de software  
Há diversos requisitos para este cenário:  

-   Requisitos de software para implantação de servidor único. Para obter mais informações, consulte [implantar um único servidor DirectAccess com configurações avançadas](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Uma única de acesso remoto).  

-   Além dos requisitos de software para um único servidor, há uma série de cluster\-requisitos específicos:  

    -   Em cada servidor de cluster IP\-certificado HTTPS nome da entidade deve corresponder ao endereço ConnectTo. Uma implantação de cluster suporta uma mistura de caractere curinga e não\-certificados curinga em servidores de cluster.  

    -   Se o servidor de local de rede estiver instalado no servidor de Acesso Remoto, em cada servidor de cluster, o certificado do servidor de local de rede deve ter o mesmo nome de assunto. Além disso, o nome do certificado do servidor de local de rede não deve ter o mesmo nome de nenhum servidor na implantação do DirectAccess.  

    -   IP\-certificados de servidor de local de rede e HTTPS devem ser emitidos usando o mesmo método com o qual o certificado para um único servidor foi emitido. Por exemplo, se o servidor único usar uma autoridade de certificação pública \(autoridade de certificação\) todos os servidores no cluster deverá ter um certificado emitido por uma CA pública. Ou se o servidor único utilizar um self\-certificado autoassinado para IP\-HTTPS e em seguida, todos os servidores no cluster deverão fazer o mesmo.  

    -   O prefixo de IPv6 atribuído a computadores cliente do DirectAccess em clusters de servidores deve ser de 59 bits. Se a VPN estiver habilitada, o prefixo da VPN também deve ser de 59 bits.  

## <a name="KnownIssues"></a>Problemas conhecidos  
Os problemas a seguir são conhecidos quando se configura um cenário de cluster:  

-   Depois de configurar o DirectAccess em um IPv4\-apenas a implantação com um único adaptador de rede e depois o DNS64 padrão \(o endereço IPv6 que contém ": 3333::"\) é automaticamente configurado no adaptador de rede, a tentativa de habilitar a carga\-balanceamento por meio do console de gerenciamento de acesso remoto faz com que um prompt para o usuário forneça um DIP IPv6. Se for fornecido um DIP IPv6, ocorrerá falha na configuração depois de clicar em **Confirmar** com o erro: O parâmetro está incorreto.  

    Para resolver esse problema:  

    1.  Baixe os scripts de backup e restauração em [Fazer Backup e Restauração da Configuração de Acesso Remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  

    2.  Fazer backup de seus GPOs de acesso remoto usando o Backup de script\-RemoteAccess.ps1  

    3.  Tente habilitar o balanceamento de carga até a etapa em que ocorre a falha. Na caixa de diálogo Habilitar balanceamento de carga, expanda a área de detalhes, à direita\-clique na área de detalhes e, em seguida, clique em **copiar Script**.  

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

    8.  Se o cmdlet falhar durante a execução \(não devido a valores de entrada incorretos\), execute o comando Restore\-RemoteAccess.ps1 e siga as instruções para certificar-se de que a integridade da sua configuração original é mantida .  

    9. Agora é possível abrir o console de Gerenciamento de Acesso Remoto novamente.  
