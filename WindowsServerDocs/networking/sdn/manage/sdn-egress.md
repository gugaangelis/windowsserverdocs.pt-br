---
title: Saída monitoração na rede virtual
description: Um aspecto fundamental de monetização de rede de nuvem é a saída de largura de banda de rede. Por exemplo, transferências de dados de saída o modelo de negócios no Microsoft Azure. Dados de saída são cobrados com base na quantidade total de dados que saem dos datacenters do Azure pela Internet em um determinado ciclo de cobrança.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: bdfb2b7321d5a4d119c9710e9ad93fc2e91ea536
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792287"
---
# <a name="egress-metering-in-a-virtual-network"></a>Saída de medição em uma rede virtual

>Aplica-se a: Windows Server 2019


Um aspecto fundamental de monetização de rede de nuvem é ser capaz de cobrar pelo uso da largura de banda de rede. Dados de saída são cobrados com base na quantidade total de dados que saem do data center por meio da Internet em um determinado ciclo de cobrança.

Medição de saída para o tráfego de rede SDN no Windows Server 2019 habilita a capacidade de oferecer os medidores de uso para transferências de dados. Tráfego de rede que deixa a cada rede virtual, mas permanece dentro do data center pode por rastreados separadamente para que ele pode ser excluído de cálculos de cobrança. Os pacotes associados para endereços IP de destino que não estão incluídos em um dos intervalos de endereços não faturados são controlados à medida que cobradas de transferências de dados.

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>(Lista de permissões de intervalos de IP) de intervalos de endereços de rede virtual não faturado

Você pode encontrar os intervalos de endereços não faturados sob o **UnbilledAddressRanges** propriedade de uma rede virtual existente. Por padrão, não há nenhum intervalo de endereço adicionado.

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

A saída será semelhante a este:
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


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Exemplo: Gerenciar os intervalos de endereços de demonstrativos de uma rede virtual

Você pode gerenciar o conjunto de prefixos de sub-rede IP para excluir da medição de egresso cobrado definindo a **UnbilledAddressRange** propriedade de uma rede virtual.  Qualquer tráfego enviado por interfaces de rede em uma rede virtual com um endereço IP de destino que corresponde a um dos prefixos não será incluído na propriedade BilledEgressBytes.

1.  Atualizar o **UnbilledAddressRanges** propriedade para conter as sub-redes que não serão cobradas para acesso.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >Se estiver adicionando várias sub-redes IP, use uma vírgula entre cada uma das sub-redes IP.  Não inclua espaços antes ou após a vírgula.

2.  Atualizar o recurso de rede Virtual com a modificação **UnbilledAddressRanges** propriedade.

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    A saída será semelhante a este:
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


3. Verifique a rede Virtual para ver o configurado **UnbilledAddressRanges**.

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   A saída agora será semelhante a este:
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

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>Verifique o cobrado o uso de demonstrativos de saída de uma rede virtual

Depois de configurar o **UnbilledAddressRanges** propriedade, você pode verificar o uso de demonstrativos e cobrado o valor de saída de cada sub-rede em uma rede virtual. O tráfego de saída de atualizações a cada quatro minutos com o total de bytes dos intervalos de demonstrativos e cobrados.

As seguintes propriedades estão disponíveis para cada sub-rede virtual:

-   **UnbilledEgressBytes** mostra o número de demonstrativos de bytes enviados por interfaces de rede conectados a essa sub-rede virtual. Bytes não faturados estão bytes enviados aos intervalos de endereços que fazem parte do **UnbilledAddressRanges** propriedade da rede virtual pai.

-   **BilledEgressBytes** mostra o número de cobrado bytes enviados por adaptadores de rede conectados a essa sub-rede virtual. Cobrado bytes são bytes enviados aos intervalos de endereços que não são parte dos **UnbilledAddressRanges** propriedade da rede virtual pai.

Use o exemplo a seguir para o uso de saída de consulta:

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

A saída será semelhante a este:
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---
