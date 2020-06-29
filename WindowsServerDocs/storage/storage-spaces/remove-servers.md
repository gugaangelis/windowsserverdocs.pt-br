---
title: Removendo servidores em Espaços de Armazenamento Diretos
ms.assetid: 9d8499a7-1307-473d-9f00-8a051164fad2
ms.prod: windows-server
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
description: Como remover servidores de um cluster de Espaços de Armazenamento Diretos no Windows Server.
ms.date: 2/5/2017
ms.localizationpriority: medium
ms.openlocfilehash: 0dd888048edc96d6001492e92ba6d519c751bdaa
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474563"
---
# <a name="removing-servers-in-storage-spaces-direct"></a>Removendo servidores em Espaços de Armazenamento Diretos

>Aplica-se a: Windows Server 2019, Windows Server 2016

Este tópico descreve como remover servidores em [Espaços de Armazenamento Direto](storage-spaces-direct-overview.md) usando o PowerShell.

## <a name="remove-a-server-but-leave-its-drives"></a>Remover um servidor, mas deixar suas unidades

Se você pretende adicionar o servidor de volta ao cluster em breve, ou se pretende manter as unidades e movê-las para outro servidor, você pode remover o servidor do cluster *sem* remover as unidades do pool de armazenamento. Este é o comportamento padrão se você usar o Gerenciador de Cluster de Failover para remover o servidor.

Use o cmdlet [Remove-ClusterNode](https://technet.microsoft.com/library/hh847251.aspx) do PowerShell:

```PowerShell
Remove-ClusterNode <Name>
```

Este cmdlet obtém sucesso rápido, independentemente de qualquer consideração de capacidade, porque o pool de armazenamento "lembra" as unidades ausentes e espera que elas voltem. Não há nenhum movimento de dados para fora das unidades ausentes. Enquanto elas permanecem ausentes, seu **OperationalStatus** aparecerá "Comunicação perdido", e os volumes aparecerão como "Incompleto".

Quando as unidades voltarem, elas serão automaticamente detectadas e novamente associados ao pool, mesmo que agora estejam em um novo servidor.

   >[!WARNING]
   > Não distribua as unidades com dados de pool de um servidor em vários outros servidores. Por exemplo, se um servidor com dez unidades falhar (porque sua unidade de inicialização ou a placa-mãe falhou, por exemplo), você **pode** mover todas as dez unidades para um servidor novo, mas **não é possível** mover cada uma delas separadamente para diversos outros servidores.

## <a name="remove-a-server-and-its-drives"></a>Remover um servidor e suas unidades

Se você quiser remover permanentemente um servidor do cluster (às vezes chamado de redimensionamento), você pode remover o servidor do cluster *e* remover suas unidades do pool de armazenamento.

Use o cmdlet **Remove-ClusterNode** com o sinalizador opcional **- CleanUpDisks**:

```PowerShell
Remove-ClusterNode <Name> -CleanUpDisks
```

Este cmdlet pode levar muito tempo (às vezes, muitas horas) para ser executado porque o Windows deve mover todos os dados armazenados no servidor para outros servidores no cluster. Quando isso for concluído, as unidades são permanentemente removidas do pool de armazenamento, retornando os volumes afetados para um estado íntegro.

### <a name="requirements"></a>Requisitos

Para um redimensionamento permanente (remover um servidor *e* suas unidades), o cluster deve atender aos dois requisitos seguintes. Caso contrário, o cmdlet **Remove-ClusterNode -CleanUpDisks** retornará um erro imediatamente, antes de começar qualquer movimento de dados, para minimizar a interrupção.

#### <a name="enough-capacity"></a>Capacidade suficiente

Primeiro, você deve ter capacidade de armazenamento suficiente nos servidores restantes para acomodar todos os volumes.

Por exemplo, se tiver quatro servidores, cada um com 10 unidades de 1 TB, você terá 40 TB de capacidade total de armazenamento físico. Depois de remover um servidor e todas as suas unidades, você terá 30 TB de capacidade restantes. Se as superfícies dos volumes for de mais de 30 TB no total, eles não caberão nos servidores restantes, e assim o cmdlet vai retornar um erro e não moverá nenhum dado.

#### <a name="enough-fault-domains"></a>Domínios de falha suficientes

Segundo, você deve ter domínios de falha suficientes (geralmente servidores) para fornecer a resiliência dos volumes.

Por exemplo, se os volumes usam espelhamento de três vias no nível do servidor para resiliência, eles não podem estar totalmente íntegros, a menos que você tenha pelo menos três servidores. Se você tiver três exatamente servidores e, em seguida, tentar remover um deles e todas as suas unidades, o cmdlet retornará um erro e não moverá nenhum dado.

Esta tabela mostra o número mínimo de domínios de falha necessários para cada tipo de resiliência.

|    Resiliência          |    Mínimo necessário de domínios de falha   |
|------------------------|-------------------------------------|
|    Espelho de duas vias      |    2                                |
|    Espelho de três vias    |    3                                |
|    Paridade dupla         |    4                                |

   >[!NOTE]
   > Tudo bem ter menos servidores por um período breve, como durante falhas ou manutenção. No entanto, para que os volumes retornem a um estado totalmente íntegro, você deve ter o número mínimo de servidores mencionado acima.

## <a name="additional-references"></a>Referências adicionais

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
