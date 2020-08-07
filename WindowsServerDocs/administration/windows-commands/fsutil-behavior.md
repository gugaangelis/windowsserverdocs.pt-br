---
title: fsutil behavior
description: Artigo de referência para o comando de comportamento fsutil, que consulta ou define o comportamento do volume NTFS.
manager: dmoss
ms.author: toklima
author: toklima
ms.topic: article
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: 3a81d8350b2c8eed1abcfafcaa0971b022678db0
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890079"
---
# <a name="fsutil-behavior"></a>fsutil behavior

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Consulta ou define o comportamento do volume NTFS, que inclui:

- Criando os nomes de arquivo de comprimento de caractere 8,3.

- Estender o uso de caracteres em nomes de arquivo curtos de comprimento de caractere 8,3 em volumes NTFS.

- Atualização do carimbo de **data/hora do último acesso** quando os diretórios são listados em volumes NTFS.

- A frequência com que os eventos de cota são gravados no log do sistema e no pool paginável NTFS e nos níveis de cache de memória do pool não paginável do NTFS.

- O tamanho da zona da tabela de arquivos mestre (zona MFT).

- Exclusão silenciosa de dados quando o sistema encontra corrupção em um volume NTFS.

- Notificação de exclusão de arquivo (também conhecida como corte ou desmapeamento).

## <a name="syntax"></a>Sintaxe

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<volumepath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <value> | [<volumepath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <frequency> | symlinkevaluation <symboliclinktype> | disabledeletenotify {1|0}}
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| Consulta | Consulta os parâmetros de comportamento do sistema de arquivos. |
| set | Altera os parâmetros de comportamento do sistema de arquivos. |
| allowextchar`{1|0}` | Permite (**1**) ou não permitir (**0**) caracteres do conjunto de caracteres estendidos (incluindo caracteres diacríticos) a serem usados em nomes de arquivo curtos de comprimento de caractere 8,3 em volumes NTFS.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| Bugcheckoncorrupt`{1|0}` | Permite (**1**) ou não permite (**0**) a geração de uma verificação de bug quando há corrupção em um volume NTFS. Esse recurso pode ser usado para impedir que o NTFS exclua dados silenciosamente quando usados com o recurso de auto-recuperação do NTFS.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| disable8dot3 [ <volumepath> ]`{1|0}` | Desabilita (**1**) ou habilita (**0**) a criação de 8,3 nomes de arquivo de comprimento de caractere em volumes formatados em FAT e NTFS. Opcionalmente, Prefixe com o *VolumePath* especificado como um nome de unidade seguido por dois-pontos ou GUID. |
| disablecompression`{1|0}` | Desabilita **(1)** ou habilita **(0)** compactação NTFS.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| disablecompressionlimit`{1|0}` | Desabilita **(1)** ou habilita **(0)** limite de compactação NTFS no volume NTFS. Quando um arquivo compactado atinge um determinado nível de fragmentação, em vez de falhar em estender o arquivo, o NTFS para de compactar extensões adicionais do arquivo. Isso foi feito para permitir que arquivos compactados sejam maiores do que normalmente seriam. Definir esse valor como **true** desabilita esse recurso, o que limita o tamanho dos arquivos compactados no sistema. Não recomendamos desabilitar esse recurso.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| disableencryption`{1|0}` | Desabilita **(1)** ou habilita **(0)** a criptografia de pastas e arquivos em volumes NTFS.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| disablefilemetadataoptimization`{1|0}` | Desabilita **(1)** ou habilita **(0) otimização de metadados de** arquivo. O NTFS tem um limite de quantas extensões um determinado arquivo pode ter. Arquivos compactados e esparsos podem se tornar muito fragmentados. Por padrão, o NTFS compacta periodicamente suas estruturas de metadados internas para permitir arquivos mais fragmentados. Definir esse valor como **true** desabilita essa otimização interna. Não recomendamos desabilitar esse recurso.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| disablelastaccess`{1|0}` | Desabilita (**1**) ou habilita (**0**) atualizações para o último carimbo de hora de acesso em cada diretório quando os diretórios são listados em um volume NTFS.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| disablespotcorruptionhandling`{1|0}` | Desabilita **(1)** ou habilita **(0) tratamento de corrupção de** pontos. Também permite que os administradores de sistema executem CHKDSK para analisar o estado de um volume sem colocá-lo offline. Não recomendamos desabilitar esse recurso.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| disabletxf`{1|0}` | Desabilita **(1)** ou habilita **(0)** TxF no volume NTFS especificado. O TxF é um recurso NTFS que fornece uma transação como semântica para operações do sistema de arquivos. O TxF está atualmente preterido, mas a funcionalidade ainda está disponível. Não recomendamos desabilitar esse recurso no volume C:.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| disablewriteautotiering`{1|0}` | Desabilita a lógica de camadas automáticas de ReFS v2 para volumes em camadas.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| encryptpagingfile`{1|0}` | Criptografa (**1**) ou não criptografa (**0**) o arquivo de paginação de memória no sistema operacional Windows.<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| mftzone`<value>` | Define o tamanho da zona MFT e é expresso como um múltiplo de unidades 200 MB. Defina *valor* como um número de **1** (o padrão é 200 MB) para **4** (o máximo é 800 MB).<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| memoryusage`<value>` | Configura os níveis de cache internos de memória de pool paginável NTFS e memória de pool não paginável. Defina como **1** ou **2**. Quando definido como **1** (o padrão), o NTFS usa a quantidade padrão de memória de pool paginável. Quando definido como **2**, o NTFS aumenta o tamanho de suas listas à parte e limites de memória. (Uma lista à parte é um pool de buffers de memória de tamanho fixo que o kernel e os drivers de dispositivo criam como caches de memória privada para operações do sistema de arquivos, como a leitura de um arquivo.)<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| quotanotify`<frequency>` | Configura com que frequência as violações de cota NTFS são relatadas no log do sistema. Os valores válidos para estão no intervalo de **0 a 4294967295**. A frequência padrão é de **3600** segundos (uma hora).<p>Você deve reiniciar o computador para que esse parâmetro entre em vigor. |
| symlinkevaluation`<symboliclinktype>` | Controla o tipo de links simbólicos que podem ser criados em um computador. As opções válidas são:<ul><li>**1** -local para links simbólicos locais,`L2L:{0|1}`</li><li>**2** -locais para links simbólicos remotos,`L2R:{1|0}`</li><li>**3** -links simbólicos remotos para locais,`R2R:{1|0}`</li><li>**4** -links simbólicos remotos para remotos,`R2L:{1|0}`</li></ul> |
| disabledeletenotify | Desabilita (**1**) ou habilita (**0**) notificações de exclusão. As notificações de exclusão (também conhecidas como Trim ou removem) são um recurso que notifica o dispositivo de armazenamento subjacente dos clusters que foram liberados devido a uma operação de exclusão de arquivo. Além disso:<ul><li>Para sistemas que usam ReFS v2, Trim é desabilitado por padrão.</li><li>Para sistemas que usam ReFS v1, o Trim é habilitado por padrão.</li><li>Para sistemas que usam NTFS, o Trim é habilitado por padrão, a menos que um administrador o desabilite.</li><li>Se a unidade de disco rígido ou a SAN reportar que não dá suporte a Trim, a unidade de disco rígido e as SANs não obterão notificações de corte.</li><li>Habilitar ou desabilitar não requer uma reinicialização.</li><li>Trim é eficaz quando o próximo comando de mapeamento é emitido.</li><li>A e/s de em andamento existentes não são afetadas pela alteração do registro.</li><li>Não requer nenhuma reinicialização de serviço quando você habilita ou desabilita o corte.</li></ul> |

#### <a name="remarks"></a>Comentários

- A zona MFT é uma área reservada que permite que a MFT (tabela de arquivos mestre) expanda conforme necessário para impedir a fragmentação de MFT. Se o tamanho médio do arquivo no volume for de 2 KB ou menos, poderá ser benéfico definir o valor de **mftzone** como **2**. Se o tamanho médio do arquivo no volume for de 1 KB ou menos, poderá ser benéfico definir o valor de **mftzone** como **4**.

- Quando **disable8dot3** é definido como **0**, sempre que você cria um arquivo com um nome de arquivo longo, o NTFS cria uma segunda entrada de arquivo que tem um nome de arquivo de comprimento de caractere 8,3. Quando o NTFS cria arquivos em um diretório, ele deve procurar os nomes de arquivo de comprimento de caractere 8,3 que estão associados aos nomes de arquivo longos. Esse parâmetro atualiza a chave do registro **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation** .

- O parâmetro **allowextchar** atualiza a chave do registro **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name** .

- O parâmetro **disablelastaccess** reduz o impacto de atualizações de log no carimbo de **data/hora do último acesso** em arquivos e diretórios. A desabilitação do último recurso de **tempo de acesso** melhora a velocidade do acesso ao arquivo e ao diretório. Esse parâmetro atualiza a chave do registro **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate** .

    **Observações:**

    - As consultas de **horário do último acesso** baseado em arquivo são precisas mesmo que todos os valores em disco não sejam atuais. O NTFS retorna o valor correto em consultas porque o valor preciso é armazenado na memória.

    - Uma hora é a quantidade máxima de tempo que o NTFS pode adiar a atualização do **último tempo de acesso** no disco. Se o NTFS atualizar outros atributos de arquivo, como **hora da última modificação**, e uma atualização de **hora do último acesso** estiver pendente, o NTFS atualizará o **horário do último acesso** com as outras atualizações sem impacto adicional no desempenho.

    - O parâmetro **disablelastaccess** pode afetar programas como backup e armazenamento remoto, que dependem desse recurso.

- O aumento da memória física nem sempre aumenta a quantidade de memória de pool paginável disponível para NTFS. Definir **memoryusage** como **2** gera o limite de memória paginável do pool. Isso poderá melhorar o desempenho se o sistema estiver abrindo e fechando vários arquivos no mesmo conjunto de arquivos e ainda não estiver usando grandes quantidades de memória do sistema para outros aplicativos ou para a memória cache. Se o computador já estiver usando grandes quantidades de memória do sistema para outros aplicativos ou para a memória de cache, aumentar o limite de memória de pool paginável e não paginado do NTFS reduzirá a memória de pool disponível para outros processos. Isso pode reduzir o desempenho geral do sistema. Esse parâmetro atualiza a chave do registro **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage** .

- O valor especificado no parâmetro **mftzone** é uma aproximação do tamanho inicial da MFT mais a zona MFT em um novo volume e é definido no momento da montagem para cada sistema de arquivos. Como o espaço no volume é usado, o NTFS ajusta o espaço reservado para o crescimento futuro da MFT. Se a zona MFT já for grande, o tamanho completo da zona MFT não será reservado novamente. Como a zona MFT é baseada no intervalo contíguo após o final do MFT, ela diminui conforme o espaço é usado.

    O sistema de arquivos não determina o novo local da zona MFT até que a zona MFT atual seja totalmente usada. Observe que isso nunca ocorre em um sistema típico.

- Alguns dispositivos podem apresentar degradação de desempenho quando o recurso de notificação de exclusão está ativado. Nesse caso, use a opção **disabledeletenotify** para desativar o recurso de notificação.

### <a name="examples"></a>Exemplos

Para consultar o comportamento de desabilitar 8dot3 Name para um volume de disco especificado com o GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, digite:

```
fsutil behavior query disable8dot3 volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Você também pode consultar o comportamento de nome 8dot3 usando o subcomando **8dot3name** .

Para consultar o sistema para ver se o TRIM está habilitado ou não, digite:

```
fsutil behavior query DisableDeleteNotify
```

Isso produz uma saída semelhante a esta:

```
NTFS DisableDeleteNotify = 1
ReFS DisableDeleteNotify is not currently set
```

Para substituir o comportamento padrão de TRIM (disabledeletenotify) para ReFS v2, digite:

```
fsutil behavior set disabledeletenotify ReFS 0
```

Para substituir o comportamento padrão de TRIM (disabledeletenotify) para NTFS e ReFS v1, digite:

```
fsutil behavior set disabledeletenotify 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [fsutil 8dot3name](fsutil-8dot3name.md)
