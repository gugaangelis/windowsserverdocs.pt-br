---
title: Melhorar o desempenho de um servidor de arquivos com o SMB Direct
description: Descreve o SMB Direct recurso no Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: ed8fd5b4114fc9fd9c7dc278a98cea8cc67a8749
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239223"
---
# SMB Direct

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016 incluem um recurso chamado SMB Direct, que permite o uso de adaptadores de rede que têm a funcionalidade de acesso de memória de Direct (RDMA) remoto. Adaptadores de rede com RDMA podem funcionar em velocidade máxima com latência muito baixa, durante o uso de CPU muito pouco. Para cargas de trabalho, como o Hyper-V ou o Microsoft SQL Server, isso permite que um servidor de arquivos remoto para se parecer com armazenamento local. SMB Direct inclui:

- Maior produtividade: aproveita a taxa de transferência total de redes de alta velocidade em que os adaptadores de rede coordenar a transferência de grandes quantidades de dados na velocidade da linha.
- Baixa latência: fornece respostas extremamente rápidas às solicitações de rede e, consequentemente, torne o armazenamento de arquivos remoto como se fosse o armazenamento em bloco diretamente conectado.
- Baixa utilização da CPU: usa menos ciclos de CPU quando transferir dados pela rede, o que deixa mais energia disponível para aplicativos de servidor.

SMB Direct é configurado automaticamente pelo Windows Server 2012 R2 e Windows Server 2012.

## SMB Multichannel e SMB Direct

SMB Multichannel é o recurso responsável para detectar os recursos RDMA de adaptadores de rede para habilitar o SMB Direct. Sem SMB Multichannel, SMB usa TCP/IP regulares com os adaptadores de rede compatíveis com RDMA (todos os adaptadores de rede fornecem uma pilha de TCP/IP juntamente com a nova pilha RDMA).

Com o SMB Multichannel, SMB detecta se um adaptador de rede tem a capacidade RDMA e, em seguida, cria várias conexões de RDMA para essa sessão única (por interface dois). Isso permite que o SMB usar a taxa de transferência alta, de baixa latência e baixa utilização da CPU oferecido por adaptadores de rede compatíveis com RDMA. Ele também oferece a tolerância a falhas se você estiver usando várias interfaces RDMA.

>[!NOTE]
>Você não deve equipe adaptadores de rede compatíveis com RDMA se você pretende usar a funcionalidade RDMA dos adaptadores de rede. Quando agrupados, os adaptadores de rede não dará suporte RDMA.
>Depois que pelo menos uma conexão de rede RDMA é criada, a conexão TCP/IP usado para a negociação de protocolo original não é mais usada. No entanto, a conexão TCP/IP é mantida em caso de falham de conexões de rede RDMA.

## Requisitos

SMB Direct requer o seguinte:

- Pelo menos dois computadores que executam o Windows Server 2012 R2 ou Windows Server 2012
- Um ou mais adaptadores de rede com capacidade RDMA.

### Considerações ao usar o SMB Direct

- Você pode usar o SMB Direct em um cluster de failover; No entanto, você precisará certificar-se de que as redes de cluster usadas para o acesso de cliente são adequadas para SMB Direct. Failover clustering supports using multiple networks for client access, along with network adapters that are RSS (Receive Side Scaling)-capable and RDMA-capable.
- Você pode usar SMB Direct no sistema de operacional de gerenciamento do Hyper-V para suporte ao uso do Hyper-V em SMB e fornecer armazenamento para uma máquina virtual que usa a pilha de armazenamento do Hyper-V. No entanto, os adaptadores de rede compatíveis com RDMA não são expostos diretamente a um cliente do Hyper-V. Se você conectar um adaptador de rede compatíveis com RDMA a um comutador virtual, os adaptadores de rede virtual do switch não será capaz de RDMA.
- Se você desabilitar o SMB Multichannel, SMB Direct também está desabilitado. Como o SMB Multichannel detecta recursos do adaptador de rede e determina se um adaptador de rede é compatível com RDMA, SMB Direct não pode ser usado pelo cliente se SMB Multichannel está desabilitada.
- SMB Direct não é compatível com RT Windows. SMB Direct requer suporte para adaptadores de rede compatíveis com RDMA, que está disponível somente no Windows Server 2012 R2 e Windows Server 2012.
- SMB Direct não é compatível com versões de baixo nível do Windows Server. Ele tem suporte somente no Windows Server 2012 R2 e Windows Server 2012.

## Habilitando e desabilitando SMB Direct

SMB Direct é habilitada por padrão quando o Windows Server 2012 R2 ou Windows Server 2012 é instalado. O cliente SMB detecta automaticamente e usa várias conexões de rede se uma configuração apropriada é identificada.

### Desabilitar o SMB Direct

Normalmente, você não precisará desabilitar o SMB Direct, no entanto, você pode desativá-la, executando um dos seguintes scripts do Windows PowerShell.

Para desativar o RDMA para uma interface específica, digite:

```PowerShell
Disable-NetAdapterRdma <name>
```

Para desativar o RDMA para todas as interfaces, digite:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

Quando você desabilita o RDMA no cliente ou servidor, os sistemas não podem usá-lo. *Rede direta* é o nome interno para Windows Server 2012 R2 e Windows Server 2012 suporte básico de rede para interfaces RDMA.

### Habilitar novamente SMB Direct

Depois de desabilitá RDMA, você pode reativá-la, executando um dos seguintes scripts do Windows PowerShell.

Para reativar RDMA para uma interface específica, digite:

```PowerShell
Enable-NetAdapterRDMA <name>
```

Para reativar RDMA para todas as interfaces, digite:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

Você precisará habilitar o RDMA no cliente e o servidor começarem a usá-lo novamente.

## Teste de desempenho do SMB Direct

Você pode testar como o desempenho está funcionando usando um dos procedimentos a seguir.

### Comparar uma cópia de arquivo com e sem o uso de SMB Direct

Aqui está como medir a taxa de transferência maior de SMB Direct:

1. Configurar o SMB Direct
2. Medir a quantidade de tempo para executar uma cópia de arquivo grande usando SMB Direct.
3. Desativar RDMA no adaptador de rede, consulte [Habilitando e desabilitando SMB Direct](#enabling-and-disabling-smb-direct).
4. Medir a quantidade de tempo para executar uma cópia de arquivo grande sem usar SMB Direct.
5. Habilitar novamente o RDMA no adaptador de rede e, em seguida, comparar dois resultados.
6. Para evitar o impacto de armazenamento em cache, você deve fazer o seguinte:
    1. Copie uma grande quantidade de dados (mais dados de memória é capaz de manipulação).
    2. Copie os dados de duas vezes, com a primeira cópia como prática e tempo, em seguida, a segunda cópia.
    3. Reinicie o servidor e o cliente antes de cada teste para garantir que eles operam em condições semelhantes.

### Falha em um dos vários adaptadores de rede durante uma cópia do arquivo com o SMB Direct

Aqui está como confirmar o recurso de failover do SMB Direct:

1. Certifique-se de que SMB Direct está funcionando em uma configuração de adaptador de rede vários.
2. Execute uma cópia de arquivo grande. Enquanto a cópia é executada, simule uma falha de um dos caminhos de rede, desconectando um dos cabos (ou desativando um dos adaptadores de rede).
3. Confirme que a cópia dos arquivos continua usando um dos adaptadores de rede restantes, e que não há nenhum erro de cópia de arquivo.

>[!NOTE]
>Para evitar falhas de uma carga de trabalho que não usa SMB Direct, verifique não se há nenhuma outras cargas de trabalho usando o caminho de rede desconectado.

## Mais informações

- [Visão geral de bloco de mensagens de servidor](file-server-smb-overview.md)
- [Aumentar a disponibilidade da rede, armazenamento e servidor: Visão geral do cenário](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Implantar o Hyper-V no SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
