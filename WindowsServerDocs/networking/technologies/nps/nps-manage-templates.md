---
title: Gerenciar os modelos NPS
description: Este tópico fornece instruções sobre como criar, aplicar, exportar e importar modelos de NPS para o servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a7f4c50d87c155c1adcd445eae8df23aab7730b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-nps-templates"></a>Gerenciar os modelos NPS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar o servidor de política de rede \(NPS\) modelos para criar elementos de configuração, como os clientes Remote Authentication Dial-In User Service \(RADIUS\) ou segredos compartilhados, que você pode reutilizar no servidor NPS local e exportar para uso em outros servidores NPS. 

Gerenciamento de modelos oferece um nó no console do NPS onde você pode criar, modificar, excluir, duplicar e exibir o uso de modelos NPS. Modelos NPS são projetados para reduzir a quantidade de tempo e custo que leva a configuração do NPS em um ou mais servidores.

Os seguintes tipos de modelo NPS estão disponíveis para configuração no gerenciamento de modelos.

- **Segredos compartilhados**. Esse tipo de modelo torna possível para especificar um segredo compartilhado que você pode reutilizar (selecionando o modelo no local apropriado no console do NPS) quando você configurar os clientes e servidores RADIUS. 

- **Clientes RADIUS**. Esse tipo de modelo torna possível para que você definir as configurações de cliente RADIUS que você pode reutilizar selecionando o modelo no local apropriado no console do NPS.

- **Servidores RADIUS remotos**. Esse modelo torna possível para definir configurações de servidores RADIUS remoto que você pode reutilizar selecionando o modelo no local apropriado no console do NPS. 

- **Filtros IP**. Esse modelo torna possível para criar o protocolo IP versão 4 (IPv4) e protocolo IP versão 6 \(IPv6\) filtros que você pode reutilizar \ (selecionando o modelo no local apropriado em console\ o NPS) quando você configura as políticas de rede.

## <a name="create-an-nps-template"></a>Criar um modelo NPS

Configurando um modelo é diferente de configurar o servidor NPS diretamente. Criar um modelo não afeta a funcionalidade do servidor NPS. É apenas quando você seleciona o modelo no local apropriado no console do NPS e aplica o modelo que o modelo afeta a funcionalidade do servidor NPS. 

Por exemplo, se você definir um cliente RADIUS no console do NPS em **clientes e servidores RADIUS**, você pode alterar a configuração do servidor NPS e tirar uma etapa na configuração do NPS para se comunicar com um dos seus servidores de acesso de rede. \ (A próxima etapa é configurar o servidor de acesso de rede \(NAS\) para se comunicar com NPS. \) 

No entanto, se você definir um novo **clientes RADIUS** modelo no console do NPS em **gerenciamento modelos** em vez de criar um novo cliente RADIUS em **clientes e servidores RADIUS**, você criou um modelo, mas você não alterou a funcionalidade do servidor NPS ainda. Para alterar a funcionalidade do servidor NPS, você deverá aplicar o modelo no local correto no console do NPS.

O procedimento a seguir fornece instruções sobre como criar um novo modelo.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-create-an-nps-template"></a>Para criar um modelo NPS


1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS. 

2. No console do NPS, expanda **gerenciamento modelos**, clique com botão direito um tipo de modelo, como **clientes RADIUS**e clique em **nova**.

3. Uma nova caixa de diálogo de propriedades de modelo é aberta que você pode usar para configurar seu modelo.

## <a name="apply-an-nps-template"></a>Aplicar um modelo NPS

Você pode usar um modelo que você criou no **gerenciamento modelos** navegando até um local no console do NPS onde você pode aplicar o modelo. Por exemplo, se você quiser aplicar um modelo de segredos compartilhados para uma configuração de cliente RADIUS, você pode usar o procedimento a seguir.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-apply-an-nps-template"></a>Para aplicar um modelo NPS

1. No servidor NPS, no Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. Abre o console do NPS.

2. No console do NPS, expanda **clientes e servidores RADIUS**e, em seguida, expanda **clientes RADIUS**.

3. Em **clientes RADIUS**, no painel de detalhes, clique com botão direito do cliente RADIUS ao qual você deseja aplicar o modelo NPS e, em seguida, clique em **propriedades**.

4. Na caixa de diálogo Propriedades caixa para o cliente RADIUS, em **selecionar um modelo de segredos compartilhados existente**, selecione o modelo que você deseja aplicar na lista de modelos.

## <a name="export-or-import-nps-templates"></a>Exportar ou importar modelos NPS

Você pode exportar modelos para uso em outros servidores NPS, ou você pode importar modelos em **gerenciamento modelos** para uso no computador local. 

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-export-or-import-nps-templates"></a>Para exportar ou importar modelos NPS

1. Para exportar modelos NPS, no console do NPS, clique com botão direito **gerenciamento modelos**e clique em **exportar modelos em um arquivo**.

2. Para importar modelos NPS, no console do NPS, clique com botão direito **gerenciamento modelos**e clique em **importar modelos de um computador** ou **importar modelos de um arquivo**.


