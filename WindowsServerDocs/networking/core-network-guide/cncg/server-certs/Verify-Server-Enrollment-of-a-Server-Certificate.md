---
title: Verifique se o servidor de registro de um certificado de servidor
description: Este tópico faz parte do guia certificados de servidor de implantação para 802.1 X com e sem fio implantações
manager: brianlic
ms.topic: article
ms.assetid: bd80a018-5a30-47c3-89fc-aacb9f5ad298
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d8ff51fa83972e2fc73ee54628eeb89e2927046d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="verify-server-enrollment-of-a-server-certificate"></a>Verifique se o servidor de registro de um certificado de servidor

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para verificar que os servidores de servidor de política de rede (NPS) tiverem registrado um certificado de servidor da autoridade de certificação (CA).   
  
>[!NOTE]  
>A associação a **Admins. do domínio** grupo é o requisito mínimo para concluir esses procedimentos.  
  
## <a name="verify-network-policy-server-nps-enrollment-of-a-server-certificate"></a>Verifique se o registro do servidor de política de rede (NPS) de um certificado de servidor  
  
Como NPS é usado para autenticar e autorizar as solicitações de conexão de rede, é importante garantir que o certificado do servidor que você tiver emitido para servidores NPS é válido quando usado em políticas de rede.  
  
Para verificar se um certificado de servidor está configurado corretamente e inscrito no servidor NPS, você deve configurar uma política de rede de teste e permitir que o NPS verificar se o NPS podem usar o certificado de autenticação.  
  
### <a name="to-verify-nps-server-enrollment-of-a-server-certificate"></a>Para verificar o registro do servidor NPS de um certificado de servidor  
  
1.  No Gerenciador do servidor, clique em **ferramentas**e clique em **NPS**. A rede política servidor Microsoft Management Console (MMC) é aberta.  
  
2.  Clique duas vezes em **políticas**, clique com botão direito **políticas de rede**e clique em **nova**. Abre o Assistente de nova política de rede.  
  
3.  Em **especificar nome da política de rede e o tipo de Conexão**, na **nome da política**, tipo **testar política**. Certifique-se de que **tipo de servidor de acesso de rede** tem o valor **não especificado**e clique em **próxima**.  
  
4.  Em **especificar condições**, clique em **adicionar**. Em **selecione condição**, clique em **Windows grupos**e clique em **adicionar**.  
  
5.  Em **grupos**, clique em **adicionar grupos**. Em **Selecionar grupo**, tipo **usuários do domínio**, e pressione ENTER. Clique em **Okey**e clique em **próxima**.  
  
6.  Em **especificar permissão de acesso**, certifique-se de que **acesso concedido** está selecionado e clique em **próxima**.  
  
7.  Em **configurar métodos de autenticação**, clique em **adicionar**. Em **Adicionar EAP**, clique em **Microsoft: EAP protegido (PEAP)**e clique em **Okey**. Em **tipos EAP**, selecione **Microsoft: EAP protegido (PEAP)**e clique em **editar**. O **editar propriedades de EAP protegidas** caixa de diálogo é aberta.  
  
8.  No **editar propriedades de EAP protegidas** na caixa **certificado emitido para**, NPS exibe o nome do seu certificado do servidor no formato *ComputerName*. *Domínio*. Por exemplo, se o servidor NPS é denominado NPS-01 e seu domínio é example.com, o NPS exibe o certificado **NPS-01.example.com**. Além disso, em **emissor**, o nome da sua autoridade de certificação é exibido e em **data de validade**, a data de validade do certificado do servidor é mostrada. Isso demonstra que o seu servidor NPS tiver registrado um certificado de servidor válidos que podem ser usados para provar sua identidade para computadores cliente que estejam tentando acessar a rede por meio de seus servidores de acesso de rede, como servidores de rede virtual privada (VPN), 802.1 pontos de acesso sem fio compatível com X, servidores de Gateway de área de trabalho remota e 802.1 X capaz Ethernet alterna.  
  
    > [!IMPORTANT]  
    > Se o NPS não exibe um certificado de servidor válido e se ele fornece a mensagem que esses certificados não podem ser encontrados no computador local, há duas causas possíveis para esse problema. É possível que a política de grupo não seja corretamente atualizada, e o servidor NPS não tiver registrado um certificado da CA. Neste caso, reinicie o servidor NPS. Quando o computador for reiniciado, política de grupo é atualizada e você pode executá-lo novamente para confirmar que o certificado do servidor é registrado. Se atualizar a política de grupo não resolver esse problema, o modelo de certificado, o registro de certificado automático ou ambas não estão configuradas corretamente. Para resolver esses problemas, comece no início deste guia e execute todas as etapas novamente para garantir que as configurações que você forneceu são precisas.  
  
9. Quando você verificou a presença de um certificado de servidor válido, você pode clicar em **Okey** e **Cancelar** para sair do Assistente de nova política de rede.  
  
    > [!NOTE]  
    > Porque você não é concluir o assistente, a política de rede de teste não é criada no NPS.  
  


