---
title: ETAPA 7 testar a conectividade do DirectAccess da Internet
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: ed2a1616-30c6-482a-9a02-4a5023621f58
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 18cf11af84ab43e7fe0fd4ea93f10e61159048a8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963962"
---
# <a name="step-7-test-directaccess-connectivity-from-the-internet"></a>ETAPA 7 testar a conectividade do DirectAccess da Internet

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A implantação de OTP (senha de uso único) do DirectAccess foi testada da sub-rede HomeNet e agora pode ser testada pela Internet.

### <a name="to-test-otp-functionality-from-the-internet-on-client1"></a>Para testar a funcionalidade de OTP da Internet em CLIENT1

1. Em CLIENT1, verifique se você está conectado como **Usuário1**. Conecte o CLIENT1 à sub-rede corpnet.

2. Na tela **Iniciar** , digite**powershell.exe**, clique com o botão direito do mouse em **PowerShell**, clique em **avançado**e, em seguida, clique em **Executar como administrador**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.

3. Na janela do Windows PowerShell, digite **gpupdate/force** e pressione Enter.

4. Desconecte o CLIENT1 da sub-rede HomeNet, conecte-o à Internet e reinicie o computador.

5. Em CLIENT1, abra o Internet Explorer e, na barra de endereços, digite **https://app1.corp.contoso.com/** e pressione Enter. Pressione F5.

   O site não deve abrir.

6. Na tela **Iniciar** , digite**RSA**e clique em **token RSA SecurID**.

7. Aguarde até que o token RSA SecurID altere a senha de uso único e, em seguida, clique em **copiar**.

8. Clique no ícone **Conexões de rede** na área de notificação para acessar o Gerenciador de Mídia do DA.

9. Clique em **conexão do local de trabalho**e clique em **continuar**.

10. Pressione Ctrl + Alt + Delete e clique no bloco **OTP (senha de uso único)** .

11. Cole o tokencode de oito dígitos copiado anteriormente e clique em **OK**. Aguarde a conclusão da autenticação. O status da conexão do local de trabalho do DirectAccess agora será **conectado**.

12. No Internet Explorer, na barra de endereços, digite **https://app1.corp.contoso.com/** e pressione Enter. Pressione F5. Você verá o site de IIS padrão no APP1.

13. Na barra de endereços do Internet Explorer, digite **https://app2.corp.contoso.com/** e pressione Enter. Pressione F5. Você verá o site do IIS padrão em APP2.

14. Na tela **Iniciar** , digite<strong> \\ \app1\files</strong>e pressione Enter.

15. Na janela pasta compartilhada **arquivos** , clique duas vezes no arquivo **Example.txt** . Você verá o conteúdo do arquivo de Example.txt.

16. Na tela **Iniciar** , digite<strong> \\ \app2\files</strong>e pressione Enter.

17. Na janela pasta compartilhada **arquivos** , clique duas vezes no novo arquivo de **Document.txtde texto** . Você verá o conteúdo do novo arquivo de Document.txt de texto.



