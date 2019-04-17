---
title: "Solução de problemas do Gerenciamento de disco"
description: Este artigo descreve como solucionar problemas de Gerenciamento de disco
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e4b361631799dccbc2b77fb5aa909052532a057d
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="troubleshooting-disk-management"></a>Solução de problemas do Gerenciamento de disco

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico lista alguns dos problemas comuns que você poderá encontrar ao usar o Gerenciamento de disco.

<a id="BKMK_1"></a>

## <a name="partitions-on-basic-disks-added-to-the-system-do-not-appear-in-the-disk-management-volume-list-view"></a>As partições em discos básicos adicionados ao sistema não são exibidas no modo de exibição de Lista de volumes do Gerenciamento de Disco.

**Causa:** volumes em discos básicos adicionados ao sistema não são montados automaticamente e recebem letras de unidade por padrão.
Os discos dinâmicos são exibidos como **Externos** quando eles são adicionados ao sistema. Para usar os volumes, você deve importar os discos **Externos** discos e, em seguida, deixá-los **Online**.
Os dispositivos de mídia removível (como unidades Zip ou Jaz) e os discos óticos (como CD-ROM ou DVD-RAM) sempre são montados automaticamente pelo sistema.

**Solução:** monte manualmente os volumes básicos, atribua letras de unidade, ou crie pontos de montagem com o Gerenciamento de disco ou os comandos [DiskPart](http://go.microsoft.com/fwlink/?LinkId=64110) ou [mountvol](http://go.microsoft.com/fwlink/?LinkId=64111).

<a id="BKMK_2"></a>

## <a name="a-basic-disks-status-is-not-initialized"></a>O status de um disco básico é Não Inicializado.

**Causa:** o disco não contém uma assinatura válida. Depois de instalar um novo disco, o sistema operacional deve gravar uma assinatura de disco, o final do marcador de setor (também chamado de palavra de assinatura) e um Registro mestre de inicialização ou a Tabela de partição de GUID para que você possa criar partições no disco. Quando você inicia o Gerenciamento de disco depois de instalar um novo disco, um assistente aparece fornecendo uma lista dos novos discos detectados pelo sistema operacional. Se você cancelar o assistente antes da gravação da assinatura de disco, o status dele permanece como **Não inicializado**.

**Solução:** inicializar o disco. O status do disco altera brevemente para **Inicializando** e, em seguida, para **Online**. Para obter instruções sobre como inicializar um disco, consulte [Inicializar novos discos](initialize-new-disks.md). 

<a id="BKMK_3"></a>

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>O status de um disco básico ou dinâmico é Ilegível.

**Causa:** o disco básico ou dinâmico não é acessível e pode estar com uma falha de hardware, corrupção ou erros de E/S. A cópia do disco do banco de dados de configuração de disco do sistema pode estar corrompida. Um ícone de erro aparece nos discos com o status **Ilegível**.

Os discos também podem exibir o status **Ilegível** enquanto estão girando ou quando o Gerenciamento de disco verifica todos os discos no sistema. Em alguns casos, a falha do disco ilegível não é recuperável. Para discos dinâmicos, o status **Ilegível** normalmente resulta de danos ou erros de E/S em parte do disco, em vez de falha no disco inteiro.

**Solução:** examine os discos ou reinicie o computador para ver se o status do disco é alterado.

<a id="BKMK_4"></a>

## <a name="a-dynamic-disks-status-is-foreign"></a>O status de um disco dinâmico é Externo.

**Causa:** o status **Externo** ocorre quando você move um disco dinâmico para o computador local de outro computador que executa os sistemas operacionais Windows 2000, Windows XP Professional, Windows XP 64-Bit Edition ou Windows Server 2003. Um ícone de aviso aparece nos discos com o status **Externo**.

Em alguns casos, um disco que foi previamente conectado ao sistema pode exibir o status **Externo**. Os dados de configuração para discos dinâmicos são armazenados em todos os discos dinâmicos, portanto, as informações sobre quais discos são de propriedade do sistema são perdidas quando ocorre falha neles.

**Solução:** adicione o disco à configuração de sistema do computador para que você possa acessar os dados no disco. Para adicionar um disco à configuração do sistema do computador, importe o disco externo (clique com botão direito do mouse no disco e, em seguida, clique em **Importar discos externos**). Todos os volumes existentes no disco externo ficam visíveis e acessíveis quando você importa o disco. 

<a id="BKMK_5"></a>

## <a name="a-dynamic-disks-status-is-online-errors"></a>O status de um disco dinâmico é Online (Erros)

**Causa:** o disco dinâmico tem erros de E/S em uma região dele. Um ícone de aviso é exibido no disco dinâmico com erros.

**Solução:** se os erros de E/S forem temporários, reative o disco para retorná-lo ao status **Online**.

<a id="BKMK_6"></a>

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>O status de um disco dinâmico é Offline ou Ausente.

**Causa:** um disco dinâmico **offline** pode estar corrompido ou não disponível de modo intermitente. Um ícone de erro aparece no disco dinâmico offline.

Se o status do disco for **Offline** e o nome do disco mudar para **Ausente**, o disco esteve disponível recentemente no sistema, mas não pode ser localizado ou identificado. O disco ausente pode estar corrompido, desligado ou desconectado.

**Solução:** colocar um disco que Offline e ausente novamente online:
1. Repare qualquer disco, controlador ou problema de cabo. 
2. Verifique se o disco físico está ligado e conectado ao computador. 
3. Em seguida, use o comando **Reativar disco** para colocar o disco novamente online.

4. Se o status do disco permanecer **Offline** e o nome do disco permanecer **Ausente**, e você determinar que o disco tem um problema que não pode ser reparado, é possível remover o disco do sistema ao clicar no disco e, em seguida, clicar em **Remover disco**). No entanto, antes de remover o disco, você deve excluir todos os volumes (ou espelhamentos) no disco. Você pode salvar todos os volumes espelhados no disco removendo o espelhamento em vez do volume inteiro. A exclusão de um volume destrói os dados nele, portanto, você deve remover um disco somente você tiver certeza de que está danificado ou inutilizável permanentemente.

**Para colocar um disco que está Offline e ainda é chamado de Disco \ # (não ausente) novamente online, tente executar um ou mais dos procedimentos a seguir:**

1. No Gerenciamento de disco, clique com o botão direito do mouse no disco e, em seguida, clique em **Reativar disco** para ativá-lo. Se o status do disco permanecer **Offline**, verifique os cabos e o controlador do disco e certifique-se de que o disco físico está íntegro. Corrija os problemas e tente reativar o disco novamente. Se o disco for reativado com sucesso, todos os volumes no disco devem retornar automaticamente o status **Íntegro**.
2. No Visualizador de Eventos, verifique os logs de eventos para quaisquer erros relacionados ao disco, como "Cópias de configuração inadequadas". Se os logs de evento contêm esse erro, entre em contato com o [Atendimento Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Tente mover o disco para outro computador. Se o disco ficar **Online** em outro computador, o problema pode ocorrer devido à configuração do computador em que o disco não fica **Online**.

4. Tente mover o disco para outro computador com discos dinâmicos. Importe o disco nesse computador e, em seguida, mova o disco para o computador no qual ele não fica **Online**. 

<a id="BKMK_7"></a>

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>O status de um volume básico ou dinâmico é Falha.

**Causa:** o volume básico ou dinâmico não pode ser iniciado automaticamente, o disco está danificado ou o sistema de arquivos está corrompido. A menos que o sistema de arquivos ou o disco possa ser reparado, o status **Falha** indica a perda de dados.

**Solução:** se o volume for básico com o status **Falha**:

-   Verifique se o disco físico subjacente está ligado e conectado ao computador. Nenhuma outra ação do usuário é possível para volumes básicos.

    Se o volume for dinâmico com o status **Falha**: 
-   Verifique se os discos subjacentes estão online. Caso contrário, retorne o status do disco para **Online**. Se a ação for bem sucedida, o volume é reiniciado automaticamente e retorna ao status **Íntegro**. Se o disco dinâmico retornar para o status **Online**, mas o volume dinâmico não retornar para o status **Íntegro**, você poderá reativar o volume manualmente. 
    
-   Se o volume dinâmico for um volume espelhado ou RAID-5 com dados antigos, colocar o disco subjacente online não reinicia automaticamente o volume. Se os discos com dados atuais são desconectados, coloque esses discos online pela primeira vez (para permitir que os dados sejam sincronizados). Caso contrário, reinicie o volume espelhado ou RAID-5 manualmente e, em seguida, execute a ferramenta de verificação de erro ou Chkdsk.exe.

<a id="BKMK_8"></a>

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>O status de um volume básico ou dinâmico é Desconhecido.

**Causa:** o status **Desconhecido** ocorre quando o setor de inicialização do volume está corrompido (possivelmente por causa de um vírus) e você não pode acessar os dados no volume. O status **Desconhecido** também ocorre quando você instala um novo disco, mas não conclui com êxito o assistente para criar uma assinatura de disco.

**Solução:** inicializar o disco. Para obter instruções, consulte [Inicializar novos discos](initialize-new-disks.md). 

<a id="BKMK_9"></a>

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>O status de um volume dinâmico é Dados Incompletos.

**Causa:** você moveu alguns, mas não todos os discos em um volume de vários discos. Os dados nesse volume serão destruídos, a menos que você mova e importe os discos restantes contendo esse volume.

**Solução:**  
1. Mova todos os discos que compõem o volume de vários discos para o computador.

2. Importe os discos. Para obter instruções sobre como mover e importar discos, consulte [Mover discos para outro computador](move-disks-to-another-computer.md).

Se você não precisar mais do volume de vários discos, é possível importar o disco e criar novos volumes nele. Para fazer isso: 

1. Clique com botão direito do mouse no volume com status **Falha** ou **Falha de redundância** e clique em **Excluir volume**. 

2. Clique com o botão direito do mouse no disco e, em seguida, clique em **Novo volume**.

<a id="BKMK_10"></a>

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>O status de um volume dinâmico é Íntegro (Em Risco).

**Causa:** indica que o volume dinâmico está acessível no momento, mas foram detectados erros de E/S no disco dinâmico subjacente. Se um erro de E/S for detectado em qualquer parte de um disco dinâmico, todos os volumes no disco exibem o status **Íntegro (em risco)** e um ícone de aviso aparece no volume.

Quando o status do volume é **Íntegro (em risco)**, o status de um disco subjacente é geralmente **Online (erros)**.

**Solução:**  
1. Reverta o status do disco subjacente para **Online**. Depois que o status do disco é revertido para **Online**, o volume deve reverter para o status **Íntegro**. Se o status **Íntegro (em risco)** continuar, o disco pode estar falhando. 

2. Faça o backup dos dados e substitua o disco assim que for possível. 

<a id="BKMK_11"></a>

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Não é possível gerenciar volumes distribuídos usando o Gerenciamento de Disco ou DiskPart.

**Causa:** alguns produtos de gerenciamento de disco de outros fornecedores substituem o Gerenciador de discos lógicos Microsoft (LDM) para o gerenciamento de disco avançado, que pode desabilitar o LDM.

**Solução:** se você estiver usando software de gerenciamento de disco de outros fornecedores e desabilitar o LDM, é necessário contatar o suporte ou a assistência do fornecedor do software de gerenciamento de disco para solucionar problemas de configuração de disco.

<a id="BKMK_virtdisk"></a>

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>O Gerenciamento de disco não pode iniciar o Serviço de disco virtual.

**Causa:** se um computador remoto não oferece suporte ao Serviço de disco virtual (VDS) ou se não for possível estabelecer uma conexão com o computador remoto porque ele está bloqueado pelo Firewall do Windows, você pode receber esse erro.

**Solução:**  
1. Se o computador remoto oferecer suporte para VDS, você pode configurar o Windows Defender Firewall para permitir conexões VDS. Se o computador remoto não oferecer suporte a VDS, você pode usar a Conexão de Área de Trabalho Remota para se conectar a ela e, em seguida, executar o Gerenciamento de disco diretamente no computador remoto.

2. Para gerenciar discos em computadores remotos que oferecem suporte a VDS, você deve configurar o Windows Defender Firewall no computador local (no qual você está executando o Gerenciamento de disco) e o computador remoto.

3. No computador local, configure o Windows Defender Firewall para habilitar a Exceção de gerenciamento remoto do volume.

<br />

> [!NOTE]
> A Exceção de gerenciamento remoto do volume inclui exceções para Vds.exe, Vdsldr.exe e a porta TCP 135.

<br />

 > [!NOTE]
 > Não há suporte para conexões remotas em grupos de trabalho. O computador local e o computador remoto devem ser membros de um domínio.