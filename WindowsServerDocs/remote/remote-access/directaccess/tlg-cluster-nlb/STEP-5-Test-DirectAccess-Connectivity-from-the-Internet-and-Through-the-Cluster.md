---
title: ETAPA 5 testar a conectividade do DirectAccess da Internet e por meio do cluster
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 92641ccf19f77becd9ed5476cd8c0178f4090f49
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310832"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>ETAPA 5 testar a conectividade do DirectAccess da Internet e por meio do cluster

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O CLIENT1 agora está pronto para teste do DirectAccess.  
  
- Testar a conectividade do DirectAccess da Internet. Conecte o CLIENT1 à Internet simulada. Quando conectado à Internet simulada, o cliente recebe endereços IPv4 públicos. Quando um cliente do DirectAccess recebe um endereço IPv4 público, ele tenta estabelecer uma conexão com o servidor de acesso remoto usando uma tecnologia de transição IPv6.  
  
- Teste a conectividade do cliente do DirectAccess por meio do cluster. Testar a funcionalidade do cluster. Antes de começar a testar, recomendamos que você desligue o EDGE1 e o EDGE2 por pelo menos cinco minutos. Há vários motivos para isso, que incluem tempos limite de cache ARP e alterações relacionadas ao NLB. Ao validar a configuração do NLB em um laboratório de teste, você precisará ser paciente, pois as alterações na configuração não serão refletidas imediatamente na conectividade até que um período de tempo tenha decorrido. Isso é importante para ter em mente quando você executa as seguintes tarefas.  
  
    > [!TIP]  
    > Recomendamos que você limpe o cache do Internet Explorer antes de executar esse procedimento e cada vez que testar a conexão por meio de um servidor de acesso remoto diferente para certificar-se de que você está testando a conexão e não recuperando as páginas da Web do cache.  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>Testar a conectividade do DirectAccess da Internet  
  
1. Desconecte o CLIENT1 do comutador corpnet e conecte-o ao comutador da Internet. Aguarde 30 segundos.  
  
2. Em uma janela do Windows PowerShell com privilégios elevados, digite **ipconfig/flushdns** e pressione Enter. Isso libera as entradas de resolução de nome que ainda podem existir no cache DNS do cliente do quando o computador cliente foi conectado ao corpnet.  
  
3. Na janela do Windows PowerShell, digite **Get-DnsClientNrptPolicy** e pressione Enter.  
  
   O resultado mostra as configurações atuais da NRPT (Tabela de Políticas de Resolução de Nomes). Essas configurações indicam que todas as conexões com. corp.contoso.com devem ser resolvidas pelo servidor DNS de acesso remoto, com o endereço IPv6 2001: DB8:1:: 2. Observe também a entrada de NRPT indicando uma exceção para o nome nls.corp.contoso.com. Nomes na lista de exceção não são respondidos pelo servidor DNS de Acesso Remoto. Você pode executar o ping no endereço IP do servidor DNS de acesso remoto para confirmar a conectividade com o servidor de acesso remoto; por exemplo, você pode executar ping em 2001: DB8:1:: 2.  
  
4. Na janela do Windows PowerShell, digite **ping App1** e pressione Enter. Você deve ver respostas do endereço IPv6 para APP1, que neste caso é 2001: DB8:1:: 3.  
  
5. Na janela do Windows PowerShell, digite **ping App2** e pressione Enter. Você deverá ver as respostas do endereço assinado NAT64 pelo EDGE1 para APP2, que neste caso é fdc9:9f4e:eb1b:7777::a00:4.  
  
   A capacidade de executar ping em APP2 é importante, pois o sucesso indica que você conseguiu estabelecer uma conexão usando NAT64/DNS64, pois APP2 é um recurso somente IPv4.  
  
6. Deixe a janela do Windows PowerShell aberta para o próximo procedimento.  
  
7. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione Enter. Você verá o site de IIS padrão no APP1.  
  
8. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione Enter. Você verá o site padrão no APP2.  
  
9. Na tela **Iniciar** , digite<strong>\\\APP2\FILES</strong>e pressione Enter. Clique duas vezes no arquivo Novo Documento de Texto.  
  
    Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso no domínio de recursos.  
  
10. Na tela **Iniciar** , digite**WF. msc**e pressione Enter.  
  
11. No console **Firewall do Windows com segurança avançada** , observe que somente o perfil **privado** ou **público** está ativo. O Firewall do Windows deve estar habilitado para que o DirectAccess funcione corretamente. Se o Firewall do Windows estiver desabilitado, a conectividade do DirectAccess não funcionará.  
  
12. No painel esquerdo do console, expanda o nó **monitoramento** e clique no nó **regras de segurança de conexão** . Você deve ver as regras de segurança de conexão ativas: **política do DirectAccess-ClientToCorp**, **política do DirectAccess-ClientToDNS64NAT64PrefixExemption**, **política do DirectAccess-ClientToInfra**e **política do DirectAccess-ClientToNlaExempt**. Role o painel central para a direita para mostrar as colunas **1ª autenticação** e **2 métodos de autenticação** . Observe que a primeira regra (ClientToCorp) usa o Kerberos V5 para estabelecer o túnel de intranet e a terceira regra (ClientToInfra) usa NTLMv2 para estabelecer o túnel de infraestrutura.  
  
13. No painel esquerdo do console, expanda o nó **associações de segurança** e clique no nó **modo principal** . Observe as associações de segurança de túnel de infraestrutura usando NTLMv2 e a associação de segurança de túnel de intranet usando Kerberos v5. Clique com o botão direito do mouse na entrada que mostra o **usuário (Kerberos V5)** como o **segundo método de autenticação** e clique em **Propriedades**. Na guia **geral** , observe que a **segunda ID local de autenticação** é **CORP\User1**, indicando que o Usuário1 foi capaz de se autenticar com êxito no domínio corp usando o Kerberos.  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>Testar a conectividade do cliente do DirectAccess por meio do cluster  
  
1. Execute um desligamento normal em EDGE2.  
  
   Você pode usar o Gerenciador de balanceamento de carga de rede para exibir o status dos servidores ao executar esses testes.  
  
2. Em CLIENT1, na janela do Windows PowerShell, digite **ipconfig/flushdns** e pressione Enter. Isso libera as entradas de resolução de nome que ainda podem existir no cache DNS do cliente.  
  
3. Na janela do Windows PowerShell, execute ping em APP1 e APP2. Você deve receber respostas de ambos os recursos.  
  
4. Na tela **Iniciar** , digite<strong>\\\app2\files</strong>. Você deve ver a pasta compartilhada no computador APP2. A capacidade de abrir o compartilhamento de arquivos em APP2 indica que o segundo túnel, que requer a autenticação Kerberos para o usuário, está funcionando corretamente.  
  
5. Abra o Internet Explorer e, em seguida, abra os sites https://app1/ e https://app2/. A capacidade de abrir ambos os sites confirma que o primeiro e o segundo túneis estão ativos e funcionando. Feche o Internet Explorer.  
  
6. Inicie o computador EDGE2.  
  
7. Em EDGE1, execute um desligamento normal.  
  
8. Aguarde 5 minutos e, em seguida, retorne para CLIENT1. Execute as etapas 2-5. Isso confirma que o CLIENT1 conseguiu fazer failover de forma transparente para EDGE2 depois que o EDGE1 ficou indisponível.
