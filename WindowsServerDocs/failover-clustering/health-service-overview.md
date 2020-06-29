---
title: Serviço de Integridade no Windows Server
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 5bc71e71-920e-454f-8195-afebd2a23725
author: cosmosdarwin
ms.date: 02/09/2018
ms.openlocfilehash: 1b607869245ff46bd01824ebe4392e283be50b0d
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473073"
---
# <a name="health-service-in-windows-server"></a>Serviço de Integridade no Windows Server

> Aplica-se a: Windows Server 2019, Windows Server 2016

O Serviço de Integridade é um novo recurso do Windows Server 2016 que melhora o monitoramento diário e a experiência operacional para clusters que executam o Espaços de Armazenamento Diretos.

## <a name="prerequisites"></a>Pré-requisitos

O Serviço de Integridade é habilitado por padrão com Espaços de Armazenamento Direto. Nenhuma ação adicional é necessária para configurá-lo ou iniciá-lo. Para saber mais sobre Espaços de Armazenamento Diretos, consulte [espaços de armazenamento diretos no Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md).

## <a name="reports"></a>Relatórios

Consulte [serviço de integridade relatórios](health-service-reports.md).

## <a name="faults"></a>Falhas

Consulte [serviço de integridade falhas](health-service-faults.md).

## <a name="actions"></a>Ações

Consulte [serviço de integridade ações](health-service-actions.md).

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

Você deve substituir o disco físico obsoleto quando possível. Geralmente, isso consiste em uma troca a quente, ou seja, desligar o nó ou o compartimento de armazenamento não é necessário. Veja Falha para obter informações úteis sobre locais e peças.

#### <a name="verification"></a>Verificação

Quando o disco de substituição for inserido, ele será verificado em relação ao documento de componentes com suporte (consulte a próxima seção).

#### <a name="pooling"></a>Agrupamento

Se permitido, o disco de substituição é substituído automaticamente no pool do predecessor para que o uso seja iniciado. Nesse ponto, o sistema retorna ao estado inicial de integridade perfeita e, em seguida, a falha desaparece.

## <a name="supported-components-document"></a>Documento de componentes com suporte

O Serviço de Integridade fornece um mecanismo de imposição para restringir os componentes usados por Espaços de Armazenamento Diretos àqueles em um documento de componentes com suporte fornecido pelo administrador ou pelo fornecedor da solução. Isso pode ser usado para impedir o uso incorreto de hardware sem suporte por você ou outros usuários, o que pode ajudar na conformidade de contrato de suporte ou garantia. Essa funcionalidade está atualmente limitada a dispositivos de disco físico, incluindo SSDs, HDDs e unidades de NVMe. O documento de componentes com suporte pode restringir o modelo, o fabricante (opcional) e a versão do firmware (opcional).

### <a name="usage"></a>Uso

O documento de componentes com suporte usa uma sintaxe inspirada em XML. É recomendável usar seu editor de texto favorito, como o [Visual Studio Code](https://code.visualstudio.com/) gratuito ou o bloco de notas, para criar um documento XML que você pode salvar e reutilizar.

#### <a name="sections"></a>Seções

O documento tem duas seções independentes: `Disks` e `Cache` .

Se a `Disks` seção for fornecida, somente as unidades listadas (as `Disk` ) terão permissão para unir pools. Todas as unidades não listadas são impedidas de unir pools, o que efetivamente impede seu uso na produção. Se esta seção for deixada vazia, qualquer unidade terá permissão para ingressar em pools.

Se a `Cache` seção for fornecida, somente as unidades listadas (as `CacheDisk` ) serão usadas para cache. Se esta seção for deixada vazia, Espaços de Armazenamento Diretos tentará [adivinhar com base no tipo de mídia e no tipo de barramento](../storage/storage-spaces/understand-the-cache.md#cache-drives-are-selected-automatically). As unidades listadas aqui também devem ser listadas em `Disks` .

>[!IMPORTANT]
> O documento de componentes com suporte não se aplica retroativamente às unidades já agrupadas e em uso.

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

Para listar várias unidades, basta adicionar `<Disk>` marcas ou outras `<CacheDisk>` .

Para injetar esse XML ao implantar Espaços de Armazenamento Diretos, use o `-XML` parâmetro:

```PowerShell
$MyXML = Get-Content <Filepath> | Out-String
Enable-ClusterS2D -XML $MyXML
```

Para definir ou modificar o documento de componentes com suporte depois que Espaços de Armazenamento Diretos tiver sido implantado:

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

Consulte [configurações de serviço de integridade](health-service-settings.md).

## <a name="additional-references"></a>Referências adicionais

- [Relatórios de Serviço de Integridade](health-service-reports.md)
- [Falhas de Serviço de Integridade](health-service-faults.md)
- [Ações de Serviço de Integridade](health-service-actions.md)
- [Configurações de Serviço de Integridade](health-service-settings.md)
- [Espaços de Armazenamento Diretos no Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
