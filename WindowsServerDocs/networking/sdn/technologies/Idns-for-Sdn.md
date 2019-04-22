---
title: Serviço DNS interno (iDNS) para SDN
description: Este tópico explica como você pode fornecer serviços DNS para suas cargas de trabalho de locatário hospedado usando o DNS interno (iDNS), que é integrado com a rede definida pelo Software no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4d4ae5ee5f5600d86349ca26b7acbdb284b45bac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824077"
---
# <a name="internal-dns-service-idns-for-sdn"></a>Serviço DNS interno (iDNS) para SDN

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Se você trabalha para um provedor de serviços de nuvem \(CSP\) ou empresa que esteja planejando implantar rede definida pelo Software \(SDN\) no Windows Server 2016, você pode fornecer serviços DNS para suas cargas de trabalho de locatário hospedado usando o DNS interno \(iDNS\), que é integrado com SDN.

Máquinas virtuais hospedadas \(VMs\) e aplicativos exigem DNS para se comunicar dentro de suas próprias redes e recursos externos na Internet. Com iDNS, você pode fornecer locatários com serviços de resolução de nome DNS para seu espaço de nome local, isolado e recursos de Internet.

Porque o serviço de iDNS não está acessível a partir de redes virtuais de locatário, diferente de por meio do proxy iDNS, o servidor não é vulnerável a atividades mal-intencionadas em redes de locatário.

**Principais recursos**

A seguir estão os principais recursos IDNs.

- Fornece que serviços de resolução para o locatário de nomes do DNS compartilhados cargas de trabalho
- Serviço DNS autoritativo para resolução de nome e o registro DNS dentro do espaço de nome do locatário
- Serviço de Recursive DNS para resolução de nomes de Internet de VMs do locatário.
- Se desejar, você pode configurar a hospedagem simultânea de nomes de malha e locatário
- Uma solução econômica de DNS - locatários não precisa implantar sua própria infraestrutura de DNS
- Alta disponibilidade com a integração do Active Directory, que é necessária.

Além desses recursos, se você estiver preocupado em manter seu AD integrado abertas de servidores DNS para a Internet, você pode implantar servidores de iDNS atrás de outro resolvedor recursivo na rede de perímetro.

Como iDNS é um servidor centralizado para todas as consultas DNS, um CSP ou Enterprise pode também implementar firewalls DNS de locatário, aplicar filtros, detectar atividades mal-intencionadas e auditar as transações em um local central

## <a name="idns-infrastructure"></a>Infraestrutura de iDNS
A infra-estrutura de iDNS inclui servidores de iDNS e iDNS proxy.

### <a name="idns-servers"></a>Servidores de iDNS
iDNS inclui um conjunto de servidores DNS que hospedam dados específicos de locatário, como registros de recursos da VM.

servidores de iDNS são os servidores autoritativos para suas zonas DNS internos e também atuam como um resolvedor para nomes públicos do locatário quando as VMs ao tentar se conectar a recursos externos.

Todos os nomes de host para VMs em redes virtuais são armazenados como registros de recursos DNS na mesma zona. Por exemplo, se você implantar iDNS para uma zona chamada Contoso. local, os registros de recurso DNS para as VMs nessa rede são armazenados na zona da Contoso. local.

VM totalmente qualificados nomes de domínio de locatário \(FQDNs\) consistem em nome do computador e a cadeia de caracteres de sufixo DNS para a rede Virtual, no formato GUID. Por exemplo, se você tiver um locatário VM denominada locatário1 que está na contoso rede Virtual, local, o FQDN da VM é locatário1. *vn-guid*. contoso. local, onde *vn guid* é a cadeia de caracteres de sufixo DNS para a rede Virtual.

>[!NOTE]
>Se você for um administrador de malha, você pode usar sua infraestrutura de DNS corporativo ou de CSP como servidores de iDNS em vez de implantar novos servidores DNS especificamente para usam como iDNS servidores. Se você implantar novos servidores IDNs ou usar sua infraestrutura existente, iDNS depende do Active Directory para fornecer alta disponibilidade. Os servidores de iDNS, portanto, devem ser integrados ao Active Directory.

### <a name="idns-proxy"></a>Proxy de iDNS
proxy de iDNS é um serviço do Windows que é executado em todos os hosts e que encaminha locatário o tráfego de DNS da rede Virtual para os servidor de iDNS.

A ilustração a seguir mostra os caminhos de tráfego DNS de redes virtuais de locatário por meio do proxy iDNS para os servidor de iDNS e a Internet.

![Infraestrutura de iDNS](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Como implantar iDNS
Quando você implantar a SDN no Windows Server 2016 por meio de scripts, iDNS é incluído automaticamente em sua implantação.

Para obter mais informações, consulte estes tópicos.

- [Implantar uma infraestrutura de rede definida por Software usando scripts](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Noções básicas sobre as etapas de implantação de iDNS
Você pode usar esta seção para obter uma compreensão de como os iDNS está instalado e configurado quando você implanta SDN usando scripts.

A seguir está um resumo das etapas necessárias para implantar os iDNS.

>[!NOTE]
>Se você tiver implantado o SDN por meio de scripts, você não precisará realizar qualquer uma dessas etapas. As etapas são fornecidas para obter informações e apenas para fins de solução de problemas.

### <a name="step-1-deploy-dns"></a>Etapa 1: DNS de implantação
Você pode implantar um servidor DNS usando o exemplo de comando do Windows PowerShell.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Etapa 2: Configurar informações de iDNS no controlador de rede
Este segmento de script é uma chamada REST que é feita pelo administrador do controlador de rede, informando-o sobre a configuração da zona de iDNS - como o endereço IP do iDNSServer e a zona que é usada para hospedar os nomes de iDNS. 

```
    Url: https://<url>/networking/v1/iDnsServer/configuration
Method: PUT
{
      "properties": {
        "connections": [
          {
            "managementAddresses": [
              "10.0.0.9"
            ],
            "credential": {
              "resourceRef": "/credentials/iDnsServer-Credentials"
            },
            "credentialType": "usernamePassword"
          }
        ],
        "zone": "contoso.local"
      }
    }
```

>[!NOTE]
>Este é um trecho da seção **ConfigureIDns configuração** em SDNExpress.ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definida por Software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Etapa 3: Configurar os serviço de Proxy de iDNS
O serviço de Proxy de iDNS é executado em cada um dos hosts do Hyper-V, fornecendo a ponte entre as redes virtuais de locatários e a rede física em que os servidores de iDNS estão localizados. As seguintes chaves do registro devem ser criadas em todos os hosts do Hyper-V.


**Porta do DNS:** Porta fixa 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "Port"
- ValueData = 53
- ValueType = "Dword"
       

**Porta de Proxy DNS:** Porta fixa 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**DNS IP:** Endereço IP fixo configurado na interface de rede, caso o locatário opta por usar o serviço de iDNS

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "String"

        
**Endereço MAC:** Endereço de controle de acesso de mídia do servidor DNS

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = "aa-bb-cc-aa-bb-cc"
- ValueType = "String"

**Endereço do servidor IDNS:** Lista de servidores de iDNS separados por uma vírgula.

- Chave do Registro: HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "Forwarders"
- ValueData = "10.0.0.9"
- ValueType = "String"



>[!NOTE]
>Este é um trecho da seção **ConfigureIDnsProxy configuração** em SDNExpress.ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definida por Software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Etapa 4: Reinicie o serviço de agente de Host de controlador de rede
Você pode usar o seguinte comando do Windows PowerShell para reiniciar o serviço de agente de Host de controlador de rede.
    
    Restart-Service nchostagent -Force
    
Para obter mais informações, consulte [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Habilitar regras de firewall para o serviço de proxy DNS
Você pode usar o seguinte comando do Windows PowerShell para criar uma regra de firewall que permite exceções para o proxy para se comunicar com a VM e o servidor de iDNS.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Para obter mais informações, consulte [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Validar os serviço de iDNS
Para validar os iDNS serviço, você deve implantar uma carga de trabalho de locatário de exemplo.

Para obter mais informações, consulte [criar uma VM e conectar-se a uma rede Virtual do locatário ou VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Se você quiser que o locatário VM para usar o serviço de iDNS, você deve deixar a configuração de servidor DNS de interfaces de rede VM em branco e permitem que as interfaces usar DHCP. 

Depois que a VM com tal um adaptador de rede é iniciada, ele recebe automaticamente uma configuração que permite que a VM que usam iDNS e a VM começa imediatamente a executar a resolução de nome usando o serviço de iDNS.

Se você configurar o locatário VM para usar o serviço de iDNS, deixando as informações de servidor DNS e servidor DNS alternativo de interface de rede em branco, o controlador de rede fornece a VM com um endereço IP e executa um registro de nome DNS em nome da VM com os servidor de iDNS . 

Controlador de rede também informa o proxy de iDNS sobre a VM e os detalhes necessários para executar a resolução de nome para a VM. 

Quando a VM inicia uma consulta DNS, o proxy atua como um encaminhador da consulta da rede Virtual para o serviço de iDNS. 

O proxy DNS também garante que as consultas VM de locatário sejam isoladas. Se o servidor de iDNS é autoritativo para a consulta, o servidor de iDNS responde com uma resposta autoritativa. Se o servidor de iDNS não está autorizado para a consulta, ele executa uma recursão DNS para resolver nomes de Internet.

>[!NOTE]
>Essas informações estão incluídas na seção **AttachToVirtualNetwork configuração** em SDNExpressTenant.ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definida por Software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

