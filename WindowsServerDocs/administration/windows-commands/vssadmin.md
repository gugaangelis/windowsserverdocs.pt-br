---
title: Vssadmin
description: Uma visão geral dos comandos vssadmin.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2e6dc257b41d588236b78f94bf6ed606c9eaeaca
ms.sourcegitcommit: fc900eb19ac26c3d6bc2de179cc4b2c1e971043e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79038029"
---
# <a name="vssadmin"></a>Vssadmin

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Exibe os backups de cópia de sombra de volume atuais e todos os provedores e gravadores de cópia de sombra instalados. Selecione um nome de comando na tabela a seguir para exibir sua sintaxe de comando.

|{1&gt;Comando&lt;1}|Descrição|Availability
|---|---|---
|[Vssadmin Add ShadowStorage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788051(v%3dws.11))|Adiciona uma associação de armazenamento de cópias de sombra de volume.| Somente servidor
|[Vssadmin criar sombra](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788055(v%3dws.11))|Cria uma nova cópia de sombra de volume.| Somente servidor
|[Vssadmin excluir sombras](vssadmin-delete-shadows.md)|Exclui cópias de sombra de volume.| Cliente e Servidor
|[Vssadmin Delete ShadowStorage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc785461(v%3dws.11))|Exclui associações de armazenamento de cópias de sombra de volume.| Somente servidor
|[VSSadmin list providers](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788108(v%3dws.11))|Lista os provedores de cópias de sombra de volume registrados.| Cliente e Servidor
|[Vssadmin da lista de sombras](vssadmin-list-shadows.md)|Lista as cópias de sombra de volume existentes.| Cliente e Servidor
|[VSSadmin list ShadowStorage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788045(v%3dws.11))|Lista todas as associações de armazenamento de cópia de sombra no sistema.| Cliente e Servidor
|[Vssadmin listar volumes](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788064(v%3dws.11))|Lista os volumes que são elegíveis para cópias de sombra.| Cliente e Servidor
|[VSSadmin list writers](vssadmin-list-writers.md)|Lista todos os gravadores de cópia de sombra de volume assinados no sistema.| Cliente e Servidor
|[Vssadmin redimensionar ShadowStorage](vssadmin-resize-shadowstorage.md)|Redimensiona o tamanho máximo para uma associação de armazenamento de cópia de sombra.| Cliente e Servidor
