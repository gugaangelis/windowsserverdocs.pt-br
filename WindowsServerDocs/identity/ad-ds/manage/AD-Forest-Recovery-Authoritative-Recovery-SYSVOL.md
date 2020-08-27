---
title: Recuperação de floresta do AD-sincronização autoritativa do SYSVOL
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.openlocfilehash: a15d88bf4bf1befa4d65758f319fdf0c35c383a8
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939926"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Recuperação de floresta do AD-executando uma sincronização autoritativa de SYSVOL replicado pelo DFSR

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Há diferentes maneiras de executar uma restauração autoritativa do SYSVOL. Você pode editar o atributo **msDFSR-Options** ou executar uma restauração de estado do sistema usando Wbadmin – authsysvol. Se você tiver a opção de restaurar um backup de estado do sistema (ou seja, estiver restaurando AD DS para a mesma instância de hardware e sistema operacional), usar Wbadmin – authsysvol é mais simples. Mas se você precisar executar uma restauração bare-metal, precisará editar o atributo **msDFSR-Options** .

Use as etapas a seguir para executar uma sincronização autoritativa do SYSVOL (se ele for replicado usando o DFSR) editando o atributo **msDFSR-Options** . Se o SYSVOL for replicado usando o FRS, consulte o [artigo 290762](https://go.microsoft.com/fwlink/?LinkId=148443).

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Para executar uma sincronização autoritativa de SYSVOL replicado pelo DFSR

1. Abra Usuários e Computadores do Active Directory.
2. Clique em **Exibir**e selecione **usuários, contatos, grupos e computadores como contêineres** e **recursos avançados**.

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png)

3. No modo de exibição de árvore, clique em **controladores de domínio**, o nome do DC que você restaurou, **DFSR-LocalSettings**e, em seguida, **volume do sistema de domínio**.

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)

4. No painel de detalhes, clique com o botão direito do mouse em **assinatura SYSVOL**, clique em **Propriedades**e clique em **Editor de atributos**.

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png)

5. Clique em **msDFSR-opções**, clique em **Editar**, digite **1**e clique em **OK**

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png)

6. Clique em **OK** para fechar o editor de atributos.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
