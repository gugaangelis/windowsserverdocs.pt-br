---
title: Acessar o cliente da web de Área de Trabalho Remota
description: Descreve como entrar para o cliente da web de área de trabalho remota.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849367"
---
# <a name="access-the-remote-desktop-web-client"></a>Acessar o cliente da web de Área de Trabalho Remota

O cliente da web de área de trabalho remota permite que você use um navegador da web compatível para acessar recursos da sua organização remoto (aplicativos e desktops) publicados pelo seu administrador. Você poderá interagir com os aplicativos remotos e áreas de trabalho, como faria com um computador local, independentemente de onde você está, sem precisar alternar para um PC desktop diferente. Depois que o administrador configura seus recursos remotos, tudo o que você precisa são seu domínio, nome de usuário, senha, a URL do seu administrador enviado a você e um navegador da web com suporte, e você está pronto para começar.

>[!NOTE]
>Curioso sobre as novas versões do cliente da web? Fazer check-out [o que há de novo para o cliente da web de área de trabalho remota?](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>O que você precisará usar o cliente da web

* Para o cliente da web, você precisará de um computador que executa o Windows, macOS, ChromeOS ou Linux. Não há suporte para dispositivos móveis no momento.
* Um navegador moderno, como o Internet Explorer 11, Microsoft Edge, Google Chrome, Safari ou Mozilla Firefox (v55.0 e posterior).
* A URL do seu administrador enviou a você.

>[!NOTE]
>A versão do Internet Explorer do cliente da web não tem áudio neste momento.
>Safari pode exibir uma tela cinza se o navegador é redimensionado ou entra em tela inteira várias vezes.

## <a name="start-using-the-remote-desktop-client"></a>Começar a usar o cliente de área de trabalho remota

Para entrar cliente, vá para a URL que o administrador enviou. Na página de entrada, insira seu nome de usuário e domínio no formato ```DOMAIN\username```, insira sua senha e, em seguida, selecione **entrar**.

>[!NOTE]
>Ao entrar no cliente web, você concorda que seu computador esteja em conformidade com a política de segurança da sua organização.

Depois de entrar, o cliente levará você até a **todos os recursos** guia, que contém todos os itens publicados para você sob um ou mais grupos recolhíveis, como o grupo de "recursos de trabalho". Você verá vários ícones que representam os aplicativos, áreas de trabalho ou pastas que contêm mais aplicativos ou áreas de trabalho que o administrador tornou disponível para o grupo de trabalho. Você pode voltar a esta guia a qualquer momento para iniciar os recursos adicionais.

Para começar a usar um aplicativo ou área de trabalho, selecione o item que você deseja usar, insira o mesmo nome de usuário e senha que você usou para entrar no cliente web se for solicitado e, em seguida, selecione **enviar**. Você também pode ser mostrada uma caixa de diálogo de consentimento para acessar recursos locais, como área de transferência e a impressora. Você pode optar por não redirecionar qualquer uma delas, ou selecione **permitir** para usar as configurações padrão. Aguarde até que o cliente web estabelecer a conexão e, em seguida, começar a usar o recurso, como você faria normalmente.

Quando tiver terminado, você pode encerrar sua sessão selecionando a **sair** botão na barra de ferramentas na parte superior da tela ou fechando a janela do navegador.

## <a name="printing-from-the-remote-desktop-web-client"></a>Impressão do cliente de web de área de trabalho remota

Siga estas etapas para imprimir a partir do cliente da web:

1. Inicie o processo de impressão como faria normalmente para o aplicativo que você deseja imprimir.
2. Quando solicitado a escolher uma impressora, selecione **impressora Virtual de área de trabalho remota**.
3. Depois de escolher suas preferências, selecione **impressão**.
4. O navegador irá gerar um arquivo PDF do seu trabalho de impressão.
5. Você pode optar por abrir o PDF e imprimir o conteúdo para a impressora local ou salvá-lo em seu computador para uso posterior.

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>Copiar e colar de cliente da web de área de trabalho remota

O cliente web atualmente suporta copiar e colar somente texto. Arquivos não podem ser copiados ou colados de e para o cliente web. Além disso, você só pode usar **Ctrl + C** e **Ctrl + V** para copiar e colar o texto.

## <a name="get-help-with-the-web-client"></a>Obtenha ajuda com o cliente da web

Se você tiver encontrado um problema que não pode ser resolvido com as informações neste artigo, você pode obter ajuda com o cliente web enviando o endereço na página de sobre do cliente web.