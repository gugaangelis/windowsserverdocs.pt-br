---
title: Implantar o Acesso Remoto em uma Empresa
description: Este tópico fornece uma introdução ao cenário do DirectAccess no Windows Server 2016 para a empresa.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4781df0a-158b-4562-b8f5-32b27615a4f8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ab337da85387be8c7d960315bb28644fa3a8a93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367504"
---
# <a name="deploy-remote-access-in-an-enterprise"></a>Implantar o Acesso Remoto em uma Empresa

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece uma introdução ao cenário do DirectAccess para a Empresa.  
  
  
> [!IMPORTANT]  
> Para implantar o DirectAccess usando este guia, você deve usar um servidor DirectAccess que esteja executando o Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Antes de iniciar a implantação, consulte a lista de configurações sem suporte, de problemas conhecidos e de pré-requisitos  
  
-   [Configurações do DirectAccess sem suporte](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-unsupported-configurations)  
  
-   [Problemas conhecidos do DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-known-issues)  
  
-   [Pré-requisitos para a implantação de pré-requisitos do DirectAccess](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/prerequisites-for-deploying-directaccess)  
  
## <a name="BKMK_OVER"></a>Descrição do cenário  
O acesso remoto inclui uma série de recursos corporativos, incluindo diversos servidores de acesso remoto em uma carga de cluster balanceada com o NLB (Balanceamento de Carga da Rede) do Windows ou com um balanceador externo de carga, além de instalação de uma implantação multissite com servidores de acesso remoto situados em locais geograficamente dispersos e implantação do DirectAccess com autenticação de cliente de dois fatores usando OTP (senha de uso único).  
  
## <a name="in-this-scenario"></a>Neste cenário  
Cada cenário corporativo é descrito em um documento que inclui instruções de planejamento e implantação. Para obter mais informações, consulte:  
  
-   [Implantar o acesso remoto em um cluster](cluster/Deploy-Remote-Access-In-Cluster.md)  
  
-   [Implantar vários servidores de acesso remoto em uma implantação multissite](multisite/Deploy-Multiple-Remote-Access-Servers-in-a-Multisite-Deployment.md)  
  
-   [Implantar o acesso remoto com autenticação OTP](otp/Deploy-RA-OTP.md)  
  
-   [Implantar o acesso remoto em um ambiente de várias florestas](multi-forest/Deploy-Remote-Access-in-a-Multi-Forest-Environment.md)  
  
## <a name="BKMK_APP"></a>Aplicativos práticos  
Os cenários corporativos do acesso remoto fornecem:  
  
-   **Maior disponibilidade**. Implantar vários servidores de acesso remoto em um cluster fornece escalabilidade e aumenta a capacidade de taxa de transferência e número de usuários. O balanceamento de carga do cluster fornece alta disponibilidade. Se um servidor no cluster falhar, os usuários remotos podem continuar acessando a rede corporativa interna por meio de um servidor diferente no cluster. O failover é transparente quando os clientes se conectam ao cluster utilizando um endereço IP virtual (VIP).  
  
-   **Facilidade de gerenciamento**. Uma implantação de cluster ou multissite pode ser configurada e gerenciada como uma entidade única usando o console de gerenciamento de acesso remoto em execução em um dos servidores de cluster. Além disso, uma implantação multissite permite que os administradores alinhem a implantação de Acesso Remoto a sites do Active Directory fornecendo a arquitetura simplificada. As configurações compartilhadas podem ser facilmente definidas entre servidores de cluster ou em todos os servidores de ponto de entrada multissite. As configurações de Acesso Remoto podem ser gerenciadas a partir de qualquer um dos servidores no cluster ou na implantação, ou remotamente utilizando as RSAT (Ferramentas de Administração do Servidor Remoto). Além disso, todo o cluster ou implantação multissite podem ser monitorados a partir de um único console de Gerenciamento de Acesso Remoto.  
  
-   **Eficiência de custo**. Uma implantação multissite de acesso remoto permite que as empresas implantem servidores de acesso remoto em vários sites correspondentes a locais de clientes. Isso fornece uma experiência de acesso previsível para clientes remotos independentemente de local e reduz os custos e largura de banda da intranet ao rotear o tráfego do cliente na Internet para o servidor de Acesso Remoto mais próximo.  
  
-   **Segurança**. A implantação de uma autenticação de cliente forte com uma OTP (senha de uso único) em vez da senha de Active Directory padrão aumenta a segurança.  
  
## <a name="BKMK_NEW"></a>Funções e recursos incluídos neste cenário  
A tabela a seguir lista funções e recursos utilizados no cenário corporativo:  
  
|Função/recurso|Como este cenário tem suporte|  
|---------|-----------------|  
|Função servidor de Acesso Remoto:|A função é instalada e desinstalada pelo console Gerenciador do Servidor. Essa função engloba o DirectAccess, que era anteriormente um recurso no Windows Server 2008 R2 e Serviços de Roteamento e Acesso Remoto que eram anteriormente um serviço de função sob a função de servidor de Serviços de Acesso e Política de Rede (NPAS). A função Acesso Remoto consiste em dois componentes:<br /><br />1.  O DirectAccess e o serviço de roteamento e acesso remoto (RRAS) VPN-DirectAccess e VPN são gerenciados juntos no console de gerenciamento de acesso remoto.<br />2.  Roteamento RRAS-os recursos de roteamento RRAS são gerenciados no console de roteamento e acesso remoto herdado.<br /><br />A Função Servidor de Acesso Remoto depende dos seguintes recursos de servidor:<br /><br />-Serviços de Informações da Internet (IIS)-esse recurso é necessário para configurar o servidor de local de rede e a investigação da Web padrão.<br />-O recurso de Console de Gerenciamento de Política de Grupo-recurso é exigido pelo DirectAccess para criar e gerenciar os objetos de Política de Grupo (GPOs) no Active Directory e deve ser instalado como um recurso necessário para a função de servidor.|  
|Recurso Ferramentas de Gerenciamento de Acesso Remoto|Este recurso é instalado da seguinte maneira:<br /><br />-Ele é instalado por padrão em um servidor de acesso remoto quando a função de acesso remoto é instalada e dá suporte à interface do usuário do console de gerenciamento remoto.<br />-Ele pode ser instalado opcionalmente em um servidor que não está executando a função de servidor de acesso remoto. Neste caso, ele é usado para gerenciamento remoto de um computador de Acesso Remoto que executa o DirectAccess e VPN.<br /><br />O recurso de Ferramentas de Gerenciamento de Acesso Remoto consiste em:<br /><br />1.  GUI de Acesso Remoto e Ferramentas de Linha de Comando<br />2.  Módulo de Acesso Remoto para o Windows PowerShell<br /><br />As dependências incluem:<br /><br />1.  Console de gerenciamento de política de grupo<br />2.  Kit de Administração do Gerenciador de Conexões RAS (CMAK)<br />3.  Windows PowerShell 3.0<br />4.  Ferramentas e Infraestrutura de Gerenciamento Gráfico|  
|NLB do Windows|Este recurso permite o balanceamento de carga de diversos servidores de Acesso Remoto.|  
  

  


