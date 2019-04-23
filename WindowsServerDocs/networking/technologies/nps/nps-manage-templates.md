---
title: Gerenciar modelos do NPS
description: Este tópico fornece instruções sobre como criar, aplicar, exportar e importar modelos de NPS para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 170a0e86f7dcca77c6efe841318b522554f8e78e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845127"
---
# <a name="manage-nps-templates"></a>Gerenciar modelos do NPS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar o servidor de políticas de rede \(NPS\) modelos para criar elementos de configuração, como Remote Authentication Dial-In User Service \(RADIUS\) clientes ou segredos compartilhados, que pode ser reutilizado no local O NPS e exportação para uso em outros NPSs. 

Gerenciamento de modelos fornece um nó no console do NPS onde você pode criar, modificar, excluir, duplicar e exibir o uso de modelos NPS. Modelos de NPS são projetados para reduzir a quantidade de tempo e o custo necessário para configurar o NPS em um ou mais servidores.

Os seguintes tipos de modelos NPS estão disponíveis para configuração no gerenciamento de modelos.

- **Segredos compartilhados**. Esse tipo de modelo torna possível para que você especifique um segredo compartilhado que possa ser reutilizado (selecionando o modelo no local apropriado no console do NPS) ao configurar clientes e servidores RADIUS. 

- **Clientes RADIUS**. Esse tipo de modelo torna possível para que você possa definir as configurações de cliente RADIUS que você pode reutilizar selecionando o modelo no local apropriado no console do NPS.

- **Servidores remotos RADIUS**. Este modelo torna possível para que você possa definir configurações do servidor RADIUS remotas que você pode reutilizar selecionando o modelo no local apropriado no console do NPS. 

- **Filtros IP**. Este modelo torna possível a criação de protocolo IP versão 4 (IPv4) e protocolo IP versão 6 \(IPv6\) filtros que você pode reutilizar \(selecionando o modelo no local apropriado no NPS console\) quando você configura políticas de rede.

## <a name="create-an-nps-template"></a>Criar um modelo do NPS

Configurando um modelo é diferente de configurar o NPS diretamente. Criando um modelo não afeta a funcionalidade do NPS. Ele é apenas quando você seleciona o modelo no local apropriado no console do NPS e aplica o modelo que o modelo afeta a funcionalidade do NPS. 

Por exemplo, se você configurar um cliente RADIUS no console do NPS sob **clientes e servidores RADIUS**, você pode alterar a configuração do NPS e uma etapa na configuração do NPS para se comunicar com um dos seus servidores de acesso de rede. \(A próxima etapa é configurar o servidor de acesso de rede \(NAS\) para se comunicar com o NPS.\) 

No entanto, se você configurar um novo **clientes RADIUS** modelo no console do NPS sob **gerenciamento de modelos** em vez de criar um novo cliente RADIUS em **clientes RADIUS e servidores**, você criou um modelo, mas você não tiver alterado a funcionalidade do NPS ainda. Para alterar a funcionalidade do NPS, você deve aplicar o modelo do local correto no console do NPS.

O procedimento a seguir fornece instruções sobre como criar um novo modelo.

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-create-an-nps-template"></a>Para criar um modelo NPS


1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS. 

2. No console do NPS, expanda **gerenciamento de modelos**, clique com botão direito um tipo de modelo, como **clientes RADIUS**e, em seguida, clique em **New**.

3. Uma nova caixa de diálogo de propriedades do modelo é aberto que você pode usar para configurar seu modelo.

## <a name="apply-an-nps-template"></a>Aplicar um modelo de NPS

Você pode usar um modelo que você criou em **gerenciamento de modelos** navegando até um local no console do NPS onde você pode aplicar o modelo. Por exemplo, se você quiser aplicar um modelo de segredos compartilhados para uma configuração de cliente RADIUS, você pode usar o procedimento a seguir.

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-apply-an-nps-template"></a>Para aplicar um modelo de NPS

1. No NPS, no Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. Abre o console do NPS.

2. No console do NPS, expanda **clientes e servidores RADIUS**e, em seguida, expanda **clientes RADIUS**.

3.na **clientes RADIUS**, no painel de detalhes, clique com botão direito do cliente RADIUS para o qual você deseja aplicar o modelo NPS e, em seguida, clique em **propriedades**.

4. Na caixa de diálogo de propriedades caixa para o cliente RADIUS, além **selecione um modelo existente de segredos compartilhados**, selecione o modelo que você deseja aplicar na lista de modelos.

## <a name="export-or-import-nps-templates"></a>Exportar ou importar modelos de NPS

Você pode exportar modelos para uso em outros NPSs, ou você pode importar modelos no **gerenciamento de modelos** para uso no computador local. 

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

### <a name="to-export-or-import-nps-templates"></a>Para exportar ou importar modelos de NPS

1. Para exportar modelos de NPS, no console do NPS, clique com botão direito **gerenciamento de modelos**e, em seguida, clique em **exportar modelos para um arquivo**.

2. Para importar os modelos de NPS, no console do NPS, clique com botão direito **gerenciamento de modelos**e, em seguida, clique em **importar modelos de um computador** ou **importar modelos de um arquivo**.


