---
title: Restaurar um sistema completo de um backup do computador cliente existente
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47e498a6-1b71-47de-88f6-8c13c221d108
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cfc4d1ce461e1e1cbb9b99970355c4dc7241911b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="restore-a-full-system-from-an-existing-client-computer-backup"></a>Restaurar um sistema completo de um backup do computador cliente existente

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Falhas de hardware e sistema operacional são raras, mas eles podem acontecer. Fã com defeito pode superaquecimento uma placa-mãe do computador e torná-lo inútil. O sistema operacional foi corrompido e se recusam a iniciar. Danos de fogo e água podem resultar em danos ao hardware permanente. Uma unidade de disco rígido pode falhar ou você pode optar por substituí-lo com um disco rígido maior.  
  
 Este documento fornece informações sobre os seguintes tópicos:  
  
-   [Qual é a restauração do sistema completo computador?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_WhatIs)  
  
-   [Como funciona o ambiente de restauração do sistema?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_HowDoes)  
  
-   [Criar uma unidade flash USB inicializável para restaurar um computador cliente](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable)  
  
-   [Usando o Assistente para restauração do sistema completo](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using)  
  
-   [Onde encontrar os drivers para o hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
##  <a name="BKMK_WhatIs"></a>Qual é a restauração do sistema completo computador?  
 Se você substituir uma unidade de disco rígido ou falha seu computador para o ponto em que ele não pode ser usado ou não inicializa, você pode restaurar o sistema de um backup anterior do computador. Uma restauração completa do sistema retorna o sistema ao estado em que ele estava no momento do backup.  
  
> [!IMPORTANT]
>  Você não pode executar uma restauração completa do sistema no hardware do computador, como uma placa de sistema, que não é semelhante ao hardware do computador que está sendo substituído. Um sistema operacional instalado depende de perto o hardware subjacente do computador. No entanto, você pode executar uma restauração completa do sistema para um disco rígido que seja igual em tamanho ou maior do que aquele que está sendo substituído.  
  
 Ao executar uma restauração completa do sistema, você pode escolher um backup do computador específico para restaurar o sistema, com todos os aplicativos, configurações e configurações familiares para o usuário antes da falha, catástrofe ou roubo. Você também pode escolher quais volumes que você quer restaurar.  
  
 Ao planejar ou Preparando para restaurar o sistema completo em um computador de rede, as seguintes considerações:  
  
### <a name="windows-preinstallation-environment"></a>Ambiente de pré-instalação do Windows  
 Ambiente de pré-instalação do Windows (Windows PE) é um sistema operacional mínimo desenvolvido para preparar um computador para instalação do Windows. Para servidores que executam o Windows Server Essentials, o Windows PE é instalado automaticamente quando você inserir a mídia de restauração em um computador para ser restaurado. Para servidores que executam o Windows Server Essentials, o Windows PE é instalado automaticamente quando você inicia o computador com o serviço de restauração do cliente ou com a unidade flash USB.  
  
 Windows PE não oferece suporte a conexões sem fio. Por isso, o computador está sendo restaurado deve estar conectado fisicamente à rede pequenas empresas.  
  
### <a name="bitlocker"></a>BitLocker  
 Criptografia de unidade de disco BitLocker (BitLocker) é um recurso de proteção de dados que está disponível em algumas versões do Windows Vista, Windows 7 e Windows 8. O BitLocker protege contra roubo de dados ou exposição em computadores perdidos ou roubados e oferece a exclusão de dados mais segura quando computadores são encerrados.  
  
 **Para o Windows Server Essentials:** se o computador que você precisa restaurar foi criptografado com o BitLocker (se ele estava apenas a unidade do sistema operacional ou a unidade do sistema operacional e único ou vários outras unidades fixas), você ainda pode usar a mídia de restauração do sistema completo contida no CD fornecido com o servidor e o Assistente de restauração do sistema completo para instalar novamente a imagem de disco rígido, incluindo o sistema operacional, a partir de um backup e restaurar os dados para o computador de novo ou reparado.  
  
 **Para o Windows Server Essentials:** se o computador que você precisa restaurar foi criptografado com o BitLocker (se ele estava apenas a unidade do sistema operacional ou a unidade do sistema operacional e único ou vários outras unidades fixas), você ainda pode usar o Assistente de restauração do sistema completo para instalar novamente a imagem de disco rígido, incluindo o sistema operacional, a partir de um backup e restaurar os dados para o computador de novo ou reparado.  
  
 Quando o servidor faz backup de arquivos, pastas e unidades, uma versão não criptografada é salvo no servidor. Durante a restauração do sistema completo, essa versão não criptografada é copiado para o computador.  
  
> [!NOTE]
>  Após uma restauração completa do sistema bem-sucedidos, você deve reativar o BitLocker no computador.  
>   
>  Para obter instruções sobre como habilitar o BitLocker em computadores que executam o Windows 8, consulte [BitLocker: como habilitar o BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254918).  
>   
>  Para obter instruções sobre como habilitar o BitLocker em computadores que executam o Windows 7, consulte [BitLocker Drive Encryption Step-by-Step guia para o Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=140225).  
  
 Para saber mais sobre noções básicas de criptografia de unidade de disco BitLocker, consulte [BitLocker perguntas frequentes (FAQ)](https://go.microsoft.com/fwlink/p/?LinkID=254917).  
  
### <a name="encrypting-file-system-encrypted-files"></a>Criptografar arquivos criptografados com o sistema de arquivos  
 O recurso de Encrypting File System (EFS) no Windows pode fornecer criptografia no nível do arquivo com base em usuário adicionais para diferentes níveis de segurança entre vários usuários do mesmo computador. É importante observar que, ao contrário de unidades criptografadas pelo BitLocker, arquivos e pastas criptografados via EFS continuem a ser criptografados em qualquer backup do computador. EFS não está disponível no Windows XP Home Edition, Windows Vista Starter, Windows Vista Home Basic, Windows Vista Home Premium, Windows 7 Starter, Windows 7 Home Basic, Windows 7 Home Premium ou Windows 8. Só está disponível no Windows 8 Pro.  
  
> [!WARNING]
>  Ao contrário do BitLocker, você só poderá acessar arquivos protegidos por EFS de dentro do sistema operacional que criptografados-los.  
  
### <a name="disk-partitions"></a>Partições de disco  
 Se o tamanho do disco rígido no novo computador for o mesmo, ou maior do que o original, o disco rígido unidade de disco é reformatada e reparticionada automaticamente. Consulte a tabela abaixo para obter informações específicas:  
  
|Computador original|Computador novo ou restaurado|  
|-----------------------|------------------------------|  
|Único disco com várias partições|Disco com várias partições e qualquer espaço extra é alocado para a última partição|  
|Único disco com uma única partição|Único disco com uma única partição e todo o espaço disponível é usado para a partição única|  
  
> [!NOTE]
>  Caso haja diferenças de layout tamanho e a partição de disco entre o original e o computador novo ou restaurado, você deve usar o gerenciamento de disco para criar as partições apropriadas no computador novo ou restaurado. Você pode fazer isso no Assistente de restauração do sistema completo.  
  
### <a name="raid-and-dynamic-disks"></a>RAID e discos dinâmicos  
 Fazer backup de matriz redundante de discos independentes (RAID) e discos dinâmicos não é permitido.  
  
##  <a name="BKMK_HowDoes"></a>Como funciona o ambiente de restauração do sistema?  
 A mídia de restauração do sistema fornecida com o Windows ServerÂ® 2012 Essentials instala o ambiente de pré-instalação do Windows (Windows PE) no computador. Windows PE substitui o ambiente do MS-DOS e contém os arquivos de programa principal para o Windows. No Windows Server Essentials, há duas maneiras com suporte para restaurar um sistema: usar o serviço de restauração do cliente, que usa uma rede e não depende de mídia, ou usando o USB flash drive.  
  
> [!NOTE]
>  Windows PE não oferece suporte a conexões sem fio. Por isso, o computador está sendo restaurado deve estar conectado fisicamente à rede pequenas empresas.  
  
 O ambiente de restauração do sistema vem com (x86) 32 bits e 64 bits (x64) arquivos de programas. Depois de inserir o sistema de mídia de restauração, escolha a versão apropriada dos arquivos. (x86) 32 bits é o padrão e está selecionado automaticamente se você não escolher dentro de 30 segundos. Se houver atualizações para o sistema completo restaurar arquivos de programas no servidor, os arquivos atualizados são baixados automaticamente no computador.  
  
 Depois que o ambiente de pré-instalação do estiver configurado, inicia o Assistente de restauração do sistema completo. O assistente ajuda você a restaurar o computador a partir de um backup anterior. Você também pode usar o ambiente de restauração do sistema para restaurar um backup para um novo computador com hardware semelhante.  
  
 Na maioria dos casos, os arquivos de programas e drivers contidos no ambiente de restauração do sistema são tudo o que é necessário reiniciar o computador novo ou restaurado. Dependendo do hardware do computador novo ou restaurado, o ambiente de restauração do sistema não pode incluir todo o armazenamento e rede drivers de adaptador que são necessárias quando você reiniciar o computador novo ou restaurado. O Assistente de restauração do sistema completo oferece uma oportunidade para instalar drivers, se necessário. Para obter informações sobre como localizar os drivers de hardware, consulte [onde pode encontrar os drivers para o hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers). Para obter informações sobre como usar a mídia de restauração do sistema, consulte [usando o Assistente de restauração do sistema completo](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using).  
  
##  <a name="BKMK_CreateBootable"></a>Criar uma unidade flash USB inicializável para restaurar um computador cliente  
 Se você precisa restaurar um computador cliente usando um backup existente, mas não é possível localizar o CD restaurar que veio com seu servidor (no Windows Server Essentials) ou se você não quiser configurar o serviço de restauração do cliente em seu servidor (no Windows Server Essentials), você pode criar uma unidade flash USB inicializável. Em seguida, você pode usar a unidade flash USB para iniciar o computador cliente e restaurar o sistema. A unidade flash USB que você usa deve ter pelo menos 1 GB ou maior.  
  
#### <a name="to-create-a-bootable-usb-flash-drive"></a>Para criar uma unidade flash USB inicializável  
  
1.  Abrir o **painel**.  
  
2.  Clique no **dispositivos** guia.  
  
3.  Em **tarefas** painel, clique em **configurações de personalizar o Backup do computador e o histórico de arquivos**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, clique em **tarefas backup do computador cliente**.  
  
4.  Clique no **ferramentas** guia e, em seguida, clique em **criar chave** no **recuperação do computador** seção. O Assistente para criação de chave de recuperação computador é aberta.  
  
5.  Insira um 1 GB ou maior USB flash drive no servidor e siga as instruções no assistente.  
  
    > [!CAUTION]
    >  Todos os dados na unidade flash USB serão excluídos.  
  
##  <a name="BKMK_Using"></a>Usando o Assistente para restauração do sistema completo  
 Depois de usar com êxito a restauração mídia, serviço de restauração do cliente ou unidade flash USB para iniciar o computador e verifique se todos os hardwares drivers são carregados no computador cliente novos ou restaurado, o Assistente de restauração do sistema completo aparece. Este assistente permite que você acessar o servidor, o backup do computador e os volumes de origem que você deseja restaurar o computador e executa o processo de restauração real.  
  
> [!NOTE]
>   Windows Server Essentials não suporta os seguintes cenários de restauração:  
>   
>  -   Restaurar um disco de registro mestre de inicialização (MBR) para um Unified Extensible Firmware Interface (UEFI) com base em computador.  
> -   Restaurar um backup UEFI/GPT para um sistema BIOS.  
>   
>  Se você restaurar dados em qualquer um desses cenários, você não poderá inicializar o sistema. Além disso, você talvez não consiga usar discos rígidos que são maiores do que dois terabytes de tamanho.  
  
 **Pré-requisitos:**  
  
-   Antes de iniciar o sistema completo o processo de restauração, use um cabo de rede (uma conexão com fio) para conectar o computador à mesma rede que o servidor. Certifique-se de que você tenha acesso a todos os discos rígidos no computador cliente.  
  
    > [!WARNING]
    >  Não tente executar uma restauração completa do sistema em um computador que usa uma conexão à rede sem fio.  
  
-   Se você sabe que o computador não tem rede crítica ou drivers de dispositivo de armazenamento, você precisará localizar e copie os drivers em uma unidade flash antes de iniciar o processo de restauração do sistema completo. Para o Windows Server Essentials: Se você estiver usando a mídia de restauração do sistema completo que é fornecida em CD, esse CD deve permanecer na unidade durante a parte inicial do processo de restauração do sistema completo. Portanto, você não deve copiar os drivers ausentes em um CD ou DVD, a menos que você tem uma segunda unidade de CD/DVD. Em vez disso, copie os drivers ausentes em uma unidade flash USB.  
  
     Para obter informações sobre como localizar os drivers para o seu computador, consulte [onde pode encontrar os drivers para o hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
-   Para o Windows Server Essentials: Se você não conseguir localizar CD de restauração do Windows Server Essentials, você pode criar uma unidade flash USB inicializável. Para obter mais informações, consulte [criar uma unidade flash USB inicializável para restaurar um computador cliente](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable).  
  
#### <a name="to-use-the-full-system-restore-wizard"></a>Para usar o Assistente de restauração do sistema completo  
  
1.  Siga um destes procedimentos:  
  
    -   Windows Server Essentials: Ative o computador cliente que você deseja restaurar, insira a mídia de restauração e, em seguida, desligue o computador.  
  
         Ligue o computador novamente e durante o consumo de energia no teste POST (Self), pressione a tecla de função apropriada (F-key) para acessar o Menu do dispositivo de inicialização e, em seguida, selecione a unidade de CD/DVD. O Gerenciador de inicialização do Windows é iniciado.  
  
    -   Windows Server Essentials: Se você estiver usando o serviço de restauração do cliente, reinicie o computador usando o **inicialização de rede** opção. Caso contrário, inicie o computador usando a chave USB.  
  
         Ligue o computador novamente e durante o consumo de energia no teste POST (Self), pressione a tecla de função apropriada (F-key) para acessar o Menu do dispositivo de inicialização e, em seguida, selecione **inicialização de rede** (ou você pode optar por inicializar a partir do PEN drive). O Gerenciador de inicialização do Windows é iniciado.  
  
    > [!NOTE]
    >  Verifique a documentação do fabricante do computador para determinar qual tecla de função acessa o Menu do dispositivo de inicialização.  
  
2.  A mídia de restauração do computador contém 32 bits (x86) e opções de inicialização de 64 bits (x64). No Gerenciador de inicialização do Windows, escolha **restauração do sistema completo (x86)** ou **restauração do sistema completo (x64)**. Se os drivers de hardware do computador 32 bits, escolha x86; Se eles forem 64 bits, escolha x64. Arquivos do Windows são carregados e o Assistente de restauração do sistema completo executa uma verificação para garantir que todos os drivers de hardware estão disponíveis.  
  
3.  No **completo sistema Assistente para restauração** janela, escolha seu idioma preferido e, em seguida, clique na seta.  
  
4.  Escolha apropriada **formato de hora e moeda**e o **teclado ou método de entrada** para esse computador. Clique em **continuar**.  
  
5.  Se drivers estão ausentes, será exibida a mensagem que o processo de restauração não é possível verificar os drivers. Clique em **fechar**e depois na caixa de diálogo de boas-vindas clique **carregar drivers**.  
  
    1.  Sobre o **detectar Hardware** caixa de diálogo, clique em **instalar drivers**.  
  
    2.  Insira a unidade flash USB que contém os drivers de hardware e, em seguida, no **Instalar Drivers** caixa de diálogo, clique em **digitalizar**.  
  
    3.  Sobre o **Instalar Drivers** caixa de diálogo, clique em **Okey** quando os drivers são encontrados.  
  
    4.  Sobre o **detectar Hardware** caixa de diálogo, clique em **continuar**.  
  
6.  Se todos os drivers foram encontrados na verificação inicial ou quando todos os drivers críticos são instalados, pela **restauração do sistema completo** janela, clique em **continuar**.  
  
7.  Sobre o **boas-vindas para o Assistente de restauração do sistema completo** página, clique em **próxima**.  
  
8.  O assistente procura em seu servidor.  
  
    1.  Se o assistente não conseguir localizar seu servidor, você terá a opção para pesquisar novamente, ou para inserir o endereço IP do servidor.  
  
    2.  Se vários servidores detectados, você deverá selecionar um.  
  
    3.  Se o servidor está localizado, o **logon < YourServerName\ >** página é exibida.  
  
9. No **logon < YourServerName\ >** página, digite *< AdministratorAccountName\ >* no **nome de usuário** caixa de texto e a senha de conta de administrador no **senha** caixa de texto e clique em **próxima**.  
  
    > [!IMPORTANT]
    >  Você deve usar uma conta de administrador é criada em inglês. Se você não tiver um, você deve criar uma nova conta de administrador. Para fazer isso, primeiro abra o **usuários** guia no painel do servidor, em seguida, definir o formato de idioma do teclado para inglês e, em seguida, execute o **adicionar uma conta de usuário** tarefa para criar a conta de administrador. Em seguida, use a nova conta de administrador para continuar a restaurar o computador cliente.  
  
10. Sobre o **selecionar um computador para restaurar** página, selecione o computador que você quer restaurar e, em seguida, clique em **próxima**. Você pode escolher qualquer uma **< ComputerName\ >: (este computador)** ou escolha outro computador na rede a **outro computador** lista suspensa.  
  
    > [!NOTE]
    >  Se este é um computador que é conhecido pelo servidor (por exemplo, um novo ou um computador redefinido), o **neste computador** opção não é exibida.  
  
11. Sobre o **selecione um backup para restaurar** página, examine a lista de backups disponíveis e selecionar o que você quer restaurar no computador.  
  
    > [!NOTE]
    >  É recomendável que você selecione um backup bem-sucedido (marcado verde). Isso ajuda a garantir que todos os arquivos de sistema e os dados são restaurados com êxito.  
  
12. (*Opcional*) Selecione um backup e, em seguida, clique em **detalhes** para abrir o **detalhes Backup** página e exibir mais informações sobre esse backup. Use as informações sobre o **detalhes Backup** página para comparar vários backups e ajudar a decidir qual backup é a melhor opção. Clique em **fechar** sobre o **detalhes do Backup** página para retornar ao **selecione um backup para restaurar** página.  
  
13. Sobre o **selecione um backup para restaurar** de página, selecione um backup e, em seguida, clique no **próxima** botão.  
  
14. Sobre o **selecionar a opção de restauração** de página, clique em um destes procedimentos e clique em **próxima**.  
  
    > [!NOTE]
    >  Esta página não será exibida se o particionamento automático não é compatível.  
  
    1.  **Permitir que o assistente totalmente restaurar o computador (recomendado)**. Esta opção ajuda a garantir que o computador é restaurado para o estado em que estava somente antes da hora e data do backup que você escolheu. Se você escolher essa opção, pule para a etapa 15.  
  
    2.  **Permitir que eu selecionar volumes restaurar (Avançado)**. Essa opção permite que você escolha os volumes que você quer restaurar e onde desejar restaurá-los. Você também pode criar partições no disco rígido.  
  
15. Sobre o **selecionar os volumes restaurar** página, você pode escolher os volumes que você quer restaurar.  
  
    > [!NOTE]
    >  Esta página será exibida se houver vários discos rígidos no computador de origem de backup, ou se a unidade de destino de restauração tenha menos espaço de armazenamento que a unidade de backup de origem.  
  
    1.  O assistente tenta corresponder os volumes de origem e de destino. Você deve verificar se o mapeamento padrão está correto.  
  
        1.  Para cancelar a seleção de um volume, clique na seta de menu de lista para esse volume e, em seguida, clique em **None**.  
  
        2.  Quando você terminar de selecionar os volumes, clique em **próxima**.  
  
    2.  Se o volume de origem e o volume de destino são do mesmo tamanho ou se o tamanho de fonte é menor que o destino, uma seta verde aparece entre os dois. Se houver uma incompatibilidade de tamanho de volume (onde o volume de origem é maior do que o volume de destino), um X vermelho é exibido entre a origem e o destino.  
  
        > [!NOTE]
        >  Um X vermelho também pode aparecer se:  
        >   
        >  -   O tamanho de setor de disco do volume de origem não coincide com o tamanho de setor de disco do volume de destino. Isso pode ocorrer se você substituir o disco físico com um disco que tenha um tamanho de setor diferentes, ou se você configurar os espaços de armazenamento (que pode ter um tamanho de setor diferente do que o disco físico).  
        > -   Você atingir o limite de número de cluster. Para restaurar o volume de origem para o volume de destino, você deve formatar o volume de destino com o mesmo tamanho de cluster como o volume de origem. Se o volume de destino for muito grande, e o tamanho do cluster é muito pequeno, você pode alcançar limitação de número de cluster.  
  
        1.  Clique em **executar o Gerenciador de disco (Avançado)**e crie um novo volume é do mesmo tamanho que o volume do sistema reservado.  
  
            > [!NOTE]
            >  Se um computador cliente é Unified Extensible Firmware Interface (UEFI) com base, você deve usar o **diskpart** ferramenta para inicializar o disco do sistema. Para fazer isso, abra uma janela de comando (pressione Ctrl + Alt + Shift por 5 segundos no ambiente do WinPE), execute **diskpart.exe**, e, em seguida, execute os seguintes comandos diskpart:  
            >   
            >  1.  **DISKPART > disco de lista**  
            > 2.  **DISKPART > select disk #***< disk\ >*  
            > 3.  **DISKPART > limpa**  
            > 4.  **DISKPART > Converter gpt**  
            > 5.  **DISKPART > criar o tamanho da partição efi =***100* (onde *100* é um tamanho de partição de exemplo em MB, deve ser igual à partição original)  
            > 6.  **DISKPART > criar o tamanho da partição msr =***128* (onde *128* é um tamanho de partição de exemplo em MB, deve ser igual à partição original)  
            > 7.  **DISKPART > Sair**  
  
        2.  *(Opcional) *Selecione a opção **não atribua uma letra de unidade ou caminho de unidade**.  
  
        3.  Formate o volume como **NTFS**.  
  
        4.  Quando a formatação for concluída, clique com botão direito o volume do sistema novo e clique em **marcar partição como ativa**.  
  
        5.  Se você precisar volumes adicionais para corresponder com outros volumes no backup, repita as etapas *ii* por meio de *iv* para criar e ativar os volumes e feche **gerenciamento de disco**.  
  
        6.  Sobre o **selecionar os volumes restaurar** página, o sistema reservado volume da fonte de backup para o volume do mesmo tamanho que você criou na etapa de mapa *v*.  
  
        7.  Todos os outros volumes de origem são mapeadas para os volumes de destino correspondentes.  
  
        8.  Clique em **próxima** para continuar com a restauração.  
  
16. Sobre o **confirmar volumes para restaurar** página, examine o mapeamento e, em seguida, clique em **próxima**. Se você precisar fazer alterações, clique em **novamente**e, em seguida, repita a etapa 14.  
  
17. O **restaurando < ComputerName\ > de < data e hora de backup\ >** página relata o progresso do processo de restauração.  
  
18. Sobre o **a restauração foi concluída com êxito** página, remova a mídia de restauração e clique em **concluir**. O computador é reiniciado.  
  
    > [!IMPORTANT]
    >  Se a criptografia de unidade de disco BitLocker foi habilitada no computador antes da restauração, você deve habilitar o BitLocker manualmente depois que o computador for reiniciado.  
  
##  <a name="BKMK_FindDrivers"></a>Onde encontrar os drivers para o hardware?  
 Dependendo do hardware do computador novo ou restaurado, a mídia de restauração não pode incluir todo o armazenamento e rede drivers de adaptador que são necessárias quando você reiniciar seu computador restaurado. Você deve determinar quais drivers estão ausentes, localizar os drivers em uma mídia existente ou no site de s do fabricante, copiá-los para uma unidade flash e, em seguida, copiá-los a partir da unidade flash para o novo ou restaurou o computador quando você executa o Assistente de restauração do sistema completo.  
  
 Quando um computador é o backup, os drivers para o computador são salvas no backup. Se a mídia de recuperação não inclui todos os drivers que você precisa, você pode abrir um backup do computador e, em seguida, copie os drivers em uma unidade flash USB.  
  
#### <a name="to-copy-drivers-from-a-backup-to-a-usb-flash-drive"></a>Para copiar os drivers de um backup em uma unidade flash USB  
  
1.  Em outro computador, abra o painel.  
  
2.  Clique em **dispositivos**e, em seguida, clique no computador para o qual você precisa de drivers.  
  
3.  Clique em **restaurar arquivos ou pastas para o computador**. O Assistente de pastas ou restaurar arquivos é aberta.  
  
4.  Clique em backup bem-sucedido mais recente e, em seguida, clique em **próxima**.  
  
5.  Clique em um volume para abrir e clique em **próxima**. Uma janela será aberta que lista os arquivos e pastas no backup.  
  
6.  Insira a unidade flash USB em um conector USB no computador e, em seguida, copie os Drivers para a pasta de restauração do sistema completo para a unidade flash USB.  
  
    > [!NOTE]
    >  Talvez você precise clicar em **um nível acima** até chegar a raiz do volume do sistema.  
  
7.  Remova a unidade flash e insira-o no computador que você está restaurando.  
  
 Você pode usar a unidade flash USB para instalar os drivers para o seu computador quando você restaurá-lo. A restaurar arquivos ou pastas assistente procura outros drivers nessa unidade flash USB ao usar o Assistente de restauração do sistema completo. Os drivers que você provavelmente precisará são o driver do adaptador de rede e drivers de dispositivo de armazenamento.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar o Backup e restauração](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
