---
title: Recuperação de floresta do AD - executar uma recuperação de servidor completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adds
ms.openlocfilehash: 9cf89c9f4875f602abea89e366cadfba8d0599c3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443017"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Recuperação de floresta do AD - executar uma recuperação de servidor completo 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para executar uma recuperação de servidor completo para o Windows Server 2016, 2012 R2 ou 2012. 

## <a name="active-directory-full-server-recovery"></a>Recuperação de servidor completo do Active Directory

Uma recuperação de servidor completo é necessária se você estiver restaurando para um hardware diferente ou uma instância diferente do sistema operacional. Lembre-se do seguinte:

- O número de unidades em que o servidor de destino precisa ser igual ao número no backup e eles precisam ser o mesmo tamanho ou maior.
- O servidor de destino precisa ser iniciado a partir do DVD do sistema operacional para acessar o **reparar o computador** opção. 
- Se o controlador de domínio está em execução em uma VM do Hyper-V e o backup de destino estiver armazenado em um local de rede, você deve instalar um adaptador de rede herdado. 
- Depois de realizar uma recuperação de servidor completo, você precisará separadamente executar uma restauração autoritativa do SYSVOL, conforme descrito em [recuperação de floresta do AD - executar uma sincronização autoritativa de SYSVOL replicado por DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

Dependendo do cenário, use um dos procedimentos a seguir para executar uma restauração completa. 
  
## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Executar uma restauração completa do servidor com um backup local com a imagem mais recente
  
1. Iniciar a instalação do Windows, especifique o idioma, hora e formato de moeda e opções de teclado e clique em **próxima**. 
2. Clique em **Reparar seu computador**.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3. Clique em **Solucionar problemas**.</br>
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4. Clique em **recuperação de imagem do sistema**.</br>
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5. Clique em **Windows Server 2016**. 
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6. Se você estiver restaurando o backup mais recente de local, clique em **usar a imagem de sistema disponível mais recente (recomendada)** e clique em **próxima**.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7. Você agora terá uma opção para:
   -  Formatar e reparticionar discos
   -  Instalar drivers
   -  Desmarcar a **avançado** recursos de reiniciar automaticamente e a verificação de erros de disco. Elas são habilitadas por padrão.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Clique em **Avançar**.
9. Clique em **concluir**. Você será solicitado perguntando se você tem certeza de que deseja continuar. Clique em **Sim**. 
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. Executar uma restauração autoritativa do SYSVOL, quando isso for concluído, conforme descrito em [recuperação de floresta do AD - executar uma sincronização autoritativa de SYSVOL replicado por DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Executar uma restauração completa do servidor por qualquer imagem local ou remota

1. Iniciar a instalação do Windows, especifique o idioma, hora e formato de moeda e opções de teclado e clique em **próxima**. 
2. Clique em **Reparar seu computador**.</br>
3. Clique em **solução de problemas**, clique em **recuperação da imagem do sistema**e clique em **Windows Server 2016**. 
4. Se você estiver restaurando o backup mais recente de local, clique em **selecionar uma imagem do sistema** e clique em **próxima**.
5. Agora você pode selecionar o local do backup que você deseja restaurar. Se a imagem for local, você pode selecioná-lo da lista. 
6. Se a imagem estiver em um compartilhamento de rede, selecione **avançado**. Você também pode selecionar **avançado** se você precisar instalar um driver.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7. Se você estiver restaurando a partir da rede depois de clicar em **Advanced** selecionar **pesquisar por uma imagem de sistema na rede**. Você será solicitado para restaurar a conectividade de rede. Selecione Okey. </br>
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Digite o caminho UNC para o local de compartilhamento de backup (por exemplo, \\\server1\backups) e clique em **Okey**. Você também pode digitar o endereço IP do servidor de destino, tal como \\\192.168.1.3\backups. 
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
9. Digite as credenciais necessárias para acessar o compartilhamento e clique em Okey. 
10. Agora **selecione a data e hora da imagem do sistema para restaurar** e clique em **próxima**.
11. Você agora terá uma opção para:
    - Formatar e reparticionar discos
    - Instalar drivers
    - Desmarcar a **avançado** recursos de reiniciar automaticamente e a verificação de erros de disco. Elas são habilitadas por padrão.
12. Clique em **Avançar**.
13. Clique em **concluir**. Você será solicitado perguntando se você tem certeza de que deseja continuar. Clique em **Sim**.  
14. Executar uma restauração autoritativa do SYSVOL, quando isso for concluído, conforme descrito em [recuperação de floresta do AD - executar uma sincronização autoritativa de SYSVOL replicado por DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>Habilitando o adaptador de rede para um backup de rede

Se você precisar habilitar um adaptador de rede do prompt de comando restaurar a partir de um compartilhamento de rede, use as etapas a seguir.

1. Iniciar a instalação do Windows, especifique o idioma, hora e formato de moeda e opções de teclado e clique em **próxima**. 
2. Clique em **Reparar seu computador**. I
3. Clique em **solução de problemas**, clique em **Prompt de comando**. 
4. Digite o seguinte comando e pressione ENTER:  

   ```  
   wpeinit  
   ```

5. Para confirmar o nome do adaptador de rede, digite:  

   ```  
   show interfaces  
   ```  

   Digite os comandos a seguir e pressione ENTER após cada comando:  

   ```  
   netsh  
   ```  

   ```  
   interface  
   ```  
  
   ```  
   tcp  
   ```  

   ```  
   ipv4  
   ```  
  
   ```  
   set address "Name of Network Adapter" static IPv4 Address SubnetMask IPv4 Gateway Address 1  
   ```  

   Por exemplo:  
  
   ```  
   set address "Local Area Connection" static 192.168.1.2 255.0.0.0 192.168.1.1 1  
   ```  

   Tipo `quit` para retornar ao prompt de comando. Tipo `ipconfig /all` para verificar se a rede adaptador tem um endereço IP e tente executar ping no endereço IP do servidor que hospeda o compartilhamento de backup para confirmar a conectividade. Quando você terminar, feche o prompt de comando. 

6. Agora que o adaptador de rede está funcionando, selecione as etapas acima para concluir a restauração.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
