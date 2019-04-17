---
title: "Serviço de integridade no Windows Server 2016"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 834fcfb749e89e4768dce3f229564feea550a432
ms.sourcegitcommit: 30fcae929ce7b611f5d3a5f8fee64b0299272110
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2017
---
# <a name="health-service-in-windows-server-2016"></a>Serviço de integridade no Windows Server 2016
> Aplica-se para o Windows Server 2016

O serviço de integridade é um novo recurso no Windows Server 2016 que melhora o monitoramento diárias e experiência operacional para clusters que executam direta de espaços de armazenamento.

## <a name="prerequisites"></a>Pré-requisitos  

O serviço de integridade é habilitado por padrão com espaços de armazenamento Direct. Nenhuma ação adicional é necessária para configurá-lo ou iniciá-lo. Para saber mais sobre direta de espaços de armazenamento, consulte [espaços de armazenamento direto no Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).  

## <a name="reports"></a>Relatórios

Consulte [serviço de integridade relata](health-service-reports.md).

## <a name="faults"></a>Falhas

Consulte [falhas de serviço de integridade](health-service-faults.md).

## <a name="actions"></a>Ações

Consulte [ações de serviço de integridade](health-service-actions.md).

## <a name="automation"></a>Automação  

Esta seção descreve os fluxos de trabalho que sejam automatizados pelo serviço de integridade no ciclo de vida do disco.  

### <a name="disk-lifecycle"></a>Ciclo de vida do disco   

O serviço de integridade automatiza a maioria dos estágios do ciclo de vida de disco físico. Digamos que o estado inicial da implantação está em perfeita integridade - o que é dizer que todos os discos físicos estão funcionando adequadamente.  

#### <a name="retirement"></a>Aposentadoria  

Discos físicos são desativados automaticamente quando eles não podem ser usados, e uma falha correspondente é acionada. Há diversos casos:  

-   Falha de mídia: o disco físico é definitivamente falhou ou dividido e deve ser substituído.  

-   Comunicação perdida: o disco físico perdeu conectividade por mais de 15 minutos consecutivos.  

-   Sem resposta: o disco físico tenha apresentado latência de segundos 5.0 mais de três ou mais vezes em uma hora.  

>[!NOTE]
> Se a conexão é perdida para muitos discos físicos ao mesmo tempo ou em um nó inteiro ou compartimento de armazenamento, o serviço de integridade será *não* eliminar esses discos, desde que eles podem não ser a causa do problema.  

Se o disco desativado estava funcionando como o cache para muitos outros discos físicos, eles serão automaticamente ser reatribuídos a outro disco em cache se houver um disponível. Nenhuma ação do usuário especial é necessária.  

#### <a name="restoring-resiliency"></a>Restaurando resiliência  

Depois que um disco físico foi desativado, o serviço de integridade começa imediatamente a copiar seus dados nos discos físicos restantes, restaurar resiliência completa. Quando isso for concluído, os dados é completamente seguros e falhas tolerantes a falhas novamente.  

>[!NOTE]
> A restauração imediata requer capacidade suficiente disponível entre os discos físicos restantes.  

#### <a name="blinking-the-indicator-light"></a>Piscar a luz indicadora  

Se possível, o serviço de integridade começará a piscar a luz indicadora no disco físico desativado ou slot. Isso continuará indefinidamente, até que o disco desativado seja substituído.  

>[!NOTE]
> Em alguns casos, o disco pode ter falhado de maneira que impede que até mesmo a sua luz indicadora funcionem - por exemplo, uma queda de energia total.  

#### <a name="physical-replacement"></a>Substituição física  

Você deve substituir o disco físico desativado quando possível. Geralmente, isso consiste em um intercambiáveis - isto é Desligando o compartimento do nó ou armazenamento não é necessária. Veja a falha para útil informações de localização e parte.  

#### <a name="verification"></a>Verificação

Quando o disco de substituição é inserido, ele será verificado contra o documento de componentes com suporte (veja a próxima seção).

#### <a name="pooling"></a>Pool  

Se permitido, o disco de substituição é substituído automaticamente no pool do seu antecessor façam uso. Neste ponto, o sistema é retornado ao seu estado inicial de integridade perfeita e, em seguida, desaparece a falha.  

## <a name="supported-components-document"></a>Documento de componentes com suporte  

O serviço de integridade fornece um mecanismo de imposição para restringir os componentes usados por espaços de armazenamento direto para aqueles em um documento de componentes de suporte fornecido pelo administrador ou fornecedor da solução. Isso pode ser usado para impedir o uso incorreta de hardware sem suporte por você ou outros, que podem ajudar a conformidade de contrato de garantia ou suporte. Essa funcionalidade é limitada no momento para dispositivos de disco físico, incluindo discos rígidos, SSDs e NVMe unidades. O documento de componentes de suporte pode restringir no modelo, fabricante (opcional) e versão de firmware (opcional).

### <a name="usage"></a>Uso  

O documento de componentes com suporte usa uma sintaxe XML e inspirados. Recomendamos usar o editor de texto favorito, como o código do Visual Studio (disponível gratuitamente [aqui](http://code.visualstudio.com/)) ou bloco de notas, crie um documento XML que você pode salvar e reutilizar.

#### <a name="sections"></a>Seções

O documento tem duas seções independentes: **discos** e **Cache**.

Se o **discos** seção for fornecida, somente as unidades listadas podem participar pools. Quaisquer unidades unlisted são impedidas de ingressar pools, o que impede efetivamente seu uso em produção. Se esta seção for deixada vazia, qualquer unidade poderá ingressar pools.

Se o **Cache** seção for fornecida, somente as unidades listadas serão usadas para armazenar em cache. Se esta seção for deixada vazia, direto de espaços de armazenamento tentará detectar com base no tipo de mídia e o tipo de barramento. Por exemplo, se sua implantação usa unidades de estado sólido (SSD) e unidades de disco rígido (HDD), o primeiro é automaticamente escolhido para armazenar em cache; No entanto, se sua implantação usa todos os flash, talvez seja necessário especificar os dispositivos de resistência maior que você gostaria de usar para armazenar em cache aqui.

>[!IMPORTANT]
> O documento de componentes compatíveis não se aplica forma retroativa às unidades já agrupadas e em uso.  

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
        <BinaryPath>\\path\to\image.bin</BinaryPath>
      </TargetFirmware>
    </Disk>
  </Disks>

  <Cache>
    <Disk>
      <Manufacturer>Fabrikam</Manufacturer>
      <Model>QRSTUV</Model>
    </Disk>
  </Cache>

</Components>

```

Para listar várias unidades, basta adicionar adicionais ** &lt;disco&gt; ** qualquer seção as marcas.

Para inserir esse XML ao implantar espaços de armazenamento direta, use o **- XML** sinalizador:

```PowerShell
Enable-ClusterS2D -XML <MyXML>
```

Para definir ou modificar o documento de componentes compatíveis depois direta de espaços de armazenamento foi implantada (isto é, quando o serviço de integridade já estiver em execução), use o seguinte cmdlet do PowerShell:

```PowerShell
$MyXML = Get-Content <\\path\to\file.xml> | Out-String  
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.SupportedComponents.Document" -Value $MyXML  
```

>[!NOTE]
>O modelo, fabricante e as propriedades de versão de firmware devem corresponder exatamente os valores que você obtenha usando o **disco físico Get** cmdlet. Isso pode ser diferente do suas expectativas "senso comum", dependendo da implementação do fornecedor. Por exemplo, em vez de "Contoso", o fabricante pode ser "CONTOSO LTD", ou pode estar em branco enquanto o modelo é "Contoso-XZY9000".  

Você pode verificar usando o seguinte cmdlet do PowerShell:  

```PowerShell
Get-PhysicalDisk | Select Model, Manufacturer, FirmwareVersion  
```

## <a name="settings"></a>Configurações

Consulte [configurações de serviço de integridade](health-service-settings.md).

## <a name="see-also"></a>Consulte também

- [Relatórios de serviço de integridade](health-service-reports.md)
- [Falhas de serviço de integridade](health-service-faults.md)
- [Ações de serviço de integridade](health-service-actions.md)
- [Configurações de serviço de integridade](health-service-settings.md)
- [Espaços de armazenamento direcionam no Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
