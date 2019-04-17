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
ms.sourcegitcommit: d5f10c0c98a9976a86be9f4fa8866650c7fcfc1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/12/2019
ms.locfileid: "9065853"
---
# Acessar o cliente da web de Área de Trabalho Remota

O cliente da web de área de trabalho remota permite que você use um navegador da web compatível para acessar recursos remotos da sua organização (aplicativos e áreas de trabalho) publicados pelo seu administrador. Você poderá interagir com os aplicativos remotos e áreas de trabalho como você faria com um computador local, não importa onde você está, sem a necessidade de alternar para um computador desktop diferente. Depois que o administrador configura seus recursos remotos, tudo que você precisa são seu domínio, nome de usuário, senha, a URL do seu administrador enviado a você e um navegador da web com suporte, e você estiver pronto.

>[!NOTE]
>Curioso sobre as novas versões do cliente de web? Confira [que há de novo para o cliente da web de área de trabalho remota?](web-client-whatsnew.md)

## O que você precisará usar o cliente da web

* Para o cliente da web, você precisará de um computador que executa o Windows, macOS, ChromeOS ou Linux. Não há suporte para dispositivos móveis no momento.
* Um navegador moderno como o Microsoft Edge, Internet Explorer 11, Google Chrome, Safari ou Mozilla Firefox (v55.0 e posterior).
* A URL do seu administrador enviou.

>[!NOTE]
>A versão do cliente da web do Internet Explorer não tem áudio neste momento.
>Safari pode exibir uma tela cinza se o navegador for redimensionado ou entra em tela inteira várias vezes.

## Comece a usar o cliente de área de trabalho remota

Para entrar no cliente, vá para a URL que o administrador enviado a você. Na página de entrada, digite seu nome de usuário e domínio no formato ```DOMAIN\username```, insira sua senha e, em seguida, selecione **entrar**.

>[!NOTE]
>Ao entrar para o cliente da web, você concorda que seu computador esteja em conformidade com a política de segurança da sua organização.

Depois que você entra, o cliente levará você para a guia **Todos os recursos** , que contém todos os itens publicados em um ou mais grupos recolhíveis, como o grupo "Recursos de trabalho". Você verá vários ícones que representam os aplicativos, áreas de trabalho ou pastas que contêm mais aplicativos ou áreas de trabalho que o administrador disponibilizou para o grupo de trabalho. Você pode voltar para essa guia a qualquer momento para iniciar recursos adicionais.

Para começar a usar um aplicativo ou a área de trabalho, selecione o item que você deseja usar, insira o mesmo nome de usuário e senha que você usou para se conectar ao cliente a web se solicitado e, em seguida, selecione **Enviar**. Você também pode ser mostrada uma caixa de diálogo de consentimento para acessar recursos locais, como área de transferência e impressora. Você pode optar por não redirecionar qualquer um dos seguintes, ou selecione **Permitir** para usar as configurações padrão. Aguarde até que o cliente estabelecer a conexão da web e, em seguida, começar a usar o recurso, como você faria normalmente.

Quando você terminar, você pode encerrar a sessão selecionando o botão **Sair** na barra de ferramentas na parte superior da tela ou fechar a janela do navegador.

## Imprimir a partir do cliente da web de área de trabalho remota

Siga estas etapas para imprimir do cliente da web:

1. Inicie o processo de impressão, como você faria normalmente para o aplicativo que você deseja imprimir a partir do.
2. Quando solicitado a escolher uma impressora, selecione **Impressora Virtual de área de trabalho remota**.
3. Depois de escolher suas preferências, selecione **Imprimir**.
4. Seu navegador irá gerar um arquivo PDF de seu trabalho de impressão.
5. Você pode optar por abrir o PDF e imprimir seu conteúdo para a impressora local ou salve-o em seu computador para uso posterior.

## Copie e cole o cliente da web de área de trabalho remota

O cliente web atualmente dá suporte a copiar e colar somente texto. Arquivos não podem ser copiados ou colados de e para o cliente da web. Além disso, você pode usar apenas **Ctrl + C** e **Ctrl + V** para copiar e colar texto.

## Obter ajuda com o cliente da web

Se você encontrou um problema que não possa ser resolvido as informações neste artigo, você pode obter ajuda com o cliente web enviando um e-mail para o endereço na página sobre do cliente da web.