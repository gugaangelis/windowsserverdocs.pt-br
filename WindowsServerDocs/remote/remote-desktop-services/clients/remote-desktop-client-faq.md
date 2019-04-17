---
title: Clientes da área de trabalho remotos perguntas Frequentes
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
ms.sourcegitcommit: d5f10c0c98a9976a86be9f4fa8866650c7fcfc1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2019
ms.locfileid: "9065943"
---
# Perguntas frequentes sobre os clientes de área de trabalho remota

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Agora que você configurou o cliente de área de trabalho remota no seu dispositivo (Android, Mac, iOS ou Windows), você pode ter perguntas. Aqui estão as respostas para as perguntas mais frequentes sobre os clientes de área de trabalho remota. 

- [Configurando](#Setting-up)
- [Conexões, gateway e redes](#connection-gateway-and-networks)
- [Cliente da Web](#web-client)
- [Monitores, áudio e mouse](#monitors-audio-and-mouse)
- [Hardware de Mac](#mac-client---hardware-questions)
- [Mensagens de erro específicas](#specific-errors)

A maioria dessas perguntas se aplicam a todos os clientes, mas há alguns itens específicos do cliente.

Se você tiver outras dúvidas que você gostaria que nos responder, deixá-los como comentários sobre este artigo.

## Configurando

### Quais computadores pode conectar ao?

Confira o artigo para obter informações sobre quais computadores que você pode se conectar a [configuração com suporte](remote-desktop-supported-config.md) .

### Como configurar um computador para a área de trabalho remota?

Eu tenho meu dispositivo configurado, mas não acho pronto do computador. Ajuda?

Primeiro, você viu o Assistente de instalação de área de trabalho remota? Ele orienta você Preparando seu computador para acesso remoto. Baixar e executar a ferramenta em seu computador tudo definem. 

Caso contrário, se você preferir fazer coisas manualmente, continue lendo.

Para Windows 10, faça o seguinte:

1. No dispositivo deseja se conectar para abrir **configurações**.
2. Selecione **sistema** e, em seguida, **área de trabalho remota**.
3. Use o controle deslizante para habilitar a área de trabalho remota.
4. Em geral, é melhor manter o computador ativo e detectável para facilitar conexões. Clique em **Mostrar configurações** para ir para as configurações de energia para o seu computador, onde você pode alterar essa configuração.
   > [!NOTE]
   > Você não pode se conectar a um computador que esteja no estado de suspensão ou hibernação, portanto, verifique se que as configurações de suspensão e hibernação no computador remoto estão definidas como **nunca**. (A hibernação não está disponível em todos os computadores).


Anote o nome do PC em **como se conectar a este computador**. Isso será necessário configurar os clientes.

Você pode conceder permissão para usuários específicos acessar este PC - para fazer isso, clique em **Selecionar usuários que podem acessar remotamente este computador**.
Membros do grupo Administradores têm acesso automaticamente.

Para Windows 8.1, siga as instruções para permitir conexões remotas em [conectar-se a outra área de trabalho usando conexões de área de trabalho remota](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8).



## Conexão, gateway e redes

### Por que não consigo me conectar usando a área de trabalho remota?

Aqui estão alguns possíveis soluções para problemas comuns que você pode encontrar ao tentar se conectar a um computador remoto. Se essas soluções não funcionam, você pode encontrar mais ajuda no [site da comunidade da Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=242079).

- **O computador remoto não pode ser encontrado.** Verifique se você tem o melhor nome de computador e, em seguida, verifique se esse nome foi inserido corretamente. Se você ainda não pode se conectar, tente usar o endereço IP do computador remoto em vez do nome do computador.
- **Há um problema com a rede.** Verifique se que você tem conexão de internet. 
- **A porta de área de trabalho remota pode ser bloqueada por um firewall.** Se você estiver usando o Firewall do Windows, siga estas etapas:

   1. Abra o Firewall do Windows. 
   2. Clique em **Permitir um aplicativo ou recurso através do Firewall do Windows**. 
   3. Clique em **Alterar configurações**. Talvez você seja solicitado uma senha de administrador ou a confirmar sua escolha.
   4. Em **aplicativos permitidos e recursos**, selecione a **Área de trabalho remota**e, em seguida, toque ou clique **Okey**.

   Se você estiver usando um firewall diferente, verifique se que a porta para área de trabalho remota (normalmente 3389) está aberta.
- **Conexões remotas não podem ser configuradas no computador remoto.** Para corrigir isso, role para cima para [como configurar um computador para a área de trabalho remota?](#how-do-i-set-up-a-pc-for-remote-desktop) pergunta neste tópico.
- **O computador remoto deverá apenas permitir PCs para se conectar com configurar autenticação no nível da rede.** 
- **O computador remoto pode ser desativado.** Você não pode se conectar a um computador que estiver desativado, asleep ou hibernação, portanto, verifique se as configurações de suspensão e hibernação no computador remoto são definidas para **nunca** (a hibernação não está disponível em todos os PCs.).

### Por que não consigo localizar ou se conectar a meu computador?
Verifique o seguinte:
- É o computador e ativo?
- Você inserir o melhor nome ou endereço IP?

   > [!IMPORTANT]
   > Usando o nome de computador requer sua rede para resolver o nome corretamente por meio do DNS. Em muitas redes domésticas, você precisa usar o endereço IP em vez do nome de host para se conectar.
- É o computador em uma rede diferente? Você configurou o computador para permitir que fora conexões por meio do?  Confira [Permitir acesso ao seu computador de fora da rede](remote-desktop-allow-outside-access.md) para obter ajuda.
- Você está se conectando para uma versão compatível do Windows? 

   > [!NOTE]
   > Não há suporte para Windows XP Home, Windows Media Center Edition, Windows Vista Home e Windows 7 Home ou Starter sem 3ª software de terceiros.

### Por que não consigo entrar em um computador remoto?
Se você pode ver a tela de entrada do computador remoto, mas você não pode entrar, você talvez não foram adicionadas ao grupo de usuários de área de trabalho remota ou a qualquer grupo com direitos de administrador no computador remoto. Solicitar que o administrador do sistema para fazer isso para você.

### Quais métodos de conexão têm suporte para redes de empresa?
Se você deseja acessar sua área de trabalho office de fora da rede da empresa, sua empresa deve fornecer com um meio de acesso remoto. O cliente de área de trabalho remota atualmente oferece suporte a:

- Gateway de servidor de terminal ou Gateway da área de trabalho remota
- Acesso da Web da área de trabalho remota
- VPN (por meio de opções de VPN internas do iOS)

### VPN não funciona

Problemas VPN podem ter várias causas. A primeira etapa é verificar se a VPN funciona na mesma rede que seu computador Mac ou PC. Se você não pode testar com um Mac ou PC, você pode tentar acessar uma página de Web da intranet da empresa com o navegador do dispositivo.

Para verificar se há outras coisas:
- **Rede 3G bloqueia ou corrompe VPN.** Há vários provedores 3G no mundo que parecem bloco ou corrompidos tráfego 3G. Verificar a conectividade VPN funciona corretamente para mais de um minuto.
- **L2TP ou PPTP VPNs.** Se você estiver usando PPTP ou L2TP na VPN, defina enviar todo o tráfego para Diante na configuração de VPN.
- **VPN é configurado incorretamente.** Um servidor VPN configurados incorretamente pode ser o motivo por que as conexões VPN nunca funcionavam ou parou de funcionar depois de algum tempo. Certifique-se de teste com o navegador da web do dispositivo ou um computador ou Mac do iOS na mesma rede se isso acontecer.

### Como testar se a VPN estiver funcionando corretamente?
Verificar se a VPN é habilitada em seu dispositivo. Você pode testar sua conexão VPN indo para uma página da Web em sua rede interna ou usando um serviço web que só está disponível por meio de VPN.

### Como configurar conexões VPN de PPTP ou L2TP?
Se você estiver usando PPTP ou L2TP na VPN, certifique-se de definir **Enviar todo o tráfego** para **Diante** na configuração de VPN.

## Cliente da Web

### Quais navegadores pode usar?

O cliente web dá suporte ao Microsoft Edge, Internet Explorer 11, Mozilla Firefox (v55.0 e posterior), Safari e Google Chrome.

### Quais computadores podem usar para acessar o cliente da web?

O cliente web dá suporte ao Windows, macOS, Linux e ChromeOS. Não há suporte para dispositivos móveis no momento.

### Pode usar o cliente da web em uma implantação de área de trabalho remota sem um gateway?

Não. O cliente requer um Gateway de área de trabalho remota para se conectar. Não sabe o que isso significa? Pergunte ao seu administrador sobre ele.

### O cliente da web de área de trabalho remota substitui a página de acesso de Web de área de trabalho remota?

Não. O cliente de área de trabalho remota web é hospedado em uma URL diferente que a página de acesso remoto de Web de área de trabalho. Você pode usar o cliente da web ou a página da Web acesso para exibir os recursos remotos em um navegador.

### Pode incorporar o cliente da web em outra página da web?

Esse recurso não é suportado no momento.

## Monitores, áudio e mouse

### Como usar todos os meus monitores?
Para usar dois ou mais telas, faça o seguinte:

1. Clique com botão direito a área de trabalho remota que você deseja habilitar várias telas para e, em seguida, clique em **Editar**.
2. Habilite **usar todos os monitores** e **tela inteira**.

### Há suporte para som bidirecional
Som upstream (do cliente para o servidor, para microfones) não é compatível com o cliente de área de trabalho remota.

### O que posso fazer se não reproduzir o som?
Saia da sessão (não apenas desconectar, saia completamente) e entrar novamente.

## Cliente de MAC - perguntas sobre hardware
### Há suporte para resolução de retina?
Sim, o cliente da área de trabalho remoto oferece suporte a resolução de retina.

### Como habilitar o mouse secundário?
Para fazer uso do botão direito do mouse dentro de uma sessão aberta, você terá três opções:

- Mouse USB de botão de dois PC padrão
- Apple mágica Mouse: Para habilitar o botão direito do mouse, clique em **Preferências de sistema** no encaixe, clique do **Mouse**e, em seguida, habilitar **Clique secundário**.
- Apple mágica Trackpad ou MacBook Trackpad: para habilitar o botão direito do mouse, clique em **Preferências do sistema** no encaixe, clique do **Mouse**e habilitar, em seguida, **clique em secundário**.

### Há suporte para AirPrint?
Não, o cliente de área de trabalho remota não dá suporte a AirPrint. (Isso é verdadeiro para os clientes Mac e iOS).

### Por caracteres incorretos que aparecem na sessão?
Se você estiver usando um teclado internacional, você pode encontrar um problema em que os caracteres que aparecem na sessão corresponder os caracteres digitados no teclado Mac.

Isso pode ocorrer nas seguintes situações:

- Você está usando um teclado que não reconhece a sessão remota. Quando a área de trabalho remota não reconhece o teclado, ele assume como padrão o idioma usado pela última vez com o computador remoto.
- Você está se conectando a uma sessão desconectada anteriormente em um computador remoto e que o computador remoto usa um idioma do teclado diferentes atualmente está tentando usar.

Você pode corrigir esse problema definindo manualmente o idioma do teclado para a sessão remota. Consulte as etapas na próxima seção.

### Como as configurações de idioma afetam teclados em uma sessão remota?
Existem muitos tipos de layouts de teclado do Mac. Algumas delas são layouts específicos do Mac ou layouts personalizados para o qual uma correspondência exata pode não estar disponível na versão do Windows que você está remotamente em. A sessão remota mapeia o teclado para a melhor correspondência do idioma do teclado disponível no computador remoto. 

Se o layout do teclado Mac é definido como a versão PC do teclado idioma (por exemplo, francês – PC) todas as suas chaves devem ser mapeadas corretamente e seu teclado deve funcionar apenas.

Se o layout do teclado Mac é definido para a versão Mac do teclado (por exemplo, francês) a sessão remota mapeará para a versão PC do idioma francês. Alguns dos atalhos de teclado Mac, que você está acostumado a usar em OSX não funcionarão na sessão remota do Windows.

Se o layout do teclado é definido como uma variação de um idioma (por exemplo, francês (Canadá)) e a sessão remota não pode mapear para essa variação exata, a sessão remota mapeará para o idioma mais próximo (por exemplo, francês). Alguns dos atalhos de teclado Mac, que você está acostumado a usar em OSX não funcionarão na sessão remota do Windows.

Se o layout do teclado está definido para um layout que a sessão remota não correspondência, sua sessão remota padrão será para lhe fornecer o idioma usado pela última vez nesse computador. Nesse caso, ou em casos em que você precisa alterar o idioma da sessão remota para corresponder ao teclado Mac, você pode definir manualmente o idioma do teclado na sessão remota para o idioma que é a correspondência mais próxima para aquele que você deseja usar da seguinte maneira.

Use as instruções a seguir para alterar o layout do teclado na sessão da área de trabalho remota:

**No Windows 10 ou Windows 8:**

1. Na sessão remota, abra região e idioma. Clique em **Iniciar gt _ Configurações gt _ hora e idioma**. Abrir **região e idioma**.
2. Adicione o idioma que você deseja usar. Em seguida, feche a janela de região e idioma.
3. Agora, a sessão remota, você verá a capacidade de alternar entre os idiomas. (No lado direito da sessão remota, perto do relógio). Clique no idioma que você deseja alternar (como o **inglês**).

Convém feche e reinicie o aplicativo que você estiver usando atualmente para que as alterações de teclado entre em vigor.


## Erros específicos

### Por que recebo um erro de "Privilégios insuficientes"?
Você não tem permissão para acessar a sessão que você deseja se conectar. A causa mais provável é que você está tentando se conectar a uma sessão do administrador. Somente administradores têm permissão para se conectar ao console. Verifique se o comutador do console está desativada nas configurações avançadas de área de trabalho remota. Se isso não é a origem do problema, contate o administrador do sistema para obter assistência.

### Por que o cliente dizer que não há nenhuma CAL?
Quando um cliente da área de trabalho remoto se conecta a um servidor de área de trabalho remota, o servidor emite uma remoto Desktop serviços cliente acesso licença (RDS CALs) armazenados pelo cliente. Sempre que o cliente se conecta novamente, ele usará a RDS CAL e o servidor não emitirá outra licença. O servidor emitirá outra licença se a RDS CAL no dispositivo está ausente ou corrompido. Quando o número máximo de dispositivos licenciados for atingido, o servidor não emitirá nova RDS CALs. Contate o administrador de rede para obter assistência.

### Por que recebi um erro "Acesso negado"?
O erro "Acesso negado" é gerado, o Gateway de área de trabalho remota e o resultado de credenciais incorretas durante a tentativa de conexão. Verifique se seu nome de usuário e senha. Se a conexão funcionou antes e o erro ocorreu recentemente, você possivelmente alterado sua senha de conta de usuário do Windows e ainda não tiver atualizado ele ainda nas configurações da área de trabalho remotas.

### O que faz "Erro RPC 23014" ou significa "Erro 0x59e6"?
Em caso de um **erro RPC 23014** ou **erro 0x59E6 tente novamente após esperar alguns minutos**, o servidor de Gateway de área de trabalho remota atingiu o número máximo de conexões ativas. Dependendo da versão do Windows em execução no Gateway RD o número máximo de conexões difere: O Windows Server 2008 R2 Standard implementação limita o número de conexões para 250. A implementação do Windows Server 2008 R2 Foundation limita o número de conexões para 50. Todas as outras implementações do Windows permitem que um número ilimitado de conexões.

### O que significa o erro "Falha ao analisar o desafio NTLM"?
Esse erro é causado por um erro de configuração no computador remoto. Verifique se a configuração de nível de segurança RDP no computador remoto é definida como "Compatível com o cliente". (Contate o administrador do sistema se precisar de ajuda para fazer isso.)

### O que faz "TS_RAP você não tem permissão para se conectar a determinado host" significa?
Esse erro ocorre quando uma diretiva de autorização de recursos no servidor de gateway para seu nome de usuário de se conectar ao computador remoto. Isso pode acontecer nos seguintes casos:

- O nome do computador remoto é o mesmo que o nome do gateway. Em seguida, quando você tenta se conectar ao computador remoto, a conexão vai para o gateway em vez disso, que você provavelmente não tem permissão para acessar. Se você precisar se conectar ao gateway, não use o nome de gateway externo como nome de computador. Em vez disso, use "localhost" ou o endereço IP (127.0.0.1) ou o nome do servidor interno.
- Sua conta de usuário não é um membro do grupo de usuários para acesso remoto.