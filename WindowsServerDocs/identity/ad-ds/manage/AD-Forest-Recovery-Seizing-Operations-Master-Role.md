---
title: Recuperação de floresta do AD-capturando uma função de mestre de operações
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: b229215eb7dde23bd1c17e6023b1c5eace0a56bf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823529"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Recuperação de floresta do AD-capturando uma função de mestre de operações  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para executar uma função de mestre de operações (também conhecida como função FSMO (operações mestras únicas) flexíveis). Você pode usar o Ntdsutil. exe, uma ferramenta de linha de comando que é instalada automaticamente em todos os DCs.  
  
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
  
|Função|Credenciais|{1&gt;Comando&lt;1}|  
|----------|-----------------|-------------|  
|Mestre de nomeação de domínio|Administrador corporativo|**Capturar o mestre de nomenclatura**|  
|Mestre de esquema|Administradores de esquemas|**Capturar o mestre de esquema**|  
|Observação mestre de infraestrutura **:** depois de executar a função de mestre de infraestrutura, você poderá receber um erro mais tarde se precisar executar adprep/rodcprep. Para obter mais informações, consulte o artigo [949257](https://support.microsoft.com/kb/949257)da base de dados de conhecimento.|Administradores do domínio|**Capturar mestre de infraestrutura**|  
|Mestre do emulador PDC|Administradores do domínio|**Capturar PDC**|  
|Mestre RID|Administradores do domínio|**Capturar mestre RID**|  

Depois de confirmar a solicitação, Active Directory ou AD DS tentar transferir a função. Quando a transferência falha, algumas informações de erro são exibidas e Active Directory ou AD DS prossegue com a captura. Após a conclusão da captura, será exibida uma lista das funções e o nome do protocolo LDAP do servidor que contém atualmente cada função. Você também pode executar **netdom query fsmo** em um prompt de comandos com privilégios elevados para verificar os detentores de função atuais.  
  
> [!NOTE]
> Se este computador não foi um mestre RID antes da falha e você tentar executar a função mestre RID, o computador tentará sincronizar com um parceiro de replicação antes de aceitar essa função. No entanto, como essa etapa é executada quando o computador está isolado, ele não terá sucesso na sincronização com um parceiro. Portanto, uma caixa de diálogo é exibida perguntando se você deseja continuar com a operação, apesar deste computador não poder ser sincronizado com um parceiro. Clique em **Sim**.  
  
## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
