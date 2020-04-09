---
title: Serviço DNS interno (iDNS) para SDN
description: Este tópico explica como você pode fornecer serviços DNS para suas cargas de trabalho de locatários hospedados usando o iDNS (DNS interno), que é integrado à rede definida pelo software no Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 717575693d40ff7d17c160772886f7ae3f648a9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854319"
---
# <a name="internal-dns-service-idns-for-sdn"></a>Serviço DNS interno (iDNS) para SDN

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Se você trabalha para um provedor de serviços de nuvem \(CSP\) ou Enterprise que está planejando implantar a rede definida pelo software \(SDN\) no Windows Server 2016, você pode fornecer serviços DNS para suas cargas de trabalho de locatários hospedados usando o DNS interno \(iDNS\), que é integrado com SDN.

Máquinas virtuais hospedadas \(VMs\) e aplicativos exigem que o DNS se comunique em suas próprias redes e com recursos externos na Internet. Com iDNS, você pode fornecer locatários com serviços de resolução de nomes DNS para seu espaço de nome local isolado e para recursos da Internet.

Como o serviço iDNS não é acessível de redes virtuais de locatário, além do proxy iDNS, o servidor não está vulnerável a atividades mal-intencionadas em redes de locatários.

**Principais recursos**

A seguir estão os principais recursos para iDNS.

- Fornece serviços de resolução de nomes DNS compartilhados para cargas de trabalho de locatário
- Serviço DNS autoritativo para resolução de nomes e registro de DNS dentro do espaço de nome do locatário
- Serviço DNS recursivo para resolução de nomes de Internet de VMs de locatário.
- Se desejar, você pode configurar a hospedagem simultânea de nomes de malha e locatário
- Uma solução de DNS econômica-os locatários não precisam implantar sua própria infraestrutura de DNS
- Alta disponibilidade com integração de Active Directory, que é necessária.

Além desses recursos, se você estiver preocupado em manter seus servidores DNS integrados do AD abertos na Internet, poderá implantar servidores de iDNS atrás de outro resolvedor recursivo na rede de perímetro.

Como iDNS é um servidor centralizado para todas as consultas DNS, um CSP ou uma empresa também pode implementar firewalls de DNS de locatário, aplicar filtros, detectar atividades mal-intencionadas e auditar transações em um local central

## <a name="idns-infrastructure"></a>Infraestrutura de iDNS
A infraestrutura de iDNS inclui servidores iDNS e proxy de iDNS.

### <a name="idns-servers"></a>Servidores iDNS
iDNS inclui um conjunto de servidores DNS que hospedam dados específicos de locatário, como registros de recursos de DNS de VM.

os servidores iDNS são os servidores autoritativos para suas zonas DNS internas e também agem como um resolvedor de nomes públicos quando as VMs de locatário tentam se conectar a recursos externos.

Todos os nomes de host para VMs em redes virtuais são armazenados como registros de recursos de DNS na mesma zona. Por exemplo, se você implantar iDNS para uma zona chamada contoso. local, os registros de recurso DNS para as VMs nessa rede serão armazenados na zona contoso. local.

Os nomes de domínio totalmente qualificados da VM de locatário \(FQDNs\) consistem no nome do computador e na cadeia de caracteres do sufixo DNS para a rede virtual, no formato GUID. Por exemplo, se você tiver uma VM de locatário denominada LOCATÁRIO1 que está na rede virtual contoso, local, o FQDN da VM será LOCATÁRIO1. *vn-GUID*. contoso. local, em que *vn-GUID* é a cadeia de caracteres de sufixo DNS para a rede virtual.

>[!NOTE]
>Se você for um administrador de malha, poderá usar o CSP ou a infraestrutura de DNS corporativo como servidores iDNS em vez de implantar novos servidores DNS especificamente para uso como servidores iDNS. Independentemente de você implantar novos servidores para iDNS ou usar sua infraestrutura existente, os iDNS se baseiam em Active Directory para fornecer alta disponibilidade. Os servidores de iDNS, portanto, devem ser integrados com Active Directory.

### <a name="idns-proxy"></a>Proxy de iDNS
proxy de iDNS é um serviço do Windows que é executado em cada host e que encaminha o tráfego DNS da rede virtual de locatário para o servidor iDNS.

A ilustração a seguir descreve os caminhos de tráfego DNS de redes virtuais de locatário por meio do proxy de iDNS para o servidor iDNS e a Internet.

![Infraestrutura de iDNS](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Como implantar iDNS
Quando você implanta o SDN no Windows Server 2016 usando scripts, os iDNS são incluídos automaticamente na sua implantação.

Para obter mais informações, consulte estes tópicos.

- [Implantar uma infraestrutura de rede definida pelo software usando scripts](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Compreendendo as etapas de implantação de iDNS
Você pode usar esta seção para entender como os iDNS são instalados e configurados quando você implanta o SDN usando scripts.

A seguir, um resumo das etapas necessárias para implantar iDNS.

>[!NOTE]
>Se você tiver implantado o SDN usando scripts, não será necessário executar nenhuma dessas etapas. As etapas são fornecidas apenas para fins de solução de problemas e informações.

### <a name="step-1-deploy-dns"></a>Etapa 1: implantar o DNS
Você pode implantar um servidor DNS usando o seguinte comando do Windows PowerShell de exemplo.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Etapa 2: configurar informações de iDNS no controlador de rede
Esse segmento de script é uma chamada REST que é feita pelo administrador para o controlador de rede, informando-o sobre a configuração de zona de iDNS, como o endereço IP do iDNSServer e a zona usada para hospedar os nomes de iDNS. 

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
>Este é um trecho da seção de **configuração ConfigureIDns** em SDNExpress. ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definida pelo software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Etapa 3: configurar o serviço proxy de iDNS
O serviço proxy de iDNS é executado em cada um dos hosts Hyper-V, fornecendo a ponte entre as redes virtuais de locatários e a rede física onde os servidores iDNS estão localizados. As seguintes chaves do registro devem ser criadas em todos os hosts Hyper-V.


**Porta DNS:** Porta fixa 53

- Chave do registro = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- Valor = "porta"
- ValueData = 53
- ValueType = "DWORD"
       

**Porta do proxy DNS:** Porta fixa 53

- Chave do registro = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- Valor = "ProxyPort"
- ValueData = 53
- ValueType = "DWORD"
        
**IP do DNS:** Endereço IP fixo configurado na interface de rede, caso o locatário opte por usar o serviço iDNS

- Chave do registro = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- Valor = "IP"
- ValueData = "169.254.169.254"
- ValueType = "cadeia de caracteres"

        
**Endereço MAC:** Endereço de controle de acesso à mídia do servidor DNS

- Chave do registro = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- Valor = "MAC"
- ValueData = "AA-BB-cc-aa-bb-CC"
- ValueType = "cadeia de caracteres"

**Endereço do servidor IDNs:** Uma lista separada por vírgulas de servidores iDNS.

- Chave do registro: HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- Valorname = "encaminhadores"
- ValueData = "10.0.0.9"
- ValueType = "cadeia de caracteres"



>[!NOTE]
>Este é um trecho da seção de **configuração ConfigureIDnsProxy** em SDNExpress. ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definida pelo software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Etapa 4: reiniciar o serviço de agente de host do controlador de rede
Você pode usar o seguinte comando do Windows PowerShell para reiniciar o serviço de agente de host do controlador de rede.
    
    Restart-Service nchostagent -Force
    
Para obter mais informações, consulte [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Habilitar regras de firewall para o serviço de proxy DNS
Você pode usar o seguinte comando do Windows PowerShell para criar uma regra de firewall que permita que as exceções do proxy se comuniquem com a VM e o servidor iDNS.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Para obter mais informações, consulte [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Validar o serviço iDNS
Para validar o serviço iDNS, você deve implantar uma carga de trabalho de locatário de exemplo.

Para obter mais informações, consulte [criar uma VM e conectar-se a uma rede virtual de locatário ou VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Se você quiser que a VM de locatário use o serviço iDNS, você deve deixar a configuração de servidor DNS de interfaces de rede VM em branco e permitir que as interfaces usem DHCP. 

Depois que a VM com tal interface de rede é iniciada, ela recebe automaticamente uma configuração que permite que a VM use iDNS, e a VM começa imediatamente a executar a resolução de nomes usando o serviço iDNS.

Se você configurar a VM de locatário para usar o serviço iDNS deixando o servidor DNS da interface de rede e as informações do servidor DNS alternativo em branco, o controlador de rede fornecerá a VM com um endereço IP e executará um registro de nome DNS em nome da VM com o servidor iDNS . 

O controlador de rede também informa o proxy de iDNS sobre a VM e os detalhes necessários para executar a resolução de nomes para a VM. 

Quando a VM inicia uma consulta DNS, o proxy atua como um encaminhador da consulta da rede virtual para o serviço iDNS. 

O proxy DNS também garante que as consultas de VM de locatário sejam isoladas. Se o servidor iDNS for autoritativo para a consulta, o servidor iDNS responderá com uma resposta autoritativa. Se o servidor iDNS não for autoritativo para a consulta, ele executará uma recursão de DNS para resolver os nomes de Internet.

>[!NOTE]
>Essas informações estão incluídas na seção **Configuration AttachToVirtualNetwork** em SDNExpressTenant. ps1. Para obter mais informações, consulte [implantar uma infraestrutura de rede definida pelo software usando scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

