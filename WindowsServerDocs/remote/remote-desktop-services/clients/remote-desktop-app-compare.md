---
title: Área de Trabalho Remota – comparar os aplicativos cliente
description: Saiba como se comparam os diferentes aplicativos de área de trabalho remota quando se trata de funções e recursos compatíveis.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 237bb79fae6460bc3b31fb1753e2d679c8d67512
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404164"
---
# <a name="compare-the-client-apps"></a>Comparar os aplicativos cliente

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Muitas vezes nos é perguntado como os diferentes aplicativos cliente de Área de Trabalho Remota se comparam uns aos outros. Todos eles fazem a mesma coisa? Aqui estão as respostas a essas perguntas.

## <a name="redirection-support"></a>Suporte a redirecionamento

As tabelas a seguir comparam o suporte para redirecionamento de dispositivo e outros redirecionamentos no aplicativo de Conexão de Área de Trabalho Remota, aplicativo Universal, aplicativo Android, aplicativo iOS, aplicativo macOS e cliente Web. Essas tabelas englobam os redirecionamentos que você pode acessar uma vez em uma sessão remota. 

Se você acessar sua área de trabalho pessoal remotamente, haverá vários redirecionamentos adicionais que você poderá configurar nas **Configurações Adicionais** para a sessão. Se os aplicativos ou Área de Trabalho Remota são gerenciados pela sua organização, o administrador pode habilitar ou desabilitar os redirecionamentos por meio das configurações de Política de Grupo.

### <a name="input-redirection"></a>Redirecionamento de entrada

| Redirecionamento | Área de Trabalho Remota<br> Conexão | Universal | Android | iOS | macOS |          Cliente Web           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  Teclado   |               X               |     X     |    X    |  X  |   X   |               X               |
|    Mouse    |               X               |     X     |    X    | X\* |   X   |               X               |
|    Touch    |               X               |     X     |    X    |  X  |       | X (Microsoft Edge e IE não compatíveis) |
|    Outro    |              Caneta              |           |         |     |       |                               |

*Exiba a [lista de dispositivos de entrada compatíveis para a versão Beta do cliente iOS da Área de Trabalho Remota](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redirecionamento de porta   

| Redirecionamento | Área de Trabalho Remota <br>Conexão | Universal | Android | iOS | macOS | Cliente Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Porta serial | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Quando você habilita o redirecionamento de porta USB, quaisquer dispositivos USB conectados à porta USB são reconhecidos automaticamente na sessão remota.

### <a name="other-redirection-devices-etc"></a>Outros redirecionamentos (dispositivos, etc.)



| Redirecionamento         | Conexão de Área de Trabalho Remota | Universal   | Android | iOS         | macOS                                    | Cliente Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Câmeras             | X                         |             |         |             |                                          |               |
| Área de Transferência           | X                         | texto, imagem | texto    | texto, imagem | X                                        | texto          |
| Unidade local/armazenamento | X                         |             | X       |             | x                                        |               |
| Location            | X                         |             |         |             |                                          |               |
| Microfones         | X                         |X            |         |             | X                                        |               |
| Impressoras            | X                         |             |         |             | X (somente CUPS)                            | Impressão PDF     |
| Scanners            | X                         |             |         |             |                                          |               |
| Cartões inteligentes         | X                         |             |         |             | X (autenticação do Windows não compatível) |               |
| Alto-falantes            | X                         | X           | X       | X           | X                                        | X (exceto o IE) |

*Para redirecionamento de impressora – o aplicativo macOS dá suporte ao driver de impressora Publisher Imagesetter por padrão. Eles não dão suporte ao redirecionamento de drivers de impressora nativos.
