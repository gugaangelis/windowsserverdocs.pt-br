---
title: Visão geral do NTFS
description: Obter uma explicação das novidades no NTFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: f85e321381adcf607c3504005a0a3448ab0f098a
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63738449"
---
# <a name="ntfs-overview"></a>Visão geral do NTFS

>Aplica-se a: Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS — o sistema de arquivos primário para versões recentes do Windows e Windows Server — fornece um conjunto completo de recursos, incluindo descritores de segurança, criptografia, cotas de disco e metadados sofisticados e pode ser usado com o Cluster CSV (Volumes Compartilhados) para fornecer continuamente volumes disponíveis que podem ser acessados simultaneamente em vários nós de um cluster de failover.

Para saber mais sobre as funcionalidades novas e alteradas do NTFS no Windows Server 2012 R2, consulte [novidades do NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Para informações adicionais sobre recursos, consulte a [informações adicionais](#additional-information) seção deste tópico. Para saber mais sobre o sistema de arquivos resiliente (ReFS) mais recente, consulte [visão geral do sistema de arquivos resiliente (ReFS)](../refs/refs-overview.md).

## <a name="practical-applications"></a>Aplicações práticas

### <a name="increased-reliability"></a>Maior confiabilidade

Para restaurar a consistência do sistema de arquivos quando o computador é reiniciado após uma falha do sistema, o NTFS usa suas informações de ponto de verificação e o arquivo de log. Após um erro de setor defeituoso NTFS remapeia dinamicamente o cluster que contém o setor defeituoso, aloca um novo cluster para os dados, marca o cluster original como inválido e não usa mais o cluster antigo. Por exemplo, após uma falha do servidor, NTFS pode recuperar os dados ao reproduzir novamente os seus arquivos de log.

NTFS monitora continuamente e corrige problemas de danos transitórios em segundo plano sem colocar o volume offline (esse recurso é conhecido como [autorrecuperação NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), apresentada no Windows Server 2008). Para problemas de corrupção maiores, o utilitário Chkdsk, no Windows Server 2012 e versões posteriores, verifica e analisa a unidade, enquanto o volume estiver online, limitando o tempo offline para o tempo necessário para restaurar a consistência de dados no volume. Quando o NTFS é usado com Volumes Compartilhados do Cluster, sem tempo de inatividade é necessário. Para obter mais informações, consulte [integridade do NTFS e Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### <a name="increased-security"></a>Aumentar a segurança

- **Segurança baseada em ACL lista de controle de arquivos e pastas de acesso**— NTFS permite que você defina permissões em um arquivo ou pasta, especifique os grupos e usuários cujo acesso você deseja restringir ou permitir o e selecione o tipo de acesso.

- **Suporte para o BitLocker Drive Encryption**— a criptografia de unidade de disco BitLocker fornece segurança adicional para informações críticas do sistema e outros dados armazenados em volumes NTFS. A partir do Windows Server 2012 R2 e Windows 8.1, o BitLocker dá suporte para criptografia de dispositivo em computadores x86 e baseadas em x64 com um Trusted Platform Module (TPM) que dá suporte ao modo de espera conectado (anteriormente disponíveis somente em dispositivos Windows RT). Criptografia do dispositivo ajuda a proteger dados em computadores baseados em Windows e ajuda a impedir que usuários mal-intencionados acessem os arquivos de sistema que utilizam para descobrir a senha do usuário ou de acessar uma unidade fisicamente removendo-o do computador e instalá-lo em um um diferente. Para obter mais informações, consulte [quais são as novidades no BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Suporte para grandes volumes**— NTFS pode dar suporte a volumes de até 256 terabytes. Suporte para tamanhos são afetados pelo tamanho do cluster e o número de clusters de volume. Com (2<sup>32</sup> – 1) clusters (o número máximo de clusters que o NTFS oferece suporte), o seguinte volume e tamanhos de arquivo têm suporte.

  |Tamanho do cluster|Maior volume|Maior arquivo|
  |---|---|---|
  |4 KB (tamanho padrão)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (tamanho máximo)|256 TB|256 TB|

>[!IMPORTANT]
>Aplicativos e serviços podem impor limites adicionais em tamanhos de arquivo e volume. Por exemplo, o limite de tamanho do volume é 64 TB, se você estiver usando o recurso versões anteriores ou um aplicativo de backup que faz uso de instantâneos de Volume Shadow Copy Service (VSS) (e você não estiver usando um compartimento RAID ou SAN). No entanto, você talvez precise usar tamanhos de volume menores, dependendo de sua carga de trabalho e o desempenho de seu armazenamento.

### <a name="formatting-requirements-for-large-files"></a>Requisitos de formatação para arquivos grandes

Para permitir que a extensão apropriada dos arquivos. vhdx grandes, há novas recomendações para formatação de volumes. Ao formatar os volumes que serão usados com eliminação de duplicação de dados ou que hospedarão os arquivos muito grandes, como arquivos. vhdx maiores que 1 TB, use o **Format-Volume** cmdlet no Windows PowerShell com os seguintes parâmetros.

|Parâmetro|Descrição|
|---|---|
|-AllocationUnitSize 64KB|Define um tamanho de unidade de alocação de NTFS de 64 KB.|
|-UseLargeFRS|Habilita o suporte para segmentos de registro de grandes arquivos (FRS). Isso é necessário para aumentar o número de extensões permitidas por arquivo no volume. Para registros de FRS grandes, o limite aumenta de extensões de cerca de 1,5 milhão para extensões de cerca de 6 milhões.|

Por exemplo, os seguintes formatos de cmdlet unidade D como um volume NTFS, com um tamanho de unidade de alocação de 64 KB e o FRS habilitado.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Você também pode usar o **formato** comando. Em um prompt de comando do sistema, digite o seguinte comando, onde **/L** formata um grande volume de FRS e **/A:64 k** define um tamanho de unidade de alocação de 64 KB:

```PowerShell
format /L /A:64k
```

### <a name="maximum-file-name-and-path"></a>Caminho e nome de arquivo máximo

O NTFS oferece suporte a nomes de arquivo longos e caminhos de comprimento estendido, com os seguintes valores máximos:

- **Suporte para nomes de arquivo longos, com compatibilidade com versões anteriores de**— NTFS permite nomes de arquivo longos, armazenando um 8.3 alias em disco (em Unicode) para fornecer compatibilidade com sistemas de arquivos que impõe um limite 8.3 nomes de arquivo e extensões. Se for necessário (por motivos de desempenho), você pode desabilitar seletivamente alias 8.3 em volumes NTFS individuais no Windows Server 2008 R2, Windows 8 e versões mais recentes do sistema operacional Windows.
  No Windows Server 2008 R2 e sistemas posteriores, os nomes curtos estão desabilitados por padrão quando um volume é formatado usando o sistema operacional. Compatibilidade de aplicativos, nomes curtos ainda estão habilitados no volume do sistema.

- **Suporte para caminhos de comprimento estendido**— funções de API do Windows muitas têm versões Unicode que permitem que um caminho de comprimento estendido de aproximadamente 32.767 caracteres — além do limite de 260 caracteres de caminho definido por MAX\_configuração do caminho. Para o nome do arquivo detalhadas e requisitos de formato de caminho e diretrizes para implementar os caminhos de comprimento estendido, consulte [nomeando arquivos, caminhos e Namespaces](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Armazenamento de cluster**— quando usados em clusters de failover, o NTFS oferece suporte a volumes continuamente disponíveis que podem ser acessados por vários nós de cluster simultaneamente quando usado em conjunto com o sistema de arquivos do Cluster CSV (Volumes Compartilhados). Para obter mais informações, consulte [usar Volumes Compartilhados do Cluster em um Cluster de Failover](../../failover-clustering/failover-cluster-csvs.md).

### <a name="flexible-allocation-of-capacity"></a>Alocação flexível da capacidade

Se o espaço em um volume é limitado, o NTFS oferece maneiras a seguir para trabalhar com a capacidade de armazenamento de um servidor:

- Use as cotas de disco para rastrear e controlar o uso de espaço em disco em volumes NTFS para usuários individuais.
- Use compactação do sistema de arquivos para maximizar a quantidade de dados que podem ser armazenados.
- Aumente o tamanho de um volume NTFS, adicionando o espaço não alocado do mesmo disco ou de um disco diferente.
- Monte um volume em qualquer pasta vazia em um volume NTFS local, se você ficar sem letras de unidade ou precisará criar o espaço adicional que pode ser acessado de uma pasta existente.

## <a name="additional-information"></a>Informações adicionais

|Tipo de conteúdo|Referências|
|---|---|
|Avaliação|- [Quais são as novidades do NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)<br>- [Quais são as novidades do NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)<br>- [Integridade do NTFS e CHKDSK](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [Autorrecuperação NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introduzida no Windows Server 2008)<br>- [O NTFS Transacional](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introduzida no Windows Server 2008)|
|Recursos da comunidade|- [Blog da equipe de armazenamento do Windows](https://blogs.msdn.microsoft.com/san/)|
|Tecnologias relacionadas|- [Armazenamento no Windows Server](../storage.md)<br>- [Use Cluster Shared Volumes em um Cluster de Failover](../../failover-clustering/failover-cluster-csvs.md)<br>-A [Volumes Compartilhados do Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>) e [Design de armazenamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>) seções [Projetando sua infraestrutura de nuvem](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [Espaços de armazenamento](../storage-spaces/overview.md)<br>- [Visão geral do resiliente (ReFS) do sistema de arquivos](../refs/refs-overview.md)
