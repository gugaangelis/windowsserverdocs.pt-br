---
title: Serviço DNS interno (iDNS) para SDN
description: Este tópico explica como você pode fornecer serviços DNS para cargas de trabalho seu locatário hospedado usando DNS interno (iDNS), que estão integrados ao Software definido de rede no Windows Server 2016.
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
ms.openlocfilehash: 4bdb495b585cf60315dee60f9365e282877fdc1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="internal-dns-service-idns-for-sdn"></a>\(IDNS\) de serviço DNS interno para SDN

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Se você trabalha para um provedor de serviços de nuvem \(CSP\) ou Enterprise que pretende implantar \(SDN\) Software de rede definidos no Windows Server 2016, você pode fornecer serviços DNS para cargas de trabalho seu locatário hospedado usando DNS interno \(iDNS\), que estão integrados ao SDN.

Máquinas virtuais hospedadas \(VMs\) e aplicativos exigem DNS para se comunicar em suas próprias redes e com recursos externos na Internet. Com iDNS, você pode fornecer locatários com serviços de resolução de nome DNS para o espaço de nome isolado, locais e recursos de Internet.

Como o serviço de iDNS não é acessível a partir de locatário redes virtuais, diferente por meio do proxy iDNS, o servidor não é vulnerável a atividades mal-intencionadas em redes de locatário.

**Principais recursos**

A seguir estão os principais recursos iDNS.

- Fornece que serviços de resolução de locatário de nome do DNS compartilhados cargas de trabalho
- Autoritativo serviço DNS para resolução de nomes e registro DNS dentro do espaço de nome de locatário
- Serviço de Recursive DNS para resolução de nomes de Internet de locatário VMs.
- Se desejar, você pode configurar a hospedagem simultânea de nomes de malha e locatário
- Uma solução DNS econômica - locatários não é necessário implantar sua própria infraestrutura DNS
- Alta disponibilidade com a integração do Active Directory, que é necessária.

Além desses recursos, se você estiver preocupado sobre como manter seu anúncio integrado servidores DNS abrir com a Internet, você pode implantar servidores iDNS atrás outra resolução recursiva na rede de perímetro.

Como iDNS é um servidor centralizado para todas as consultas DNS, um CSP ou Enterprise pode também implementar locatário DNS firewalls, aplicar filtros, detectar atividades mal-intencionadas e transações em um local central de auditoria

## <a name="idns-infrastructure"></a>iDNS infraestrutura
A infraestrutura de iDNS inclui iDNS servidores e iDNS proxy.

### <a name="idns-servers"></a>iDNS servidores
iDNS inclui um conjunto de servidores DNS que hospedam dados específico de locatário, como registros de recurso de DNS VM.

servidores de iDNS são os servidores autoritativos para as zonas de DNS internos e também atuar como um resolvedor para nomes públicos do locatário quando VMs tentar se conectar aos recursos externos.

Todos os nomes de host para VMs em redes virtuais são armazenados como registros de recurso de DNS na mesma zona. Por exemplo, se você implantar iDNS para uma zona chamada Contoso. local, os registros de recurso DNS para VMs nessa rede são armazenados na zona da Contoso. local.

Locatário VM totalmente qualificado nomes de domínio \(FQDNs\) consistem em nome do computador e a cadeia de caracteres de sufixo DNS para a rede Virtual, no formato GUID. Por exemplo, se você tiver um locatário VM denominado TENANT1 que está no contoso rede Virtual, local, o FQDN da VM é TENANT1. *vn guid*. contoso. local, onde *vn guid* é a cadeia de caracteres de sufixo DNS para a rede Virtual.

>[!NOTE]
>Se você for um administrador de malha, você pode usar sua infraestrutura do CSP ou Enterprise DNS como servidores de iDNS em vez de implantação de novos servidores DNS especificamente para usam como iDNS servidores. Se você implantar novos servidores para iDNS ou usar sua infraestrutura existente, iDNS depende do Active Directory para fornecer alta disponibilidade. Os servidores de iDNS, portanto, devem ser integrados com o Active Directory.

### <a name="idns-proxy"></a>iDNS Proxy
iDNS proxy é um serviço do Windows que é executado em cada host e que encaminha o locatário tráfego DNS de rede Virtual para o servidor iDNS.

A ilustração a seguir mostra os caminhos de tráfego DNS de locatário redes virtuais por meio do proxy iDNS para o servidor iDNS e da Internet.

![iDNS infraestrutura](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Como implantar iDNS
Quando você implanta SDN no Windows Server 2016 usando scripts, iDNS é incluído automaticamente na sua implantação.

Para obter mais informações, consulte os tópicos a seguir.

- [Implante uma infraestrutura de rede definidos do Software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Noções básicas sobre iDNS etapas de implantação
Você pode usar esta seção para obter uma compreensão de como iDNS está instalado e configurado quando você implanta SDN usando scripts.

A seguir está um resumo das etapas necessárias para implantar iDNS.

>[!NOTE]
>Se você implantou SDN usando scripts, você não precisa executar qualquer uma dessas etapas. As etapas são fornecidas para fins de solução de problemas somente e informações.

### <a name="step-1-deploy-dns"></a>Etapa 1: Implantar DNS
Você pode implantar um servidor DNS usando o seguinte exemplo de comando do Windows PowerShell.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Etapa 2: Configurar iDNS informações no controlador de rede
Esse segmento de script é uma chamada REST é feita pelo administrador para o controlador de rede, informando-lo sobre a configuração de zona iDNS - por exemplo, o endereço IP do iDNSServer e na zona que é usada para hospedar os nomes de iDNS. 

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
>Este é um trecho da seção **configuração ConfigureIDns** em SDNExpress.ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definidos do Software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Etapa 3: Configurar o serviço Proxy iDNS
O o serviço Proxy iDNS é executado em cada um dos hosts Hyper-V, fornecendo a ponte entre as redes virtuais de locatários e a rede física onde os servidores iDNS estão localizados. As seguintes chaves do registro devem ser criadas em cada host do Hyper-V.


**Porta do DNS:** corrigido porta 53

- Chave do registro = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "Porta"
- ValueData = 53
- ValueType = "Dword"
       

**Porta do Proxy DNS:** corrigido porta 53

- Chave do registro = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**DNS IP:** esse é o endereço IP fixo configurado na interface de rede, caso o locatário opta por usar o serviço de iDNS.

- Chave do registro = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "Cadeia de caracteres"

        
**Endereço MAC:** endereço de controle de acesso de mídia do servidor DNS

- Chave do registro = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = "aa-bb-cc-aa-bb-cc"
- ValueType = "Cadeia de caracteres"

**Endereço do servidor IDNS:** lista de servidores iDNS CSV.

- Chave do registro: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "Encaminhadores"
- ValueData = "10.0.0.9"
- ValueType = "Cadeia de caracteres"



>[!NOTE]
>Este é um trecho da seção **configuração ConfigureIDnsProxy** em SDNExpress.ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definidos do Software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Etapa 4: Reiniciar o serviço de agente de Host de controlador de rede
Você pode usar o seguinte comando do Windows PowerShell para reiniciar o serviço de agente de Host de controlador de rede.
    
    Restart-Service nchostagent -Force
    
Para obter mais informações, consulte [reiniciar serviço](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Habilitar regras de firewall para o serviço de proxy DNS
Você pode usar o seguinte comando do Windows PowerShell para criar uma regra de firewall que permite exceções para o proxy para se comunicar com a VM e o servidor iDNS.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Para obter mais informações, consulte [habilitar NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Validar o serviço iDNS
Para validar o iDNS serviço, você deve implantar uma carga de trabalho de locatário do exemplo.

Para obter mais informações, consulte [cria uma VM e conectar-se a uma rede Virtual de locatário ou VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Se você quiser locatário VM a usar o serviço de iDNS, você deve sair da configuração de servidor DNS de interfaces de rede VM em branco e permitir que as interfaces usar DHCP. 

Depois que a VM com tal uma interface de rede é iniciada, ele recebe uma configuração que permite que a VM usar iDNS automaticamente e a máquina virtual começa imediatamente a executar a resolução de nome, usando o serviço de iDNS.

Se você definir o locatário VM a usar o serviço de iDNS, deixando informações de servidor DNS e servidor DNS alternativo de interface de rede em branco, o controlador de rede fornece a VM com um endereço IP e executa um registro de nome DNS em nome da VM com o servidor iDNS. 

Controlador de rede também informa o proxy iDNS sobre a VM e os detalhes necessários para executar a resolução de nome para a VM. 

Quando a VM inicia uma consulta DNS, o proxy age como um encaminhador da consulta da rede Virtual para o serviço de iDNS. 

O proxy DNS também garante que as consultas VM locatário estão isoladas. Se o servidor iDNS é autoritativo para a consulta, o servidor iDNS responde com uma resposta autorizada. Se o servidor iDNS não está autorizado para a consulta, ele executa recursão DNS para resolver nomes de Internet.

>[!NOTE]
>Essas informações estão incluídas na seção **configuração AttachToVirtualNetwork** em SDNExpressTenant.ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definidos do Software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

