---
title: Etapa 1 – Instalar a função de servidor do WSUS
description: Tópico sobre o WSUS (Serviço de Atualização do Windows Server) – descreve como instalar a função de servidor usando Gerenciador do Servidor
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: fabc8619-350e-403b-96f8-116424931300
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 22554a9669c30cc827c509824f187fbaaedb1272
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361705"
---
# <a name="step-1-install-the-wsus-server-role"></a>Etapa 1: Instale a Função de Servidor do WSUS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A próxima etapa na implantação do servidor do WSUS é instalar a função de servidor do WSUS. O procedimento a seguir descreve como instalar a função de servidor do WSUS usando o Gerenciador do Servidor.

> [!IMPORTANT]
> Este procedimento de instalação só trata sobre como instalar o WSUS usando o WID (Banco de Dados Interno do Windows). Os procedimentos para instalar o WSUS usando o Microsoft SQL Server estão documentados [neste artigo](https://social.technet.microsoft.com/wiki/contents/articles/10020.installing-wsus-server-role-on-windows-server-2012-with-microsoft-sql-database.aspx).

### <a name="to-install-the-wsus-server-role"></a>Para instalar a função de servidor do WSUS

1.  Faça logon no servidor em que pretende instalar a função de servidor do WSUS usando uma conta que seja membro do grupo Administradores Locais.

2.  No **Gerenciador do Servidor**, clique em **Gerenciar** e depois em **Adicionar Funções e Recursos**.

3.  Na página **Antes de começar** , clique em **Avançar**.

4.  Na página **Selecionar tipo de instalação**, confirme se a opção **Instalação baseada em função ou recurso** está selecionada e clique em **Avançar**.

5.  Na página **Selecionar servidor de destino**, escolha a localização do servidor (de um pool de servidores ou de um disco rígido virtual). Depois de selecionar a localização, escolha o servidor em que deseja instalar a função de servidor do WSUS e clique em **Avançar**.

6.  Na página **Selecionar funções de servidor**, escolha **Windows Server Update Services**.  **Adicionar recursos necessários para o Windows Server Update Services** se abre. Clique em **Adicionar Recursos** e depois em **Avançar**.

7.  Clique na página **Selecionar recursos**. Mantenha as seleções padrão e clique em **Avançar**.

    > [!IMPORTANT]
    > O WSUS só exige a configuração da função de Servidor Web padrão. Se você for solicitado a adicionar configuração de função de Servidor Web ao configurar o WSUS, aceite tranquilamente os valores padrão e continue a configuração do WSUS.

8.  Na página **Windows Server Update Services** , clique em **Avançar**.

9. Na página **Selecionar serviços de função** , use as configurações padrão e clique em **Avançar**.

    > [!TIP]
    > Você deve selecionar um tipo de banco de dados. Se as opções de banco de dados forem limpas (não selecionadas), haverá falha nas tarefas pós-instalação.

10. Na página **Seleção de local do conteúdo** , digite um local válido para armazenar as atualizações. Por exemplo, você pode criar uma pasta chamada WSUS_database na raiz da unidade K especificamente para essa finalidade e digitar **k:\WSUS_database** como o local válido.

11. Clique em **Avançar**. A página **Função de Servidor Web (IIS)** se abre. Analise as informações e clique em **Avançar**. Em **Selecionar os serviços de função para instalar no servidor Web (IIS)** , mantenha os padrões e, em seguida, clique em **Avançar**.

12. Na página **Confirmar seleções de instalação**, examine as opções selecionadas e clique em **Instalar**. O Assistente de instalação do WSUS é executado. Essa operação pode levar vários minutos para ser concluída.

13. Depois que a instalação do WSUS estiver concluída, na janela de resumo, na página **Progresso da instalação** clique em **Iniciar tarefas pós-instalação**. O texto é alterado, solicitando: **Aguarde enquanto o servidor é configurado**. Quando a tarefa é concluída, o texto é alterado para: **Configuração concluída com êxito**. Clique em **Fechar**.

14. No **Gerenciador do Servidor**, verifique se uma notificação aparece informando que é necessário reiniciar. Esse comportamento pode variar de acordo com a função de servidor instalada. Se for necessário reiniciar, não se esqueça de reiniciar o servidor para concluir a instalação.

> [!IMPORTANT]
> Neste momento, o processo de instalação é concluído, mas para que o WSUS fique funcional você precisa prosseguir para a [Etapa 2: Configurar o WSUS](2-configure-wsus.md).

