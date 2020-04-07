---
title: Área de Trabalho Remota – Comparar os clientes
description: Saiba como se comparam os diferentes clientes da Área de Trabalho Remota quando se trata de recursos e funções compatíveis.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 03/28/2020
ms.localizationpriority: medium
ms.openlocfilehash: 11c91ac951db27915d9313f7f98e5e2cfc56b726
ms.sourcegitcommit: 78c00944b6990702d28bdcc4a9215927ca901bfb
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80440375"
---
# <a name="compare-the-clients"></a>Comparar os clientes

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Geralmente recebemos a pergunta de como os diferentes clientes da Área de Trabalho Remota se comparam uns aos outros. Todos eles fazem a mesma coisa? Aqui estão as respostas a essas perguntas.

## <a name="redirection-support"></a>Suporte a redirecionamento

As tabelas abaixo comparam o suporte para dispositivos e outros redirecionamentos em diferentes clientes. Essas tabelas englobam os redirecionamentos que você pode acessar uma vez em uma sessão remota.

Se você acessar sua área de trabalho pessoal remotamente, haverá vários redirecionamentos adicionais que você poderá configurar nas **Configurações Adicionais** para a sessão. Se sua organização gerencia seus aplicativos ou sua Área de Trabalho Remota, o administrador pode habilitar ou desabilitar os redirecionamentos por meio das configurações da Política de Grupo ou das propriedades de RDP.

### <a name="input-redirection"></a>Redirecionamento de entrada

| Redirecionamento | Windows Inbox</br>(MSTSC) | Windows Desktop</br>(MSRDC) | Windows Store | Android | iOS | macOS | Cliente Web    |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|---------------|
| Keyboard    | X                         | X                           | X             | X       | X   | X     | X             |
| Mouse       | X                         | X                           | X             | X       | X\* | X     | X             |
| Touch       | X                         | X                           | X             | X       | X   |       | X (exceto o IE) |
| Caneta         | X                         | X                           |               |         |     |       |               |

*Veja a [lista de dispositivos de entrada com suporte para o cliente iOS da Área de Trabalho Remota](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redirecionamento de porta

| Redirecionamento | Windows Inbox</br>(MSTSC) | Windows Desktop</br>(MSRDC) | Windows Store | Android | iOS | macOS | Cliente Web |
|-------------|---------------------------|-----------------------------|---------------|---------|-----|-------|------------|
| Porta serial | X                         | X                           |               |         |     |       |            |
| USB         | X                         | X                           |               |         |     |       |            |

Quando você habilita o redirecionamento de porta USB, quaisquer dispositivos USB conectados à porta USB são reconhecidos automaticamente na sessão remota.

### <a name="other-redirection-devices-etc"></a>Outros redirecionamentos (dispositivos, etc.)

| Redirecionamento         | Windows Inbox</br>(MSTSC) | Windows Desktop</br>(MSRDC) | Windows Store | Android | iOS         | macOS                           | Cliente Web    |
|---------------------|---------------------------|-----------------------------|---------------|---------|-------------|---------------------------------|---------------|
| Câmeras             | X                         | X                           |               |         |             | X                               |               |
| Área de Transferência           | X                         | X                           | X             | texto    | texto, imagem | X                               | texto          |
| Unidade local/armazenamento | X                         | X                           |               | X       |             | X                               |               |
| Local            | X                         | X                           |               |         |             |                                 |               |
| Microfones         | X                         | X                           | X             |         |             | X                               |               |
| Impressoras            | X                         | X                           |               |         |             | X (somente CUPS)                   | Impressão PDF     |
| Scanners            | X                         | X                           |               |         |             |                                 |               |
| Smart Cards         | X                         | X                           |               |         |             | X (não há suporte para o logon do Windows) |               |
| Speakers            | X                         | X                           | X             | X       | X           | X                               | X (exceto o IE) |

*Para redirecionamento de impressora – o aplicativo macOS dá suporte ao driver de impressora Publisher Imagesetter por padrão. Eles não dão suporte ao redirecionamento de drivers de impressora nativos.
