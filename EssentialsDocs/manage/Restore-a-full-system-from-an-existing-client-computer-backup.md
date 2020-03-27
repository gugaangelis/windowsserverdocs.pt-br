---
title: Restaurar um sistema completo de um backup de computador cliente existente
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47e498a6-1b71-47de-88f6-8c13c221d108
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b9629f41c4e8eb707b19914a297d9d8b88c6aead
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310650"
---
# <a name="restore-a-full-system-from-an-existing-client-computer-backup"></a>Restaurar um sistema completo de um backup de computador cliente existente

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Falhas no hardware e no sistema operacional são raras, mas podem acontecer. Um ventilador com defeito poderia superaquecer a placa-mãe de um computador e inutilizá-lo. O sistema operacional poderia ser corrompido e não ser iniciado. Danos causados por fogo e água podem resultar em danos permanentes ao hardware. Uma unidade de disco rígido pode falhar ou você pode decidir substituí-la por uma unidade de disco rígido maior.  
  
 Este documento fornece informações sobre os seguintes tópicos:  
  
-   [O que é a restauração do sistema completo do computador?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_WhatIs)  
  
-   [Como funciona o ambiente de restauração do sistema?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_HowDoes)  
  
-   [Criar uma unidade flash USB inicializável para restaurar um computador cliente](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable)  
  
-   [Usando o assistente de restauração completa do sistema](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using)  
  
-   [Onde posso encontrar os drivers para o meu hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
##  <a name="what-is-computer-full-system-restore"></a><a name="BKMK_WhatIs"></a>O que é a restauração do sistema completo do computador?  
 Caso você substitua uma unidade de disco rígido ou o computador falhe de modo que não possa ser usado nem inicializado, é possível restaurar o sistema a partir de um backup anterior do computador. Uma restauração completa do sistema retorna o sistema para o estado em que estava no momento do backup.  
  
> [!IMPORTANT]
>  Você não pode realizar uma restauração completa do sistema em um hardware de computador, como uma placa de sistema, que não seja semelhante ao hardware que será substituído. O sistema operacional instalado é totalmente dependente do hardware subjacente do computador. No entanto, você pode realizar uma restauração completa do sistema em um disco rígido com tamanho igual ou maior do que aquele que será substituído.  
  
 Ao restaurar um sistema completamente, você pode escolher um backup de computador específico para restaurar o sistema, com todos os aplicativos, configurações e opções conhecidos do usuário antes de falhas, catástrofes ou roubos. Você também pode escolher quais volumes deseja restaurar.  
  
 Ao planejar ou preparar a restauração completa do sistema em um computador da rede, considere o seguinte:  
  
### <a name="windows-preinstallation-environment"></a>Ambiente de Pré-instalação do Windows  
 Ambiente de Pré-instalação do Windows (Windows PE) é um sistema operacional mínimo projetado para preparar o computador para a instalação do Windows. Para servidores que executam o Windows Server Essentials, o Windows PE é instalado automaticamente quando você insere a mídia de restauração em um computador a ser restaurado. Para servidores que executam o Windows Server Essentials, o Windows PE é instalado automaticamente quando você inicia o computador com o serviço de restauração do cliente ou com a unidade flash USB.  
  
 O Windows PE não tem suporte para conexões sem fio. Em função disso, o computador sendo restaurado deve estar fisicamente conectado à rede para pequenas empresas.  
  
### <a name="bitlocker"></a>BitLocker  
 O Criptografia de Unidade de Disco BitLocker (BitLocker) é um recurso de proteção de dados que está disponível em algumas versões do Windows Vista, Windows 7 e Windows 8. O BitLocker protege contra roubo ou exposição dos dados nos computadores que são perdidos ou roubados e oferece uma exclusão de dados mais segura quando os computadores forem desativados.  
  
 **Para o Windows Server Essentials:** Se o computador que você precisa restaurar foi criptografado usando o BitLocker (seja apenas a unidade do sistema operacional ou a unidade do sistema operacional e uma ou várias unidades fixas), você ainda pode usar a mídia de restauração completa do sistema contida no CD fornecido com o servidor e o assistente de restauração completa do sistema para reinstalar a imagem do disco rígido, incluindo o sistema operacional, de um backup e restaurar os dados para o computador novo ou reparado.  
  
 **Para o Windows Server Essentials:** Se o computador que você precisa restaurar foi criptografado usando o BitLocker (seja apenas a unidade do sistema operacional ou a unidade do sistema operacional e uma ou várias unidades fixas), você ainda poderá usar o assistente de restauração completa do sistema para reinstalar a imagem do disco rígido, incluindo o sistema operacional, de um backup e restaurar os dados para o computador novo ou reparado.  
  
 Quando o servidor faz backup de unidades, pastas e arquivos, uma versão não criptografada é salva no servidor. Durante a restauração completa do sistema, essa versão não criptografada é copiada para o computador.  
  
> [!NOTE]
>  Após o sistema ser completamente restaurado com êxito, você precisa reativar o BitLocker no computador.  
>   
>  Para obter instruções sobre como habilitar o BitLocker em computadores que executam o Windows 8, consulte [BitLocker: como habilitar o BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254918).  
>   
>  Para obter instruções sobre como habilitar o BitLocker em computadores que executam o Windows 7, consulte [criptografia de unidade de disco BitLocker guia passo a passo para o Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=140225).  
  
 Para mais informações sobre os fundamentos da Criptografia de Unidade de Disco BitLocker, consulte [Perguntas frequentes sobre o BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254917).  
  
### <a name="encrypting-file-system-encrypted-files"></a>Criptografia dos arquivos criptografados pelo sistema de arquivos  
 O recurso EFS (Sistema de Arquivos com Criptografia) no Windows pode oferecer criptografia adicional em nível de arquivo baseado em usuário para os diferentes níveis de segurança entre os vários usuários do mesmo computador. É importante observar que, ao contrário das unidades criptografadas pelo BitLocker, as pastas e os arquivos criptografados pelo EFS continuam sendo criptografados no backup de qualquer computador. O EFS não está disponível no Windows XP Home Edition, no Windows Vista Starter, no Windows Vista Home Basic, no Windows Vista Home Premium, no Windows 7 Starter, no Windows 7 Home Basic, no Windows 7 Home Premium ou no Windows 8. Só está disponível no Windows 8 Pro.  
  
> [!WARNING]
>  Ao contrário do BitLocker, você somente pode acessar arquivos protegidos por EFS de dentro do sistema operacional que realizou a criptografia.  
  
### <a name="disk-partitions"></a>Partições de disco  
 Se o tamanho da unidade de disco rígido no computador novo for o mesmo ou maior do que o original, a unidade de disco rígido será automaticamente reformatada e reparticionada. Consulte a tabela abaixo para obter especificações de:  
  
|Computador original|Computador restaurado ou novo|  
|-----------------------|------------------------------|  
|Único disco com várias partições|Único disco com várias partições, e qualquer espaço extra que está alocado à última partição|  
|Único disco com uma única partição|Único disco com uma única partição, e todo o espaço disponível que é usado para a partição única|  
  
> [!NOTE]
>  Se houver diferenças de tamanho do disco e de layout da partição entre o computador original e o computador restaurado ou novo, você deve usar Gerenciamento de Disco para criar as partições apropriadas no computador restaurado ou novo. Você pode fazer isso no Assistente de Restauração Completa do Sistema.  
  
### <a name="raid-and-dynamic-disks"></a>RAID e discos dinâmicos  
 Não há suporte para backup de RAID (Redundant Array of Independent Disk) e de discos dinâmicos.  
  
##  <a name="how-does-the-system-restore-environment-work"></a><a name="BKMK_HowDoes"></a>Como funciona o ambiente de restauração do sistema?  
 A mídia de restauração do sistema fornecida com o Windows ServerÂ® 2012 Essentials instala Ambiente de Pré-Instalação do Windows (Windows PE) no computador. O Windows PE substitui o ambiente MS-DOS e contém os principais arquivos de programa para o Windows. No Windows Server Essentials, há duas maneiras suportadas de restaurar um sistema: usando o serviço de restauração do cliente, que usa uma rede e não conta com a mídia ou usando a unidade flash USB.  
  
> [!NOTE]
>  O Windows PE não tem suporte para conexões sem fio. Em função disso, o computador sendo restaurado deve estar fisicamente conectado à rede para pequenas empresas.  
  
 O ambiente de restauração do sistema vem com arquivos de programa de 32 bits (x86) e 64 bits (x64). Depois de inserir a mídia de restauração do sistema, escolha a versão adequada dos arquivos. 32 bit (x86) é o padrão e selecionado automaticamente se você não escolher dentro de 30 segundos. Se houver atualizações aos arquivos de programa de restauração completa do sistema no servidor, os arquivos de atualização são baixados para o computador automaticamente.  
  
 Depois de o ambiente de pré-instalação estar configurado, o assistente de Restauração Completa do Sistema é iniciado. O assistente ajuda a restaurar o computador de um backup anterior. Você também pode usar o ambiente de restauração do sistema para restaurar um backup para um novo computador com hardware similar.  
  
 Na maioria dos casos, os arquivos de programa e os drivers contidos no ambiente de restauração do sistema são tudo o que é necessário para reiniciar o computador novo ou restaurado. Dependendo do hardware do computador novo ou restaurado, o ambiente de restauração do sistema talvez não inclua todos os drivers de adaptador de armazenamento e de rede que são necessários para reiniciar seu computador novo ou restaurado. O assistente de Restauração Completa do Sistema dá a oportunidade de instalar drivers, se necessário. Para obter mais informações sobre como encontrar os drivers de hardware, consulte [Onde localizar os drivers para meu hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers). Para informações sobre como usar a mídia de restauração do sistema, consulte [Usando o assistente de Restauração Completa do Sistema](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using).  
  
##  <a name="create-a-bootable-usb-flash-drive-to-restore-a-client-computer"></a><a name="BKMK_CreateBootable"></a>Criar uma unidade flash USB inicializável para restaurar um computador cliente  
 Se você precisar restaurar um computador cliente de um backup existente, mas não conseguir localizar o CD de restauração fornecido com o servidor (no Windows Server Essentials) ou não quiser configurar o serviço de restauração do cliente no servidor (no Windows Server Essentials) , você pode criar uma unidade flash USB inicializável. Então, pode usar a unidade flash USB para iniciar o computador cliente e restaurar o sistema. A unidade flash USB usada deve ter, no mínimo, 1 GB.  
  
#### <a name="to-create-a-bootable-usb-flash-drive"></a>Para criar uma unidade flash USB inicializável  
  
1.  Abra o **Dashboard**.  
  
2.  Clique na guia **Dispositivos**.  
  
3.  No painel **Tarefas**, clique em **Personalizar Backup do Computador e configurações do Histórico do Arquivo**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **tarefas de backup do computador cliente**.  
  
4.  Clique na guia **Ferramentas** e depois em **Criar chave** na seção **Recuperação do computador**. O assistente Criar chave de recuperação do computador será aberto.  
  
5.  Insira uma unidade flash USB de 1 GB ou maior no servidor e siga as instruções do assistente.  
  
    > [!CAUTION]
    >  Todos os dados na unidade flash USB serão excluídos.  
  
##  <a name="using-the-full-system-restore-wizard"></a><a name="BKMK_Using"></a>Usando o assistente de restauração completa do sistema  
 Após usar com êxito a mídia de restauração, o serviço de restauração do cliente ou a unidade flash USB para iniciar e verificar se todos os drivers de hardware estão carregados no computador cliente novo ou restaurado, o Assistente de recuperação completa do sistema é exibido. Esse assistente permite acessar o servidor, o backup do computador e os volumes de origem que você deseja restaurar para o computador, além de realizar o próprio processo de restauração.  
  
> [!NOTE]
>   O Windows Server Essentials não oferece suporte aos seguintes cenários de restauração:  
> 
> - Restauração de um disco de MBR (registro mestre de inicialização) em um computador baseado em Unified Extensible Firmware Interface (UEFI).  
>   -   Restaurar um backup UEFI/GPT para um sistema BIOS.  
> 
>   Se você restaurar os dados em qualquer um desses cenários, não poderá inicializar o sistema. Além disso, talvez você não consiga usar discos rígidos com mais de dois terabytes.  
  
 **Pré-requisitos**  
  
-   Antes de iniciar o processo de restauração do sistema completo, use um cabo de rede (conexão com fio) para conectar o computador à mesma rede do servidor. Verifique se tem acesso a todos os discos rígidos no computador cliente.  
  
    > [!WARNING]
    >  Não tente realizar uma restauração completa do sistema em um computador que use uma conexão sem fio com a rede.  
  
-   Se souber que o computador não tem drivers essenciais do dispositivo de armazenamento ou de rede, precisará localizar e copiar esses drivers para uma unidade flash antes de iniciar o processo de recuperação completa do sistema. Para o Windows Server Essentials: se você estiver usando a mídia de restauração completa do sistema fornecida no CD, esse CD deverá permanecer na unidade durante a parte inicial do processo de restauração completa do sistema. Portanto, você não deve copiar os drivers ausentes em um CD ou DVD a menos que tenha uma segunda unidade de CD/DVD. Em vez disso, copie os drivers ausentes para uma unidade flash USB.  
  
     Para obter informações sobre como localizar os drivers do seu computador, consulte [Onde encontrar os drivers para meu hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
-   Para o Windows Server Essentials: se não for possível localizar o CD de restauração do Windows Server Essentials, você poderá criar uma unidade flash USB inicializável. Para obter mais informações, consulte [Criar uma unidade flash USB inicializável para restaurar um computador cliente](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable).  
  
#### <a name="to-use-the-full-system-restore-wizard"></a>Para usar o Assistente de Restauração Completa do Sistema  
  
1. Siga um destes procedimentos:  
  
   -   Windows Server Essentials: Ative o computador cliente que você deseja restaurar, insira a mídia de restauração e desligue o computador.  
  
        Ligue o computador novamente e, durante o POST (Power On Self Test), pressione a tecla de função correta (tecla F) para acessar o menu Inicializar Dispositivo e, em seguida, selecione a unidade de CD/DVD. O Gerenciador de Inicialização do Windows é iniciado.  
  
   -   Windows Server Essentials: se você estiver usando o serviço de restauração do cliente, reinicie o computador usando a opção **inicializar da rede** . Caso contrário, inicie o computador usando a chave USB.  
  
        Ligue o computador novamente durante POST, pressione a tecla de função apropriada (tecla F) para acessar o Menu Inicializar Dispositivo e, em seguida, selecione **Inicialização de rede** (ou você pode optar por inicializar a partir da chave USB). O Gerenciador de Inicialização do Windows é iniciado.  
  
   > [!NOTE]
   >  Consulte a documentação do fabricante do computador para determinar qual a tecla de função que acessa o menu Inicializar dispositivo.  
  
2. A mídia de restauração do computador contém opções de inicialização de 32 bits (x86) e 64 bits (x64). No Gerenciador de Inicialização do Windows, escolha **Restauração Completa do Sistema (x86)** ou **Restauração Completa do Sistema (x64)** . Se os drivers de hardware do computador tiverem 32 bits, escolha x86; se eles tiverem 64 bits, escolha x64. Os arquivos do Windows são carregados e o Assistente de Restauração Completa realiza uma verificação para certificar-se de que todos os drivers de hardware estejam disponíveis.  
  
3. Na janela **Assistente de Restauração Completa do Sistema**, escolha seu idioma preferencial e clique na seta.  
  
4. Escolha o **Formato de hora e moeda** adequado e o **Teclado ou método de entrada** para esse computador. Clique em **Continuar**.  
  
5. Se os drivers estiverem ausentes, a mensagem o processo de restauração não poderá verificar se os drivers são exibidos. Clique em **Fechar** e na caixa de diálogo de boas-vindas, clique em **Carregar drivers**.  
  
   1.  Na caixa de diálogo **Detectar Hardware**, clique em **Instalar Drivers**.  
  
   2.  Insira a unidade flash USB que contém os drivers do hardware e, na caixa de diálogo **Instalar Drivers**, clique em **Digitalizar**.  
  
   3.  Na caixa de diálogo **Instalar Drivers**, clique em **OK** quando os drivers forem encontrados.  
  
   4.  Na caixa de diálogo **Detectar Hardware**, clique em **Continuar**.  
  
6. Se todos os drivers forem encontrados na verificação inicial ou quando todos os drivers críticos estiverem instalados, na janela **Restauração Completa do Sistema**, clique em **Continuar**.  
  
7. Na página **Bem-vindo ao Assistente de Restauração Completa do Sistema**, clique em **Avançar**.  
  
8. O assistente procura o seu servidor.  
  
   1.  Se o assistente não puder localizar o servidor, você terá a opção de procurar novamente ou de inserir o endereço IP do servidor.  
  
   2.  Se vários servidores forem detectados, você precisará selecionar um.  
  
   3.  Se o servidor estiver localizado, a página **fazer logon no < nomedoservidor\>** será exibida.  
  
9. Na página **fazer logon no < nomedoservidor\>** , digite *< AdministratorAccountName\>* na caixa de **texto nome de usuário** e a senha da conta de administrador na caixa de texto **senha** e clique em **Avançar**.  
  
    > [!IMPORTANT]
    >  Você deve usar uma conta de administrador criada em inglês. Se não tiver uma, você deve criar uma nova conta de administrador. Para fazer isso, primeiro, abra a guia **Usuários** no painel do servidor, em seguida, defina o formato de idioma do teclado para inglês e execute a tarefa **Adicionar uma conta de usuário** para criar a conta de administrador. Em seguida, use a nova conta de administrador para continuar a restaurar o computador cliente.  
  
10. Na página **Selecionar um computador para restaurar**, selecione o computador que deseja restaurar e clique em **Avançar**. Você pode escolher **< computername\>: (este computador)** ou escolher um computador diferente na rede na lista suspensa **outro computador** .  
  
    > [!NOTE]
    >  Se este for um computador desconhecido para o servidor (por exemplo, um computador novo ou realocado), a opção **Este computador** não será exibida.  
  
11. Na página **Selecionar um backup para restaurar**, revise a lista de backups disponíveis e selecione aquele backup cujo computador você deseja restaurar.  
  
    > [!NOTE]
    >  Recomenda-se selecionar um backup bem-sucedido (com marca de seleção verde). Isso ajuda a garantir que todos os arquivos de dados e do sistema sejam restaurados com êxito.  
  
12. (*Opcional*) Selecione um backup e clique em **Detalhes** para abrir a página **Detalhes de Backup** e exibir mais informações sobre esse backup. Use as informações na página **Detalhes de Backup** para comparar vários backups e ajudá-lo a decidir qual é a melhor opção. Clique em **Fechar** na página **Detalhes de backup** para voltar à página **Selecionar um backup para restaurar**.  
  
13. Na página **Selecionar um backup para restaurar**, selecione um backup e clique em **Avançar**.  
  
14. Na página **Selecionar opção de restauração**, clique em um dos seguintes itens e em **Avançar**.  
  
    > [!NOTE]
    >  Esta página não será exibida se o particionamento automático não for suportado.  
  
    1.  **Permitir que o assistente restaure totalmente o computador (recomendado)** . Esta opção ajuda a garantir que computador seja restaurado para o estado em que se encontrava antes da hora e data do backup escolhido. Se você escolher essa opção, pule para a etapa 15.  
  
    2.  **Permitir que eu selecione os volumes a serem restaurados (avançado)** . Esta opção permite escolher os volumes que você deseja restaurar e onde você deseja restaurá-los. Você também pode criar partições no disco rígido.  
  
15. Na página **Selecionar os volumes para restaurar**, você pode escolher os volumes que deseja restaurar.  
  
    > [!NOTE]
    >  Essa página é exibida se houver vários discos rígidos no computador de origem do backup ou se a unidade de destino da restauração tiver menos espaço de armazenamento que a unidade de origem do backup.  
  
    1. O assistente tenta fazer a correspondência dos volumes de origem e de destino. Você deverá verificar se o mapeamento padrão está correto.  
  
       1.  Para desmarcar um volume, clique na seta do menu da lista do volume e clique em **Nenhum**.  
  
       2.  Ao concluir a seleção dos volumes, clique em **Avançar**.  
  
    2. Se o volume de origem e o volume de destino tiverem o mesmo tamanho ou se o tamanho de origem for menor que o de destino, uma seta verde será exibida entre eles. Se houver incompatibilidade no tamanho do volume (quando o volume de origem for maior que o volume de destino), um X vermelho será exibido entre a origem e o destino.  
  
       > [!NOTE]
       >  Um X vermelho também poderá ser exibido se:  
       > 
       > - O tamanho do setor do disco do volume de origem não corresponder ao tamanho do setor do disco do volume de destino. Isso pode ocorrer se você substituir o disco físico por um disco com um tamanho de setor diferente, ou se você configurar os Espaços de Armazenamento (que podem ter um tamanho de setor diferente que aquele do disco físico).  
       >   -   O limite do número de cluster foi atingido. Para restaurar o volume de origem ao volume de destino, você deve formatar o volume de destino com o mesmo tamanho de cluster que o volume de origem. Se o volume de destino for muito grande e, se o tamanho do cluster for muito pequeno, o limite do número de cluster pode ser atingido.  
  
       1. Clique em **Executar Gerenciador de Disco (avançado)** e crie um novo volume com o mesmo tamanho que o volume reservado pelo sistema.  
  
          > [!NOTE]
          >  Se um computador cliente for baseado em Unified Extensible Firmware Interface (UEFI), você deverá usar a ferramenta **DiskPart** para inicializar o disco do sistema. Para fazer isso, abra uma janela Comando (pressione Ctrl+Alt+Shift por 5 segundos no ambiente WinPE), execute **diskpart.exe** e depois execute os seguintes comandos de diskpart:  
          > 
          > 1. **Disco de lista de > do DISKPART**  
          >    2. O **DISKPART > a seleção** do disco # *< disco\>*  
          >    3. **DISKPART > limpo**  
          >    4. **> De conversão de GPT do DISKPART**  
          >    5. **DISKPART > criar partição tamanho da EFI =** *100* (em que *100* é um tamanho de partição de exemplo em MB, deve ser o mesmo que a partição original)  
          >    6. **DISKPART > criar partição de tamanho MSR =** *128* (em que *128* é um tamanho de partição de exemplo em MB, deve ser igual à partição original)  
          >    7. **Saída do DISKPART >**  
  
       2. *(Opcional)* Selecione a opção **Não Atribuir uma Letra ou Caminho de Unidade**.  
  
       3. Formate o volume como **NTFS**.  
  
       4. Ao concluir a formatação, clique com o botão direito do mouse no novo volume do sistema e clique em **Marcar Partição como Ativa**.  
  
       5. Se você precisar de mais volumes que correspondam a outros volumes no backup, repita as etapas de *ii* a *iv* para criar e ativar os volumes e feche **Gerenciamento de Disco**.  
  
       6. Na página **Selecione os volumes para restaurar**, mapeie o volume reservado pelo sistema da origem do backup para o volume de mesmo tamanho que você criou na etapa *v*.  
  
       7. Mapeie todos os volumes de origem para os volumes de destino correspondentes.  
  
       8. Clique em **Avançar** para continuar com a restauração.  
  
16. Na página **Confirmar volumes que serão restaurados**, examine o mapeamento e clique em **Avançar**. Se você precisar fazer alterações, clique em **Voltar** e repita a etapa 14.  
  
17. A **restauração < computername\> da < data e hora da página de\>de backup** relata o progresso do processo de restauração.  
  
18. Na página **A restauração foi concluída com êxito**, remova a mídia de restauração e clique em **Concluir**. O computador é reiniciado.  
  
    > [!IMPORTANT]
    >  Se a opção Criptografia de Unidade de Disco BitLocker foi habilitada no computador antes da restauração, você deverá habilitar manualmente o Bitlocker após reiniciar o computador.  
  
##  <a name="where-can-i-find-the-drivers-for-my-hardware"></a><a name="BKMK_FindDrivers"></a>Onde posso encontrar os drivers para o meu hardware?  
 Dependendo do hardware do computador novo ou restaurado, a mídia de restauração talvez não inclua todos os adaptadores de armazenamento e de rede e os drivers que são necessários para reiniciar seu computador restaurado. Você deve determinar quais drivers estão ausentes, localizar esses drivers em uma mídia existente ou no site do fabricante, copiá-los para uma unidade flash e, em seguida, copiá-los da unidade flash para o computador novo ou restaurado ao executar o assistente de restauração completa do sistema.  
  
 Quando um é submetido a backup, os drivers do computador são salvos em backup. Se sua mídia de recuperação não inclui todos os drivers necessários, é possível abrir um backup para aquele computador e, em seguida, copiar os drivers para uma unidade flash USB.  
  
#### <a name="to-copy-drivers-from-a-backup-to-a-usb-flash-drive"></a>Para copiar drivers de um backup para uma unidade flash USB  
  
1. Em outro computador, abra o Dashboard.  
  
2. Clique em **Dispositivos** e clique no computador para o qual os drivers são necessários.  
  
3. Clique em **Restaurar arquivos ou pastas do computador**. O Assistente de Restauração de Arquivos ou Pastas é aberto.  
  
4. Clique no backup com êxito mais recente e, em seguida, clique em **Avançar**.  
  
5. Clique em um volume para abrir e clique em **Avançar**. Uma janela que lista os arquivos e pastas no backup é aberta.  
  
6. Insira sua unidade flash USB em um conector USB no computador e, em seguida, copie a pasta Drivers para a restauração completa do sistema para sua unidade flash USB.  
  
   > [!NOTE]
   >  Talvez seja necessário clicar em **Até um nível**, até que você alcance a raiz do volume do sistema.  
  
7. Remova a unidade flash e insira-a no computador que você está restaurando.  
  
   Você pode usar unidade flash USB para instalar os drivers para seu computador quando for restaurá-lo. O assistente de restauração ou assistente de pastas procura drivers adicionais nessa unidade flash USB, enquanto usa o Assistente de Restauração Completa do Sistema. Provavelmente, você precisará do driver do adaptador de rede e dos drivers do dispositivo de armazenamento.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
