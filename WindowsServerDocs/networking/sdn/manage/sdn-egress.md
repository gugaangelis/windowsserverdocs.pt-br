---
title: Saída monitoração na rede virtual
description: Um aspecto fundamental do monetização de rede em nuvem é A saída da largura de banda da rede. Por exemplo-transferências de dados de saída no modelo de negócios Microsoft Azure. Os dados de saída são cobrados com base na quantidade total de dados que saem dos data centers do Azure pela Internet em um determinado ciclo de cobrança.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 10/02/2018
ms.openlocfilehash: a5d530d5cd1b42206bd6881ee902496713573793
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854429"
---
# <a name="egress-metering-in-a-virtual-network"></a>Saída de medição em uma rede virtual

>Aplica-se a: Windows Server 2019


Um aspecto fundamental do monetização de rede em nuvem é ser capaz de cobrar pela utilização da largura de banda da rede. Os dados de saída são cobrados com base na quantidade total de dados que saem do data center pela Internet em um determinado ciclo de cobrança.

A medição de saída para o tráfego de rede SDN no Windows Server 2019 permite a capacidade de oferecer medidores de uso para transferências de dados de saída. O tráfego de rede que deixa cada rede virtual, mas permanece dentro do data center pode ser acompanhado separadamente para que possa ser excluído dos cálculos de cobrança. Os pacotes associados aos endereços IP de destino que não estão incluídos em um dos intervalos de endereços não faturados são controlados como transferências de dados de saída cobradas.

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>Intervalos de endereços não cobrados da rede virtual (lista de permissões de intervalos de IP)

Você pode encontrar intervalos de endereços não faturados na propriedade **UnbilledAddressRanges** de uma rede virtual existente. Por padrão, não há nenhum intervalo de endereços adicionado.

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

A saída terá uma aparência semelhante a esta:
   ```
    AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
    DhcpOptions            :
    UnbilledAddressRanges  :
    ConfigurationState     :
    ProvisioningState      : Succeeded
    Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                         29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
    VirtualNetworkPeerings :
    EncryptionCredential   :
    LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Exemplo: gerenciar os intervalos de endereços não faturados de uma rede virtual

Você pode gerenciar o conjunto de prefixos de sub-rede IP para excluir da medição de egresso cobrada definindo a propriedade **UnbilledAddressRange** de uma rede virtual.  Qualquer tráfego enviado por interfaces de rede na rede virtual com um endereço IP de destino que corresponda a um dos prefixos não será incluído na propriedade BilledEgressBytes.

1.  Atualize a propriedade **UnbilledAddressRanges** para conter as sub-redes que não serão cobradas pelo acesso.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >Se estiver adicionando várias sub-redes IP, use uma vírgula entre cada uma das sub-redes IP.  Não inclua nenhum espaço antes ou depois da vírgula.

2.  Atualize o recurso de rede virtual com a propriedade **UnbilledAddressRanges** modificada.

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    A saída terá uma aparência semelhante a esta:
      ```
         Confirm
         Performing the operation 'New-NetworkControllerVirtualNetwork' on entities of type
         'Microsoft.Windows.NetworkController.VirtualNetwork' via
         'https://sdn.contoso.com/networking/v3/virtualNetworks/VNet1'. Are you sure you want to continue?
         [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y


         Tags             :
         ResourceRef      : /virtualNetworks/VNet1
         InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
         Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
         ResourceMetadata :
         ResourceId       : VNet1
         Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
      ```


3. Verifique a rede virtual para ver o **UnbilledAddressRanges**configurado.

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   A saída agora terá uma aparência semelhante a esta:
   ```
   AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
   DhcpOptions            :
   UnbilledAddressRanges  : 10.10.2.0/24,192.168.2.0/24
   ConfigurationState     :
   ProvisioningState      : Succeeded
   Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                        29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
   VirtualNetworkPeerings :
   EncryptionCredential   :
   LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>Verificar o uso de egresso não cobrado de uma rede virtual cobrada

Depois de configurar a propriedade **UnbilledAddressRanges** , você pode verificar o uso de egresso cobrado e não faturado de cada sub-rede em uma rede virtual. O tráfego de saída é atualizado a cada quatro minutos com o total de bytes dos intervalos cobrados e não cobrados.

As seguintes propriedades estão disponíveis para cada sub-rede virtual:

-   **UnbilledEgressBytes** mostra o número de bytes não cobrados enviados por interfaces de rede conectadas a esta sub-rede virtual. Bytes não faturados são bytes enviados a intervalos de endereços que fazem parte da propriedade **UnbilledAddressRanges** da rede virtual pai.

-   **BilledEgressBytes** mostra o número de bytes cobrados enviados por interfaces de rede conectadas a esta sub-rede virtual. Bytes cobrados são bytes enviados a intervalos de endereços que não fazem parte da propriedade **UnbilledAddressRanges** da rede virtual pai.

Use o exemplo a seguir para consultar o uso de egresso:

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

A saída terá uma aparência semelhante a esta:
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---
