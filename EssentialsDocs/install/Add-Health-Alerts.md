---
title: Adicionar alertas de integridade
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cf0c062b92c687f5f7b33b419eafdca2dd3bbbfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="add-health-alerts"></a>Adicionar alertas de integridade

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Um suplemento de integridade fornece definições para alertas, verificações de integridade e reparos para problemas de rede. Um suplemento de integridade consiste em arquivos xml que fazem anotações no código ou dados que são usados para avaliar informações de integridade para um recurso específico. Suplementos de integridade são criados por desenvolvedores e instalados nos computadores servidor e cliente pelo administrador.  
  
 Consulte o [SDK do Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648) para obter detalhes sobre a criação de um suplemento de integridade.  
  
## <a name="installing-health-add-in-files"></a>Instalando arquivos de suplemento de integridade  
 Depois que o desenvolvedor criar os arquivos xml, você deve colocar uma cópia dos arquivos no local apropriado nos computadores cliente e servidor.  
  
#### <a name="to-install-the-xml-files-on-the-server"></a>Para instalar os arquivos xml no servidor  
  
1.  No **%ProgramFiles%\Windows Server\Bin\Feature Definitions** pasta, crie uma nova pasta chamada **Meusuplementodeintegridade**. Você pode dar qualquer nome essa pasta. Recomenda-se que o nome da pasta seja igual ao nome do recurso.  
  
2.  Copie o Definition.xml e Definition.xml. config arquivos para a nova pasta.  
  
3.  Se você tiver criado arquivos binários para condições ou ações, você também deve copiar esses arquivos para **%ProgramFiles%\Windows Server\Bin**.  
  
 Computadores cliente que executam uma tarefa agendada cada seis horas que transfere os arquivos XML para o local apropriado. Você pode forçar a sincronização entre o computador cliente e o servidor executando manualmente a tarefa.  
  
#### <a name="to-install-the-xml-files-on-the-client-computer"></a>Para instalar os arquivos xml no computador cliente  
  
1.  Abra o Agendador de tarefas.  
  
2.  Execute o **HealthDefintionUpdateTask** no Agendador de tarefas.  
  
    > [!NOTE]
    >  Essa tarefa não instala arquivos binários. Você deve copiar manualmente os arquivos binários para o **%ProgramFiles%\Windows Server\Bin** pasta no computador cliente.  
  
## <a name="see-also"></a>Consulte também  
 [Criar e personalizar a imagem](Creating-and-Customizing-the-Image.md)   
 [Personalizações adicionais](Additional-Customizations.md)   
 [Preparando a imagem para implantação](Preparing-the-Image-for-Deployment.md)   
 [Testando a experiência do cliente](Testing-the-Customer-Experience.md)