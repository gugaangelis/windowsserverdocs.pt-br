---
title: Adicionar Alertas de Integridade
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 270e0aac-dc42-46f3-a20b-a68ffbded06d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 3a3c04a48e06ef0040943b866eaf1ddb0be8fa89
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624077"
---
# <a name="add-health-alerts"></a>Adicionar Alertas de Integridade

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Um suplemento de integridade fornece definições para alertas, verificações de integridade e reparos de problemas na rede. Um suplemento de integridade consiste em arquivos xml que anotam códigos ou dados usados para avaliar informações de integridade de um recurso específico. Os suplementos de integridade são criados por desenvolvedores e instalados no servidor e nos computadores clientes pelo administrador.

 Consulte o [SDK do Windows Server Solutions](https://go.microsoft.com/fwlink/?LinkID=248648) para obter detalhes sobre a criação de um suplemento de integridade.

## <a name="installing-health-add-in-files"></a>Instalando arquivos de suplemento de integridade
 É necessário colocar uma cópia dos arquivos xml criados pelo desenvolvedor no local apropriado no servidor e nos computadores clientes.

#### <a name="to-install-the-xml-files-on-the-server"></a>Para instalar os arquivos xml no servidor

1. Na pasta **%ProgramFiles%\Windows Server\Bin\Feature Definitions** , crie uma nova pasta nomeada **MyHealthAddIn**. Você pode dar a essa pasta qualquer nome. Sugere-se que o nome da pasta seja o mesmo do recurso.

2. Copie os arquivos Definition.xml e Definition.xml.config para a nova pasta.

3. Se você criou arquivos binários para condições ou ações, também deverá copiá-los para **%ProgramFiles%\Windows Server\Bin**.

   Os computadores clientes executam uma tarefa agendada a cada 6 horas que coloca os arquivos XML no local apropriado. É possível forçar a sincronização entre o computador cliente e o servidor executando manualmente a tarefa.

#### <a name="to-install-the-xml-files-on-the-client-computer"></a>Para instalar os arquivos xml no computador cliente

1.  Abra o Agendador de Tarefas.

2.  Execute o **HealthDefintionUpdateTask** no Agendador de Tarefas.

    > [!NOTE]
    >  Essa tarefa não instala arquivos binários. É preciso copiar os arquivos binários manualmente para a pasta **%ProgramFiles%\Windows Server\Bin** no computador cliente.

## <a name="see-also"></a>Consulte Também
 [Criando e personalizando a imagem](Creating-and-Customizing-the-Image.md) [personalizações adicionais](Additional-Customizations.md) [preparando a imagem para](Preparing-the-Image-for-Deployment.md) [testar a implantação da experiência do cliente](Testing-the-Customer-Experience.md)