---
title: "Recuperação de floresta do AD - capturar uma função de mestre de operações"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adfs
ms.openlocfilehash: 7ca6e3746586feeb3573b1ad6ba02831d1f5addd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Recuperação de floresta do AD - capturar uma função mestre de operações  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para capturar uma função mestre de operações (também conhecido como uma função de operações mestre únicas flexíveis (FSMO)). Você pode usar Ntdsutil.exe, uma ferramenta de linha de comando que é instalada automaticamente em todos os controladores de domínio.  
  
## <a name="to-seize-an-operations-master-role"></a>Para capturar uma função mestre de operações  
  
1.  No prompt de comando, digite o seguinte comando e pressione ENTER:  
  
    ```  
    ntdsutil  
    ```  
  
2.  No **ntdsutil:** prompt, digite o seguinte comando e pressione ENTER:  
  
    ```  
    roles  
    ```  
  
3.  No **manutenção FSMO:** prompt, digite o seguinte comando e pressione ENTER:  
  
    ```  
    connections  
    ```  
  
4.  No **conexões de servidor:** prompt, digite o seguinte comando e pressione ENTER:  
  
    ```  
    Connect to server ServerFQDN  
    ```  
  
     Onde *ServerFQDN* é o nome de domínio totalmente qualificado (FQDN) do controlador de domínio, por exemplo: **se conectar ao servidor nycdc01.example.com**.  
  
     Se *ServerFQDN* não ter sucesso, use o nome NetBIOS do controlador de domínio.  
  
5.  No **conexões de servidor:** prompt, digite o seguinte comando e pressione ENTER:  
  
    ```  
    quit  
    ```  
  
6.  Dependendo da função que você deseja capturar, no **manutenção FSMO:** prompt, digite o comando apropriado, conforme descrito na tabela a seguir e pressione ENTER.  
  
    |Função|Credenciais|Comando|  
    |----------|-----------------|-------------|  
    |Mestre de nomeação de domínio|Administradores corporativos|**Capturar nomenclatura mestre**|  
    |Mestre de esquema|Administradores de esquema|**Capturar mestre de esquema**|  
    |Mestre de infraestrutura **Observação:** depois que você executar a função de mestre de infraestrutura, você pode receber um erro mais tarde se você precisar executar Adprep /Rodcprep. Para obter mais informações, consulte o artigo KB [949257](https://support.microsoft.com/kb/949257).|Administradores de domínio|**Capturar mestre de infraestrutura**|  
    |Mestre emulador do PDC|Administradores de domínio|**Capturar pdc**|  
    |Mestre RID|Administradores de domínio|**Capturar rid mestre**|  
  
     Depois que você confirmar a solicitação, Active Directory ou AD DS tenta transferir a função. Quando a transferência falha, algumas informações de erro é exibida e receita com a captura do Active Directory ou AD DS. Depois que a captura for concluída, aparece uma lista de funções e o nome de Lightweight Directory Access Protocol (LDAP) do servidor que possui cada função no momento. Você também pode executar **Netdom Query FSMO** em um prompt de comando com privilégios elevados para verificar os titulares de função atual.  
  
    > [!NOTE]
    >  Se este computador não era um mestre RID antes da falha e você tentar executar a função mestre RID, o computador tenta sincronizar com um parceiro de replicação antes de aceitar essa função. No entanto, como esta etapa é executada quando o computador é isolado, ele não serão bem-sucedidas em sincronização com um parceiro. Portanto, uma caixa de diálogo é exibida solicitando que você se deseja continuar com a operação Apesar deste computador não está sendo sincronizada com um parceiro. Clique em **Sim**.  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
