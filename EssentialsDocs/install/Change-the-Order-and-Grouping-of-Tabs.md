---
title: Alterar a ordem e o agrupamento de guias
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79a417fd-1b3e-47ab-ae33-bb1faf95c86d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 578c5619cfdf076bb2735254494f393d56d35713
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-order-and-grouping-of-tabs"></a>Alterar a ordem e o agrupamento de guias

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Você pode alterar a ordem das guias no painel para que sua guia seja a primeira (à esquerda) na linha de guias. Para fazer isso, você adicionar uma entrada ao registro. Você também pode afetar o agrupamento de guias adicionando entradas ao registro. A ordem das guias pode ser sua guia principal seguido por guias internas da Microsoft, seguidas de qualquer uma de suas guias adicionais e, em seguida, seguido pelos guias de terceiros.  
  
## <a name="change-the-order-of-the-tabs-in-the-dashboard"></a>Alterar a ordem das guias no painel  
 Você deve adicionar o identificador do suplemento que exibe sua guia no registro para definir a ordem.  
  
#### <a name="to-display-your-tab-first-in-the-list-of-tabs"></a>Para exibir sua guia pela primeira vez na lista de guias  
  
1.  No computador de referência, clique em **iniciar**, insira **regedit**e pressione **Enter**.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**e, em seguida, expanda **Windows Server**. Se o **OEM** chave não existir, você deve concluir as seguintes etapas para criá-lo:  
  
    1.  Clique com botão direito **Windows Server**, aponte para **nova**e clique em **chave**.  
  
    2.  Tipo **OEM** para o nome da chave.  
  
3.  Clique com botão direito **OEM**, aponte para **nova**e clique em **valor de cadeia de caracteres**.  
  
4.  Tipo **DashboardMainTabID** como o nome de cadeia de caracteres e pressione **Enter**.  
  
5.  Clique com botão direito na nova cadeia no painel direito e clique em **modificar**.  
  
6.  Digite o GUID que foi definido para a guia de nível superior e, em seguida, pressione **Enter**.  
  
     Para obter mais informações sobre como criar e identificar guias de nível superior, consulte [criar uma guia de nível superior](https://msdn.microsoft.com/library/gg513957) no SDK do Windows Server Solutions.  
  
7.  Salve as alterações no registro.  
  
8.  Você também deve incluir o GUID para sua guia principal de nível superior na lista de identificadores para agrupar guias. Para fazer isso, execute as etapas listadas em **alterar o agrupamento de guias no painel**.  
  
## <a name="change-the-grouping-of-tabs-in-the-dashboard"></a>Alterar o agrupamento de guias no painel  
 Você pode garantir que suas guias são agrupadas e incluídas na lista de guias internas da Microsoft, adicionando os identificadores ao registro.  
  
#### <a name="to-change-the-grouping-of-tabs"></a>Para alterar o agrupamento de guias  
  
1.  Se regedit não estiver aberto, clique em **iniciar**, insira **regedit**e pressione **Enter**.  
  
2.  No painel esquerdo, expanda **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**e, em seguida, expanda **Windows Server**.  
  
3.  Clique com botão direito **OEM**, aponte para **nova**e clique em **chave**.  
  
4.  Tipo **DashboardAddins** como o nome da chave e pressione **Enter**.  
  
5.  Clique com botão direito **DashboardAddins**, aponte para **nova**e clique em **valor de cadeia de caracteres**.  
  
6.  Digite o identificador GUID para sua guia como o nome de cadeia de caracteres. Um valor não é necessário.  
  
7.  Repita as etapas 5 e 6 para cada uma das guias e subguias.  
  
8.  Salve as alterações no registro.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)