---
title: ETAPA 5 testar a conectividade do DirectAccess da Internet e por meio do Cluster
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8399bdfa-809a-45e4-9963-f9b6a631007f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3077aa54163ed9548ae3f45f8c673c731b8ef73b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446653"
---
# <a name="step-5-test-directaccess-connectivity-from-the-internet-and-through-the-cluster"></a>ETAPA 5 testar a conectividade do DirectAccess da Internet e por meio do Cluster

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

CLIENT1 agora está pronto para teste do DirectAccess.  
  
- Teste a conectividade do DirectAccess da Internet. Conecte CLIENT1 à Internet simulada. Quando conectado à Internet simulada, o cliente é atribuído a endereços IPv4 públicos. Quando um cliente DirectAccess é atribuído um endereço IPv4 público, ele tenta estabelecer uma conexão para o servidor de acesso remoto usando uma tecnologia de transição IPv6.  
  
- Teste a conectividade de cliente do DirectAccess por meio do cluster. Testar a funcionalidade do cluster. Antes de começar o teste, é recomendável que você desligue EDGE1 e EDGE2 pelo menos cinco minutos. Há vários motivos para isso, que incluem alterações relacionadas ao NLB e tempos limite de cache ARP. Ao validar a configuração de NLB em um laboratório de teste, você precisará seja paciente enquanto as alterações de configuração não serão imediatamente refletidas na conectividade de até após um período de tempo decorrido. Isso é importante ter em mente quando você executa as seguintes tarefas.  
  
    > [!TIP]  
    > É recomendável que você limpar o cache do Internet Explorer antes de executar esse procedimento, e cada vez que você teste a conexão por meio de um servidor diferente do acesso remoto para certificar-se de que você está testando a conexão e não recuperar as páginas da Web do cache.  
  
## <a name="test-directaccess-connectivity-from-the-internet"></a>Testar a conectividade do DirectAccess da Internet  
  
1. Desconecte o CLIENT1 do comutador da rede corporativa e conectá-lo ao comutador da Internet. Aguarde 30 segundos.  
  
2. Em uma janela elevada do Windows PowerShell, digite **ipconfig /flushdns** e pressione ENTER. Isso libera as entradas de resolução de nome que ainda podem existir no cache DNS do cliente de quando o computador cliente estava conectado à rede corporativa.  
  
3. Na janela do Windows PowerShell, digite **Get-DnsClientNrptPolicy** e pressione ENTER.  
  
   O resultado mostra as configurações atuais da NRPT (Tabela de Políticas de Resolução de Nomes). Essas configurações indicam que todas as conexões com. corp.contoso.com devem ser resolvidas pelo servidor DNS de acesso remoto, com o 2001:db8:1::2 de endereço IPv6. Observe também a entrada de NRPT indicando uma exceção para o nome nls.corp.contoso.com. Nomes na lista de exceção não são respondidos pelo servidor DNS de Acesso Remoto. Você pode executar o ping do endereço IP do servidor DNS de acesso remoto para confirmar a conectividade com o servidor de acesso remoto; Por exemplo, você pode executar o ping 2001:db8:1::2.  
  
4. Na janela do Windows PowerShell, digite **ping app1** e pressione ENTER. Você deve ver as respostas do endereço IPv6 para o APP1, que nesse caso é 2001:db8:1::3.  
  
5. Na janela do Windows PowerShell, digite **ping app2** e pressione ENTER. Você deverá ver as respostas do endereço assinado NAT64 pelo EDGE1 para APP2, que neste caso é fdc9:9f4e:eb1b:7777::a00:4.  
  
   A capacidade de executar o ping APP2 é importante, porque o sucesso indica que você não pode estabelecer uma conexão usando o NAT64 e o DNS64, como APP2 é um recurso somente do IPv4.  
  
6. Deixe a janela do Windows PowerShell aberta para o próximo procedimento.  
  
7. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione ENTER. Você verá o site de IIS padrão no APP1.  
  
8. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione ENTER. Você verá o site padrão no APP2.  
  
9. Sobre o **inicie** tela, digite<strong>\\\App2\Files</strong>, e pressione ENTER. Clique duas vezes no arquivo Novo Documento de Texto.  
  
    Isso demonstra que você conseguiu se conectar a um servidor de somente IPv4 usando SMB para obter um recurso no domínio de recurso.  
  
10. Sobre o **inicie** tela, digite**WF. msc**, e pressione ENTER.  
  
11. No **Firewall do Windows com segurança avançada** do console, observe que somente os **privada** ou **perfil público** está ativa. O Firewall do Windows deve ser habilitado para DirectAccess funcione corretamente. Se o Firewall do Windows está desabilitado, conectividade do DirectAccess não funcionará.  
  
12. No painel esquerdo do console, expanda o **Monitoring** nó e clique no **regras de segurança de Conexão** nó. Você deve ver as regras de segurança de conexão do Active Directory: **DirectAccess Policy-ClientToCorp**, **diretiva DirectAccess-ClientToDNS64NAT64PrefixExemption**, **diretiva DirectAccess-ClientToInfra**, e **DirectAccess Policy-ClientToNlaExempt**. Role o painel do meio para a direita para mostrar o **métodos de autenticação 1º** e **2º métodos de autenticação** colunas. Observe que a primeira regra (ClientToCorp) usa Kerberos V5 para estabelecer o túnel de intranet e a terceira regra (ClientToInfra) usa NTLMv2 para estabelecer o túnel de infraestrutura.  
  
13. No painel esquerdo do console, expanda o **associações de segurança** nó e clique no **modo principal** nó. Observe que as associações de segurança do túnel de infraestrutura usando NTLMv2 e a associação de segurança do túnel de intranet usando Kerberos V5. Clique com botão direito na entrada que mostra **usuário (Kerberos V5)** como o **método de autenticação 2º** e clique em **propriedades**. Sobre o **gerais** guia, observe o **segunda autenticação ID Local** é **CORP\User1**, indicando que o Usuário1 foi capaz de autenticar com êxito para o domínio CORP usando Kerberos.  
  
## <a name="test-directaccess-client-connectivity-through-the-cluster"></a>Testar a conectividade de cliente do DirectAccess por meio do cluster  
  
1. Execute um desligamento normal em EDGE2.  
  
   Você pode usar o Gerenciador de balanceamento de carga de rede para exibir o status dos servidores ao executar esses testes.  
  
2. Em CLIENT1, na janela do Windows PowerShell, digite **ipconfig /flushdns** e pressione ENTER. Isso libera as entradas de resolução de nome que ainda podem existir no cliente de cache do DNS.  
  
3. Na janela do Windows PowerShell, execute ping APP1 e APP2. Você deve receber respostas de ambos esses recursos.  
  
4. Sobre o **inicie** tela, digite<strong>\\\app2\files</strong>. Você deve ver a pasta compartilhada no computador APP2. A capacidade de abrir o compartilhamento de arquivos no APP2 indica que o segundo túnel, o que requer autenticação Kerberos para o usuário, está funcionando corretamente.  
  
5. Abra o Internet Explorer e, em seguida, abra os sites da Web https://app1/ e https://app2/. A capacidade de abrir os dois sites confirma que os túneis primeiros e segundo estão funcionando e funcionando. Feche o Internet Explorer.  
  
6. Inicie o computador EDGE2.  
  
7. Em EDGE1 execute um desligamento normal.  
  
8. Aguarde 5 minutos e, em seguida, retornar ao CLIENT1. Execute as etapas 2 a 5. Isso confirma que o CLIENT1 não conseguiu failover transparente para EDGE2 depois EDGE1 tornaram-se indisponível.
