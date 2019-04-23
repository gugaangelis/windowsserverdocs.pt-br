---
title: rdpsign
description: Saiba como assinar digitalmente um arquivo RDP.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 648b179f5b2feb8a7585c815aee47804e3bf1532
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882267"
---
# <a name="rdpsign"></a>rdpsign

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você possa assinar digitalmente um arquivo do protocolo RDP (. rdp).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/sha1 \<hash>|Especifica a impressão digital, que é o hash do certificado de autenticação que está incluído no repositório de certificados de Secure Hash Algorithm 1 (SHA1).|
|/q|Modo silencioso. Nenhuma saída quando o comando for bem-sucedido e a saída mínima se o comando falhar.|
|/v|modo detalhado. Exibe todos os avisos, mensagens e status.|
|/l|Testa os resultados de assinatura e de saída sem, na verdade, substituindo qualquer um dos arquivos de entrada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   A impressão digital SHA1 deve representar um publicador de arquivo. rdp confiáveis. Para obter a impressão digital do certificado, abra o snap-in de certificados, duas vezes no certificado que você deseja usar (no repositório de certificados do computador local ou em seu repositório de certificados pessoais), clique o **detalhes** guia e, em seguida, o **campo** , clique em **impressão digital**.

    > [!NOTE]
    > Quando você copiar a impressão digital para uso com a ferramenta rdpsign.exe, você deve remover espaços.

-   Você deve especificar o arquivo. rdp (ou arquivos) para entrar usando o nome de arquivo completo. Caracteres curinga não são aceitas.
-   Os arquivos de saída com sinal substituirá os arquivos de entrada.
-   Se qualquer um dos arquivos. rdp não podem ser lidos ou gravados no, a ferramenta continuará para o próximo arquivo se vários arquivos forem especificados.

## <a name="BKMK_examples"></a>Exemplos
-   Para assinar um arquivo. rdp que é denominado File1.rdp, navegue até a pasta onde você salvou o arquivo. rdp e, em seguida, digite o seguinte:
    ```
    rdpsign /sha1 hash file1.rdp
    ```
    > [!NOTE]
    > O *hash* valor representa o SHA1 impressão digital do certificado, sem espaços.
-   Para testar se a assinatura digital terá êxito sem, na verdade, o arquivo de assinatura para um arquivo. rdp, digite o seguinte:
    ```
    rdpsign /sha1 hash /l file1.rdp
    ```
-   Para assinar vários arquivos. rdp, separe os nomes de arquivos usando espaços. Por exemplo, para assinar vários arquivos. rdp que são nomeados File1.rdp, File2.rdp e File3.rdp, digite o seguinte:
    ```
    rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
    ```
## <a name="see-also"></a>Consulte também
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
