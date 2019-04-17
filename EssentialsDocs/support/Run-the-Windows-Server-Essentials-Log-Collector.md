---
title: Execute o coletor de Log do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6b49fee7ca4a19d5a501cf96c1ce356f8242c81f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Execute o coletor de Log do Windows Server Essentials
Você pode executar o coletor de Log do Windows Server Essentials no servidor ou um computador na rede. Se você executar o coletor de Log do servidor, você só pode coletar logs do servidor. Se você executar o coletor de Log em um computador de rede, você pode optar por coletar logs do servidor, além dos logs para aquele computador.  
  
 Você deve ter privilégios administrativos apropriados para executar o coletor de Log. Se você está coletando arquivos de log para um servidor, você deve ser um administrador de servidor; Se você está coletando arquivos de log em um computador da rede, você deve ser um administrador do cliente para aquele computador.  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Para executar o coletor de Log no servidor usando o Assistente  
  
1.  Sobre o **iniciar** página do servidor, clique em **coletor de Log do Windows Server Essentials**.  
  
    > [!NOTE]
    >  -   Se o programa de coletor de Log não aparece no **iniciar** página, navegue até **%system%\Program arquivos (x86) \Windows Server Essentials Log coletor**e clique duas vezes em **LogCollector**.  
    > -   Se você não estiver conectado ao servidor com privilégios administrativos, o coletor de Log solicita que você inserir suas credenciais.  
  
2.  Quando você for solicitado para um local para salvar os arquivos de log coletados, você pode escolher o local padrão, **\ \ \ < ServerName\ > \logs**, ou especificar outro local. Para aceitar o local padrão, clique em **próxima**. Para alterar o local, clique em **procurar**, navegue até a pasta onde deseja salvar os arquivos de log e, em seguida, clique em **salvar**.  
  
    > [!NOTE]
    >  Você não precisa fornecer nomes de arquivo para os arquivos de log. O coletor de Log nomeia a coleção de arquivo zip concatenando o nome do computador e o carimbo de data / hora do arquivo.  
  
3.  Uma barra de progresso é exibida enquanto os logs estão sendo coletados.  
  
4.  Para exibir o conteúdo do arquivo de coleção de log, selecione o **abrir o local do arquivo onde os logs foram salvas** caixa de seleção e clique em **fechar** para fechar o assistente e abra o arquivo de log da coleção.  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Para executar o coletor de Log em um computador de rede usando o Assistente  
  
1.  Navegue até **%system%\Program arquivos (x86) \Windows Server Essentials Log coletor**e clique duas vezes o arquivo **LogCollector.exe**.  
  
    > [!NOTE]
    >  Se você não estiver conectado ao computador da rede com privilégios administrativos, insira seu nome de usuário e senha quando solicitado e, em seguida, clique em **próxima**.  
  
2.  Selecione quais logs você gostaria de coletar, da seguinte maneira:  
  
    1.  Selecione o **arquivos de log do servidor** caixa de seleção para coletar arquivos de log no servidor.  
  
    2.  O **arquivos de log do computador cliente (este computador)** caixa de seleção é marcada por padrão, indicando que o coletor de Log coletará os logs do computador de rede que esteja executando o. Se você só deseja coletar logs do servidor, limpe o **arquivos de log do computador cliente (este computador)** caixa de seleção.  
  
    3.  Clique em **próxima**.  
  
3.  Quando solicitado, digite o nome de usuário e senha para um administrador de servidor e, em seguida, clique em **próxima**.  
  
4.  Digite ou navegue até o local onde deseja salvar os arquivos de log e, em seguida, clique em **próxima**.  
  
    > [!NOTE]
    >  Você não precisa fornecer nomes de arquivo para os arquivos de log. O coletor de Log nomeia a coleção de arquivo zip concatenando o nome do computador e o carimbo de data / hora do arquivo.  
  
5.  Uma barra de progresso é exibida enquanto os logs estão sendo coletados.  
  
6.  Para exibir o conteúdo do arquivo de coleção de log, selecione o **abrir o local do arquivo onde os logs foram salvas** caixa de seleção e clique em **fechar** para fechar o assistente e abra o arquivo de log da coleção.  
  
### <a name="running-the-log-collector-manually"></a>Executando o coletor de Log manualmente  
 Depois que o coletor de Log é instalado, é criada uma tarefa agendada para executar a ferramenta. Posteriormente, você pode executar o coletor de Log do **Gerenciador de tarefas agendadas** sem usar o assistente, se houver problemas com a partir do assistente.  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Para executar manualmente o coletor de Log no servidor  
  
1.  Faça logon diretamente ou remotamente o servidor.  
  
2.  Abrir o **Agendador de tarefas**.  
  
3.  Na raiz do **biblioteca do Agendador de tarefas**, navegue até a tarefa agendada denominada **LogCollector**.  
  
4.  Clique com botão direito **LogCollector**e clique em **executar**. O coletor de Log coloca os logs na pasta padrão no servidor, **\ \ \ < ServerName\ > \Logs**. Se você não tiver permissões de gravação para a pasta ou a pasta não existir, os logs são colocados no **< temp\ >** subdiretório.  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Para executar manualmente o coletor de Log em um computador da rede  
  
1.  Logon diretamente ou remotamente ao computador da rede.  
  
2.  Abrir o **Agendador de tarefas**.  
  
3.  Na raiz do **biblioteca do Agendador de tarefas**, navegue até a tarefa agendada denominada **LogCollector**.  
  
4.  Clique com botão direito **LogCollector**e clique em **executar**. O coletor de Log coloca os logs no **< temp\ >** pasta no computador de rede.
