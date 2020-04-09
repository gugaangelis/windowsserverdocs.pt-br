---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: Adicionar uma descrição da declaração
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9c5293dc6070c9483054ce1dd827a20ec377573b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859709"
---
# <a name="add-a-claim-description"></a>Adicionar uma descrição da declaração


Em uma organização de parceiro de conta, os administradores criam declarações para representar a associação de um usuário em um grupo ou função ou para representar alguns dados sobre um usuário, por exemplo, o número de identificação de funcionário de um usuário.

Em uma organização de parceiro de recurso, os administradores criam declarações correspondentes para representar grupos e usuários que podem ser reconhecidos como usuários de recursos. Como as declarações de saída na organização do parceiro de conta mapeiam para declarações de entrada na organização do parceiro de recurso, o parceiro de recurso é capaz de aceitar as credenciais que o parceiro de conta fornece. 

Você pode usar o procedimento a seguir para adicionar uma declaração.

A associação em **Administradores**, ou equivalente, no computador local é o mínimo necessário para concluir este procedimento.  Examine os detalhes sobre como usar as contas apropriadas e as associações de grupo em [grupos padrão e de domínio](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Para adicionar uma descrição de declaração

1. Em Gerenciador do Servidor, clique em **ferramentas**e, em seguida, selecione **Gerenciamento de AD FS**. 

2. Expanda **serviço** e clique com o botão direito do mouse em **Adicionar Descrição da declaração**.
   ![adicionar descrição de declaração](media/Add-a-Claim-Description/claimdesc1.png)

3. Na caixa de diálogo Adicionar uma descrição de declaração, em **nome de exibição**, digite um nome exclusivo que identifique o grupo ou a função dessa declaração.

4. Adicione um **nome curto**.

5. Em **identificador de declaração**, digite um URI que esteja associado ao grupo ou função da declaração que você usará.

6. Em **Descrição**, digite o texto que melhor descreve a finalidade dessa declaração.

7. Dependendo das necessidades da sua organização, marque uma das seguintes caixas de seleção, conforme apropriado, para publicar essa declaração em metadados de Federação:


~~~
- To publish this claim to make partners aware that this server can accept this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can accept**.
- To publish this claim to make partners aware that this server can issue this claim, click **Publish this claim in federation metadata as a claim type that this Federation Service can send**.
~~~

8. Clique em **OK**.

![Adicionar Descrição da declaração](media/Add-a-Claim-Description/claimdesc2.png)


## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
