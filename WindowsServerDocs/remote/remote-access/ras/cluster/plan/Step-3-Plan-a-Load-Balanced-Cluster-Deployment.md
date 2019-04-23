---
title: Etapa 3 planejar uma implantação de Cluster de balanceamento de carga
description: Este tópico faz parte do guia de implantação de acesso remoto em um Cluster no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7540c17b-81de-47de-a04f-3247afa26f70
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 12485ddb9cbb70766c018e8f99caa91cfa6ee9da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861327"
---
# <a name="step-3-plan-a-load-balanced-cluster-deployment"></a>Etapa 3 planejar uma implantação de Cluster de balanceamento de carga

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A próxima etapa é planejar a configuração de balanceamento de carga e a implantação de cluster.  
  
|Tarefa|Descrição|  
|----|--------|  
|3.1 planejar balanceamento de carga|Decida se deseja usar o Windows Network NLB Balanceamento de carga () ou um balanceador de carga externo (ELB).|  
|3.2 plano IP-HTTPS|Se um certificado autoassinado não for usado, o servidor de acesso remoto precisa de um certificado SSL em cada servidor no cluster, para autenticar conexões IP-HTTPS.|  
|3.3 o plano para conexões de cliente VPN|Observe os requisitos para conexões de cliente VPN.|  
|3.4 planejar o servidor de local de rede|Se o site de servidor de local de rede está hospedado no servidor de acesso remoto e um certificado autoassinado não é usado, certifique-se de que cada servidor no cluster tem um certificado de servidor para autenticar a conexão para o site.|  
  
## <a name="bkmk_2_1_Plan_LB"></a>3.1 planejar balanceamento de carga  
Acesso remoto pode ser implantado em um único servidor ou em um cluster de servidores de acesso remoto. Tráfego para o cluster pode ser a carga balanceada, para fornecer alta disponibilidade e escalabilidade para os clientes DirectAccess. Há duas opções de balanceamento de carga:  
  
-   **Windows NLB**-NLB do Windows é um recurso do Windows server. Para usá-lo, você não exigir hardware adicional porque todos os servidores no cluster são responsáveis por gerenciar a carga de tráfego. O NLB do Windows dá suporte a um máximo de oito servidores em um cluster de acesso remoto.  
  
-   **Balanceador de carga externo**-usando um balanceador externo de carga requer hardware externo para gerenciar a carga do tráfego entre os servidores de cluster de acesso remoto. Além disso, usando um balanceador externo de carga dá suporte a um máximo de 32 servidores de acesso remoto em um cluster. Alguns pontos para ter em mente ao configurar o balanceamento de carga externo são:  
  
    -   O administrador deve assegurar que os IPs virtuais configurado por meio do Assistente de balanceamento de carga do acesso remoto são usados nos balanceadores de carga externo (como o sistema do Gerenciador de tráfego do F5 Big-Ip Local). Quando o balanceamento de carga externo estiver habilitado, os endereços IP nas interfaces internas e externas serão promovidos a endereços IP virtuais e precisam ser inseridas nos balanceadores de carga. Isso é feito para que o administrador não precisa alterar a entrada DNS para o nome público da implantação do cluster. Além disso, os pontos de extremidade de túnel IPsec são derivados de IPs de servidor. Se o administrador fornece IPs virtuais separadas, o cliente não será capaz de se conectar ao servidor. Veja o exemplo de configuração do DirectAccess com balanceamento de carga externo 3.1.1 exemplo de configuração do balanceador externo de carga.  
  
    -   Muitos balanceadores de carga externo (incluindo F5) não têm suporte para balanceamento de carga de 6to4 e ISATAP. Se o servidor de acesso remoto é um roteador ISATAP, a função ISATAP deve ser movida para um computador diferente. Além disso, quando a função ISATAP estiver em um computador diferente, os servidores do DirectAccess devem ter conectividade IPv6 nativa com o roteador ISATAP. Observe que essa conectividade deve estar presente antes de configurar o DirectAccess.  
  
    -   Para o balanceamento de carga externo, se o Teredo deve ser usado, todos os servidores de acesso remoto devem ter dois endereços IPv4 públicos consecutivos como endereços IP dedicados. Os IPs virtuais do cluster também deve ter dois endereços IPv4 públicos consecutivos. Isso não é verdadeiro para NLB do Windows em que apenas os IPs virtuais do cluster deve ter dois endereços IPv4 públicos consecutivos. Se não for usado o Teredo, dois endereços IP consecutivos não são necessários.  
  
    -   O administrador pode alternar de NLB do Windows para o balanceador de carga externo e vice-versa. Observe que o administrador não pode mudar do balanceador de carga externo para NLB do Windows se ele tiver mais de 8 servidores na implantação do balanceador externo de carga.  
  
### <a name="ELBConfigEx"></a>3.1.1 exemplo de configuração de Balanceador de carga externo  
Esta seção descreve as etapas de configuração para habilitar um balanceador externo de carga em uma nova implantação de acesso remoto. Ao usar um balanceador de carga externo, o cluster de acesso remoto pode se parecer com a figura a seguir, onde os servidores de acesso remoto estão conectados à rede corporativa por meio de um balanceador de carga na rede interna e à Internet através de um balanceador de carga conectado à rede externa:  
  
![Exemplo de configuração de Balanceador de carga externo](../../../../media/Step-3-Plan-a-Load-Balanced-Cluster-Deployment/ELBDiagram-URA_Enterprise_NLB-.png)  
  
##### <a name="planning-information"></a>Informações de planejamento  
  
1.  Externos VIPs (IPs que o cliente usará para se conectar ao acesso remoto) foram decidiu ser 131.107.0.102, 131.107.0.103  
  
2.  Balanceador na rede externa self-IPs - 131.107.0.245 (Internet), 131.107.1.245 de carga  
  
    É a rede de perímetro (também conhecida como zona desmilitarizada e DMZ) entre o balanceador de carga na rede externa e o servidor de acesso remoto.  
  
3.  Endereços IP para o servidor de acesso remoto na rede de perímetro - 131.107.1.102, 131.107.1.103  
  
4.  Endereços IP para o servidor de acesso remoto na rede (ou seja, entre o servidor de acesso remoto e o balanceador de carga na rede interna) - ELB 30.11.1.101, 2006:2005:11:1::101  
  
5.  Carregar balanceador na rede interna self-IPs - 30.11.1.245 2006:2005:11:1::245 (ELB) 30.1.1.245 2006:2005:1:1::245 (Corpnet)  
  
6.  Internos VIPs (endereços IP usados para investigação da web do cliente e servidor de local de rede, se instalado em servidores de acesso remoto) foram decidiu ser 30.1.1.10, 2006:2005:1:1::10  
  
##### <a name="steps"></a>Etapas  
  
1.  Configure o adaptador de rede externo do servidor de acesso remoto (que está conectado à rede de perímetro) com endereços 131.107.0.102, 131.107.0.103. Essa etapa é necessária para a configuração do DirectAccess detectar os pontos de extremidade de túnel IPsec corretos.  
  
2.  Configure o adaptador de rede interno do servidor de acesso remoto (que está conectado à rede ELB) com os endereços IP do servidor de local de rede/web-investigação (30.1.1.10 2006:2005:1:1::10). Essa etapa é necessária para permitir que os clientes acessem o IP de investigação da web, portanto, o Assistente de conectividade de rede corretamente indica o status de conexão para o DirectAccess. Esta etapa também permite acesso ao servidor de local de rede, se ele estiver configurado no servidor DirectAccess.  
  
    > [!NOTE]  
    > Certifique-se de que o controlador de domínio é acessível a partir do servidor de acesso remoto com essa configuração.  
  
3.  Configure um único servidor do DirectAccess no servidor de acesso remoto.  
  
4.  Habilite na configuração do DirectAccess de balanceamento de carga externa. Usar 131.107.1.102 como o endereço IP dedicado (DIP) externo (131.107.1.103 será selecionada automaticamente), use 30.11.1.101, 2006:2005:11:1::101 como as quedas internas.  
  
5.  Configure o externo VIPs (IPs virtuais) no balanceador externo de carga com endereços 131.107.0.102 e 131.107.0.103. Além disso, configure o internos VIPs no balanceador externo de carga com endereços 30.1.1.10 e 2006:2005:1:1::10.  
  
6.  O servidor de acesso remoto agora será configurado com os endereços IP planejados, e os endereços IP internos e externos para o cluster serão configurados de acordo com os endereços IP planejados.  
  
## <a name="bkmk_2_2_NLB"></a>3.2 plano IP-HTTPS  
  
1.  **Requisitos de certificado**-durante a implantação de um único servidor de acesso remoto que você optou por usar um certificado IP-HTTPS emitido por uma CA (autoridade de certificação pública ou interna) ou um certificado autoassinado. Para a implantação de cluster, você deve usar o mesmo tipo de certificado em cada membro do cluster de acesso remoto. Ou seja, se você usou um certificado emitido por uma CA pública (recomendada), você deve instalar um certificado emitido por uma CA pública em cada membro do cluster. O nome da entidade do novo certificado deve ser idêntico ao nome do assunto do certificado IP-HTTPS atualmente usado em sua implantação. Observe que se você estiver usando certificados autoassinados eles serão configurados automaticamente em cada servidor durante a implantação de cluster.  
  
2.  **Requisitos de prefixo**-acesso remoto permite que o balanceamento de carga de tráfego com base em SSL e o tráfego do DirectAccess. Para balancear a carga todos IPv6 com base em tráfego do DirectAccess, acesso remoto deve examinar o encapsulamento IPv4 para todas as tecnologias de transição. Como o tráfego IP-HTTPS é criptografado, examinar o conteúdo do encapsulamento IPv4 não é possível. Para habilitar o tráfego de IP-HTTPS para ser com balanceamento de carga, você deve alocar um prefixo IPv6 largo o suficiente para que um prefixo IPv6 /64 diferente pode ser atribuído a cada membro do cluster. Você pode configurar um máximo de 32 servidores em um cluster com balanceamento de carga; Portanto, você deve especificar um /59 prefixo. Esse prefixo deve ser roteável para o endereço IPv6 interno do cluster de acesso remoto e é configurado no Assistente de configuração do servidor de acesso remoto.  
  
    > [!NOTE]  
    > Os requisitos de prefixo são relevantes apenas em uma rede interna do IPv6 habilitado (somente IPv6 ou IPV4 + IPv6). Em uma rede corporativa somente IPv4, o prefixo do cliente é configurado automaticamente e o administrador não pode alterá-lo.  
  
## <a name="BKMK_3.3"></a>3.3 o plano para conexões de cliente VPN  
Há várias considerações para conexões de cliente VPN:  
  
-   Tráfego do cliente VPN não pode ser balanceado se endereços de cliente VPN são alocados usando DHCP. Um pool de endereços estáticos é necessário.  
  
-   RRAS pode ser habilitado em um cluster com balanceamento de carga que foi implantado para o DirectAccess apenas, usando **habilitar VPN** no painel de tarefas do console do gerenciamento de acesso remoto.  
  
-   Todas as alterações VPN concluídas no console de roteamento e gerenciamento de acesso remoto (rrasmgmt.msc) precisará ser replicado manualmente em todos os servidores de acesso remoto no cluster.  
  
-   Para habilitar o tráfego do cliente VPN IPv6 com balanceamento de carga, você deve especificar um prefixo IPv6 de 59 bits.  
  
## <a name="BKMK_nls"></a>3.4 planejar o servidor de local de rede  
Se você estiver executando o site de servidor de local de rede no servidor de acesso remoto único, durante a implantação você optou por usar um certificado emitido por uma CA (autoridade de certificação interna) ou um certificado autoassinado.  Observe o seguinte:  
  
1.  Cada membro do cluster de acesso remoto deve ter um certificado para o servidor de local de rede que corresponde à entrada DNS para o site de servidor de local de rede.  
  
2.  O certificado para cada servidor de cluster deve ser emitido da mesma maneira como o certificado único acesso remoto certificado do servidor atual rede local server. Por exemplo, se você usou um certificado emitido por uma CA interna, você deve instalar um certificado emitido pela autoridade de certificação interna em cada membro do cluster.  
  
3.  Se você tiver usado um certificado autoassinado, um certificado autoassinado será configurado automaticamente para cada servidor durante a implantação de cluster.  
  
4.  O nome da entidade do certificado não deve ser idêntico ao nome de qualquer um dos servidores na implantação do acesso remoto.  
