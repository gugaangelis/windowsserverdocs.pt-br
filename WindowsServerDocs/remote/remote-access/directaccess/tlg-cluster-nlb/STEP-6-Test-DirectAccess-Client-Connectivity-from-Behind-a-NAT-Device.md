---
title: ETAPA 6 de testar a conectividade do cliente DirectAccess por trás de um dispositivo NAT
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5512f2b150af5b4e2cd5409524af8fec4be478a7
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281558"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>ETAPA 6 de testar a conectividade do cliente DirectAccess por trás de um dispositivo NAT

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Quando o cliente de DirectAccess é conectado à Internet por trás de um dispositivo NAT ou um servidor de proxy da web, o cliente do DirectAccess usa o Teredo ou o IP-HTTPS para se conectar ao servidor de Acesso Remoto. 

Se o dispositivo NAT permite que a saída da porta UDP 3544 para endereço IP público do servidor de acesso remoto, Teredo é usado. Se o acesso pelo Teredo não estiver disponível, o cliente do DirectAccess retornará para o IP-HTTPS pela porta TCP 443 de saída, que permite o acesso através de firewalls e servidores de proxy da web pela porta SSL tradicional. 

Se o proxy da web pedir autenticação, a conexão do IP-HTTPS falhará automaticamente. As conexões do IP-HTTPS também falharão se o proxy da web realizar uma inspeção SSL de saída, devido ao fato de a sessão HTTPS ser encerrada pelo proxy da web em vez do servidor de Acesso Remoto. Nesta seção, você realizará os mesmos testes executados ao conectar-se usando uma conexão 6to4 da seção anterior.  
  
Os procedimentos a seguir são executados em ambos os computadores cliente:  
  
1. Teste conectividade Teredo. O primeiro conjunto de testes são executadas quando o cliente do DirectAccess é configurado para usar o Teredo. Esta é a configuração automática usada quando o dispositivo NAT permite acesso de saída à porta UDP 3544.  
  
2. Testar a conectividade IP-HTTPS. O segundo conjunto de testes são executadas quando o cliente do DirectAccess é configurado para usar IP-HTTPS. Para demonstrar a conectividade IP-HTTPS, o Teredo é desabilitada nos computadores cliente.  
  
> [!TIP]  
> É recomendável que você limpar o cache do Internet Explorer antes de executar esses procedimentos para garantir que você está testando a conexão e não recuperar as páginas do site do cache.  
  
## <a name="prerequisites"></a>Pré-requisitos

Antes de realizar esses testes, desconecte o CLIENT1 do comutador da Internet e conecte-o ao comutador da Rede doméstica. Se for perguntado que tipo de rede você pretende definir para a rede atual, selecione **Rede Doméstica**.  
  
Inicie EDGE1 e EDGE2 se já não estiverem em execução.  
  
## <a name="test-teredo-connectivity"></a>Testar conectividade Teredo  
  
1. Em CLIENT1, abra uma janela elevada do Windows PowerShell, digite **ipconfig/all** e pressione ENTER.  
  
2. Examine o resultado do comando ipconfig.  
  
   O CLIENT1 estará então conectado à Internet por trás de um dispositivo NAT e um endereço IPv4 privado será atribuído a ele. Quando o cliente do DirectAccess estiver atrás de um dispositivo NAT e receber um endereço IPv4 privado, a tecnologia de transição IPv6 preferida será Teredo. Se você examinar a saída do comando ipconfig, você deverá ver uma seção para o adaptador de túnel Teredo Pseudo-interface de túnel e, em seguida, uma descrição de Microsoft Teredo Tunneling Adapter, com um endereço IP que começa com 2001: consistente com o que está sendo um Teredo endereço. Se você não vir a seção Teredo, ative o Teredo com o seguinte comando: **netsh interface Teredo definir estado enterpriseclient** e, em seguida, execute novamente o comando ipconfig. Você não verá um gateway padrão listado para o adaptador de túnel Teredo.  
  
3. Na janela do Windows PowerShell, digite **ipconfig /flushdns** e pressione ENTER.  
  
   Isso liberará as entradas de resolução de nome no cache do DNS do cliente que ainda poderiam existir de quando o computador estava conectado à Internet.  
  
4. Na janela do Windows PowerShell, digite **Get-DnsClientNrptPolicy** e pressione ENTER.  
  
   O resultado mostra as configurações atuais da NRPT (Tabela de Políticas de Resolução de Nomes). Essas configurações indicam que todas as conexões com .corp.contoso.com devem ser resolvidas pelo servidor DNS de Acesso Remoto com o endereço IPv6 2001:db8:1::2. Observe também a entrada de NRPT indicando uma exceção para o nome nls.corp.contoso.com. Nomes na lista de exceção não são respondidos pelo servidor DNS de Acesso Remoto. Você pode executar o ping do endereço IP do servidor DNS de acesso remoto para confirmar a conectividade com o servidor de acesso remoto; Por exemplo, você pode executar o ping 2001:db8:1::2 neste exemplo.  
  
5. Na janela do Windows PowerShell, digite **ping app1** e pressione ENTER. Você deverá ver as respostas do endereço IPv6 do APP1, 2001:db8:1::3.  
  
6. Na janela do Windows PowerShell, digite **ping app2** e pressione ENTER. Você deverá ver as respostas do endereço assinado NAT64 pelo EDGE1 para APP2, que neste caso é fdc9:9f4e:eb1b:7777::a00:4.  
  
7. Deixe a janela do Windows PowerShell aberta para o próximo procedimento.  
  
8. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione ENTER. Você verá o site de IIS padrão no APP1.  
  
9. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione ENTER. Você verá o site padrão no APP2.  
  
10. Sobre o **inicie** tela, digite<strong>\\\App2\Files</strong>, e pressione ENTER. Clique duas vezes no arquivo Novo Documento de Texto. Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso em um host somente IPv4.  
  
## <a name="test-ip-https-connectivity"></a>Testar conectividade IP-HTTPS  
  
1. Abra uma janela elevada do Windows PowerShell, digite **teredo do netsh interface definir estado desabilitado** e pressione ENTER. Isso desativa o Teredo no computador cliente e permite que o computador cliente configure a si mesmo para usar IP-HTTPS. Uma resposta **Ok** aparecerá quando o comando for concluído.  
  
2. Na janela do Windows PowerShell, digite **ipconfig/all** e pressione ENTER.  
  
3. Examine o resultado do comando ipconfig. Este computador estará então conectado à Internet por trás de um dispositivo NAT e um endereço IPv4 privado será atribuído a ele. O Teredo foi desabilitado e o cliente do DirectAccess retornará para o IP-HTTPS. Quando você examinar a saída do comando ipconfig, você verá uma seção para o adaptador de túnel iphttpsinterface com um endereço IP que é iniciado por 2001:db8:1:100 com isso que está sendo um endereço de IP-HTTPS com base no prefixo que foi configurado durante a configuração compatível com DirectAccess. Você não verá um gateway padrão listado para o adaptador de túnel IP-HTTPS.  
  
4. Na janela do Windows PowerShell, digite **ipconfig /flushdns** e pressione ENTER. Isso liberará as entradas de resolução de nome no cache do DNS do cliente que ainda poderiam existir de quando o computador estava conectado à rede corporativa.  
  
5. Na janela do Windows PowerShell, digite **ping app1** e pressione ENTER. Você deverá ver as respostas do endereço IPv6 do APP1, 2001:db8:1::3.  
  
6. Na janela do Windows PowerShell, digite **ping app2** e pressione ENTER. Você deverá ver as respostas do endereço assinado NAT64 pelo EDGE1 para APP2, que neste caso é fdc9:9f4e:eb1b:7777::a00:4.  
  
7. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione ENTER. Você verá o site de IIS padrão no APP1.  
  
8. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione ENTER. Você verá o site padrão no APP2.  
  
9. Sobre o **inicie** tela, digite<strong>\\\App2\Files</strong>, e pressione ENTER. Clique duas vezes no arquivo Novo Documento de Texto. Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso em um host somente IPv4.
