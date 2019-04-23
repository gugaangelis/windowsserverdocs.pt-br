---
Title: Serviço de Cópias de Sombra de Volume
ms.date: 01/30/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7b61a0494b8a63168b40bfaed42dedf0fff40c35
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887257"
---
# <a name="volume-shadow-copy-service"></a>Serviço de Cópias de Sombra de Volume

Aplica-se a: 2019 do Windows Server, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 e Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

Fazer backup e restaurar dados críticos de negócios podem ser muito complexas devido aos seguintes problemas:

  - Os dados geralmente precisam ser feito enquanto os aplicativos que produzem os dados ainda estão em execução. Isso significa que alguns dos arquivos de dados podem ser abertos ou eles podem estar em um estado inconsistente.  
      
  - Se o conjunto de dados for grande, pode ser difícil fazer backup de tudo ao mesmo tempo.  
      

Corretamente executando operações de backup e restauração exige fechar coordenação entre os aplicativos de backup, os aplicativos de linha de negócios que estão sendo feitos backup, e o hardware de gerenciamento de armazenamento e software. O Volume Shadow Copy Service (VSS), que foi introduzida no Windows Server® 2003, facilita a conversa entre esses componentes para permitir que elas funcionam melhor juntos. Quando todos os componentes oferece suporte ao VSS, você pode usá-los para fazer backup dos dados de aplicativo sem colocar os aplicativos offline.

O VSS coordena as ações que são necessárias para criar uma cópia de sombra consistentes (também conhecido como um instantâneo ou uma cópia de point-in-time) dos dados que deve ser feito backup. A cópia de sombra pode ser usada como-está, ou ele pode ser usado em cenários como o seguinte:

  - Você deseja fazer backup de dados e sistema de estado informações do aplicativo, incluindo arquivamento de dados para outra unidade de disco rígido, fita ou outra mídia removível.  
      
  - Você é mineração de dados.  
      
  - Você está realizando backups de disco para disco.  
      
  - Você precisa de uma rápida recuperação da perda de dados, restaurando os dados para o LUN original ou um LUN totalmente novo que substitui um LUN original que falhou.  
      

Recursos do Windows e aplicativos que usam o VSS incluem o seguinte:

  - [Backup do Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Cópias de sombra de pastas compartilhadas](http://go.microsoft.com/fwlink/?linkid=142874) (http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892) (http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [A restauração do sistema](http://go.microsoft.com/fwlink/?linkid=180893) (http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Como funciona o serviço de cópias de sombra de Volume

Uma solução completa do VSS requer que todas as partes básicas a seguir:

**Serviço VSS**   parte do sistema operacional Windows que garante que os outros componentes pode se comunicar entre si corretamente e funcionam em conjunto.

**Solicitante VSS**   o software que solicita a criação real de cópias de sombra (ou outras operações de alto nível, como a importação ou excluí-los). Normalmente, esse é o aplicativo de backup. O utilitário de Backup do Windows Server e o aplicativo do System Center Data Protection Manager são os solicitadores do VSS. Os solicitantes de não-Microsoft® VSS incluem quase todos os software de backup que é executado no Windows.

**O VSS writer**   o componente que garanta que temos um conjunto de dados consistente para fazer backup. Isso normalmente é fornecido como parte de um aplicativo de linha de negócios, como SQL Server® ou o Exchange Server. Gravadores VSS para vários componentes do Windows, como o registro, estão incluídos no sistema operacional Windows. Os gravadores VSS não são da Microsoft estão incluídos com muitos aplicativos para Windows que precisa para garantir a consistência de dados durante backup.

**Provedor VSS**   o componente que cria e mantém as cópias de sombra. Isso pode ocorrer no software ou no hardware. O sistema operacional Windows inclui um provedor VSS que usa a cópia na gravação. Se você usar uma rede de área de armazenamento (SAN), é importante que você instale o provedor de VSS de hardware para o SAN, se fornecido. Um provedor de hardware descarrega a tarefa de criar e manter uma cópia de sombra do sistema operacional do host.

O diagrama a seguir ilustra como o VSS service coordena com os solicitantes, gravadores e provedores para criar uma cópia de sombra de um volume.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   diagrama da arquitetura de serviço de cópias de sombra de Volume

### <a name="how-a-shadow-copy-is-created"></a>Como uma cópia de sombra é criada

Nesta seção coloca as diversas funções do solicitante, o gravador e o provedor no contexto, listando as etapas que precisam ser realizadas para criar uma cópia de sombra. O diagrama a seguir mostra como o serviço de cópias de sombra de Volume controla a coordenação geral de como o solicitante, o gravador e o provedor.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figura 2** processo de criação de cópia de sombra

Para criar uma cópia de sombra, o solicitante, o gravador e o provedor de executam as seguintes ações:

1.  O solicitante pede que o serviço de cópias de sombra de Volume para enumerar os gravadores, reunir os metadados do gravador e preparar para a criação de cópias de sombra.  
      
2.  Cada gravador cria uma descrição de XML dos armazenamentos de dados e componentes que precisa ser submetido ao backup e fornece-o para o serviço de cópias de sombra de Volume. O gravador também define um método de restauração, o que é usado para todos os componentes. O serviço de cópias de sombra de Volume fornece a descrição do escritor para o solicitante, que seleciona os componentes que serão submetidos a backup.  
      
3.  O serviço de cópias de sombra de Volume notifica todos os gravadores para preparar seus dados para fazer uma cópia de sombra.  
      
4.  Cada gravador prepara os dados conforme apropriado, como a conclusão de todas as transações abertas, sem interrupção de logs de transações e liberação de caches. Quando os dados estão prontos para ser copiado de sombra, o gravador notifica o serviço de cópias de sombra de Volume.  
      
5.  O serviço de cópias de sombra de Volume instrui os gravadores para congelar temporariamente solicitações de e/s de gravação do aplicativo (leitura ainda são possíveis de solicitações de e/s) por alguns segundos em que são necessárias para criar a cópia de sombra do volume ou volumes. O congelamento de aplicativo não pode levar mais de 60 segundos. O serviço de cópias de sombra de Volume libera os buffers do sistema de arquivos e, em seguida, congela o sistema de arquivos, o que garante que os metadados do sistema de arquivo é registrado corretamente e os dados a serem copiados em sombra são escritos em uma ordem consistente.  
      
6.  O serviço de cópias de sombra de Volume informa ao provedor para criar a cópia de sombra. O período de criação de cópia de sombra dura não mais do que 10 segundos, durante o qual todas as de gravação solicitações de e/s para o sistema de arquivos permanecem congeladas.  
      
7.  O serviço de cópias de sombra de Volume libera solicitações de e/s de gravação de sistema de arquivos.  
      
8.  VSS instrui os gravadores para descongelar as solicitações de e/s de gravação do aplicativo. Neste ponto, os aplicativos são livres para continuar a gravar dados no disco que está sendo copiado de sombra.  
      

> [!NOTE]
> A criação de cópias de sombra pode ser anulada se os gravadores são mantidos no estado de congelamento por mais de 60 segundos ou se os provedores de levar mais de 10 segundos para confirmar a cópia de sombra. 
<br>

9.  O solicitante pode repetir o processo (vá para a etapa 1) ou notificar o administrador para tentar novamente mais tarde.  
      
10. Se a cópia de sombra for criada com êxito, o serviço de cópias de sombra de Volume retorna as informações de local para a cópia de sombra para o solicitante. Em alguns casos, a cópia de sombra pode ser temporariamente disponibilizada como um volume de leitura / gravação para que o VSS e um ou mais aplicativos podem alterar o conteúdo da cópia de sombra antes da cópia de sombra for concluída. Depois que o VSS e os aplicativos de fazer suas alterações, a cópia de sombra é somente leitura. Essa fase é chamada de recuperação automática, e ele é usado para desfazer as transações de aplicativo ou sistema de arquivos no volume de cópia de sombra que não foram concluídas antes que a cópia de sombra foi criada.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Como o provedor cria uma cópia de sombra

Um provedor de cópias de sombra de hardware ou software usa um dos métodos a seguir para criar uma cópia de sombra:

**Conclua a cópia**   esse método faz uma cópia completa (denominada "cópia completa" ou "clonar") do volume original em um determinado ponto no tempo. Essa cópia é somente leitura.

**Copy-on-write**   esse método não copia o volume original. Em vez disso, ele faz uma cópia de diferencial por meio da cópia de todas as alterações (solicitações de e/s de gravação concluídas) que são feitas no volume após um determinado ponto no tempo.

**Redirecionamento-on-write**   esse método não copia o volume original e não faz quaisquer alterações para o volume original após um determinado ponto no tempo. Em vez disso, ele faz uma cópia de diferencial, redirecionando todas as alterações para um volume diferente.

## <a name="complete-copy"></a>Cópia completa

Uma cópia completa geralmente é criada, fazendo um divisão "espelho" da seguinte maneira:

1.  O volume original e o volume de cópia de sombra são um conjunto de volumes espelhados.  
      
2.  O volume de cópia de sombra é separado do volume original. Isso interrompe a conexão de espelhamento.  
      

Depois que a conexão de espelhamento for interrompida, o volume original e o volume de cópia de sombra são independentes. O volume original continua a aceitar todas as alterações (solicitações de e/s de gravação), enquanto a cópia de sombra de volume permanece uma cópia exata de somente leitura dos dados originais no momento da interrupção.

### <a name="copy-on-write-method"></a>Método de cópia na gravação

O método de cópia na gravação, quando ocorre uma alteração para o volume original (mas antes da gravação solicitação de e/s é concluída), cada bloco a ser modificada é ler e, em seguida, gravado as área de armazenamento cópia sombra do volume (também chamada de sua área"diff"). A área de armazenamento de cópia de sombra pode estar no mesmo volume ou um volume diferente. Isso preserva uma cópia do bloco de dados no volume original antes da alteração substitui-lo.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Hora</th>
<th>Fonte de dados (dados e status)</th>
<th>Cópia de sombra (e dados de status)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dados originais: 1 2 3 4 5</p></td>
<td><p>Nenhuma cópia: —</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dados alterados no cache: 3 de 3'</p></td>
<td><p>Cópia de sombra criada (apenas diferenças): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dados originais substituídos: 1 2 3’ 4 5</p></td>
<td><p>As diferenças e índice armazenados na cópia de sombra: 3</p></td>
</tr>
</tbody>
</table>

**Tabela 1**   o método de cópia na gravação de criação de cópias de sombra

O método de cópia na gravação é um método rápido para criar uma cópia de sombra, pois ele copia apenas os dados que são alterados. Os blocos copiados na área de comparação podem ser combinados com os dados alterados no volume original para restaurar o volume para seu estado antes que as alterações foram feitas. Se houver muitas alterações, o método de cópia na gravação pode se tornar caro.

### <a name="redirect-on-write-method"></a>Método de redirecionamento na gravação

No método de redirecionamento na gravação, sempre que o volume original receba uma alteração (solicitação de e/s de gravação), a alteração não é aplicada para o volume original. Em vez disso, a alteração é gravada para a área de armazenamento de cópia de sombra do volume para outro.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Hora</th>
<th>Fonte de dados (dados e status)</th>
<th>Cópia de sombra (e dados de status)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Dados originais: 1 2 3 4 5</p></td>
<td><p>Nenhuma cópia: —</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Dados alterados no cache: 3 de 3'</p></td>
<td><p>Cópia de sombra criada (apenas diferenças): 3’</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Dados originais inalterados: 1 2 3 4 5</p></td>
<td><p>As diferenças e índice armazenados na cópia de sombra: 3’</p></td>
</tr>
</tbody>
</table>

**Tabela 2**   o método de redirecionamento na gravação de criação de cópias de sombra

Como o método de cópia na gravação, o método de redirecionamento-on-write é um método rápido para criar uma cópia de sombra, pois ele copia somente as alterações aos dados. Os blocos copiados na área de comparação podem ser combinados com os dados inalterados no volume original para criar uma cópia completa e atualizada dos dados. Se houver muitas solicitações de e/s de leitura, o método de redirecionamento na gravação pode se tornar caro.

## <a name="shadow-copy-providers"></a>Provedores de cópia de sombra

Há dois tipos de provedores de cópia de sombra: provedores de hardware e provedores baseados em software. Também é um provedor de sistema, que é um provedor de software que é incorporado ao sistema operacional Windows.

### <a name="hardware-based-providers"></a>Provedores de hardware

Ato de provedores de cópia de sombra com base em hardware como uma interface entre o serviço de cópias de sombra de Volume e o nível de hardware, trabalhando em conjunto com um adaptador de armazenamento de hardware ou um controlador de. O trabalho de criar e manter a cópia de sombra é realizado pela matriz de armazenamento.

Provedores de hardware sempre fazer a cópia de sombra de um LUN inteiro, mas o serviço de cópias de sombra de Volume expõe apenas a cópia de sombra do volume ou volumes que foram solicitados.

Um provedor de cópias de sombra com base em hardware utiliza o serviço de cópias de sombra de Volume de funcionalidade que define o ponto no tempo, permite a sincronização de dados, gerencia a cópia de sombra e fornece uma interface comum com aplicativos de backup. No entanto, o serviço de cópias de sombra de Volume não especifica o mecanismo subjacente pelo qual o provedor de hardware gera e mantém cópias de sombra.

### <a name="software-based-providers"></a>Provedores de software

Provedores de cópia baseada em software normalmente interceptar e processar solicitações de gravação e/s em uma camada de software entre o sistema de arquivos e o software do Gerenciador de volume.

Esses provedores são implementados como um componente DLL do modo de usuário e o driver de dispositivo de pelo menos um modo de kernel, normalmente um driver de filtro de armazenamento. Ao contrário de provedores de hardware, provedores de software criam cópias de sombra no nível do software, não no nível de hardware.

Um provedor de cópias de sombra baseada em software deve manter uma exibição de "point-in-time" de um volume tendo acesso a um conjunto de dados que pode ser usado para recriar o status do volume antes do tempo de criação de cópia de sombra. Um exemplo é a técnica de cópia na gravação do provedor do sistema. No entanto, o serviço de cópias de sombra de Volume não coloca nenhuma restrição na qual técnica os provedores baseados em software usam para criar e manter cópias de sombra.

Um provedor de software é aplicável para uma ampla variedade de plataformas de armazenamento de um provedor de hardware, e ele deve funcionar igualmente bem com discos básicos ou volumes lógicos. (Um volume lógico é um volume que é criado pela combinação de espaço livre de dois ou mais discos.) Em contraste com cópias de sombra de hardware, fornecedores de software de consumam recursos do sistema operacional para manter a cópia de sombra.

Para obter mais informações sobre os discos básicos, consulte [quais são discos e Volumes básicos?](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) no TechNet.

### <a name="system-provider"></a>Provedor do sistema

Um provedor de cópias de sombra, o provedor do sistema, é fornecido no sistema operacional Windows. Embora um provedor padrão é fornecido no Windows, outros fornecedores são livres para fornecer implementações que são otimizadas para seus aplicativos de software e hardware de armazenamento.

Para manter a exibição "point-in-time" de um volume que está contido em uma cópia de sombra, o provedor do sistema usa uma técnica de cópia na gravação. Cópias dos blocos que foram modificados desde o início da criação de cópias de sombra no volume são armazenadas em uma área de armazenamento de cópia de sombra.

O provedor do sistema pode expor o volume de produção, que pode ser gravado e lidos do normalmente. Quando a cópia de sombra for necessário, ele aplica logicamente as diferenças a dados no volume de produção para expor a cópia de sombra completa.

Para o provedor do sistema, a área de armazenamento de cópia de sombra deve ser em um volume NTFS. O volume a ser copiados em sombra não precisa ser um volume NTFS, mas pelo menos um volume montado no sistema deve ser um volume NTFS.

Os arquivos de componente que compõem o provedor do sistema são Swprv. dll e Volsnap. sys.

### <a name="in-box-vss-writers"></a>Na caixa gravadores VSS

O sistema operacional Windows inclui um conjunto de gravadores VSS que é responsável por enumerar os dados que são exigidos por vários recursos do Windows.

Para obter mais informações sobre esses criadores, consulte os seguintes sites:

  - [Os gravadores VSS caixa](http://go.microsoft.com/fwlink/?linkid=180895) (http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Novos gravadores VSS de caixa de entrada para o Windows Server 2008 e Windows Vista SP1](http://go.microsoft.com/fwlink/?linkid=180896) (http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Novos gravadores VSS de caixa de entrada para o Windows Server 2008 R2 e Windows 7](http://go.microsoft.com/fwlink/?linkid=180897) (http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Como as cópias de sombra são usadas

Além de fazer backup de informações de estado de sistema e dados de aplicativo, as cópias de sombra podem ser usadas para várias finalidades, incluindo o seguinte:

  - Restaurando LUNs (ressincronização de LUN e de troca de LUN)  
      
  - Restaurar arquivos individuais (cópias de sombra para pastas compartilhadas)  
      
  - Mineração de dados por meio de cópias de sombra transportáveis  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Restaurando LUNs (ressincronização de LUN e de troca de LUN)

No Windows Server 2008 R2 e Windows 7, os solicitadores do VSS podem usar um recurso da cópia de sombra do hardware provedor chamado ressincronização de LUN (ou "Ressincronização LUN"). Este é um esquema de recuperação rápida que permite que um administrador do aplicativo restaurar dados de uma cópia de sombra para o LUN original ou para um novo LUN.

A cópia de sombra pode ser um clone completo ou uma cópia de sombra diferencial. Em ambos os casos, no final da operação de ressincronização, o LUN de destino terá o mesmo conteúdo que a cópia de sombra LUN. Durante a operação de ressincronização, a matriz executa uma cópia de nível de bloco da cópia de sombra para o LUN de destino.


> [!NOTE]
> A cópia de sombra deve ser uma cópia de sombra transportáveis de hardware. 
<br>


A maioria das matrizes permitem que as operações de e/s de produção retomar logo após a operação de ressincronização é iniciada. Enquanto a operação de ressincronização está em andamento, leia as solicitações são redirecionadas para a cópia de sombra LUN e solicitações de gravação para o LUN de destino. Isso permite que as matrizes recuperar grandes conjuntos de dados e retomar as operações normais em vários segundos.

A ressincronização de LUN é diferente da troca de LUN. Uma troca LUN é um cenário de recuperação rápida VSS tem suporte desde o Windows Server 2003 SP1. Em uma troca LUN, a cópia de sombra é importada e, em seguida, convertida em um volume de leitura / gravação. A conversão é uma operação irreversível e o volume e o LUN subjacente não podem ser controlado com as APIs do VSS depois disso. A lista a seguir descreve como a ressincronização de LUN se compara com troca de LUN:

  - Na ressincronização de LUN, a cópia de sombra não é alterada, para que possa ser usada várias vezes. No LUN trocar, a cópia de sombra pode ser usada apenas uma vez para uma recuperação. Para os administradores mais preocupadas com segurança, isso é importante. Quando a ressincronização de LUN é usada, o solicitante pode repetir a operação de restauração inteira se algo der errado na primeira vez.  
      
  - No final de uma troca LUN, a cópia de sombra LUN é usada para solicitações de e/s de produção. Por esse motivo, a cópia de sombra LUN deve usar a mesma qualidade de armazenamento como o LUN de produção original para garantir que o desempenho não é afetado após a operação de recuperação. Se a ressincronização de LUN é usada em vez disso, o provedor de hardware pode manter a cópia de sombra no armazenamento que é mais barato do que o armazenamento de qualidade de produção.  
      
  - Se o destino do LUN não é utilizável e precisa ser recriado, troca de LUN pode ser mais econômico porque ele não requer um LUN de destino.  
      


> [!WARNING]
> Todas as operações listadas são operações de nível de LUN. Se você tentar recuperar um volume específico por meio de ressincronização de LUN, você pretende involuntariamente reverter todos os outros volumes que estão compartilhando o LUN. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Restaurar arquivos individuais (cópias de sombra para pastas compartilhadas)

Cópias de sombra de pastas compartilhadas usa o serviço de cópias de sombra de Volume para fornecer cópias de arquivos que estão localizados em um recurso de rede compartilhado, como um servidor de arquivos. Com cópias de sombra para pastas compartilhadas, os usuários podem recuperar rapidamente arquivos excluídos ou alterados que estão armazenados na rede. Porque eles podem fazer isso sem assistência do administrador, as cópias de sombra para pastas compartilhadas pode aumentar a produtividade e reduzir os custos administrativos.

Para obter mais informações sobre cópias de sombra para pastas compartilhadas, consulte [cópias de sombra de pastas compartilhadas](http://go.microsoft.com/fwlink/?linkid=180898) (http://go.microsoft.com/fwlink/?LinkId=180898) no TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Mineração de dados por meio de cópias de sombra transportáveis

Com um provedor de hardware que foi projetado para uso com o serviço de cópias de sombra de Volume, você pode criar cópias de sombra transportáveis do que podem ser importadas em servidores dentro do mesmo subsistema (por exemplo, uma rede SAN). Essas cópias de sombra podem ser usadas para propagar uma produção ou teste a instalação com dados somente leitura para mineração de dados.

Com o serviço de cópias de sombra de Volume e uma matriz de armazenamento com um provedor de hardware que foi projetado para uso com o serviço de cópias de sombra de Volume, é possível criar uma cópia de sombra do volume de dados de origem em um servidor e, em seguida, importe a cópia de sombra para outro servidor  (ou de volta para o mesmo servidor). Esse processo é realizado em poucos minutos, independentemente do tamanho dos dados. O processo de transporte é realizado por meio de uma série de etapas que usam um solicitante de cópia de sombra (um aplicativo de gerenciamento de armazenamento) que dá suporte a cópias de sombra transportáveis.

## <a name="to-transport-a-shadow-copy"></a>Para transportar uma cópia de sombra

1.  Crie uma cópia de sombra transportáveis da fonte de dados em um servidor.

2.  Importar a cópia de sombra para um servidor que está conectado à SAN (você pode importar para um servidor diferente ou mesmo servidor).

3.  Os dados agora estão prontos para ser usado.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   transporte entre dois servidores e criação de cópias de sombra


> [!NOTE]
> Uma cópia de sombra transportáveis do que é criada no Windows Server 2003 não pode ser importada em um servidor que está executando o Windows Server 2008 ou Windows Server 2008 R2. Uma cópia de sombra transportáveis do que foi criada no Windows Server 2008 ou Windows Server 2008 R2 não pode ser importada em um servidor que está executando o Windows Server 2003. No entanto, uma cópia de sombra é criada no Windows Server 2008 pode ser importada em um servidor que está executando o Windows Server 2008 R2 e vice-versa. 
<br>


As cópias de sombra são somente leitura. Se você quiser converter uma cópia de sombra em uma LUN de leitura/gravação, você pode usar um aplicativo de gerenciamento de armazenamento com base no serviço de disco Virtual (incluindo alguns solicitantes), além do serviço de cópias de sombra de Volume. Ao usar este aplicativo, você pode remover a cópia de sombra do gerenciamento de serviço de cópias de sombra de Volume e convertê-lo em uma LUN de leitura/gravação.

Transporte do serviço de cópias de sombra de volume é uma solução avançada em computadores que executam o Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 ou Windows Server 2008 R2. Ele funciona somente se houver um provedor de hardware na matriz de armazenamento. Transporte de cópia de sombra pode ser usado para várias finalidades, incluindo backups em fita, mineração de dados e teste.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

Estas perguntas Frequentes responde a perguntas sobre o Volume Shadow Copy Service (VSS) para administradores de sistema. Para obter informações sobre interfaces de programação de aplicativo do VSS, consulte [serviço de cópias de sombra de Volume](http://go.microsoft.com/fwlink/?linkid=180899) (http://go.microsoft.com/fwlink/?LinkId=180899) na biblioteca do Windows Developer Center.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>Quando o serviço de cópias de sombra de Volume foi introduzido? Em quais versões de sistema operacional do Windows está disponível?

O VSS foi introduzido no Windows XP. Ele está disponível no Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 e Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>O que é a diferença entre uma cópia de sombra e um backup?

No caso de um backup de disco rígido, a cópia de sombra criada também é o backup. Dados podem ser copiados desativar a cópia de sombra para uma restauração ou a cópia de sombra pode ser usada para um cenário de recuperação rápida — por exemplo, a ressincronização de LUN ou trocar de LUN.

Quando dados são copiados da cópia de sombra em fita ou outra mídia removível, o conteúdo que é armazenado na mídia que constitui o backup. A cópia de sombra pode ser excluída depois que os dados são copiados dele.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>O que é o maior volume de tamanho que dá suporte ao serviço de cópias de sombra de Volume?

Serviço de cópias de sombra de volume oferece suporte a um tamanho de volume de até 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>Fiz um backup no Windows Server 2008. Posso restaurá-los no Windows Server 2008 R2?

Isso depende do software de backup que você usou. A maioria dos programas de backup oferecer suporte a esse cenário para dados, mas não para backups de estado do sistema.

Cópias de sombra são criadas em qualquer uma dessas versões do Windows podem ser usadas por outro.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>Fiz um backup no Windows Server 2003. Posso restaurá-los no Windows Server 2008?

Isso depende do software de backup que você usou. Se você criar uma cópia de sombra no Windows Server 2003, você não pode usá-lo no Windows Server 2008. Além disso, se você criar uma cópia de sombra no Windows Server 2008, não é possível restaurá-lo no Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>Como desabilitar o VSS?

É possível desabilitar o serviço de cópias de sombra de Volume usando o Console de gerenciamento Microsoft. No entanto, você não deve fazer isso. Desabilitar o VSS negativamente afeta qualquer software que você usa o que depende dele, como a restauração do sistema e o Backup do Windows Server.

Para obter mais informações, consulte os seguintes sites da Web do Microsoft TechNet:

  - [A restauração do sistema](http://go.microsoft.com/fwlink/?linkid=157113) (http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Backup do Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>Pode excluir arquivos de uma cópia de sombra para economizar espaço?

O VSS é projetado para criar cópias de sombra de volumes inteiros. Arquivos temporários, como arquivos de paginação, são omitidos automaticamente cópias de sombra para economizar espaço.

Para excluir arquivos específicos de cópias de sombra, use a seguinte chave do registro: **FilesNotToSnapshot**.


> [!NOTE]
> O <STRONG>FilesNotToSnapshot</STRONG> chave do registro se destina a ser usado apenas por aplicativos. Os usuários que tentam usá-lo encontrará limitações, como o seguinte:
<br>
<UL>
<LI>Ele não é possível excluir arquivos de uma cópia de sombra foi criada em um servidor Windows usando o recurso de versões anteriores.<BR><BR>
<LI>Ele não é possível excluir arquivos de cópias de sombra para pastas compartilhadas.<BR><BR>
<LI>Ele pode excluir arquivos de uma cópia de sombra foi criada usando o [Diskshadow](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/diskshadow) utilitário, mas ele não é possível excluir arquivos de uma cópia de sombra foi criada usando o [Vssadmin](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/vssadmin) utilitário.<BR><BR>
<LI>Arquivos são excluídos de uma cópia de sombra em uma base de melhor esforço. Isso significa que eles não são garantidos para ser excluído.<BR><BR></LI></UL>


Para obter mais informações, consulte [excluindo arquivos de cópias de sombra](http://go.microsoft.com/fwlink/?linkid=180904) (http://go.microsoft.com/fwlink/?LinkId=180904) no MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Meu programa de backup não são da Microsoft falhou com um erro do VSS. O que pode fazer?

Confira a seção de suporte do produto do site da empresa que criou o programa de backup. Pode haver uma atualização de produto que você pode baixar e instalar para corrigir o problema. Caso contrário, entre em contato com o departamento de suporte de produto da empresa.

Os administradores do sistema podem usar as informações de solução de problemas do VSS no seguinte site da Microsoft TechNet Library para coletar informações de diagnóstico sobre problemas relacionados ao VSS.

Para obter mais informações, consulte [serviço de cópias de sombra de Volume](http://go.microsoft.com/fwlink/?linkid=180905) (http://go.microsoft.com/fwlink/?LinkId=180905) no TechNet.

### <a name="what-is-the-diff-area"></a>O que é a "área de comparação"?

A área de armazenamento de cópia de sombra (ou "área de comparação") é o local onde os dados para a cópia de sombra é criada pelo provedor de software de sistema são armazenados.

### <a name="where-is-the-diff-area-located"></a>Onde a área de comparação está localizada?

A área de comparação pode estar localizada em qualquer volume local. No entanto, ele deve estar localizado em um volume NTFS com espaço suficiente para armazená-lo.

### <a name="how-is-the-diff-area-location-determined"></a>Como é o local de área de comparação determinado?

Os seguintes critérios são avaliados nesta ordem, para determinar a localização de área de comparação:

  - Se um volume já tiver uma cópia de sombra existente, esse local será usado.  
      
  - Se houver uma associação manual pré-configurada entre o volume original e o local de volume de cópia de sombra, esse local é usado.  
      
  - Se os dois critérios anteriores não fornecerem um local, o serviço de cópias de sombra escolhe um local com base em espaço livre disponível. Se mais de um volume está sendo copiado de sombra, o serviço de cópias de sombra cria uma lista de locais de instantâneo possíveis com base no tamanho do espaço livre, em ordem decrescente. O número de locais fornecido é igual ao número de volumes que estão sendo copiados em sombra.  
      
  - Se o volume que estão sendo copiados em sombra é um dos locais possíveis, em seguida, uma associação local será criada. Caso contrário, uma associação com o volume com mais espaço disponível é criada.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>O VSS pode criar cópias de sombra de volumes não NTFS?

Sim. No entanto, cópias de sombra persistente podem ser feitas somente para volumes NTFS. Além disso, pelo menos um volume montado no sistema deve ser um volume NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>O que é o número máximo de cópias de sombra, que posso criar ao mesmo tempo?

O número máximo de volumes de cópia de sombra em um conjunto de cópias de sombra única é 64. Observe que isso não é igual ao número de cópias de sombra.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>Qual é o número máximo de cópias de sombra de software criado pelo provedor de sistema que eu possa manter para um volume?

O número máximo é de cópias de sombra para cada volume é 512. No entanto, por padrão você só pode manter 64 cópias de sombra que são usadas pelo recurso de cópias de sombra de pastas compartilhadas. Para alterar o limite para o recurso de cópias de sombra de pastas compartilhadas, use a seguinte chave do registro: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>Como controlar o espaço que é usado para o espaço de armazenamento de cópia de sombra?

Tipo de **vssadmin redimensionar shadowstorage** comando.

Para obter mais informações, consulte [Vssadmin redimensionar shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) no TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>O que acontece quando eu ficar sem espaço?

Cópias de sombra de volume são excluídas, começando com a cópia de sombra mais antiga.

## <a name="volume-shadow-copy-service-tools"></a>Ferramentas de serviço de cópia de sombra de volume

O sistema operacional Windows fornece as seguintes ferramentas para trabalhar com o VSS:

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

O DiskShadow está um solicitante VSS que você pode usar para gerenciar todos os instantâneos de hardware e software que você pode ter em um sistema. O DiskShadow inclui comandos como o seguinte:

  - **list**: Lista os gravadores VSS, provedores VSS e cópias de sombra  
      
  - **Criar**: Cria uma nova cópia de sombra  
      
  - **import**: Importa uma cópia de sombra transportáveis  
      
  - **expose**: Expõe uma cópia de sombra persistente (como uma letra de unidade, por exemplo)  
      
  - **revert**: Reverte o volume de volta para uma cópia de sombra especificada  
      

Essa ferramenta destina para uso por profissionais de TI, mas os desenvolvedores podem também ser útil ao testar um gravador VSS ou o provedor VSS.

O DiskShadow está disponível apenas em sistemas operacionais Windows Server. Ele não está disponível em sistemas de operacionais de cliente do Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin é usado para criar, excluir e listar as informações sobre cópias de sombra. Ele também pode ser usado para redimensionar a área de armazenamento de cópia de sombra ("área de comparação").

VssAdmin inclui comandos como o seguinte:

  - **Criar sombra**: Cria uma nova cópia de sombra  
      
  - **Excluir sombras**: Exclusões de cópias de sombra  
      
  - **Listar provedores**: Lista todos os provedores registrados do VSS  
      
  - **lista de gravadores**: Lista todos os inscritos gravadores VSS  
      
  - **Redimensionar shadowstorage**: Altera o tamanho máximo da área de armazenamento de cópia de sombra  
      

VssAdmin só pode ser usado para administrar as cópias de sombra são criadas pelo provedor de software de sistema.

VssAdmin está disponível no cliente do Windows e versões do sistema operacional Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Chaves do registro do Volume Shadow Copy Service

As seguintes chaves do registro estão disponíveis para uso com o VSS:

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Essa chave é usada para especificar quais usuários têm acesso às cópias de sombra.

Para obter mais informações, consulte as entradas a seguir no site do MSDN:

  - [Considerações de segurança para gravadores](http://go.microsoft.com/fwlink/?linkid=157739) (http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Considerações de segurança para os solicitantes](http://go.microsoft.com/fwlink/?linkid=180908) (http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Essa chave especifica o número máximo de cópias de sombra acessíveis ao cliente que podem ser armazenados em cada volume do computador. Cópias de sombra acessíveis ao cliente são usadas pelas cópias de sombra para pastas compartilhadas.

Para obter mais informações, consulte a seguinte entrada no site do MSDN:

**MaxShadowCopies** sob [chaves de registro para Backup e restauração](http://go.microsoft.com/fwlink/?linkid=180909) (http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Essa chave especifica o tamanho inicial mínimo, em MB, da área de armazenamento de cópia de sombra.

Para obter mais informações, consulte a seguinte entrada no site do MSDN:

**MinDiffAreaFileSize** sob [chaves de registro para Backup e restauração](http://go.microsoft.com/fwlink/?linkid=180910) (http://go.microsoft.com/fwlink/?LinkId=180910)

`##`#' Versões de sistema operacional com suporte

A tabela a seguir lista as versões mínima do sistema operacional com suporte para recursos VSS.


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
<td><p>Nenhum suporte</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p><strong>FilesNotToSnapshot</strong> chave do registro</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>Cópias de sombra transportáveis</p></td>
<td><p>Nenhum suporte</p></td>
<td><p>Windows Server 2003 com SP1</p></td>
</tr>
<tr class="even">
<td><p>Cópias de sombra de hardware</p></td>
<td><p>Nenhum suporte</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Versões anteriores do Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>Recuperação rápida de usar a troca LUN</p></td>
<td><p>Nenhum suporte</p></td>
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
<td>Esta é a capacidade de importar uma cópia de sombra mais de uma vez. Operação de importação de apenas um pode ser executada por vez.
<p></p></td>
</tr>
</tbody>
</table>
<p></p>
</div></td>
<td><p>Nenhum suporte</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Cópias de sombra de pastas compartilhadas</p></td>
<td><p>Nenhum suporte</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Cópias de sombra transportáveis recuperado automaticamente</p></td>
<td><p>Nenhum suporte</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sessões simultâneas de backup (até 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Restaurar único sessão simultânea com backups</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 com SP2</p></td>
</tr>
<tr class="even">
<td><p>Até 8 restauração de sessões simultâneas com backups</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Consulte também

[Serviço de cópias de sombra de volume na Central de desenvolvedores do Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)