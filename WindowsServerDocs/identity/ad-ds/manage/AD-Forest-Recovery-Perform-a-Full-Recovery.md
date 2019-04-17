---
title: "Recuperação de floresta do AD - realizar uma recuperação de servidor completo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.technology: identity-adfs
ms.openlocfilehash: 5de71cc005e9ec4c1425f9fa805b3c043ab4ea36
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Recuperação de floresta do AD - realizar uma recuperação de servidor completo 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
Use o procedimento a seguir para realizar uma recuperação do servidor completo para o Windows Server 2016, 2012 R2 ou 2012. 

## <a name="active-directory-full-server-recovery"></a>Recuperação de servidor completo do Active Directory
Uma recuperação do servidor completo é necessária se você estiver restaurando para um hardware diferente ou uma instância de sistema operacional diferente. Tenha em mente o seguinte:

- O número de unidades em que o servidor de destino precisa ser igual ao número no backup e precisam ser o mesmo tamanho ou maior.
- O servidor de destino precisa ser iniciado a partir do DVD do sistema operacional para acessar o **reparar o computador** opção. 
- Se o destino que DC está em execução em uma máquina virtual no Hyper-V e o backup é armazenado em um local de rede, você deve instalar um adaptador de rede herdado.  
- Depois de realizar uma recuperação do servidor completo, você precisa separadamente executar uma restauração autorizada do SYSVOL, conforme descrito em [recuperação de floresta do AD - realizar uma sincronização autoritativa da SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).


Dependendo do cenário, use um dos procedimentos a seguir para executar uma restauração completa.  
  
### <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Executar uma restauração do servidor completo com um local de backup com a imagem mais recente
  
1.  Iniciar a instalação do Windows, especifique o idioma, hora e o formato de moeda e opções de teclado e clique em **próxima**.  
2.  Clique em **reparar o computador**.</br>
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3.  Clique em **solucionar**.</br>
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4.  Clique em **recuperação da imagem do sistema**.</br>
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5.  Clique em **Windows Server 2016**.  
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6.  Se você for restaurar o backup mais recente de local, clique em **usar a imagem de sistema mais recentes disponíveis (recomendada)** e clique em **próxima**.
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7.  Você agora terá uma opção para:
    -  Formatar e reparticionar discos
    -  Instalar drivers
    -  Desmarcando a **avançado** recursos de reiniciar automaticamente e verificar se há erros de disco.  Estes são habilitados por padrão.
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Clique em **próxima**.
9. Clique em **concluir **.  Você será solicitado perguntando se você tem certeza de que deseja continuar.  Clique em **Sim**.  
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png) 
10. Após a conclusão executar uma restauração autorizada SYSVOL, conforme descrito em [recuperação de floresta do AD - realizar uma sincronização autoritativa da SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).
 

### <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Executar uma restauração do servidor completo com qualquer imagem local ou remota
1.  Iniciar a instalação do Windows, especifique o idioma, hora e o formato de moeda e opções de teclado e clique em **próxima**.  
2.  Clique em **reparar o computador**.</br>
3.  Clique em **solucionar**, clique em **recuperação da imagem do sistema**e clique em **Windows Server 2016**.  
4.  Se você for restaurar o backup mais recente de local, clique em **selecionar uma imagem de sistema** e clique em **próxima**.

5.  Agora você pode selecionar o local do backup que você quer restaurar.  Se a imagem for local, você poderá selecioná-lo na lista.  
6.  Se a imagem estiver em um compartilhamento de rede, selecione **avançado**.  Você também pode selecionar **avançado** se você precisar instalar um driver.
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7.  Se você estiver restaurando da rede depois de clicar em **avançado** selecione **procure uma imagem do sistema na rede**.  Você pode ser solicitado a restaurar a conectividade de rede.  Selecione Okey. </br>
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Digite o caminho UNC para o local de backup de compartilhamento (por exemplo, \\\server1\backups) e clique em **Okey**.  Você também pode digitar o endereço IP do servidor de destino, como \\\192.168.1.3\backups.  
![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
10. Digite as credenciais necessárias para acessar o compartilhamento e clique em Okey.  
11. Agora **selecione a data e hora de imagem do sistema para restaurar** e clique em **próxima**.
12. Você agora terá uma opção para:
    1.   Formatar e reparticionar discos
    2.   Instalar drivers
    3.   Desmarcando a **avançado** recursos de reiniciar automaticamente e verificar se há erros de disco.  Estes são habilitados por padrão.
13. Clique em **próxima**.
14. Clique em **concluir **.  Você será solicitado perguntando se você tem certeza de que deseja continuar.  Clique em **Sim**.   
15. Após a conclusão executar uma restauração autorizada SYSVOL, conforme descrito em [recuperação de floresta do AD - realizar uma sincronização autoritativa da SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).


### <a name="enabling-the-network-adapter-for-a-network-backup"></a>Habilitando o adaptador de rede para um backup de rede
Se você precisar permitir que um adaptador de rede no prompt de comando restaurar a partir de um compartilhamento de rede, use as etapas a seguir.

1.  Iniciar a instalação do Windows, especifique o idioma, hora e o formato de moeda e opções de teclado e clique em **próxima**.  
2.  Clique em **reparar o computador**. Eu
3.  Clique em **solucionar**, clique em **Prompt de comando**.  
4.  Digite o seguinte comando e pressione ENTER:  
  
    ```  
    wpeinit  
    ```   
5.  Para confirmar o nome do adaptador de rede, digite:  
  
    ```  
    show interfaces  
    ```  
  
     Digite os seguintes comandos e pressione ENTER após cada comando:  
  
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
  
     Tipo `quit`para retornar ao prompt de comando. Tipo `ipconfig /all`para verificar a rede adaptador tem um endereço IP e tente fazer isso com o endereço IP do servidor que hospeda o compartilhamento de backup para confirmar a conectividade. Quando terminar, feche o prompt de comando.  
  
6.  Agora que o adaptador de rede está funcionando, selecione as etapas acima para concluir a restauração.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
