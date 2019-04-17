---
title: Gerenciar aplicativos no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae89c46a-0afd-4858-9150-ec97650f45a4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e98d661ac71697bc0e38b6a25fe2f9d2b0b7254f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-applications-in-windows-server-essentials"></a>Gerenciar aplicativos no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 O servidor painel no Windows Server Essentials e Windows Server 2012 R2 com a função de experiência do Windows Server Essentials instalada torna possível realizar tarefas administrativas comuns. Para executar essas tarefas, consulte o seguinte:  
  
-   [Tarefas de gerenciamento de aplicativos no painel](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Instalar ou remover suplementos usando o painel](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_2)  
  
##  <a name="BKMK_1"></a>Tarefas de gerenciamento de aplicativos no painel  
 O **aplicativos** página de gerenciamento do painel fornece:  
  
-   Uma lista de suplementos instalados, que exibe:  
  
    -   O nome do serviço online ou suplementos  
  
    -   O status de atualização do add-in  
  
    -   O status da assinatura do add-in  
  
    -   O nome da empresa ou fornecedor que está disponibilizando o suplemento  
  
-   Um painel de tarefas que inclui um conjunto de tarefas de gerenciamento de um suplemento selecionado  
  
-   Uma lista de suplementos que estão disponíveis para baixar e instalar do Microsoft Pinpoint  
  
 A tabela a seguir descreve várias tarefas de gerenciamento do suplemento que estão disponíveis no painel do servidor. Algumas das tarefas são específicas de suplemento, então eles só ficam visíveis quando você seleciona um suplemento na lista.  
  
|Nome da tarefa|Descrição|  
|---------------|-----------------|  
|Remover o suplemento|Remove o suplemento selecionado do servidor e de todos os outros computadores na rede.|  
|Instalar o suplemento em computadores de rede|Ajuda você a agendar a instalação do suplemento selecionado em todos os outros computadores na rede.|  
|Obtenha ajuda com o suplemento|Abre seu navegador da Internet para um site do qual você pode procurar soluções para problemas e saiba mais sobre um suplemento selecionado.|  
|Atualizar o suplemento|Ajuda você baixar e instalar as atualizações para os suplementos que estão instalados no servidor e nos computadores da rede.|  
|Renovar a assinatura de suplemento|Abre seu navegador da Internet para um site do qual você pode renovar sua assinatura de suplemento.|  
|Leia a declaração de privacidade para o suplemento|Abre seu navegador da Internet para um site do qual você pode exibir a declaração de privacidade.|  
|Como instalar ou remover suplementos?|Abre seu navegador da Internet para uma página da web que exibe o tópico de Ajuda do assunto.|  
  
##  <a name="BKMK_2"></a>Instalar ou remover suplementos usando o painel  
 Um suplemento é um aplicativo de software que fornece recursos adicionais e a funcionalidade do seu servidor. Um número cada vez maior de suplementos é disponibilizado pela Microsoft e outros fornecedores independentes de Software (ISVs).  
  
 Antes de tirar vantagem da funcionalidade estendida que um suplemento fornece, primeiro você deve instalar o suplemento no servidor.  
  
#### <a name="to-install-an-add-in-from-microsoft-pinpoint"></a>Para instalar um suplemento da Microsoft Pinpoint  
  
1.  No painel do servidor, clique em **aplicativos**e, em seguida, clique no **Microsoft Pinpoint** guia.  Uma lista de suplementos disponíveis é exibida.  
  
2.  Clique em Adicionar do que você deseja instalar. A página de informações suplemento é exibida.  
  
3.  Na página informações suplemento, clique em baixar e siga as instruções na tela para baixar e instalar o suplemento.  
  
4.  Siga as instruções no Assistente para instalar o suplemento.  
  
5.  Quando a instalação for concluída, reinicie o painel, abra o **aplicativos** página do painel do servidor e verifique se o suplemento aparece na exibição de lista.  
  
#### <a name="to-install-an-add-in-from-another-provider"></a>Para instalar um suplemento de outro fornecedor  
  
1.  Abra o Windows Explorer e navegue até o local do arquivo de instalação de suplementos.  
  
2.  Clique duas vezes no arquivo para executar o Assistente para instalação.  
  
3.  Siga as instruções no Assistente para instalar o suplemento.  
  
4.  Quando a instalação for concluída, reinicie o painel, abra o **aplicativos** página e, em seguida, verifique se o suplemento aparece na exibição de lista.  
  
#### <a name="to-remove-an-add-in"></a>Para remover um suplemento  
  
1.  Abra o painel do servidor.  
  
2.  Clique no **aplicativos** guia.  
  
3.  Sobre o **suplementos**, selecione o suplemento que você deseja remover e, em seguida, clique em **remover o suplemento**.  
  
4.  No **remover suplemento** janela, clique em **remover**.  
  
    > [!NOTE]
    >  Talvez seja necessário reiniciar o painel para remover completamente o suplemento.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Visão geral do painel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
