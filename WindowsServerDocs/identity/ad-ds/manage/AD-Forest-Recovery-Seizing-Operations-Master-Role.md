---
title: Recuperação de floresta do AD-capturando uma função de mestre de operações
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.openlocfilehash: 7d7b1abfaf7e3ed4f3780ff2d819340ba8fe98c0
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941526"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Recuperação de floresta do AD-capturando uma função de mestre de operações

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para executar uma função de mestre de operações (também conhecida como função FSMO (operações mestras únicas) flexíveis). Você pode usar Ntdsutil.exe, uma ferramenta de linha de comando que é instalada automaticamente em todos os DCs.

## <a name="to-seize-an-operations-master-role"></a>Para executar uma função de mestre de operações

1. No prompt de comando, digite o seguinte comando e pressione ENTER:

   ```
   ntdsutil
   ```

2. No prompt **Ntdsutil:** , digite o seguinte comando e pressione ENTER:

   ```
   roles
   ```

3. No prompt **manutenção do FSMO:** , digite o seguinte comando e pressione ENTER:

   ```
   connections
   ```

4. No prompt **conexões do servidor:** , digite o seguinte comando e pressione ENTER:

   ```
   Connect to server ServerFQDN
   ```

   Em que *ServerFQDN* é o FQDN (nome de domínio totalmente qualificado) desse DC, por exemplo: **conectar-se ao servidor nycdc01.example.com**.

   Se *ServerFQDN* não tiver sucesso, use o nome NetBIOS do controlador de domínio.

5. No prompt **conexões do servidor:** , digite o seguinte comando e pressione ENTER:

   ```
   quit
   ```

6. Dependendo da função que você deseja executar, no prompt de manutenção do **FSMO:** , digite o comando apropriado, conforme descrito na tabela a seguir, e pressione Enter.

|Função|Credenciais|Comando|
|----------|-----------------|-------------|
|Mestre de nomeação de domínio|Administradores Corporativos|**Capturar o mestre de nomenclatura**|
|Mestre de esquema|Administradores de esquemas|**Capturar o mestre de esquema**|
|Observação mestre de infraestrutura **:**  depois de executar a função de mestre de infraestrutura, você poderá receber um erro mais tarde se precisar executar adprep/rodcprep. Para obter mais informações, consulte o artigo [949257](https://support.microsoft.com/kb/949257)da base de dados de conhecimento.|Administradores de Domínio|**Capturar mestre de infraestrutura**|
|Mestre do emulador PDC|Administradores de Domínio|**Capturar PDC**|
|Mestre de RID do |Administradores de Domínio|**Capturar mestre RID**|

Depois de confirmar a solicitação, Active Directory ou AD DS tentar transferir a função. Quando a transferência falha, algumas informações de erro são exibidas e Active Directory ou AD DS prossegue com a captura. Após a conclusão da captura, será exibida uma lista das funções e o nome do protocolo LDAP do servidor que contém atualmente cada função. Você também pode executar **netdom query fsmo** em um prompt de comandos com privilégios elevados para verificar os detentores de função atuais.

> [!NOTE]
> Se este computador não foi um mestre RID antes da falha e você tentar executar a função mestre RID, o computador tentará sincronizar com um parceiro de replicação antes de aceitar essa função. No entanto, como essa etapa é executada quando o computador está isolado, ele não terá sucesso na sincronização com um parceiro. Portanto, uma caixa de diálogo é exibida perguntando se você deseja continuar com a operação, apesar deste computador não poder ser sincronizado com um parceiro. Clique em **Sim**.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
