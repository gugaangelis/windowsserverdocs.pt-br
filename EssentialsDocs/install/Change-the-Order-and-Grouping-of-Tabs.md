---
title: Alterar a Ordem e o Agrupamento de Guias
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 5a52f4b14423d1ef337cba9c2d18dbf4c196ca93
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623877"
---
# <a name="change-the-order-and-grouping-of-tabs"></a>Alterar a Ordem e o Agrupamento de Guias

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

É possível alterar a ordem das guias no Dashboard para que sua guia seja a primeira (à esquerda) na linha de guias. Para fazer isso, adicione uma entrada no Registro. Também é possível afetar o agrupamento de guias adicionando entradas ao Registro. A ordem das guias pode iniciar pela guia principal seguida pelas guias internas da Microsoft, seguidas por quaisquer das suas guias adicionais e, finalmente, pelas guias de terceiros.

## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>Alterar a ordem das guias do Dashboard
 É preciso adicionar o identificador do suplemento que exibe sua guia ao Registro a fim de definir a ordem.

#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>Para exibir sua guia primeiro na lista de guias

1.  No computador de referência, clique em **Iniciar**, digite **regedit** e pressione **Enter**.

2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Se a chave **OEM** não existir, é necessário concluir as seguintes etapas para criá-la:

    1.  Clique com o botão direito do mouse em **Windows Server**, aponte para **Novo** e clique em **Chave**.

    2.  Digite **OEM** para o nome da chave.

3.  Clique com o botão direito do mouse em **OEM**, aponte para **Novo** e clique em **Valor da Cadeia de Caracteres**.

4.  Digite **DashboardMainTabID** como o nome da cadeia de caracteres e pressione **Enter**.

5.  Clique com o botão direito do mouse na nova cadeia de caracteres no painel à direita e clique em **Modificar**.

6.  Digite o GUID definido para a guia de nível superior e pressione **Enter**.

     Para obter mais informações sobre a criação e a identificação de guias de nível superior, consulte [Create a Top-Level Tab (Criar guia de nível superior)](/previous-versions/windows/server-essentials/gg513957(v=msdn.10)) no SDK do Windows Server Solutions.

7.  Salve as alterações no Registro.

8.  É necessário incluir também o GUID para a guia principal de nível superior na lista de identificadores para guias agrupadas. Para isso, execute as etapas listadas em **Alterar o agrupamento de guias no Dashboard**.

## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>Alterar o agrupamento de guias no Dashboard
 É possível garantir que as guias estão agrupadas e incluídas na lista de guias internas da Microsoft adicionando os identificadores ao Registro.

#### <a name="to-change-the-grouping-of-tabs"></a>Para alterar o agrupamento de guias

1.  Se regedit não estiver aberto, clique em **Iniciar**, digite **regedit** e pressione **Enter**.

2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**.

3.  Clique com o botão direito do mouse em **OEM**, aponte para **Novo** e clique em **Chave**.

4.  Digite **DashboardAddins** como o nome da chave e pressione **Enter**.

5.  Clique com o botão direito do mouse em **DashboardAddins**, aponte para **Novo** e clique em **Valor da Cadeia de Caracteres**.

6.  Digite o identificador GUID de sua guia como o nome de cadeia de caracteres. Um valor não é necessário.

7.  Repita as etapas 5 e 6 para cada uma de suas guias e subguias.

8.  Salve as alterações no Registro.

## <a name="see-also"></a>Consulte Também
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)