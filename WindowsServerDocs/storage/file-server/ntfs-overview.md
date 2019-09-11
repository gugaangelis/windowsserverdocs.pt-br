---
title: Visão geral do NTFS
description: Uma explicação do que é o NTFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: e43d0520f97f28af54f794daf7ad263bc9928fac
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867351"
---
# <a name="ntfs-overview"></a>Visão geral do NTFS

>Aplica-se a: Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS — o sistema de arquivos primário para versões recentes do Windows e do Windows Server — fornece um conjunto completo de recursos, incluindo descritores de segurança, criptografia, cotas de disco e metadados avançados, e pode ser usado com CSV (volumes compartilhados de cluster) para fornecer continuamente volumes disponíveis que podem ser acessados simultaneamente de vários nós de um cluster de failover.

Para saber mais sobre a funcionalidade nova e alterada em NTFS no Windows Server 2012 R2, consulte [novidades no NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Para obter informações adicionais sobre recursos, consulte a seção [informações adicionais](#additional-information) deste tópico. Para saber mais sobre o ReFS (sistema de arquivos resiliente) mais recente, consulte [visão geral do refs (sistema de arquivos resiliente)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Aplicações práticas

### <a name="increased-reliability"></a>Maior confiabilidade

O NTFS usa seu arquivo de log e informações de ponto de verificação para restaurar a consistência do sistema de arquivos quando o computador é reiniciado após uma falha do sistema. Após um erro de setor defeituoso, o NTFS remapeia dinamicamente o cluster que contém o setor defeituoso, aloca um novo cluster para os dados, marca o cluster original como defeituoso e não usa mais o cluster antigo. Por exemplo, após uma falha do servidor, o NTFS pode recuperar dados repetindo seus arquivos de log.

O NTFS monitora e corrige continuamente problemas transitórios de corrupção em segundo plano sem colocar o volume offline (esse recurso é conhecido como [NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))de auto-recuperação, introduzido no Windows Server 2008). Para problemas maiores de corrupção, o utilitário chkdsk, no Windows Server 2012 e posterior, examina e analisa a unidade enquanto o volume está online, limitando o tempo offline ao tempo necessário para restaurar a consistência dos dados no volume. Quando o NTFS é usado com volumes compartilhados clusterizados, nenhum tempo de inatividade é necessário. Para obter mais informações, consulte [integridade do NTFS e chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Maior segurança

- **Segurança baseada na ACL (lista de controle de acesso) para arquivos e pastas**— o NTFS permite que você defina permissões em um arquivo ou pasta, especifique os grupos e usuários cujo acesso você deseja restringir ou permitir e selecione tipo de acesso.

- **Suporte para criptografia de unidade de disco BitLocker**— o criptografia de unidade de disco BitLocker fornece segurança adicional para informações críticas do sistema e outros dados armazenados em volumes NTFS. A partir do Windows Server 2012 R2 e Windows 8.1, o BitLocker fornece suporte para criptografia de dispositivo em computadores x86 e x64 com um Trusted Platform Module (TPM) que dá suporte a autônomos conectados (anteriormente disponíveis apenas em dispositivos Windows RT). A criptografia de dispositivo ajuda a proteger dados em computadores baseados no Windows e ajuda a impedir que usuários mal-intencionados acessem os arquivos do sistema dos quais eles dependem para descobrir a senha do usuário ou acessar uma unidade removendo-a fisicamente do PC e instalando-a em um um diferente. Para obter mais informações, consulte [What ' s New in BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Suporte para grandes volumes**— o NTFS pode dar suporte a volumes tão grandes que 256 terabytes. Os tamanhos de volume com suporte são afetados pelo tamanho do cluster e pelo número de clusters. Com os clusters (2<sup>32</sup> – 1) (o número máximo de clusters que o NTFS dá suporte), há suporte para os tamanhos de arquivo e volume a seguir.

  |Tamanho do cluster|Maior volume|Arquivo maior|
  |---|---|---|
  |4 KB (tamanho padrão)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (tamanho máximo)|256 TB|256 TB|

>[!IMPORTANT]
>Serviços e aplicativos podem impor limites adicionais em tamanhos de arquivos e volumes. Por exemplo, o limite de tamanho de volume é de 64 TB se você estiver usando o recurso de versões anteriores ou um aplicativo de backup que usa instantâneos de Serviço de Cópias de Sombra de Volume (VSS) (e você não estiver usando um compartimento de SAN ou RAID). No entanto, talvez seja necessário usar tamanhos de volume menores, dependendo da carga de trabalho e do desempenho do seu armazenamento.

### <a name="formatting-requirements-for-large-files"></a>Requisitos de formatação para arquivos grandes

Para permitir a extensão adequada de arquivos. vhdx grandes, há novas recomendações para a formatação de volumes. Ao formatar volumes que serão usados com eliminação de duplicação de dados ou hospedar arquivos muito grandes, como arquivos. vhdx maiores que 1 TB, use o cmdlet **Format-volume** no Windows PowerShell com os seguintes parâmetros.

|Parâmetro|Descrição|
|---|---|
|-AllocationUnitSize 64 KB|Define um tamanho de unidade de alocação NTFS de 64 KB.|
|-UseLargeFRS|Habilita o suporte para o FRS (segmentos de registro de arquivo grande). Isso é necessário para aumentar o número de extensões permitidas por arquivo no volume. Para grandes registros de FRS, o limite aumenta de aproximadamente 1,5 milhões extensões para cerca de 6 milhões extensões.|

Por exemplo, o cmdlet a seguir formata a unidade D como um volume NTFS, com o FRS habilitado e um tamanho de unidade de alocação de 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Você também pode usar o comando **Format** . Em um prompt de comando do sistema, digite o seguinte comando, em que **/l** formata um volume de FRS grande e **/a: 64K** define um tamanho de unidade de alocação de 64 KB:

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Nome e caminho máximos do arquivo

O NTFS dá suporte a nomes de arquivo longos e caminhos de comprimento estendido, com os seguintes valores máximos:

- **Suporte para nomes de arquivo longos, com compatibilidade com versões anteriores**— o NTFS permite nomes de arquivo longos, armazenando um alias 8,3 no disco (em Unicode) para fornecer compatibilidade com sistemas de arquivos que impõem um limite de 8,3 em nomes de arquivos e extensões. Se necessário (por motivos de desempenho), você pode desabilitar seletivamente a alias de 8,3 em volumes NTFS individuais no Windows Server 2008 R2, Windows 8 e versões mais recentes do sistema operacional Windows.
  No Windows Server 2008 R2 e em sistemas posteriores, os nomes curtos são desabilitados por padrão quando um volume é formatado usando o sistema operacional. Para compatibilidade de aplicativos, nomes curtos ainda são habilitados no volume do sistema.

- **Suporte para caminhos de comprimento estendido**— muitas funções da API do Windows têm versões Unicode que permitem um caminho de tamanho estendido de aproximadamente 32.767 caracteres, além do limite de caminho de 260\_caracteres definido pela configuração de caminho máximo. Para obter um nome de arquivo detalhado e requisitos de formato de caminho e diretrizes para implementar caminhos de comprimento estendido, consulte [nomes de arquivos, caminhos e namespaces](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Armazenamento em cluster**— quando usado em clusters de failover, o NTFS dá suporte a volumes continuamente disponíveis que podem ser acessados por vários nós de cluster simultaneamente quando usados em conjunto com o sistema de arquivos CSV (volumes compartilhados do cluster). Para obter mais informações, consulte [usar volumes compartilhados clusterizados em um cluster de failover](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Alocação flexível da capacidade

Se o espaço em um volume for limitado, o NTFS fornecerá as seguintes maneiras de trabalhar com a capacidade de armazenamento de um servidor:

- Use cotas de disco para controlar e controlar o uso de espaço em disco em volumes NTFS para usuários individuais.
- Use a compactação do sistema de arquivos para maximizar a quantidade de dados que podem ser armazenados.
- Aumente o tamanho de um volume NTFS adicionando espaço não alocado do mesmo disco ou de um disco diferente.
- Monte um volume em qualquer pasta vazia em um volume NTFS local se você ficar sem letras de unidade ou precisar criar espaço adicional que possa ser acessado de uma pasta existente.

## <a name="additional-information"></a>Informações adicionais

- [Recomendações de tamanho de cluster para ReFS e NTFS](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/Cluster-size-recommendations-for-ReFS-and-NTFS/ba-p/425960)
- [Visão geral do ReFS (sistema de arquivos resiliente)](../refs/refs-overview.md)
- [O que há de novo no NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)
- [O que há de novo no NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)
- [Integridade do NTFS e chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))
- [Recuperação automática de NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introduzido no Windows Server 2008)
- [NTFS Transacional](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introduzido no Windows Server 2008)
- [Armazenamento no Windows Server](../storage.md)