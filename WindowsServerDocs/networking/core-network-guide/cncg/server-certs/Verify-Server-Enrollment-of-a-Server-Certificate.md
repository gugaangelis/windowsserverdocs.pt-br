---
title: Verificar registro de servidor de um certificado do servidor
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b1a2739bf658892eaea2c14dcfe840f7fcd7eff3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949290"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Verificar registro de servidor de um certificado do servidor

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para verificar se os servidores do servidor de diretivas de rede (NPS) registraram um certificado de servidor da autoridade de certificação (CA).

>[!NOTE]
>A associação no grupo **Admins** . do domínio é o mínimo necessário para concluir esses procedimentos.

## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Verificar o registro do servidor de políticas de rede (NPS) de um certificado do servidor

Como o NPS é usado para autenticar e autorizar solicitações de conexão de rede, é importante garantir que o certificado do servidor emitido para NPSs seja válido quando usado em políticas de rede.

Para verificar se um certificado de servidor está configurado corretamente e registrado no NPS, você deve configurar uma política de rede de teste e permitir que o NPS Verifique se o NPS pode usar o certificado para autenticação.

### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>Para verificar o registro de NPS de um certificado de servidor

1.  No Gerenciador do Servidor, clique em **Ferramentas**e, em seguida, clique em **Servidor de Políticas de Rede**. O console de gerenciamento Microsoft (MMC) do servidor de diretivas de rede é aberto.

2.  Clique duas vezes em **políticas**, clique com o botão direito do mouse em **políticas de rede**e clique em **novo**. O assistente Nova Diretiva de Rede é exibido.

3.  Em **especificar nome da política de rede e tipo de conexão**, em **nome da política**, digite **política de teste**. Verifique se o **tipo de servidor de acesso à rede** tem o valor **não especificado**e clique em **Avançar**.

4.  Em **especificar condições**, clique em **Adicionar**. Em **Selecionar condição**, clique em **grupos do Windows**e, em seguida, clique em **Adicionar**.

5.  Em **grupos**, clique em **Adicionar grupos**. Em **selecionar grupo**, digite **usuários do domínio**e pressione Enter. Clique em **OK** e em **Avançar**.

6.  Em **especificar permissão de acesso**, verifique se **acesso concedido** está selecionado e clique em **Avançar**.

7.  Em **Configurar métodos de autenticação**, clique em **Adicionar**. Em **Adicionar EAP**, clique em **Microsoft: EAP protegido (PEAP)** e clique em **OK**. Em **tipos de EAP**, selecione **Microsoft: EAP protegido (PEAP)** e clique em **Editar**. A caixa de diálogo **Editar propriedades EAP protegidas** é aberta.

8.  Na caixa de diálogo **Editar propriedades EAP protegidas** , em **certificado emitido para**, o NPS exibe o nome do certificado do servidor no formato *ComputerName*. *Domínio*. Por exemplo, se o seu NPS for chamado de NPS-01 e seu domínio for example.com, o NPS exibirá o certificado **NPS-01.example.com**. Além disso, no **emissor**, o nome da autoridade de certificação é exibido e, na **data de expiração**, a data de expiração do certificado do servidor é mostrada. Isso demonstra que o NPS registrou um certificado de servidor válido que pode ser usado para provar sua identidade para os computadores cliente que estão tentando acessar a rede por meio de seus servidores de acesso à rede, como servidores de rede virtual privada (VPN), pontos de acesso sem fio compatíveis com 802.1 X, servidores de gateway Área de Trabalho Remota e comutadores Ethernet compatíveis com 802.1 X.

    > [!IMPORTANT]
    > Se o NPS não exibir um certificado de servidor válido e se ele fornecer a mensagem de que esse certificado não pode ser encontrado no computador local, haverá duas razões possíveis para esse problema. É possível que Política de Grupo não tenha sido atualizada corretamente, e o NPS não registrou um certificado da autoridade de certificação. Nessa circunstância, reinicie o NPS. Quando o computador for reiniciado, Política de Grupo será atualizado e você poderá executar esse procedimento novamente para verificar se o certificado do servidor está registrado. Se a atualização Política de Grupo não resolver esse problema, o modelo de certificado, o registro automático de certificado ou ambos não estão configurados corretamente. Para resolver esses problemas, comece no início deste guia e execute todas as etapas novamente para garantir que as configurações fornecidas sejam precisas.

9. Depois de verificar a presença de um certificado de servidor válido, você pode clicar em **OK** e em **Cancelar** para sair do assistente de nova política de rede.

    > [!NOTE]
    > Como você não está concluindo o assistente, a política de rede de teste não é criada no NPS.



