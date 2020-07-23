---
title: rdpsign
description: Artigo de referência para o comando rdpsign, que permite que você assine digitalmente um arquivo protocolo RDP (. RDP).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4a6fa8ce-3d32-49a5-b056-bcc1a23391f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5769e568d6cee062b354b4d8c7284808968a2b02
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86956308"
---
# <a name="rdpsign"></a>rdpsign

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você assine digitalmente um arquivo protocolo RDP (. RDP).

> [!NOTE]
> Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
rdpsign /sha1 <hash> [/q | /v |] [/l] <file_name.rdp>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /sha1`<hash>` | Especifica a impressão digital, que é o hash Secure Hash Algorithm 1 (SHA1) do certificado de autenticação que está incluído no repositório de certificados. Usado no Windows Server 2012 R2 e mais antigo. |
| /sha256`<hash>` | Especifica a impressão digital, que é o hash do algoritmo de hash seguro 256 (SHA256) do certificado de autenticação que está incluído no repositório de certificados. Substitui/SHA1 no Windows Server 2016 e mais recente. |
| /q | Modo silencioso. Nenhuma saída quando o comando for bem sucedido e a saída mínima se o comando falhar. |
| /v | modo detalhado. Exibe todos os avisos, mensagens e status. |
| /l | Testa os resultados de assinatura e saída sem realmente substituir nenhum dos arquivos de entrada. |
| `<file_name.rdp>` | O nome do arquivo. rdp. Você deve especificar o (ou arquivos) arquivo. rdp para assinar usando o nome completo do arquivo. Caracteres curinga não são aceitos. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- A impressão digital do certificado SHA1 ou SHA256 deve representar um Publicador de arquivo. rdp confiável. Para obter a impressão digital do certificado, abra o snap-in **certificados** , clique duas vezes no certificado que você deseja usar (no repositório de certificados do computador local ou em seu armazenamento de certificados pessoais), clique na guia **detalhes** e, na lista **campo** , clique em **impressão digital**.

    > [!NOTE]
    > Ao copiar a impressão digital para uso com a ferramenta de rdpsign.exe, você deve remover os espaços.

- Os arquivos de saída assinados substituem os arquivos de entrada.

- Se vários arquivos forem especificados, e se qualquer um dos arquivos. rdp não puder ser lido ou gravado, a ferramenta continuará no próximo arquivo.

### <a name="examples"></a>Exemplos

Para assinar um arquivo. rdp chamado *arquivo1. rdp*, navegue até a pasta em que você salvou o arquivo. RDP e digite:

```
rdpsign /sha1 hash file1.rdp
```

> [!NOTE]
> O valor de *hash* representa a impressão digital do certificado SHA1, sem espaços.

Para testar se a assinatura digital terá sucesso para um arquivo. rdp sem realmente assinar o arquivo, digite:

```
rdpsign /sha1 hash /l file1.rdp
```

Para assinar vários arquivos. rdp nomeados, *arquivo1. rdp*, *file2. rdp*e *arquivo3. rdp*, digite (incluindo os espaços entre os nomes de arquivo):

```
rdpsign /sha1 hash file1.rdp file2.rdp file3.rdp
```

## <a name="see-also"></a>Consulte Também

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
