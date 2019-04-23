---
title: Clientes de Desktop remotos perguntas Frequentes
description: Perguntas frequentes sobre os clientes de área de trabalho remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 785a18cf-a5d0-4bc2-95e4-9ef53ee8f65a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: ec1b0a17c578f2d8ac55d1704af6b267b6bb8e5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865927"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>Perguntas frequentes sobre os clientes de área de trabalho remota

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Agora que você configurou o cliente de área de trabalho remota em seu dispositivo (Android, Mac, iOS ou Windows), você pode ter dúvidas. Aqui estão as respostas às perguntas mais frequentes sobre os clientes de área de trabalho remota. 

- [Como configurar](#Setting-up)
- [Conexões de gateway e redes](#connection-gateway-and-networks)
- [cliente Web](#web-client)
- [Monitores, áudio e mouse](#monitors-audio-and-mouse)
- [Hardware do Mac](#mac-client---hardware-questions)
- [Mensagens de erro específicas](#specific-errors)

A maioria dessas perguntas se aplicam a todos os clientes, mas há alguns itens específicos do cliente.

Se você tiver perguntas adicionais que você gostaria de respondermos, deixá-los como comentários sobre este artigo.

## <a name="setting-up"></a>Como configurar

### <a name="which-pcs-can-i-connect-to"></a>Quais computadores posso me conectar?

Confira a [suporte para configuração](remote-desktop-supported-config.md) artigo para obter informações sobre quais computadores você pode se conectar ao.

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>Como configurar um computador para a área de trabalho remota?

Tenho meu dispositivo configurado, mas acho que pronto do PC. Ajuda?

Primeiro, você viu que o Assistente de instalação de área de trabalho remota? Ele orienta você por meio de preparação de seu PC para acesso remoto. Baixar e executar a ferramenta em seu computador para obter tudo o que é definido. 

Caso contrário, se preferir fazer coisas manualmente, continue lendo.

Para Windows 10, faça o seguinte:

1. No dispositivo que você deseja se conectar, abra **configurações**.
2. Selecione **System** e, em seguida **área de trabalho remota**.
3. Use o controle deslizante para habilitar a área de trabalho remota.
4. Em geral, é melhor manter o PC ativos e detectável para facilitar as conexões. Clique em **Mostrar configurações** para ir para as configurações de energia para o seu PC, onde você pode alterar essa configuração.
   > [!NOTE]
   > Você não pode se conectar a um computador que está no estado de suspensão ou hibernação, portanto, verifique se as configurações de suspensão e hibernação no PC remoto estiverem definida como **nunca**. (A hibernação não está disponível em todos os computadores.)


Anote o nome deste PC sob **como se conectar a este computador**. Você precisará dela para configurar os clientes.

Você pode conceder permissão para usuários específicos acessar este PC - para fazer isso, clique em **selecione os usuários que podem acessar remotamente esse PC**.
Automaticamente, membros do grupo Administradores têm acesso.

Para Windows 8.1, siga as instruções para permitir conexões remotas no [conectar-se a outra área de trabalho usando conexões de área de trabalho remota](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## <a name="connection-gateway-and-networks"></a>Conexão, o gateway e redes

### <a name="why-cant-i-connect-using-remote-desktop"></a>Por que não consigo me conectar usando a área de trabalho remota?

Aqui estão algumas possíveis soluções para problemas comuns que pode ocorrer ao tentar se conectar a um computador remoto. Se essas soluções não funcionarem, você pode encontrar mais ajuda sobre o [site Microsoft Community](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **O computador remoto não foi encontrado.** Verifique se você tem o nome correto do PC e, em seguida, verifique se você inseriu corretamente esse nome. Se ainda não é possível se conectar, tente usar o endereço IP do PC remoto em vez do nome do computador.
- **Há um problema com a rede.** Verifique se que você tem conexão de internet. 
- **A porta de área de trabalho remota pode ser bloqueada por um firewall.** Se você estiver usando o Firewall do Windows, siga estas etapas:

   1. Abra o Firewall do Windows. 
   2. Clique em **permitir um aplicativo ou recurso pelo Firewall do Windows**. 
   3. Clique em **alterar configurações**. Você pode ser solicitado para uma senha de administrador ou para confirmar sua escolha.
   4. Sob **aplicativos e recursos permitidos**, selecione **área de trabalho remota**e, em seguida, toque ou clique em **Okey**.

   Se você estiver usando um firewall diferente, verifique se que a porta para a área de trabalho remota (normalmente 3389) é aberta.
- **Conexões remotas não podem ser configuradas no PC remoto.** Para corrigir esse problema, role para cima até [como para configurar um computador para a área de trabalho remota?](#how-do-i-set-up-a-pc-for-remote-desktop) pergunta neste tópico.
- **PC remoto pode permitir apenas PCs conectar-se com a configuração de autenticação de nível de rede.** 
- **O computador remoto pode ter sido desligado.** Você não pode se conectar a um computador que está desligado, suspensão ou hibernação, portanto, verifique se as configurações de suspensão e hibernação no PC remoto estiverem definida como **nunca** (hibernação não está disponível em todos os computadores.).

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>Por que não é possível localizar ou se conectar ao meu PC?
Verifique o seguinte:
- É o PC e ativo?
- Você inseriu o nome correto ou o endereço IP?

   > [!IMPORTANT]
   > Usando o nome de computador requer sua rede para resolver o nome corretamente por meio do DNS. Em muitas redes domésticas, você precisa usar o endereço IP em vez do nome de host para se conectar.
- É o PC em uma rede diferente? Você configurou o computador para permitir que o fora de conexões por meio de?  Fazer check-out [permitir o acesso ao seu PC de fora da sua rede](remote-desktop-allow-outside-access.md) para obter ajuda.
- Você estiver se conectando a uma versão com suporte do Windows? 

   > [!NOTE]
   > Não há suporte para Windows XP Home, Windows Media Center Edition, Windows Vista Home e Windows 7 Home ou Starter sem 3ª software de terceiros.

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>Por que não posso entrar em um computador remoto?
Se você pode ver a tela de logon do computador remoto, mas não conseguir entrar, você talvez não foram adicionados ao grupo de usuários da área de trabalho remota ou a qualquer grupo com direitos de administrador no PC remoto. Peça ao seu administrador de sistema para fazer isso para você.

### <a name="which-connection-methods-are-supported-for-company-networks"></a>Quais métodos de conexão têm suporte para redes da empresa?
Se você quiser acessar sua área de trabalho office de fora da rede da empresa, sua empresa deve fornecem um meio de acesso remoto. O cliente de área de trabalho remota atualmente suporta o seguinte:

- Gateway de servidor de terminal ou Gateway de área de trabalho remota
- Acesso via Web da área de trabalho remota
- VPN (por meio de opções de VPN internos do iOS)

### <a name="vpn-doesnt-work"></a>VPN não funciona

Problemas de VPN podem ter várias causas. A primeira etapa é verificar se a VPN funciona na mesma rede que o seu PC ou Mac. Se você não pode testar com um PC ou Mac, você pode tentar acessar uma página de web da intranet da empresa com o navegador do dispositivo.

Outros itens a serem verificados:
- **A rede 3G bloqueia ou corrompe VPN.** Há vários provedores de 3G no mundo que parecem como bloqueada ou corrompida tráfego de 3G. Verificar a conectividade VPN funciona corretamente para mais de um minuto.
- **L2TP ou PPTP VPNs.** Se você estiver usando o PPTP ou L2TP em sua VPN, defina enviar todo o tráfego como ON na configuração de VPN.
- **VPN está configurado incorretamente.** Um servidor VPN configurado incorretamente pode ser o motivo por que as conexões VPN nunca funcionou ou parou de funcionar após algum tempo. Certifique-se de testar com o iOS navegador da web do dispositivo ou um PC ou Mac na mesma rede, se isso acontecer.

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>Como testar se a VPN está funcionando corretamente?
Verifique se que a VPN estiver habilitada em seu dispositivo. Você pode testar sua conexão VPN indo para uma página da Web em sua rede interna ou usando um serviço web que só está disponível por meio da VPN.

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>Como fazer para configurar conexões VPN de PPTP ou L2TP?
Se você estiver usando o PPTP ou L2TP em sua VPN, certifique-se de definir **enviar todo o tráfego** à **ON** na configuração de VPN.

## <a name="web-client"></a>cliente Web

### <a name="which-browsers-can-i-use"></a>Quais navegadores pode usar?

O cliente web dá suporte ao Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 e posterior), Google Chrome e Safari.

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>Quais computadores podem usar para acessar o cliente da web?

O cliente web dá suporte a Windows, macOS, Linux e ChromeOS. Não há suporte para dispositivos móveis no momento.

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>Pode usar o cliente da web em uma implantação de área de trabalho remota sem um gateway?

Nenhum. O cliente requer um Gateway de área de trabalho remota para se conectar. Não sabe o que isso significa? Peça ao seu administrador sobre ele.

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>O cliente da web de área de trabalho remota substitui a página de acesso remoto via Web da área de trabalho?

Nenhum. O cliente da web de área de trabalho remota é hospedado em um URL diferente que a página de acesso remoto via Web da área de trabalho. Você pode usar o cliente da web ou a página de acesso via Web para exibir os recursos remotos em um navegador.

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>Pode inserir o cliente da web em outra página da web?

Não há suporte para esse recurso no momento.

## <a name="monitors-audio-and-mouse"></a>Monitores, áudio e mouse

### <a name="how-do-i-use-all-of-my-monitors"></a>Como usar todos os meus monitores?
Para usar duas ou mais telas, faça o seguinte:

1. A área de trabalho remota que você deseja habilitar várias telas para e, em seguida, clique com o botão direito **editar**.
2. Habilitar **usar todos os monitores** e **tela inteira**.

### <a name="is-bi-directional-sound-supported"></a>Há suporte para som bidirecional
Não há suporte para som upstream (do cliente ao servidor, para microfones) pelo cliente de área de trabalho remota.

### <a name="what-can-i-do-if-the-sound-wont-play"></a>O que fazer se o som não será reproduzido?
Saia da sessão (não apenas se desconectar, sair completamente) e entre novamente.

## <a name="mac-client---hardware-questions"></a>Cliente Mac - perguntas de hardware
### <a name="is-retina-resolution-supported"></a>Há suporte para resolução retina
Sim, o cliente de área de trabalho remota oferece suporte à resolução retina.

### <a name="how-do-i-enable-secondary-right-click"></a>Como habilitar o botão direito do mouse secundário?
Para fazer uso do botão direito do mouse dentro de uma sessão aberta, você tem três opções:

- Mouse USB de botão de PC dois padrão
- Mouse mágica da Apple: Para habilitar o botão direito do mouse, clique em **preferências do sistema** no dock, clique em **Mouse**e, em seguida, habilite **clique secundário**.
- Trackpad mágica da Apple ou MacBook Trackpad: Para habilitar o botão direito do mouse, clique em **preferências do sistema** no dock, clique em **Mouse**e, em seguida, habilite **clique secundário**.

### <a name="is-airprint-supported"></a>Há suporte para AirPrint
Não, o cliente de área de trabalho remota não dá suporte a AirPrint. (Isso é verdadeiro para clientes Mac e iOS.)

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>Por caracteres incorretos que aparecem na sessão?
Se você estiver usando um teclado internacional, você pode encontrar um problema em que os caracteres que aparecem na sessão corresponder aos caracteres digitados no teclado do Mac.

Isso pode ocorrer nos seguintes cenários:

- Você está usando um teclado que não reconhece a sessão remota. Quando a área de trabalho remota não reconhecer o teclado, o padrão é o idioma usado pela última vez com o PC remoto.
- Você está se conectando a uma sessão desconectada anteriormente em um computador remoto e que o PC remoto usa um idioma do teclado diferente no momento, você está tentando usar.

Você pode corrigir esse problema, defina manualmente o idioma do teclado para a sessão remota. Consulte as etapas na próxima seção.

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>Como as configurações de idioma afeta teclados em uma sessão remota?
Há muitos tipos de layouts de teclado do Mac. Alguns deles são layouts específicos de Mac ou layouts personalizados para o qual uma correspondência exata pode não estar disponível na versão do Windows que você está a comunicação remota em. A sessão remota mapeia seu teclado para a melhor correspondência do idioma do teclado no PC remoto. 

Se o layout do teclado está definido para a versão de PC do teclado de idioma (por exemplo, francês – PC) todas as suas chaves devem ser mapeadas corretamente e o teclado de Mac deve funcionar.

Se o seu layout de teclado do Mac está definido para a versão do Mac de um teclado (por exemplo, francês) na sessão remota mapeará para a versão de PC do idioma francês. Alguns dos atalhos de teclado do Mac que são usados para uso no OSX não funcionará na sessão remota do Windows.

Se o layout do teclado está definido como uma variação de um idioma (por exemplo, francês (Canadá)) e se a sessão remota não é possível mapear a essa variação exata, a sessão remota mapeará para a linguagem mais próxima (por exemplo, francês). Alguns dos atalhos de teclado do Mac que são usados para uso no OSX não funcionará na sessão remota do Windows.

Se o layout do teclado é definido como um layout que da sessão remota não pode corresponder a nada, sua sessão remota será padrão para dar a você o idioma usado pela última vez com esse PC. Nesse caso, ou em casos em que você precisa alterar o idioma da sessão remota para coincidir com o teclado do Mac, você pode definir manualmente o idioma do teclado na sessão remota para a linguagem que é a correspondência mais próxima àquela que você deseja usar como a seguir.

Use as instruções a seguir para alterar o layout do teclado dentro da sessão da área de trabalho remota:

**No Windows 10 ou Windows 8:**

1. De dentro da sessão remota, abra região e idioma. Clique em **Iniciar > Configurações > hora e idioma**. Abra **região e idioma**.
2. Adicione o idioma que você deseja usar. Em seguida, feche a janela de região e idioma.
3. Agora, na sessão remota, você verá a capacidade de alternar entre os idiomas. (No lado direito da sessão remota, perto do relógio). Clique no idioma que você deseja alternar (como **Eng**).

Talvez você precise fechar e reiniciar o aplicativo que você está usando no momento para que as alterações de teclado entrem em vigor.


## <a name="specific-errors"></a>Erros específicos

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>Por que recebo um erro de "Privilégios insuficientes"?
Você não tem permissão para acessar a sessão que você deseja se conectar. A causa mais provável é que você está tentando se conectar a uma sessão de administrador. Somente os administradores têm permissão para se conectar ao console. Verifique se que o comutador de console está desativado nas configurações avançadas de área de trabalho remota. Se isso não é a origem do problema, entre em contato com o administrador do sistema para obter assistência adicional.

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>Por que o cliente disse que não há nenhuma CAL?
Quando um cliente de área de trabalho remoto se conecta a um servidor de área de trabalho remota, o servidor emite uma remotos da área de trabalho serviços cliente acesso licença (RDS CAL) armazenado pelo cliente. Sempre que o cliente se conecta novamente, ele usará seu RDS CAL e o servidor não emitirá outra licença. O servidor emitirá a licença de outro se o CAL RDS no dispositivo está ausente ou corrompido. Quando é atingido o número máximo de dispositivos licenciados, o servidor não emitirá novo RDS CALs. Para obter assistência, entre em contato com seu administrador de rede.

### <a name="why-did-i-get-an-access-denied-error"></a>Por que eu recebi um erro "Acesso negado"?
O erro "Acesso negado" é gerado pelo Gateway de área de trabalho remota e o resultado de credenciais incorretas durante a tentativa de conexão. Verifique se seu nome de usuário e senha. Se a conexão estava funcionando antes e o erro ocorreu recentemente, você possivelmente alterou sua senha de conta de usuário do Windows e ainda não atualizou ainda nas configurações de área de trabalho remotas.

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>Qual o "Erro RPC 23014" ou a média de "Erro 0x59e6"?
No caso de um **erro de RPC 23014** ou **erro 0x59E6 tente novamente após aguardar alguns minutos**, o servidor de Gateway de área de trabalho remota atingiu o número máximo de conexões ativas. Dependendo da versão do Windows em execução no Gateway de área de trabalho remota, o número máximo de conexões é diferente: A implementação do Windows Server 2008 R2 Standard limita o número de conexões para 250. A implementação do Windows Server 2008 R2 Foundation limita o número de conexões para 50. Todas as outras implementações do Windows permitem que um número ilimitado de conexões.

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>O que significa o erro "Falha ao analisar o desafio NTLM"?
Esse erro é causado por uma configuração incorreta no PC remoto. Verifique se a configuração de nível de segurança RDP no PC remoto é definida como "Cliente compatível". (Se comunicar com seu administrador de sistema se você precisar de ajuda para fazer isso.)

### <a name="what-does-tsrap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>O que faz a "TS_RAP você não tem permissão para se conectar ao host de determinado" significa?
Esse erro ocorre quando uma política de autorização de recursos no servidor gateway para seu nome de usuário se conecte ao PC remoto. Isso pode ocorrer nas seguintes instâncias:

- O nome do computador remoto é igual ao nome do gateway. Em seguida, quando você tenta se conectar ao computador remoto, a conexão vai para o gateway em vez disso, que provavelmente não terá permissão para acessar. Se você precisar se conectar ao gateway, não use o nome do gateway externo como o nome do computador. Em vez disso, use "localhost" ou o endereço IP (127.0.0.1) ou o nome do servidor interno.
- Sua conta de usuário não é um membro do grupo de usuários para acesso remoto.