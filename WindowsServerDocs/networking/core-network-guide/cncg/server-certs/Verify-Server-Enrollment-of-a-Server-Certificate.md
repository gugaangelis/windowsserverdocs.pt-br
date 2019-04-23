---
title: Verificar registro de servidor de um certificado do servidor
description: Este tópico faz parte do guia de certificados de servidor de implantação para 802.1 X com fio e implantações sem fio
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 45ba7a9a7fc5b9622ab1b9a94f38f4bf4de13192
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850897"
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Verificar registro de servidor de um certificado do servidor

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para verificar se os seus servidores do servidor de diretivas de rede (NPS) tem um certificado do servidor da autoridade de certificação (CA) registrados.   
  
>[!NOTE]  
>Associação a **Admins. do domínio** grupo é o mínimo necessário para concluir esses procedimentos.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Verificar o registro do servidor de diretivas de rede (NPS) de um certificado de servidor  
  
Como o NPS é usado para autenticar e autorizar solicitações de conexão de rede, é importante garantir que o certificado do servidor que você tiver emitido para NPSs é válido quando usada em diretivas de rede.  
  
Para verificar se um certificado de servidor está configurado corretamente e está registrado para o NPS, você deve configurar uma política de rede de teste e permitir que o NPS verificar se o NPS pode usar o certificado para autenticação.  
  
### <a name="to-verify-nps-enrollment-of-a-server-certificate"></a>Para verificar o registro de NPS de um certificado de servidor  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **Network Policy Server**. A rede política de servidor Microsoft Management Console (MMC) é aberto.  
  
2.  Clique duas vezes em **diretivas**, clique com botão direito **políticas de rede**e clique em **New**. Abre o Assistente de nova diretiva de rede.  
  
3.  Na **especificar o nome de política de rede e o tipo de Conexão**, na **nome da política**, tipo **testar política**. Certifique-se de que **tipo de servidor de acesso de rede** tem o valor **Unspecified**e, em seguida, clique em **próxima**.  
  
4.  Na **especificar condições**, clique em **Add**. Na **Selecionar condição**, clique em **grupos do Windows**e, em seguida, clique em **adicionar**.  
  
5.  Na **grupos**, clique em **adicionar grupos**. No **Selecionar grupo**, digite **os usuários do domínio**, e pressione ENTER. Clique em **OK** e em **Avançar**.  
  
6.  Na **especificar a permissão de acesso**, certifique-se de que **acesso concedido** está selecionado e, em seguida, clique em **próxima**.  
  
7.  Na **configurar métodos de autenticação**, clique em **Add**. Na **Adicionar EAP**, clique em **Microsoft: EAP protegido (PEAP)** e, em seguida, clique em **Okey**. Na **tipos de EAP**, selecione **Microsoft: EAP protegido (PEAP)** e, em seguida, clique em **editar**. O **editar propriedades de EAP protegidas** caixa de diálogo é aberta.  
  
8.  No **editar propriedades de EAP protegidas** na caixa **certificado emitido para o**, NPS exibe o nome do seu certificado de servidor no formato *ComputerName*. *Domínio*. Por exemplo, se o NPS é chamado de NPS-01 e seu domínio for exemplo.com, o NPS exibe o certificado **NPS 01.example.com**. Além disso, no **emissor**, o nome de sua autoridade de certificação é exibido e, na **data de validade**, a data de expiração do certificado do servidor é mostrada. Isso demonstra que o NPS registrou um certificado de servidor válido que pode ser usada para provar sua identidade para computadores cliente que estão tentando acessar a rede por meio de seus servidores de acesso de rede, como servidores de rede virtual privada (VPN), 802.1 X capaz pontos de acesso sem fio, servidores de Gateway de área de trabalho remota e 802.1 comutadores Ethernet compatíveis com X.  
  
    > [!IMPORTANT]  
    > Se o NPS não exibe um certificado de servidor válido e se ele fornece a mensagem de que esse certificado não pode ser encontrado no computador local, há dois motivos possíveis para esse problema. É possível que a diretiva de grupo não atualizada corretamente, e o NPS não registrou um certificado da autoridade de certificação. Nessa circunstância, reinicie o NPS. Quando o computador for reiniciado, política de grupo é atualizada e você pode executar esse procedimento novamente para verificar que o certificado do servidor está registrado. Se a atualização de diretiva de grupo não resolver esse problema, o modelo de certificado, o registro automático de certificado ou ambos não estão configurados corretamente. Para resolver esses problemas, comece no início deste guia e executar todas as etapas novamente para garantir que as configurações que você forneceu são precisas.  
  
9. Quando você verificar a presença de um certificado de servidor válido, você pode clicar **Okey** e **Cancelar** para sair do Assistente de nova diretiva de rede.  
  
    > [!NOTE]  
    > Porque você não estiver Concluindo o assistente, a política de rede de teste não é criada no NPS.  
  


