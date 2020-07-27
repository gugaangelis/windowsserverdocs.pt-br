---
title: Solução de problemas do Gerenciamento de Disco
description: Este artigo descreve como solucionar problemas do Gerenciamento de Disco
ms.date: 12/20/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 359b4bed3090463bfb92431e06e325d981568248
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961018"
---
# <a name="troubleshooting-disk-management"></a>Solução de problemas do Gerenciamento de Disco

> **Aplica-se a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico lista alguns dos problemas comuns que você poderá enfrentar ao usar o Gerenciamento de Disco e apresenta as etapas de solução de problemas que você pode tentar.

> [!TIP]
> Se você receber um erro ou se algo não funcionar ao seguir estes procedimentos, não se desespere! Este tópico é somente a primeira coisa a tentar; há também muitas informações no site [Microsoft Community](https://answers.microsoft.com/en-us/windows) na seção [Arquivos, pastas e armazenamento](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) sobre a grande variedade de configurações de hardware e software com as quais você pode estar lidando. Se você ainda precisar de ajuda, publique uma pergunta ou [entre em contato com o Suporte da Microsoft](https://support.microsoft.com/contactus/) ou com o fabricante do seu hardware.

## <a name="how-to-open-disk-management"></a>Como abrir o Gerenciamento de Disco

Antes de começarmos a fazer coisas complicadas, eis uma maneira fácil de acessar o Gerenciamento de Disco, caso você ainda não esteja lá:

1. Digite **Gerenciamento do Computador** na caixa de pesquisa da barra de tarefas, selecione e mantenha o cursor (ou clique com o botão direito do mouse) em **Gerenciamento do Computador** e, em seguida, selecione **Executar como administrador** > **Sim**.
2. Depois que o Gerenciamento do Computador for aberto, acesse **Armazenamento** > **Gerenciamento de Disco**.

## <a name="disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps"></a>Discos ausentes ou não inicializados, além das etapas gerais de solução de problemas

![Gerenciamento de Disco mostra um disco desconhecido que deve ser inicializado.](media/uninitialized-disk.PNG)

**Causa**: Se você tem um disco que não aparece no Explorador de arquivos e está listado no Gerenciamento de Disco como *Não inicializado*, é possível que o disco não tenha uma assinatura de disco válida. Basicamente, isso significa que o disco nunca foi inicializado e formatado ou a formatação da unidade foi corrompida de alguma forma. 

Também é possível que o disco esteja tendo problemas de hardware ou de conexão, mas falaremos sobre isso nos parágrafos seguintes.

**Solução:**   se a unidade for totalmente nova e apenas precisar ser inicializada, apagando todos os dados nela, a solução é fácil - consulte [Inicializar novos discos](initialize-new-disks.md). No entanto, há uma boa chance de que você já tentou isso e não funcionou. Ou talvez você tem um disco cheio de arquivos importantes e não quer apagar o disco ao inicializá-lo.

Há uma série de motivos pelos quais um disco ou um cartão de memória pode estar ausente ou não ser inicializado, sendo um causa comum para a falha do disco. Não há muitos procedimentos para corrigir um disco com falha, mas aqui estão algumas etapas para tentar ver se é possível fazê-lo funcionar novamente. Se o disco funcionar após uma destas etapas, não se preocupe com as próximas etapas, apenas relaxe, comemore e até atualize seus backups.

1. Examine o disco no Gerenciamento de Disco. Se ele aparecer *Offline* conforme mostrado aqui, tente clicar com o botão direito do mouse e selecionar **Online**.

    ![Disco mostrado como offline](media/offline-disk.png)
2. Se o disco aparecer no Gerenciamento de Disco como *Online* e tiver uma partição primária listada como *Íntegra*, conforme mostrado aqui, isso é um bom sinal.

    ![Disco mostrado como online com um volume íntegro](media/healthy-volume.png)
    - Se uma partição tiver um sistema de arquivos, mas não tiver nenhuma letra da unidade (por exemplo, E:), consulte [Alterar uma letra da unidade](change-a-drive-letter.md) para adicionar uma letra da unidade manualmente.
    - Se uma partição não tiver um sistema de arquivos (se ela estiver listada como RAW em vez de NTFS, ReFS, FAT32 ou exFAT) e você souber que o disco está vazio, selecione e mantenha o cursor (ou clique com o botão direito do mouse) na partição e selecione **Formato**. A formatação de um disco apaga todos os dados contidos nele, portanto, não faça isso se você estiver tentando recuperar arquivos do disco. Em vez disso, pule para a próxima etapa.
    - Se a partição estiver listada como *Não Alocada* e você souber que ela está vazia, selecione e mantenha o cursor (ou clique com o botão direito do mouse) na partição não alocada e, em seguida, selecione **Novo Volume Simples** e siga as instruções para criar um volume no espaço livre. Não faça isso se você estiver tentando recuperar arquivos dessa partição. Em vez disso, pule para a próxima etapa.

    > [!NOTE]
    > Ignore todas as partições listadas como **Partição do sistema EFI** ou como **Partição de recuperação**. Essas partições ficam cheias de arquivos realmente importantes que o PC precisa para operar corretamente. É melhor deixá-las como estão para realizar os trabalhos delas, iniciando seu PC e ajudando você a se recuperar de problemas.
3. Se você tiver um disco externo que não está aparecendo, desconecte o disco, reconecte-o e, em seguida, selecione **Ação** > **Examinar Novamente os Discos**. 
4. Desligue o PC, desligue o disco rígido externo (se for um disco externo com cabo de alimentação) e, em seguida, religue o PC e o disco.
    Para desligar o PC no Windows 10, selecione o botão Iniciar, selecione o botão de Energia e, em seguida, selecione **Desligar**.
5. Conecte o disco uma porta USB diferente que está diretamente no computador (não em um hub).
    Às vezes, os discos USB não recebem energia suficiente de algumas portas ou há outros problemas com portas específicas. Isso é especialmente comum com hubs USB, mas às vezes, há diferenças entre portas em um PC, portanto, tente algumas portas diferentes se houver.
6. Tente um cabo diferente.
    Pode parecer inútil, mas como cabos falham muito, tente usar um cabo diferente para conectar o disco. Se você tiver um disco interno em um PC desktop, provavelmente será necessário desligar o computador antes de trocar os cabos. Consulte o manual do seu PC para obter detalhes.
7. Verifique o Gerenciador de Dispositivos para detectar a problemas.
    Selecione e mantenha o cursor (ou clique com o botão direito do mouse) no botão Iniciar e, em seguida, selecione Gerenciador de Dispositivos no menu de contexto. Localize todos os dispositivos com um ponto de exclamação ao lado ou outros problemas, clique duas vezes no dispositivo e leia o status dele.

    Aqui está uma lista de [Códigos de erro no Gerenciador de Dispositivos](https://support.microsoft.com/help/310123/error-codes-in-device-manager-in-windows), mas uma abordagem que às vezes funciona é selecionar e manter o cursor (ou clicar com o botão direito do mouse) no dispositivo problemático, selecionar **Desinstalar dispositivo** e, em seguida, **Ação** > **Verificar se há alterações de hardware**.

    ![Gerenciador de Dispositivos mostrando um dispositivo USB desconhecido](media/device-manager.PNG)
8. Conecte o disco em um PC diferente.
    
    Se o disco não funcionar em outro PC, é um bom sinal de que há algum mau funcionamento com o disco e não com seu PC. Sabemos que não é nada agradável. Há mais algumas etapas que você pode tentar em [Erro de unidade USB externa "Você deve inicializar o disco para que o Gerenciador de Discos Lógicos possa acessá-lo"](https://social.technet.microsoft.com/Forums/windows/en-US/2b069948-82e9-49ef-bbb7-e44ec7bfebdb/forum-faq-external-usb-drive-error-you-must-initialize-the-disk-before-logical-disk-manager-can?forum=w7itprohardware), mas talvez seja hora de pesquisar e pedir ajuda no site [Microsoft Community](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) ou entrar em contato com o fabricante do disco ou com o [Suporte da Microsoft](https://support.microsoft.com/contactus/).

    Se você simplesmente não conseguir fazer o disco funcionar, também existem aplicativos que podem tentar recuperar dados de um disco com falha ou, se os arquivos forem realmente importantes, é possível pagar um laboratório de recuperação de dados para tentar recuperá-los. Se você encontrar algo que funcione para você, nos informe na seção de comentários abaixo.

> [!IMPORTANT]
> Discos falham com bastante frequência, portanto, é importante fazer backup regularmente de todos os arquivos importantes para você. Se você tiver um disco que às vezes não aparece, ou dá erros, considere isso um lembrete para verificar novamente seus métodos de backup. Tudo bem se você estiver um pouco perdido, pois todos nós já estivemos. A melhor solução de backup é aquela adotada por você, portanto, encorajamos a encontrar uma solução que funcione para você e permaneça fiel a ela.
> 
> [!TIP]
> Para obter informações sobre como usar aplicativos criados no Windows para fazer backup de arquivos para uma unidade externa, como uma unidade USB, consulte [Fazer backup e restaurar os arquivos](https://support.microsoft.com/help/17143/windows-10-back-up-your-files). Também é possível salvar arquivos no Microsoft OneDrive, que sincroniza arquivos do PC para a nuvem. Se o disco rígido falhar, ainda será possível obter todos os arquivos que você armazena no OneDrive em OneDrive.com. Para obter mais informações, consulte [OneDrive em seu PC](https://support.microsoft.com/help/17184/windows-10-onedrive).

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>O status de um disco básico ou dinâmico é Ilegível

**Causa:**  o disco básico ou dinâmico não está acessível e pode ter uma falha de hardware, dados corrompidos ou erros de E/S. A cópia do disco do banco de dados de configuração de disco do sistema pode estar corrompida. Um ícone de erro aparece nos discos com o status **Ilegível**.

Os discos também podem exibir o status **Ilegível** enquanto estão girando ou quando o Gerenciamento de Disco está examinando novamente todos os discos no sistema. Em alguns casos, um disco ilegível falhou e não é recuperável. Para discos dinâmicos, o status **Ilegível** normalmente resulta de dados corrompidos ou erros de E/S em parte do disco, e não de falha do disco inteiro.

**Solução:**  examine novamente os discos ou reinicie o computador para ver se o status do disco é alterado. Além disso, tente as etapas de solução de problemas descritas em [O status de um disco é Não inicializado ou o disco está inteiramente ausente](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

## <a name="a-dynamic-disks-status-is-foreign"></a>O status de um disco dinâmico é Externo

**Causa:**  o status **Externo** ocorre quando você move um disco dinâmico de outro computador PC para o computador local. Um ícone de aviso aparece nos discos com o status **Externo**.

Em alguns casos, um disco que foi previamente conectado ao sistema pode exibir o status **Externo**. Os dados de configuração para discos dinâmicos são armazenados em todos os discos dinâmicos, portanto, as informações sobre quais discos são de propriedade do sistema são perdidas quando todos os discos dinâmicos falham.

**Solução:**  adicione o disco à configuração de sistema do computador para que você possa acessar os dados no disco. Para adicionar um disco à configuração do sistema do computador, importe o disco externo (selecione e mantenha o cursor (ou clique com o botão direito do mouse) no disco e, em seguida, clique em **Importar Discos Externos**). Todos os volumes existentes no disco externo ficam visíveis e acessíveis quando você importa o disco. 

## <a name="a-dynamic-disks-status-is-online-errors"></a>O status de um disco dinâmico é Online (Erros)

**Causa:**  o disco dinâmico tem erros de E/S em uma região dele. Um ícone de aviso é exibido no disco dinâmico com erros.

**Solução:**   se os erros de E/S forem temporários, reative o disco para retorná-lo ao status **Online**.

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>O status de um disco dinâmico é Offline ou Ausente

**Causa:**  um disco dinâmico **Offline** pode estar corrompido ou não disponível de forma intermitente. Um ícone de erro aparece no disco dinâmico offline.

Se o status do disco for **Offline** e o nome do disco mudar para **Ausente**, o disco esteve recentemente disponível no sistema, mas não pode ser localizado ou identificado. O disco ausente pode estar corrompido, desligado ou desconectado.

**Solução:** Recoloque online um disco que está Offline e Ausente:

1. Repare qualquer problema no cabo, disco ou controlador. 
2. Verifique se o disco físico está ligado e conectado ao computador. 
3. Em seguida, use o comando **Reativar disco** para colocar o disco novamente online.
4. Tente as etapas de solução de problemas descritas em [O status de um disco é Não inicializado ou o disco está inteiramente ausente](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).
5. Se o status do disco permanecer **Offline**, o nome do disco permanecer **Ausente** e você determinar que o disco tem um problema que não pode ser reparado, você poderá remover o disco do sistema ao selecionar e manter o cursor (ou clicar com o botão direito do mouse) no disco e, em seguida, clicar em **Remover Disco**. No entanto, antes de remover o disco, você deve excluir todos os volumes (ou espelhamentos) no disco. É possível salvar todos os volumes espelhados no disco removendo o espelhamento em vez do volume inteiro. A exclusão de um volume destrói os dados nele, portanto, somente remova um disco se você tiver certeza absoluta de que ele está permanentemente danificado ou inutilizável.

**Para recolocar online um disco que está Offline e ainda nomeado como Disco \# (Não ausente), tente um ou mais dos procedimentos a seguir:**

1. No Gerenciamento de Disco, selecione e mantenha o cursor (ou clique com o botão direito do mouse) no disco e, em seguida, clique em **Reativar Disco** para recolocá-lo online. Se o status do disco permanecer **Offline**, verifique os cabos e o controlador do disco e verifique se o disco físico está íntegro. Corrija quaisquer problemas e tente reativar o disco novamente. Se o disco for reativado com sucesso, todos os volumes no disco devem retornar automaticamente ao status **Íntegro**.
2. No Visualizador de Eventos, verifique os logs de eventos para ver todos os erros relacionados ao disco, como "Cópias de configuração inadequadas". Se os logs de evento contêm esse erro, contate o [Microsoft Product Support Services](/previous-versions/visualstudio/visual-basic-6/aa263468(v=vs.60)).

3. Tente mover o disco para outro computador. Se o disco ficar **Online** em outro computador, o problema ocorreu provavelmente devido à configuração do computador em que o disco não fica **Online**.

4. Tente mover o disco para outro computador que tenha discos dinâmicos. Importe o disco nesse computador e, em seguida, mova o disco de volta para o computador no qual ele não fica **Online**. 

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>O status de um volume básico ou dinâmico é Falha

**Causa:**   o volume básico ou dinâmico não pode ser iniciado automaticamente, o disco está danificado ou o sistema de arquivos está corrompido. A menos que o sistema de arquivos ou o disco possa ser reparado, o status **Falha** indica a perda de dados.

**Solução:**

Se o volume for básico com o status **Falha**:

- Verifique se o disco físico subjacente está ligado e conectado ao computador.
- Tente as etapas de solução de problemas descritas em [O status de um disco é Não inicializado ou o disco está inteiramente ausente](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

Se o volume for dinâmico com o status **Falha**:

-   Verifique se os discos subjacentes estão online. Caso contrário, retorne os discos para o status **Online**. Se a ação for bem sucedida, o volume é reiniciado automaticamente e retorna ao status **Íntegro**. Se o disco dinâmico retornar para o status **Online**, mas o volume dinâmico não retornar para o status **Íntegro**, é possível reativar o volume manualmente.
-   Se o volume dinâmico for um volume espelhado ou RAID-5 com dados antigos, colocar o disco subjacente online não reiniciará o volume automaticamente. Se os discos contendo dados atuais forem desconectados, primeiro coloque esses discos online (para permitir que os dados sejam sincronizados). Caso contrário, reinicie manualmente o volume espelhado ou RAID-5 e, em seguida, execute a ferramenta de verificação de erros ou Chkdsk.exe.
- Tente as etapas de solução de problemas descritas em [O status de um disco é Não inicializado ou o disco está inteiramente ausente](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>O status de um volume básico ou dinâmico é Desconhecido

**Causa:**   o status **Desconhecido** ocorre quando o setor de inicialização do volume está corrompido (possivelmente por causa de um vírus) e não é possível acessar os dados no volume. O status **Desconhecido** também ocorre quando você instala um novo disco, mas não conclui com êxito o assistente para criar uma assinatura de disco.

**Solução:**   Inicialize o disco. Para obter instruções, consulte [Inicializar novos discos](initialize-new-disks.md).

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>O status de um volume dinâmico é Dados incompletos

**Causa**: Você moveu alguns, mas não todos os discos em um volume de vários discos. Os dados nesse volume serão destruídos, a menos que você mova e importe os discos restantes contendo esse volume.

**Solução:**

1. Mova todos os discos que compõem o volume de vários discos para o computador.
2. Importe os discos. Para obter instruções sobre como mover e importar discos, consulte [Mover discos para outro computador](move-disks-to-another-computer.md).

Se você não precisar mais do volume de vários discos, é possível importar o disco e criar novos volumes nele. Para fazer isso:

1. Selecione e mantenha o cursor (ou clique com o botão direito do mouse) no volume com status **Falha** ou **Falha de redundância** e, em seguida, clique em **Excluir volume**.
2. Selecione e mantenha o cursor (ou clique com o botão direito do mouse) no disco e clique em **Novo volume**.

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>O status de um volume dinâmico é Íntegro (em risco)

**Causa:**   indica que o volume dinâmico está acessível atualmente, mas foram detectados erros de E/S no disco dinâmico subjacente. Se um erro de E/S for detectado em qualquer parte de um disco dinâmico, todos os volumes no disco exibem o status **Íntegro (em risco)** e um ícone de aviso aparece no volume.

Quando o status do volume é **Íntegro (em risco)** , o status de um disco subjacente é geralmente **Online (erros)** .

**Solução:**  
1. Retorne o status do disco subjacente para **Online**. Depois que o status do disco é revertido para **Online**, o volume deve retornar para o status **Íntegro**. Se o status **Íntegro (em risco)** persistir, o disco pode estar falhando. 

2. Faça o backup dos dados e substitua o disco assim que possível.

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Não é possível gerenciar volumes distribuídos usando o Gerenciamento de Disco ou DiskPart

**Causa:**   alguns produtos de gerenciamento de disco não Microsoft substituem o LDM (Gerenciador de Discos Lógicos) da Microsoft para o gerenciamento de disco avançado, o que pode desabilitar o LDM.

**Solução:**   se você estiver usando software de gerenciamento de disco não Microsoft que desabilitaram o LDM, é necessário contatar o suporte ou a assistência do fornecedor do software de gerenciamento de disco para solucionar problemas de configuração de disco.

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>O Gerenciamento de Disco não pode iniciar o Serviço de Disco Virtual

**Causa:**   se um computador remoto não dá suporte para VDS (Serviço de Disco Virtual) ou se não for possível estabelecer uma conexão com o computador remoto porque ele está bloqueado pelo Firewall do Windows, você pode receber esse erro.

**Solução:**

1. Se o computador remoto oferecer suporte para VDS, é possível configurar o Windows Defender Firewall para permitir conexões VDS. Se o computador remoto não oferecer suporte para VDS, você pode usar a Conexão de Área de Trabalho Remota para se conectar a ele e, em seguida, executar o Gerenciamento de Disco diretamente no computador remoto.
2. Para gerenciar discos em computadores remotos que oferecem suporte para VDS, é necessário configurar o Windows Defender Firewall no computador local (no qual está executando o Gerenciamento de Disco) e no computador remoto.
3. No computador local, configure o Windows Defender Firewall para habilitar a Exceção de Gerenciamento de Volume Remoto.

> [!NOTE]
> A Exceção de Gerenciamento de Volume Remoto inclui exceções para Vds.exe, Vdsldr.exe e a porta TCP 135.

> [!NOTE]
> Não há suporte para conexões remotas em grupos de trabalho. O computador local e o computador remoto devem ser membros de um domínio.

Veja também

- [Liberar espaço em disco no Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)
