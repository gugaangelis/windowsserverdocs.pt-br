---
title: Etapa 2 configurar o APP1
description: Este tópico faz parte do guia de laboratório de teste - demonstrar o DirectAccess com autenticação OTP e SecurID de RSA para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19a7a4a6-9a04-42ea-a5d0-ecb28a34dbaa
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 093f8215691b21d7b7fefc3b1c51f3a41af9bea6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283130"
---
# <a name="step-2-configure-app1"></a>Etapa 2 configurar o APP1

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Use as etapas a seguir para preparar o APP1 para o suporte OTP:  
  
1. Para criar e implantar um modelo de certificado usado para assinar as solicitações de certificado OTP. Configure um modelo de certificado usado para assinar as solicitações de certificado OTP.  
  
2. Para criar e implantar um modelo de certificado para certificados OTP emitidos pela autoridade de certificação corporativa. Configure um modelo de certificado para certificados OTP emitidos pela autoridade de certificação corporativa.  
  
> [!WARNING]  
> O design deste guia de laboratório de teste inclui servidores de infraestrutura, como um controlador de domínio e uma autoridade de certificação (CA) que executam o Windows Server 2012 R2 ou Windows Server 2012. Usando este guia de laboratório de teste para configurar os servidores de infraestrutura que estão executando outros sistemas operacionais não foi testado e instruções para configurar outros sistemas operacionais não estão incluídas neste guia.  
  
## <a name="DAOTPRA"></a>Para criar e implantar um modelo de certificado usado para assinar as solicitações de certificado OTP  
  
1.  Execute **certtmpl**, e pressione ENTER.  
  
2.  No Console de modelos de certificado, no painel de detalhes, clique com botão direito do **computador** modelo e clique em **Duplicar modelo**.  
  
3.  No **propriedades do novo modelo** caixa de diálogo o **compatibilidade** guia, o **autoridade de certificação** , selecione o sistema operacional que você deseja e o  **Alterações resultantes** caixa de diálogo clicar **Okey**. No **destinatário do certificado** lista Selecione o sistema operacional que você deseja, e, nas **alterações resultantes** clique da caixa de diálogo **Okey**.  
  
4.  Sobre o **propriedades do novo modelo** caixa de diálogo, clique o **geral** guia.  
  
5.  Sobre o **gerais** guia **nome de exibição do modelo**, tipo **DAOTPRA**. Defina a **período de validade** como 2 dias e defina o **período de renovação** para 1 dia. Se o **modelos de certificado** aviso é exibido, clique em **Okey**.  
  
6.  Clique na guia **Segurança** e clique em **Adicionar**.  
  
7.  Sobre o **selecionar usuários, computadores, contas de serviço ou grupos** caixa de diálogo, clique em **tipos de objeto**. Sobre o **tipos de objetos** caixa de diálogo, selecione **computadores**e, em seguida, clique em **Okey**. No **digite os nomes de objeto para selecionar** , digite **CN=edge1**, clique em **Okey**e, no **permitir** coluna, selecione o **leitura** , **Registrar**, e **registrar automaticamente** caixas de seleção. Clique em **usuários autenticados**, selecione o **leitura** caixa de seleção sob o **permitir** coluna e desmarque todas as outras caixas de seleção. Clique em **computadores do domínio**e desmarque **registrar** sob o **permitir** coluna. Clique em **Admins. do domínio** e **administradores de empresa** e clique em **controle total** sob o **permitir** coluna para ambos. Clique em **Aplicar**.  
  
8.  Clique o **nome da entidade** guia e, em seguida, clique em **compilar com as informações do Active Directory do**. No **formato de nome de assunto:** lista select **nome DNS**, certifique-se de que o **nome DNS** caixa está marcada e clique em **aplicar**.  
  
9. Clique o **extensões** guia, selecione **políticas de aplicativo** e, em seguida, clique em **editar**. Remova todas as políticas de aplicativo existente. Clique em **Add**e, na **adicionar política de aplicativo** caixa de diálogo, clique em **New**, insira **DA OTP RA** no **nome:** campo e **1.3.6.1.4.1.311.81.1.1** na **identificador de objeto:** campo e, em seguida, clique em **Okey**. Sobre o **adicionar política de aplicativo** caixa de diálogo, clique em **Okey**. Sobre o **Editar extensão de políticas de aplicativo**, clique em **Okey**. Sobre o **propriedades do novo modelo** caixa de diálogo, clique em **Okey**.  
  
## <a name="DAOTPLogon"></a>Para criar e implantar um modelo de certificado para certificados OTP emitidos pela autoridade de certificação corporativa  
  
1.  No Console de modelos de certificado, no painel de detalhes, clique com botão direito do **Logon de cartão inteligente** modelo e clique em **Duplicar modelo**.  
  
2.  No **propriedades do novo modelo** caixa de diálogo a **compatibilidade** guia o **autoridade de certificação** lista, clique no sistema operacional que você deseja usar e, na **Alterações resultantes** caixa de diálogo, clique em **Okey**. No **destinatário do certificado** lista Selecione o sistema operacional que você deseja usar, e, na **alterações resultantes** caixa de diálogo clicar **Okey**.  
  
3.  Sobre o **propriedades do novo modelo** caixa de diálogo, clique o **geral** guia.  
  
4.  Sobre o **gerais** guia **nome de exibição do modelo**, tipo **DAOTPLogon**. No **período de validade**, na lista suspensa, clique em **horas**diante a **modelos de certificado** caixa de diálogo, clique em **Okey**e certifique-se Se o número de horas é definido como 1. Na **período de renovação**, digite **0**.  
  
    > [!IMPORTANT]  
    > **Autoridade de certificação do Windows Server 2003**. Em situações em que a autoridade de certificação (CA) está em um computador que está executando o Windows Server 2003 e, em seguida, o modelo de certificado deve ser configurado em um computador diferente. Isso é necessário porque a definição de **período de validade** nas horas não é possível quando executando versões do Windows anteriores ao Windows Server 2008 e Windows Vista. Se o computador que você pode usar para configurar o modelo não tem a função de servidor de serviços de certificados do Active Directory instalada, ou se ele for um computador cliente, em seguida, você precisa instalar o snap-in de modelos de certificado. Para obter mais informações, consulte [instalar o Snap-In de modelos de certificado](https://technet.microsoft.com/library/cc732445.aspx).  
    >   
    > **Autoridade de certificação do Windows Server 2008 R2**. Se você já tiver implantado uma autoridade de certificação (CA) que está executando o Windows Server 2008 R2, você deve configurar o modelo de certificado **período de renovação** 1 ou 2 horas e o **período de validade** para ser maior do que o **período de renovação**, mas não mais de 4 horas. Se você configurar um modelo de certificado **período de validade** de mais de 4 horas, com uma autoridade de certificação que esteja executando o Windows Server 2008 R2, o Assistente de instalação do DirectAccess não pode detectar o modelo de certificado e o DirectAccess a instalação falhará.  
  
5.  Clique o **segurança** guia, selecione **Authenticated Users**, no **permitir** coluna e selecione o **leitura** e **registrar**  caixas de seleção. Clique em **OK**. Clique em **Admins. do domínio** e **administradores de empresa**e clique em **controle total** sob o **permitir** coluna para ambos. Clique em **Aplicar**.  
  
6.  Clique o **nome da entidade** guia e, em seguida, clique em **compilar com as informações do Active Directory do**. No **formato de nome de assunto:** lista select **nome totalmente diferenciado**, certifique-se de que o **nome principal do usuário (UPN)** caixa está marcada e clique em **aplicar** .  
  
7.  Clique o **Server** guia, selecione o **não armazenar certificados e solicitações no banco de dados da autoridade de certificação** caixa de seleção, desmarque o **não incluem informações de revogação em certificados emitidos** caixa de seleção e, em seguida, o **propriedades do novo modelo** caixa de diálogo, clique em **aplicar**.  
  
8.  Clique o **requisitos de emissão** guia, selecione o **esse número de assinaturas autorizadas:** caixa de seleção, defina o valor como 1. No **tipo de política exigido na assinatura:** lista select **política de aplicativo**e, nas **política de aplicativo** lista select **DA OTP RA**. Sobre o **propriedades do novo modelo** caixa de diálogo, clique em **Okey**.  
  
9. Clique o **extensões** guia e, na **políticas de aplicativo** clique em **editar**. Exclua **autenticação de cliente**, mantenha **SmartCardLogon**e clique em **Okey** duas vezes.  
  
10. Feche o Console de Modelos de Certificado.  
  
11. Sobre o **inicie** tela, digite**certsrv**, e pressione ENTER.  
  
12. Na árvore de console autoridade de certificação, expanda **corp-APP1-CA-1**, clique em **modelos de certificado**, clique com botão direito **modelos de certificado**, aponte para **New**e clique em **modelo de certificado a ser emitido**.  
  
13. Na lista de modelos de certificado, clique em **DAOTPRA** e **DAOTPLogon**e clique em **Okey**.  
  
14. No painel de detalhes do console, você deverá ver a **DAOTPRA** modelo de certificado com um **finalidade** dos **DA OTP RA** e o **DAOTPLogon** modelo de certificado com um **finalidade** de **Logon de cartão inteligente**.  
  
15. Reinicie os serviços.  
  
16. Feche a console da Autoridade de Certificação.  
  
17. Abra um prompt de comando com privilégios elevados. Tipo de **CertUtil.exe - SetReg DBFlags + DBFLAGS_ENABLEVOLATILEREQUESTS**, e pressione ENTER.  
  
18. Deixe a janela de Prompt de comando aberta para a próxima etapa.  
  


