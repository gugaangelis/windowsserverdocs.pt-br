---
title: Configurar notificações por email
description: Este artigo descreve como configurar notificações por email
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 71a9f75d84aafcd852ae71494d133dda91e848f1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954683"
---
# <a name="configure-e-mail-notifications"></a>Configurar notificações por email

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Ao criar cotas e triagens de arquivo, você tem a opção de enviar notificações por email para os usuários quando o seu limite de cota está se aproximando ou depois que eles tentaram salvar arquivos que foram bloqueados. Quando você gera relatórios de armazenamento, você tem a opção de enviar os relatórios aos destinatários específicos por email. Se desejar notificar rotineiramente certos administradores sobre os eventos de cotas e triagem de arquivos ou enviar relatórios de armazenamento, configure um ou mais destinatários padrão.

Para enviar essas notificações e relatórios de armazenamento, é necessário especificar o servidor SMTP usado para encaminhamento de mensagens de email.

## <a name="to-configure-e-mail-options"></a>Para configurar opções de email

1. Na árvore de console, clique com o botão direito do mouse em **Gerenciador de Recursos de Servidor de Arquivos** e em **Configurar Opções**. A caixa de diálogo **Opções do Gerenciador de Recursos de Servidor de Arquivos** é aberta.

2. Na guia **Notificações por Email**, em **Nome do servidor SMTP ou endereço IP**, digite o nome do host ou o endereço IP do servidor SMTP para encaminhar as notificações por email e relatórios de armazenamento.

3. Se você desejar notificar rotineiramente certos administradores sobre os eventos de cota e triagem de arquivos ou relatórios de armazenamento de email, em **Administradores destinatários padrão**, digite cada endereço de email.

   Use o formato <em>account@domain</em> . Usar ponto-e-vírgula para separar múltiplas contas.

4. Para especificar um endereço "De" diferente de notificações por email e relatórios de armazenamento enviados do Gerenciador de Recursos de Servidor de Arquivos, em **Endereço de email padrão "De"**, digite o endereço e email que deseja exibir em sua mensagem.

5. Para testar suas configurações, clique em **Enviar email de teste**.

6. Clique em **OK**.


## <a name="additional-references"></a>Referências adicionais

-   [Definir opções do Gerenciador de Recursos de Servidor de Arquivos](setting-file-server-resource-manager-options.md)