---
title: Mover discos para outro computador
description: Este artigo descreve como mover discos para outro computador
ms.date: 10/12/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1527dbd567eb72e407023ecdcfade04856a3127a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971183"
---
# <a name="move-disks-to-another-computer"></a>Mover discos para outro computador

> **Aplica-se a:** Windows 10, Windows 8.1, Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção descreve as etapas a serem seguidas, bem como as considerações associadas, a fim de mover discos para outro computador. É recomendado imprimir este procedimento ou anotar as etapas antes de tentar mover discos de um computador para outro.

> [!NOTE]
> Para concluir estas etapas, no mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores**.

## <a name="verify-volume-health"></a>Verificar a integridade do volume

Use o Gerenciamento de Disco para verificar se o status dos volumes nos discos é **Íntegro**. Se o status não for **Íntegro**, repare os volumes antes de mover os discos.

Para verificar o status do volume, no menu **Exibir**, verifique a coluna **Status** na exibição **Lista de volumes** ou em **Tamanho do volume** e nas informações do sistema de arquivos na **Exibição gráfica**.

## <a name="uninstall-the-disks"></a>Desinstalar os discos

Desinstale os discos que você quer mover usando o Gerenciador de Dispositivos.

**Para desinstalar os discos**

1.  Abra o Gerenciador de Dispositivos no Gerenciamento do Computador.

2.  Na lista de dispositivos, clique duas vezes em **Unidades de disco**.

3.  Clique com o botão direito do mouse nos discos que você quer desinstalar e clique em **Desinstalar**.

4.  Na caixa de diálogo **Confirmar remoção de dispositivo**, clique em **OK**.

## <a name="remove-dynamic-disks"></a>Remover discos dinâmicos

1. Se os discos que você deseja mover são dinâmicos, no Gerenciamento de Disco, clique com botão direito do mouse nos discos que você quer mover e, em seguida, clique em **Remover disco**.

2. Depois de ter removido os discos dinâmicos, ou se você estiver movendo discos básicos, agora é possível desconectá-los fisicamente. Se os discos forem externos, agora é possível desconectá-los do computador. Se os discos forem internos, desligue o computador e, em seguida, remova-os fisicamente.

## <a name="install-disks-in-the-new-computer"></a>Instalar discos no novo computador

1. Se os discos forem externos, conecte-os ao computador. Se os discos forem internos, verifique se o computador está desligado e, em seguida, instale os discos no computador.

2. Inicie o computador com os discos que você moveu e siga as instruções na caixa de diálogo Novo hardware encontrado.

## <a name="detect-new-disks"></a>Detectar novos discos

1. No novo computador, abra o Gerenciamento de Disco.
2. Clique em **Ação** e, em seguida, clique em **Examinar discos novamente**.
3. Clique com o botão direito do mouse em qualquer disco marcado como **Externo**.
4. Clique em **Importar discos externos** e siga as instruções na tela.

## <a name="additional-considerations"></a>Considerações adicionais

-   Quando são movidos para outro computador, os volumes básicos recebem a próxima letra da unidade disponível nesse computador.
-   Os volumes dinâmicos mantêm a letra da unidade que tinham no computador anterior. Se um volume dinâmico não tinha uma letra da unidade no computador anterior, ele não receberá uma letra da unidade quando for movido para outro computador. Se a letra da unidade já estiver em uso no computador no qual o volume foi movido, ele receberá a próxima letra da unidade disponível.

-   Se um administrador tiver usado o comando **mountvol /n** ou **diskpart automount** para impedir que novos volumes sejam adicionados ao sistema, os volumes movidos de outro computador não serão montados e não receberão uma letra da unidade. Para usar o volume, é necessário montar manualmente o volume e atribuir uma letra da unidade usando o Gerenciamento de Disco ou os comandos **DiskPart** e **mountvol**.

-   Se estiver movendo volumes estendidos, distribuídos, espelhados ou RAID-5, é altamente recomendável que você mova todos os discos que contém o volume em conjunto. Caso contrário, os volumes nos discos não poderão ficar online e não estarão acessíveis, exceto para excluí-los.

-   É possível mover vários discos de computadores diferentes para um computador ao instalar os discos, abrir o Gerenciamento de Disco, clicar com o botão direito em qualquer um dos novos discos e, em seguida, clicar em **Importar discos externos**. Ao importar vários discos de computadores diferentes, sempre importe todos os discos de um computador por vez. Por exemplo, se você quiser mover discos de dois computadores, importe todos os discos do primeiro computador e, em seguida, importe todos os discos do segundo computador.

-   O Gerenciamento de Disco descreve a condição dos volumes nos discos antes de serem importados. Examine essas informações com cuidado. Em caso de problemas, essas informações indicam o que acontecerá com cada volume nesses discos quando eles forem importados.

-   Se você mover um disco GUID (Tabela de Partição GUID) contendo o sistema operacional Windows para um computador baseado em x64 ou x86, será possível acessar os dados, mas não inicializar a partir desse sistema operacional.