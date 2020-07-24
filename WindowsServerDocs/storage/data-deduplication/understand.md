---
ms.assetid: acc0803b-fa05-4fc3-b94d-2916abf4fdbd
title: Noções básicas da eliminação de duplicação de dados
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 54ad12c1631df39b6fdbe06c78e1a300ff5d73bd
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955598"
---
# <a name="understanding-data-deduplication"></a>Noções básicas da eliminação de duplicação de dados

> Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Este documento descreve como funciona a [Eliminação de Duplicação de Dados](overview.md).

## <a name="how-does-data-deduplication-work"></a><a name="how-does-dedup-work"></a>Como funciona a Eliminação de Duplicação de Dados?

A Eliminação de Duplicação de Dados no Windows Server foi criada com os dois princípios a seguir:

1. **A otimização não interferir as gravações no disco**  
    A eliminação de duplicação de dados otimiza os dados usando um modelo de pós-processamento. Todos os dados são gravados de forma não otimizada no disco e são otimizados posteriormente pela Eliminação de Duplicação de Dados.

2. **A otimização não deve alterar a semântica de acesso**  
    Os usuários e aplicativos que acessam dados em um volume otimizado não têm a menor a ideia de que os arquivos que eles estão acessando passaram pela eliminação de duplicação.

Uma vez habilitada para um volume, a eliminação de duplicação de dados é executada em segundo plano para:

- Identificar padrões repetidos em arquivos nesse volume.
- Mover continuamente esses fragmentos, ou partes, com ponteiros especial chamados [pontos de nova análise](#dedup-term-reparse-point) que apontam para uma cópia única nessa parte.

Isso ocorre em quatro etapas, descritas a seguir:

1. Verificação de arquivos que atendam à política de otimização no sistema de arquivos.  
![Verificação do sistema de arquivos](media/understanding-dedup-how-dedup-works-1.gif)  
2. Divisão dos arquivos em partes de tamanho variável.  
![Divisão dos arquivos em partes](media/understanding-dedup-how-dedup-works-2.gif)
3. Identificação de partes exclusivas.  
![Identificação de partes exclusivas](media/understanding-dedup-how-dedup-works-3.gif)
4. Inserção das partes no repositório de partes e, opcionalmente, compactação.  
![Movimentação para o armazenamento de partes](media/understanding-dedup-how-dedup-works-4.gif)
5. Substituição do fluxo de arquivos original dos arquivos agora otimizados com um ponto de nova análise para o repositório de partes.  
![Substituição do fluxo de arquivos com o ponto de nova análise](media/understanding-dedup-how-dedup-works-5.gif)

Quando os arquivos otimizados são lidos, o sistema de arquivos os envia com um ponto de nova análise ao filtro do sistema de arquivos de Eliminação de Duplicação de Dados (Dedup.sys). O filtro redireciona a operação de leitura para as partes apropriadas que compõem o fluxo para esse arquivo no repositório de partes. As modificações em intervalos de arquivos que passaram pela eliminação de duplicação são gravadas de forma não otimizada no disco e são otimizadas pelo [Trabalho de otimização](understand.md#job-info) da próxima vez em que ele for executado.

## <a name="usage-types"></a><a id="usage-type"></a>Tipos de uso
Os tipos de uso a seguir fornecem uma configuração razoável de Eliminação de Duplicação de Dados para cargas de trabalho comuns:  

| Tipo de uso | Cargas de trabalho ideais | Qual é a diferença |
|------------|-----------------|------------------|
| <a id="usage-type-default"></a>Padrão | Servidor de arquivos de finalidade geral:<ul><li>Compartilhamentos de equipe</li><li>Pastas de trabalho</li><li>Redirecionamento de pasta</li><li>Compartilhamentos de desenvolvimento de software</li></ul> | <ul><li>Otimização em segundo plano</li><li>Política de otimização padrão:<ul><li>Idade mínima do arquivo = 3 dias</li><li>Otimizar arquivos em uso = Não</li><li>Otimizar arquivos parciais = Não</li></ul></li></ul> |
| <a id="usage-type-hyperv"></a>Hyper-V | Servidores de VDI (Virtual Desktop Infrastructure) | <ul><li>Otimização em segundo plano</li><li>Política de otimização padrão:<ul><li>Idade mínima do arquivo = 3 dias</li><li>Otimizar arquivos em uso = Sim</li><li>Otimizar arquivos parciais = Sim</li></ul></li><li>Ajustes "nos bastidores" para interoperabilidade do Hyper-V</li></ul> |
| <a id="usage-type-backup"></a>Backup | Aplicativos de backup virtualizados, como o [Microsoft Data Protection Manager (DPM)](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)) | <ul><li>Otimização da prioridade</li><li>Política de otimização padrão:<ul><li>Idade mínima do arquivo = 0 dias</li><li>Otimizar arquivos em uso = Sim</li><li>Otimizar arquivos parciais = Não</li></ul></li><li>Ajustes "nos bastidores" para interoperabilidade com soluções de DPM ou semelhantes a DPM</li></ul> |

## <a name="jobs"></a><a id="job-info"></a>Trabalhos
A Eliminação de Duplicação de Dados usa uma estratégia de pós-processamento para otimizar e manter a eficiência do espaço de um volume.

| Nome do trabalho | Descrições do trabalho | Cronograma padrão |
|----------|------------------|------------------|
| <a id="job-info-optimization"></a>Otimização | O trabalho de **otimização** elimina a duplicação de dados em um volume de acordo com as configurações de política de volume, (opcionalmente) compactando essas partes e armazenando partes exclusivamente no repositório de partes. O processo de otimização usado pela Eliminação de Duplicação de Dados está descrito detalhadamente em [Como funciona a Eliminação de Duplicação de Dados?](understand.md#how-does-dedup-work). | Uma vez a cada hora |
| <a id="job-info-gc"></a>Coleta de lixo | O trabalho **Coleta de lixo** recupera o espaço em disco removendo partes desnecessárias que não estão mais sendo referenciadas por arquivos modificados ou excluídos recentemente. | Todo sábado às 2h35 |
| <a id="job-info-scrubbing"></a>Depuração de integridade | O trabalho **Depuração de integridade** identifica danos no repositório de partes devido a falhas de disco ou setores inválidos. Quando possível, a Eliminação de Duplicação de Dados pode usar automaticamente os recursos do volume (como espelhamento ou paridade em um volume de Espaços de Armazenamento) para reconstruir os dados corrompidos. Além disso, a Eliminação de Duplicação de Dados guarda cópias de backup das partes populares quando elas são referenciadas mais de 100 vezes em uma área denominada ponto de acesso. | Todo sábado às 3h35 |
| <a id="job-info-unoptimization"></a>Cancelamento da otimização | O trabalho **Cancelamento da otimização**, um trabalho especial que só pode ser executado manualmente, desfaz a otimização feita pela eliminação de duplicação e desabilita a Eliminação de Duplicação de Dados nesse volume. | [Somente sob demanda](run.md#disabling-dedup) |

## <a name="data-deduplication-terminology"></a><a id="dedup-term"></a>Terminologia de eliminação de duplicação de dados
| Termo | Definição |
|------|------------|
| <a id="dedup-term-chunk"></a>Chunk | Uma parte é uma seção de um arquivo que foi selecionada pelo algoritmo de fragmentação da Eliminação de Duplicação de Dados como algo provável de ocorrer em outros arquivos semelhantes. |
| <a id="dedup-term-chunk-store"></a>Repositório de partes | O repositório de partes é uma série organizada de arquivos de contêiner na pasta System Volume Information que a Eliminação de Duplicação de Dados usa para armazenar partes de forma exclusiva. |
| <a id="dedup-term-dedup"></a>Dedup | Uma abreviação da Eliminação de Duplicação de Dados usada normalmente no PowerShell, em APIs e componentes do Windows Server e na comunidade do Windows Server. |
| <a id="dedup-term-file-metadata"></a>Metadados de arquivos | Cada arquivo contém metadados que descrevem propriedades interessantes sobre o arquivo que não estão relacionadas ao conteúdo principal do arquivo. Por exemplo, Data de criação, Data da última leitura, Autor, etc. |
| <a id="dedup-term-file-stream"></a>Fluxo de arquivos | O fluxo de arquivos é o conteúdo principal do arquivo. Essa é a parte do arquivo que otimiza a Eliminação de Duplicação de Dados. |
| <a id="dedup-term-file-system"></a>Sistema de arquivos | O sistema de arquivos é a estrutura de dados em disco e em software que o sistema operacional usa para armazenar arquivos na mídia de armazenamento. Há suporte para a Eliminação de Duplicação de Dados em volumes formatados em NTFS. |
| <a id="dedup-term-file-system-filter"></a>Filtro do sistema de arquivos | Um filtro de sistema de arquivos é um plug-in que modifica o comportamento padrão do sistema de arquivos. Para preservar a semântica de acesso, a Eliminação de Duplicação de Dados usa um filtro de sistema de arquivos (Dedup.sys) para redirecionar leituras ao conteúdo otimizado de forma totalmente transparente para o usuário ou aplicativo que faz a solicitação de leitura. |
| <a id="dedup-term-optimization"></a>Otimização | Um arquivo será considerado otimizado (ou com duplicação eliminada) pela Eliminação de Duplicação de Dados se tiver sido fragmentado e suas partes exclusivas tiverem sido armazenadas no repositório de partes. |
| <a id="dedup-term-in-policy"></a>Política de otimização | A política de otimização especifica os arquivos que devem ser considerados para Eliminação de Duplicação de Dados. Por exemplo, os arquivos poderão ser considerados fora da política se forem totalmente novos, estiverem abertos, em um determinado caminho no volume ou se forem de um determinado tipo de arquivo. |
| <a id="dedup-term-reparse-point"></a>Ponto de nova análise | Um [ponto de nova análise](/windows/win32/fileio/reparse-points) é uma marca especial que notifica o sistema de arquivos para passar a e/s para um filtro do sistema de arquivos especificado. Quando o fluxo de arquivos do arquivo tiver sido otimizado, a Eliminação de Duplicação de Dados substitui o fluxo de arquivos por um ponto de nova análise, que permite à Eliminação de Duplicação de Dados preservar a semântica de acesso nesse arquivo. |
| <a id="dedup-term-volume"></a>Volume | Um volume é uma construção do Windows para uma unidade de armazenamento lógico que pode abranger vários dispositivos de armazenamento físicos em um ou mais servidores. A Eliminação de duplicação é habilitada de acordo com o volume. |
| <a id="dedup-term-workload"></a>Carga de trabalho | Uma carga de trabalho é um aplicativo executado no Windows Server. Entre os exemplos de carga de trabalho de exemplo estão o servidor de arquivos de finalidade geral, Hyper-V e o SQL Server. |

> [!Warning]  
> A menos que seja indicado pela Equipe de suporte autorizada da Microsoft, não tente modificar manualmente o repositório de partes. Isso pode resultar em corrupção ou perda de dados.

## <a name="frequently-asked-questions"></a>Perguntas frequentes
**Qual é a diferença entre a Eliminação de Duplicação de Dados e os outros produtos de otimização?**  
Há várias diferenças importantes entre a Eliminação de Duplicação de Dados e outros produtos de otimização de armazenamento comuns:

* *Qual é a diferença entre a Eliminação de Duplicação de Dados e o Single Instance Store?*  
    O Single Instance Store, ou o SIS, é uma tecnologia anterior à Eliminação de Duplicação de Dados e foi introduzida pela primeira vez no Windows Storage Server 2008 R2. Para otimizar um volume, o Single Instance Store identifica os arquivos completamente idênticos e os substitui por links lógico em uma única cópia de um arquivo armazenado no armazenamento comum do SIS. Ao contrário do Single Instance Store, a Eliminação de Duplicação de Dados pode conseguir uma economia de espaço dos arquivos que não são idênticos, mas que compartilham muitos padrões comuns, e de arquivos que contêm vários padrões repetidos. O Single Instance Store foi preterido no Windows Server 2012 R2 e removido no Windows Server 2016 em favor da Eliminação de Duplicação de Dados.

* *Qual é a diferença entre a Eliminação de Duplicação de Dados e a compactação NTFS?*  
    A compactação NTFS é um recurso do NTFS que você pode habilitar opcionalmente no nível do volume. Com a compactação NTFS, cada arquivo é otimizado individualmente por meio de compactação no momento da gravação. Ao contrário da compactação NTFS, a Eliminação de Duplicação de Dados pode conseguir economia de espaço em todos os arquivos em um volume. Isso é melhor do que a compactação NTFS, porque <u>ambos</u> os arquivos podem ter uma duplicação interna (solucionada pela compactação NTFS) e ter semelhanças com outros arquivos no volume (não solucionado pela compactação NTFS). Além disso, a Eliminação de Duplicação de Dados tem um modelo de pós-processamento, o que significa que arquivos novos ou modificados serão gravadas em disco de forma não otimizada e serão otimizados posteriormente pela Eliminação de Duplicação de Dados.

* *Como a eliminação de duplicação de dados difere dos formatos de arquivo mortos como zip, rar, 7z, CAB, etc.?*  
    Os formatos de arquivo morto zip, rar, 7z, cab, etc. executam a compactação em um conjunto especificado de arquivos. Como a Eliminação de Duplicação de Dados, padrões duplicados dentro de arquivos e padrões duplicados em arquivos são otimizados. No entanto, você precisa escolher os arquivos que você deseja incluir no arquivo morto. A semântica de acesso também é diferente. Para acessar um arquivo específico dentro do arquivo morto, você precisa abrir o arquivo morto, selecionar um arquivo específico e descompactar esse arquivo para uso. A Eliminação de Duplicação de Dados opera de forma transparente para usuários e administradores e não exige qualquer inicialização manual. Além disso, a Eliminação de Duplicação de Dados preserva a semântica de acesso: os arquivos otimizados aparecem inalterados após a otimização.

**Posso alterar as configurações de Eliminação de Duplicação de Dados para o meu tipo de uso selecionado?**  
Sim. Embora a Eliminação de Duplicação de Dados forneça padrões razoáveis para **Cargas de trabalho recomendadas**, ainda convém ajustar as configurações da Eliminação de Duplicação de Dados para obter o máximo proveito de seu armazenamento. Além disso, outras cargas de trabalho [exigirão alguns ajustes para garantir que a Eliminação de Duplicação de Dados não interfira com a carga de trabalho](install-enable.md#enable-dedup-sometimes-considerations).

**Posso executar manualmente um trabalho de Eliminação de Duplicação de Dados?**  
Sim, [todos os trabalhos de Eliminação de Duplicação de Dados podem ser executados manualmente](run.md#running-dedup-jobs-manually). Isso pode ser desejável se os trabalhos agendados não tiverem sido executados por falta de recursos do sistema ou por um erro. Além disso, o cancelamento da otimização do trabalho pode ser executado manualmente.

**Posso monitorar os resultados históricos dos trabalhos de Eliminação de Duplicação de Dados?**  
Sim, [todos os trabalhos de Eliminação de Duplicação de Dados criam entradas no Log de Eventos do Windows](run.md#monitoring-dedup).

**Posso alterar as agendas padrão para os trabalhos de Eliminação de Duplicação de Dados em meu sistema?**  
Sim, [todas as agendas são configuráveis](advanced-settings.md#modifying-job-schedules). A modificação das agendas padrão da Eliminação de Duplicação de Dados é especialmente desejável para garantir que os trabalhos da Eliminação de Duplicação de Dados tenham tempo para concluir e não compitam por recursos com a carga de trabalho.
