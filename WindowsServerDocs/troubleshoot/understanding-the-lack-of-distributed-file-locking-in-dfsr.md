---
title: Entendendo (a falta de) bloqueio de arquivo distribuído no DFSR
description: Este artigo discute a ausência de um mecanismo de bloqueio de arquivo distribuído de vários hosts no Windows e, especificamente, em pastas replicadas pelo DFSR.
ms.prod: windows-server
ms.technology: server-general
ms.date: 06/10/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 739bcd1127877d4c9b1339b64270262d9d48634f
ms.sourcegitcommit: fa9a8badf4eb366aeeca7d2905e2cad711ee8dae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84714970"
---
# <a name="understanding-the-lack-of-distributed-file-locking-in-dfsr"></a>Entendendo (a falta de) bloqueio de arquivo distribuído no DFSR

Este artigo discute a ausência de um mecanismo de bloqueio de arquivo distribuído de vários hosts no Windows e, especificamente, em pastas replicadas pelo DFSR.
  
## <a name="some-background"></a>Alguns planos de fundo

  - Bloqueio de arquivo distribuído – refere-se ao conceito de ter várias cópias de um arquivo em vários computadores e quando um arquivo é aberto para gravação, todas as outras cópias são bloqueadas. Isso impede que um arquivo seja modificado em vários servidores ao mesmo tempo por vários usuários.
  - Replicação de Sistema de Arquivos Distribuído – o [DFSR](/previous-versions/windows/desktop/dfsr/distributed-file-system-replication--dfsr-) opera em um design de vários mestres com base no estado. Na replicação baseada em estado, cada servidor no sistema de vários mestres aplica atualizações à sua réplica à medida que elas chegam, sem trocar arquivos de log (em vez disso, ele usa vetores de versão para manter as informações de "atualização"). Nenhum servidor é sempre autoritativo arbitrariamente após a sincronização inicial, portanto, ele é altamente disponível e muito flexível em várias topologias de rede.
  - [SMB](/openspecs/windows_protocols/ms-smb/f210069c-7086-4dc2-885e-861d837df688) é o protocolo comum usado no Windows para acessar arquivos pela rede. Em termos simplificados, é um protocolo de cliente-servidor que usa um redirecionador para que os sistemas de arquivos remotos pareçam ser sistemas de arquivos locais. Ele não é específico do Windows e é muito comum – um exemplo de não-Microsoft conhecido é o samba, que permite que Linux, Mac e outros sistemas operacionais atuem como clientes/servidores SMB e participem de redes Windows.

  
É importante fazer uma delineação clara de onde o DFSR e o SMB residem em seu ambiente de dados replicado. O SMB permite que os usuários acessem seus arquivos e não tem nenhum reconhecimento do DFSR. Da mesma forma, o DFSR (usando o Protocolo RPC) mantém os arquivos sincronizados entre os servidores e não tem conscientização do SMB. Não confunda o bloqueio distribuído conforme definido nesta postagem e no [bloqueio oportunista](/windows/win32/fileio/opportunistic-locks).  
  
Então, aqui está onde as coisas podem ir para a forma de Pera, como diz a Brits.  
  
Como os usuários podem modificar dados em vários servidores e, como cada servidor do Windows só conhece um bloqueio de arquivo, *e* como o [DFSR não sabe nada sobre esses bloqueios em outros servidores](/previous-versions/windows/it-pro/windows-server-2003/cc773238(v=ws.10)), torna-se possível que os usuários substituam as alterações uns dos outros. O DFSR usa um algoritmo de conflito "último escritor vence", portanto, alguém precisa perder e a pessoa para salvar o último é manter suas alterações. A cópia do arquivo perdedor é parapiada na pasta *ConflictAndDeleted*  
  
Agora, isso é muito *menos* comum do que as pessoas gostam de acreditar. Normalmente, os verdadeiros arquivos compartilhados são modificados em um ambiente local; na filial ou na mesma linha de baias. Normalmente, eles são trabalhados por pessoas na mesma equipe, de modo que as pessoas geralmente sabem que os colegas estão modificando os dados. E, como eles geralmente estão no mesmo site, as chances são muito maiores que todos os usuários que trabalham em um documento compartilhado usarão o mesmo servidor. O Windows SMB lida com a situação aqui. Quando um usuário tiver um arquivo bloqueado para modificação e seu colega tentar editá-lo, o outro usuário receberá um erro como:  
  
![Arquivo em uso](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/1.jpg)
  
E se o aplicativo que abre o arquivo for realmente inteligente, como o Word 2007, ele poderá lhe fornecer:  
  
![Arquivo em uso](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/2.jpg) 
  
O DFSR tem um mecanismo para arquivos bloqueados, mas está apenas dentro do próprio contexto do servidor. O DFSR não replicará um arquivo para dentro ou para fora se sua cópia local tiver um bloqueio exclusivo. Mas isso não impede que ninguém em outro servidor modifique o arquivo.  
  
De volta ao tópico, o problema dos dados compartilhados *que estão sendo* modificados geograficamente existe, e para algumas pessoas é muito gnarly. Ocasionalmente, é perguntado por que o DFSR não lida com esse bloqueio e tira tudo com uma onda da varinha mágica. Acontece que esse é um cenário interessante e difícil de resolver para um sistema de replicação de vários mestres. Vamos explorar.  
  
## <a name="third-party-solutions"></a>Soluções de terceiros
  
Há algumas soluções de fornecedor que assumem esse problema, que normalmente lidam com um ou mais dos seguintes métodos \* :  

  - Uso de um mecanismo de agente

> Ter um "Cop" de tráfego central permite que um servidor esteja ciente de todos os outros servidores e quais arquivos eles bloquearam por usuários. Infelizmente, isso também significa que geralmente há um único ponto de falha no sistema de bloqueio distribuído.

![Topologia](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/3.png) 

  - Requisito para uma rede totalmente roteada

> Como um agente central deve ser capaz de se comunicar com todos os servidores que participam da replicação de arquivos, isso remove a capacidade de lidar com topologias de rede complexas. Topologias de anel e topologias de vários hubs e spokes geralmente não são possíveis. Em uma rede não totalmente roteada, alguns servidores podem não conseguir entrar em contato diretamente entre si ou com um agente e só podem conversar com um parceiro que possa se comunicar com outro servidor – e assim por diante. Isso é bom em um ambiente de vários mestres, mas não com um mecanismo de agente.

![Topologia](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/4.png)

  - São limitados a um par de servidores

> Algumas soluções limitam a topologia a um par de servidores a fim de simplificar o mecanismo de bloqueio distribuído. Para ambientes maiores, isso pode não ser viável.

  - Fazer uso de agentes em clientes e servidores
  - Não usar replicação de vários mestres
  - Não fazer uso do MS clustering
  - Fazer uso de dispositivos especializados

  
***\*** Observe que eu digo normalmente \! não poste ameaças de morte, pois você tem uma solução que não implementa um ou mais desses métodos\!*  
  
## <a name="deeper-thoughts"></a>Pensamentos mais aprofundados  
  
Como você imagina mais sobre esse problema, alguns problemas fundamentais começam a ser cortados. Por exemplo, se tivermos quatro servidores com dados que podem ser modificados por usuários em quatro sites e a conexão WAN com um deles ficar offline, o que fazemos? Os usuários ainda podem acessar seus servidores individuais, mas deveriam permiti-los? Não queremos que eles façam alterações que estejam em conflito, mas, definitivamente, queremos que eles continuem a trabalhar e tornando nosso dinheiro na empresa. Se bloquearmos arbitrariamente as alterações nesse ponto, nenhum usuário poderá trabalhar mesmo que, na verdade, pode não haver nenhum conflito acontecendo\! Não há como informar aos outros servidores que o arquivo está em uso e que você está de volta ao quadrado.  
  
![Topologia](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/5.png)
  
Em seguida, há o próprio SMB e o tratamento de erros de bloqueios de relatórios. Na verdade, não podemos alterar como os relatórios SMB compartilham violações à medida que violamos uma tonelada de aplicativos e os clientes não entendem novas mensagens de erro estendidas mesmo assim. Aplicativos como o Word 2007 fazem um truque Undercover para descobrir quem está bloqueando arquivos, mas a grande maioria dos aplicativos não sabe quem tem um arquivo em uso (ou até mesmo que o SMB exista. Realmente.). Assim, quando um usuário recebe a mensagem ' este arquivo está em uso ', isso não é particularmente acionável – caso todos chamem o suporte técnico? O suporte técnico tem acesso a todos os servidores de arquivos para ver quais usuários estão acessando arquivos? Confuso.  
  
Como queremos vários mestres para alta disponibilidade, um sistema de agente é menos desejável; Talvez seja necessário ter algo em execução em todos os servidores que permita que todos se comuniquem mesmo por meio de redes não totalmente roteadas. Isso exigirá técnicas de sincronização muito complexas. Ele adicionará alguma sobrecarga à rede (embora provavelmente não seja muito) e precisará ser um raio rápido para garantir que não estamos mantendo o usuário em seu trabalho; Ele precisa OutRun a própria replicação de arquivo, na verdade, talvez seja necessário estar vinculado à replicação de alguma forma. Ele também precisará levar em conta as interrupções do servidor que estão relacionadas à rede e não falhas do servidor, de alguma forma.  

![Topologia](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/6.png)
  
E, em seguida, voltamos ao software cliente especial para esse cenário que melhor entende os bloqueios e pode dar ao usuário algumas informações úteis ("chamar Susana em contabilidade e informar a ela para liberar esse documento", "a topologia de bloqueio de arquivo está interrompida e o administrador está impedindo que você abra esse arquivo até que ele seja corrigido", etc). Fazer isso funcionar bem com milhões de aplicativos em execução no Windows certamente será interessante. Há muitos sistemas operacionais que não teriam suporte ou que o software – o Windows 2000 está fora do suporte base e XP em breve será. Os clientes Linux e Mac não teriam esse software até que eles achassem que era importante, de modo que o cliente teria que esperar que seus fornecedores tivessem algo análogo.  
  
## <a name="more-inforamtion"></a>Mais informações
  
Agora, a maneira mais fácil de controlar essa situação no DFSR é usar namespaces do DFS para orientar os usuários em locais previsíveis, com um namespace consistente. Ao configurar corretamente a topologia do site do DFSN e os links de servidor, você força os usuários a compartilharem o mesmo servidor local e permitirão que eles acessem computadores remotos somente quando o servidor ' Main ' estiver inoperante. Para a maioria dos ambientes, isso funciona bem. Alternativa ao DFSR, o SharePoint é uma opção devido ao seu sistema de check-out/check-in. O BranchCache (proveniente do Windows Server 2008 R2 e do Windows 7) pode ser uma opção para você, pois foi projetado para facilitar a leitura de arquivos em um cenário de ramificação, mas no final, os dados autoritativos ainda residirão em apenas um servidor – mais sobre isso [aqui](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127252(v=ws.11)). E, mais uma vez, esses fornecedores têm suas soluções.

