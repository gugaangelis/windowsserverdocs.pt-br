---
title: ETAPA 12 testar a conectividade de DirectAccess
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
ms.assetid: 65ac1c23-3a47-4e58-888d-9dde7fba1586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 767d9e0760f29023aaf049ad108792f17dac309f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856957"
---
# <a name="step-12-test-directaccess-connectivity"></a>ETAPA 12 testar a conectividade de DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Antes de você pode testar a conectividade dos computadores clientes quando eles estão localizados em redes de Internet ou da rede doméstica, certifique-se de que eles têm as configurações de política de grupo correto.  
  
- Para verificar se os clientes têm a política de grupo correta  
  
- Testar a conectividade do DirectAccess da Internet por meio de EDGE1  
  
- Mover CLIENT2 para o grupo de segurança Win7_Clients_Site2  
  
- Testar a conectividade do DirectAccess da Internet por meio de EDGE1 2  
  
## <a name="prerequisites"></a>Pré-requisitos  
Conectar ambos os computadores cliente à rede da rede corporativa e, em seguida, reinicie ambos os computadores cliente.  
  
## <a name="policy"></a>Verifique se os clientes têm a política de grupo correta  
  
1.  Em CLIENT1, clique em **iniciar**, digite **powershell.exe**, clique com botão direito **powershell**, clique em **avançado**e, em seguida, clique em **Executar como administrador**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
2.  Na janela do Windows PowerShell, digite **ipconfig** e pressione ENTER.  
  
    Certifique-se de que o endereço IPv4 de adaptador de rede corporativa começa com 10.0.0.  
  
3.  Na janela do Windows PowerShell, digite **Get-DnsClientNrptPolicy** e pressione ENTER. As entradas da Tabela de Políticas de Resolução de Nomes (NRPT) para o DirectAccess são exibidas.  
  
    -   . corp.contoso.com-essas configurações indicam que todas as conexões para corp.contoso.com devem ser resolvidas por um dos servidores DNS do DirectAccess, com o IPv6 2001:db8:1::2 de endereço ou 2001:db8:2::20.  
  
    -   NLS.corp.contoso.com essas configurações indicam que há uma isenção para o nome nls.corp.contoso.com.  
  
4.  Deixe a janela do Windows PowerShell aberta para o próximo procedimento.  
  
5.  No Cliente2, clique em **iniciar**, clique em **todos os programas**, clique em **Acessórios**, clique em **Windows PowerShell**, clique com botão direito **Windows PowerShell**e, em seguida, clique em **executar como administrador**. Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, confirme se a ação exibida é a que você deseja e, em seguida, clique em **Sim**.  
  
6.  Na janela do Windows PowerShell, digite **ipconfig** e pressione ENTER.  
  
    Certifique-se de que o endereço IPv4 de adaptador de rede corporativa começa com 10.0.0.  
  
7.  Na janela do Windows PowerShell, digite **netsh namespace show política** e pressione ENTER.  
  
    Na saída, deve haver duas seções:  
  
    -   . corp.contoso.com-essas configurações indicam que todas as conexões para corp.contoso.com devem ser resolvidas pelo servidor DNS do DirectAccess, com o 2001:db8:1::2 de endereço IPv6.  
  
    -   NLS.corp.contoso.com essas configurações indicam que há uma isenção para o nome nls.corp.contoso.com.  
  
8.  Deixe a janela do Windows PowerShell aberta para o próximo procedimento.  
  
## <a name="EDGE1"></a>Testar a conectividade do DirectAccess da Internet por meio de EDGE1  
  
1.  Desconecte 2-EDGE1 da rede da Internet.  
  
2.  Desconecte CLIENT1 e CLIENT2 do comutador da rede corporativa e conectá-los ao comutador da Internet. Aguarde 30 segundos.  
  
3.  Em CLIENT1, na janela do Windows PowerShell, digite **ipconfig/all** e pressione ENTER.  
  
4.  Examine a saída do comando ipconfig.  
  
    O computador cliente agora está conectado à Internet e tem um endereço IPv4 público. Quando o cliente DirectAccess tem um endereço IPv4 público, ele usa as tecnologias de transição Teredo ou IP-HTTPS IPv6 para as mensagens de IPv6 de túnel em uma Internet IPv4 entre o cliente DirectAccess e o servidor de acesso remoto. Observe que o Teredo é a tecnologia de transição preferencial.  
  
5.  Na janela do Windows PowerShell, digite **ipconfig /flushdns** e pressione ENTER. Isso libera as entradas de resolução de nome que ainda podem existir no cache DNS do cliente de quando o computador cliente estava conectado à rede corporativa.  
  
6.  Desabilite a interface Teredo para garantir que o computador cliente usa o IP-HTTPS para se conectar à rede corporativa com o seguinte comando:  
  
    ```  
    netsh interface teredo set state disable  
    ```  
  
7.  Certifique-se de que você está conectado por meio de EDGE1. Tipo de **netsh interface httpstunnel show interfaces** e pressione ENTER.  
  
    A saída deve conter a URL: https://edge1.contoso.com:443/IPHTTPS.  
  
    > [!TIP]  
    > Em CLIENT1, você também pode executar o seguinte comando do Windows PowerShell: **Get-NetIPHTTPSConfiguration**. A saída mostra as conexões de URL do servidor disponíveis e o perfil ativo no momento.  
  
8.  Na janela do Windows PowerShell, digite **ping app1** e pressione ENTER. Você deve ver as respostas do endereço IPv6 atribuído ao APP1, que nesse caso é 2001:db8:1::3.  
  
9. Na janela do Windows PowerShell, digite **ping 2-app1** e pressione ENTER. Você deve ver as respostas do endereço IPv6 atribuído a 2-APP1, que nesse caso é 2001:db8:2::3.  
  
10. Na janela do Windows PowerShell, digite **ping app2** e pressione ENTER. Você deverá ver as respostas do endereço assinado NAT64 atribuído pelo EDGE1 para APP2, que nesse caso é fd**c9:9f4e:eb1b**: 7777::a00:4. Observe que os valores em negrito irão variar devido a como o endereço é gerado.  
  
    A capacidade de executar o ping APP2 é importante, porque o sucesso indica que você não pode estabelecer uma conexão usando o NAT64 e o DNS64, como APP2 é um recurso somente do IPv4.  
  
11. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione ENTER. Você verá o site de IIS padrão no APP1.  
  
12. Na barra de endereços do Internet Explorer, digite **https://2-app1/** e pressione ENTER. Você verá o site padrão no APP1 2.  
  
13. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione ENTER. Você verá o site padrão no APP2.  
  
14. Sobre o **inicie** tela, digite**\\\2-App1\Files**, e pressione ENTER. Clique duas vezes no arquivo de texto de exemplo.  
  
    Isso demonstra que você conseguiu se conectar ao servidor de arquivos no domínio corp2.corp.contoso.com quando conectados por meio de EDGE1.  
  
15. Sobre o **inicie** tela, digite**\\\App2\Files**, e pressione ENTER. Clique duas vezes no arquivo Novo Documento de Texto.  
  
    Isso demonstra que você conseguiu se conectar a um servidor de somente IPv4 usando SMB para obter um recurso no domínio de recurso.  
  
16. Sobre o **inicie** tela, digite**WF. msc**, e pressione ENTER.  
  
17. No **Firewall do Windows com segurança avançada** do console, observe que somente os **perfil público** está ativa. O Firewall do Windows deve ser habilitado para DirectAccess funcione corretamente. Se o Firewall do Windows está desabilitado, conectividade do DirectAccess não funcionará.  
  
18. No painel esquerdo do console, expanda o **Monitoring** nó e clique no **regras de segurança de Conexão** nó. Você deve ver as regras de segurança de conexão do Active Directory: **DirectAccess Policy-ClientToCorp**, **diretiva DirectAccess-ClientToDNS64NAT64PrefixExemption**, **diretiva DirectAccess-ClientToInfra**, e **DirectAccess Policy-ClientToNlaExempt**. Role o painel do meio para a direita para mostrar o **métodos de autenticação 1º** e **2º métodos de autenticação** colunas. Observe que a primeira regra (ClientToCorp) usa Kerberos V5 para estabelecer o túnel de intranet e a terceira regra (ClientToInfra) usa NTLMv2 para estabelecer o túnel de infraestrutura.  
  
19. No painel esquerdo do console, expanda o **associações de segurança** nó e clique no **modo principal** nó. Observe que as associações de segurança do túnel de infraestrutura usando NTLMv2 e a associação de segurança do túnel de intranet usando Kerberos V5. Clique com botão direito na entrada que mostra **usuário (Kerberos V5)** como o **método de autenticação 2º** e clique em **propriedades**. Sobre o **gerais** guia, observe o **segunda autenticação ID Local** é **CORP\User1**, indicando que o Usuário1 foi capaz de autenticar com êxito para o domínio CORP usando Kerberos.  
  
20. Repita este procedimento na etapa 3 em CLIENT2.  
  
## <a name="secgroup"></a>Mover CLIENT2 para o grupo de segurança Win7_Clients_Site2  
  
1.  No DC1, clique em **inicie**, digite **DSA. msc**, e pressione ENTER.  
  
2.  No console do Active Directory Users and Computers, abra **corp.contoso.com/Users** e clique duas vezes em **Win7_Clients_Site1**.  
  
3.  Sobre o **Win7_Clients_Site1 propriedades** caixa de diálogo, clique no **membros** , clique em **CLIENT2**, clique em **remover**, clique em **Yes**e, em seguida, clique em **Okey**.  
  
4.  Clique duas vezes em **Win7_Clients_Site2**e, em seguida, no **propriedades Win7_Clients_Site2** caixa de diálogo, clique o **membros** guia.  
  
5.  Clique em **Add**e, na **selecionar usuários, contatos, computadores ou contas de serviço** caixa de diálogo, clique em **tipos de objeto**, selecione **computadores**e, em seguida, clique em **Okey**.  
  
6.  Na **digite os nomes de objeto para selecionar**, digite **CLIENT2**e, em seguida, clique em **Okey**.  
  
7.  Reinicie o CLIENT2 e faça logon usando a conta User1/corp.  
  
8.  No Cliente2, abra uma janela elevada do Windows PowerShell, digite **netsh namespace show política** e pressione ENTER.  
  
    Na saída, deve haver duas seções:  
  
    -   . corp.contoso.com-essas configurações indicam que todas as conexões para corp.contoso.com devem ser resolvidas pelo servidor DNS do DirectAccess, com o 2001:db8:2::20 de endereço IPv6.  
  
    -   NLS.corp.contoso.com essas configurações indicam que há uma isenção para o nome nls.corp.contoso.com.  
  
## <a name="DAConnect"></a>Testar a conectividade do DirectAccess da Internet por meio de EDGE1 2  
  
1.  Conecte-se 2 EDGE1 à rede de Internet.  
  
2.  Desconecte EDGE1 da rede da Internet.  
  
3.  Em CLIENT1, abra uma janela elevada do Windows PowerShell.  
  
4.  Na janela do Windows PowerShell, digite **ipconfig /flushdns** e pressione ENTER. Isso libera as entradas de resolução de nome que ainda podem existir no cache DNS do cliente de quando o computador cliente estava conectado à rede corporativa.  
  
5.  Certifique-se de que você está conectado por meio de EDGE1 2. Tipo de **netsh interface httpstunnel show interfaces** e pressione ENTER.  
  
    A saída deve conter a URL: https://2-edge1.contoso.com:443/IPHTTPS.  
  
    > [!TIP]  
    > Em CLIENT1, você também pode executar o comando a seguir: **Get-NetIPHTTPSConfiguration**. A saída mostra as conexões de URL do servidor disponíveis e o perfil ativo no momento.  
  
    > [!NOTE]  
    > CLIENT1 altera automaticamente o servidor por meio do qual ele se conecta aos recursos corporativos. Se a saída do comando mostra uma conexão para EDGE1, aguarde aproximadamente cinco minutos e, em seguida, tente novamente.  
  
6.  Na janela do Windows PowerShell, digite **ping app1** e pressione ENTER. Você deve ver as respostas do endereço IPv6 atribuído ao APP1, que nesse caso é 2001:db8:1::3.  
  
7.  Na janela do Windows PowerShell, digite **ping 2-app1** e pressione ENTER. Você deve ver as respostas do endereço IPv6 atribuído a 2-APP1, que nesse caso é 2001:db8:2::3.  
  
8.  Na janela do Windows PowerShell, digite **ping app2** e pressione ENTER. Você deverá ver as respostas do endereço assinado NAT64 atribuído pelo EDGE1 para APP2, que nesse caso é fd**c9:9f4e:eb1b**: 7777::a00:4. Observe que os valores em negrito irão variar devido a como o endereço é gerado.  
  
    A capacidade de executar o ping APP2 é importante, porque o sucesso indica que você não pode estabelecer uma conexão usando o NAT64 e o DNS64, como APP2 é um recurso somente do IPv4.  
  
9. Abra o Internet Explorer, na barra de endereços do Internet Explorer, digite **https://app1/** e pressione ENTER. Você verá o site de IIS padrão no APP1.  
  
10. Na barra de endereços do Internet Explorer, digite **https://2-app1/** e pressione ENTER. Você verá o site padrão no APP2.  
  
11. Na barra de endereços do Internet Explorer, digite **https://app2/** e pressione ENTER. Você verá o site padrão no APP3.  
  
12. Sobre o **inicie** tela, digite**\\\App1\Files**, e pressione ENTER. Clique duas vezes no arquivo de texto de exemplo.  
  
    Isso demonstra que você conseguiu se conectar ao servidor de arquivos no domínio corp.contoso.com quando conectados por meio de EDGE1 2.  
  
13. Sobre o **inicie** tela, digite**\\\App2\Files**, e pressione ENTER. Clique duas vezes no arquivo Novo Documento de Texto.  
  
    Isso demonstra que você conseguiu se conectar a um servidor de somente IPv4 usando SMB para obter um recurso no domínio de recurso.  
  
14. Repita esse procedimento em CLIENT2 da etapa 3.  
  


