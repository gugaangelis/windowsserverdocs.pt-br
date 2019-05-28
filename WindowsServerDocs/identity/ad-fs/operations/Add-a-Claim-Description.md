---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: Adicionar uma descrição da declaração
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 454d261aa520778a6129ac9809f53894937b036a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190143"
---
# <a name="add-a-claim-description"></a>Adicionar uma descrição da declaração


Em uma organização do parceiro de conta, os administradores criam declarações para representar a associação de um usuário em um grupo ou uma função ou para representar alguns dados sobre um usuário, por exemplo, um usuário funcionário número de identificação.

Em uma organização de parceiro de recurso, os administradores criam declarações correspondentes para representar grupos e usuários que podem ser reconhecidos como usuários de recursos. Porque a saída de declarações no mapa de organização do parceiro de conta para declarações de entrada na organização do parceiro de recurso, o parceiro de recurso é capaz de aceitar as credenciais que o parceiro de conta fornece. 

Você pode usar o procedimento a seguir para adicionar uma declaração.

A associação a **Administradores**, ou equivalente, no computador local é o requisito mínimo para concluir esse procedimento.  Examine os detalhes sobre como usar as contas apropriadas e associações de grupos em [domínio grupos padrão Local e](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Para adicionar uma descrição de declaração

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, selecione **gerenciamento do AD FS**. 

2.  Expandir **Service** e, em que o botão direito do mouse **adicionar descrição de declaração**.
![Adicionar descrição de declaração](media\Add-a-Claim-Description\claimdesc1.png)

3.  Na caixa Adicionar uma descrição de declaração na caixa **nome de exibição**, digite um nome exclusivo que identifica o grupo ou função para esta declaração.

4.  Adicionar um **abreviada nome**.

5.  Na **identificador de declaração**, digite um URI que é associado um grupo ou função da declaração que você usará.

6.  Sob **descrição**, digite o texto que melhor descreve a finalidade dessa declaração.

7.  Dependendo das necessidades da sua organização, selecione qualquer uma das seguintes caixas de seleção, conforme apropriado, para publicar esta declaração nos metadados de Federação:


    - Para publicar esta declaração de parceiros cientes de que esse servidor pode aceitar essa declaração, clique em **publicar esta declaração nos metadados da federação como um tipo de declaração que este serviço de Federação pode aceitar**.
    - Para publicar esta declaração de parceiros cientes de que esse servidor pode emitir essa declaração, clique em **publicar esta declaração nos metadados da federação como um tipo de declaração que este serviço de Federação pode enviar**.

8.  Clique em **OK**.

![Adicionar descrição de declaração](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
