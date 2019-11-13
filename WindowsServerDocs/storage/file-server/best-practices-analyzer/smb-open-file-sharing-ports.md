---
title: 'SMB: portas de compartilhamento de arquivo e impressora devem ser abertas'
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 22cd926fdb873538631a6f6850157dceb5a020d7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385835"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB: portas de compartilhamento de arquivo e impressora devem ser abertas


Atualizado: 2 de fevereiro de 2011

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, Windows Server 2008 R2

*Este tópico destina-se a resolver um problema específico identificado por uma verificação de analisador de práticas recomendadas. Você deve aplicar as informações neste tópico somente a computadores que tiveram os serviços de arquivo Analisador de Práticas Recomendadas executados neles e estão enfrentando o problema resolvido por este tópico. Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a).


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Sistema Operacional</strong></p></td>
<td><p>Windows Server</p></td>
</tr>
<tr class="even">
<td><p><strong>Produto/recurso</strong></p></td>
<td><p>Serviços de arquivo</p></td>
</tr>
<tr class="odd">
<td><p><strong>Severity</strong></p></td>
<td><p>Erro</p></td>
</tr>
<tr class="even">
<td><p><strong>Categoria</strong></p></td>
<td><p>Configuração</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>Problema

> *As portas de firewall necessárias para o compartilhamento de arquivos e impressoras não estão abertas (portas 445 e 139).*

## <a name="impact"></a>Impacto

> *Os computadores não poderão acessar pastas compartilhadas e outros serviços de rede baseados em protocolo SMB neste servidor.*

## <a name="resolution"></a>Resolução

> *Habilite o compartilhamento de arquivos e impressoras para se comunicar por meio do firewall do computador.*

A associação ao grupo **Administradores** , ou equivalente, é o mínimo necessário para concluir esse procedimento.

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>Para abrir as portas de firewall para habilitar o compartilhamento de arquivos e impressoras

1.  Abra o painel de controle, clique em **sistema e segurança**e, em seguida, clique em **Firewall do Windows**.

2.  No painel esquerdo, clique em **Configurações avançadas**e, na árvore de console, clique em **regras de entrada**.

3.  Em **regras de entrada**, localize o arquivo de regras **e compartilhamento de impressora (NB-sessão-in)** e **compartilhamento de arquivos e impressoras (SMB-in)** .

4.  Para cada regra, clique com o botão direito do mouse na regra e clique em **Habilitar Regra**.

## <a name="additional-references"></a>Referências adicionais

[Noções básicas sobre pastas compartilhadas e o Firewall do Windows](https://technet.microsoft.com/library/cc731402.aspx)(https://technet.microsoft.com/library/cc731402.aspx)

