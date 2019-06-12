---
title: ETAPA 6 de testar a conectividade do DirectAccess da sub-rede da rede doméstica
description: Este tópico faz parte do guia de laboratório de teste - demonstrar o DirectAccess com autenticação OTP e SecurID de RSA para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9b77cfd-8dd4-476b-a118-f3d6bf59e7b1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe3014b8b37fab35532f3ecf833188fda9e8b744
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446645"
---
# <a name="step-6-test-directaccess-connectivity-from-the-homenet-subnet"></a>ETAPA 6 de testar a conectividade do DirectAccess da sub-rede da rede doméstica

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A implantação do DirectAccess senha única (OTP) agora está concluída e você pode começar a testar a conectividade de sub-rede da rede doméstica.  
  
### <a name="to-test-otp-functionality-from-the-homenet-subnet-on-client1"></a>Para testar a funcionalidade OTP da sub-rede da rede doméstica em CLIENT1  
  
1. Em CLIENT1, certifique-se de que você efetuou logon como **User1**.  
  
2. No **inicie** tela, digite**powershell.exe**, clique com botão direito **powershell**, clique em **avançado**e, em seguida, clique em **executar como administrador**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
3. Na janela do Windows PowerShell, digite **gpupdate /force**, e pressione ENTER.  
  
4. Desconecte CLIENT1 da sub-rede Corpnet e conectá-lo à sub-rede da rede doméstica.  
  
5. Em CLIENT1, abra o Internet Explorer e, na barra de endereços, digite **https://app1.corp.contoso.com/** e pressione ENTER. Pressione F5.  
  
   Não deve abrir o site.  
  
6. Sobre o **inicie** tela, digite**RSA**e clique em **RSA SecurID Token**.  
  
7. Aguarde até que o token RSA SecurID altera a senha de uso única e, em seguida, clique em **cópia**.  
  
8. Clique no ícone **Conexões de rede** na área de notificação para acessar o Gerenciador de Mídia do DA.  
  
9. Clique em **Conexão de DirectAccess da Contoso**e clique em **continuar**.  
  
10. Pressione Control + Alt + Delete e, em seguida, clique no **senha única (OTP)** lado a lado.  
  
11. Cole o código de token do oito dígito copiados anteriormente e, em seguida, clique em **Okey**. Aguarde a autenticação seja concluída. O status de Conexão do DirectAccess no local de trabalho agora será **Connected**.  
  
12. No Internet Explorer, na barra de endereços, digite **https://app1.corp.contoso.com/** e pressione ENTER. Pressione F5. Você verá o site de IIS padrão no APP1.  
  
13. Na barra de endereços do Internet Explorer, digite **https://app2.corp.contoso.com/** e pressione ENTER. Pressione F5. Você verá o site do IIS padrão no APP2.  
  
14. Sobre o **inicie** tela, digite<strong>\\\app1\files</strong>, e pressione ENTER.  
  
15. No **arquivos** janela de pasta compartilhada, clique duas vezes o **example** arquivo. Você verá o conteúdo do arquivo example.  
  
16. Sobre o **inicie** tela, digite<strong>\\\app2\files</strong>, e pressione ENTER.  
  
17. No **arquivos** janela de pasta compartilhada, clique duas vezes o **Document. txt de texto novo** arquivo. Você verá o conteúdo do arquivo Document. txt de texto novo.  
  


