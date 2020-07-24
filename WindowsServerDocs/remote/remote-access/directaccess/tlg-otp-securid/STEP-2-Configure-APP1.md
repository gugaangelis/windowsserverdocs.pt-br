---
title: ETAPA 2 configurar o APP1
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 19a7a4a6-9a04-42ea-a5d0-ecb28a34dbaa
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 03cddb638277f04a79cf4f41d6d2df308a1cf50e
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966138"
---
# <a name="step-2-configure-app1"></a>ETAPA 2 configurar o APP1

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Use as etapas a seguir para preparar o APP1 para o suporte a OTP:  
  
1. Para criar e implantar um modelo de certificado usado para assinar solicitações de certificado OTP. Configure um modelo de certificado usado para assinar solicitações de certificado OTP.  
  
2. Para criar e implantar um modelo de certificado para certificados OTP emitidos pela AC corporativa. Configure um modelo de certificado para certificados OTP emitidos pela AC corporativa.  
  
> [!WARNING]  
> O design deste guia de laboratório de teste inclui servidores de infraestrutura, como um controlador de domínio e uma CA (autoridade de certificação) que executam o Windows Server 2012 R2 ou o Windows Server 2012. O uso deste guia de laboratório de teste para configurar servidores de infraestrutura que executam outros sistemas operacionais não foi testado e as instruções para configurar outros sistemas operacionais não estão incluídas neste guia.  
  
## <a name="to-create-and-deploy-a-certificate-template-used-to-sign-otp-certificate-requests"></a><a name="DAOTPRA"></a>Para criar e implantar um modelo de certificado usado para assinar solicitações de certificado OTP  
  
1.  Execute **certtmpl. msc**e pressione Enter.  
  
2.  No console modelos de certificado, no painel de detalhes, clique com o botão direito do mouse no modelo de **computador** e clique em **duplicar modelo**.  
  
3.  Na caixa **de diálogo Propriedades do novo modelo** , na guia **compatibilidade** , na lista **autoridade de certificação** , selecione o sistema operacional desejado e, na caixa de diálogo **alterações resultantes** , clique em **OK**. Na lista **destinatário do certificado** , selecione o sistema operacional desejado e, na caixa de diálogo **alterações resultantes** , clique em **OK**.  
  
4.  Na caixa **de diálogo Propriedades do novo modelo** , clique na guia **geral** .  
  
5.  Na guia **geral** , em **nome de exibição do modelo**, digite **DAOTPRA**. Defina o **período de validade** como 2 dias e defina o **período de renovação** como 1 dia. Se o aviso de **modelos de certificado** for exibido, clique em **OK**.  
  
6.  Clique na guia **Segurança** e clique em **Adicionar**.  
  
7.  Na caixa de diálogo **Selecionar usuários, computadores, contas de serviço ou grupos** , clique em **tipos de objeto**. Na caixa de diálogo **tipos de objeto** , selecione **computadores**e clique em **OK**. Na caixa **Inserir os nomes de objeto a serem selecionados** , digite **EDGE1**, clique em **OK**e, na coluna **permitir** , marque as caixas de seleção **ler**, **registrar**e **registrar automaticamente** . Clique em **usuários autenticados**, marque a caixa de seleção **ler** na coluna **permitir** e desmarque todas as outras caixas de seleção. Clique em **computadores de domínio**e desmarque **registrar** na coluna **permitir** . Clique em **Admins** . do domínio e em **administradores corporativos** e clique em **controle total** na coluna **permitir** para ambos. Clique em **Aplicar**.  
  
8.  Clique na guia **nome da entidade** e, em seguida, clique em **criar com base nessa Active Directory informações**. No **formato nome da entidade:** selecione **nome DNS**, verifique se a caixa **nome DNS** está marcada e clique em **aplicar**.  
  
9. Clique na guia **extensões** , selecione **políticas de aplicativo** e clique em **Editar**. Remova todas as políticas de aplicativo existentes. Clique **em Adicionar**e, na caixa de diálogo **Adicionar política de aplicativo** , clique em **novo**, insira **ra da OTP do da** no campo **nome:** e **1.3.6.1.4.1.311.81.1.1** no campo **identificador do objeto:** e clique em **OK**. Na caixa de diálogo **Adicionar política de aplicativo** , clique em **OK**. Na **extensão editar políticas de aplicativo**, clique em **OK**. Na caixa **de diálogo Propriedades do novo modelo** , clique em **OK**.  
  
## <a name="to-create-and-deploy-a-certificate-template-for-otp-certificates-issued-by-the-corporate-ca"></a><a name="DAOTPLogon"></a>Para criar e implantar um modelo de certificado para certificados OTP emitidos pela AC corporativa  
  
1.  No console modelos de certificado, no painel de detalhes, clique com o botão direito do mouse no modelo de **logon de cartão inteligente** e clique em **duplicar modelo**.  
  
2.  Na caixa **de diálogo Propriedades do novo modelo** , na guia **compatibilidade** da lista **autoridade de certificação** , clique no sistema operacional que você deseja usar e, na caixa de diálogo **alterações resultantes** , clique em **OK**. Na lista **destinatário do certificado** , selecione o sistema operacional que você deseja usar e, na caixa de diálogo **alterações resultantes** , clique em **OK**.  
  
3.  Na caixa **de diálogo Propriedades do novo modelo** , clique na guia **geral** .  
  
4.  Na guia **geral** , em **nome de exibição do modelo**, digite **DAOTPLogon**. Em **período de validade**, na lista suspensa, clique em **horas**, na caixa de diálogo **modelos de certificado** , clique em **OK**e verifique se o número de horas está definido como 1. Em **período de renovação**, digite **0**.  
  
    > [!IMPORTANT]  
    > **AC do Windows Server 2003**. Em situações em que a autoridade de certificação (CA) está em um computador que está executando o Windows Server 2003, o modelo de certificado deve ser configurado em um computador diferente. Isso é necessário porque a definição do **período de validade** em horas não é possível ao executar versões do Windows antes do windows Server 2008 e do Windows Vista. Se o computador que você usa para configurar o modelo não tiver a função de servidor de serviços de certificados Active Directory instalada, ou se for um computador cliente, talvez seja necessário instalar o snap-in de modelos de certificado. Para obter mais informações, consulte [instalar o snap-in de modelos de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732445(v=ws.11)).  
    >   
    > **AC do Windows Server 2008 R2**. Se você já tiver implantado uma autoridade de certificação (CA) que executa o Windows Server 2008 R2, deverá configurar o **período de renovação** do modelo de certificado para 1 ou 2 horas, e o **período de validade** será maior do que o **período de renovação**, mas não mais de 4 horas. Se você configurar um período de **validade** de um modelo de certificado de mais de 4 horas com uma AC que esteja executando o Windows Server 2008 R2, o assistente de instalação do DirectAccess não poderá detectar o modelo de certificado e a instalação do DirectAccess falhará.  
  
5.  Clique na guia **segurança** , selecione **usuários autenticados**, na coluna **permitir** , e marque as caixas de seleção **ler** e **registrar** . Clique em **OK**. Clique em **Admins** . do domínio e **Administração de empresa**e clique em **controle total** na coluna **permitir** para ambos. Clique em **Aplicar**.  
  
6.  Clique na guia **nome da entidade** e, em seguida, clique em **criar com base nessa Active Directory informações**. No **formato nome da entidade:** selecione o **nome totalmente diferenciado**, verifique se a caixa **nome principal do usuário (UPN)** está marcada e clique em **aplicar**.  
  
7.  Clique na guia **servidor** , marque a caixa de seleção não **armazenar certificados e solicitações no banco de dados de autoridade de certificação** , desmarque a caixa de seleção não **incluir informações de revogação em certificados emitidos** e, em seguida, na caixa **de diálogo Propriedades do novo modelo** , clique em **aplicar**.  
  
8.  Clique na guia **requisitos de emissão** , marque a caixa de seleção **este número de assinaturas autorizadas:** e defina o valor como 1. No **tipo de política necessário em assinatura:** lista Selecione **política de aplicativo**e, na lista política de **aplicativo** , selecione **da OTP ra**. Na caixa **de diálogo Propriedades do novo modelo** , clique em **OK**.  
  
9. Clique na guia **extensões** e, em **políticas de aplicativo** , clique em **Editar**. Exclua a **autenticação do cliente**, mantenha **SmartCardLogon**e clique em **OK** duas vezes.  
  
10. Feche o Console de Modelos de Certificado.  
  
11. Na tela **Iniciar** , digite**certsrv. msc**e pressione Enter.  
  
12. Na árvore de console da autoridade de certificação, expanda **Corp-App1-CA-1**, clique em **modelos de certificado**, clique com o botão direito do mouse em **modelos de certificado**, aponte para **novo**e clique em **modelo de certificado a ser emitido**.  
  
13. Na lista de modelos de certificado, clique em **DAOTPRA** e **DAOTPLogon**e clique em **OK**.  
  
14. No painel de detalhes do console, você deve ver o modelo de certificado **DAOTPRA** com uma **finalidade pretendida** de **ra da OTP da** e o modelo de certificado **DAOTPLogon** com uma **finalidade pretendida** de **logon de cartão inteligente**.  
  
15. Reinicie os serviços.  
  
16. Feche a console da Autoridade de Certificação.  
  
17. Abra um prompt de comandos com privilégios elevados. Digite **CertUtil.exe-SetReg DBFlags + DBFLAGS_ENABLEVOLATILEREQUESTS**e pressione Enter.  
  
18. Deixe a janela de prompt de comando aberta para a próxima etapa.  
  
