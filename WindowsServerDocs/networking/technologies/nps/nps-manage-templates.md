---
title: Gerenciar modelos do NPS
description: Este tópico fornece instruções sobre como criar, aplicar, exportar e importar modelos de NPS para o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ac733c11b277f09e64779c33d3392303fc34d98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396158"
---
# <a name="manage-nps-templates"></a>Gerenciar modelos do NPS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar os modelos do servidor de políticas de rede \(NPS @ no__t-1 para criar elementos de configuração, como serviço RADIUS clientes \(RADIUS @ no__t-3 ou segredos compartilhados, que você pode reutilizar no NPS local e exportar para uso em outros NPSs. 

O gerenciamento de modelos fornece um nó no console do NPS, no qual você pode criar, modificar, excluir, duplicar e exibir o uso de modelos de NPS. Os modelos de NPS são projetados para reduzir a quantidade de tempo e o custo necessário para configurar o NPS em um ou mais servidores.

Os seguintes tipos de modelo de NPS estão disponíveis para configuração no gerenciamento de modelos.

- **Segredos compartilhados**. Esse tipo de modelo possibilita que você especifique um segredo compartilhado que pode ser reutilizado (selecionando o modelo no local apropriado no console do NPS) ao configurar clientes e servidores RADIUS. 

- **Clientes RADIUS**. Esse tipo de modelo possibilita que você defina as configurações do cliente RADIUS que podem ser reutilizadas selecionando o modelo no local apropriado no console do NPS.

- **Servidores RADIUS remotos**. Esse modelo possibilita que você defina as configurações do servidor RADIUS remoto que você pode reutilizar selecionando o modelo no local apropriado no console do NPS. 

- **Filtros de IP**. Esse modelo possibilita que você crie filtros IPv4 (protocolo IP versão 4) e IP versão 6 \(IPv6 @ no__t-1 que podem ser reutilizados \(by selecionando o modelo no local apropriado no console do NPS @ no__t-3 quando você configurar políticas de rede.

## <a name="create-an-nps-template"></a>Criar um modelo de NPS

Configurar um modelo é diferente de configurar diretamente o NPS. A criação de um modelo não afeta a funcionalidade do NPS. Ele é apenas quando você seleciona o modelo no local apropriado no console do NPS e aplica o modelo que o modelo afeta a funcionalidade do NPS. 

Por exemplo, se você configurar um cliente RADIUS no console do NPS em **clientes e servidores RADIUS**, altere a configuração do NPS e execute uma etapa para configurar o NPS para se comunicar com um dos seus servidores de acesso à rede. a próxima etapa do @no__t 0The é configurar o servidor de acesso à rede \(NAS @ no__t-2 para se comunicar com o NPS. \) 

No entanto, se você configurar um novo modelo de **clientes RADIUS** no console do NPS em **Gerenciamento de modelos** , em vez de criar um novo cliente RADIUS em **clientes e servidores RADIUS**, você criou um modelo, mas não alterou o Funcionalidade do NPS ainda. Para alterar a funcionalidade do NPS, você deve aplicar o modelo do local correto no console do NPS.

O procedimento a seguir fornece instruções sobre como criar um novo modelo.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

### <a name="to-create-an-nps-template"></a>Para criar um modelo de NPS


1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto. 

2. No console do NPS, expanda **Gerenciamento de modelos**, clique com o botão direito do mouse em um tipo de modelo, como **clientes RADIUS**e clique em **novo**.

3. Uma nova caixa de diálogo Propriedades do modelo é aberta e você pode usar o para configurar o modelo.

## <a name="apply-an-nps-template"></a>Aplicar um modelo de NPS

Você pode usar um modelo que você criou no **Gerenciamento de modelos** navegando até um local no console do NPS em que você pode aplicar o modelo. Por exemplo, se você quiser aplicar um modelo de segredos compartilhados a uma configuração de cliente RADIUS, poderá usar o procedimento a seguir.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

### <a name="to-apply-an-nps-template"></a>Para aplicar um modelo de NPS

1. No NPS, em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, clique em **servidor de políticas de rede**. O console do NPS é aberto.

2. No console do NPS, expanda **clientes e servidores RADIUS**e expanda **clientes RADIUS**.

3.In **clientes RADIUS**, no painel de detalhes, clique com o botão direito do mouse no cliente RADIUS ao qual você deseja aplicar o modelo de NPS e clique em **Propriedades**.

4. Na caixa de diálogo Propriedades do cliente RADIUS, em **selecionar um modelo de segredos compartilhados existente**, selecione o modelo que você deseja aplicar na lista de modelos.

## <a name="export-or-import-nps-templates"></a>Exportar ou importar modelos de NPS

Você pode exportar modelos para uso em outros NPSs, ou você pode importar modelos para o **Gerenciamento de modelos** para uso no computador local. 

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

### <a name="to-export-or-import-nps-templates"></a>Para exportar ou importar modelos de NPS

1. Para exportar modelos de NPS, no console do NPS, clique com o botão direito do mouse em **Gerenciamento de modelos**e clique em **exportar modelos para um arquivo**.

2. Para importar modelos de NPS, no console do NPS, clique com o botão direito do mouse em **Gerenciamento de modelos**e clique em **importar modelos de um computador** ou **importar modelos de um arquivo**.


