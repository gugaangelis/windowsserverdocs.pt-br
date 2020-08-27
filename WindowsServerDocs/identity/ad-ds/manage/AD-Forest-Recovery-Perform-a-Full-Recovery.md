---
title: Recuperação de floresta do AD-executando uma recuperação completa do servidor
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 1a1182a6-4462-4a13-806e-0e642a0d5db2
ms.openlocfilehash: 189cbf826af3f5cf32eb799d86c04c6d0f03d051
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939736"
---
# <a name="ad-forest-recovery---performing-a-full-server-recovery"></a>Recuperação de floresta do AD-executando uma recuperação completa do servidor

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Use o procedimento a seguir para executar uma recuperação completa do servidor para o Windows Server 2016, 2012 R2 ou 2012.

## <a name="active-directory-full-server-recovery"></a>Active Directory recuperação de servidor completo

Uma recuperação de servidor completa será necessária se você estiver restaurando para um hardware diferente ou uma instância diferente do sistema operacional. Lembre-se do seguinte:

- O número de unidades no servidor de destino precisa ser igual ao número no backup e precisa ter o mesmo tamanho ou maior.
- O servidor de destino precisa ser iniciado no DVD do sistema operacional para acessar a opção **reparar o computador** .
- Se o controlador de domínio de destino estiver sendo executado em uma VM no Hyper-V e o backup estiver armazenado em um local de rede, você deverá instalar um adaptador de rede herdado.
- Depois de executar uma recuperação completa do servidor, você precisa executar separadamente uma restauração autoritativa do SYSVOL, conforme descrito em [recuperação da floresta do AD, executando uma sincronização autoritativa de SYSVOL replicado pelo DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

Dependendo do seu cenário, use um dos procedimentos a seguir para executar uma restauração completa.

## <a name="perform-a-full-server-restore-with-a-local-backup-with-the-latest-image"></a>Executar uma restauração completa do servidor com um backup local com a imagem mais recente

1. Inicie Instalação do Windows, especifique o idioma, o formato de hora e de moeda e as opções de teclado e clique em **Avançar**.
2. Clique em **Reparar seu computador**.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore1.png)
3. Clique em **Solucionar problemas**.</br>
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore2.png)
4. Clique em **recuperação de imagem do sistema**.</br>
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore3.png)
5. Clique em **Windows Server 2016**.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore4.png)
6. Se você estiver restaurando o backup local mais recente, clique em **usar a imagem de sistema mais recente disponível (recomendado)** e clique em **Avançar**.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore5.png)
7. Agora você terá uma opção para:
   -  Formatar e reparticionar discos
   -  Instalar drivers
   -  Desmarcando os recursos **avançados** de reinicialização automática e verificação de erros de disco. Essas regras são habilitadas por padrão.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore6.png)
8. Clique em **Próximo**.
9. Clique em **Concluir**. Você será solicitado a perguntar se tem certeza de que deseja continuar. Clique em **Sim**.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore11.png)
10. Quando isso for concluído, execute uma restauração autoritativa do SYSVOL, conforme descrito em [recuperação de floresta do AD, executando uma sincronização autoritativa de SYSVOL replicado pelo DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="perform-a-full-server-restore-with-any-image-local-or-remote"></a>Executar uma restauração completa do servidor com qualquer imagem local ou remota

1. Inicie Instalação do Windows, especifique o idioma, o formato de hora e de moeda e as opções de teclado e clique em **Avançar**.
2. Clique em **Reparar seu computador**.</br>
3. Clique em **solucionar problemas**, clique em **recuperação de imagem do sistema**e clique em **Windows Server 2016**.
4. Se você estiver restaurando o backup local mais recente, clique em **selecionar uma imagem do sistema** e clique em **Avançar**.
5. Agora você pode selecionar o local do backup que deseja restaurar. Se a imagem for local, você poderá selecioná-la na lista.
6. Se a imagem estiver em um compartilhamento de rede, selecione **avançado**. Você também pode selecionar **avançado** se precisar instalar um driver.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore7.png)
7. Se você estiver restaurando da rede depois de clicar em **avançado** , selecione **procurar uma imagem do sistema na rede**. Você pode ser solicitado a restaurar a conectividade de rede. Selecione OK. </br>
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore8.png)
8. Digite o caminho UNC para o local do compartilhamento de backup (por exemplo, \\ \server1\backups) e clique em **OK**. Você também pode digitar o endereço IP do servidor de destino, como \\ \192.168.1.3\backups.
   ![Restauração do servidor](media/AD-Forest-Recovery-Perform-a-Full-Recovery/restore9.png)
9. Digite as credenciais necessárias para acessar o compartilhamento e clique em OK.
10. Agora, **Selecione a data e a hora da imagem do sistema a ser restaurada** e clique em **Avançar**.
11. Agora você terá uma opção para:
    - Formatar e reparticionar discos
    - Instalar drivers
    - Desmarcando os recursos **avançados** de reinicialização automática e verificação de erros de disco. Essas regras são habilitadas por padrão.
12. Clique em **Próximo**.
13. Clique em **Concluir**. Você será solicitado a perguntar se tem certeza de que deseja continuar. Clique em **Sim**.
14. Quando isso for concluído, execute uma restauração autoritativa do SYSVOL, conforme descrito em [recuperação de floresta do AD, executando uma sincronização autoritativa de SYSVOL replicado pelo DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="enabling-the-network-adapter-for-a-network-backup"></a>Habilitando o adaptador de rede para um backup de rede

Se você precisar habilitar um adaptador de rede do prompt de comando para restaurar de um compartilhamento de rede, use as etapas a seguir.

1. Inicie Instalação do Windows, especifique o idioma, o formato de hora e de moeda e as opções de teclado e clique em **Avançar**.
2. Clique em **Reparar seu computador**. I
3. Clique em **solucionar problemas**, clique em **prompt de comando**.
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

   Digite `quit` para retornar a um prompt de comando. Digite `ipconfig /all` para verificar se o adaptador de rede tem um endereço IP e tente executar o ping no endereço IP do servidor que hospeda o compartilhamento de backup para confirmar a conectividade. Feche o prompt de comando quando terminar.

6. Agora que o adaptador de rede está funcionando, selecione as etapas acima para concluir a restauração.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
