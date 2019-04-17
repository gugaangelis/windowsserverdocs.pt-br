---
title: Modelos NPS
description: Este tópico fornece uma visão geral dos modelos de servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2835959b8c076ef7b6aeb1fca31a62717ef95037
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="nps-templates"></a>Modelos NPS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Modelos \(NPS\) o servidor de políticas de rede permitem que você crie elementos de configuração, como os clientes Remote Authentication Dial-In User Service \(RADIUS\) ou segredos compartilhados, que você pode reutilizar no servidor NPS local e exportar para uso em outros servidores NPS.

Modelos NPS são projetados para reduzir a quantidade de tempo e custo que leva a configuração do NPS em um ou mais servidores. Os seguintes tipos de modelo NPS estão disponíveis para configuração no gerenciamento de modelos:

- Segredos compartilhados
- Clientes RADIUS
- Servidores RADIUS remotos
- Filtros IP
- Grupos de servidores de correção

Configurando um modelo é diferente de configurar o servidor NPS diretamente. Criar um modelo não afeta a funcionalidade do servidor NPS. É apenas quando você seleciona o modelo no local apropriado no console do NPS o modelo afeta a funcionalidade do servidor NPS. 

Por exemplo, se você definir um cliente RADIUS no console do NPS em clientes e servidores RADIUS, você tiver alterado a configuração do servidor NPS e avançou na configuração do NPS para se comunicar com um dos seus rede acesso servidores \(NAS's\). \ (A próxima etapa seria configurar o para se comunicar com NPS. \) no entanto, se você definir um novo modelo de clientes RADIUS no console do NPS em **gerenciamento modelos** em vez de criar um novo cliente RADIUS em **clientes e servidores RADIUS**, você criou um modelo, mas você não alterou a funcionalidade do servidor NPS ainda. Para alterar a funcionalidade do servidor NPS, você deve selecionar o modelo no local correto no console do NPS.

## <a name="creating-templates"></a>Criando modelos

Para criar um modelo, abra o console NPS, clique com botão direito um tipo de modelo, como **filtros IP**e clique em **nova**. Uma nova caixa de diálogo de propriedades de modelo é aberta que permite que você configure seu modelo.

## <a name="using-templates-locally"></a>Usando modelos localmente

Você pode usar um modelo que você criou no **gerenciamento modelos** navegando até um local no console do NPS onde o modelo pode ser aplicado. Por exemplo, se você criar um novo modelo segredos compartilhados que você deseja aplicar a uma configuração de cliente RADIUS, em **clientes e servidores RADIUS** e **clientes RADIUS**, abra as propriedades de cliente RADIUS. Em **selecionar um modelo de segredos compartilhados existente**, selecione o modelo que você criou anteriormente na lista de modelos disponíveis.

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
