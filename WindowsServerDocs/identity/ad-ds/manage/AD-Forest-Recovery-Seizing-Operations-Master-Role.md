---
title: Recuperação de floresta do AD - captura de uma função de mestre de operações
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 1994d49652ee9eb10f6afc73cf5b4630b4718e77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858457"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Recuperação de floresta do AD - captura de uma função de mestre de operações  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para executar uma função de mestre de operações (também conhecido como uma função FSMO flexible single master operations ()). Você pode usar Ntdsutil.exe, uma ferramenta de linha de comando que é instalada automaticamente em todos os controladores de domínio.  
  
## <a name="to-seize-an-operations-master-role"></a>Para executar uma função de mestre de operações  
  
1. No prompt de comando, digite o seguinte comando e pressione ENTER:  

   ```  
   ntdsutil  
   ```  

2. Com o **ntdsutil:** prompt, digite o seguinte comando e pressione ENTER:  

   ```  
   roles  
   ```  

3. Com o **FSMO maintenance:** prompt, digite o seguinte comando e pressione ENTER:  

   ```  
   connections  
   ```  

4. Com o **conexões de servidor:** prompt, digite o seguinte comando e pressione ENTER:  

   ```  
   Connect to server ServerFQDN  
   ```  

   Em que *ServerFQDN* é o nome de domínio totalmente qualificado (FQDN) do controlador de domínio, por exemplo: **conectar ao servidor nycdc01.example.com**.  

   Se *ServerFQDN* não ter êxito, use o nome NetBIOS do controlador de domínio.  

5. Com o **conexões de servidor:** prompt, digite o seguinte comando e pressione ENTER:  

   ```  
   quit  
   ```  

6. Dependendo da função que você deseja executar, nos **FSMO maintenance:** prompt, digite o comando apropriado, conforme descrito na tabela a seguir e pressione ENTER.  
  
|Função|Credenciais|Comando|  
|----------|-----------------|-------------|  
|Mestre de nomeação de domínio|Administrador corporativo|**Capturar o mestre de nomeação**|  
|Mestre de esquema|Administradores de esquemas|**Capturar o mestre de esquema**|  
|Mestre de infraestrutura **Observação:**  Depois de você executar a função de mestre de infraestrutura, você pode receber um erro posteriormente se você precisar executar Adprep /Rodcprep. Para obter mais informações, consulte o artigo KB [949257](https://support.microsoft.com/kb/949257).|Administradores do domínio|**Capturar o mestre de infraestrutura**|  
|Mestre emulador PDC|Administradores do domínio|**Capturar pdc**|  
|Mestre de RID do|Administradores do domínio|**Capturar o mestre de rid**|  

Depois de confirmar a solicitação, Active Directory ou AD DS tenta transferir a função. Quando a transferência falhar, algumas informações de erro é exibida e Active Directory ou AD DS continua com a tomada de controle. Após concluir a tomada de controle, é exibida uma lista das funções e o nome de Lightweight Directory Access Protocol (LDAP) do servidor que contém cada função no momento. Você também pode executar **Netdom Query FSMO** em um prompt de comando elevado para verificar os detentores de função atual.  
  
> [!NOTE]
> Se este computador não era um mestre de RID antes da falha e você tentar executar a função de mestre de RID, o computador tenta sincronizar com um parceiro de replicação antes de aceitar essa função. No entanto, porque essa etapa é executada quando o computador estiver isolado, ele não terá êxito na sincronização com um parceiro. Portanto, uma caixa de diálogo é exibida perguntando se deseja continuar com a operação, apesar deste computador não conseguir sincronizar com um parceiro. Clique em **Sim**.  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
