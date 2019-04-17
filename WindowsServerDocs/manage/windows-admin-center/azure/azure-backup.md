---
title: Fazer backup de seus servidores do Windows do Windows Admin Center com Backup do Azure
description: Use o Windows Admin Center (Project Honolulu) para servidores de Backup do Windows com o Backup do Azure
ms.technology: manage
ms.topic: article
author: saurabhsensharma
ms.author: saurse
ms.date: 03/25/2019
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 3983d0b65bc69ef9fd40f3c8e196d40534b1b8f9
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296842"
---
# Fazer backup de seus servidores do Windows do Windows Admin Center com Backup do Azure

>Aplicável a: Windows Admin Center Preview, o Windows Admin Center

[Saiba mais sobre a integração do Azure com o Windows Admin Center.](../plan/azure-integration-options.md)

Windows Admin Center simplifica o processo de backup de seus servidores do Windows Azure e protege contra exclusões acidentais ou mal-intencionados, danos e até mesmo ransomware. Para automatizar a instalação, você pode conectar o gateway do Windows Admin Center no Azure.

Use as informações a seguir para configurar o Backup para você, Windows Server e criar uma política de Backup para fazer backup de Volumes do seu servidor e o estado do sistema Windows do Centro de administração do Windows.

## Qual é o Backup do Azure e como ele funciona com o Windows Admin Center? 

**Backup do Azure** é o serviço com base em Azure, você pode usar para fazer backup (ou proteger) e restaurar seus dados na nuvem da Microsoft. Backup do Azure substitui seu local existente ou a solução de backup externo com uma solução baseada em nuvem que é seguro, confiável e custo competitiva.
[Saiba mais sobre o Backup do Azure](https://docs.microsoft.com/azure/backup/backup-overview).

Backup do Azure oferece vários componentes que você baixar e implantar no computador apropriado, servidor, ou na nuvem. O componente ou agente, que você implantar depende do que você queira proteger. Todos os componentes de Backup do Azure (não importa se você está protegendo dados no local ou no Azure) podem ser usados para fazer backup de dados em um cofre de serviços de recuperação no Azure.

A integração do Backup do Azure no Centro de administração do Windows é ideal para fazer backup de volumes e os sistema Windows estado local Windows servidores físicos ou virtuais. Isso torna o por um mecanismo abrangente fazer backup de servidores de arquivos, controladores de domínio e servidores Web IIS.

Windows Admin Center expõe a integração de Backup do Azure por meio da ferramenta de **Backup** nativa. A ferramenta de **Backup** fornece instalação, gerenciamento e monitoramento experiências para iniciar rapidamente fazer backup de seus servidores, execute comuns de backup e restaurar as operações e para monitorar a integridade geral de backup de seus servidores do Windows.

## Pré-requisitos e planejamento

- Uma conta do Azure com pelo menos uma assinatura ativa
- O destino de servidores do Windows que você deseja backup deve ter acesso à Internet para Azure
- [Conectar seu gateway do Windows Admin Center no Azure](azure-integration.md)

Para iniciar o fluxo de trabalho para fazer backup do Windows Server, abra uma conexão de servidor, clique na ferramenta **Backup** e siga as etapas mencionadas abaixo.

## Configurar o Backup do Azure
Quando você clica na ferramenta **Backup** para uma conexão de servidor no qual o Backup do Azure não está habilitado ainda, você verá a tela de **boas-vindas para Backup do Azure** . Clique no botão **definir o Backup do Azure** . Isso seria abrir o Assistente de instalação do Backup do Azure. Siga as etapas listadas abaixo no Assistente para fazer backup do servidor.

Se o Backup do Azure já está configurado, clicando na ferramenta **Backup** abrirá o **Painel de Backup**. Consulte a seção ([gerenciamento e monitoramento](#management-and-monitoring)) para descobrir as operações e tarefas que podem ser executadas no painel.

### Etapa 1: Logon Microsoft Azure
Entrar no você conta do Azure. 

> [!NOTE]
> Se você tiver se conectado seu gateway do Windows Admin Center no Azure, você deve ser automaticamente conectado ao Azure. Você pode clicar em **saída** ainda mais entrar como um usuário diferente.

### Etapa 2: Configurar o Backup do Azure
Selecione as configurações apropriadas para Backup do Azure, conforme descrito a seguir

 - **Id da assinatura:** A assinatura do Azure que deseja usar o backup do servidor Windows Azure. Todos os ativos Azure como o grupo de recursos do Azure, o Cofre de serviços de recuperação será criado na inscrição selecionada.
 - **Cofre:** O Cofre de serviços de recuperação onde os backups dos seus servidores serão armazenados. Você pode selecionar na compartimentos existentes ou Windows Admin Center criará um novo cofre.  
 - **Grupo de recursos:** O grupo de recursos do Azure é um contêiner para uma coleção de recursos. O Cofre de serviços de recuperação é criado ou contido no grupo de recursos especificado. Você pode selecionar de grupos de recursos existentes ou Windows Admin Center criará uma nova.
 - **Local:** A região do Azure onde o Cofre de serviços de recuperação será criado. É recomendável selecionar região do Azure mais próxima ao Windows Server.

### Etapa 3: Selecionar itens de Backup e agendamento

- Selecione o que você deseja fazer backup do seu servidor. Windows Admin Center permite que você escolha de uma combinação de **Volumes** e o **Estado do sistema Windows** , dando para o tamanho estimado de dados que estão selecionados para backup.

> [!NOTE]
> O primeiro backup é um backup completo de todos os dados selecionados. No entanto, backups subsequentes são natureza incrementais e transferir somente as alterações nos dados desde o backup anterior.

- Selecione vários predefinido **Agendamentos de Backup** para você o estado do sistema e/ou Volumes.

### Etapa 4: Insira a senha de criptografia

- Insira uma **Senha de criptografia** de sua preferência (mínimos 16 caracteres).  **Backup do Azure** protege os dados de backup com uma senha de criptografia configurado pelo usuário e gerenciada pelo usuário. A senha de criptografia é necessária para recuperar dados de Backup do Azure.

> [!NOTE]
> A senha deve ser armazenada em um local externo seguro, como outro servidor ou o [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal). A Microsoft não armazenar a senha e não pode recuperar ou redefinir a senha, caso ele seja perdido ou esquecido.

- Revisar todas as configurações e clique em **Aplicar**

Windows Admin Center, em seguida, executará as seguintes operações

1. Criar um grupo de recursos do Azure, se ele ainda não existir
2. Criar um cofre de serviços de recuperação do Azure conforme especificado
3. Instalar e registrar o agente de serviços de recuperação do Microsoft Azure para o cofre
4. Crie o agendamento de Backup e a retenção de acordo com as opções selecionadas e associá-los com o Windows Server.

## Gerenciamento e monitoramento

Quando você tiver com êxito instalação Backup do Azure, você veria **Backup painel** quando você abrir a ferramenta de Backup para uma conexão de servidor existente. Você pode executar as seguintes tarefas no **Painel de Backup**

- **Acessar o cofre do Azure:** Você pode clicar no link do **Cofre de serviços de recuperação** na guia **Visão geral** do **Painel de Backup** para ser levado para o cofre do Azure para executar um [conjunto avançado de operações de gerenciamento](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server)
- **Executar um backup ad hoc:** Clique em **Backup agora** para executar um backup ad hoc. 
- **Trabalhos de monitor e configurar notificações de alerta:** Navegue até a guia **trabalhos** do painel para monitorar em andamento ou ultrapassou trabalhos e [Configurar notificações de alerta](https://docs.microsoft.com/azure/backup/backup-azure-manage-windows-server#configuring-notifications-for-alerts) para receber emails para qualquer falha trabalhos ou outros alertas relacionadas a backup.
- **Pontos de recuperação de modo de exibição e recupere dados:** Clique na guia **Pontos de recuperação** do painel para exibir os pontos de recuperação e clique em **Recuperar dados** para obter as etapas para recuperar dados do Azure.