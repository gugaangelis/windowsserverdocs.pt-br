---
title: ETAPA 6 teste a conectividade do DirectAccess da sub-rede HomeNet
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dbbaae146a0101decfc62d950b98c1af10fb39b2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814401"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>ETAPA 6 teste a conectividade do DirectAccess da sub-rede HomeNet

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

A implantação de OTP (senha de uso único) do DirectAccess agora está concluída e você pode começar a testar a conectividade da sub-rede HomeNet.  
  
### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>Para testar a funcionalidade de OTP da sub-rede HomeNet em CLIENT1  
  
1. Em CLIENT1, certifique-se de que você está conectado como **Usuário1**.  
  
2. Na tela **Iniciar** , digite**PowerShell. exe**, clique com o botão direito do mouse em **PowerShell**, clique em **avançado**e, em seguida, clique em **Executar como administrador**. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
3. Na janela do Windows PowerShell, digite **gpupdate/force**e pressione Enter.  
  
4. Desconecte o CLIENT1 da sub-rede corpnet e conecte-o à sub-rede HomeNet.  
  
5. Em CLIENT1, abra o Internet Explorer e, na barra de endereços, digite **https://app1.corp.contoso.com/** e pressione Enter. Pressione F5.  
  
   O site não deve abrir.  
  
6. Na tela **Iniciar** , digite**RSA**e clique em **token RSA SecurID**.  
  
7. Aguarde até que o token RSA SecurID altere a senha de uso único e, em seguida, clique em **copiar**.  
  
8. Clique no ícone **Conexões de rede** na área de notificação para acessar o Gerenciador de Mídia do DA.  
  
9. Clique em **conexão do contoso DirectAccess**e clique em **continuar**.  
  
10. Pressione Ctrl + Alt + Delete e clique no bloco **OTP (senha de uso único)** .  
  
11. Cole o tokencode de oito dígitos copiado anteriormente e clique em **OK**. Aguarde a conclusão da autenticação. O status da conexão do local de trabalho do DirectAccess agora será **conectado**.  
  
12. No Internet Explorer, na barra de endereços, digite **https://app1.corp.contoso.com/** e pressione Enter. Pressione F5. Você verá o site de IIS padrão no APP1.  
  
13. Na barra de endereços do Internet Explorer, digite **https://app2.corp.contoso.com/** e pressione Enter. Pressione F5. Você verá o site do IIS padrão em APP2.  
  
14. Na tela **Iniciar** , digite<strong>\\\APP1\FILES</strong>e pressione Enter.  
  
15. Na janela pasta compartilhada **arquivos** , clique duas vezes no arquivo **example. txt** . Você verá o conteúdo do arquivo example. txt.  
  
16. Na tela **Iniciar** , digite<strong>\\\APP2\FILES</strong>e pressione Enter.  
  
17. Na janela pasta compartilhada **arquivos** , clique duas vezes no **novo arquivo Text. txt** . Você verá o conteúdo do novo arquivo text document. txt.  
  


