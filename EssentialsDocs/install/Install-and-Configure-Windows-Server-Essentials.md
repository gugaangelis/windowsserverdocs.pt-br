---
title: Instalar e configurar o Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 06/17/2013
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e95cf219-46a4-4041-bd81-0c4c2a0622cf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 600aea223ebf48e1370f06070a4c3db7329c6f2f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311654"
---
# <a name="install-and-configure-windows-server-essentials"></a>Instalar e configurar o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_InstallConfigure"></a>   

 Este documento fornece instruções passo a passo para instalar e configurar o Windows Server Essentials. Antes de começar a instalação, examine e conclua as tarefas descritas em [antes de instalar o Windows Server Essentials](Before-You-Install-Windows-Server-Essentials.md).  

 Este documento fornece instruções passo a passo para instalar e configurar o Windows Server Essentials. Antes de começar a instalação, examine e conclua as tarefas descritas em [antes de instalar o Windows Server Essentials](../install/Before-You-Install-Windows-Server-Essentials.md).  
  
 Você instala e configura o Windows Server Essentials em duas etapas:  
  

1.  [Etapa 1: instalar o sistema operacional Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) Nesta etapa, você instala o sistema operacional em seu servidor.  
  
2.  [Etapa 2: configurar o sistema operacional Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) Nesta etapa, você conclui a instalação fornecendo informações sobre sua empresa, configurações de domínio e administrador de rede. Essas informações são usadas para que o servidor fique pronto para uso.  

1.  [Etapa 1: instalar o sistema operacional Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_ManualInstallation) Nesta etapa, você instala o sistema operacional em seu servidor.  
  
2.  [Etapa 2: configurar o sistema operacional Windows Server Essentials](../install/Install-and-Configure-Windows-Server-Essentials.md#BKMK_Step2Configure) Nesta etapa, você conclui a instalação fornecendo informações sobre sua empresa, configurações de domínio e administrador de rede. Essas informações são usadas para que o servidor fique pronto para uso.  

  
###  <a name="step-1-install-the-windows-server-essentials-operating-system"></a><a name="BKMK_ManualInstallation"></a>Etapa 1: instalar o sistema operacional Windows Server Essentials  
  
> [!IMPORTANT]
>  Depois que o sistema operacional for instalado, não personalize seu servidor até concluir [a etapa 2: configurar o sistema operacional Windows Server Essentials](#BKMK_Step2Configure).  
  
 **Tempo de conclusão estimado:** Aproximadamente 30 minutos.  
  
> [!NOTE]
>  Os tempos de conclusão estimados em todo esse procedimento são baseados nos requisitos mínimos de hardware.  
  
##### <a name="to-install-the-operating-system"></a>Para instalar o sistema operacional  
  
1. Conecte seu computador à rede com um cabo de rede.  
  
   > [!IMPORTANT]
   >  Não desconecte o computador da rede durante a instalação. Fazer isso pode provocar falha na instalação.  
  
2. Ligue o computador e, em seguida, insira o DVD do Windows Server Essentials na unidade de DVD.  
  
    Se você estiver realizando uma instalação sem supervisão, conecte a mídia removível (como um disquete ou unidade flash USB) contendo os arquivos de resposta. Dependendo do conteúdo dos arquivos de resposta, você pode não ver alguns ou todas as telas de instalação a seguir.  
  
3. Reinicie seu computador. Quando a mensagem **Pressione qualquer tecla para inicializar a partir do CD ou DVD** aparecer, pressione qualquer tecla.  
  
   > [!NOTE]
   >  Se o computador não iniciar a partir do DVD, garanta que o leitor de CD-ROM esteja listado primeiro na sequência de inicialização do BIOS. Para mais informações sobre a sequência de inicialização do BIOS, consulte a documentação do fabricante do computador.  
  
4. Selecione o **Idioma** que deseja instalar, o **formato de hora e moeda** e o **Teclado ou método de entrada** e então clique em **Avançar**.  
  
5. Clique em **Instalar agora**.  
  
6. Em **Digite a chave do produto**, digite a chave do produto.  
  
7. Leia os **Termos de licença**. Se você aceitá-los, marque a caixa de seleção **Eu aceito os termos de licença** e depois clique em **Avançar**.  
  
   > [!NOTE]
   >  Se escolher não aceitar os termos de licença, a instalação não continua.  
  
8. Em **Que tipo de instalação você deseja?** , clique em **Personalizada: Instalar somente o Windows (avançada)**  
  
9. Em **Onde deseja instalar o Windows?** , selecione o disco rígido em que deseja instalar o sistema operacional Windows. Verifique se todos os discos rígidos internos estão disponíveis para instalação.  
  
    > [!IMPORTANT]
    >   O Windows Server Essentials deve ser instalado como C: volume e o tamanho do volume deve ser de pelo menos 60 GB. Recomenda-se criar duas partições no disco do sistema operacional, e não usar C: (partição do sistema) para armazenar nenhum dado de negócio.  
  
    > [!NOTE]
    >  Se um disco rígido não estiver listado (por exemplo, um disco rígido SATA), é preciso carregar os drivers do dispositivo para esse disco rígido. Obtenha o driver do dispositivo do fabricante e salve-o em uma mídia removível (como um disquete ou unidade flash USB). Conecte a mídia removível ao seu computador e clique em **Carregar Driver**.  
  
     Se você precisar excluir e/ou criar partições, consulte as seguintes etapas:  
  
    1.  Para excluir uma partição, selecione a partição, clique em **Opções de unidade (avançadas)** e clique em **Excluir**. Depois de excluir a partição do sistema, crie uma nova partição usando as instruções na etapa **b** ou **c**.  
  
        > [!NOTE]
        >  Depois de clicar em **Opções de unidade (avançadas)** , essa opção não aparecerá novamente. Nesse caso, ignore a parte da etapa que se refere a opções de unidade.  
  
    2.  Para criar uma partição a partir de um espaço não particionado, clique no disco rígido que deseja particionar, clique em **Opções de unidade (avançadas)** , clique em **Novo**, clique na caixa de texto **Tamanho** e digite o tamanho da partição que deseja criar. Por exemplo, se usar o tamanho de partição recomendado de 120 gigabytes (GB), digite **122880**e depois clique em **Aplicar**. Depois de a partição ter sido criada, clique em **Avançar**. A partição é formatada antes de a instalação continuar.  
  
    3.  Para criar uma partição que use todo o espaço não particionado, clique no disco rígido que você deseja particionar, clique em **Opções de unidade (avançadas)** , clique em **Novo** e clique em **Aplicar** para aceitar o tamanho padrão da partição. Depois de a partição ter sido criada, clique em **Avançar**. A partição é formatada antes de a instalação continuar.  
  
        > [!IMPORTANT]
        >  Você não pode mover o sistema operacional para uma partição diferente depois de concluir esta etapa.  
  
   Durante a instalação, os arquivos temporários são copiados para uma pasta de instalação no seu computador, o que leva cerca de 30 minutos. Após a instalação do sistema operacional Windows Server Essentials, o computador é reiniciado. Agora, você está pronto para configurar o sistema operacional Windows Server Essentials.  
  
###  <a name="step-2-configure-the-windows-server-essentials-operating-system"></a><a name="BKMK_Step2Configure"></a>Etapa 2: configurar o sistema operacional Windows Server Essentials  
  
> [!IMPORTANT]
>  Se você estiver migrando de uma versão anterior do Windows Small Business Server para o Windows Server Essentials, deverá seguir um processo diferente. Para informações sobre a migração de instalações, consulte o seguinte:  
> 
> - [Migrar do Windows SBS 2003](../migrate/Migrate-Windows-Small-Business-Server-2003-to-Windows-Server-Essentials.md)  
>   -   [Migrar do Windows SBS 2008](../migrate/Migrate-Windows-Small-Business-Server-2008-to-Windows-Server-Essentials.md)  
  
 Durante essa fase da instalação, você é solicitado a responder algumas perguntas sobre a sua organização. Essa informação é usada para configurar o sistema operacional.  
  
> [!IMPORTANT]
>  Antes de iniciar essa etapa, garanta que o adaptador de rede local esteja conectado a um roteador ou a um comutador ligado e funcionando adequadamente.  
  
 **Tempo de conclusão estimado:** Aproximadamente 30 minutos  
  
##### <a name="to-configure-the-operating-system"></a>Para configurar o sistema operacional  
  
1.  Na página **Verificar as configurações de data e hora**, clique em **Alterar configurações de data e hora do sistema** para selecionar as configurações de data, hora e fuso horário para o servidor. Ao concluir, clique em **Avançar**.  
  
    > [!IMPORTANT]
    >  Se você estiver instalando o Windows Server Essentials em uma máquina virtual, certifique-se de escolher as mesmas configurações de fuso horário que estão sendo usadas pelo sistema operacional do host. Se as configurações de fuso horário diferirem, a instalação do servidor podem não ser bem-sucedida.  
  
2.  Na página **Escolha o modo de instalação do servidor**, faça uma das seguintes ações:  
  
    1.  Escolha **instalação limpa** para configurar uma nova instalação completa do software Windows Server Essentials Server.  
  
    2.  Escolha **migração de servidor** para instalar o Windows Server Essentials e ingresse esse servidor em um domínio existente do Windows.  
  
3.  Na página **Personalizar o servidor**, insira o nome da sua organização, um nome de domínio interno e o nome do servidor.  
  
    > [!IMPORTANT]
    >  O nome do servidor deve ser exclusivo na sua rede. Não é possível alterar o nome do servidor ou o nome de domínio interno depois de concluir esta etapa.  
  
4.  Clique em **Avançar**.  
  
5.  Na página **Fornecer as informações da conta do administrador**, digite as informações para uma nova conta de administrador.  
  
    > [!CAUTION]
    >  Não nomeie o administrador da conta de administrador de rede ou o administrador de rede. Esses nomes de conta são reservados para uso do sistema.  
  
6.  Na página **Fornecer as informações da conta de usuário padrão**, digite as informações para uma nova conta de usuário padrão e clique em **Avançar**.  
  
7.  Na página **Manter o servidor atualizado automaticamente**, selecione como deseja receber atualizações do Windows para o servidor e depois clique em **Avançar**.  
  
8.  A página **Atualizando e preparando o servidor** exibe o progresso do processo de instalação final. Isso leva tempo para concluir, e o computador reiniciará algumas vezes.  
  
9. Depois de o último servidor reiniciar, a página **Seu servidor está pronto para ser usado** aparece. Clique em **Fechar**.  
  
10. Clique no bloco Dashboard na tela **Inicial** e, no Dashboard, conclua as tarefas de **Configurar meu Servidor** na página **Home**. Você deve concluir essas tarefas imediatamente após a conclusão da instalação do Windows Server Essentials.  
  
> [!NOTE]
>  Depois de a instalação terminar, você é automaticamente conectado no servidor com a nova conta de administrador adicionada durante a instalação. A senha da conta de Administrador integrada é definida para a mesma senha que a nova conta de administrador, e então a conta de Administrador integrada é desativada.  
  
 Você pode usar o servidor recém-instalado por um período limitado (conhecido como o período de avaliação) sem inserir uma chave do produto (Product Key). Depois do período de avaliação, é preciso inserir uma chave de produto para ativar o servidor ou estender o período de avaliação. É possível estender o período de avaliação um máximo de duas vezes. Quando atingir o número máximo de dias permitidos para o período de avaliação, é preciso ativar o servidor com uma chave de produto.  
  
### <a name="customize-windows-server-essentials"></a>Personalizar o Windows Server Essentials  
 A **Home** Page do painel do Windows Server Essentials vincula-se às tarefas de **instalação** que você deve concluir imediatamente após a instalação do servidor. Ao executar essas tarefas, você pode ajudar a proteger as informações armazenadas no servidor e habilitar os recursos disponíveis no Windows Server Essentials.  
  
 Se você escolher não realizar as tarefas, os usuários podem não ter acesso a alguns recursos de rede. Para retornar a essas tarefas mais tarde, retorne à **Home** Page do painel do Windows Server Essentials.  
  
 A tabela a seguir define os itens que podem aparecer na lista de tarefas de configuração.  
  
|{1&gt;Tarefa&lt;1}|Descrição
|----------|-----------------|  
|Obter atualizações para outros produtos da Microsoft|Clique nessa tarefa para acessar um link que executa uma ferramenta que permite que você especifique se deseja usar Microsoft Update para obter automaticamente atualizações para o Windows Server Essentials e outros produtos da Microsoft, como o Office.  
|Adicionar contas de usuário|Clique nessa tarefa para visualizar breves informações sobre a adição de contas de usuário. Um link para executar o **Assistente para Adicionar uma Conta de Usuário** é fornecido. Para obter mais informações, consulte [Incluir uma conta de usuário](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
|Adicionar pastas de servidor|Clique nessa tarefa para visualizar breves informações sobre a adição de pastas do servidor. Um link para executar o **Assistente para Adicionar uma Pasta** é fornecido. Também é fornecido um link para um tópico de ajuda online sobre usar Pastas do Servidor. Para obter mais informações, consulte [Incluir ou mover uma pasta no servidor](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5). 
|Configurar Backup do Servidor|Clique nessa tarefa para visualizar breves informações sobre usar o Servidor de Backup para proteger seus dados. Um link para executar o **Assistente para Configurar o Servidor de Backup** é fornecido. Para obter mais informações, consulte [Configurar ou personalizar o backup do servidor](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1). 
|Configurar o Acesso em Qualquer Local|Clique nesta tarefa para exibir informações resumidas sobre o recurso de acesso em qualquer local no Windows Server Essentials. Um link para a página **Configurações do Acesso em Qualquer Local** é fornecido. Para obter mais informações, consulte [gerenciar o acesso em qualquer local](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md). 
|Configurar notificação de alerta por email|Clique nessa tarefa para visualizar breves informações sobre notificação de alerta por email. Um link para executar a ferramenta **Configurar notificação por email para alertas** é fornecido. Para obter mais informações, consulte [Configurar notificações por email para alertas](../manage/Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email).  
|Configurar Servidor de Mídia|Clique nessa tarefa para visualizar breves informações sobre o uso do Servidor de Mídia para compartilhar músicas, vídeos e arquivos de imagem. Um link para a página  **Configurações de Mídia** é fornecido. Também é fornecido um link para um tópico de ajuda online com mais informações sobre o Servidor de Mídia. Para obter mais informações, consulte [gerenciar mídia digital](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md). 
|Computadores conectados|Clique nessa tarefa para visualizar breves informações sobre como conectar um computador em rede ao servidor. Para obter mais informações, consulte [Computadores conectados ao servidor](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).
  
## <a name="see-also"></a>Consulte também  
  
-   [Instalar o Windows Server Essentials](Install-Windows-Server-Essentials.md)

