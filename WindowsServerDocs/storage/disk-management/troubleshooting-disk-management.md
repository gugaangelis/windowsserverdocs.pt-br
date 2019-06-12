---
title: Solução de problemas do Gerenciamento de Disco
description: Este artigo descreve como solucionar problemas de Gerenciamento de disco
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4d9448cc642ef522fa129dcfe97e2286f16bad1b
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812537"
---
# <a name="troubleshooting-disk-management"></a>Solução de problemas do Gerenciamento de Disco

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico lista alguns dos problemas comuns que você poderá encontrar ao usar o Gerenciamento de disco.

> [!TIP]
> Se você receber um erro ou se algo não funciona ao executar estes procedimentos - não entre em pânico! Há uma tonelada de informações o [comunidade da Microsoft](https://answers.microsoft.com/en-us/windows) do site - tente pesquisar o [arquivos, pastas e armazenamento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) seção e, se você ainda precisar de Ajuda, poste uma pergunta lá e a Microsoft ou outros membros do comunidade tentará ajudar. Se você tiver comentários sobre como melhorar esses tópicos, adoraríamos ouvir sua opinião! Responder apenas a *esta página é útil?* prompt e deixar comentários lá ou no thread de comentários público na parte inferior deste tópico.

## <a name="a-disks-status-is-not-initialized-or-the-disk-is-missing"></a>Status de um disco não é inicializado ou o disco está ausente

![Gerenciamento de disco mostrando um disco desconhecido que deve ser inicializado.](media/uninitialized-disk.PNG)

**Causa:** Se você tiver um disco que não aparece no Explorador de arquivos e é listado no gerenciamento de disco como *não inicializada*, pode ser porque o disco não tem uma assinatura de disco válido. Basicamente, isso significa que o disco nunca foi inicializado e formatado ou a formatação da unidade se tornaram corrompida alguma forma. 

Também é possível que o disco está tendo problemas de hardware ou de conectar, mas falaremos sobre que em apenas alguns parágrafos.

**Solução:**   se a unidade é completamente nova e apenas precisa ser inicializado, apagando todos os dados nele, a solução é fácil – veja [inicializar novos discos](initialize-new-disks.md). No entanto, há uma boa chance de que você já tentou isso e não funcionou. Ou talvez você tenha um disco cheio de arquivos importantes, e você não deseja apagar o disco pelo inicializá-la.

Há vários motivos, um disco pode estar faltando ou não seja inicializado com um sendo comuns de razão porque o disco está falhando. Há somente muito mais você pode fazer para corrigir o tipo de disco, mas aqui estão algumas etapas para testar ver se podemos pode fazê-lo funcionar novamente. Se o disco funciona depois de uma destas etapas, não se preocupar com as próximas etapas, basta voltar, celebrar e atualizar talvez seus backups.

1. Examinar o disco no gerenciamento de disco. Se ela aparecer *Offline* conforme mostrado aqui, tente direito do mouse e selecionando **Online**.

    ![Disco mostrado como offline](media/offline-disk.png)
2. Se o disco aparece no gerenciamento de disco como *Online*, e tem uma partição primária que está listada como *Íntegro*, conforme mostrado aqui, que é um bom sinal.

    ![Disco mostrado como online com um volume Íntegro](media/healthy-volume.png)
    - Se a partição tiver um sistema de arquivos, mas sem letra de unidade (por exemplo, e:), consulte [alterar uma letra de unidade](change-a-drive-letter.md) para adicionar uma letra de unidade manualmente.
    - Se ele não tem um sistema de arquivos (NTFS, ReFS, FAT32 ou exFAT) e você souber que o disco estiver vazio, a partição com o botão direito e selecione **formato**. Formatação de um disco apaga todos os dados nele, portanto, não faça isso se você está tentando recuperar arquivos do disco - em vez disso, pule para a próxima etapa.
3. Se você tiver um externo de disco, desconecte o disco, conectá-lo de volta e, em seguida, selecione **ação** > **examinar discos**. 
4. Desligar o PC, desligar o disco rígido externo (se for um disco externo com um cabo de alimentação) e, em seguida, ativar seu PC e o disco novamente.
    Para desativar seu PC no Windows 10, selecione o botão Iniciar, selecione o botão de energia e, em seguida, selecione **desligar**.
5. Conecte o disco uma porta USB diferente que está diretamente em seu computador (não em um hub).
    Às vezes, os discos USB não obter capacidade suficiente de algumas portas ou ter outros problemas com as portas específicas. Isso é especialmente comum com hubs USB, mas às vezes, há diferenças entre portas em um PC, portanto, tente algumas portas diferentes se você os tiver.
6. Tente um cabo diferente.
    Ele pode parecer uma louco, mas cabos falhar muito, tente usar um cabo diferente para conectar o disco. Se você tiver um disco interno em um PC desktop, você provavelmente precisará desligar seu computador antes de alternar os cabos - consulte o manual do seu PC para obter detalhes.
7. Verifique o Gerenciador de dispositivos para problemas.
    Pressione e mantenha (ou clique com botão direito) no botão Iniciar, em seguida, selecione Gerenciador de dispositivos no menu de contexto. Procurar por todos os dispositivos com um ponto de exclamação ao lado de-lo ou outros problemas, duas vezes no dispositivo e, em seguida, ler seu status.

    Aqui está uma lista de [códigos de erro no Gerenciador de dispositivos](https://support.microsoft.com/help/310123/error-codes-in-device-manager-in-windows), mas uma abordagem, às vezes, funciona é com o botão direito do dispositivo problemático, selecione **dispositivo desinstalar**e, em seguida, **ação**  >  **Examinar alterações de hardware**.

    ![Gerenciador de dispositivos mostrando um dispositivo USB desconhecido](media/device-manager.PNG)
8. Conecte o disco em um computador diferente.
    
    Se o disco não funciona em outro PC, é um bom sinal de que há algo ruim acontecendo com o disco e não em seu computador. Nenhum diversão, nós sabemos. Há mais algumas etapas que você pode testar em [externa USB drive erro "Você deve inicializar o disco antes que o Gerenciador de discos lógicos pode acessá-lo"](https://social.technet.microsoft.com/Forums/windows/en-US/2b069948-82e9-49ef-bbb7-e44ec7bfebdb/forum-faq-external-usb-drive-error-you-must-initialize-the-disk-before-logical-disk-manager-can?forum=w7itprohardware), mas, talvez seja hora para procurar e solicitar ajuda no [Microsoft community](https://answers.microsoft.com/en-us/windows) do site ou entre em contato com o fabricante do disco.

    Se você apenas não é possível fazê-lo funcionar, também há aplicativos que podem tentar recuperar dados de um disco com defeito ou se os arquivos são realmente importantes, você pode pagar um laboratório de recuperação de dados para tentar recuperá-los. Se você encontrar algo que funciona para você, fale na seção de comentários abaixo.

> [!IMPORTANT]
> Discos falharem com bastante frequência, portanto, é importante fazer backup regularmente de todos os arquivos que importantes para você. Se você tiver um disco não aparece, às vezes, ou em erros, considere isso um lembrete para verificar seus métodos de backup. Ele é Okey se você estiver um pouco behind - Todos somos lá. A melhor solução de backup é aquele usado, portanto, é recomendável que você encontrar um que funciona para você e permanecer fiel a ele.
> 
> [!TIP]
> Para obter informações sobre como usar aplicativos criados no Windows para arquivos de backup para uma unidade externa, como uma unidade USB, consulte [fazer backup e restaurar os arquivos](https://support.microsoft.com/help/17143/windows-10-back-up-your-files). Você também pode salvar arquivos no OneDrive da Microsoft, que sincroniza arquivos do seu computador para a nuvem. Se o disco rígido falhar, você ainda poderá obter todos os arquivos que armazena no OneDrive do OneDrive.com. Para obter mais informações, consulte [OneDrive em seu PC](https://support.microsoft.com/help/17184/windows-10-onedrive).

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>O status de um disco básico ou dinâmico é ilegível

**Causa:**  o disco básico ou dinâmico não está acessível e pode estar com uma falha de hardware, corrupção ou erros de e/s. A cópia do disco do banco de dados de configuração de disco do sistema pode estar corrompida. Um ícone de erro aparece nos discos com o status **Ilegível**.

Os discos também podem exibir o status **Ilegível** enquanto estão girando ou quando o Gerenciamento de disco verifica todos os discos no sistema. Em alguns casos, a falha do disco ilegível não é recuperável. Para discos dinâmicos, o status **Ilegível** normalmente resulta de danos ou erros de E/S em parte do disco, em vez de falha no disco inteiro.

**Solução:**  examinar novamente os discos ou reinicie o computador para ver se o status do disco é alterado. Além disso, tente as etapas de solução de problemas descritas [é de status de um disco não inicializado ou o disco está ausente inteiramente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-dynamic-disks-status-is-foreign"></a>O status de um disco dinâmico é externo

**Causa:**  as **Foreign** status ocorre quando você move um disco dinâmico no computador local de outro computador PC. Um ícone de aviso aparece nos discos com o status **Externo**.

Em alguns casos, um disco que foi previamente conectado ao sistema pode exibir o status **Externo**. Os dados de configuração para discos dinâmicos são armazenados em todos os discos dinâmicos, portanto, as informações sobre quais discos são de propriedade do sistema são perdidas quando ocorre falha neles.

**Solução:**  adicionar o disco à configuração de sistema do computador para que você pode acessar dados no disco. Para adicionar um disco à configuração do sistema do computador, importe o disco externo (clique com botão direito do mouse no disco e, em seguida, clique em **Importar discos externos**). Todos os volumes existentes no disco externo ficam visíveis e acessíveis quando você importa o disco. 

## <a name="a-dynamic-disks-status-is-online-errors"></a>Status de um disco dinâmico é Online (erros)

**Causa:**  o disco dinâmico tem erros de e/s em uma região do disco. Um ícone de aviso é exibido no disco dinâmico com erros.

**Solução:**   se os erros de e/s são temporários, reative o disco para retorná-lo a **Online** status.

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>Status de um disco dinâmico é Offline ou ausente

**Causa:**  um **Offline** disco dinâmico pode estar corrompido ou temporariamente não disponível. Um ícone de erro aparece no disco dinâmico offline.

Se o status do disco for **Offline** e o nome do disco mudar para **Ausente**, o disco esteve disponível recentemente no sistema, mas não pode ser localizado ou identificado. O disco ausente pode estar corrompido, desligado ou desconectado.

**Solução:** Para colocar um disco que está Offline e ausente novamente online:

1. Repare qualquer disco, controlador ou problema de cabo. 
2. Verifique se o disco físico está ligado e conectado ao computador. 
3. Em seguida, use o comando **Reativar disco** para colocar o disco novamente online.
4. Repita as etapas de solução de problemas descritas [é de status de um disco não inicializado ou o disco está ausente inteiramente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).
5. Se o status do disco permanecer **Offline** e o nome do disco permanecer **Ausente**, e você determinar que o disco tem um problema que não pode ser reparado, é possível remover o disco do sistema ao clicar no disco e, em seguida, clicar em **Remover disco**). No entanto, antes de remover o disco, você deve excluir todos os volumes (ou espelhamentos) no disco. Você pode salvar todos os volumes espelhados no disco removendo o espelhamento em vez do volume inteiro. A exclusão de um volume destrói os dados nele, portanto, você deve remover um disco somente você tiver certeza de que está danificado ou inutilizável permanentemente.

**Para colocar um disco que está Offline e ainda é chamado de disco \# (não ausente) novamente online, tente uma ou mais dos procedimentos a seguir:**

1. No Gerenciamento de disco, clique com o botão direito do mouse no disco e, em seguida, clique em **Reativar disco** para ativá-lo. Se o status do disco permanecer **Offline**, verifique os cabos e o controlador do disco e certifique-se de que o disco físico está íntegro. Corrija os problemas e tente reativar o disco novamente. Se o disco for reativado com sucesso, todos os volumes no disco devem retornar automaticamente o status **Íntegro**.
2. No Visualizador de Eventos, verifique os logs de eventos para quaisquer erros relacionados ao disco, como "Cópias de configuração inadequadas". Se os logs de evento contêm esse erro, entre em contato com o [Atendimento Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Tente mover o disco para outro computador. Se o disco ficar **Online** em outro computador, o problema pode ocorrer devido à configuração do computador em que o disco não fica **Online**.

4. Tente mover o disco para outro computador com discos dinâmicos. Importe o disco nesse computador e, em seguida, mova o disco para o computador no qual ele não fica **Online**. 

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>O status de um volume básico ou dinâmico é falha

**Causa:**   o volume básico ou dinâmico não pode ser iniciado automaticamente, o disco está danificado ou o sistema de arquivos está corrompido. A menos que o sistema de arquivos ou o disco possa ser reparado, o status **Falha** indica a perda de dados.

**Solução:**

Se o volume for um volume básico com **Failed** status:

- Verifique se o disco físico subjacente está ligado e conectado ao computador.
- Repita as etapas de solução de problemas descritas [é de status de um disco não inicializado ou o disco está ausente inteiramente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

Se o volume for dinâmico com o status **Falha**:

-   Verifique se os discos subjacentes estão online. Caso contrário, retorne o status do disco para **Online**. Se a ação for bem sucedida, o volume é reiniciado automaticamente e retorna ao status **Íntegro**. Se o disco dinâmico retornar para o status **Online**, mas o volume dinâmico não retornar para o status **Íntegro**, você poderá reativar o volume manualmente.
-   Se o volume dinâmico for um volume espelhado ou RAID-5 com dados antigos, colocar o disco subjacente online não reinicia automaticamente o volume. Se os discos com dados atuais são desconectados, coloque esses discos online pela primeira vez (para permitir que os dados sejam sincronizados). Caso contrário, reinicie o volume espelhado ou RAID-5 manualmente e, em seguida, execute a ferramenta de verificação de erro ou Chkdsk.exe.
- Repita as etapas de solução de problemas descritas [é de status de um disco não inicializado ou o disco está ausente inteiramente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>Status de um volume básico ou dinâmico é desconhecido

**Causa:**   as **desconhecido** status ocorre quando o setor de inicialização para o volume está corrompido (possivelmente devido a um vírus) e você não pode acessar dados no volume. O status **Desconhecido** também ocorre quando você instala um novo disco, mas não conclui com êxito o assistente para criar uma assinatura de disco.

**Solução**  inicializar o disco. Para obter instruções, consulte [Inicializar novos discos](initialize-new-disks.md).

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>Status de um volume dinâmico é dados incompletos

**Causa:** Você moveu alguns, mas não todos os discos em um volume em vários discos. Os dados nesse volume serão destruídos, a menos que você mova e importe os discos restantes contendo esse volume.

**Solução:**

1. Mova todos os discos que compõem o volume de vários discos para o computador.
2. Importe os discos. Para obter instruções sobre como mover e importar discos, consulte [Mover discos para outro computador](move-disks-to-another-computer.md).

Se você não precisar mais do volume de vários discos, é possível importar o disco e criar novos volumes nele. Para fazer isso:

1. Clique com botão direito do mouse no volume com status **Falha** ou **Falha de redundância** e clique em **Excluir volume**.
2. Clique com o botão direito do mouse no disco e, em seguida, clique em **Novo volume**.

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>Status de um volume dinâmico é Íntegro (em risco)

**Causa:**   indica que o volume dinâmico está acessível no momento, mas foram detectados erros de e/s no disco dinâmico subjacente. Se um erro de E/S for detectado em qualquer parte de um disco dinâmico, todos os volumes no disco exibem o status **Íntegro (em risco)** e um ícone de aviso aparece no volume.

Quando o status do volume é **Íntegro (em risco)** , o status de um disco subjacente é geralmente **Online (erros)** .

**Solução:**  
1. Reverta o status do disco subjacente para **Online**. Depois que o status do disco é revertido para **Online**, o volume deve reverter para o status **Íntegro**. Se o status **Íntegro (em risco)** continuar, o disco pode estar falhando. 

2. Faça o backup dos dados e substitua o disco assim que for possível. 

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Não é possível gerenciar volumes distribuídos usando o gerenciamento de disco ou DiskPart

**Causa:**   alguns produtos de gerenciamento de disco não-Microsoft substituir o Gerenciador de discos lógicos (LDM) Microsoft avançados para gerenciamento de disco, que pode desabilitar o LDM.

**Solução:**   se você estiver usando software de gerenciamento de disco não são da Microsoft que tenha desabilitado o LDM, você deve contatar o fornecedor no software de gerenciamento de disco não são da Microsoft para obter assistência na solução de problemas com o disco ou de suporte configuração.

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Gerenciamento de disco não é possível iniciar o serviço de disco Virtual

**Causa:**   se um computador remoto não suporta o serviço de disco Virtual (VDS) ou se você não puder estabelecer uma conexão com o computador remoto porque ele é bloqueado pelo Firewall do Windows, você poderá receber esse erro.

**Solução:**

1. Se o computador remoto oferecer suporte para VDS, você pode configurar o Windows Defender Firewall para permitir conexões VDS. Se o computador remoto não oferecer suporte a VDS, você pode usar a Conexão de Área de Trabalho Remota para se conectar a ela e, em seguida, executar o Gerenciamento de disco diretamente no computador remoto.
2. Para gerenciar discos em computadores remotos que oferecem suporte a VDS, você deve configurar o Windows Defender Firewall no computador local (no qual você está executando o Gerenciamento de disco) e o computador remoto.
3. No computador local, configure o Windows Defender Firewall para habilitar a Exceção de gerenciamento remoto do volume.

> [!NOTE]
> A Exceção de gerenciamento remoto do volume inclui exceções para Vds.exe, Vdsldr.exe e a porta TCP 135.

> [!NOTE]
> Não há suporte para conexões remotas em grupos de trabalho. O computador local e o computador remoto devem ser membros de um domínio.