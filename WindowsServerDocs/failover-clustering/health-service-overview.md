---
title: Serviço de integridade no Windows Server
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: 5afb64dcf0c59697ed55d7cf51ef1bc36e7e0e36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863807"
---
# <a name="health-service-in-windows-server"></a>Serviço de integridade no Windows Server

> Aplica-se ao Windows Server 2016

O serviço de integridade é um novo recurso no Windows Server 2016, o que melhora o monitoramento de rotina e experiência operacional para clusters que executam espaços de armazenamento diretos.

## <a name="prerequisites"></a>Pré-requisitos  

O Serviço de Integridade é habilitado por padrão com Espaços de Armazenamento Direto. Nenhuma ação adicional é necessária para configurá-lo ou iniciá-lo. Para saber mais sobre espaços de armazenamento diretos, consulte [espaços de armazenamento diretos no Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Relatórios

Ver [relatórios de integridade do serviço](health-service-reports.md).

## <a name="faults"></a>Falhas

Ver [falhas de serviço de integridade](health-service-faults.md).

## <a name="actions"></a>Ações

Ver [ações de serviço de integridade](health-service-actions.md).

## <a name="automation"></a>Automação  

Esta seção descreve os fluxos de trabalho que são automatizados pelo Serviço de Integridade no ciclo de vida do disco.  

### <a name="disk-lifecycle"></a>Ciclo de vida do disco   

O Serviço de Integridade automatiza a maioria dos estágios do ciclo de vida do disco físico. Digamos que o estado inicial de sua implantação esteja com integridade perfeita, ou seja, todos os discos físicos estão funcionando corretamente.  

#### <a name="retirement"></a>Desativação  

Os discos físicos são desativados automaticamente quando não podem ser usados e uma falha correspondente é gerada. Há vários casos:  

-   Falha de mídia: o disco físico está definitivamente com falha ou danificado e deve ser substituído.  

-   Perda de comunicação: o disco físico perdeu a conectividade por mais de 15 minutos consecutivos.  

-   Sem resposta: o disco físico apresentou latência de mais de 5,0 segundos três ou mais vezes em uma hora.  

>[!NOTE]
> Se a conectividade for perdida em muitos discos físicos ao mesmo tempo ou para um nó ou compartimento de armazenamento inteiro, o Serviço de Integridade *não* desativará esses discos, pois eles podem não ser a causa-raiz do problema.  

Se o disco desativado atuava como o cache de muitos outros discos físicos, eles serão automaticamente reatribuídos para outro disco de cache, caso disponível. Nenhuma ação especial do usuário é necessária.  

#### <a name="restoring-resiliency"></a>Restauração de resiliência  

Depois que um disco físico é desativado, o Serviço de Integridade começa imediatamente a copiar seus dados para os discos físicos restantes, para restaurar a capacidade de recuperação completa. Quando isso é concluído, os dados estão completamente seguros e tolerantes a falhas novamente.  

>[!NOTE]
> Essa restauração imediata requer capacidade disponível suficiente entre os discos físicos restantes.  

#### <a name="blinking-the-indicator-light"></a>Piscar a luz indicadora  

Se possível, o Serviço de Integridade começará a piscar a luz indicadora no disco físico obsoleto ou em seu slot. Isso continuará indefinidamente, até que o disco obsoleto seja substituído.  

>[!NOTE]
> Em alguns casos, o disco pode ter falhado de uma maneira que impeça até mesmo que seu indicador luminoso funcione (por exemplo, uma perda total de energia).  

#### <a name="physical-replacement"></a>Substituição física  

Você deve substituir o disco físico obsoleto quando possível. Geralmente, isso consiste em intercambiar - ou seja, desligar o compartimento de armazenamento ou o nó não é necessária. Veja Falha para obter informações úteis sobre locais e peças.  

#### <a name="verification"></a>Verificação

Quando o disco de substituição é inserido, ele será verificado contra o documento de componentes com suporte (consulte a próxima seção).

#### <a name="pooling"></a>Pool  

Se permitido, o disco de substituição é substituído automaticamente no pool do predecessor para que o uso seja iniciado. Nesse ponto, o sistema retorna ao estado inicial de integridade perfeita e, em seguida, a falha desaparece.  

## <a name="supported-components-document"></a>Documento de componentes com suporte  

O serviço de integridade fornece um mecanismo de imposição para restringir os componentes usados por espaços de armazenamento diretos para aqueles em um documento de componentes com suporte fornecido pelo administrador ou fornecedor da solução. Isso pode ser usado para impedir o uso incorreto de hardware sem suporte por você ou outros usuários, o que pode ajudar na conformidade de contrato de suporte ou garantia. Essa funcionalidade é atualmente limitada para dispositivos de disco físico, incluindo SSDs, HDDs, e unidades de NVMe. O documento de componentes com suporte pode restringir o modelo, fabricante (opcional) e versão de firmware (opcional).

### <a name="usage"></a>Uso  

O documento de componentes com suporte usa uma sintaxe inspirada em XML. É recomendável usar seu editor de texto favorito, como a versão gratuita [Visual Studio Code](http://code.visualstudio.com/) ou bloco de notas, para criar um documento XML que você pode salvar e reutilizar.

#### <a name="sections"></a>Seções

O documento tem duas seções independentes: `Disks` e `Cache`.

Se o `Disks` seção for fornecido, somente as unidades listadas (como `Disk`) têm permissão para ingressar em pools. Todas as unidades não listadas serão impedidas de ingressar em pools, o que efetivamente impede seu uso em produção. Se esta seção é deixada em branco, qualquer unidade poderá ingressar em pools.

Se o `Cache` seção for fornecido, somente as unidades listadas (como `CacheDisk`) são usados para armazenar em cache. Se esta seção é deixada em branco, espaços de armazenamento diretos tenta [estimativa com base no tipo de mídia e tipo de barramento](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically). Unidades listadas aqui também devem ser listadas no `Disks`.

>[!IMPORTANT]
> O documento de componentes com suporte não se aplica retroativamente a unidades já no pool e em uso.  

#### <a name="example"></a>Exemplo

```XML
<Components>

  <Disks>
    <Disk>
      <Manufacturer>Contoso</Manufacturer>
      <Model>XYZ9000</Model>
      <AllowedFirmware>
        <Version>2.0</Version>
        <Version>2.1</Version>
        <Version>2.2</Version>
      </AllowedFirmware>
      <TargetFirmware>
        <Version>2.1</Version>
        <BinaryPath>C:\ClusterStorage\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Disks>

  <Cache>
    <CacheDisk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </CacheDisk>
  </Cache>

</Components>

```

Para listar várias unidades, basta adicionar mais `<Disk>` ou `<CacheDisk>` marcas.

Para injetar esse XML durante a implantação de espaços de armazenamento diretos, use o `-XML` parâmetro:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Enable-ClusterS2D -XML $MyXML
```

Para definir ou modificar o documento de componentes com suporte quando espaços de armazenamento diretos tiver sido implantado:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>As propriedades de modelo, fabricante e versão do firmware devem corresponder exatamente aos valores que você obtém usando o cmdlet **Get-PhysicalDisk**. Isso pode ser diferente de sua expectativa de "senso comum", dependendo da implementação do fornecedor. Por exemplo, em vez de "Contoso", o fabricante pode ser "CONTOSO-LTD" ou pode estar em branco, enquanto o modelo é "Contoso-XZY9000".  

Você pode verificar usando o seguinte cmdlet do PowerShell:  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>Configurações

Ver [configurações do serviço de integridade](health-service-settings.md).

## <a name="see-also"></a>Consulte também

- [Relatórios de integridade do serviço](health-service-reports.md)
- [Falhas de serviço de integridade](health-service-faults.md)
- [Ações de serviço de integridade](health-service-actions.md)
- [Configurações de serviço de integridade](health-service-settings.md)
- [Espaços de armazenamento diretos no Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
