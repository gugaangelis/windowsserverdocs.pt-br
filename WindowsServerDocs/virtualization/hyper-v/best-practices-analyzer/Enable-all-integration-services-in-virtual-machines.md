---
title: Habilitar todos os serviços de integração em máquinas virtuais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 16e202ad-3795-40c9-8176-7ca319e56d26
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d1a3fd16be0d85f185efb9ceb9d7b2f690eaae47
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969973"
---
# <a name="enable-all-integration-services-in-virtual-machines"></a>Habilitar todos os serviços de integração em máquinas virtuais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*Um ou mais serviços de integração estão desabilitados ou não estão funcionando em uma máquina virtual.*

## <a name="impact"></a>Impacto

*O serviço ou o recurso de integração pode não funcionar corretamente para as seguintes máquinas virtuais:*

\<list of virtual machine names>

## <a name="resolution"></a>Resolução

*Use o snap-in de serviços ou a ferramenta de linha de comando sc config para verificar se o serviço está configurado para iniciar automaticamente e não está parado.*

#### <a name="to-configure-how-a-service-is-started-using-the-services-snap-in"></a>Para configurar como um serviço é iniciado usando o snap-in de serviços

1.  Use Serviços de Área de Trabalho Remota ou conexão de máquina virtual para se conectar à máquina virtual e fazer logon no sistema operacional convidado.

2.  Abrir Serviços. (Clique em **Iniciar**, clique na caixa **Iniciar pesquisa** , digite **Services. msc**e pressione Enter.)

3.  No painel de detalhes, clique com o botão direito no serviço que deseja configurar e clique em **Propriedades**.

4.  Na guia **geral** , em tipo de **inicialização** , clique em **automático**.

#### <a name="to-configure-how-a-service-is-started-using-sc-config"></a>Para configurar como um serviço é iniciado usando SC config

1.  Abra o Windows PowerShell. (Na área de trabalho, clique em **Iniciar** e comece a digitar **Windows PowerShell**.)

2.  Clique com o botão direito do mouse em **Windows PowerShell** e clique em **Executar como administrador**.

3.  Substitua <Service-Name> pelo nome do serviço e, em seguida, digite:

    ```
    sc config <service-name> start=auto
    ```



