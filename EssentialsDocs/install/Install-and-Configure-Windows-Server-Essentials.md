---
title: Instalar e configurar o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 06/17/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cdad118c30fbf303b55ec7ea25bbe3e209c016db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="install-and-configure-windows-server-essentials"></a>Instalar e configurar o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_InstallConfigure"></a>   

 Este documento fornece instruções passo a passo para instalar e configurar o Windows Server Essentials. Antes de começar a instalação, revise e conclua as tarefas que são descritas na [antes de você instalar o Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md).  

 Este documento fornece instruções passo a passo para instalar e configurar o Windows Server Essentials. Antes de começar a instalação, revise e conclua as tarefas que são descritas na [antes de você instalar o Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md).  
  
 Instalar e configurar o Windows Server Essentials em duas etapas:  
  

1.  [Etapa 1: Instalar o sistema operacional Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) nesta etapa, você instalará o sistema operacional em seu servidor.  
  
2.  [Etapa 2: Configurar o sistema operacional Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) nesta etapa, você deve concluir a instalação, fornecendo informações sobre sua empresa, as configurações do domínio e administrador de rede. Essas informações são usadas para preparar o servidor para uso.  

1.  [Etapa 1: Instalar o sistema operacional Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) nesta etapa, você instalará o sistema operacional em seu servidor.  
  
2.  [Etapa 2: Configurar o sistema operacional Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) nesta etapa, você deve concluir a instalação, fornecendo informações sobre sua empresa, as configurações do domínio e administrador de rede. Essas informações são usadas para preparar o servidor para uso.  

  
###  <a name="BKMK_ManualInstallation"></a>Etapa 1: Instalar o sistema operacional Windows Server Essentials  
  
> [!IMPORTANT]
>  Depois que o sistema operacional é instalado, não personalizar seu servidor até que você termine [etapa 2: configurar o sistema operacional Windows Server Essentials](#BKMK_Step2Configure).  
  
 **Tempo de conclusão estimado:** aproximadamente 30 minutos.  
  
> [!NOTE]
>  Os tempos de conclusão estimado durante esse procedimento se baseiam em requisitos mínimos de hardware.  
  
##### <a name="to-install-the-operating-system"></a>Para instalar o sistema operacional  
  
1.  Conecte o computador à rede com um cabo de rede.  
  
    > [!IMPORTANT]
    >  Não desconecte o computador da rede durante a instalação. Isso pode causar falhas na instalação.  
  
2.  Ligue o computador e, em seguida, insira o DVD do Windows Server Essentials na unidade de DVD.  
  
     Se você estiver executando uma instalação autônoma, conecte a mídia removível (como um disquete ou uma unidade flash USB) que contém os arquivos de resposta. Dependendo do conteúdo de seus arquivos de resposta, você talvez não veja alguns ou qualquer um dos seguintes telas de instalação.  
  
3.  Reinicie o computador. Quando a mensagem **pressione qualquer tecla para inicializar do CD ou DVD** aparecer, pressione qualquer tecla.  
  
    > [!NOTE]
    >  Se o computador não for iniciado do DVD, certifique-se de que a unidade de CD-ROM está listada primeiro na sequência de inicialização do BIOS. Para saber mais sobre a sequência de inicialização do BIOS, consulte a documentação do fabricante do computador.  
  
4.  Selecione o **idioma** que você deseja instalar, **formato de hora e moeda**, e **teclado ou método de entrada**e clique em **próxima**.  
  
5.  Clique em **instalar agora**.  
  
6.  Em **inserir a chave do produto**, digite a chave do produto.  
  
7.  Ler o **termos de licença**. Se você aceitar, selecione o **aceito os termos de licença** caixa de seleção e, em seguida, clique em **próxima**.  
  
    > [!NOTE]
    >  Se você não optar por aceitar os termos de licença, a instalação não continua.  
  
8.  Em **qual tipo de instalação você deseja? **, clique em **personalizado: instalar o Windows somente (Avançado)**  
  
9. Em **onde você deseja instalar o Windows? **, selecione o disco rígido em que você deseja instalar o sistema operacional Windows. Verifique se todos os seus discos rígidos internos estão disponíveis para instalação.  
  
    > [!IMPORTANT]
    >   Windows Server Essentials deve ser instalado como volume c: e o tamanho do volume deve ter pelo menos 60 GB. É recomendável que você crie duas partições no disco do sistema operacional e não usa a unidade c: (partição do sistema) para armazenar dados comerciais.  
  
    > [!NOTE]
    >  Se um disco rígido não estiver listado (por exemplo, um disco rígido de Serial Advanced Technology Attachment (SATA)), você deve carregar os drivers de dispositivo para esse disco rígido. Obter o driver de dispositivo do fabricante e salve-o em uma mídia removível (como um disquete ou uma unidade flash USB). Conecte a mídia removível ao computador e clique em **carregar Driver**.  
  
     Se você precisar excluir e/ou criar partições, consulte as seguintes etapas:  
  
    1.  Para excluir uma partição, selecione a partição, clique em **opções de unidade (avançadas)**e clique em **excluir**. Depois de excluir a partição do sistema, crie uma nova partição usando as instruções nos dois passos **b** ou etapa **c**.  
  
        > [!NOTE]
        >  Depois de clicar em **opções de unidade (avançadas)**, opção não será exibida novamente. Nesse caso, pule a parte da etapa que se refere às opções de unidade.  
  
    2.  Para criar uma partição de um espaço não particionado, clique no disco rígido que você deseja particionar, clique em **opções de unidade (avançadas)**, clique em **nova**e, em seguida, no **tamanho** texto, digite o tamanho da partição que você deseja criar. Por exemplo, se você usar o tamanho de partição recomendado de 120 gigabytes (GB), digite **122880**e clique em **aplicar**. Depois que a partição é criada, clique em **próxima**. A partição é formatada antes que a instalação continua.  
  
    3.  Para criar uma partição que usa todo o espaço não particionado, clique no disco rígido que você deseja particionar, clique em **opções de unidade (avançadas)**, clique em **nova**e clique em **aplicar** para aceitar o tamanho de partição padrão. Depois que a partição é criada, clique em **próxima**. A partição é formatada antes que a instalação continua.  
  
        > [!IMPORTANT]
        >  Você não pode mover o sistema operacional em uma partição diferente depois de concluir essa etapa.  
  
 Durante a instalação, arquivos temporários são copiados para uma pasta de instalação em seu computador, o que leva cerca de 30 minutos. Depois que o sistema operacional Windows Server Essentials está instalado, seu computador é reiniciado. Agora, você estará pronto para configurar o sistema operacional Windows Server Essentials.  
  
###  <a name="BKMK_Step2Configure"></a>Etapa 2: Configurar o sistema operacional Windows Server Essentials  
  
> [!IMPORTANT]
>  Se você estiver migrando de uma versão anterior do Windows Small Business Server para Windows Server Essentials, você deve seguir um processo diferente. Para obter informações sobre instalações de migração, consulte o seguinte:  
>   
>  -   [Migrar do Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
> -   [Migrar do Windows SBS 2008](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 Durante essa fase da instalação, você precisará responder a algumas perguntas sobre sua organização. Essas informações são usadas para configurar o sistema operacional.  
  
> [!IMPORTANT]
>  Antes de iniciar esta etapa, certifique-se de que o adaptador de rede local está conectado a um roteador ou um botão que está ativado e funcionando corretamente.  
  
 **Tempo de conclusão estimado:** aproximadamente 30 minutos  
  
##### <a name="to-configure-the-operating-system"></a>Para configurar o sistema operacional  
  
1.  Sobre o **configurações de hora e verifique se a data** página, clique em **alterar data do sistema e configurações de hora** para selecionar as configurações de data, hora e fuso horário do seu servidor. Quando tiver terminado, clique em **próxima**.  
  
    > [!IMPORTANT]
    >  Se você estiver instalando o Windows Server Essentials em uma máquina virtual, certifique-se de que você escolher as mesmas configurações de fuso horário que estão sendo usadas pelo sistema operacional host. Se as configurações de fuso horário forem diferentes, a instalação do servidor, talvez não seja bem-sucedida.  
  
2.  Sobre o **modo de instalação do servidor de escolher** página, siga um destes procedimentos:  
  
    1.  Escolha **instalação limpa** para configurar uma instalação do software de servidor do Windows Server Essentials totalmente nova.  
  
    2.  Escolha **migração do servidor** para instalar o Windows Server Essentials e associar este servidor a um domínio do Windows existente.  
  
3.  Sobre o **personalizar seu servidor** de página, insira o nome da sua organização, um nome de domínio interno e o nome do servidor.  
  
    > [!IMPORTANT]
    >  O nome do servidor deve ser um nome exclusivo em sua rede. Depois de concluir essa etapa, você não pode alterar o nome do servidor ou o nome de domínio interno.  
  
4.  Clique em **próxima**.  
  
5.  Sobre o **fornecer suas informações de conta de administrador** página, digite as informações para uma nova conta de administrador.  
  
    > [!CAUTION]
    >  Não dê um nome a conta de administrador de rede administrador ou o administrador de rede. Esses nomes de conta são reservados para uso pelo sistema.  
  
6.  Sobre o **fornecer suas informações de conta de usuário padrão** página, digite as informações para uma nova conta de usuário padrão e, em seguida, clique em **próxima**.  
  
7.  Sobre o **manter seu servidor atualizados automaticamente** página, escolha como deseja receber atualizações do Windows para o servidor e, em seguida, clique em **próxima**.  
  
8.  O **atualizando e Preparando seu servidor** página exibe o progresso do processo de instalação final. Isso leva tempo para serem concluídos, e o computador será reiniciado algumas vezes.  
  
9. Após o último servidor reiniciar, o **o servidor está pronto para ser usado** página será exibida. Clique em **fechar**.  
  
10. Clique no bloco do painel no **iniciar** de tela e, em seguida, no painel, conclua a **definir meu servidor** tarefas no **Home** página. Você deve concluir essas tarefas imediatamente após a instalação do Windows Server Essentials é concluído.  
  
> [!NOTE]
>  Após a conclusão da instalação, você está conectado automaticamente servidor com a nova conta de administrador que você adicionou durante a instalação. A senha de conta de administrador interno é definida como a mesma senha como a nova conta de administrador e, em seguida, a conta de administrador interno é desabilitada.  
  
 Você pode usar seu servidor recém-instalado por um período limitado de tempo (conhecido como o período de avaliação) sem inserir uma chave do produto. Após o período de avaliação, você deve inserir uma chave do produto para ativar o servidor ou estender o período de avaliação. Você pode estender o período de avaliação um máximo de duas vezes. Quando você atingir o número máximo de dias permitido para o período de avaliação, você deve ativar seu servidor com uma chave do produto.  
  
### <a name="customize-windows-server-essentials"></a>Personalizar o Windows Server Essentials  
 O **Home** página do Windows Server Essentials painel vincula a **instalação** tarefas que você deve concluir imediatamente depois que você instalar o servidor. Ao realizar essas tarefas, você pode ajudar a proteger as informações que são armazenadas no servidor e habilitar os recursos que estão disponíveis no Windows Server Essentials.  
  
 Se você optar por não executar as tarefas, os usuários talvez não tenha acesso a alguns recursos de rede. Para retornar para essas tarefas mais tarde, retornar para o Windows Server Essentials Dashboard **Home** página.  
  
 A tabela a seguir define os itens que podem aparecer na lista de tarefas de configuração.  
  
|Tarefa|Descrição
|----------|-----------------|  
|Obter atualizações para outros produtos da Microsoft|Clique nessa tarefa para acessar um link que executa uma ferramenta que permite especificar se deseja usar o Microsoft Update para obter automaticamente as atualizações para o Windows Server Essentials e outros produtos da Microsoft como o Office.  
|Adicionar contas de usuário|Clique nessa tarefa para exibir breves informações sobre como adicionar contas de usuário. Um link para executar o **adicionar um Assistente de conta de usuário** é fornecido. Para obter mais informações, consulte [adicionar uma conta de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
|Adicionar pastas de servidor|Clique nessa tarefa para exibir breves informações sobre como adicionar pastas de servidor. Um link para executar o **um Assistente para adicionar pasta** é fornecido. Também fornecido é um link para um tópico da Ajuda online sobre o uso de pastas do servidor. Para obter mais informações, consulte [adicionar ou mover uma pasta de servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5). 
|Configurar o Backup do servidor|Clique nessa tarefa para exibir breves informações sobre como usar o Backup do servidor para proteger seus dados. Um link para executar o **definir o Assistente de Backup do servidor** é fornecido. Para obter mais informações, consulte [definido para cima e para personalizar o backup do servidor](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1). 
|Configurar o acesso em qualquer lugar|Clique nessa tarefa para exibir breves informações sobre o recurso de acesso em qualquer local no Windows Server Essentials. Um link para o **as configurações de acesso em qualquer lugar** página é fornecida. Para obter mais informações, consulte [gerenciar o acesso em qualquer local](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md). 
|Configurar a notificação de alerta por email|Clique nessa tarefa para exibir informações breves sobre notificação de alerta por email. Um link para executar o **configurar uma notificação de email para alertas** ferramenta é fornecida. Para obter mais informações, consulte [configurar notificações de email para alertas](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
|Configurar o servidor de mídia|Clique nessa tarefa para exibir breves informações sobre como usar o servidor de mídia para compartilhar músicas, vídeos e arquivos de imagem. Um link para o **configurações Media** página é fornecida. Também fornecido é um link para um tópico da Ajuda online para saber mais sobre o servidor de mídia. Para obter mais informações, consulte [gerenciar mídia Digital](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md). 
|Conectar computadores|Clique nessa tarefa para exibir breves informações sobre como se conectar a um computador da rede para o servidor. Para obter mais informações, consulte [conectar computadores para o servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).
  
## <a name="see-also"></a>Consulte também  
  
-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)

