---
title: ETAPA 13 testar a conectividade do DirectAccess por trás de um dispositivo NAT
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a4c944a61c44b9b67831bfd4e2852941e577e6b5
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308788"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>ETAPA 13 testar a conectividade do DirectAccess por trás de um dispositivo NAT

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Quando o cliente de DirectAccess é conectado à Internet por trás de um dispositivo NAT ou um servidor de proxy da web, o cliente do DirectAccess usa o Teredo ou o IP-HTTPS para se conectar ao servidor de Acesso Remoto. Se o dispositivo NAT permitir a porta UDP de saída 3544 para o endereço IP público do servidor de acesso remoto, o Teredo será usado. Se o acesso pelo Teredo não estiver disponível, o cliente do DirectAccess retornará para o IP-HTTPS pela porta TCP 443 de saída, que permite o acesso através de firewalls e servidores de proxy da web pela porta SSL tradicional. Se o proxy da web pedir autenticação, a conexão do IP-HTTPS falhará automaticamente. As conexões do IP-HTTPS também falharão se o proxy da web realizar uma inspeção SSL de saída, devido ao fato de a sessão HTTPS ser encerrada pelo proxy da web em vez do servidor de Acesso Remoto.  
  
Os procedimentos a seguir são executados em ambos os computadores cliente:  
  
1. Testar a conectividade Teredo. O primeiro conjunto de testes é executado quando o cliente do DirectAccess é configurado para usar o Teredo. Esta é a configuração automática usada quando o dispositivo NAT permite acesso de saída à porta UDP 3544. Primeiro, execute os testes em CLIENT1 e, em seguida, execute os testes em CLIENT2.  
  
2. Testar a conectividade IP-HTTPS. O segundo conjunto de testes é executado quando o cliente do DirectAccess é configurado para usar IP-HTTPS. Para demonstrar a conectividade IP-HTTPS, o Teredo é desabilitada nos computadores cliente. Primeiro, execute os testes em CLIENT1 e, em seguida, execute os testes em CLIENT2.  
  
## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}  
Inicie o EDGE1 e 2-EDGE1 se eles ainda não estiverem em execução e verifique se eles estão conectados à sub-rede da Internet.  
  
Antes de executar esses testes, desconecte CLIENT1 e CLIENT2 do comutador da Internet e conecte-os à opção HomeNet Se for solicitado que tipo de rede você deseja definir a rede atual, selecione **rede doméstica**.  
  
## <a name="test-teredo-connectivity"></a><a name="TeredoCLIENT1"></a>Testar a conectividade de Teredo  
  
1. Em CLIENT1, abra uma janela elevada do Windows PowerShell.  
  
2. Habilite o adaptador Teredo, digite **netsh interface teredo set state enterpriseclient**e pressione Enter.  
  
3. Na janela do Windows PowerShell, digite **ipconfig/all** e pressione Enter.  
  
4. Examine o resultado do comando ipconfig.  
  
   Este computador estará então conectado à Internet por trás de um dispositivo NAT e um endereço IPv4 privado será atribuído a ele. Quando o cliente do DirectAccess estiver atrás de um dispositivo NAT e receber um endereço IPv4 privado, a tecnologia de transição IPv6 preferida será Teredo. Se você examinar a saída do comando ipconfig, verá uma seção para a pseudo interface de túnel Teredo do adaptador de túnel e, em seguida, uma descrição do adaptador de túnel do Microsoft Teredo, com um endereço IP que começa com 2001:0 consistente com ser um Teredo corrigir. Você deverá ver o gateway padrão listado para o adaptador de túnel Teredo como ':: '.  
  
5. Na janela do Windows PowerShell, digite **ipconfig/flushdns** e pressione Enter.  
  
   Isso liberará as entradas de resolução de nome no cache do DNS do cliente que ainda poderiam existir de quando o computador estava conectado à Internet.  
  
6. Na janela do Windows PowerShell, digite **ping App1** e pressione Enter. Você deverá ver as respostas do endereço IPv6 do APP1, 2001:db8:1::3.  
  
7. Na janela do Windows PowerShell, digite **ping App2** e pressione Enter. Você deve ver respostas do endereço NAT64 atribuído por EDGE1 a APP2, que neste caso é FD**C9:9F4E: eb1b**: 7777:: A00:4. Observe que os valores em negrito variam em virtude de como o endereço é gerado.  
  
8. Na janela do Windows PowerShell, digite **ping 2-App1** e pressione Enter. Você deve ver respostas do endereço IPv6 de 2-APP1, 2001: DB8:2:: 3.  
  
9. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://2-app1/** e pressione Enter. Você verá o site do IIS padrão em 2-APP1.  
  
10. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione Enter. Você verá o site padrão no APP2.  
  
11. Na tela **Iniciar** , digite<strong>\\\APP2\FILES</strong>e pressione Enter. Clique duas vezes no arquivo Novo Documento de Texto. Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso em um host somente IPv4.  
  
12. Repita este procedimento em CLIENT2.  
  
## <a name="test-ip-https-connectivity"></a><a name="IPHTTPS_CLIENT1"></a>Testar conectividade IP-HTTPS  
  
1. Em CLIENT1, abra uma janela do Windows PowerShell com privilégios elevados e digite **netsh interface teredo definir estado desabilitado** e pressione Enter. Isso desativa o Teredo no computador cliente e permite que o computador cliente configure a si mesmo para usar IP-HTTPS. Uma resposta **Ok** aparecerá quando o comando for concluído.  
  
2. Na janela do Windows PowerShell, digite **ipconfig/all** e pressione Enter.  
  
3. Examine o resultado do comando ipconfig. Este computador estará então conectado à Internet por trás de um dispositivo NAT e um endereço IPv4 privado será atribuído a ele. O Teredo foi desabilitado e o cliente do DirectAccess retornará para o IP-HTTPS. Ao examinar a saída do comando ipconfig, você verá uma seção para o adaptador de encapsulamento iphttpsinterface com um endereço IP que começa com 2001: DB8:1: 1000 ou 2001: DB8:2: 2000 consistente com esse ser um endereço IP-HTTPS com base nos prefixos que foram configurado ao configurar o DirectAccess. Você não verá um gateway padrão listado para o adaptador de túnel IPHTTPSInterface.  
  
4. Na janela do Windows PowerShell, digite **ipconfig/flushdns** e pressione Enter. Isso liberará as entradas de resolução de nome no cache do DNS do cliente que ainda poderiam existir de quando o computador estava conectado à rede corporativa.  
  
5. Na janela do Windows PowerShell, digite **ping App1** e pressione Enter. Você deverá ver as respostas do endereço IPv6 do APP1, 2001:db8:1::3.  
  
6. Na janela do Windows PowerShell, digite **ping App2** e pressione Enter. Você deve ver respostas do endereço NAT64 atribuído por EDGE1 a APP2, que neste caso é FD**C9:9F4E: eb1b**: 7777:: A00:4. Observe que os valores em negrito variam em virtude de como o endereço é gerado.  
  
7. Na janela do Windows PowerShell, digite **ping 2-App1** e pressione Enter. Você deve ver respostas do endereço IPv6 de 2-APP1, 2001: DB8:2:: 3.  
  
8. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://2-app1/** e pressione Enter. Você verá o site do IIS padrão em 2-APP1.  
  
9. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione Enter. Você verá o site padrão no APP2.  
  
10. Na tela **Iniciar** , digite<strong>\\\APP2\FILES</strong>e pressione Enter. Clique duas vezes no arquivo Novo Documento de Texto. Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso em um host somente IPv4.  
  
11. Repita este procedimento em CLIENT2.  
  


