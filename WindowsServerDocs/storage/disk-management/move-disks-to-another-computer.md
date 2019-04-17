---
title: Mover discos para outro computador
description: Este artigo descreve como mover discos para outro computador
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6b235ce8e5b936940629d5977a17bbc729efbe82
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="move-disks-to-another-computer"></a>Mover discos para outro computador

> **Aplicável a:** Windows 10, Windows 8.1, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção descreve as etapas a serem seguidas, bem como as considerações associadas, a fim de mover discos para outro computador. É recomendado imprimir esse procedimento ou anotar as etapas antes de tentar mover discos de um computador para outro.

> [!NOTE]
> No mínimo, você deve ser um membro do grupo **Operadores de backup** ou **Administradores** para concluir estas etapas.

## <a name="verify-volume-health"></a>Verificar a integridade do volume

Use o Gerenciamento de Disco para garantir que o status dos volumes nos discos é **Íntegro**. Se o status não for **Íntegro**, repare os volumes antes de mover os discos.

Para verificar o status do volume no menu **Exibir**, verifique a coluna **Status** na exibição **Lista de volume** ou em **tamanho do volume** e nas informações do sistema de arquivos na **Exibição gráfica**.

## <a name="uninstall-the-disks"></a>Desinstalar os discos

Desinstale os discos que você deseja mover usando o Gerenciador de Dispositivos.

**Para desinstalar os discos**

1.  Abra o Gerenciador de Dispositivos no Gerenciamento do computador.

2.  Na lista de dispositivos, clique duas vezes em **Unidades de disco**.

3.  Clique com o botão direito do mouse nos discos que você deseja desinstalar e, em seguida, clique em **Desinstalar**.

4.  Na caixa de diálogo **Confirmar remoção de dispositivo**, clique em **OK**.

## <a name="remove-dynamic-disks"></a>Remover discos dinâmicos

1. Se os discos que você deseja mover são dinâmicos, no Gerenciamento de Disco, clique com botão direito do mouse nos discos que você deseja mover e, em seguida, clique em **Remover disco**.

2. Depois de ter removido discos dinâmicos ou se você estiver movendo discos básicos, agora você pode desconectá-los fisicamente. Se os discos forem externos, é possível desconectá-los do computador. Se os discos forem internos, desligue o computador e, em seguida, remova-os fisicamente.

## <a name="install-disks-in-the-new-computer"></a>Instale os discos no novo computador

1. Se os discos forem externos, conecte-os ao computador. Se os discos forem internos, verifique se o computador está desativado e, em seguida, instale os discos no computador.

2. Inicie o computador com os discos que você moveu e siga as instruções na caixa de diálogo Novo hardware encontrado.

## <a name="detect-new-disks"></a>Detectar novos discos

1. No novo computador, abra o Gerenciamento de Disco. 
2. Clique em **Ação** e, em seguida, clique em **Examinar discos novamente**.
3. Clique com o botão direito do mouse em qualquer disco marcado como **Externo**. 
4. Clique em **Importar discos externos** e siga as instruções na tela.

## <a name="additional-considerations"></a>Considerações adicionais

-   Quando são movidos para outro computador, os volumes básicos recebem a próxima letra de unidade disponível nesse computador. 
-   Os volumes dinâmicos mantêm a letra de unidade que tinham no computador anterior. Se um volume dinâmico não tinha uma letra de unidade no computador anterior, ele não receberá uma letra de unidade quando for movido para outro computador. Se a letra da unidade já estiver em uso no computador para o qual o volume é movido, ele recebe a próxima letra de unidade disponível.

-   Se um administrador tiver usado o comando **mountvol /n** ou **diskpart automount** para impedir que novos volumes sejam adicionados ao sistema, os volumes movidos de outro computador não são montados e não recebem uma letra de unidade. Para usar o volume, você deve montar manualmente o volume e atribuir uma letra de unidade usando o Gerenciamento de Disco ou os comandos **DiskPart** e **mountvol**.

-   Se estiver movendo volumes estendidos, distribuídos, espelhados ou RAID-5, é altamente recomendável que você mova todos os discos que contém o volume juntos. Caso contrário, os volumes nos discos não podem ficar online e não estarão acessíveis, exceto para excluí-los.

-   Você pode mover vários discos de computadores diferentes para um computador ao instalar os discos, abrir o Gerenciamento de Disco, clicar em qualquer um dos novos discos e, em seguida, clicar em **Importar discos externos**. Ao importar vários discos de computadores diferentes, sempre importe todos os discos de um computador por vez. Por exemplo, se você quiser mover discos de dois computadores, importe todos os discos do primeiro computador e, em seguida, importe todos os discos do segundo computador.

-   O Gerenciamento de Disco descreve a condição dos volumes nos discos antes de serem importados. Examine essas informações com cuidado. Em caso de problemas, essas informações informam o que acontecerá com cada volume desses discos depois que são importados.

-   Se você mover um disco da Tabela de Partição de GUID (GPT) contendo o sistema operacional Windows para um computador baseado em x64 ou x86, é possível acessar os dados, mas inicializar a partir desse sistema operacional.