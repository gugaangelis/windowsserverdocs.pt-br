---
title: Visão geral do NTFS
description: O NTFS — o sistema de arquivos primário para versões recentes do Windows e do Windows Server — fornece um conjunto completo de recursos, incluindo descritores de segurança, criptografia, cotas de disco e metadados sofisticados e pode ser usado com CSV (Volume Compartilhado de Cluster) para fornecer volumes continuamente disponíveis que podem ser acessados simultaneamente em vários nós de um cluster de failover.
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 09/30/2020
ms.localizationpriority: medium
ms.openlocfilehash: 30fe719b7e36706e59650ab18a82276879f92830
ms.sourcegitcommit: d04d63d48856bccf5d5a9b1df6b25e254e7eda2b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91622841"
---
# <a name="ntfs-overview"></a>Visão geral do NTFS

>Aplica-se a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

O NTFS — o sistema de arquivos primário para versões recentes do Windows e do Windows Server — fornece um conjunto completo de recursos, incluindo descritores de segurança, criptografia, cotas de disco e metadados sofisticados e pode ser usado com CSV (Volume Compartilhado de Cluster) para fornecer volumes continuamente disponíveis que podem ser acessados simultaneamente em vários nós de um cluster de failover.

Para obter informações adicionais sobre recursos, confira a seção [Informações adicionais](#additional-information) deste tópico. Para saber mais sobre o ReFS (Sistema de Arquivos Resiliente) mais recente, confira a [visão geral do ReFS (Sistema de Arquivos Resiliente)](../refs/refs-overview.md).

## <a name="increased-reliability"></a>Maior confiabilidade

O NTFS usa seu arquivo de log e informações de ponto de verificação para restaurar a consistência do sistema de arquivos quando o computador é reiniciado após uma falha do sistema. Após um erro de setor defeituoso, o NTFS remapeia dinamicamente o cluster que contém o setor defeituoso, aloca um novo cluster para os dados, marca o cluster original como defeituoso e não usa mais o cluster antigo. Por exemplo, após uma falha do servidor, o NTFS pode recuperar dados repetindo seus arquivos de log.

O NTFS monitora e corrige continuamente problemas transitórios de danificação em segundo plano sem colocar o volume offline (esse recurso é conhecido como [NTFS de autorrecuperação](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771388(v=ws.10)), introduzido no Windows Server 2008). Para problemas maiores de danificação, o utilitário chkdsk, no Windows Server 2012 e posterior, examina e analisa a unidade enquanto o volume está online, limitando o tempo offline ao tempo necessário para restaurar a consistência dos dados no volume. Quando o NTFS é usado com Volumes Compartilhados de Cluster, nenhum tempo de inatividade é necessário. Para obter mais informações, confira [Integridade do NTFS e Chkdsk](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

## <a name="increased-security"></a>Maior segurança

- **Segurança baseada na ACL (lista de controle de acesso) para arquivos e pastas** – o NTFS permite que você defina permissões em um arquivo ou pasta, especifique os grupos e os usuários cujo acesso você deseja restringir ou permitir e selecione tipo de acesso.

- **Suporte para Criptografia de Unidade de Disco BitLocker** – a Criptografia de Unidade de Disco BitLocker de Disco fornece segurança adicional para informações críticas do sistema e outros dados armazenados em volumes NTFS. Do Windows Server 2012 R2 e do Windows 8.1 em diante, o BitLocker fornece suporte para criptografia de dispositivo em computadores x86 e x64 com um TPM (Trusted Platform Module) compatível com autônomos conectados (anteriormente disponível apenas em dispositivos Windows RT). A criptografia de dispositivo ajuda a proteger dados em computadores baseados no Windows e a impedir que usuários mal-intencionados acessem os arquivos do sistema dos quais eles dependem para descobrir a senha do usuário ou acessar uma unidade removendo-a fisicamente do PC e instalando-a em outro computador. Para obter mais informações, confira [Novidades do BitLocker](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

## <a name="support-for-large-volumes"></a>Suporte a volumes grandes

O NTFS pode dar suporte a volumes tão grandes quanto 8 petabytes no Windows Server 2019 e mais recente e no Windows 10, versão 1709 e mais recente (o suporte a versões anteriores é de até 256 TB). Os tamanhos de volume compatíveis são afetados pelo tamanho do cluster e pelo número de clusters. Com os clusters (2<sup>32</sup> – 1) (o número máximo de clusters compatível com o NTFS), há suporte para os tamanhos de volume e arquivo a seguir.

  | Tamanho do cluster         | Maior volume e arquivo |
  | -------------------  | -------------- |
  | 4 KB (tamanho padrão)  | 16 TB          |
  | 8 KB                 | 32 TB          |
  | 16 KB                | 64 TB          |
  | 32 KB                | 128 TB         |
  | 64 KB (máximo anterior)  | 256 TB         |
  | 128 KB               | 512 TB         |
  | 256 KB               | 1 PB           |
  | 512 KB               | 2 PB           |
  | 1024 KB              | 4 PB           |
  | 2048 KB (tamanho máximo)   | 8 PB           |

Observe que, se você tentar montar um volume com um tamanho de cluster maior do que o máximo compatível da versão do Windows que você está usando, obterá o erro STATUS_UNRECOGNIZED_VOLUME.

>[!IMPORTANT]
>Serviços e aplicativos podem impor limites adicionais a tamanhos de arquivos e volumes. Por exemplo, o limite de tamanho de volume será de 64 TB se você estiver usando o recurso de versões anteriores ou um aplicativo de backup que usa instantâneos do VSS (Serviço de Cópias de Sombra de Volume) (e você não estiver usando um compartimento de SAN ou RAID). No entanto, talvez seja necessário usar tamanhos de volume menores, dependendo da carga de trabalho e do desempenho do seu armazenamento.

## <a name="formatting-requirements-for-large-files"></a>Requisitos de formatação para arquivos grandes

Para permitir a extensão adequada de arquivos .vhdx grandes, há novas recomendações para a formatação de volumes. Ao formatar volumes que serão usados com Eliminação de Duplicação de Dados ou que hospedarão arquivos muito grandes, como arquivos .vhdx maiores que 1 TB, use o cmdlet **Format-Volume** no Windows PowerShell com os parâmetros a seguir.

|Parâmetro|Descrição|
|---|---|
|-AllocationUnitSize 64KB|Define um tamanho de unidade de alocação NTFS de 64 KB.|
|-UseLargeFRS|Habilita o suporte para FRS (segmentos de registro de arquivo grande). Isso é necessário para aumentar o número de extensões permitidas por arquivo no volume. Para registros FRS grandes, o limite aumenta de aproximadamente 1,5 milhão de extensões para cerca de 6 milhões de extensões.|

Por exemplo, o cmdlet a seguir formata a unidade D como um volume NTFS, com o FRS habilitado e um tamanho de unidade de alocação de 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Você também pode usar o comando **format**. Em um prompt de comando do sistema, digite o seguinte comando, em que **/L** formata um volume FRS grande e **/A:64k** define um tamanho de unidade de alocação de 64 KB:

```PowerShell
format /L /A:64k
```

## <a name="maximum-file-name-and-path"></a>Caminho e nome do arquivo máximos

O NTFS é compatível com nomes de arquivo longos e caminhos de comprimento estendido, com os seguintes valores máximos:

- **Suporte para nomes de arquivo longos, com compatibilidade com versões anteriores**— o NTFS permite nomes de arquivo longos, armazenando um alias 8.3 no disco (em Unicode) para fornecer compatibilidade com sistemas de arquivos que impõem um limite de 8.3 em nomes de arquivos e extensões. Se necessário (por motivos de desempenho), você poderá desabilitar seletivamente a alias 8.3 em volumes NTFS individuais no Windows Server 2008 R2, Windows 8 e versões mais recentes do sistema operacional Windows.
  No Windows Server 2008 R2 e em sistemas posteriores, os nomes curtos são desabilitados por padrão quando um volume é formatado usando o sistema operacional. Para compatibilidade do aplicativo, nomes curtos ainda são habilitados no volume do sistema.

- **Suporte para caminhos de comprimento estendido**– muitas funções de API do Windows têm versões Unicode que permitem um caminho de tamanho estendido de aproximadamente 32.767 caracteres, além do limite de caminho de 260 caracteres definido pela configuração de MAX\_PATH. Para obter um nome de arquivo detalhado e requisitos de formato de caminho e diretrizes para implementar caminhos de comprimento estendido, confira [Nomes de arquivos, caminhos e namespaces](/windows/win32/fileio/naming-a-file).

- **Armazenamento clusterizado** — quando usado em clusters de failover, o NTFS é compatível com volumes continuamente disponíveis que podem ser acessados por vários nós de cluster simultaneamente quando usados em conjunto com o sistema de arquivos CSV (Volumes Compartilhados de Cluster). Para obter mais informações, confira [Usar Volumes Compartilhados de Cluster em um Cluster de Failover](../../failover-clustering/failover-cluster-csvs.md).

## <a name="flexible-allocation-of-capacity"></a>Alocação flexível da capacidade

Se o espaço em um volume for limitado, o NTFS fornecerá as seguintes maneiras de trabalhar com a capacidade de armazenamento de um servidor:

- Usar cotas de disco para acompanhar e controlar o uso de espaço em disco em volumes NTFS para usuários individuais.
- Usar a compactação do sistema de arquivos para maximizar a quantidade de dados que podem ser armazenados.
- Aumentar o tamanho de um volume NTFS adicionando espaço não alocado do mesmo disco ou de um disco diferente.
- Montar um volume em qualquer pasta vazia em um volume NTFS local se você ficar sem letras de unidade ou precisar criar espaço adicional que possa ser acessado de uma pasta existente.

## <a name="additional-information"></a>Informações adicionais

- [Recomendações de tamanho de cluster para ReFS e NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Visão geral do ReFS (Sistema de Arquivos Resiliente)](../refs/refs-overview.md)
- [Novidades do NTFS](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [Novidades do NTFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Integridade do NTFS e Chkdsk](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [Autorrecuperação do NTFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc771388(v=ws.10)) (introduzida no Windows Server 2008)
- [NTFS Transacional](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introduzido no Windows Server 2008)
- [Armazenamento no Windows Server](../storage.yml)
