---
title: Definir grupos de servidores RADIUS remotos
description: Este tópico fornece informações sobre como configurar grupos de servidores remotos RADIUS no servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f293fd18176115365e5e243a90a034676b3262f9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-remote-radius-server-groups"></a>Definir grupos de servidores RADIUS remotos

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para definir grupos de servidores remotos RADIUS quando você deseja configurar NPS para atuar como um servidor proxy e solicitações de conexão direta para outros servidores NPS para processamento.

## <a name="add-a-remote-radius-server-group"></a>Adicionar um grupo de servidores remotos RADIUS

Você pode usar este procedimento para adicionar um novo grupo de servidores remotos RADIUS no snap-in do servidor de política de rede (NPS).

Quando você configura o NPS como um proxy RADIUS, você pode criar uma nova política de solicitação de conexão que usa NPS para determinar qual conexão solicitações para encaminhar mensagens para outros servidores RADIUS. Além disso, a política de solicitação de conexão é configurada, especificando um RAIO grupo de servidores remotos que contém um ou mais servidores RADIUS, que informa ao NPS onde enviar as solicitações de conexão que correspondem a diretiva de solicitação de conexão.

>[!NOTE]
>Você também pode configurar um novo grupo de servidores RADIUS remoto durante o processo de criação de uma nova política de solicitação de conexão.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para concluir este procedimento.

### <a name="to-add-a-remote-radius-server-group"></a>Para adicionar um grupo de servidores remotos RADIUS 

1. No Gerenciador do servidor, clique em **ferramentas**e clique em **NPS** para abrir o console NPS.
2. Na árvore de console, clique duas vezes em **clientes e servidores RADIUS**, clique com botão direito **grupos de servidores remotos RADIUS**e clique em **nova**.
3. O **novos grupo de servidores remotos RADIUS** caixa de diálogo é aberta. Em **nome do grupo**, digite um nome para o grupo de servidores remotos RADIUS.
4. **Em servidores RADIUS**, clique em **adicionar**. O **adicionar servidores RADIUS** caixa de diálogo é aberta. Digite o endereço IP do servidor RADIUS que você deseja adicionar ao grupo, ou digite \(FQDN\) o nome de domínio totalmente qualificado do servidor RADIUS e clique em **verificar**.
5. Em **adicionar servidores RADIUS**, clique no **autenticação/contabilidade** guia. Em **segredo compartilhado** e **confirmar o segredo compartilhado**, digite o segredo compartilhado. Quando você configurar o computador local como um cliente RADIUS no servidor RADIUS remoto, você deve usar o mesmo segredo compartilhado.
6. Se você não estiver usando o protocolo EAP (Extensible Authentication) para autenticação, clique em **solicitação deve conter o atributo de autenticador de mensagem**. Por padrão, o EAP usa o atributo autenticador de mensagem.
7. Verifique se os números de porta de autenticação e estatísticas estão corretos para a implantação.
8. Se você usar um segredo compartilhado diferente para contabilidade, em **contabilidade**, desmarque o **usar o mesmo segredo compartilhado para autenticação e estatísticas** caixa de seleção e digite o segredo compartilhado de contabilização em **segredo compartilhado** e **confirmar o segredo compartilhado**.
9. Se você não quiser encaminhar inicial de servidor de acesso de rede e parar de mensagens para o servidor RADIUS remoto, limpe o **encaminhar inicial de servidor de acesso de rede e interromper notificações para esse servidor** caixa de seleção.

Para obter mais informações sobre o gerenciamento de NPS, consulte [gerenciar o servidor de políticas de rede](nps-manage-top.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).

