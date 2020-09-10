---
title: Opções de área de trabalho remota
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 51076946-ea9b-4ac7-9a6e-d6023816b97d
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: f4603adb7da1df0853b4c816254241d21b969fc5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89625936"
---
# <a name="remote-desktop-options"></a>Opções de área de trabalho remota

## <a name="connection-speed"></a>Velocidade de conexão
 A velocidade da conexão com um computador da rede usando o Acesso via Web remoto determina as opções de área de trabalho que estão disponíveis para você no computador host. A tabela a seguir informa quais opções de área de trabalho estão disponíveis para velocidade que estão se conectando ao computador remoto por meio do Acesso via Web remoto.

| Opção de área de trabalho | Modem lento (28,8 Kbps) | Modem rápido (56 Kbps) (padrão) | Banda larga (128 Kbps - 1,5 Mbps) | Rede de área local (1,5 Mbps ou superior) |
|--|--|--|--|--|
| Suavização de fonte | Não | Não | Não | Sim |
| Composição de área de trabalho | Não | Não | Sim | Sim |
| Mostrar conteúdo da janela ao arrastar | Não | Não | Sim | Sim |
| Animação de menus e janelas | Não | Não | Sim | Sim |
| Temas | Não | Sim | Sim | Sim |
| Bitmaps em cache | Sim | Sim | Sim | Sim |

## <a name="screen-size"></a>Tamanho da tela
 Essa opção determina o tamanho da janela que é aberto no computador local quando você se conecta a um computador remoto por meio do site de Acesso via Web remoto. O tamanho da janela é expresso em pixels.

> [!NOTE]
>  Quando você se conectar ao servidor, o Painel será aberto. O tamanho do Painel padrão é 1024 x 741 e dimensionável.

-   Tela inteira (vários monitores)

-   1280 x 720

-   1024 x 768

-     800 x 600

-   640 x 480

## <a name="enable-the-remote-computer-to-print-to-my-local-printer"></a>Permitir que o computador remoto imprima na impressora local
 Habilitado por padrão. Esta opção permite imprimir na impressora que está conectada ao computador local do computador remoto.

## <a name="play-sounds-from-the-remote-computer"></a>Tocar sons do computador remoto
 Habilitado por padrão. Esta opção permite tocar sons, como sons do sistema, no computador local do computador remoto.

## <a name="enable-copy-and-paste-between-the-remote-computer-and-the-local-computer"></a>Habilitar copiar e colar entre o computador remoto e o computador local
 Não habilitado por padrão. Esta opção permite que você copie e cole os arquivos entre o computador remoto e o computador local na mesma forma como copiaria e colaria os arquivos de um local para outro no computador local.

## <a name="enable-the-remote-computer-to-access-drives-on-my-local-computer"></a>Permitir que o computador remoto acesse unidades no meu computador local
 Não habilitado por padrão. Esta opção permite que você acesse os arquivos e pastas nas unidades de disco rígido conectadas ao computador local do computador remoto.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar o Acesso via Web Remoto](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [Usar o Acesso via Web Remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)
