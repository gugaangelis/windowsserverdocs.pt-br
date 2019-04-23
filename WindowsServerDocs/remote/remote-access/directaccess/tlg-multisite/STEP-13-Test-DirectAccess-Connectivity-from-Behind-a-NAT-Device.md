---
title: ETAPA 13 testar a conectividade do DirectAccess por trás de um dispositivo NAT
description: 'Este tópico é parte do guia de laboratório de teste: demonstrar uma implantação de multissite de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 796825c3-5e3e-4745-a921-25ab90b95ede
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8a9324e7c5b72f60422b1263e76c7d5e14cccf4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880967"
---
# <a name="step-13-test-directaccess-connectivity-from-behind-a-nat-device"></a>ETAPA 13 testar a conectividade do DirectAccess por trás de um dispositivo NAT

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Quando o cliente de DirectAccess é conectado à Internet por trás de um dispositivo NAT ou um servidor de proxy da web, o cliente do DirectAccess usa o Teredo ou o IP-HTTPS para se conectar ao servidor de Acesso Remoto. Se o dispositivo NAT permite que a saída da porta UDP 3544 para endereço IP público do servidor de acesso remoto, Teredo é usado. Se o acesso pelo Teredo não estiver disponível, o cliente do DirectAccess retornará para o IP-HTTPS pela porta TCP 443 de saída, que permite o acesso através de firewalls e servidores de proxy da web pela porta SSL tradicional. Se o proxy da web pedir autenticação, a conexão do IP-HTTPS falhará automaticamente. As conexões do IP-HTTPS também falharão se o proxy da web realizar uma inspeção SSL de saída, devido ao fato de a sessão HTTPS ser encerrada pelo proxy da web em vez do servidor de Acesso Remoto.  
  
Os procedimentos a seguir são executados em ambos os computadores cliente:  
  
1. Teste conectividade Teredo. O primeiro conjunto de testes são executadas quando o cliente do DirectAccess é configurado para usar o Teredo. Esta é a configuração automática usada quando o dispositivo NAT permite acesso de saída à porta UDP 3544. Primeiro, execute os testes em CLIENT1 e, em seguida, execute os testes em CLIENT2.  
  
2. Testar a conectividade IP-HTTPS. O segundo conjunto de testes são executadas quando o cliente do DirectAccess é configurado para usar IP-HTTPS. Para demonstrar a conectividade IP-HTTPS, o Teredo é desabilitada nos computadores cliente. Primeiro, execute os testes em CLIENT1 e, em seguida, execute os testes em CLIENT2.  
  
## <a name="prerequisites"></a>Pré-requisitos  
Inicie EDGE1 e EDGE1 2 se ainda não estiverem executando e certifique-se de que eles estão conectados à sub-rede da Internet.  
  
Antes de executar esses testes, desconecte CLIENT1 e CLIENT2 do comutador da Internet e conecte-se ao comutador da rede doméstica. Se for perguntado que tipo de rede que você deseja definir para a rede atual, selecione **rede doméstica**.  
  
## <a name="TeredoCLIENT1"></a>Testar conectividade Teredo  
  
1.  Em CLIENT1, abra uma janela elevada do Windows PowerShell.  
  
2.  Para ativar o adaptador de Teredo, digite **teredo do netsh interface definir estado enterpriseclient**, e pressione ENTER.  
  
3.  Na janela do Windows PowerShell, digite **ipconfig/all** e pressione ENTER.  
  
4.  Examine o resultado do comando ipconfig.  
  
    Este computador estará então conectado à Internet por trás de um dispositivo NAT e um endereço IPv4 privado será atribuído a ele. Quando o cliente do DirectAccess estiver atrás de um dispositivo NAT e receber um endereço IPv4 privado, a tecnologia de transição IPv6 preferida será Teredo. Se você examinar a saída do comando ipconfig, você deverá ver uma seção para o adaptador de túnel Teredo Pseudo-interface de túnel e, em seguida, uma descrição de Microsoft Teredo Tunneling Adapter, com um endereço IP que começa com 2001: 0 consistente com o que está sendo um Teredo endereço. Você deve ver o gateway padrão listado para o adaptador de túnel Teredo como ':: '.  
  
5.  Na janela do Windows PowerShell, digite **ipconfig /flushdns** e pressione ENTER.  
  
    Isso liberará as entradas de resolução de nome no cache do DNS do cliente que ainda poderiam existir de quando o computador estava conectado à Internet.  
  
6.  Na janela do Windows PowerShell, digite **ping app1** e pressione ENTER. Você deverá ver as respostas do endereço IPv6 do APP1, 2001:db8:1::3.  
  
7.  Na janela do Windows PowerShell, digite **ping app2** e pressione ENTER. Você deverá ver as respostas do endereço assinado NAT64 atribuído pelo EDGE1 para APP2, que nesse caso é fd**c9:9f4e:eb1b**: 7777::a00:4. Observe que os valores em negrito irão variar devido a como o endereço é gerado.  
  
8.  Na janela do Windows PowerShell, digite **ping 2-app1** e pressione ENTER. Você deve ver as respostas do endereço IPv6 do APP1 de 2, 2001:db8:2::3.  
  
9. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://2-app1/** e pressione ENTER. Você verá o site do IIS padrão no APP1 2.  
  
10. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione ENTER. Você verá o site padrão no APP2.  
  
11. Sobre o **inicie** tela, digite**\\\App2\Files**, e pressione ENTER. Clique duas vezes no arquivo Novo Documento de Texto. Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso em um host somente IPv4.  
  
12. Repita esse procedimento em CLIENT2.  
  
## <a name="IPHTTPS_CLIENT1"></a>Testar conectividade IP-HTTPS  
  
1.  Em CLIENT1, abra com privilégios elevados janela Windows PowerShell e digite **teredo do netsh interface definir estado desabilitado** e pressione ENTER. Isso desativa o Teredo no computador cliente e permite que o computador cliente configure a si mesmo para usar IP-HTTPS. Uma resposta **Ok** aparecerá quando o comando for concluído.  
  
2.  Na janela do Windows PowerShell, digite **ipconfig/all** e pressione ENTER.  
  
3.  Examine o resultado do comando ipconfig. Este computador estará então conectado à Internet por trás de um dispositivo NAT e um endereço IPv4 privado será atribuído a ele. O Teredo foi desabilitado e o cliente do DirectAccess retornará para o IP-HTTPS. Quando você examinar a saída do comando ipconfig, você verá uma seção para o adaptador de túnel iphttpsinterface com um endereço IP que começa com 2001:db8:1:1000 ou 2001:db8:2:2000 consistentes assim um endereço de IP-HTTPS com base nos prefixos que estavam sendo definido ao configurar o DirectAccess. Você não verá um gateway padrão listado para o adaptador de túnel IPHTTPSInterface.  
  
4.  Na janela do Windows PowerShell, digite **ipconfig /flushdns** e pressione ENTER. Isso liberará as entradas de resolução de nome no cache do DNS do cliente que ainda poderiam existir de quando o computador estava conectado à rede corporativa.  
  
5.  Na janela do Windows PowerShell, digite **ping app1** e pressione ENTER. Você deverá ver as respostas do endereço IPv6 do APP1, 2001:db8:1::3.  
  
6.  Na janela do Windows PowerShell, digite **ping app2** e pressione ENTER. Você deverá ver as respostas do endereço assinado NAT64 atribuído pelo EDGE1 para APP2, que nesse caso é fd**c9:9f4e:eb1b**: 7777::a00:4. Observe que os valores em negrito irão variar devido a como o endereço é gerado.  
  
7.  Na janela do Windows PowerShell, digite **ping 2-app1** e pressione ENTER. Você deve ver as respostas do endereço IPv6 do APP1 de 2, 2001:db8:2::3.  
  
8.  Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://2-app1/** e pressione ENTER. Você verá o site do IIS padrão no APP1 2.  
  
9. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione ENTER. Você verá o site padrão no APP2.  
  
10. Sobre o **inicie** tela, digite**\\\App2\Files**, e pressione ENTER. Clique duas vezes no arquivo Novo Documento de Texto. Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso em um host somente IPv4.  
  
11. Repita esse procedimento em CLIENT2.  
  


