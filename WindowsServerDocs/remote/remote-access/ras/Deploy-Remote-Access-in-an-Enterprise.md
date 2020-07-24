---
title: Implantar o Acesso Remoto em uma Empresa
description: Este tópico fornece uma introdução ao cenário do DirectAccess no Windows Server 2016 para a empresa.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4781df0a-158b-4562-b8f5-32b27615a4f8
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0cf216cb785d01ed08bb3a4490b25d4c4549b1c4
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965588"
---
# <a name="deploy-remote-access-in-an-enterprise"></a>Implantar o Acesso Remoto em uma Empresa

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece uma introdução ao cenário do DirectAccess para a Empresa.  
  
  
> [!IMPORTANT]  
> Para implantar o DirectAccess usando este guia, você deve usar um servidor DirectAccess que esteja executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Antes de iniciar a implantação, consulte a lista de configurações sem suporte, de problemas conhecidos e de pré-requisitos  
  
-   [Configurações do DirectAccess sem suporte](../directaccess/directaccess-unsupported-configurations.md)  
  
-   [Problemas conhecidos do DirectAccess](../directaccess/directaccess-known-issues.md)  
  
-   [Pré-requisitos para a implantação de pré-requisitos do DirectAccess](../directaccess/prerequisites-for-deploying-directaccess.md)  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Descrição do cenário  
O acesso remoto inclui uma série de recursos corporativos, incluindo diversos servidores de acesso remoto em uma carga de cluster balanceada com o NLB (Balanceamento de Carga da Rede) do Windows ou com um balanceador externo de carga, além de instalação de uma implantação multissite com servidores de acesso remoto situados em locais geograficamente dispersos e implantação do DirectAccess com autenticação de cliente de dois fatores usando OTP (senha de uso único).  
  
## <a name="in-this-scenario"></a>Neste cenário  
Cada cenário corporativo é descrito em um documento que inclui instruções de planejamento e implantação. Para obter mais informações, consulte:  
  
-   [Implantar o acesso remoto em um cluster](cluster/Deploy-Remote-Access-In-Cluster.md)  
  
-   [Implantar vários servidores de acesso remoto em uma implantação multissite](multisite/Deploy-Multiple-Remote-Access-Servers-in-a-Multisite-Deployment.md)  
  
-   [Implantar o acesso remoto com autenticação OTP](otp/Deploy-RA-OTP.md)  
  
-   [Implantar o acesso remoto em um ambiente de várias florestas](multi-forest/Deploy-Remote-Access-in-a-Multi-Forest-Environment.md)  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas  
Os cenários corporativos do acesso remoto fornecem:  
  
-   **Maior disponibilidade**. Implantar vários servidores de acesso remoto em um cluster fornece escalabilidade e aumenta a capacidade de taxa de transferência e número de usuários. O balanceamento de carga do cluster fornece alta disponibilidade. Se um servidor no cluster falhar, os usuários remotos podem continuar acessando a rede corporativa interna por meio de um servidor diferente no cluster. O failover é transparente quando os clientes se conectam ao cluster utilizando um endereço IP virtual (VIP).  
  
-   **Facilidade de gerenciamento**. Uma implantação de cluster ou multissite pode ser configurada e gerenciada como uma entidade única usando o console de gerenciamento de acesso remoto em execução em um dos servidores de cluster. Além disso, uma implantação multissite permite que os administradores alinhem a implantação de Acesso Remoto a sites do Active Directory fornecendo a arquitetura simplificada. As configurações compartilhadas podem ser facilmente definidas entre servidores de cluster ou em todos os servidores de ponto de entrada multissite. As configurações de Acesso Remoto podem ser gerenciadas a partir de qualquer um dos servidores no cluster ou na implantação, ou remotamente utilizando as RSAT (Ferramentas de Administração do Servidor Remoto). Além disso, todo o cluster ou implantação multissite podem ser monitorados a partir de um único console de Gerenciamento de Acesso Remoto.  
  
-   **Eficiência de custo**. Uma implantação multissite de acesso remoto permite que as empresas implantem servidores de acesso remoto em vários sites correspondentes a locais de clientes. Isso fornece uma experiência de acesso previsível para clientes remotos independentemente de local e reduz os custos e largura de banda da intranet ao rotear o tráfego do cliente na Internet para o servidor de Acesso Remoto mais próximo.  
  
-   **Segurança**. A implantação de uma autenticação de cliente forte com uma OTP (senha de uso único) em vez da senha de Active Directory padrão aumenta a segurança.  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista funções e recursos utilizados no cenário corporativo:  
  
|Função/recurso|Como este cenário tem suporte|  
|---------|-----------------|  
|Função servidor de Acesso Remoto:|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Essa função engloba o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2 e Serviços de Roteamento e Acesso Remoto que eram anteriormente um serviço de função sob a função de servidor de Serviços de Acesso e Política de Rede (NPAS). A função Acesso Remoto consiste em dois componentes:<p>1. DirectAccess e roteamento e serviços de acesso remoto (RRAS) VPN-DirectAccess e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />2. roteamento RRAS-os recursos de roteamento RRAS são gerenciados no console de roteamento e acesso remoto herdado.<p>A Função Servidor de Acesso Remoto depende dos seguintes recursos de servidor:<p>-Serviços de Informações da Internet (IIS)-esse recurso é necessário para configurar o servidor de local de rede e a investigação da Web padrão.<br />-O recurso de Console de Gerenciamento de Política de Grupo-recurso é exigido pelo DirectAccess para criar e gerenciar os objetos de Política de Grupo (GPOs) no Active Directory e deve ser instalado como um recurso necessário para a função de servidor.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<p>-Ele é instalado por padrão em um servidor de acesso remoto quando a função de acesso remoto é instalada e dá suporte à interface do usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não está executando a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<p>O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<p>1. GUI de acesso remoto e ferramentas de linha de comando<br />2. módulo de acesso remoto para Windows PowerShell<p>As dependências incluem:<p>1. Console de Gerenciamento de Política de Grupo<br />2. kit de administração do Gerenciador de conexões RAS (CMAK)<br />3. Windows PowerShell 3,0<br />4. ferramentas e infraestrutura de gerenciamento gráfico|  
|NLB do Windows|Este recurso permite o balanceamento de carga de diversos servidores de Acesso Remoto.|  
  

  
