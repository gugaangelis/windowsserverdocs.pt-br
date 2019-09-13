---
title: Serviço de Cópias de Sombra de Volume
ms.date: 01/30/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3fc184f8f23e4325198e3a1a08f20109c2c577e8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867320"
---
# <a name="volume-shadow-copy-service"></a>Serviço de Cópias de Sombra de Volume

Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

O backup e a restauração de dados críticos de negócios podem ser muito complexos devido aos seguintes problemas:

  - Os dados geralmente precisam de backup, enquanto os aplicativos que produzem os dados ainda estão em execução. Isso significa que alguns dos arquivos de dados podem estar abertos ou podem estar em um estado inconsistente.  
      
  - Se o conjunto de dados for grande, pode ser difícil fazer backup de todos eles ao mesmo tempo.  
      

A execução correta de operações de backup e restauração exige uma coordenação próxima entre os aplicativos de backup, os aplicativos de linha de negócios que estão sendo submetidos a backup e o hardware e o software de gerenciamento de armazenamento. O Serviço de Cópias de Sombra de Volume (VSS), que foi introduzido no Windows Server® 2003, facilita a conversa entre esses componentes para permitir que eles funcionem melhor juntos. Quando todos os componentes dão suporte ao VSS, você pode usá-los para fazer backup dos dados do aplicativo sem colocar os aplicativos offline.

O VSS coordena as ações que são necessárias para criar uma cópia de sombra consistente (também conhecida como um instantâneo ou uma cópia pontual) dos dados cujo backup será feito. A cópia de sombra pode ser usada no estado em que se encontra ou pode ser usada em cenários como o seguinte:

  - Você deseja fazer backup de dados do aplicativo e de informações do estado do sistema, incluindo o arquivamento de dados em outra unidade de disco rígido, em fita ou em outra mídia removível.  
      
  - Você está Data Mining.  
      
  - Você está executando backups de disco para disco.  
      
  - Você precisa de uma recuperação rápida da perda de dados restaurando os dados para o LUN original ou para um LUN totalmente novo que substitua um LUN original que falhou.  
      

Os recursos e aplicativos do Windows que usam o VSS incluem o seguinte:

  - [Backup do Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Cópias de sombra de pastas compartilhadas](http://go.microsoft.com/fwlink/?linkid=142874) (http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [Data Protection Manager do System Center](http://go.microsoft.com/fwlink/?linkid=180892) (http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [Restauração do sistema](http://go.microsoft.com/fwlink/?linkid=180893) (http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Como Serviço de Cópias de Sombra de Volume funciona

Uma solução completa do VSS requer todas as seguintes partes básicas:

   Parte do serviço VSS do sistema operacional Windows que garante que os outros componentes possam se comunicar entre si corretamente e trabalhar em conjunto.

**Solicitante do VSS**   o software que solicita a criação real de cópias de sombra (ou outras operações de alto nível, como importar ou excluí-las). Normalmente, esse é o aplicativo de backup. O utilitário Backup do Windows Server e o aplicativo System Center Data Protection Manager são solicitantes VSS. Os solicitantes do VSS que não são da Microsoft® incluem quase todos os softwares de backup que são executados no Windows.

**Gravador VSS o**componente que garante que temos um conjunto de dados consistente para fazer backup.    Normalmente, isso é fornecido como parte de um aplicativo de linha de negócios, como SQL Server® ou Exchange Server. Os gravadores VSS para vários componentes do Windows, como o registro, estão incluídos no sistema operacional Windows. Os gravadores VSS não Microsoft são incluídos em muitos aplicativos para Windows que precisam garantir a consistência dos dados durante o backup.

**Provedor VSS o**componentequecriaemantémascópiasdesombra   . Isso pode ocorrer no software ou no hardware. O sistema operacional Windows inclui um provedor VSS que usa cópia em gravação. Se você usar uma SAN (rede de área de armazenamento), é importante que você instale o provedor de hardware VSS para a SAN, se um for fornecido. Um provedor de hardware descarrega a tarefa de criar e manter uma cópia de sombra do sistema operacional do host.

O diagrama a seguir ilustra como o serviço VSS coordena com solicitantes, gravadores e provedores para criar uma cópia de sombra de um volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   diagrama arquitetônico de serviço de cópias de sombra de volume

### <a name="how-a-shadow-copy-is-created"></a>Como uma cópia de sombra é criada

Esta seção coloca as várias funções do solicitante, do gravador e do provedor no contexto, listando as etapas que precisam ser executadas para criar uma cópia de sombra. O diagrama a seguir mostra como o Serviço de Cópias de Sombra de Volume controla a coordenação geral do solicitante, gravador e provedor.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figura 2** Processo de criação de cópia de sombra

Para criar uma cópia de sombra, o solicitante, o gravador e o provedor executam as seguintes ações:

1.  O solicitante solicita que o Serviço de Cópias de Sombra de Volume enumere os gravadores, reúna os metadados do gravador e prepare-se para a criação da cópia de sombra.  
      
2.  Cada gravador cria uma descrição XML dos componentes e armazenamentos de dados que precisam de backup e os fornece à Serviço de Cópias de Sombra de Volume. O gravador também define um método Restore, que é usado para todos os componentes do. O Serviço de Cópias de Sombra de Volume fornece a descrição do gravador para o solicitante, que seleciona os componentes que serão submetidos a backup.  
      
3.  O Serviço de Cópias de Sombra de Volume notifica todos os gravadores para preparar seus dados para fazer uma cópia de sombra.  
      
4.  Cada gravador prepara os dados conforme apropriado, como a conclusão de todas as transações abertas, os logs de transações sem interrupção e a liberação de caches. Quando os dados estiverem prontos para serem copiados em sombra, o gravador notificará a Serviço de Cópias de Sombra de Volume.  
      
5.  O Serviço de Cópias de Sombra de Volume informa os gravadores para congelar temporariamente as solicitações de e/s de gravação de aplicativos (as solicitações de e/s ainda são possíveis) por alguns segundos que são necessárias para criar a cópia de sombra do volume ou dos volumes. O congelamento do aplicativo não tem permissão para demorar mais de 60 segundos. O Serviço de Cópias de Sombra de Volume libera os buffers do sistema de arquivos e, em seguida, congela o sistema de arquivos, o que garante que os metadados do sistema de arquivos sejam registrados corretamente e que os dados a serem copiados por sombra sejam gravados em uma ordem consistente.  
      
6.  O Serviço de Cópias de Sombra de Volume informa ao provedor para criar a cópia de sombra. O período de criação da cópia de sombra não dura mais do que 10 segundos, durante o qual todas as solicitações de e/s de gravação para o sistema de arquivos permanecem congeladas.  
      
7.  O Serviço de Cópias de Sombra de Volume libera solicitações de e/s de gravação do sistema de arquivos.  
      
8.  O VSS informa os gravadores para descongelar as solicitações de e/s de gravação do aplicativo. Neste ponto, os aplicativos são livres para retomar a gravação de dados no disco que está sendo copiado por sombra.  
      

> [!NOTE]
> A criação da cópia de sombra pode ser anulada se os gravadores forem mantidos no estado de congelamento por mais de 60 segundos ou se os provedores demorarem mais de 10 segundos para confirmar a cópia de sombra. 
<br>

9. O solicitante pode repetir o processo (volte para a etapa 1) ou notificar o administrador para tentar novamente mais tarde.  
      
10. Se a cópia de sombra for criada com êxito, o Serviço de Cópias de Sombra de Volume retornará as informações de local para a cópia de sombra para o solicitante. Em alguns casos, a cópia de sombra pode ser temporariamente disponibilizada como um volume de leitura/gravação para que o VSS e um ou mais aplicativos possam alterar o conteúdo da cópia de sombra antes que a cópia de sombra seja concluída. Depois que o VSS e os aplicativos fizerem suas alterações, a cópia de sombra será feita como somente leitura. Essa fase é chamada de recuperação automática e é usada para desfazer qualquer transação de sistema de arquivos ou aplicativo no volume de cópia de sombra que não foi concluído antes da criação da cópia de sombra.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Como o provedor cria uma cópia de sombra

Um provedor de cópia de sombra de hardware ou software usa um dos seguintes métodos para criar uma cópia de sombra:

**Cópia completa esse**método faz uma cópia completa (chamada de "cópia completa" ou "clone") do volume original em um determinado momento.    Esta cópia é somente leitura.

**Copiar em gravação**   esse método não copia o volume original. Em vez disso, ele faz uma cópia diferencial copiando todas as alterações (solicitações de e/s de gravação concluídas) que são feitas no volume após um determinado ponto no tempo.

**Redirect-on-Write**   esse método não copia o volume original e não faz nenhuma alteração no volume original após um determinado ponto no tempo. Em vez disso, ele faz uma cópia diferencial redirecionando todas as alterações para um volume diferente.

## <a name="complete-copy"></a>Concluir cópia

Uma cópia completa geralmente é criada fazendo um "espelho dividido" da seguinte maneira:

1.  O volume original e o volume da cópia de sombra são um conjunto de volumes espelhado.  
      
2.  O volume da cópia de sombra é separado do volume original. Isso interrompe a conexão espelho.  
      

Após a interrupção da conexão espelho, o volume original e o volume da cópia de sombra são independentes. O volume original continua aceitando todas as alterações (solicitações de e/s de gravação), enquanto o volume da cópia de sombra permanece uma cópia exata somente leitura dos dados originais no momento da interrupção.

### <a name="copy-on-write-method"></a>Método Copy-on-Write

No método Copy-on-Write, quando uma alteração no volume original ocorre (mas antes da conclusão da solicitação de e/s de gravação), cada bloco a ser modificado é lido e gravado na área de armazenamento de cópia de sombra do volume (também chamada de "área de comparação"). A área de armazenamento de cópia de sombra pode estar no mesmo volume ou em um volume diferente. Isso preserva uma cópia do bloco de dados no volume original antes que a alteração o substitua.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>Dados de origem (status e dados)</th>
<th>Cópia de sombra (status e dados)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dados originais: 1 2 3 4 5</p></td>
<td><p>Sem cópia: –</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dados alterados no cache: 3 a 3 '</p></td>
<td><p>Cópia de sombra criada (somente diferenças): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dados originais substituídos: 1 2 3 ' 4 5</p></td>
<td><p>Diferenças e índice armazenados na cópia de sombra: 3</p></td>
</tr>
</tbody>
</table>

**Tabela 1**   o método Copy-on-Write de criação de cópias de sombra

O método Copy-on-Write é um método rápido para criar uma cópia de sombra, pois ela copia apenas os dados que são alterados. Os blocos copiados na área diff podem ser combinados com os dados alterados no volume original para restaurar o volume para seu estado antes que qualquer uma das alterações tenha sido feita. Se houver muitas alterações, o método Copy-on-Write poderá se tornar caro.

### <a name="redirect-on-write-method"></a>Método Redirect-on-Write

No método Redirect-on-Write, sempre que o volume original recebe uma alteração (solicitação de e/s de gravação), a alteração não é aplicada ao volume original. Em vez disso, a alteração é gravada na área de armazenamento de cópia de sombra de outro volume.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>Dados de origem (status e dados)</th>
<th>Cópia de sombra (status e dados)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dados originais: 1 2 3 4 5</p></td>
<td><p>Sem cópia: –</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dados alterados no cache: 3 a 3 '</p></td>
<td><p>Cópia de sombra criada (somente diferenças): Beta</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dados originais inalterados: 1 2 3 4 5</p></td>
<td><p>Diferenças e índice armazenados na cópia de sombra: Beta</p></td>
</tr>
</tbody>
</table>

**Tabela 2**   o método Redirect-on-Write de criação de cópias de sombra

Assim como o método Copy-on-Write, o método Redirect-on-Write é um método rápido para criar uma cópia de sombra, pois ela copia apenas as alterações nos dados. Os blocos copiados na área de comparação podem ser combinados com os dados inalterados no volume original para criar uma cópia completa e atualizada dos dados. Se houver muitas solicitações de e/s de leitura, o método Redirect-on-Write poderá se tornar caro.

## <a name="shadow-copy-providers"></a>Provedores de cópia de sombra

Há dois tipos de provedores de cópia de sombra: provedores baseados em hardware e provedores baseados em software. Há também um provedor de sistema, que é um provedor de software interno do sistema operacional Windows.

### <a name="hardware-based-providers"></a>Provedores baseados em hardware

Os provedores de cópia de sombra com base em hardware atuam como uma interface entre o Serviço de Cópias de Sombra de Volume e o nível de hardware, trabalhando em conjunto com um adaptador ou controlador de armazenamento de hardware. O trabalho de criar e manter a cópia de sombra é executado pela matriz de armazenamento.

Os provedores de hardware sempre usam a cópia de sombra de um LUN inteiro, mas o Serviço de Cópias de Sombra de Volume expõe apenas a cópia de sombra do volume ou dos volumes que foram solicitados.

Um provedor de cópia de sombra baseado em hardware utiliza a funcionalidade de Serviço de Cópias de Sombra de Volume que define o ponto no tempo, permite a sincronização de dados, gerencia a cópia de sombra e fornece uma interface comum com aplicativos de backup. No entanto, o Serviço de Cópias de Sombra de Volume não especifica o mecanismo subjacente pelo qual o provedor baseado em hardware produz e mantém cópias de sombra.

### <a name="software-based-providers"></a>Provedores baseados em software

Os provedores de cópia de sombra baseados em software normalmente interceptam e processam solicitações de e/s de leitura e gravação em uma camada de software entre o sistema de arquivos e o software do Gerenciador de volumes.

Esses provedores são implementados como um componente DLL do modo de usuário e pelo menos um driver de dispositivo em modo kernel, normalmente um driver de filtro de armazenamento. Diferentemente dos provedores baseados em hardware, os provedores baseados em software criam cópias de sombra no nível de software, não no nível de hardware.

Um provedor de cópia de sombra baseado em software deve manter uma exibição "pontual" de um volume, tendo acesso a um conjunto de dados que pode ser usado para recriar o status do volume antes da hora de criação da cópia de sombra. Um exemplo é a técnica de cópia em gravação do provedor do sistema. No entanto, o Serviço de Cópias de Sombra de Volume não impõe restrições sobre a técnica que os provedores baseados em software usam para criar e manter cópias de sombra.

Um provedor de software é aplicável a uma variedade maior de plataformas de armazenamento do que um provedor baseado em hardware, e ele deve funcionar com discos básicos ou volumes lógicos igualmente bem. (Um volume lógico é um volume que é criado pela combinação de espaço livre de dois ou mais discos.) Em contraste com as cópias de sombra de hardware, os provedores de software consomem recursos do sistema operacional para manter a cópia de sombra.

Para obter mais informações sobre discos básicos, consulte [o que são discos e volumes básicos?](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) no TechNet.

### <a name="system-provider"></a>Provedor do sistema

Um provedor de cópia de sombra, o provedor do sistema, é fornecido no sistema operacional Windows. Embora um provedor padrão seja fornecido no Windows, outros fornecedores são livres para fornecer implementações otimizadas para seus aplicativos de hardware e software de armazenamento.

Para manter a exibição "pontual" de um volume que está contido em uma cópia de sombra, o provedor do sistema usa uma técnica de copiar na gravação. Cópias dos blocos no volume que foram modificados desde o início da criação da cópia de sombra são armazenadas em uma área de armazenamento de cópia de sombra.

O provedor do sistema pode expor o volume de produção, que pode ser gravado e lido normalmente. Quando a cópia de sombra é necessária, ela aplica logicamente as diferenças aos dados no volume de produção para expor a cópia de sombra completa.

Para o provedor do sistema, a área de armazenamento de cópia de sombra deve estar em um volume NTFS. O volume a ser copiado em sombra não precisa ser um volume NTFS, mas pelo menos um volume montado no sistema deve ser um volume NTFS.

Os arquivos de componente que compõem o provedor de sistema são SWPRV. dll e Volsnap. sys.

### <a name="in-box-vss-writers"></a>Gravadores VSS in-box

O sistema operacional Windows inclui um conjunto de gravadores VSS que são responsáveis por enumerar os dados exigidos por vários recursos do Windows.

Para obter mais informações sobre esses gravadores, consulte os seguintes sites da Microsoft:

  - [Gravadores VSS in-box](http://go.microsoft.com/fwlink/?linkid=180895) (http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Novos gravadores do VSS in-box para Windows Server 2008 e Windows Vista SP1](http://go.microsoft.com/fwlink/?linkid=180896) (http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Novos gravadores do VSS in-box para Windows Server 2008 R2 e Windows 7](http://go.microsoft.com/fwlink/?linkid=180897) (http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Como as cópias de sombra são usadas

Além de fazer backup de dados de aplicativos e informações de estado do sistema, as cópias de sombra podem ser usadas para várias finalidades, incluindo as seguintes:

  - Restaurando LUNs (ressincronização de LUN e troca de LUN)  
      
  - Restaurando arquivos individuais (Cópias de Sombra para Pastas Compartilhadas)  
      
  - Mineração de dados usando cópias de sombra transportáveis  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Restaurando LUNs (ressincronização de LUN e troca de LUN)

No Windows Server 2008 R2 e no Windows 7, os solicitantes do VSS podem usar um recurso de provedor de cópias de sombra de hardware chamado ressincronização de LUN (ou "LUN Resync"). Esse é um esquema de recuperação rápida que permite que um administrador de aplicativos restaure dados de uma cópia de sombra para o LUN original ou para um novo LUN.

A cópia de sombra pode ser um clone completo ou uma cópia de sombra diferencial. Em ambos os casos, no final da operação de ressincronização, o LUN de destino terá o mesmo conteúdo que o LUN de cópia de sombra. Durante a operação de ressincronização, a matriz executa uma cópia de nível de bloco da cópia de sombra para o LUN de destino.


> [!NOTE]
> A cópia de sombra deve ser uma cópia de sombra de hardware transportável. 
<br>


A maioria das matrizes permite que as operações de e/s de produção retomem logo após o início da operação de ressincronização. Enquanto a operação de ressincronização está em andamento, as solicitações de leitura são redirecionadas para o LUN de cópia de sombra e gravam solicitações para o LUN de destino. Isso permite que as matrizes recuperem conjuntos de dados muito grandes e retomem as operações normais em vários segundos.

A ressincronização de LUN é diferente da troca de LUN. Uma permuta de LUN é um cenário de recuperação rápida com suporte do VSS desde o Windows Server 2003 SP1. Em uma permuta de LUN, a cópia de sombra é importada e, em seguida, convertida em um volume de leitura/gravação. A conversão é uma operação irreversível, e o volume e o LUN subjacente não podem ser controlados com as APIs VSS depois disso. A lista a seguir descreve como a ressincronização de LUN se compara com a troca de LUN:

  - Na ressincronização de LUN, a cópia de sombra não é alterada, portanto, ela pode ser usada várias vezes. Na troca de LUN, a cópia de sombra pode ser usada apenas uma vez para uma recuperação. Para os administradores mais preocupados com segurança, isso é importante. Quando a ressincronização de LUN é usada, o solicitante pode repetir a operação de restauração inteira se algo der errado na primeira vez.  
      
  - No final de uma permuta de LUN, o LUN de cópia de sombra é usado para solicitações de e/s de produção. Por esse motivo, o LUN de cópia de sombra deve usar a mesma qualidade de armazenamento que o LUN de produção original para garantir que o desempenho não seja afetado após a operação de recuperação. Se a ressincronização de LUN for usada em vez disso, o provedor de hardware poderá manter a cópia de sombra no armazenamento que é menos cara do que o armazenamento de qualidade de produção.  
      
  - Se o LUN de destino for inutilizável e precisar ser recriado, a troca de LUN poderá ser mais econômica porque não requer um LUN de destino.  
      


> [!WARNING]
> Todas as operações listadas são operações de nível de LUN. Se você tentar recuperar um volume específico usando a ressincronização de LUN, será inadvertidamente reverter todos os outros volumes que estão compartilhando o LUN. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Restaurando arquivos individuais (Cópias de Sombra para Pastas Compartilhadas)

Cópias de Sombra para Pastas Compartilhadas usa o Serviço de Cópias de Sombra de Volume para fornecer cópias point-in-time de arquivos localizados em um recurso de rede compartilhado, como um servidor de arquivos. Com o Cópias de Sombra para Pastas Compartilhadas, os usuários podem recuperar rapidamente os arquivos excluídos ou alterados que estão armazenados na rede. Como eles podem fazer isso sem a assistência do administrador, Cópias de Sombra para Pastas Compartilhadas pode aumentar a produtividade e reduzir os custos administrativos.

Para obter mais informações sobre cópias de sombra para pastas compartilhadas, consulte [cópias de sombra para pastas compartilhadas](http://go.microsoft.com/fwlink/?linkid=180898) (http://go.microsoft.com/fwlink/?LinkId=180898) no TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Mineração de dados usando cópias de sombra transportáveis

Com um provedor de hardware projetado para uso com o Serviço de Cópias de Sombra de Volume, você pode criar cópias de sombra transportáveis que podem ser importadas para servidores no mesmo subsistema (por exemplo, uma SAN). Essas cópias de sombra podem ser usadas para propagar uma instalação de produção ou de teste com dados somente leitura para Data Mining.

Com o Serviço de Cópias de Sombra de Volume e uma matriz de armazenamento com um provedor de hardware projetado para uso com o Serviço de Cópias de Sombra de Volume, é possível criar uma cópia de sombra do volume de dados de origem em um servidor e, em seguida, importar a cópia de sombra para outro servidor  (ou voltar ao mesmo servidor). Esse processo é realizado em alguns minutos, independentemente do tamanho dos dados. O processo de transporte é realizado por meio de uma série de etapas que usam um solicitante de cópia de sombra (um aplicativo de gerenciamento de armazenamento) que dá suporte a cópias de sombra transportáveis.

## <a name="to-transport-a-shadow-copy"></a>Para transportar uma cópia de sombra

1.  Crie uma cópia de sombra transportável dos dados de origem em um servidor.

2.  Importe a cópia de sombra para um servidor que esteja conectado à SAN (você pode importar para um servidor diferente ou o mesmo servidor).

3.  Os dados agora estão prontos para serem usados.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   criação e transporte de cópia de sombra entre dois servidores


> [!NOTE]
> Uma cópia de sombra transportável criada no Windows Server 2003 não pode ser importada para um servidor que esteja executando o Windows Server 2008 ou o Windows Server 2008 R2. Uma cópia de sombra transportável criada no Windows Server 2008 ou no Windows Server 2008 R2 não pode ser importada para um servidor que esteja executando o Windows Server 2003. No entanto, uma cópia de sombra criada no Windows Server 2008 pode ser importada para um servidor que esteja executando o Windows Server 2008 R2 e vice-versa. 
<br>


As cópias de sombra são somente leitura. Se você quiser converter uma cópia de sombra em um LUN de leitura/gravação, poderá usar um aplicativo de gerenciamento de armazenamento baseado em serviço de disco virtual (incluindo alguns solicitantes) Além do Serviço de Cópias de Sombra de Volume. Ao usar esse aplicativo, você pode remover a cópia de sombra do gerenciamento de Serviço de Cópias de Sombra de Volume e convertê-la em um LUN de leitura/gravação.

Serviço de Cópias de Sombra de Volume transporte é uma solução avançada em computadores que executam o Windows Server 2003 Enterprise Edition, o Windows Server 2003 Datacenter Edition, o Windows Server 2008 ou o Windows Server 2008 R2. Ele só funcionará se houver um provedor de hardware na matriz de armazenamento. O transporte de cópia de sombra pode ser usado para várias finalidades, incluindo backups em fita, Data Mining e teste.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

Este FAQ responde perguntas sobre o Serviço de Cópias de Sombra de Volume (VSS) para administradores do sistema. Para obter informações sobre interfaces de programação de aplicativo VSS, consulte [serviço de cópias de sombra de volume](http://go.microsoft.com/fwlink/?linkid=180899) (http://go.microsoft.com/fwlink/?LinkId=180899) na biblioteca do Windows Developer Center.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Quando a Serviço de Cópias de Sombra de Volume foi introduzida? Em quais versões do sistema operacional Windows ele está disponível?

O VSS foi introduzido no Windows XP. Ele está disponível no Windows XP, no Windows Server 2003, no Windows Vista®, no Windows Server 2008, no Windows 7 e no Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>Qual é a diferença entre uma cópia de sombra e um backup?

No caso de um backup da unidade de disco rígido, a cópia de sombra criada também é o backup. Os dados podem ser copiados para fora da cópia de sombra de uma restauração ou a cópia de sombra pode ser usada para um cenário de recuperação rápida — por exemplo, ressincronização de LUN ou troca de LUN.

Quando os dados são copiados da cópia de sombra para uma fita ou de outra mídia removível, o conteúdo armazenado na mídia constitui o backup. A cópia de sombra em si pode ser excluída depois que os dados são copiados dela.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>Qual é o maior volume de tamanho ao qual Serviço de Cópias de Sombra de Volume dá suporte?

O Serviço de Cópias de Sombra de Volume dá suporte a um tamanho de volume de até 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>Fiz um backup no Windows Server 2008. Posso restaurá-lo no Windows Server 2008 R2?

Depende do software de backup que você usou. A maioria dos programas de backup dá suporte a esse cenário para dados, mas não para backups de estado do sistema.

As cópias de sombra criadas em qualquer uma dessas versões do Windows podem ser usadas no outro.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>Fiz um backup no Windows Server 2003. Posso restaurá-lo no Windows Server 2008?

Depende do software de backup usado. Se você criar uma cópia de sombra no Windows Server 2003, não poderá usá-la no Windows Server 2008. Além disso, se você criar uma cópia de sombra no Windows Server 2008, não poderá restaurá-la no Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Como posso desabilitar o VSS?

É possível desabilitar o Serviço de Cópias de Sombra de Volume usando o console de gerenciamento Microsoft. No entanto, você não deve fazer isso. Desabilitar o VSS afeta negativamente qualquer software que você usar que dependa dele, como a restauração do sistema e o Backup do Windows Server.

Para obter mais informações, consulte os seguintes sites da Web do Microsoft TechNet:

  - [Restauração do sistema](http://go.microsoft.com/fwlink/?linkid=157113) (http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Backup do Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>Posso excluir arquivos de uma cópia de sombra para economizar espaço?

O VSS foi projetado para criar cópias de sombra de volumes inteiros. Arquivos temporários, como arquivos de paginação, são automaticamente omitidos de cópias de sombra para economizar espaço.

Para excluir arquivos específicos de cópias de sombra, use a seguinte chave do registro: **FilesNotToSnapshot**.


> [!NOTE]
> A chave do registro <STRONG>FilesNotToSnapshot</STRONG> destina-se a ser usada somente por aplicativos. Os usuários que tentarem usá-lo encontrarão limitações, como as seguintes:
> <br>
> <UL>
> <LI>Ele não pode excluir arquivos de uma cópia de sombra criada em um Windows Server usando o recurso de versões anteriores.<BR><BR>
> <LI>Ele não pode excluir arquivos de cópias de sombra para pastas compartilhadas.<BR><BR>
> <LI>Ele pode excluir arquivos de uma cópia de sombra que foi criada usando o utilitário <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">DiskShadow</a> , mas não pode excluir arquivos de uma cópia de sombra que foi criada usando o utilitário <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">vssadmin</a> .<BR><BR>
> <LI>Os arquivos são excluídos de uma cópia de sombra em uma base de melhor esforço. Isso significa que não há garantia de que eles sejam excluídos.<BR><BR></LI></UL>


Para obter mais informações, consulte [excluindo arquivos de cópias de sombra](http://go.microsoft.com/fwlink/?linkid=180904) (http://go.microsoft.com/fwlink/?LinkId=180904) no msdn.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Meu programa de backup que não é da Microsoft falhou com um erro de VSS. O que posso fazer?

Verifique a seção de suporte ao produto do site da empresa que criou o programa de backup. Pode haver uma atualização de produto que você pode baixar e instalar para corrigir o problema. Caso contrário, entre em contato com o departamento de suporte do produto da empresa.

Os administradores do sistema podem usar as informações de solução de problemas do VSS no seguinte site da Microsoft TechNet Library para coletar informações de diagnóstico sobre problemas relacionados ao VSS.

Para obter mais informações, consulte [serviço de cópias de sombra de volume](http://go.microsoft.com/fwlink/?linkid=180905) (http://go.microsoft.com/fwlink/?LinkId=180905) no TechNet.

### <a name="what-is-the-diff-area"></a>O que é a "área de comparação"?

A área de armazenamento de cópia de sombra (ou "área de comparação") é o local onde os dados da cópia de sombra criada pelo provedor de software do sistema são armazenados.

### <a name="where-is-the-diff-area-located"></a>Onde está localizada a área de comparação?

A área de comparação pode estar localizada em qualquer volume local. No entanto, ele deve estar localizado em um volume NTFS que tenha espaço suficiente para armazená-lo.

### <a name="how-is-the-diff-area-location-determined"></a>Como o local da área de comparação é determinado?

Os critérios a seguir são avaliados, nesta ordem, para determinar o local da área de comparação:

  - Se um volume já tiver uma cópia de sombra existente, esse local será usado.  
      
  - Se houver uma associação manual pré-configurada entre o volume original e o local do volume de cópias de sombra, esse local será usado.  
      
  - Se os dois critérios anteriores não fornecerem um local, o serviço de cópias de sombra escolherá um local com base no espaço livre disponível. Se mais de um volume estiver sendo copiado em sombra, o serviço de cópias de sombra criará uma lista de possíveis locais de instantâneo com base no tamanho do espaço livre, em ordem decrescente. O número de locais fornecidos é igual ao número de volumes que estão sendo copiados em sombra.  
      
  - Se o volume que está sendo copiado em sombra for um dos locais possíveis, uma associação local será criada. Caso contrário, uma associação com o volume com o espaço mais disponível será criada.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>O VSS pode criar cópias de sombra de volumes não NTFS?

Sim. No entanto, cópias de sombra persistentes só podem ser feitas para volumes NTFS. Além disso, pelo menos um volume montado no sistema deve ser um volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>Qual é o número máximo de cópias de sombra que posso criar ao mesmo tempo?

O número máximo de volumes copiados de sombra em um único conjunto de cópias de sombra é 64. Observe que isso não é o mesmo que o número de cópias de sombra.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Qual é o número máximo de cópias de sombra de software criadas pelo provedor de sistema que posso manter em um volume?

O número máximo de cópias de sombra de software para cada volume é 512. No entanto, por padrão, você só pode manter 64 cópias de sombra usadas pelo recurso cópias de sombra de pastas compartilhadas. Para alterar o limite para as cópias de sombra do recurso de pastas compartilhadas, use a seguinte chave do registro: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>Como posso controlar o espaço usado para o espaço de armazenamento de cópia de sombra?

Digite o comando **vssadmin Resize ShadowStorage** .

Para obter mais informações, consulte [vssadmin Resize ShadowStorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) no TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>O que acontecerá quando eu ficar sem espaço?

As cópias de sombra do volume são excluídas, começando com a cópia de sombra mais antiga.

## <a name="volume-shadow-copy-service-tools"></a>Ferramentas de Serviço de Cópias de Sombra de Volume

O sistema operacional Windows fornece as seguintes ferramentas para trabalhar com o VSS:

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

O DiskShadow é um solicitante VSS que você pode usar para gerenciar todos os instantâneos de hardware e software que você pode ter em um sistema. O DiskShadow inclui comandos como os seguintes:

  - **lista**: Lista gravadores VSS, provedores VSS e cópias de sombra  
      
  - **criar**: Cria uma nova cópia de sombra  
      
  - **importar**: Importa uma cópia de sombra transportável  
      
  - **expor**: Expõe uma cópia de sombra persistente (como uma letra da unidade, por exemplo)  
      
  - **reverter**: Reverte um volume de volta para uma cópia de sombra especificada  
      

Essa ferramenta é destinada ao uso por profissionais de ti, mas os desenvolvedores também podem achar útil ao testar um VSS Writer ou um provedor VSS.

O DiskShadow está disponível apenas em sistemas operacionais Windows Server. Ele não está disponível em sistemas operacionais cliente do Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin é usado para criar, excluir e listar informações sobre cópias de sombra. Ele também pode ser usado para redimensionar a área de armazenamento de cópia de sombra ("área de comparação").

VssAdmin inclui comandos como os seguintes:

  - **criar sombra**: Cria uma nova cópia de sombra  
      
  - **excluir sombras**: Exclui cópias de sombra  
      
  - **listar provedores**: Lista todos os provedores VSS registrados  
      
  - **listar gravadores**: Lista todos os gravadores VSS assinados  
      
  - **redimensionar ShadowStorage**: Altera o tamanho máximo da área de armazenamento de cópia de sombra  
      

VssAdmin só pode ser usado para administrar cópias de sombra criadas pelo provedor de software do sistema.

O VssAdmin está disponível nas versões do sistema operacional Windows Client e do Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Serviço de Cópias de Sombra de Volume chaves do registro

As seguintes chaves do registro estão disponíveis para uso com o VSS:

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Essa chave é usada para especificar quais usuários têm acesso a cópias de sombra.

Para obter mais informações, consulte as seguintes entradas no site do MSDN:

  - [Considerações de segurança para gravadores](http://go.microsoft.com/fwlink/?linkid=157739) (http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Considerações de segurança para solicitantes](http://go.microsoft.com/fwlink/?linkid=180908) (http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Essa chave especifica o número máximo de cópias de sombra acessíveis pelo cliente que podem ser armazenadas em cada volume do computador. As cópias de sombra acessíveis pelo cliente são usadas pelo Cópias de Sombra para Pastas Compartilhadas.

Para obter mais informações, consulte a seguinte entrada no site da MSDN:

**MaxShadowCopies** em [chaves do registro para backup e restauração](http://go.microsoft.com/fwlink/?linkid=180909) (http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Essa chave especifica o tamanho inicial mínimo, em MB, da área de armazenamento de cópia de sombra.

Para obter mais informações, consulte a seguinte entrada no site da MSDN:

**MinDiffAreaFileSize** em [chaves do registro para backup e restauração](http://go.microsoft.com/fwlink/?linkid=180910) (http://go.microsoft.com/fwlink/?LinkId=180910)

`##`# ' Versões de sistema operacional com suporte

A tabela a seguir lista as versões mínimas de sistema operacional com suporte para recursos do VSS.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Recurso VSS</th>
<th>Cliente mínimo com suporte</th>
<th>Servidor mínimo com suporte</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Ressincronização de LUN</p></td>
<td><p>Nenhum com suporte</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p>Chave do registro <strong>FilesNotToSnapshot</strong></p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>Cópias de sombra transportáveis</p></td>
<td><p>Nenhum com suporte</p></td>
<td><p>Windows Server 2003 com SP1</p></td>
</tr>
<tr class="even">
<td><p>Cópias de sombra de hardware</p></td>
<td><p>Nenhum com suporte</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Versões anteriores do Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>Recuperação rápida usando permuta de LUN</p></td>
<td><p>Nenhum com suporte</p></td>
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
<td><p>Nenhum com suporte</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Cópias de Sombra para Pastas Compartilhadas</p></td>
<td><p>Nenhum com suporte</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Cópias de sombra recuperadas automaticamente transportáveis</p></td>
<td><p>Nenhum com suporte</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sessões de backup simultâneas (até 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Sessão de restauração única simultânea com backups</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 com SP2</p></td>
</tr>
<tr class="even">
<td><p>Até 8 sessões de restauração simultâneas com backups</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Consulte também

[Serviço de Cópias de Sombra de Volume no Windows Developer Center](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)