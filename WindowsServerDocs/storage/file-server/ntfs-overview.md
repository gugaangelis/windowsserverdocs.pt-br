---
title: Visão geral do NTFS
description: Uma explicação sobre o que é o NTFS.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/06/2018
ms.localizationpriority: medium
ms.openlocfilehash: 556110fe7bed1aed002ef6d985324ff5171e770e
ms.sourcegitcommit: 5ed023a2ef3a9002daf41c7717feb1df186d2a14
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2019
ms.locfileid: "9122099"
---
# Visão geral do NTFS

>Aplica-se a: Windows 10, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

NTFS — o sistema de arquivos primário para versões recentes do Windows e Windows Server — fornece um conjunto completo de recursos, incluindo descritores de segurança, criptografia, cotas de disco e metadados sofisticados e pode ser usado com Volumes de compartilhados de Cluster (CSV) para fornecer continuamente volumes disponíveis que podem ser acessados simultaneamente em vários nós de um cluster de failover.

Para saber mais sobre as funcionalidades novas e alteradas em NTFS no Windows Server 2012 R2, consulte [as novidades em NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)). Para obter informações de recurso adicional, consulte a seção de [informações adicionais](#additional-information) deste tópico. Para saber mais sobre o sistema de arquivos resiliente (ReFS) mais recente, consulte [Visão geral do sistema de arquivos resiliente (ReFS)](../refs/refs-overview.md).

## Aplicações práticas

### Maior confiabilidade

NTFS usa suas informações de arquivo e o ponto de verificação de log para restaurar a consistência do sistema de arquivos quando o computador é reiniciado após uma falha do sistema. Depois de um erro de setor defeituoso, o NTFS remapeia dinamicamente o cluster que contém o setor defeituoso, aloca um novo cluster para os dados, marca no cluster original como defeituoso e não usa o cluster antigo. Por exemplo, após um travamento de servidor, NTFS pode recuperar dados reproduzindo seus arquivos de log.

NTFS monitora continuamente e corrige problemas de corrupção transitório em segundo plano sem colocar o volume offline (esse recurso é conhecido como [autorecuperação NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)), introduzido no Windows Server 2008). Para problemas de corrupção maiores, o utilitário Chkdsk, no Windows Server 2012 e versões posteriores, verificações e analisa a unidade enquanto o volume estiver online, limitando o tempo offline para o tempo necessário para restaurar a consistência de dados no volume. Quando o NTFS é usado com Volumes Compartilhados do Cluster, sem tempo de inatividade é necessário. Para obter mais informações, consulte [NTFS integridade e Chkdsk](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11)).

### Maior segurança

- **Segurança baseada em lista de controle de acesso ACL para arquivos e pastas**— NTFS permite que você defina permissões em um arquivo ou pasta, especifique os grupos e usuários cujo acesso você deseja restringir ou permitir e selecione o tipo de acesso.

- **Suporte para criptografia de unidade BitLocker**— criptografia de unidade BitLocker oferece segurança adicional para informações críticas do sistema e outros dados armazenados em volumes NTFS. A partir do Windows Server 2012 R2 e Windows 8.1, o BitLocker oferece suporte para criptografia de dispositivo em x86 e x64 com base em computadores com um Trusted Platform Module (TPM) que dá suporte à conectada stand-by (anteriormente disponível apenas em dispositivos Windows RT). Criptografia de dispositivo ajuda a proteger dados em computadores baseados no Windows, e ele ajuda a impedir que os usuários mal-intencionados acessem os arquivos de sistema que eles dependem para descobrir a senha do usuário ou de acessar uma unidade fisicamente removê-lo do computador e instalá-lo em um um diferente. Para obter mais informações, consulte [Novidades do BitLocker](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn306081(v%3dws.11)).

- **Suporte a volumes grandes**— NTFS pode dar suporte a volumes grandes como 256 terabytes. Suporte para tamanhos são afetados pelo tamanho do cluster e o número de clusters de volume. Com (2<sup>32</sup> – 1) clusters (o número máximo de clusters que dá suporte a NTFS), o seguinte volume e tamanhos de arquivo são compatíveis.

  |Tamanho de cluster|Maior volume|Arquivo maior|
  |---|---|---|
  |4 KB (tamanho padrão)|16 TB|16 TB|
  |8 KB|32 TB|32 TB|
  |16 KB|64 TB|64 TB|
  |32 KB|128 TB|128 TB|
  |64 KB (tamanho máximo)|256 TB|256 TB|

>[!IMPORTANT]
>Serviços e aplicativos podem impor limites adicionais em tamanhos de arquivo e o volume. Por exemplo, o limite de tamanho do volume é 64 TB se você estiver usando o recurso de versões anteriores ou um aplicativo de backup que faz uso de instantâneos do serviço de cópia de sombra de Volume (VSS) (e você não estiver usando um compartimento de SAN ou RAID). No entanto, convém usar tamanhos de volume menores dependendo de sua carga de trabalho e o desempenho de seu armazenamento.

### Requisitos de formatação arquivos grandes

Para permitir que a extensão adequada dos arquivos. vhdx grande, há novas recomendações para formatar volumes. Ao formatar volumes que serão usados com eliminação de duplicação de dados ou hospedarão arquivos muito grandes, como arquivos. vhdx maiores que 1 TB, use o cmdlet do **Formato de Volume** no Windows PowerShell com os parâmetros a seguir.

|Parâmetro|Descrição|
|---|---|
|-AllocationUnitSize 64KB|Define um tamanho de unidade de alocação de NTFS 64 KB.|
|-UseLargeFRS|Habilita o suporte para segmentos de registro de arquivo grande (FRS). Isso é necessário para aumentar o número de extensões permitido por arquivo no volume. Para os registros de FRS grandes, o limite aumenta de cerca de 1,5 milhão extensões para extensões de cerca de 6 milhões.|

Por exemplo, os seguintes formatos de cmdlet unidade D como um volume NTFS com FRS habilitado e um tamanho de unidade de alocação de 64 KB.

```PowerShell
Format-Volume -DriveLetter D -FileSystem NTFS -AllocationUnitSize 64KB -UseLargeFRS
```

Você também pode usar o comando de **formato** . Em um prompt de comando do sistema, digite o seguinte comando, onde **/L** formata um grande volume de FRS e **/A:64 k** define um tamanho de unidade de alocação de 64 KB:

```PowerShell
format /L /A:64k
```

### Caminho e nome de arquivo máximo

NTFS dá suporte a nomes de arquivo longos e os caminhos de comprimento estendida, com os seguintes valores máximo:

- **Suporte para nomes de arquivo longos, compatibilidade com versões anteriores**— NTFS permite que os nomes de arquivo longos, armazenando um 8.3 alias em disco (em Unicode) para fornecer compatibilidade com sistemas de arquivos que impõem um limite 8.3 em nomes de arquivo e extensões. Se necessário (por motivos de desempenho), você pode desabilitar seletivamente serrilhado 8.3 nos volumes NTFS individuais no Windows Server 2008 R2, Windows 8 e versões mais recentes do sistema operacional Windows.
  No Windows Server 2008 R2 e sistemas posteriores, os nomes curtos são desabilitados por padrão quando um volume é formatado usando o sistema operacional. Para compatibilidade de aplicativo, nomes curtos ainda estão habilitados no volume do sistema.

- **Suporte para caminhos de comprimento estendida**— funções de API do Windows muitos têm versões Unicode que permitem que um caminho de comprimento estendido de aproximadamente 32.767 caracteres — além do limite de 260 caracteres caminho definido pela configuração MAX\_PATH. Para o nome do arquivo detalhadas e requisitos de formato do caminho e diretrizes para a implementação de caminhos de comprimento estendida, consulte [nomeando arquivos, caminhos e Namespaces](https://msdn.microsoft.com/library/windows/desktop/aa365247).

- **Armazenamento de cluster**— quando usado em clusters de failover, NTFS dá suporte a volumes continuamente disponíveis que podem ser acessados por vários nós de cluster simultaneamente quando usado em conjunto com o sistema de arquivos de Volumes Compartilhados do Cluster (CSV). Para obter mais informações, consulte [Usar Volumes Compartilhados do Cluster em um Cluster de Failover](../../failover-clustering/failover-cluster-csvs.md).

### Alocação flexível de capacidade

Se o espaço em um volume é limitado, NTFS fornece as seguintes maneiras de trabalhar com a capacidade de armazenamento de um servidor:

- Use as cotas de disco para rastrear e controlar o uso de espaço em disco em volumes NTFS para usuários individuais.
- Use compactação de sistema de arquivos para maximizar a quantidade de dados que podem ser armazenados.
- Aumente o tamanho de um volume NTFS, adicionando um espaço não alocado do mesmo disco ou de um disco diferente.
- Monte um volume em qualquer pasta vazia em um volume NTFS local se perdem letras de unidade ou precisa criar espaço adicional que é acessível a partir de uma pasta existente.

## Informações adicionais

|Tipo de conteúdo|Referências|
|---|---|
|Avaliação|- [Novidades no NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn466520(v%3dws.11)) (Windows Server 2012 R2)<br>- [Novidades no NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff383236(v=ws.10)) (Windows Server 2008 R2, Windows 7)<br>- [CHKDSK e integridade do NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831536(v%3dws.11))<br>- [Autorecuperação NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)) (introduzido no Windows Server 2008)<br>- [NTFS Transacional](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc730726(v%3dws.10)) (introduzido no Windows Server 2008)|
|Recursos da comunidade|- [Blog da equipe de armazenamento Windows](https://blogs.msdn.microsoft.com/san/)|
|Tecnologias relacionadas|- [Armazenamento no Windows Server](../storage.md)<br>- [Cluster de uso compartilhado Volumes em um Cluster de Failover](../../failover-clustering/failover-cluster-csvs.md)<br>-As seções [Volumes Compartilhados do Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#cluster-shared-volumes>) e o [Design de armazenamento](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)#storage-design>) de [Projetar sua infraestrutura de nuvem](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831630(v%3dws.11)) <br>- [Espaços de armazenamento](../storage-spaces/overview.md)<br>- [Visão geral do resiliente (ReFS) do sistema de arquivos](../refs/refs-overview.md)
