---
title: Perguntas frequentes de clientes de Área de Trabalho Remota
description: Perguntas frequentes sobre os clientes de área de trabalho remota
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 54ed455955053ebb234864f827759385ecf3d3c5
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935032"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>Perguntas frequentes sobre os clientes de área de trabalho remota

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Agora que configurou o cliente de Área de Trabalho Remota em seu dispositivo (Android, Mac, iOS ou Windows), você pode ter dúvidas. Veja as respostas para as perguntas mais frequentes sobre os clientes de Área de Trabalho Remota. 

- [Configurar](#setting-up)
- [Conexões, gateway e redes](#connection-gateway-and-networks)
- [Cliente Web](#web-client)
- [Monitores, áudio e mouse](#monitors-audio-and-mouse)
- [Hardware Mac](#mac-client---hardware-questions)
- [Mensagens de erro específicas](#specific-errors)

A maioria dessas perguntas se aplica a todos os clientes, mas há alguns itens específicos do cliente.

Se você tiver perguntas adicionais que gostaria que respondêssemos, deixe-as como comentários neste artigo.

## <a name="setting-up"></a>Configurar

### <a name="which-pcs-can-i-connect-to"></a>A quais computadores posso me conectar?

Confira o artigo [configuração compatível](remote-desktop-supported-config.md) para obter informações sobre a quais computadores você pode se conectar.

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>Como configurar um computador para a Área de Trabalho Remota?

Tenho meu dispositivo configurado, mas não acho que o computador está pronto. Ajuda?

Primeiro, você viu o Assistente de Instalação de Área de Trabalho Remota? Ele vai orientá-lo na preparação do computador para acesso remoto. Baixe e execute a ferramenta em seu computador configurar tudo. 

Caso contrário, se preferir fazer manualmente, continue lendo.

No Windows 10, faça o seguinte:

1. No dispositivo ao qual deseja se conectar, abra **Configurações**.
2. Selecione **System** e **Área de Trabalho Remota**.
3. Use o controle deslizante para habilitar a Área de Trabalho Remota.
4. Em geral, é melhor manter o PC ativo e detectável para facilitar as conexões. Clique em **Mostrar configurações** para ir para as configurações de energia do computador onde é possível alterar essa configuração.
   > [!NOTE]
   > Não é possível se conectar a um computador que está no estado de suspensão ou hibernação; portanto, verifique se as configurações de suspensão e hibernação no computador remoto estão definidas como **Nunca**. A hibernação não está disponível em todos os computadores.


Anote o nome deste computador em **Como se conectar a este computador**. Você precisará dele para configurar os clientes.

Você pode conceder permissão de acesso a este computador para usuários específicos. Para fazer isso, clique em **Selecionar os usuários que podem acessar este computador remotamente**.
Os membros do grupo Administradores têm acesso automático.

No Windows 8.1, siga as instruções para permitir conexões remotas em [Conectar-se a outra área de trabalho usando Conexão de Área de Trabalho Remota](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## <a name="connection-gateway-and-networks"></a>Conexão, gateway e redes

### <a name="why-cant-i-connect-using-remote-desktop"></a>Por que não consigo me conectar usando a Área de Trabalho Remota?

Aqui estão algumas possíveis soluções para problemas comuns que podem ocorrer ao tentar se conectar a um computador remoto. Se essas soluções não funcionarem, você poderá encontrar mais ajuda no [site da Microsoft Community](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **O computador remoto não foi encontrado.** Verifique se o nome do computador está correto e, em seguida, verifique se inseriu o nome corretamente. Caso ainda não consiga se conectar, tente usar o endereço IP do computador remoto em vez do nome dele.
- **Há um problema com a rede.** Verifique se você tem uma conexão de Internet. 
- **A porta de Área de Trabalho Remota pode estar bloqueada por um firewall.** Se você estiver usando o Firewall do Windows, siga estas etapas:

  1. Abra o Firewall do Windows. 
  2. Clique em **Permitir um aplicativo ou recurso pelo Firewall do Windows**. 
  3. Clique em **Alterar configurações**. Talvez você seja solicitado a inserir uma senha de administrador ou a confirmar sua opção.
  4. Em **Aplicativos e recursos permitidos**, selecione **Área de Trabalho Remota** e toque ou clique em **OK**.

     Se você estiver usando um firewall diferente, verifique se a porta para a Área de Trabalho Remota (normalmente 3389) é aberta.
- **Conexões remotas não podem ser configuradas no computador remoto.** Para corrigir esse problema, role para cima até a pergunta [Como configurar um computador para a Área de Trabalho Remota?](#how-do-i-set-up-a-pc-for-remote-desktop) neste tópico.
- **O computador remoto pode permitir apenas a conexão de computadores com a Autenticação no Nível da Rede configurada.** 
- **O computador remoto pode ter sido desligado.** Não é possível se conectar a um computador que está desligado, no estado de suspensão ou hibernação, portanto, verifique se as configurações de suspensão e hibernação no computador remoto estão definidas como **Nunca** (a hibernação não está disponível em todos os computadores).

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>Por que não é possível localizar ou se conectar ao meu computador?

Verifique o seguinte:

- O computador está ligado e ativo?
- Você inseriu o nome ou o endereço IP correto?

   > [!IMPORTANT]
   > Para usar o nome do computador, é preciso que a rede resolva o nome corretamente por DNS. Em muitas redes domésticas, você precisa usar o endereço IP em vez do nome de host para se conectar.
- O computador está em uma rede diferente? Você configurou o computador para permitir conexões de fora?  Confira [Conceder acesso ao seu computador de fora da rede](remote-desktop-allow-outside-access.md) para obter ajuda.
- Você está se conectando a uma versão do Windows com suporte? 

   > [!NOTE]
   > Não há suporte para Windows XP Home, Windows Media Center Edition, Windows Vista Home e Windows 7 Home ou Starter sem software de terceiros.

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>Por que não consigo entrar em um computador remoto?

Se consegue ver a tela de logon do computador remoto, mas não consegue entrar, você pode não ter sido adicionado ao Grupo de Usuários de Área de Trabalho Remota ou a qualquer grupo com direitos de administrador no computador remoto. Peça ao administrador de sistema para fazer isso para você.

### <a name="which-connection-methods-are-supported-for-company-networks"></a>Quais métodos de conexão têm suporte em redes de empresa?

Se você quiser acessar sua área de trabalho do escritório de fora da rede da empresa, a empresa deve fornecem um meio de acesso remoto. O cliente de área de trabalho remota atualmente tem suporte para o seguinte:

- Gateway de Servidor de Terminal ou Gateway de Área de Trabalho Remota
- Acesso via Web à Área de Trabalho Remota
- VPN (com opções de VPN internas do iOS)

### <a name="vpn-doesnt-work"></a>A VPN não funciona

Problemas de VPN podem ter várias causas. A primeira etapa é verificar se a VPN funciona na mesma rede que o PC ou Mac. Se você não puder testar com um PC ou Mac, é possível tentar acessar uma página da Web da intranet da empresa com o navegador do dispositivo.

Outros itens a serem verificados:
- **A rede 3G bloqueia ou corrompe a VPN.** Há vários provedores de 3G no mundo que parecem bloquear ou corromper o tráfego de 3G. Verifique se a conectividade VPN funciona corretamente por mais de um minuto.
- **VPNs L2TP ou PPTP.** Se você estiver usando o PPTP ou L2TP em sua VPN, configure Enviar Todo Tráfego como LIGADO na configuração de VPN.
- **A VPN está configurada incorretamente.** Um servidor VPN configurado incorretamente pode ser o motivo pelo qual as conexões VPN nunca funcionaram ou pararam de funcionar após algum tempo. Certifique-se de testar com o navegador do dispositivo iOS ou em um PC ou Mac na mesma rede, se isso acontecer.

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>Como testar se a VPN está funcionando corretamente?

Verifique se a VPN está habilitada em seu dispositivo. Você pode testar sua conexão VPN indo para uma página da Web na rede interna ou usando um serviço da Web que só está disponível pela VPN.

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>Como fazer para configurar conexões VPN de PPTP ou L2TP?

Se você estiver usando o PPTP ou L2TP em sua VPN, certifique-se de configurar **Enviar todo o tráfego** como **LIGADO** na configuração de VPN.

## <a name="web-client"></a>Cliente Web

### <a name="which-browsers-can-i-use"></a>Quais navegadores posso usar?

O cliente web é compatível com Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 e posterior), Google Chrome e Safari.

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>Que computadores posso usar para acessar o cliente web?

O cliente é compatível com Windows, macOS, Linux e ChromeOS. Não há suporte para dispositivos móveis no momento.

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>Posso usar o cliente web em uma implantação de Área de Trabalho Remota sem um gateway?

Não. O cliente precisa de um Gateway de Área de Trabalho Remota para se conectar. Não sabe o que isso significa? Pergunte ao seu administrador sobre ele.

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>O cliente da web de Área de Trabalho Remota substitui a página de Acesso à Web da Área de Trabalho Remota?

Não. O cliente da web de Área de Trabalho Remota é hospedado em uma URL diferente da página de Acesso à Web da Área de Trabalho Remota. Você pode usar o cliente da web ou a página de Acesso à Web para exibir os recursos remotos em um navegador.

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>Posso inserir o cliente da Web em outra página da Web?

Este recurso não tem suporte no momento.

## <a name="monitors-audio-and-mouse"></a>Monitores, áudio e mouse

### <a name="how-do-i-use-all-of-my-monitors"></a>Como usar todos os meus monitores?
Para usar duas ou mais telas, faça o seguinte:

1. Clique com o botão direito do mouse na área de trabalho remota para a qual deseja habilitar várias telas e clique em **Editar**.
2. Habilite **Usar todos os monitores** e **Tela inteira**.

### <a name="is-bi-directional-sound-supported"></a>Há suporte para som bidirecional?
O som bidirecional pode ser configurado no cliente do Windows por conexão. As configurações relevantes podem ser acessadas na seção **Áudio remoto** da guia de opções **Recursos Locais**.

### <a name="what-can-i-do-if-the-sound-wont-play"></a>O que fazer se o som não for reproduzido?
Saia da sessão (não apenas se desconecte, saia totalmente) e entre novamente.

## <a name="mac-client---hardware-questions"></a>Cliente Mac - perguntas de hardware
### <a name="is-retina-resolution-supported"></a>Há suporte para resolução de retina?
Sim, o cliente de área de trabalho remota dá suporte à resolução de retina.

### <a name="how-do-i-enable-secondary-right-click"></a>Como habilitar o clique secundário com o botão direito do mouse?
Para fazer uso do botão direito do mouse dentro de uma sessão aberta, você tem três opções:

- Mouse USB para PC de dois botões padrão
- Apple Magic Mouse: Para habilitar o botão direito do mouse, clique em **Preferências do Sistema** no dock, clique em **Mouse** e habilite **Clique secundário**.
- Apple Magic Trackpad ou MacBook Trackpad: Para habilitar o botão direito do mouse, clique em **Preferências do Sistema** no dock, clique em **Mouse** e habilite **Clique secundário**.

### <a name="is-airprint-supported"></a>Há suporte para AirPrint?
Não, o cliente de Área de Trabalho Remota não é compatível com AirPrint. Isso acontece tanto em clientes Mac quanto iOS.

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>Por que aparecem caracteres incorretos na sessão?
Se você estiver usando um teclado internacional, poderá encontrar um problema em que os caracteres que aparecem na sessão correspondem aos caracteres digitados no teclado do Mac.

Isso pode acontecer nos seguintes cenários:

- Você está usando um teclado que não é reconhecido pela sessão remota. Quando a Área de Trabalho Remota não reconhece o teclado, ela adota como padrão o idioma usado pela última vez com o computador remoto.
- Você está se conectando a uma sessão desconectada anteriormente em um computador remoto e o computador remoto usa um idioma diferente do teclado do que você está tentando usar no momento.

Você pode corrigir esse problema configurando manualmente o idioma do teclado para a sessão remota. Veja as etapas na próxima seção.

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>Como as configurações de idioma afetam os teclados em uma sessão remota?
Há muitos tipos de layouts de teclado do Mac. Alguns deles são layouts específicos de Mac ou layouts personalizados para os quais uma correspondência exata pode não estar disponível na versão remota do Windows que você está usando. A sessão remota mapeia seu teclado para a melhor correspondência de idioma de teclado disponível no computador remoto. 

Se o layout do teclado Mac estiver configurado para a versão do computador do teclado do idioma (por exemplo, francês – computador) todas as teclas deverão ser mapeadas corretamente e o teclado deverá funcionar.

Se o seu layout de teclado do Mac estiver definido para a versão do Mac de um teclado (por exemplo, francês) a sessão remota mapeará para a versão de PC do idioma francês. Alguns dos atalhos de teclado do Mac com os quais você está acostumado no OSX não funcionarão na sessão remota do Windows.

Se o layout do teclado está definido como uma variação de um idioma (por exemplo, francês do Canadá) e se a sessão remota não conseguir mapear para essa variação exata, a sessão remota mapeará para o idioma mais próximo (por exemplo, francês). Alguns dos atalhos de teclado do Mac com os quais você está acostumado no OSX não funcionarão na sessão remota do Windows.

Se o layout do teclado estiver definido como um layout para o qual não haja correspondência na sessão remota, a sessão remota adotará como padrão oferecer o último idioma usado com esse PC. Nesse caso, ou em casos em que precisa alterar o idioma da sessão remota para corresponder ao teclado do Mac, você pode definir manualmente o idioma do teclado na sessão remota para o idioma mais próximo ao que você deseja usar como a seguir.

Use as instruções a seguir para alterar o layout do teclado na sessão da área de trabalho remota:

**No Windows 10 ou Windows 8:**

1. De dentro da sessão remota, abra Região e Idioma. Clique em **Iniciar > Configurações > Hora e Idioma**. Abra **Região e Idioma**.
2. Adicione o idioma que você deseja usar. Feche a janela de Região e Idioma.
3. Agora, na sessão remota, você terá a capacidade de alternar entre idiomas. No lado direito da sessão remota, perto do relógio. Clique no idioma para o qual deseja alternar (como **Eng**).

Talvez você precise fechar e reiniciar o aplicativo que está usando no momento para que as alterações de teclado entrem em vigor.


## <a name="specific-errors"></a>Erros específicos

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>Por que recebo um erro de "Privilégios insuficientes"?
Você não tem permissão para acessar a sessão à qual deseja se conectar. A causa mais provável é que você está tentando se conectar a uma sessão de administrador. Somente os administradores têm permissão para se conectar ao console. Verifique se o botão do console está desligado nas configurações avançadas de área de trabalho remota. Se isso não for a origem do problema, entre em contato com o administrador do sistema para obter assistência adicional.

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>Por que o cliente disse que não há nenhuma CAL?
Quando um cliente de área de trabalho remoto se conecta a um servidor de Área de Trabalho Remota, o servidor emite uma Licença de acesso para cliente de Serviços de Área de Trabalho Remota (RDS CAL) armazenada pelo cliente. Sempre que o cliente se conectar novamente, ele usará sua RDS CAL e o servidor não emitirá outra licença. O servidor emitirá outra licença se a RDS CAL no dispositivo estiver ausente ou corrompida. Quando o número máximo de dispositivos licenciados for atingido, o servidor não emitirá novas RDS CALs. Entre em contato com o administrador de rede para obter assistência.

### <a name="why-did-i-get-an-access-denied-error"></a>Por que recebi um erro de "Acesso Negado"?
O erro "Acesso Negado" é gerado pelo Gateway de Área de Trabalho Remota e o resultado de credenciais incorretas durante a tentativa de conexão. Verifique seu nome de usuário e senha. Se a conexão estava funcionando antes e o erro ocorreu recentemente, você possivelmente alterou a senha de conta de usuário do Windows e ainda não a atualizou nas configurações de área de trabalho remota.

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>O que significam os erros "RPC Error 23014" ou "Error 0x59e6"?
Caso haja um erro **RPC error 23014** ou **Error 0x59E6, tente novamente após alguns minutos**, o servidor de Gateway de Área de Trabalho Remota atingiu o número máximo de conexões ativas. Dependendo da versão do Windows em execução no Gateway de Área de Trabalho Remota, o número máximo de conexões é diferente: A implementação do Windows Server 2008 R2 Standard limita o número de conexões a 250. A implementação do Windows Server 2008 R2 Foundation limita o número de conexões a 50. Todas as outras implementações do Windows permitem que um número ilimitado de conexões.

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>O que significa o erro "Falha de análise do desafio NTLM"?
Esse erro é causado por uma configuração incorreta no computador remoto. Verifique se a configuração de nível de segurança do protocolo RDP no computador remoto está definida como "Compatível com o Cliente". Converse com o administrador de sistema caso precise de ajuda para fazer isso.

### <a name="what-does-ts_rap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>O que significa "TS_RAP Você não tem permissão para se conectar ao host determinado"?
Esse erro ocorre quando uma Política de Autorização de Recursos no servidor gateway impede que seu nome de usuário se conecte ao computador remoto. Isso pode acontecer nas seguintes instâncias:

- O nome do computador remoto é igual ao nome do gateway. Então, quando você tenta se conectar ao computador remoto, a conexão vai para o gateway em vez disso e você provavelmente não tem permissão para acessá-lo. Se você precisar se conectar ao gateway, não use o nome do gateway externo como o nome do computador. Em vez disso, use "localhost", o endereço IP (127.0.0.1) ou o nome do servidor interno.
- Sua conta de usuário não é um membro do grupo de usuários para acesso remoto.
