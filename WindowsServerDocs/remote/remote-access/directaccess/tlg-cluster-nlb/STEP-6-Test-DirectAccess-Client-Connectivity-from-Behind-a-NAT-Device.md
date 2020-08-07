---
title: ETAPA 6 teste a conectividade do cliente do DirectAccess por trás de um dispositivo NAT
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: aded2881-99ed-4f18-868b-b765ab926597
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2ff8b054aef6755b4c98c423471c22fe45aa1866
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951131"
---
# <a name="step-6-test-directaccess-client-connectivity-from-behind-a-nat-device"></a>ETAPA 6 teste a conectividade do cliente do DirectAccess por trás de um dispositivo NAT

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Quando o cliente de DirectAccess é conectado à Internet por trás de um dispositivo NAT ou um servidor de proxy da web, o cliente do DirectAccess usa o Teredo ou o IP-HTTPS para se conectar ao servidor de Acesso Remoto.

Se o dispositivo NAT permitir a porta UDP de saída 3544 para o endereço IP público do servidor de acesso remoto, o Teredo será usado. Se o acesso pelo Teredo não estiver disponível, o cliente do DirectAccess retornará para o IP-HTTPS pela porta TCP 443 de saída, que permite o acesso através de firewalls e servidores de proxy da web pela porta SSL tradicional.

Se o proxy da web pedir autenticação, a conexão do IP-HTTPS falhará automaticamente. As conexões do IP-HTTPS também falharão se o proxy da web realizar uma inspeção SSL de saída, devido ao fato de a sessão HTTPS ser encerrada pelo proxy da web em vez do servidor de Acesso Remoto. Nesta seção, você realizará os mesmos testes executados ao conectar-se usando uma conexão 6to4 da seção anterior.

Os procedimentos a seguir são executados em ambos os computadores cliente:

1. Testar a conectividade Teredo. O primeiro conjunto de testes é executado quando o cliente do DirectAccess é configurado para usar o Teredo. Esta é a configuração automática usada quando o dispositivo NAT permite acesso de saída à porta UDP 3544.

2. Testar a conectividade IP-HTTPS. O segundo conjunto de testes é executado quando o cliente do DirectAccess é configurado para usar IP-HTTPS. Para demonstrar a conectividade IP-HTTPS, o Teredo é desabilitada nos computadores cliente.

> [!TIP]
> É recomendável que você limpe o cache do Internet Explorer antes de executar esses procedimentos para garantir que você está testando a conexão e não recuperando as páginas do site do cache.

## <a name="prerequisites"></a>Pré-requisitos

Antes de realizar esses testes, desconecte o CLIENT1 do comutador da Internet e conecte-o ao comutador da Rede doméstica. Se for perguntado que tipo de rede você pretende definir para a rede atual, selecione **Rede Doméstica**.

Inicie EDGE1 e EDGE2 se já não estiverem em execução.

## <a name="test-teredo-connectivity"></a>Testar conectividade Teredo

1. Em CLIENT1, abra uma janela elevada do Windows PowerShell, digite **ipconfig/all** e pressione Enter.

2. Examine o resultado do comando ipconfig.

   O CLIENT1 estará então conectado à Internet por trás de um dispositivo NAT e um endereço IPv4 privado será atribuído a ele. Quando o cliente do DirectAccess estiver atrás de um dispositivo NAT e receber um endereço IPv4 privado, a tecnologia de transição IPv6 preferida será Teredo. Ao observar o resultado do comando ipconfig, deverá ver uma seção indicando Pseudo-interface de criação de túneis Teredo do adaptador de túnel e a descrição Microsoft Teredo Tunneling Adapter com um endereço IP iniciando com 2001: conforme endereços Teredo. Se você não vir a seção Teredo, habilite o Teredo com o seguinte comando: **netsh interface teredo set state enterpriseclient** e execute novamente o comando ipconfig. Você não verá um gateway padrão listado para o adaptador de túnel Teredo.

3. Na janela do Windows PowerShell, digite **ipconfig/flushdns** e pressione Enter.

   Isso liberará as entradas de resolução de nome no cache do DNS do cliente que ainda poderiam existir de quando o computador estava conectado à Internet.

4. Na janela do Windows PowerShell, digite **Get-DnsClientNrptPolicy** e pressione Enter.

   O resultado mostra as configurações atuais da NRPT (Tabela de Políticas de Resolução de Nomes). Essas configurações indicam que todas as conexões com .corp.contoso.com devem ser resolvidas pelo servidor DNS de Acesso Remoto com o endereço IPv6 2001:db8:1::2. Observe também a entrada de NRPT indicando uma exceção para o nome nls.corp.contoso.com. Nomes na lista de exceção não são respondidos pelo servidor DNS de Acesso Remoto. Você pode executar o ping do endereço IP do servidor DNS de Acesso Remoto para confirmar se há conectividade com o servidor de Acesso Remot; por exemplo, você pode executar o ping 2001:db8:1::2 neste exemplo.

5. Na janela do Windows PowerShell, digite **ping App1** e pressione Enter. Você deverá ver as respostas do endereço IPv6 do APP1, 2001:db8:1::3.

6. Na janela do Windows PowerShell, digite **ping App2** e pressione Enter. Você deverá ver as respostas do endereço assinado NAT64 pelo EDGE1 para APP2, que neste caso é fdc9:9f4e:eb1b:7777::a00:4.

7. Deixe a janela do Windows PowerShell aberta para o próximo procedimento.

8. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione Enter. Você verá o site de IIS padrão no APP1.

9. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione Enter. Você verá o site padrão no APP2.

10. Na tela **Iniciar** , digite<strong> \\ \App2\Files</strong>e pressione Enter. Clique duas vezes no arquivo Novo Documento de Texto. Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso em um host somente IPv4.

## <a name="test-ip-https-connectivity"></a>Testar conectividade IP-HTTPS

1. Abra uma janela do Windows PowerShell com privilégios elevados, digite **netsh interface teredo definir estado desabilitado** e pressione Enter. Isso desativa o Teredo no computador cliente e permite que o computador cliente configure a si mesmo para usar IP-HTTPS. Uma resposta **Ok** aparecerá quando o comando for concluído.

2. Na janela do Windows PowerShell, digite **ipconfig/all** e pressione Enter.

3. Examine o resultado do comando ipconfig. Este computador estará então conectado à Internet por trás de um dispositivo NAT e um endereço IPv4 privado será atribuído a ele. O Teredo foi desabilitado e o cliente do DirectAccess retornará para o IP-HTTPS. Ao observar o resultado do comando ipconfig, você verá uma seção para o Adaptador de túnel iphttpsinterface com um endereço IP iniciado por 2001:db8:1:100 compatível com endereços IP-HTTPS com base no prefixo definido ao configurar o DirectAccess. Você não verá um gateway padrão listado para o adaptador de túnel IP-HTTPS.

4. Na janela do Windows PowerShell, digite **ipconfig/flushdns** e pressione Enter. Isso liberará as entradas de resolução de nome no cache do DNS do cliente que ainda poderiam existir de quando o computador estava conectado à rede corporativa.

5. Na janela do Windows PowerShell, digite **ping App1** e pressione Enter. Você deverá ver as respostas do endereço IPv6 do APP1, 2001:db8:1::3.

6. Na janela do Windows PowerShell, digite **ping App2** e pressione Enter. Você deverá ver as respostas do endereço assinado NAT64 pelo EDGE1 para APP2, que neste caso é fdc9:9f4e:eb1b:7777::a00:4.

7. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione Enter. Você verá o site de IIS padrão no APP1.

8. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione Enter. Você verá o site padrão no APP2.

9. Na tela **Iniciar** , digite<strong> \\ \App2\Files</strong>e pressione Enter. Clique duas vezes no arquivo Novo Documento de Texto. Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso em um host somente IPv4.
