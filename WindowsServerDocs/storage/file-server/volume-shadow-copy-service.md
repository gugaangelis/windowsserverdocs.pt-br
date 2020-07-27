---
title: Serviço de Cópias de Sombra de Volume
ms.date: 01/30/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 9948fab77ab4869c27fd63e623315bd1b3e9ff47
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966688"
---
# <a name="volume-shadow-copy-service"></a>Serviço de Cópias de Sombra de Volume

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

O backup e a restauração de dados comercialmente críticos podem ser muito complexos devido aos seguintes problemas:

  - Os dados geralmente precisam de backup, enquanto os aplicativos que produzem os dados ainda estão em execução. Isso significa que alguns dos arquivos de dados podem estar abertos ou podem estar em um estado inconsistente.

  - Se o conjunto de dados for grande, poderá ser difícil fazer backup de todos eles ao mesmo tempo.


A execução correta de operações de backup e restauração exige uma forte coordenação entre os aplicativos de backup, os aplicativos de linha de negócios que estão sendo submetidos a backup e o hardware e o software de gerenciamento de armazenamento. O Serviço de Cópias de Sombra de Volume (VSS), que foi introduzido no Windows Server® 2003 facilita a conversa entre esses componentes para permitir que eles funcionem melhor juntos. Quando todos os componentes são compatíveis com o VSS, você pode usá-los para fazer backup dos dados do aplicativo sem colocar os aplicativos offline.

O VSS coordena as ações necessárias para criar uma cópia de sombra consistente (também conhecida como um instantâneo ou uma cópia pontual) dos dados cujo backup será feito. A cópia de sombra pode ser usada no estado em que se encontra ou em cenários como o seguinte:

  - Você deseja fazer backup de dados do aplicativo e de informações de estado do sistema, incluindo o arquivamento de dados em outra unidade de disco rígido, em fita ou em outra mídia removível.

  - Você está realizando a mineração de dados.

  - Você está executando backups de disco para disco.

  - Você precisa de uma recuperação rápida da perda de dados restaurando os dados para o LUN original ou para um LUN totalmente novo que substitua um LUN original que falhou.


Os recursos e aplicativos do Windows que usam o VSS incluem o seguinte:

  - [Backup do Windows Server](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkId=180891)

  - [Cópias de sombra de pastas compartilhadas](https://go.microsoft.com/fwlink/?linkid=142874) (https://go.microsoft.com/fwlink/?LinkId=142874)

  - [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=180892) (https://go.microsoft.com/fwlink/?LinkId=180892)

  - [Restauração do Sistema](https://go.microsoft.com/fwlink/?linkid=180893) (https://go.microsoft.com/fwlink/?LinkId=180893)


## <a name="how-volume-shadow-copy-service-works"></a>Como o Serviço de Cópias de Sombra de Volume funciona

Uma solução completa do VSS requer todas as seguintes partes básicas:

**Serviço VSS**   Parte do sistema operacional Windows que garante que os outros componentes consigam se comunicar entre si corretamente e funcionar juntos.

**Solicitante do VSS**   O software que solicita a criação real de cópias de sombra (ou outras operações de alto nível, como importar ou excluir as cópias). Normalmente, esse é o aplicativo de backup. O utilitário Backup do Windows Server e o aplicativo System Center Data Protection Manager são solicitantes do VSS. Solicitantes do VSS que não são da Microsoft® incluem quase todos os programas de software de backup executados no Windows.

**Gravador VSS**   O componente que garante que temos um conjunto de dados consistente para fazer backup. Normalmente, isso é fornecido como parte de um aplicativo de linha de negócios, como SQL Server® ou Exchange Server. Os gravadores VSS para vários componentes do Windows, como o Registro, estão incluídos no sistema operacional Windows. Os gravadores VSS não Microsoft são incluídos em muitos aplicativos para Windows que precisam garantir a consistência dos dados durante o backup.

**Provedor VSS**   O componente que cria e mantém as cópias de sombra. Isso pode ocorrer no software ou no hardware. O sistema operacional Windows inclui um provedor VSS que usa cópia em gravação. Se você usa uma SAN (rede de área de armazenamento), é importante instalar o provedor de hardware VSS para a SAN, se houver um. Um provedor de hardware descarrega a tarefa de criar e manter uma cópia de sombra do sistema operacional do host.

O diagrama a seguir ilustra como o serviço VSS coordena-se com solicitantes, gravadores e provedores para criar uma cópia de sombra de um volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   Diagrama arquitetônico do Serviço de Cópias de Sombra de Volume

### <a name="how-a-shadow-copy-is-created"></a>Como uma cópia de sombra é criada

Esta seção coloca as várias funções do solicitante, do gravador e do provedor em contexto, listando as etapas que precisam ser executadas para criar uma cópia de sombra. O diagrama a seguir mostra como o Serviço de Cópias de Sombra de Volume controla a coordenação geral do solicitante, do gravador e do provedor.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figura 2** Processo de criação de cópia de sombra

Para criar uma cópia de sombra, o solicitante, o gravador e o provedor executam as seguintes ações:

1.  O solicitante solicita que o Serviço de Cópias de Sombra de Volume enumere os gravadores, reúna os metadados do gravador e prepare-se para a criação da cópia de sombra.

2.  Cada gravador cria uma descrição XML dos componentes e armazenamentos de dados que precisam de backup e os fornece ao Serviço de Cópias de Sombra de Volume. O gravador também define um método de restauração, que é usado para todos os componentes. O Serviço de Cópias de Sombra de Volume fornece a descrição do gravador para o solicitante, que seleciona os componentes que serão submetidos a backup.

3.  O Serviço de Cópias de Sombra de Volume notifica todos os gravadores para preparar seus dados para fazer uma cópia de sombra.

4.  Cada gravador prepara os dados conforme apropriado, como a conclusão de todas as transações abertas, os logs de transações sem interrupção e a liberação de caches. Quando os dados estiverem prontos para serem copiados em sombra, o gravador notificará a Serviço de Cópias de Sombra de Volume.

5.  O Serviço de Cópias de Sombra de Volume informa os gravadores para congelar temporariamente as solicitações de E/S de gravação de aplicativos (as solicitações de E/S de leitura ainda são possíveis) pelos poucos segundos necessários para criar a cópia de sombra dos volumes. O congelamento do aplicativo não tem permissão para demorar mais de 60 segundos. O Serviço de Cópias de Sombra de Volume libera os buffers do sistema de arquivos e, em seguida, congela o sistema de arquivos, o que garante o registro correto dos metadados do sistema de arquivos e a gravação em uma ordem consistente dos dados a serem copiados por sombra.

6.  O Serviço de Cópias de Sombra de Volume informa ao provedor para criar a cópia de sombra. O período de criação da cópia de sombra não dura mais de 10 segundos e, nesse período, todas as solicitações de E/S de gravação para o sistema de arquivos permanecem congeladas.

7.  O Serviço de Cópias de Sombra de Volume libera solicitações de E/S de gravação do sistema de arquivos.

8.  O VSS informa os gravadores para descongelar as solicitações de E/S de gravação do aplicativo. Neste ponto, os aplicativos são livres para retomar a gravação de dados no disco que está sendo copiado por sombra.


> [!NOTE]
> A criação da cópia de sombra poderá ser anulada se os gravadores forem mantidos no estado de congelamento por mais de 60 segundos ou se os provedores demorarem mais de 10 segundos para confirmar a cópia de sombra.
<br>

9. O solicitante pode repetir o processo (volte para a etapa 1) ou notificar o administrador para tentar novamente mais tarde.

10. Se a cópia de sombra for criada com êxito, o Serviço de Cópias de Sombra de Volume retornará as informações de localização para a cópia de sombra para o solicitante. Em alguns casos, a cópia de sombra pode ser temporariamente disponibilizada como um volume de leitura/gravação para que o VSS e um ou mais aplicativos possam alterar o conteúdo da cópia de sombra antes da conclusão da cópia de sombra. Depois que o VSS e os aplicativos fizerem suas alterações, a cópia de sombra passará a ser somente leitura. Essa fase é chamada de recuperação automática e é usada para desfazer qualquer transação dedo aplicativo ou sistema de arquivos no volume de cópia de sombra que não tenha sido concluída antes da criação da cópia de sombra.


### <a name="how-the-provider-creates-a-shadow-copy"></a>Como o provedor cria uma cópia de sombra

Um provedor de cópia de sombra de hardware ou software usa um dos seguintes métodos para criar uma cópia de sombra:

**Concluir a cópia**   Esse método faz uma cópia completa (chamada de "cópia completa" ou "clone") do volume original em um determinado momento. Esta cópia é somente leitura.

**Cópia em gravação**   Esse método não copia o volume original. Em vez disso, ele faz uma cópia diferencial copiando todas as alterações (solicitações de E/S de gravação concluídas) que são feitas no volume após um determinado ponto no tempo.

**Redirecionamento na gravação**   Esse método não copia o volume original e não faz nenhuma alteração no volume original após um determinado ponto no tempo. Em vez disso, faz uma cópia diferencial redirecionando todas as alterações para um volume diferente.

## <a name="complete-copy"></a>Cópia completa

Uma cópia completa geralmente é criada fazendo um "espelho dividido" da seguinte maneira:

1. O volume original e o volume da cópia de sombra são um conjunto de volumes espelhados.

2. O volume da cópia de sombra é separado do volume original. Isso interrompe a conexão espelho.


Após a interrupção da conexão espelho, o volume original e o volume da cópia de sombra são independentes. O volume original continua aceitando todas as alterações (solicitações de E/S de gravação), enquanto o volume da cópia de sombra continua sendo uma cópia exata somente leitura dos dados originais no momento da interrupção.

### <a name="copy-on-write-method"></a>Método de cópia na gravação

No método de cópia na gravação, quando ocorre uma alteração no volume original (mas antes da conclusão da solicitação de E/S de gravação), cada bloco a ser modificado é lido e gravado na área de armazenamento de cópia de sombra do volume (também chamada de "área de comparação"). A área de armazenamento de cópia de sombra pode estar no mesmo volume ou em um volume diferente. Isso preserva uma cópia do bloco de dados no volume original antes que a alteração a substitua.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Hora</th>
<th>Dados de origem (status e dados)</th>
<th>Cópia de sombra (status e dados)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dados originais: 1 2 3 4 5</p></td>
<td><p>Sem cópia: –</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dados alterados no cache: 3 a 3'</p></td>
<td><p>Cópia de sombra criada (somente diferenças): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dados originais substituídos: 1 2 3' 4 5</p></td>
<td><p>Diferenças e índice armazenados na cópia de sombra: 3</p></td>
</tr>
</tbody>
</table>

**Tabela 1**   O método de cópia na gravação de criação de cópias de sombra

O método d cópia em gravação é um método rápido para criar uma cópia de sombra, pois copia apenas os dados que mudaram. Os blocos copiados na área de comparação podem ser combinados com os dados alterados no volume original para restaurar o volume para seu estado antes que qualquer uma das alterações tenha sido feita. Se houver muitas alterações, o método de cópia na gravação poderá se tornar caro.

### <a name="redirect-on-write-method"></a>Método de redirecionamento na gravação

No método de redirecionamento na gravação, sempre que o volume original recebe uma alteração (solicitação de E/S de gravação), a alteração não é aplicada ao volume original. Em vez disso, ela é gravada na área de armazenamento de cópia de sombra de outro volume.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Hora</th>
<th>Dados de origem (status e dados)</th>
<th>Cópia de sombra (status e dados)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dados originais: 1 2 3 4 5</p></td>
<td><p>Sem cópia: –</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dados alterados no cache: 3 a 3'</p></td>
<td><p>Cópia de sombra criada (somente diferenças): 3'</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dados originais inalterados: 1 2 3 4 5</p></td>
<td><p>Diferenças e índice armazenados na cópia de sombra: 3'</p></td>
</tr>
</tbody>
</table>

**Tabela 2**   O método de redirecionamento na gravação de criação de cópias de sombra

Assim como o método de cópia na gravação, o método de redirecionamento na gravação é um método rápido para criar uma cópia de sombra, pois copia apenas as alterações aos dados. Os blocos copiados na área de comparação podem ser combinados com os dados inalterados no volume original para criar uma cópia completa e atualizada dos dados. Se houver muitas solicitações de E/S de leitura, o método de redirecionamento na gravação poderá se tornar caro.

## <a name="shadow-copy-providers"></a>Provedores de cópia de sombra

Há dois tipos de provedores de cópia de sombra: provedores baseados em hardware e provedores baseados em software. Há também um provedor de sistema, que é um provedor de software interno do sistema operacional Windows.

### <a name="hardware-based-providers"></a>Provedores baseados em hardware

Os provedores de cópia de sombra baseados em hardware atuam como uma interface entre o Serviço de Cópias de Sombra de Volume e o nível de hardware, trabalhando em conjunto com um adaptador ou controlador de armazenamento de hardware. O trabalho de criar e manter a cópia de sombra é executado pela matriz de armazenamento.

Os provedores de hardware sempre usam a cópia de sombra de um LUN inteiro, mas o Serviço de Cópias de Sombra de Volume expõe apenas a cópia de sombra dos volumes que foram solicitados.

Um provedor de cópia de sombra baseado em hardware usa a funcionalidade de Serviço de Cópias de Sombra de Volume que define o ponto no tempo, permite a sincronização de dados, gerencia a cópia de sombra e fornece uma interface comum com os aplicativos de backup. No entanto, o Serviço de Cópias de Sombra de Volume não especifica o mecanismo subjacente que o provedor baseado em hardware usa para produzir e manter cópias de sombra.

### <a name="software-based-providers"></a>Provedores baseados em software

Os provedores de cópia de sombra baseados em software normalmente interceptam e processam solicitações de E/S de leitura e gravação em uma camada de software entre o sistema de arquivos e o software do gerenciador de volumes.

Esses provedores são implementados como um componente DLL do modo de usuário e pelo menos um driver de dispositivo em modo kernel, normalmente um driver de filtro de armazenamento. Diferentemente dos provedores baseados em hardware, os provedores baseados em software criam cópias de sombra no nível do software, não no nível do hardware.

Um provedor de cópia de sombra baseado em software deve manter uma exibição "pontual" de um volume, tendo acesso a um conjunto de dados que pode ser usado para recriar o status do volume antes do momento da criação da cópia de sombra. Um exemplo é a técnica de cópia em gravação do provedor do sistema. Porém, o Serviço de Cópias de Sombra de Volume não impõe restrições à técnica que os provedores baseados em software usam para criar e manter cópias de sombra.

Um provedor de software é aplicável a uma variedade maior de plataformas de armazenamento do que um provedor baseado em hardware e ele deve funcionar igualmente bem com discos básicos ou volumes lógicos. (Um volume lógico é um volume que é criado pela combinação de espaço livre de dois ou mais discos.) Em contraste com as cópias de sombra de hardware, os provedores de software consomem recursos do sistema operacional para manter a cópia de sombra.

Para obter mais informações sobre discos básicos, confira [O que são discos e volumes básicos?](https://go.microsoft.com/fwlink/?linkid=180894) (https://go.microsoft.com/fwlink/?LinkId=180894) no TechNet.

### <a name="system-provider"></a>Provedor do sistema

Um provedor de cópia de sombra, o provedor do sistema, é fornecido no sistema operacional Windows. Embora um provedor padrão seja fornecido no Windows, outros fornecedores são livres para fornecer implementações otimizadas para seus aplicativos de hardware e software de armazenamento.

Para manter a exibição "pontual" de um volume contido em uma cópia de sombra, o provedor do sistema usa uma técnica de cópia em gravação. Cópias dos blocos no volume que foram modificados desde o início da criação da cópia de sombra são armazenadas em uma área de armazenamento de cópia de sombra.

O provedor do sistema pode expor o volume de produção, que pode ser gravado e lido normalmente. Quando a cópia de sombra é necessária, ela aplica logicamente as diferenças aos dados no volume de produção para expor a cópia de sombra completa.

Para o provedor do sistema, a área de armazenamento de cópia de sombra deve estar em um volume NTFS. O volume a ser copiado em sombra não precisa ser um volume NTFS, mas pelo menos um volume montado no sistema deve ser um volume NTFS.

Os arquivos de componente que compõem o provedor de sistema são swprv.dll e volsnap.sys.

### <a name="in-box-vss-writers"></a>Gravadores do VSS In-Box

O sistema operacional Windows inclui um conjunto de gravadores do VSS responsáveis por enumerar os dados exigidos por vários recursos do Windows.

Para obter mais informações sobre esses gravadores, confira a seguinte página da Web do Microsoft Docs:

- [Gravadores do VSS In-Box](/windows/win32/vss/in-box-vss-writers) (https://docs.microsoft.com/windows/win32/vss/in-box-vss-writers)


## <a name="how-shadow-copies-are-used"></a>Como as cópias de sombra são usadas

Além de fazer backup de dados de aplicativos e informações de estado do sistema, as cópias de sombra podem ser usadas para várias finalidades, incluindo:

  - Como restaurar LUNs (ressincronização de LUN e troca de LUN)

  - Como restaurar arquivos individuais (Cópias de Sombra para Pastas Compartilhadas)

  - Mineração de dados usando cópias de sombra transportáveis


### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Como restaurar LUNs (ressincronização de LUN e troca de LUN)

No Windows Server 2008 R2 e no Windows 7, os solicitantes do VSS podem usar um recurso de provedor de cópias de sombra de hardware chamado de "LUN Resync" (ressincronização de LUN). Esse é um esquema de recuperação rápida que permite que um administrador de aplicativos restaure dados de uma cópia de sombra para o LUN original ou para um LUN novo.

A cópia de sombra pode ser um clone completo ou uma cópia de sombra diferencial. Em ambos os casos, no final da operação de ressincronização, o LUN de destino terá o mesmo conteúdo que o LUN de cópia de sombra. Durante a operação de ressincronização, a matriz executa uma cópia no nível do bloco da cópia de sombra para o LUN de destino.


> [!NOTE]
> A cópia de sombra deve ser uma cópia de sombra de hardware transportável.
<br>


A maioria das matrizes permite que as operações de E/S de produção sejam retomadas logo após o início da operação de ressincronização. Enquanto a operação de ressincronização está em andamento, as solicitações de leitura são redirecionadas para o LUN de cópia de sombra e gravam solicitações para o LUN de destino. Isso permite que as matrizes recuperem conjuntos de dados muito grandes e retomem as operações normais em vários segundos.

A ressincronização de LUN é diferente da troca de LUN. Uma troca de LUN é um cenário de recuperação rápida compatível do VSS desde o Windows Server 2003 SP1. Em uma troca de LUN, a cópia de sombra é importada e, em seguida, convertida em um volume de leitura/gravação. A conversão é uma operação irreversível e o volume e o LUN subjacentes não podem ser controlados com as APIs do VSS depois disso. A seguinte lista descreve como a ressincronização de LUN se compara com a troca de LUN:

  - Na ressincronização de LUN, a cópia de sombra não é alterada, portanto, pode ser usada várias vezes. Na troca de LUN, a cópia de sombra pode ser usada apenas uma vez para uma recuperação. Para os administradores mais preocupados com segurança, isso é importante. Quando a ressincronização de LUN é usada, o solicitante pode repetir toda a operação de restauração se algo dá errado na primeira vez.

  - No final de uma troca de LUN, o LUN da cópia de sombra é usado para solicitações de E/S de produção. Por esse motivo, o LUN de cópia de sombra deve usar a mesma qualidade de armazenamento que o LUN de produção original para garantir que o desempenho não seja afetado após a operação de recuperação. Se a ressincronização de LUN for usada em vez disso, o provedor de hardware poderá manter a cópia de sombra no armazenamento que tem menor custo do que o armazenamento de qualidade de produção.

  - Se o LUN de destino não puder ser usado e precisar ser recriado, a troca de LUN poderá ser mais econômica porque não requer um LUN de destino.


> [!WARNING]
> Todas as operações listadas são no nível de LUN. Se você tentar recuperar um volume específico usando a ressincronização de LUN, reverterá inadvertidamente todos os outros volumes que estão compartilhando o LUN.
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Como restaurar arquivos individuais (Cópias de Sombra para Pastas Compartilhadas)

O recurso de Cópias de Sombra para Pastas Compartilhadas usa o Serviço de Cópias de Sombra de Volume para fornecer cópias pontuais de arquivos localizados em um recurso de rede compartilhado, como um servidor de arquivos. Com o Cópias de Sombra para Pastas Compartilhadas, os usuários podem recuperar rapidamente os arquivos excluídos ou alterados que estão armazenados na rede. Como eles podem fazer isso sem a assistência do administrador, o recurso de Cópias de Sombra para Pastas Compartilhadas pode aumentar a produtividade e reduzir os custos administrativos.

Para obter mais informações sobre o recurso de Cópias de Sombra para Pastas Compartilhadas, confira [Cópias de Sombra para Pastas Compartilhadas](https://go.microsoft.com/fwlink/?linkid=180898) (https://go.microsoft.com/fwlink/?LinkId=180898) no TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Mineração de dados usando cópias de sombra transportáveis

Com um provedor de hardware projetado para uso com o Serviço de Cópias de Sombra de Volume, você pode criar cópias de sombra transportáveis que podem ser importadas para servidores no mesmo subsistema (por exemplo, uma SAN). Essas cópias de sombra podem ser usadas para propagar uma instalação de produção ou de teste com os dados somente leitura para mineração de dados.

Com o Serviço de Cópias de Sombra de Volume e uma matriz de armazenamento com um provedor de hardware projetado para uso com o Serviço de Cópias de Sombra de Volume, é possível criar uma cópia de sombra do volume de dados de origem em um servidor e, em seguida, importar a cópia de sombra para outro servidor (ou de volta para o mesmo servidor). Esse processo é realizado em alguns minutos, independentemente do tamanho dos dados. O processo de transporte é realizado por meio de uma série de etapas que usam um solicitante de cópia de sombra (um aplicativo de gerenciamento de armazenamento) compatível com cópias de sombra transportáveis.

## <a name="to-transport-a-shadow-copy"></a>Para transportar uma cópia de sombra

1.  Crie uma cópia de sombra transportável usando os dados de origem em um servidor.

2.  Importe a cópia de sombra para um servidor que esteja conectado à SAN (você pode importar para um servidor diferente ou para o mesmo servidor).

3.  Os dados agora estão prontos para uso.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   Criação e transporte da cópia de sombra entre dois servidores


> [!NOTE]
> Uma cópia de sombra transportável criada no Windows Server 2003 não pode ser importada para um servidor que está executando o Windows Server 2008 ou o Windows Server 2008 R2. Uma cópia de sombra transportável criada no Windows Server 2008 ou no Windows Server 2008 R2 não pode ser importada para um servidor que está executando o Windows Server 2003. No entanto, uma cópia de sombra criada no Windows Server 2008 pode ser importada para um servidor que está executando o Windows Server 2008 R2 e vice-versa.
<br>


As cópias de sombra são somente leitura. Se você quiser converter uma cópia de sombra em um LUN de leitura/gravação, poderá usar um aplicativo de gerenciamento de armazenamento baseado em serviço de disco virtual (incluindo alguns solicitantes) além do Serviço de Cópias de Sombra de Volume. Ao usar esse aplicativo, você pode remover a cópia de sombra do gerenciamento de Serviço de Cópias de Sombra de Volume e convertê-la em um LUN de leitura/gravação.

O transporte do Serviço de Cópias de Sombra de Volume é uma solução avançada em computadores que executam o Windows Server 2003 Enterprise Edition, o Windows Server 2003 Datacenter Edition, o Windows Server 2008 ou o Windows Server 2008 R2. Ele só funcionará se houver um provedor de hardware na matriz de armazenamento. O transporte de cópia de sombra pode ser usado para várias finalidades, incluindo backups em fita, mineração de dados e teste.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

Estas perguntas frequentes respondem a perguntas sobre o VSS (Serviço de Cópias de Sombra de Volume) para administradores do sistema. Para obter informações sobre interfaces de programação de aplicativo VSS, confira [Serviço de Cópias de Sombra de Volume](https://go.microsoft.com/fwlink/?linkid=180899) (https://go.microsoft.com/fwlink/?LinkId=180899) na Biblioteca do Centro de Desenvolvedores do Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Quando o Serviço de Cópias de Sombra de Volume foi introduzido? Em quais versões do sistema operacional Windows ele está disponível?

O VSS foi introduzido no Windows XP. Ele está disponível no Windows XP, no Windows Server 2003, no Windows Vista®, no Windows Server 2008, no Windows 7 e no Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>Qual é a diferença entre uma cópia de sombra e um backup?

No caso de um backup da unidade de disco rígido, a cópia de sombra criada também é o backup. Os dados podem ser copiados para da cópia de sombra de uma restauração ou a cópia de sombra pode ser usada para um cenário de recuperação rápida, por exemplo, ressincronização de LUN ou troca de LUN.

Quando os dados são copiados da cópia de sombra para uma fita ou de outra mídia removível, o conteúdo armazenado na mídia constitui o backup. A cópia de sombra em si pode ser excluída depois que os dados são copiados dela.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>Qual é o maior tamanho de volume compatível com o Serviço de Cópias de Sombra de Volume?

O Serviço de Cópias de Sombra de Volume é compatível com um tamanho de volume de até 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>Fiz um backup no Windows Server 2008. Posso restaurá-lo no Windows Server 2008 R2?

Depende do software de backup que você usou. A maioria dos programas de backup é compatível com esse cenário para dados, mas não para backups de estado do sistema.

As cópias de sombra criadas em qualquer uma dessas versões do Windows podem ser usadas no outro.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>Fiz um backup no Windows Server 2003. Posso restaurá-lo no Windows Server 2008?

Depende do software de backup que você usou. Se você criar uma cópia de sombra no Windows Server 2003, não poderá usá-la no Windows Server 2008. Também, se você criar uma cópia de sombra no Windows Server 2008, não poderá restaurá-la no Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Como posso desabilitar o VSS?

É possível desabilitar o Serviço de Cópias de Sombra de Volume usando o Console de Gerenciamento Microsoft. No entanto, você não deve fazer isso. Desabilitar o VSS afeta negativamente qualquer software que você usa que depende dele, como a Restauração do Sistema e o Backup do Windows Server.

Para obter mais informações, confira os seguintes sites do Microsoft TechNet:

- [Restauração do Sistema](https://go.microsoft.com/fwlink/?linkid=157113) (https://go.microsoft.com/fwlink/?LinkID=157113)

- [Backup do Windows Server](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkID=180891)


### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>Posso excluir arquivos de uma cópia de sombra para economizar espaço?

O VSS foi projetado para criar cópias de sombra de volumes inteiros. Arquivos temporários, como arquivos de paginação, são omitidos automaticamente de cópias de sombra para economizar espaço.

Para excluir arquivos específicos de cópias de sombra, use a seguinte chave do Registro: **FilesNotToSnapshot**.


> [!NOTE]
> A chave do Registro <STRONG>FilesNotToSnapshot</STRONG> deve ser usada somente por aplicativos. Os usuários que tentarem usá-lo encontrarão limitações, como as seguintes:
> <br>
> <UL>
> <LI>Ela não pode excluir arquivos de uma cópia de sombra criada em um Windows Server usando o recurso Versões Anteriores.<BR><BR>
> <LI>Ela não pode excluir arquivos de cópias de sombra para pastas compartilhadas.<BR><BR>
> <LI>Ela pode excluir arquivos de uma cópia de sombra criada usando o utilitário <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](../../administration/windows-commands/diskshadow.md)">DiskShadow</a>, mas não pode excluir arquivos de uma cópia de sombra criada usando o utilitário <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](../../administration/windows-commands/vssadmin.md)">Vssadmin</a>.<BR><BR>
> <LI>Os arquivos são excluídos de uma cópia de sombra com base no melhor esforço. Isso significa que não há garantia de exclusão.<BR><BR></LI></UL>


Para obter mais informações, confira [Como excluir arquivos de cópias de sombra](https://go.microsoft.com/fwlink/?linkid=180904) (https://go.microsoft.com/fwlink/?LinkId=180904) no MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Meu programa de backup que não é da Microsoft falhou com um erro de VSS. O que posso fazer?

Confira a seção de suporte ao produto do site da empresa que criou o programa de backup. Pode haver uma atualização de produto para você baixar e instalar para corrigir o problema. Caso contrário, entre em contato com o departamento de suporte do produto da empresa.

Os administradores do sistema podem usar as informações de solução de problemas do VSS no site da Microsoft TechNet Library a seguir para coletar informações de diagnóstico de problemas relacionados ao VSS.

Para obter mais informações, confira [Serviço de Cópias de Sombra de Volume](https://go.microsoft.com/fwlink/?linkid=180905) (https://go.microsoft.com/fwlink/?LinkId=180905) no TechNet.

### <a name="what-is-the-diff-area"></a>O que é a "área de comparação"?

A área de armazenamento de cópia de sombra (ou "área de comparação") é a localização em que os dados da cópia de sombra criada pelo provedor de software do sistema são armazenados.

### <a name="where-is-the-diff-area-located"></a>Em que local está a área de comparação?

A área de comparação pode estar em qualquer volume local. No entanto, ela deve estar em um volume NTFS que tenha espaço suficiente para armazená-la.

### <a name="how-is-the-diff-area-location-determined"></a>Como a localização da área de comparação é determinada?

Os seguintes critérios são avaliados, nesta ordem, para determinar a localização da área de comparação:

  - Se um volume já tiver uma cópia de sombra existente, essa localização será usada.

  - Se houver uma associação manual pré-configurada entre o volume original e a localização do volume de cópia de sombra, esse localização será usada.

  - Se os dois critérios anteriores não fornecerem uma localização, o serviço de cópia de sombra escolherá uma localização com base no espaço livre disponível. Se mais de um volume estiver sendo copiado em sombra, o serviço de cópias de sombra criará uma lista de possíveis locais de instantâneo com base no tamanho do espaço livre, em ordem decrescente. O número de locais fornecidos é igual ao número de volumes que estão sendo copiados em sombra.

  - Se o volume que está sendo copiado em sombra for um dos locais possíveis, uma associação local será criada. Caso contrário, uma associação com o volume com o espaço mais disponível será criada.


### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>O VSS pode criar cópias de sombra de volumes não NTFS?

Sim. No entanto, cópias de sombra persistentes só podem ser feitas para volumes NTFS. Além disso, pelo menos um volume montado no sistema deve ser um volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>Qual é o número máximo de cópias de sombra que posso criar ao mesmo tempo?

O número máximo de volumes copiados de sombra em um conjunto de cópias de sombra é 64. Observe que isso não é o mesmo que o número de cópias de sombra.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Qual é o número máximo de cópias de sombra de software criadas pelo provedor de sistema que posso manter em um volume?

O número máximo de cópias de sombra de software para cada volume é 512. No entanto, por padrão, você só pode manter 64 cópias de sombra usadas pelo recurso Cópias de Sombra de Pastas Compartilhadas. Para alterar o limite para o recurso Cópias de Sombra de Pastas Compartilhadas, use a seguinte chave do Registro: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>Como posso controlar o espaço usado para o espaço de armazenamento de cópia de sombra?

Digite o comando **vssadmin resize shadowstorage**.

Para obter mais informações, confira [Vssadmin resize shadowstorage](https://go.microsoft.com/fwlink/?linkid=180906) (https://go.microsoft.com/fwlink/?LinkId=180906) no TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>O que acontecerá quando eu ficar sem espaço?

As cópias de sombra do volume serão excluídas, começando com a cópia de sombra mais antiga.

## <a name="volume-shadow-copy-service-tools"></a>Ferramentas do Serviço de Cópias de Sombra de Volume

O sistema operacional Windows fornece as seguintes ferramentas para trabalhar com o VSS:

  - [DiskShadow](https://go.microsoft.com/fwlink/?linkid=180907) (https://go.microsoft.com/fwlink/?LinkId=180907)

  - [VssAdmin](https://go.microsoft.com/fwlink/?linkid=84008) (https://go.microsoft.com/fwlink/?LinkId=84008)


### <a name="diskshadow"></a>DiskShadow

O DiskShadow é um solicitante VSS que você pode usar para gerenciar todos os instantâneos de hardware e software que você pode ter em um sistema. O DiskShadow inclui comandos como os seguintes:

  - **list**: lista gravadores do VSS, provedores do VSS e cópias de sombra

  - **create**: cria uma cópia de sombra

  - **import**: importa uma cópia de sombra transportável

  - **expose**: expõe uma cópia de sombra persistente (como uma letra da unidade)

  - **revert**: reverte um volume de volta para uma cópia de sombra especificada


Essa ferramenta é destinada ao uso por profissionais de TI, mas também pode ser útil para desenvolvedores no teste de um VSS Writer ou de um provedor VSS.

O DiskShadow está disponível apenas em sistemas operacionais Windows Server. Ele não está disponível em sistemas operacionais cliente do Windows.

### <a name="vssadmin"></a>VssAdmin

O VssAdmin é usado para criar, excluir e listar informações sobre cópias de sombra. Ele também pode ser usado para redimensionar a área de armazenamento de cópia de sombra ("área de comparação").

O VssAdmin inclui comandos como os seguintes:

  - **create shadow**: cria uma cópia de sombra

  - **delete shadows**: exclui cópias de sombra

  - **list providers**: lista todos os provedores do VSS registrados

  - **list writers**: lista todos os gravadores do VSS inscritos

  - **resize shadowstorage**: altera o tamanho máximo da área de armazenamento de cópia de sombra


O VssAdmin só pode ser usado para administrar cópias de sombra criadas pelo provedor de software do sistema.

O VssAdmin está disponível nas versões do sistema operacional Windows Client e Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Chaves do Registro do Serviço de Cópias de Sombra de Volume

As seguintes chaves do Registro estão disponíveis para uso com o VSS:

  - **VssAccessControl**

  - **MaxShadowCopies**

  - **MinDiffAreaFileSize**


### <a name="vssaccesscontrol"></a>VssAccessControl

Essa chave é usada para especificar quais usuários têm acesso a cópias de sombra.

Para obter mais informações, confira as seguintes entradas no site da MSDN:

  - [Considerações de segurança para gravadores](https://go.microsoft.com/fwlink/?linkid=157739) (https://go.microsoft.com/fwlink/?LinkId=157739)

  - [Considerações de segurança para solicitantes](https://go.microsoft.com/fwlink/?linkid=180908) (https://go.microsoft.com/fwlink/?LinkId=180908)


### <a name="maxshadowcopies"></a>MaxShadowCopies

Essa chave especifica o número máximo de cópias de sombra acessíveis pelo cliente que podem ser armazenadas em cada volume do computador. As cópias de sombra acessíveis pelo cliente são usadas por Cópias de Sombra para Pastas Compartilhadas.

Para obter mais informações, confira a seguinte entrada no site do MSDN:

**MaxShadowCopies** em [Chaves do Registro para Backup e Restauração](https://go.microsoft.com/fwlink/?linkid=180909) (https://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Essa chave especifica o tamanho inicial mínimo, em MB, da área de armazenamento de cópia de sombra.

Para obter mais informações, confira a seguinte entrada no site do MSDN:

**MinDiffAreaFileSize** em [Chaves do Registro para Backup e Restauração](https://go.microsoft.com/fwlink/?linkid=180910) (https://go.microsoft.com/fwlink/?LinkId=180910)

### <a name="supported-operating-system-versions"></a>Versões do sistema operacional compatíveis

A tabela a seguir lista as versões mínimas do sistema operacional compatíveis para recursos do VSS.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Recurso do VSS</th>
<th>Cliente mínimo com suporte</th>
<th>Servidor mínimo com suporte</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Ressincronização de LUN</p></td>
<td><p>Nenhum compatível</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p>Chave do Registro <strong>FilesNotToSnapshot</strong></p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>Cópias de sombra transportáveis</p></td>
<td><p>Nenhum compatível</p></td>
<td><p>Windows Server 2003 com SP1</p></td>
</tr>
<tr class="even">
<td><p>Cópias de sombra de hardware</p></td>
<td><p>Nenhum compatível</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Versões anteriores do Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>Recuperação rápida usando troca de LUN</p></td>
<td><p>Nenhum compatível</p></td>
<td><p>Windows Server 2003 com SP1</p></td>
</tr>
<tr class="odd">
<td><p>Várias importações de cópias de sombra de hardware</p>
<div class="alert">
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><img src="media/volume-shadow-copy-service/Dd560667.note(WS.10).gif" />Observação</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Essa é a capacidade de importar uma cópia de sombra mais de uma vez. Somente uma operação de importação pode ser executada de cada vez.
<p></p></td>
</tr>
</tbody>
</table>
<p></p>
</div></td>
<td><p>Nenhum compatível</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Cópias de Sombra para Pastas Compartilhadas</p></td>
<td><p>Nenhum compatível</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Cópias de sombra transportáveis recuperadas automaticamente</p></td>
<td><p>Nenhum compatível</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sessões de backup simultâneas (até 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Sessão de restauração única simultânea com backups</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 com SP2</p></td>
</tr>
<tr class="even">
<td><p>Até oito sessões de restauração simultâneas com backups</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="additional-references"></a>Referências adicionais

[Serviço de Cópias de Sombra de Volume no Centro de Desenvolvedores do Windows](/windows/desktop/vss/volume-shadow-copy-service-overview)
