---
title: Modelos NPS
description: Este tópico fornece uma visão geral dos modelos de servidor de diretiva de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0647dbf0f99a01e32ba68475b439501e2dbeebfe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823307"
---
# <a name="nps-templates"></a>Modelos NPS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Servidor de diretivas de rede \(NPS\) modelos permitem que você crie elementos de configuração, como Remote Authentication Dial-In User Service \(RADIUS\) clientes ou segredos compartilhados, que pode ser reutilizado no local O NPS e exportação para uso em outros NPSs.

Modelos de NPS são projetados para reduzir a quantidade de tempo e o custo necessário para configurar o NPS em um ou mais servidores. Os seguintes tipos de modelos NPS estão disponíveis para configuração no gerenciamento de modelos:

- Segredos compartilhados
- Clientes RADIUS
- Servidores remotos RADIUS
- Filtros IP
- Os grupos de servidores

Configurando um modelo é diferente de configurar o NPS diretamente. Criando um modelo não afeta a funcionalidade do NPS. Ele é somente quando você seleciona o modelo no local apropriado no console do NPS que o modelo afeta a funcionalidade do NPS. 

Por exemplo, se você configurar um cliente RADIUS no console do NPS em clientes e servidores RADIUS, você tiver alterado a configuração do NPS e avançou na configuração do NPS para se comunicar com um dos seus servidores de acesso de rede \(do NAS\) . \(A próxima etapa seria configurar o para se comunicar com o NPS.\) No entanto, se você configurar um novo modelo de clientes RADIUS no console do NPS sob **gerenciamento de modelos** em vez de criar um novo cliente RADIUS sob **clientes e servidores RADIUS**, você criou um modelo, mas você não alteraram a funcionalidade do NPS ainda. Para alterar a funcionalidade do NPS, você deve selecionar o modelo do local correto no console do NPS.

## <a name="creating-templates"></a>Criação de modelos

Para criar um modelo, abra o console do NPS, clique com botão direito um tipo de modelo, como **filtros de IP**e, em seguida, clique em **New**. Uma nova caixa de diálogo de propriedades do modelo é aberto que permite que você configure seu modelo.

## <a name="using-templates-locally"></a>Usando modelos localmente

Você pode usar um modelo que você criou na **gerenciamento de modelos** navegando até um local no console do NPS onde o modelo pode ser aplicado. Por exemplo, se você criar um novo modelo de segredos compartilhados que você deseja aplicar a uma configuração de cliente RADIUS, no **clientes e servidores RADIUS** e **clientes RADIUS**, abra as propriedades do cliente RADIUS. Na **selecione um modelo existente de segredos compartilhados**, selecione o modelo que você criou anteriormente na lista de modelos disponíveis.

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
