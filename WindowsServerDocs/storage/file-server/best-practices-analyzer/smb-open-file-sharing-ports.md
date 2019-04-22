---
Title: 'SMB: Arquivo e impressora, portas de compartilhamento devem estar abertas'
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: b6ad1f1f8573fc380e999e5ec2091cea8ebb8aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820357"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB: Arquivo e impressora, portas de compartilhamento devem estar abertas


Atualizado: 2 de fevereiro de 2011

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012, Windows Server 2008 R2

*Este tópico destina-se a um problema específico identificado por um exame do analisador de práticas recomendadas. Você deve aplicar as informações neste tópico somente a computadores que estão enfrentando o problema abordado por este tópico e que tem tido o analisador de práticas recomendadas do arquivo serviços executados em relação a elas. Para obter mais informações sobre as práticas recomendadas e varreduras, consulte* [Best Practices Analyzer](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a).


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
<td><p><strong>Recurso do produto</strong></p></td>
<td><p>Serviços de arquivo</p></td>
</tr>
<tr class="odd">
<td><p><strong>Severidade</strong></p></td>
<td><p>Erro</p></td>
</tr>
<tr class="even">
<td><p><strong>categoria</strong></p></td>
<td><p>Configuração</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>Problema

> *As portas de firewall são necessárias para o compartilhamento de arquivo e impressora não abrir (portas 445 e 139).*

## <a name="impact"></a>Impacto

> *Computadores não poderão acessar pastas compartilhadas e outros serviços de rede com base em SMB Server Message Block neste servidor.*

## <a name="resolution"></a>Resolução

> *Habilite o compartilhamento de arquivo e impressora para se comunicar por meio do firewall do computador.*

A associação ao grupo **Administradores** , ou equivalente, é o mínimo necessário para concluir esse procedimento.

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>Para abrir as portas de firewall para habilitar o compartilhamento de arquivo e impressora

1.  Abra o painel de controle, clique em **sistema e segurança**e, em seguida, clique em **Firewall do Windows**.

2.  No painel esquerdo, clique em **configurações avançadas**e na árvore de console, clique em **regras de entrada**.

3.  Sob **regras de entrada**, localize as regras **compartilhamento de arquivo e impressora (NB-Sessão-entrada)** e **compartilhamento de arquivo e impressora (SMB-entrada)**.

4.  Para cada regra, clique com botão direito na regra e, em seguida, clique em **Habilitar regra**.

## <a name="additional-references"></a>Referências adicionais

[Noções básicas sobre pastas compartilhadas e o Firewall do Windows](http://technet.microsoft.com/en-us/library/cc731402.aspx)(http://technet.microsoft.com/en-us/library/cc731402.aspx)

