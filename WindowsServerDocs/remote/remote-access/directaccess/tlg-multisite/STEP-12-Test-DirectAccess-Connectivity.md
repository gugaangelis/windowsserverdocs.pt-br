---
title: Etapa 12 testar conectividade do DirectAccess
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3f67ef0131d5fc765c3fe99fdff85d93e869902e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308838"
---
# <a name="step-12-test-directaccess-connectivity"></a>Etapa 12 testar conectividade do DirectAccess

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Antes de poder testar a conectividade dos computadores cliente quando eles estão localizados na Internet ou nas redes HomeNet, você deve verificar se eles têm as configurações de política de grupo corretas.  
  
- Para verificar se os clientes têm a política de grupo correta  
  
- Testar a conectividade do DirectAccess da Internet por meio do EDGE1  
  
- Mover o CLIENT2 para o grupo de segurança Win7_Clients_Site2  
  
- Testar a conectividade do DirectAccess da Internet por meio de 2-EDGE1  
  
## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}  
Conecte os dois computadores cliente à rede corpnet e reinicie os dois computadores cliente.  
  
## <a name="verify-clients-have-the-correct-group-policy"></a><a name="policy"></a>Verificar se os clientes têm a política de grupo correta  
  
1.  Em CLIENT1, clique **em Iniciar**, digite **PowerShell. exe**, clique com o botão direito do mouse em **PowerShell**, clique em **avançado**e, em seguida, clique em **Executar como administrador**. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
2.  Na janela do Windows PowerShell, digite **ipconfig** e pressione Enter.  
  
    Verifique se o endereço IPv4 do adaptador corpnet começa com 10.0.0.  
  
3.  Na janela do Windows PowerShell, digite **Get-DnsClientNrptPolicy** e pressione Enter. As entradas da Tabela de Políticas de Resolução de Nomes (NRPT) para o DirectAccess são exibidas.  
  
    -   . corp.contoso.com-essas configurações indicam que todas as conexões com corp.contoso.com devem ser resolvidas por um dos servidores DNS do DirectAccess, com o endereço IPv6 2001: DB8:1:: 2 ou 2001: DB8:2:: 20.  
  
    -   nls.corp.contoso.com-essas configurações indicam que há uma isenção para o nome nls.corp.contoso.com.  
  
4.  Deixe a janela do Windows PowerShell aberta para o próximo procedimento.  
  
5.  Em CLIENT2, clique em **Iniciar**, clique em **todos os programas**, clique em **acessórios**, clique em **Windows PowerShell**, clique com o botão direito do mouse em **Windows PowerShell**e clique em **Executar como administrador**. Se a caixa de diálogo **Controle da Conta de Usuário** for exibida, confirme que a ação exibida é aquela que você deseja e clique em **Sim**.  
  
6.  Na janela do Windows PowerShell, digite **ipconfig** e pressione Enter.  
  
    Verifique se o endereço IPv4 do adaptador corpnet começa com 10.0.0.  
  
7.  Na janela do Windows PowerShell, digite **netsh namespace show policy** e pressione Enter.  
  
    Na saída, deve haver duas seções:  
  
    -   . corp.contoso.com-essas configurações indicam que todas as conexões com corp.contoso.com devem ser resolvidas pelo servidor DNS do DirectAccess, com o endereço IPv6 2001: DB8:1:: 2.  
  
    -   nls.corp.contoso.com-essas configurações indicam que há uma isenção para o nome nls.corp.contoso.com.  
  
8.  Deixe a janela do Windows PowerShell aberta para o próximo procedimento.  
  
## <a name="test-directaccess-connectivity-from-the-internet-through-edge1"></a><a name="EDGE1"></a>Testar a conectividade do DirectAccess da Internet por meio do EDGE1  
  
1. Desconecte 2-EDGE1 da rede da Internet.  
  
2. Desconecte CLIENT1 e CLIENT2 do comutador corpnet e conecte-os ao comutador da Internet. Aguarde 30 segundos.  
  
3. Em CLIENT1, na janela do Windows PowerShell, digite **ipconfig/all** e pressione Enter.  
  
4. Examine a saída do comando ipconfig.  
  
   O computador cliente agora está conectado à Internet e tem um endereço IPv4 público. Quando o cliente DirectAccess tem um endereço IPv4 público, ele usa as tecnologias de transição Teredo ou IP-HTTPS IPv6 para encapsular as mensagens IPv6 por meio de uma Internet IPv4 entre o cliente DirectAccess e o servidor de acesso remoto. Observe que o Teredo é a tecnologia de transição preferida.  
  
5. Na janela do Windows PowerShell, digite **ipconfig/flushdns** e pressione Enter. Isso libera as entradas de resolução de nome que ainda podem existir no cache DNS do cliente do quando o computador cliente foi conectado ao corpnet.  
  
6. Desabilite a interface Teredo para garantir que o computador cliente use IP-HTTPS para se conectar ao corpnet com o seguinte comando:  
  
   ```  
   netsh interface teredo set state disable  
   ```  
  
7. Verifique se você está conectado por meio do EDGE1. Digite **netsh interface httpstunnel mostrar interfaces** e pressione Enter.  
  
   A saída deve conter a URL: https://edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > Em CLIENT1, você também pode executar o seguinte comando do Windows PowerShell: **Get-NetIPHTTPSConfiguration**. A saída mostra as conexões de URL de servidor disponíveis e o perfil ativo no momento.  
  
8. Na janela do Windows PowerShell, digite **ping App1** e pressione Enter. Você deve ver respostas do endereço IPv6 atribuído a APP1, que neste caso é 2001: DB8:1:: 3.  
  
9. Na janela do Windows PowerShell, digite **ping 2-App1** e pressione Enter. Você deve ver respostas do endereço IPv6 atribuído a 2-APP1, que neste caso é 2001: DB8:2:: 3.  
  
10. Na janela do Windows PowerShell, digite **ping App2** e pressione Enter. Você deve ver respostas do endereço NAT64 atribuído por EDGE1 a APP2, que neste caso é FD**C9:9F4E: eb1b**: 7777:: A00:4. Observe que os valores em negrito variam em virtude de como o endereço é gerado.  
  
    A capacidade de executar ping em APP2 é importante, pois o sucesso indica que você conseguiu estabelecer uma conexão usando NAT64/DNS64, pois APP2 é um recurso somente IPv4.  
  
11. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione Enter. Você verá o site de IIS padrão no APP1.  
  
12. Na barra de endereços do Internet Explorer, digite **https://2-app1/** e pressione Enter. Você verá o site da Web padrão em 2-APP1.  
  
13. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione Enter. Você verá o site padrão no APP2.  
  
14. Na tela **Iniciar** , digite<strong>\\\2-APP1\FILES</strong>e pressione Enter. Clique duas vezes no arquivo de texto de exemplo.  
  
    Isso demonstra que você foi capaz de se conectar ao servidor de arquivos no domínio corp2.corp.contoso.com quando conectado por meio de EDGE1.  
  
15. Na tela **Iniciar** , digite<strong>\\\APP2\FILES</strong>e pressione Enter. Clique duas vezes no arquivo Novo Documento de Texto.  
  
    Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso no domínio de recursos.  
  
16. Na tela **Iniciar** , digite**WF. msc**e pressione Enter.  
  
17. No console **Firewall do Windows com segurança avançada** , observe que somente o **perfil público** está ativo. O Firewall do Windows deve estar habilitado para que o DirectAccess funcione corretamente. Se o Firewall do Windows estiver desabilitado, a conectividade do DirectAccess não funcionará.  
  
18. No painel esquerdo do console, expanda o nó **monitoramento** e clique no nó **regras de segurança de conexão** . Você deve ver as regras de segurança de conexão ativas: **política do DirectAccess-ClientToCorp**, **política do DirectAccess-ClientToDNS64NAT64PrefixExemption**, **política do DirectAccess-ClientToInfra**e **política do DirectAccess-ClientToNlaExempt**. Role o painel central para a direita para mostrar as colunas **1ª autenticação** e **2 métodos de autenticação** . Observe que a primeira regra (ClientToCorp) usa o Kerberos V5 para estabelecer o túnel de intranet e a terceira regra (ClientToInfra) usa NTLMv2 para estabelecer o túnel de infraestrutura.  
  
19. No painel esquerdo do console, expanda o nó **associações de segurança** e clique no nó **modo principal** . Observe as associações de segurança de túnel de infraestrutura usando NTLMv2 e a associação de segurança de túnel de intranet usando Kerberos v5. Clique com o botão direito do mouse na entrada que mostra o **usuário (Kerberos V5)** como o **segundo método de autenticação** e clique em **Propriedades**. Na guia **geral** , observe que a **segunda ID local de autenticação** é **CORP\User1**, indicando que o Usuário1 foi capaz de se autenticar com êxito no domínio corp usando o Kerberos.  
  
20. Repita este procedimento da etapa 3 em CLIENT2.  
  
## <a name="move-client2-to-the-win7_clients_site2-security-group"></a><a name="secgroup"></a>Mover o CLIENT2 para o grupo de segurança Win7_Clients_Site2  
  
1.  No DC1, clique em **Iniciar**, digite **DSA. msc**e pressione Enter.  
  
2.  No console Active Directory usuários e computadores, abra **Corp.contoso.com/users** e clique duas vezes em **Win7_Clients_Site1**.  
  
3.  Na caixa de diálogo **Propriedades do Win7_Clients_Site1** , clique na guia **Membros** , clique em **CLIENT2**, em **remover**, em **Sim**e em **OK**.  
  
4.  Clique duas vezes em **Win7_Clients_Site2**e, na caixa de diálogo **Propriedades do Win7_Clients_Site2** , clique na guia **Membros** .  
  
5.  Clique em **Adicionar**e, na caixa de diálogo **Selecionar usuários, contatos, computadores ou contas de serviço** , clique em tipos de **objeto**, selecione **computadores**e clique em **OK**.  
  
6.  Em **Inserir os nomes de objeto a serem selecionados**, digite **CLIENT2**e clique em **OK**.  
  
7.  Reinicie o CLIENT2 e faça logon usando a conta Corp/Usuário1.  
  
8.  Em CLIENT2, abra uma janela elevada do Windows PowerShell, digite **netsh namespace show policy** e pressione Enter.  
  
    Na saída, deve haver duas seções:  
  
    -   . corp.contoso.com-essas configurações indicam que todas as conexões com corp.contoso.com devem ser resolvidas pelo servidor DNS do DirectAccess, com o endereço IPv6 2001: DB8:2:: 20.  
  
    -   nls.corp.contoso.com-essas configurações indicam que há uma isenção para o nome nls.corp.contoso.com.  
  
## <a name="test-directaccess-connectivity-from-the-internet-through-2-edge1"></a><a name="DAConnect"></a>Testar a conectividade do DirectAccess da Internet por meio de 2-EDGE1  
  
1. Conecte o 2-EDGE1 à rede da Internet.  
  
2. Desconecte o EDGE1 da rede da Internet.  
  
3. Em CLIENT1, abra uma janela elevada do Windows PowerShell.  
  
4. Na janela do Windows PowerShell, digite **ipconfig/flushdns** e pressione Enter. Isso libera as entradas de resolução de nome que ainda podem existir no cache DNS do cliente do quando o computador cliente foi conectado ao corpnet.  
  
5. Verifique se você está conectado por meio de 2-EDGE1. Digite **netsh interface httpstunnel mostrar interfaces** e pressione Enter.  
  
   A saída deve conter a URL: https://2-edge1.contoso.com:443/IPHTTPS.  
  
   > [!TIP]  
   > Em CLIENT1, você também pode executar o seguinte comando: **Get-NetIPHTTPSConfiguration**. A saída mostra as conexões de URL de servidor disponíveis e o perfil ativo no momento.  
  
   > [!NOTE]  
   > O CLIENT1 altera automaticamente o servidor por meio do qual ele se conecta aos recursos corporativos. Se a saída do comando mostrar uma conexão com o EDGE1, aguarde aproximadamente cinco minutos e tente novamente.  
  
6. Na janela do Windows PowerShell, digite **ping App1** e pressione Enter. Você deve ver respostas do endereço IPv6 atribuído a APP1, que neste caso é 2001: DB8:1:: 3.  
  
7. Na janela do Windows PowerShell, digite **ping 2-App1** e pressione Enter. Você deve ver respostas do endereço IPv6 atribuído a 2-APP1, que neste caso é 2001: DB8:2:: 3.  
  
8. Na janela do Windows PowerShell, digite **ping App2** e pressione Enter. Você deve ver respostas do endereço NAT64 atribuído por EDGE1 a APP2, que neste caso é FD**C9:9F4E: eb1b**: 7777:: A00:4. Observe que os valores em negrito variam em virtude de como o endereço é gerado.  
  
   A capacidade de executar ping em APP2 é importante, pois o sucesso indica que você conseguiu estabelecer uma conexão usando NAT64/DNS64, pois APP2 é um recurso somente IPv4.  
  
9. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione Enter. Você verá o site de IIS padrão no APP1.  
  
10. Na barra de endereços do Internet Explorer, digite **https://2-app1/** e pressione Enter. Você verá o site padrão no APP2.  
  
11. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione Enter. Você verá o site da Web padrão em APP3.  
  
12. Na tela **Iniciar** , digite<strong>\\\APP1\FILES</strong>e pressione Enter. Clique duas vezes no arquivo de texto de exemplo.  
  
    Isso demonstra que você conseguiu se conectar ao servidor de arquivos no domínio corp.contoso.com quando conectado por meio de 2 a EDGE1.  
  
13. Na tela **Iniciar** , digite<strong>\\\APP2\FILES</strong>e pressione Enter. Clique duas vezes no arquivo Novo Documento de Texto.  
  
    Isso demonstra que você conseguiu se conectar a um servidor somente IPv4 usando SMB para obter um recurso no domínio de recursos.  
  
14. Repita esse procedimento em CLIENT2 da etapa 3.  
  


