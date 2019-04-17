---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: "Adicionar uma descrição de declaração"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0e388ef656d3b690da62b077cb9f9e678a771e64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-claim-description"></a>Adicionar uma descrição de declaração

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Em uma organização de parceiro de conta, os administradores criar declarações para representar a participação do usuário em um grupo ou função ou para representar alguns dados sobre um usuário, por exemplo, um usuário funcionário número de identificação.

Em uma organização de parceiro de recurso, os administradores criar declarações correspondentes para representar grupos e os usuários que podem ser reconhecidos como usuários do recurso. Como saída declarações no mapa de organização de parceiro de conta para declarações de entrada na organização do parceiro de recurso, o parceiro de recurso é capaz de aceitar as credenciais que fornece o parceiro de conta. 

Você pode usar o procedimento a seguir para adicionar uma reivindicação.

A associação ao grupo **administradores**, ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.  Examinar detalhes sobre como usar as contas apropriadas e agrupar associações em [Local e os grupos de domínio padrão ](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Para adicionar uma descrição de declaração

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **AD FS gerenciamento **. 

2.  Expanda **serviço** e sobre o botão direito do mouse **adicionar reivindicação descrição**.
![Adicione a declaração de descrição](media\Add-a-Claim-Description\claimdesc1.png)

3.  Em Adicionar uma caixa de diálogo reivindicar descrição na caixa **nome de exibição**, digite um nome exclusivo que identifica o grupo ou a função para essa declaração.

4.  Adicione um **curtas nome**.

5.  Em **reivindicar identificador**, digite um URI que está associado com o grupo ou a função da reivindicação que você usará.

6.  Em **descrição**, digite o texto que melhor descreve a finalidade desta declaração.

7.  Dependendo das necessidades da sua organização, selecione uma das caixas de seleção a seguir, conforme apropriado, para publicar dessa declaração em metadados de Federação:


    - Para publicar esta declaração para tornar ciente de que esse servidor pode aceitar essa declaração de parceiros, clique em **publicar dessa declaração nos metadados de federação como um tipo de declaração que esse serviço de Federação pode aceitar**.
    - Para publicar esta declaração para tornar ciente de que esse servidor pode emitir dessa declaração de parceiros, clique em **publicar dessa declaração nos metadados de federação como um tipo de declaração que esse serviço de Federação pode enviar**.

8.  Clique em **Okey**.

![Adicione a declaração de descrição](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
