---
title: Área de trabalho remota - comparar os aplicativos cliente
description: Saiba como se comparam os diferentes aplicativos de área de trabalho remota quando se trata de funções e recursos com suporte.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0e001b590f524711185e3dd70db3bc52a9b8d9af
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447118"
---
# <a name="compare-the-client-apps"></a>Compare os aplicativos cliente

>Aplica-se a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Muitas vezes, serão solicitados que como os diferentes aplicativos de cliente de área de trabalho remota se comparam uns aos outros. Todos eles fazem a mesma coisa? Aqui estão as respostas a essas perguntas.

## <a name="redirection-support"></a>Suporte a redirecionamento

As tabelas a seguir comparam o suporte para o dispositivo e outros redirecionamentos no aplicativo de Conexão de área de trabalho remota, aplicativo Universal, aplicativo Android, aplicativo iOS, macOS cliente de aplicativo e a web. Essas tabelas abordam os redirecionamentos que você pode acessar uma vez em uma sessão remota. 

Se você remoto em sua área de trabalho pessoal, há redirecionamentos adicionais que você pode configurar o **configurações adicionais** para a sessão. Se seus aplicativos ou a área de trabalho remota são gerenciados pela sua organização, o administrador pode habilitar ou desabilitar o redirecionamentos por meio das configurações de diretiva de grupo.

### <a name="input-redirection"></a>Redirecionamento de entrada

| Redirecionamento | Área de Trabalho Remota<br> Conexão | Universal | Android | iOS | macOS |          cliente Web           |
|-------------|-------------------------------|-----------|---------|-----|-------|-------------------------------|
|  Teclado   |               X               |     X     |    X    |  X  |   X   |               X               |
|    Mouse    |               X               |     X     |    X    | X\* |   X   |               X               |
|    Touch    |               X               |     X     |    X    |  X  |       | X (Edge e IE sem suporte) |
|    Outro    |              Caneta              |           |         |     |       |                               |

* Exibir o [lista de dispositivos de entrada com suporte para o cliente de versão Beta do iOS de área de trabalho remota](remote-desktop-ios.md#supported-input-devices).

### <a name="port-redirection"></a>Redirecionamento de porta   

| Redirecionamento | Área de Trabalho Remota <br>Conexão | Universal | Android | iOS | macOS | cliente Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Porta serial | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Quando você habilita o redirecionamento de porta USB, qualquer dispositivo USB conectado à porta USB é reconhecido automaticamente na sessão remota.

### <a name="other-redirection-devices-etc"></a>Outros redirecionamentos (dispositivos, etc.)



| Redirecionamento         | Conexão de Área de Trabalho Remota | Universal   | Android | iOS         | macOS                                    | cliente Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Câmeras             | X                         |             |         |             |                                          |               |
| Área de Transferência           | X                         | texto, imagem | texto    | texto, imagem | X                                        | texto          |
| Local de armazenamento/unidade | X                         |             | X       |             | x                                        |               |
| Location            | X                         |             |         |             |                                          |               |
| Microfones         | X                         |X            |         |             | X                                        |               |
| Impressoras            | X                         |             |         |             | X CUPS (somente)                            | Impressão PDF     |
| Scanners            | X                         |             |         |             |                                          |               |
| Cartões inteligentes         | X                         |             |         |             | X (autenticação do Windows não tem suportada) |               |
| Alto-falantes            | X                         | X           | X       | X           | X                                        | X (exceto o IE) |

* Para redirecionamento de impressora – o aplicativo macOS dá suporte ao driver de impressora fotocompositora Publisher por padrão. Eles não dá suporte ao redirecionamento de drivers de impressora nativo.
