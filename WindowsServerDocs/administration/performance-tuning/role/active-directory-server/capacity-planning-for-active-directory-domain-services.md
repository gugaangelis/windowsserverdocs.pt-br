---
title: Planejamento de capacidade para Active Directory Domain Services
description: Discussão detalhada dos fatores a serem considerados durante o planejamento de capacidade para AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 5a9e2d39d4eedd1e8fdb4bfeaf267ad4cb4c596a
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "67799828"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Planejamento de capacidade para Active Directory Domain Services

Este tópico foi escrito originalmente por Ken Brumfield, engenheiro de campo Premier sênior na Microsoft e fornece recomendações para o planejamento de capacidade para Active Directory Domain Services (AD DS).

## <a name="goals-of-capacity-planning"></a>Metas de planejamento de capacidade

O planejamento de capacidade não é o mesmo que solucionar problemas de incidentes de desempenho. Eles estão fortemente relacionados, mas muito diferentes. Os objetivos do planejamento de capacidade são:  

- Implemente e opere corretamente um ambiente 
- Minimize o tempo gasto na solução de problemas de desempenho.  
  
No planejamento de capacidade, uma organização pode ter um destino de linha de base de 40% de utilização do processador durante períodos de pico para atender aos requisitos de desempenho do cliente e acomodar o tempo necessário para atualizar o hardware no datacenter. Enquanto que, para ser notificado sobre incidentes de desempenho anormal, um limite de alerta de monitoramento pode ser definido em 90% por um intervalo de 5 minutos.

A diferença é que, quando um limite de gerenciamento de capacidade é continuamente excedido (um evento único não é uma preocupação), a adição de capacidade (ou seja, adição em processadores mais rápidos) seria uma solução ou o dimensionamento do serviço em vários servidores seria um soluções. Os limites de alerta de desempenho indicam que a experiência do cliente está sofrendo no momento e são necessárias etapas imediatas para resolver o problema.

Como uma analogia: o gerenciamento de capacidade está prestes a evitar um acidente de carro (condução defensiva, verificando se os freios estão funcionando corretamente e assim por diante) enquanto a solução de problemas de desempenho é o que a polícia, o departamento de incêndio e os profissionais médicos de emergência fazem após um acidente. Isso é sobre a "condução defensiva", estilo Active Directory.

Nos últimos anos, as diretrizes de planejamento de capacidade para sistemas de expansão mudaram drasticamente. As seguintes alterações nas arquiteturas do sistema enfrentam suposições fundamentais sobre a criação e o dimensionamento de um serviço:

- plataformas de servidor de 64 bits  
- Virtualização  
- Maior atenção ao consumo de energia  
- Armazenamento SSD  
- Cenários de nuvem  

Além disso, a abordagem está mudando de um exercício de planejamento de capacidade baseado em servidor para um exercício de planejamento de capacidade baseado em serviço. Active Directory Domain Services (AD DS), um serviço distribuído e maduro que muitos produtos da Microsoft e de terceiros usam como back-end, se torna um dos produtos mais críticos a serem planejados corretamente para garantir a capacidade necessária para que outros aplicativos sejam executados.

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>Requisitos de linha de base para diretrizes de planejamento de capacidade

Ao longo deste artigo, são esperados os seguintes requisitos de linha de base:

- Os leitores têm lido e estão familiarizados com as [diretrizes de ajuste de desempenho para o Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- A plataforma Windows Server é uma arquitetura baseada em x64. Mas mesmo que seu ambiente de Active Directory esteja instalado no Windows Server 2003 x86 (agora além do fim do ciclo de vida do suporte) e tenha uma DIT (árvore de informações de diretório) com menos de 1,5 GB de tamanho e que possa ser facilmente mantida na memória, as diretrizes desse o artigo ainda é aplicável.
- O planejamento de capacidade é um processo contínuo e você deve examinar regularmente o quão bem o ambiente está atendendo às expectativas.
- A otimização ocorrerá em vários ciclos de vida de hardware à medida que os custos de hardware forem alterados. Por exemplo, a memória se torna mais barata, o custo por núcleo diminui ou o preço das diferentes opções de armazenamento mudam.
- Planejar o pico do período de ocupado do dia. É recomendável observar isso em intervalos de 30 minutos ou de hora. Qualquer coisa maior pode ocultar os picos reais e qualquer coisa menos pode ser distorcida por "picos transitórios".
- Planeje o crescimento no decorrer do ciclo de vida do hardware para a empresa. Isso pode incluir uma estratégia de atualização ou adição de hardware de maneira escalonada ou uma atualização completa a cada três a cinco anos. Cada uma delas exigirá uma "adivinhação" à medida que a carga em Active Directory aumentará. Os dados históricos, se coletados, ajudarão nessa avaliação. 
- Planejar a tolerância a falhas. Depois que uma estimativa *N* for derivada, planeje cenários que incluam *n* &ndash; 1 *, n* &ndash; 2, *n* &ndash; *x*.
  - Adicione servidores adicionais de acordo com a necessidade organizacional para garantir que a perda de um único ou de vários servidores não exceda as estimativas máximas de capacidade de pico.
  - Considere também que o plano de crescimento e o plano de tolerância a falhas precisam ser integrados. Por exemplo, se um controlador de domínio for necessário para dar suporte à carga, mas a estimativa for que a carga será dobrada no próximo ano e exigirá dois DCs no total, não haverá capacidade suficiente para dar suporte à tolerância a falhas. A solução seria iniciar com três DCs. Também é possível planejar adicionar o terceiro DC após 3 ou 6 meses se os orçamentos forem apertados.

    >[!NOTE]
    >Adicionar aplicativos com reconhecimento de Active Directory pode ter um impacto perceptível na carga do DC, quer a carga seja proveniente dos clientes ou servidores de aplicativos.

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>Processo de três etapas para o ciclo de planejamento de capacidade

No planejamento de capacidade, primeiro decida qual qualidade de serviço é necessária. Por exemplo, um datacenter principal dá suporte a um nível mais alto de simultaneidade e requer uma experiência mais consistente para usuários e aplicativos de consumo, o que exige maior atenção à redundância e à minimização de gargalos do sistema e da infraestrutura. Por outro lado, um local satélite com alguns usuários não precisa do mesmo nível de simultaneidade ou tolerância a falhas. Portanto, o escritório de satélite pode não precisar de muita atenção para otimizar o hardware e a infraestrutura subjacentes, o que pode levar à economia de custos. Todas as recomendações e diretrizes neste documento são para o desempenho ideal e podem ser liberadas seletivamente para cenários com requisitos menos exigentes.

A próxima pergunta é: virtualizada ou física? De uma perspectiva de planejamento de capacidade, não há nenhuma resposta correta ou incorreta; Há apenas um conjunto diferente de variáveis com as quais trabalhar. Os cenários de virtualização são fornecidos para uma das duas opções:

- "Mapeamento direto" com um convidado por host (em que a virtualização existe unicamente para abstrair o hardware físico do servidor)
- "Host compartilhado"

Cenários de teste e produção indicam que o cenário de "mapeamento direto" pode ser tratado de forma idêntica a um host físico. "Host compartilhado", no entanto, apresenta várias considerações escritas em mais detalhes posteriormente. O cenário "host compartilhado" significa que AD DS também está competindo por recursos, e há penalidades e considerações de ajuste para fazer isso.

Com essas considerações em mente, o ciclo de planejamento de capacidade é um processo de três etapas iterativas:

1. Meça o ambiente existente, determine onde estão os gargalos do sistema atualmente e obtenha noções básicas ambientais necessárias para planejar a quantidade de capacidade necessária.
1. Determine o hardware necessário de acordo com os critérios descritos na etapa 1.
1. Monitore e valide se a infraestrutura conforme implementada está operando nas especificações. Alguns dados coletados nesta etapa se tornam a linha de base para o próximo ciclo de planejamento de capacidade.

### <a name="applying-the-process"></a>Aplicando o processo

Para otimizar o desempenho, verifique se esses principais componentes estão corretamente selecionados e ajustados para as cargas de aplicativo:

1. Memória
1. Rede
1. Armazenamento
1. Processador
1. Logon de Rede

Os requisitos de armazenamento básico do AD DS e o comportamento geral do software cliente bem escrito permitem que os ambientes com até 10.000 a 20.000 usuários tenham um investimento pesado no planejamento de capacidade com relação ao hardware físico, como quase qualquer servidor moderno o sistema de classe tratará a carga. Dito isso, a tabela a seguir resume como avaliar um ambiente existente a fim de selecionar o hardware certo. Cada componente é analisado em detalhes nas seções subsequentes para ajudar AD DS os administradores a avaliarem sua infraestrutura usando as recomendações de linha de base e as entidades específicas do ambiente.

Em geral:

- Qualquer dimensionamento com base nos dados atuais só será preciso para o ambiente atual.
- Para todas as estimativas, espere a demanda para aumentar o ciclo de vida do hardware.
- Determine se deve exceder o tamanho atual e se expandir no ambiente maior ou adicionar capacidade ao ciclo de vida.
- Para a virtualização, todas as mesmas entidades e metodologias de planejamento de capacidade se aplicam, exceto pelo fato de que a sobrecarga da virtualização precisa ser adicionada a qualquer item relacionado ao domínio.
- O planejamento de capacidade, como qualquer coisa que tente prever, não é uma ciência precisa. Não espere coisas para calcular perfeitamente e com precisão de 100%. As diretrizes aqui são a recomendação mais simples; Adicione capacidade para segurança adicional e valide continuamente se o ambiente permanece no destino.

### <a name="data-collection-summary-tables"></a>Tabelas de Resumo de coleta de dados

#### <a name="new-environment"></a>Novo ambiente

| Componente | Estimativas |
|-|-|
|Tamanho do armazenamento/banco de dados|40 KB a 60 KB para cada usuário|
|RAM|Tamanho do Banco de Dados<br />Recomendações do sistema operacional base<br />Aplicativos de terceiros|
|Rede|1 GB|
|CPU|1000 usuários simultâneos para cada núcleo|

#### <a name="high-level-evaluation-criteria"></a>Critérios de avaliação de alto nível

| Componente | Critérios de avaliação | Considerações de planejamento |
|-|-|-|
|Tamanho do armazenamento/banco de dados|A seção intitulada "para ativar o registro em log do espaço em disco liberado pela desfragmentação" nos [limites de armazenamento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Desempenho de armazenamento/banco de dados|<ul><li>"LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/leitura", "LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/gravação", LogicalDisk ( *\<unidade de banco de dados NTDS )\>* \Avg de disco s/transferência "</li><li>"LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \ leituras/s", "LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \ gravações/s", "LogicalDisk ( *\<unidade\>debancodedadosNTDS)* ) \ Transferências/s "</li></ul>|<ul><li>O armazenamento tem duas preocupações para abordar<ul><li>Espaço disponível, que com o tamanho do armazenamento baseado em SSD e com base no eixo de hoje é irrelevante para a maioria dos ambientes do AD.</li> <li>Operações de e/s (entrada/saída) disponíveis – em muitos ambientes, isso geralmente é ignorado. Mas é importante avaliar apenas ambientes em que não há RAM suficiente para carregar todo o banco de dados NTDS na memória.</li></ul><li>O armazenamento pode ser um tópico complexo e deve envolver a experiência do fornecedor de hardware para o dimensionamento adequado. Particularmente com cenários mais complexos, como cenários de SAN, NAS e iSCSI. No entanto, em geral, o custo por gigabyte de armazenamento geralmente está no oposição direto ao custo por e/s:<ul><li>O RAID 5 tem um custo menor por gigabyte do que o RAID 1, mas o RAID 1 tem um custo menor por e/s</li><li>Unidades de disco rígido com base no eixo têm menor custo por gigabyte, mas o SSDs tem um custo menor por e/s</li></ul><li>Após a reinicialização do computador ou do serviço de Active Directory Domain Services, o cache do ESE (mecanismo de armazenamento extensível) estará vazio e o desempenho será vinculado ao disco enquanto o cache estiver quente.</li><li>Na maioria dos ambientes, O AD tem e/s de leitura intensiva em um padrão aleatório para discos, negando grande parte do benefício das estratégias de otimização de cache e leitura.  Além disso, o AD tem um modo de cache maior na memória do que a maioria dos caches do sistema de armazenamento.</li></ul>
|RAM|<ul><li>Tamanho do banco de dados</li><li>Recomendações do sistema operacional base</li><li>Aplicativos de terceiros</li></ul>|<ul><li>O armazenamento é o componente mais lento em um computador. Quanto mais que puder ser residente na RAM, menos será necessário ir para o disco.</li><li>Certifique-se de que RAM suficiente esteja alocada para armazenar o sistema operacional, agentes (antivírus, backup, monitoramento), banco de dados NTDS e crescimento ao longo do tempo.</li><li>Para ambientes em que a maximização da quantidade de RAM não é econômica (como locais de satélite) ou não viável (DIT é muito grande), faça referência à seção de armazenamento para garantir que o armazenamento seja dimensionado corretamente.</li></ul>|
|Rede|<ul><li>"Interface de rede\*() \Bytes recebidos/s"</li><li>"Interface de rede\*() \Bytes enviados/s"|<ul><li>Em geral, o tráfego enviado de um controlador de domínio excede muito o tráfego enviado para um controlador de domínio.</li><li>Como uma conexão Ethernet comutada é full-duplex, o tráfego de entrada e saída de rede precisa ser dimensionado independentemente.</li><li>Consolidar o número de controladores de domínio aumentará a quantidade de largura de banda usada para enviar respostas de volta às solicitações do cliente para cada DC, mas será quase o bastante para linear para o site como um todo.</li><li>Se você remover os DCs do local satélite, não se esqueça de adicionar a largura de banda para o controlador de domínio satélite aos DCs do Hub, bem como usá-lo para avaliar a quantidade de tráfego de WAN que haverá.</li></ul>|
|CPU|<ul><li>"Disco lógico ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/leitura"</li><li>"Processo (Lsass)\\% tempo do processador"</li></ul>|<ul><li>Depois de eliminar o armazenamento como um afunilamento, resolva a quantidade de energia de computação necessária.</li><li>Embora não seja perfeitamente linear, o número de núcleos de processador consumidos em todos os servidores dentro de um escopo específico (como um site) pode ser usado para medir quantos processadores são necessários para dar suporte à carga total do cliente. Adicione o mínimo necessário para manter o nível de serviço atual em todos os sistemas dentro do escopo.</li><li>Alterações na velocidade do processador, incluindo alterações relacionadas ao gerenciamento de energia, os números de impacto derivados do ambiente atual. Em geral, é impossível avaliar com precisão como a saída de um processador de 2,5 GHz para um processador de 3 GHz reduzirá o número de CPUs necessárias.</li></ul>|
|Logon de Rede|<ul><li>"Netlogon (\*) \Semaphore aquisições"</li><li>"Tempo limite\*de \Semaphore de Netlogon"</li><li>"Tempo de\*espera de semáforo \"</li></ul>|<ul><li>NET logon Secure Channel/MaxConcurrentAPI afeta apenas ambientes com autenticações NTLM e/ou validação PAC. A validação da PAC está ativada por padrão nas versões do sistema operacional anteriores ao Windows Server 2008. Essa é uma configuração do cliente, portanto, os DCs serão afetados até que isso seja desativado em todos os sistemas cliente.</li><li>Ambientes com autenticação de confiança cruzada significativa, que inclui relações de confiança entre florestas, têm maior risco se não forem dimensionados corretamente.</li><li>As consolidações do servidor aumentarão a simultaneidade da autenticação de confiança cruzada.</li><li>Os surtos precisam ser acomodados, como failovers de cluster, à medida que os usuários reautenticam em massa para o novo nó de cluster.</li><li>Os sistemas cliente individuais (como um cluster) também podem precisar de ajuste.</li></ul>|

## <a name="planning"></a>Planejamento

Por muito tempo, a recomendação da Comunidade para o dimensionamento de AD DS tem sido "colocar o máximo de RAM como o tamanho do banco de dados". Na maior parte, essa recomendação é tudo que a maioria dos ambientes precisa estar preocupado. Mas o ecossistema que consome AD DS ficou muito maior, como tem os ambientes de AD DS em si, desde sua introdução em 1999. Embora o aumento no poder de computação e o switch de arquiteturas x86 para arquiteturas x64 tenham tornado os aspectos mais sutis de dimensionamento para o desempenho irrelevante para um conjunto maior de clientes que executam AD DS em hardware físico, o crescimento da virtualização tem reintroduziu as preocupações de ajuste para um público maior do que antes.

As diretrizes a seguir tratam de como determinar e planejar as demandas de Active Directory como um serviço, independentemente de serem implantadas em um físico, um mix virtual/físico ou um cenário puramente virtualizado. Dessa forma, dividiremos a avaliação em cada um dos quatro componentes principais: armazenamento, memória, rede e processador. Em suma, para maximizar o desempenho na AD DS, o objetivo é chegar ao próximo limite possível do processador.

## <a name="ram"></a>RAM

Simplesmente, quanto mais que puder ser armazenado em cache na RAM, menos será necessário ir para o disco. Para maximizar a escalabilidade do servidor, a quantidade mínima de RAM deve ser a soma do tamanho atual do banco de dados, o tamanho total do SYSVOL, o valor recomendado do sistema operacional e as recomendações do fornecedor para os agentes (antivírus, monitoramento, backup e assim por diante) ). Um valor adicional deve ser adicionado para acomodar o crescimento durante o tempo de vida do servidor. Isso será ambientalmente subjetivo com base nas estimativas de crescimento do banco de dados com base nas alterações ambientais.

Para ambientes em que a maximização da quantidade de RAM não é econômica (como locais de satélite) ou não viável (DIT é muito grande), faça referência à seção de armazenamento para garantir que o armazenamento seja projetado corretamente.

Um registro que surge no contexto geral na memória de dimensionamento é o dimensionamento do arquivo de paginação. No mesmo contexto que qualquer outra memória relacionada, o objetivo é minimizar o processo de disco muito mais lento. Portanto, a pergunta deve ir de "como o arquivo de paginação deve ser dimensionado?" para "a quantidade de RAM necessária para minimizar a paginação?" A resposta para a última pergunta é descrita no restante desta seção. Isso deixa a maior parte da discussão sobre o dimensionamento do arquivo de paginação para o realm das recomendações gerais do sistema operacional e a necessidade de configurar o sistema para despejos de memória, que não estão relacionados ao desempenho de AD DS.

### <a name="evaluating"></a>Avaliar

A quantidade de RAM de que um DC (controlador de domínio) precisa é, na verdade, um exercício complexo por estes motivos:

- Alto potencial de erro ao tentar usar um sistema existente para medir a quantidade de RAM necessária, pois o LSASs reduzirá as condições de pressão de memória, artificialmente a necessidade.
- O fato subjetivo de que um controlador de domínio individual precisa apenas armazenar em cache o que é "interessante" para seus clientes. Isso significa que os dados que precisam ser armazenados em cache em um controlador de domínio em um site com apenas um servidor Exchange serão muito diferentes dos dados que precisam ser armazenados em cache em um DC que autentica apenas os usuários.
- O trabalho de avaliar a RAM para cada DC caso a caso é proibitivo e muda à medida que o ambiente é alterado.
- Os critérios por trás da recomendação ajudarão a tomar decisões informadas: 
- Quanto mais que puder ser armazenado em cache na RAM, menos será necessário ir para o disco. 
- O armazenamento é, de longe, o componente mais lento de um computador. O acesso a dados na mídia de armazenamento SSD e baseada em eixo está na ordem de 1, 000, 1.000 vezes mais lenta do que o acesso aos dados na RAM.

Portanto, para maximizar a escalabilidade do servidor, a quantidade mínima de RAM é a soma do tamanho do banco de dados atual, o tamanho total do SYSVOL, o valor recomendado do sistema operacional e as recomendações de fornecedor para os agentes (antivírus, monitoramento, backup, e assim por diante). Adicione outros valores para acomodar o crescimento durante o tempo de vida do servidor. Isso será ambientalmente subjetivo com base nas estimativas de crescimento do banco de dados. No entanto, para locais de satélite com um pequeno conjunto de usuários finais, esses requisitos podem ser reduzidos, pois esses sites não precisarão armazenar em cache o máximo para atender à maioria das solicitações.

Para ambientes em que a maximização da quantidade de RAM não é econômica (como locais de satélite) ou não viável (DIT é muito grande), faça referência à seção de armazenamento para garantir que o armazenamento seja dimensionado corretamente.

> [!NOTE]
> Um registro durante o dimensionamento da memória é o dimensionamento do arquivo de paginação. Como o objetivo é minimizar a ir para o disco muito mais lento, a pergunta vai de "como o arquivo de paginação deve ser dimensionado?" para "a quantidade de RAM necessária para minimizar a paginação?" A resposta para a última pergunta é descrita no restante desta seção. Isso deixa a maior parte da discussão sobre o dimensionamento do arquivo de paginação para o realm das recomendações gerais do sistema operacional e a necessidade de configurar o sistema para despejos de memória, que não estão relacionados ao desempenho de AD DS.

### <a name="virtualization-considerations-for-ram"></a>Considerações sobre virtualização para RAM

Evite a confirmação da memória no host. A meta fundamental por trás da otimização da quantidade de RAM é minimizar a quantidade de tempo gasto em disco. Em cenários de virtualização, o conceito de excesso de confirmações de memória existe quando mais RAM é alocada para os convidados e, em seguida, existe no computador físico. Isso, por si só, não é um problema. Ele se torna um problema quando a memória total usada ativamente por todos os convidados excede a quantidade de RAM no host e o host subjacente inicia a paginação. O desempenho se torna ligado ao disco nos casos em que o controlador de domínio está indo para o NTDS. dit para obter dados, ou o controlador de domínio está indo para o arquivo de paginação para obter dados ou o host está em disco para obter dados que o convidado pensa que está na RAM.

### <a name="calculation-summary-example"></a>Exemplo de Resumo de cálculo

|Componente|Memória estimada (exemplo)|
|-|-|
|RAM recomendada do sistema operacional de base (Windows Server 2008)|2 GB|
|Tarefas internas do LSASs|200 MB|
|Agente de monitoramento|100 MB|
|Antivírus|100 MB|
|Banco de dados (catálogo global)|8,5 GB tem certeza de que???|
|Amortecedor para execução de backup, administradores para fazer logon sem impacto|1 GB|
|Total|12 GB|

**Aconselhável 16 GB**

Com o passar do tempo, a suposição pode ser feita de que mais dados serão adicionados ao banco e o servidor provavelmente estará em produção por três a cinco anos. Com base em uma estimativa de crescimento de 33%, 16 GB seria uma quantidade razoável de RAM a ser colocada em um servidor físico. Em uma máquina virtual, considerando a facilidade com que as configurações podem ser modificadas e a RAM pode ser adicionada à VM, a partir de 12 GB, com o plano para monitorar e atualizar no futuro, é razoável.

## <a name="network"></a>Rede

### <a name="evaluating"></a>Avaliar
Esta seção tem menos informações sobre a avaliação das demandas relacionadas ao tráfego de replicação, que se concentram no tráfego atravessando a WAN e são totalmente cobertas em [Active Directory tráfego de replicação](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), do que diz respeito à avaliação da largura de banda total e da rede capacidade necessária, inclusive consultas de clientes, Política de Grupo aplicativos e assim por diante. Para ambientes existentes, isso pode ser coletado usando contadores de desempenho "interface de\*r(ede) \Bytes recebidos/s" e "interface de\*(rede) \Bytes enviados/s". Intervalos de exemplo para contadores de interface de rede em 15, 30 ou 60 minutos. Qualquer coisa menor geralmente será muito volátil para boas medidas; qualquer coisa maior suavizará as exibições diárias em excesso.

> [!NOTE]
> Em geral, a maior parte do tráfego de rede em um DC é de saída, pois o DC responde às consultas do cliente. Esse é o motivo para o foco no tráfego de saída, embora seja recomendável avaliar cada ambiente também para o tráfego de entrada. As mesmas abordagens podem ser usadas para abordar e revisar os requisitos de tráfego de rede de entrada. Para obter mais informações, consulte o artigo [929851 da base de dados de conhecimento: O intervalo de portas dinâmicas padrão para TCP/IP foi alterado no Windows Vista e no Windows](http://support.microsoft.com/kb/929851)Server 2008.

### <a name="bandwidth-needs"></a>Necessidades de largura de banda

Planejar a escalabilidade de rede abrange duas categorias distintas: a quantidade de tráfego e a carga de CPU do tráfego de rede. Cada um desses cenários é direcionado diretamente em comparação com alguns dos outros tópicos deste artigo.

Para avaliar a quantidade de tráfego que deve ter suporte, há duas categorias exclusivas de planejamento de capacidade para AD DS em termos de tráfego de rede. A primeira é o tráfego de replicação que percorre os controladores de domínio e é abordado minuciosamente na referência [Active Directory tráfego de replicação](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) e ainda é relevante para as versões atuais do AD DS. O segundo é o tráfego de cliente para servidor intra-site. Um dos cenários mais simples para planejar, o tráfego intra-site recebe predominantemente pequenas solicitações de clientes em relação às grandes quantidades de dados enviados de volta aos clientes. 100 MB geralmente serão adequados em ambientes de até 5.000 usuários por servidor, em um site. O uso de um adaptador de rede de 1 GB e o suporte ao ajuste lateral (RSS) é recomendado para qualquer coisa acima de 5.000 usuários. Para validar esse cenário, especialmente no caso de cenários de consolidação de servidor, examine a interface de\*rede () \ Bytes/s em todos os DCS de um site, adicione-os juntos e divida pelo número de destino dos controladores de domínio para garantir que é a capacidade adequada. A maneira mais fácil de fazer isso é usar a exibição "área empilhada" no monitor de desempenho e confiabilidade do Windows (anteriormente conhecido como perfmon), garantindo que todos os contadores sejam dimensionados da mesma forma.

Considere o exemplo a seguir (também conhecido como uma maneira realmente complexa de validar que a regra geral é aplicável a um ambiente específico). As seguintes suposições são feitas:

- O objetivo é reduzir o volume máximo possível de servidores. O ideal é que um servidor carregue a carga e um servidor adicional seja implantado para redundância (*N* + 1 cenário). 
- Nesse cenário, o adaptador de rede atual dá suporte a apenas 100 MB e está em um ambiente alternado.  
  A utilização máxima de largura de banda de rede de destino é 60% em um cenário N (perda de um DC).
- Cada servidor tem cerca de 10.000 clientes conectados a ele.

Conhecimento obtido dos dados no gráfico (interface de rede (\*) \Bytes enviados/s):

1. O dia útil começa a subir em cerca de 5:30 e termina em 7:00 PM.
1. O período de pico mais ocupado é de 8:00 às 8:15, com mais de 25 bytes enviados/s no controlador de domínio mais ocupado.  
   > [!NOTE]
   > Todos os dados de desempenho são históricos. Portanto, o ponto de dados de pico em 8:15 indica a carga de 8:00 a 8:15.
1. Há picos antes de 4:00 AM, com mais de 20 bytes enviados/s no DC mais ocupado, o que pode indicar a carga de fusos horários ou atividades de infraestrutura de segundo plano diferentes, como backups. Como o pico às 8:00 está excedendo essa atividade, ele não é relevante.
1. Há cinco controladores de domínio no site.
1. A carga máxima é de cerca de 5,5 MB/s por DC, que representa 44% da conexão de 100 MB. Usando esses dados, é possível estimar que a largura de banda total necessária entre 8:00 AM e 8:15 AM é 28 MB/s.
   >[!NOTE]
   >Tenha cuidado com o fato de que os contadores de interface de rede enviados/recebidos estão em bytes e a largura de banda da rede é medida em bits. 100 MB &divide; 8 = 12,5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusões

1. Esse ambiente atual atende ao nível N + 1 de tolerância a falhas à utilização de destino de 60%. Colocar um sistema offline mudará a largura de banda por servidor de cerca de 5,5 MB/s (44%) a cerca de 7 MB/s (56%).
1. Com base na meta mencionada anteriormente de consolidação para um servidor, isso excede a utilização máxima de destino e, teoricamente, a possível utilização de uma conexão de 100 MB.
1. Com uma conexão de 1 GB, isso representará 22% da capacidade total.
1. Em condições normais de operação no cenário *N* + 1, a carga do cliente será igualmente distribuída em cerca de 14 MB/s por servidor ou 11% da capacidade total.
1. Para garantir que a capacidade seja adequada durante a indisponibilidade de um controlador de domínio, os destinos de operação normais por servidor seriam de aproximadamente 30% de utilização de rede ou 38 MB/s por servidor. Os destinos de failover seriam 60% de utilização de rede ou 72 MB/s por servidor.  
  
Em suma, a implantação final dos sistemas deve ter um adaptador de rede de 1 GB e estar conectada a uma infraestrutura de rede que dará suporte à carga mencionada. Uma observação adicional é que, dada a quantidade de tráfego de rede gerado, a carga de CPU das comunicações de rede pode ter um impacto significativo e limitar a escalabilidade máxima de AD DS. Esse mesmo processo pode ser usado para estimar a quantidade de comunicação de entrada para o controlador de domínio. Mas, considerando o predominância do tráfego de saída relativo ao tráfego de entrada, ele é um exercício acadêmico para a maioria dos ambientes. Garantir o suporte de hardware para RSS é importante em ambientes com mais de 5.000 usuários por servidor. Para cenários com alto tráfego de rede, o balanceamento de carga de interrupção pode ser um afunilamento. Isso pode ser detectado pelo tempo de\*interrupção\% do processador () sendo distribuído desuniformemente entre CPUs. NICs habilitadas para RSS podem mitigar essa limitação e aumentar a escalabilidade.

> [!NOTE]
> Uma abordagem semelhante pode ser usada para estimar a capacidade adicional necessária ao consolidar data centers ou para desativar um controlador de domínio em um local satélite. Basta coletar o tráfego de saída e de entrada para os clientes e essa será a quantidade de tráfego que agora estará presente nos links WAN.  
>  
> Em alguns casos, você pode ter mais tráfego do que o esperado porque o tráfego é mais lento, como quando a verificação de certificado falha ao atender ao tempo limite agressivo na WAN. Por esse motivo, o dimensionamento e a utilização da WAN devem ser um processo iterativo e contínuo.

### <a name="virtualization-considerations-for-network-bandwidth"></a>Considerações sobre virtualização para largura de banda de rede

É fácil fazer recomendações para um servidor físico: 1 GB para servidores que dão suporte a mais de 5000 usuários. Depois que vários convidados começarem a compartilhar uma infraestrutura de comutador virtual subjacente, será necessária atenção adicional para garantir que o host tenha largura de banda de rede adequada para dar suporte a todos os convidados no sistema e, portanto, exija o rigor adicional. Isso não é nada mais do que uma extensão para garantir a infraestrutura de rede no computador host. Isso é independente de a rede estar inclusiva do controlador de domínio em execução como uma máquina virtual convidada em um host com o tráfego de rede que está passando por um comutador virtual ou se estiver conectada diretamente a um comutador físico. O comutador virtual é apenas mais um componente em que o uplink precisa dar suporte à quantidade de dados transmitidos. Assim, o adaptador de rede física do host físico vinculado ao comutador deve ser capaz de dar suporte à carga de DC, além de todos os outros convidados que compartilham o comutador virtual conectado ao adaptador de rede física.

### <a name="calculation-summary-example"></a>Exemplo de Resumo de cálculo

|Sistema|Largura de banda de pico|
|-|-|
DC 1|6,5 MB/s|
DC 2|6,25 MB/s|
|DC 3|6,25 MB/s|
|DC 4|5,75 MB/s|
|DC 5|4,75 MB/s|
|Total|28,5 MB/s|

**Aconselhável 72 MB/s** (28,5 MB/s dividido por 40%)

|Contagem de sistema (es) de destino|Largura de banda total (acima)|
|-|-|
|2|28,5 MB/s|
|Comportamento normal resultante|28,5 &divide; 2 = 14,25 MB/s|

Como sempre, com o decorrer do tempo, a pressuposição pode ser feita para que a carga do cliente aumente e esse crescimento deve ser planejado da melhor maneira possível. O valor recomendado a ser planejado para permitir um crescimento estimado no tráfego de rede de 50%.

## <a name="storage"></a>Armazenamento

O planejamento de armazenamento constitui dois componentes:

- Capacidade ou tamanho do armazenamento
- Desempenho

Uma grande quantidade de tempo e A documentação são gastos na capacidade de planejamento, deixando o desempenho com frequência totalmente ignorado. Com os custos de hardware atuais, a maioria dos ambientes não é grande o suficiente para que seja realmente uma preocupação, e a recomendação para "colocar o máximo de RAM como o tamanho do banco de dados" geralmente cobre o restante, embora possa ser um exagero para locais de satélite em maior sistemas.

### <a name="sizing"></a>Dimensionamento

#### <a name="evaluating-for-storage"></a>Avaliando para armazenamento

Comparado a 13 anos atrás quando Active Directory foi introduzido, um tempo em que os drives de 4 GB e 9 GB eram os tamanhos de unidade mais comuns, o dimensionamento para Active Directory não é nem mesmo uma consideração para todos os ambientes, exceto para os maiores. Com os menores tamanhos de disco rígido disponíveis no intervalo de 180 GB, todo o sistema operacional, o SYSVOL e o NTDS. dit podem se ajustar facilmente a uma unidade. Assim, é recomendável substituir o investimento pesado nessa área.

A única recomendação para a consideração é garantir que 110% do tamanho de NTDS. dit esteja disponível para habilitar a desfragmentação. Além disso, as acomodações de crescimento durante a vida útil do hardware devem ser feitas.

A primeira e mais importante consideração é avaliar o tamanho em que o NTDS. dit e o SYSVOL serão. Essas medidas levarão ao dimensionamento de alocação de disco fixo e RAM. Devido ao menor custo (relativamente) desses componentes, a matemática não precisa ser rigorosa e precisa. O conteúdo sobre como avaliar isso para ambientes novos e existentes pode ser encontrado na série de artigos de [armazenamento de dados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) . Especificamente, consulte os seguintes artigos:

- **Para ambientes &ndash; existentes** , a seção intitulada "para ativar o registro em log de espaço em disco liberado por desfragmentação" no artigo [limites de armazenamento](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **Para novos ambientes &ndash;**  , o artigo intitulava [estimativas de crescimento para Active Directory usuários e unidades organizacionais](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > Os artigos são baseados em estimativas de tamanho de dados feitas no momento do lançamento do Active Directory no Windows 2000. Use tamanhos de objeto que reflitam o tamanho real dos objetos em seu ambiente.

Ao revisar ambientes existentes com vários domínios, pode haver variações de tamanhos de banco de dados. Quando isso for verdadeiro, use o menor catálogo global (GC) e tamanhos não GC.  

O tamanho do banco de dados pode variar entre as versões do sistema operacional. Os DCs que executam sistemas operacionais anteriores, como o Windows Server 2003, têm um tamanho de banco de dados menor do que um DC que executa um sistema operacional mais recente, como o Windows Server 2008 R2, especialmente quando recursos como Active Directory Lixeira ou Mobilidade de Credenciais estão habilitados.

> [!NOTE]  
  >
>- Para novos ambientes, observe que as estimativas nas estimativas de crescimento para Active Directory usuários e unidades organizacionais indicam que 100.000 usuários (no mesmo domínio) consomem cerca de 450 MB de espaço. Observe que os atributos populados podem ter um grande impacto na quantidade total. Os atributos serão preenchidos em vários objetos por produtos de terceiros e da Microsoft, incluindo o Microsoft Exchange Server e o Lync. Uma avaliação baseada no portfólio dos produtos no ambiente é preferida, mas o exercício de detalhamento da matemática e do teste de estimativas precisas para todos os ambientes mais amplos pode não valer a pena ser tempo e esforço significativo.
>- Verifique se 110% do tamanho de NTDS. dit está disponível como espaço livre para habilitar a desfragmentação offline e planeje o crescimento durante um ciclo de vida de três a cinco anos. Dado como o armazenamento barato está, estimando o armazenamento em 300%, o tamanho da DIT como alocação de armazenamento é seguro para acomodar o crescimento e a necessidade potencial de desfragmentação offline.

#### <a name="virtualization-considerations-for-storage"></a>Considerações sobre virtualização para armazenamento

Em um cenário em que vários arquivos de VHD (disco rígido virtual) estão sendo alocados em um único volume, use um disco de tamanho fixo de pelo menos 210% (100% da DIT + 110% de espaço livre) o tamanho da DIT para garantir que haja espaço suficiente reservado.  

#### <a name="calculation-summary-example"></a>Exemplo de Resumo de cálculo

|Dados coletados da fase de avaliação| |
|-|-|
|Tamanho de NTDS. dit|35 GB|
|Modificador para permitir desfragmentação offline|2.1|
|Armazenamento total necessário|73,5 GB|

> [!NOTE]
> Esse armazenamento necessário é além do armazenamento necessário para SYSVOL, sistema operacional, arquivo de paginação, arquivos temporários, dados armazenados em cache local (como arquivos do instalador) e aplicativos.

### <a name="storage-performance"></a>Desempenho do armazenamento

#### <a name="evaluating-performance-of-storage"></a>Avaliando o desempenho do armazenamento

Como o componente mais lento em qualquer computador, o armazenamento pode ter o maior impacto adverso na experiência do cliente. Para esses ambientes grandes o suficiente para os quais as recomendações de dimensionamento de RAM não são viáveis, as consequências de sobrepesquisar o armazenamento de planejamento para o desempenho podem ser devastadoras.  Além disso, as complexidades e variedades de tecnologia de armazenamento aumentam ainda mais o risco de falha, já que a relevância de práticas recomendadas de "colocar sistema operacional, logs e banco de dados" em discos físicos separados é limitada em cenários úteis.  Isso ocorre porque a prática recomendada de longa duração é baseada na suposição de que um "disco" é um eixo dedicado e que a e/s permitia ser isolada.  Essas suposições que tornam isso verdadeira não são mais relevantes com a introdução de:

- Novos tipos de armazenamento e cenários de armazenamento virtualizados e compartilhados
- Eixos compartilhados em uma rede de área de armazenamento (SAN)
- Arquivo VHD em um armazenamento de SAN ou conectado à rede
- Unidades de estado sólido
- Arquiteturas de armazenamento em camadas (por exemplo, armazenamento de camada de armazenamento SSD em cache maior com base no eixo)

Em suma, a meta final de todos os esforços de desempenho de armazenamento, independentemente da arquitetura de armazenamento subjacente e do design, é garantir que a quantidade necessária de operações de entrada/saída por segundo (IOPS) esteja disponível e que esses IOPS ocorram dentro de um período de tempo aceitável . Esta seção aborda como avaliar quais AD DS demandas do armazenamento subjacente para garantir que as soluções de armazenamento sejam projetadas corretamente.  Considerando a variabilidade das atuais tecnologias de armazenamento, é melhor trabalhar com os fornecedores de armazenamento para garantir o IOPS adequado.  Para esses cenários com armazenamento anexado localmente, faça referência ao apêndice C para obter noções básicas sobre como criar cenários tradicionais de armazenamento local.  Essas entidades geralmente são aplicáveis a camadas de armazenamento mais complexas e também ajudarão na caixa de diálogo com os fornecedores que dão suporte a soluções de armazenamento de back-end.

- Considerando a ampla variedade de opções de armazenamento disponíveis, é recomendável envolver o conhecimento de equipes de suporte a hardware ou fornecedores para garantir que a solução específica atenda às necessidades de AD DS. Os números a seguir são as informações que seriam fornecidas para os especialistas em armazenamento.

Para ambientes em que o banco de dados é muito grande para ser mantido na RAM, use os contadores de desempenho para determinar a quantidade de e/s que precisa ter suporte:

- LogicalDisk (\*) disco \Avg s/leitura (por exemplo, se NTDS. dit estiver armazenado na D:/ , o caminho completo seria o LogicalDisk (D:) \Avg de disco s/leitura)
- LogicalDisk (\*) \Avg de disco s/gravação
- LogicalDisk (\*) \Avg de disco s/transferência
- LogicalDisk (\*) \ leituras/s
- LogicalDisk (\*) \ gravações/s
- LogicalDisk (\*) \ transferências/s

Eles devem ser amostrados em intervalos de 15/30/60 minutos para avaliar as demandas do ambiente atual.

#### <a name="evaluating-the-results"></a>Avaliando os resultados

> [!NOTE]
> O foco está em leituras do banco de dados, pois esse é geralmente o componente mais exigente, a mesma lógica pode ser aplicada a gravações no arquivo de log, substituindo o LogicalDisk ( *\<log\>NTDS*) \Avg de disco s/gravação e LogicalDisk (*Log\>NTDS) \ gravações/s):\<*
>  
> - LogicalDisk ( *\<NTDS\>* ) \Avg de disco s/leitura indica se o armazenamento atual tem ou não o tamanho adequado.  Se os resultados forem aproximadamente iguais ao tempo de acesso do disco para o tipo de disco, o LogicalDisk ( *\<NTDS\>* ) \ leituras/s será uma medida válida.  Verifique as especificações do fabricante para o armazenamento no back-end, mas bons intervalos para disco de \Avg do LogicalDisk ( *\<NTDS\>* ) s/leitura serão aproximadamente:
>   - 7200 – 9 a 12,5 milissegundos (MS)
>   - 10.000 – 6 a 10 ms
>   - 15.000 – 4 a 6 ms  
>   - SSD – 1 a 3 MS  
>   - >[!NOTE]
>     >Há recomendações informando que o desempenho do armazenamento está degradado em 15ms para 20 ms (dependendo da origem).  A diferença entre os valores acima e as outras diretrizes é que os valores acima são o intervalo de operação normal.  As outras recomendações são diretrizes de solução de problemas para identificar quando a experiência do cliente degrada significativamente e se torna perceptível.  Apêndice de referência C para uma explicação mais profunda.
> - LogicalDisk ( *\<NTDS\>* ) \ leituras/s é a quantidade de e/s que está sendo executada.
>   - Se o LogicalDisk *\<(\>NTDS*) \Avg de disco s/leitura estiver dentro do intervalo ideal para o armazenamento de back-end, o LogicalDisk ( *\<NTDS\>* ) \ leituras/s pode ser usado diretamente para dimensionar o armazenamento.
>   - Se o LogicalDisk *\<(\>NTDS*) \Avg de disco s/leitura não estiver dentro do intervalo ideal para o armazenamento de back-end, e/s adicional será necessária de acordo com a fórmula a seguir:
>     > (LogicalDisk ( *\<NTDS\>* ) \Avg de disco s/leitura &divide; ) (tempo de acesso ao disco &times; físico) (LogicalDisk ( *\<NTDS\>* ) \Avg de disco s/leitura)

Considerações:

- Observe que, se o servidor estiver configurado com uma quantidade de RAM abaixo do ideal, esses valores serão imprecisos para fins de planejamento.  Eles serão erroneamente no lado alto e ainda poderão ser usados como um cenário de pior caso.
- A adição/otimização de RAM irá impulsionar uma redução na quantidade de e/s de leitura (disco de disco de memória ( *\<NTDS\>* ) \ leituras/s.  Isso significa que a solução de armazenamento pode não precisar ser tão robusta quanto a calculada inicialmente.  Infelizmente, qualquer coisa mais específica do que essa instrução geral depende ambientalmente da carga do cliente e orientação geral não pode ser fornecida.  A melhor opção é ajustar o dimensionamento do armazenamento depois de otimizar a RAM.

#### <a name="virtualization-considerations-for-performance"></a>Considerações sobre virtualização para desempenho

Semelhante a todas as discussões sobre virtualização anteriores, a chave aqui é garantir que a infraestrutura compartilhada subjacente possa dar suporte à carga de DC mais os outros recursos usando a mídia compartilhada subjacente e todos os caminhos para ela. Isso é verdadeiro se um controlador de domínio físico estiver compartilhando a mesma mídia subjacente em uma infraestrutura SAN, NAS ou iSCSI como outros servidores ou aplicativos, seja um convidado que usa acesso de passagem a uma infraestrutura SAN, NAS ou iSCSI que compartilha o a mídia subjacente, ou se o convidado estiver usando um arquivo VHD que reside em mídia compartilhada localmente ou em uma infraestrutura SAN, NAS ou iSCSI. O exercício de planejamento se refere a garantir que a mídia subjacente possa dar suporte à carga total de todos os consumidores.

Além disso, de uma perspectiva de convidado, como há caminhos de código adicionais que devem ser percorridos, há um impacto no desempenho de ter que passar por um host para acessar qualquer armazenamento. Não surpreendentemente, o teste de desempenho de armazenamento indica que a virtualização tem um impacto na taxa de transferência que está sujeita à utilização do processador do sistema host (Confira o apêndice A: Critérios de dimensionamento de CPU), que é obviamente influenciado pelos recursos do host exigidos pelo convidado. Isso contribui para as considerações de virtualização em relação às necessidades de processamento em um cenário virtualizado (consulte [considerações de virtualização para processamento](#virtualization-considerations-for-processing)).

Tornar isso mais complexo é que há uma variedade de opções de armazenamento diferentes que estão disponíveis e que todas têm impactos de desempenho diferentes. Como uma estimativa segura ao migrar do físico para o virtual, use um multiplicador de 1,10 para se ajustar para diferentes opções de armazenamento para convidados virtualizados no Hyper-V, como armazenamento de passagem, adaptador SCSI ou IDE. Os ajustes que precisam ser feitos durante a transferência entre os diferentes cenários de armazenamento são irrelevantes para se o armazenamento é local, SAN, NAS ou iSCSI.

#### <a name="calculation-summary-example"></a>Exemplo de Resumo de cálculo

Determinando a quantidade de e/s necessária para um sistema íntegro em condições normais de operação:

- LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \ transferências/s durante o período de pico de 15 minutos 
- Para determinar a quantidade de e/s necessária para o armazenamento onde a capacidade do armazenamento subjacente é excedida:
  >*IOPS necessário* = (LogicalDisk ( *\<unidade de banco\>de dados NTDS*) disco rígido &divide; \Avg s/ler *\<destino média\>de disco s/leitura*) &times; disco lógico ( *\<Unidade\>de banco de dados NTDS*) \ leitura/s

|Contador|Valor|
|-|-|
|LogicalDisk real ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/transferência|.02 segundos (20 milissegundos)|
|LogicalDisk de destino ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/transferência|.01 segundo|
|Multiplicador para alteração na e/s disponível|0, 2 &divide; 0, 1 = 2|  
  
|Nome do valor|Valor|
|-|-|
|LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \ transferências/s|400|
|Multiplicador para alteração na e/s disponível|2|
|IOPS total necessário durante o período de pico|800|

Para determinar a taxa na qual o cache é desejado para ser quente:

- Determine o tempo máximo aceitável para aquecimento do cache. É a quantidade de tempo que deve ser executada para carregar todo o banco de dados do disco, ou para cenários em que o banco de dados inteiro não pode ser carregado na RAM, esse seria o tempo máximo para preencher a RAM.
- Determine o tamanho do banco de dados, excluindo o espaço em branco.  Para obter mais informações, consulte [avaliando para armazenamento](#evaluating-for-storage).  
- Divida o tamanho do banco de dados em 8 KB; Esse será o total de IOs necessário para carregar o banco de dados.
- Divida o total de IOs pelo número de segundos no intervalo de tempo definido.

Observe que a taxa calculada, embora precisa, não será exata porque as páginas carregadas anteriormente serão removidas se o ESE não estiver configurado para ter um tamanho de cache fixo e AD DS, por padrão, usará o tamanho do cache variável.

|Pontos de dados a serem coletados|Valores
|-|-|
|Tempo máximo aceitável para quente|10 minutos (600 segundos)
|Tamanho do banco de dados|2 GB|  
  
|Etapa de cálculo|Fórmula|Resultado|
|-|-|-|
|Calcular o tamanho do banco de dados em páginas|(2 GB &times; 1024 &times; 1024) = *tamanho do banco de dados em KB*|2\.097.152 KB|
|Calcular o número de páginas no banco de dados|2\.097.152 KB &divide; 8 KB = *número de páginas*|262.144 páginas|
|Calcular o IOPS necessário para o cache totalmente quente|262.144 páginas &divide; 600 segundos = *IOPS necessário*|IOPS DE 437|

## <a name="processing"></a>Processando

### <a name="evaluating-active-directory-processor-usage"></a>Avaliando Active Directory uso do processador

Para a maioria dos ambientes, depois que o armazenamento, a RAM e a rede são ajustados corretamente conforme descrito na seção de planejamento, o gerenciamento da quantidade de capacidade de processamento será o componente que merece mais atenção. Há dois desafios na avaliação da capacidade da CPU necessária:

- Se os aplicativos no ambiente estão sendo bem comportados em uma infraestrutura de serviços compartilhados e são discutidos na seção intitulada "acompanhamento de pesquisas caras e ineficientes" no artigo criando a Microsoft Active Aplicativos habilitados para diretório ou migração para fora de chamadas SAM de nível inferior para chamadas LDAP.  
  
  Em ambientes maiores, o motivo disso é importante é que aplicativos mal codificados podem impulsionar o volatilidade na carga da CPU, "roubar" uma quantidade inordenada de tempo de CPU de outros aplicativos, artificialmente impulsionando as necessidades de capacidade e distribuindo de forma desigual a carga contra os DCs.  
- Como AD DS é um ambiente distribuído com uma grande variedade de clientes potenciais, estimar a despesa de um "cliente único" é ambientalmente subjetivo devido aos padrões de uso e ao tipo ou à quantidade de aplicativos que aproveitam AD DS. Em suma, assim como a seção Networking, para uma grande aplicabilidade, isso é melhor abordado da perspectiva da avaliação da capacidade total necessária no ambiente.

Para ambientes existentes, à medida que o dimensionamento de armazenamento foi discutido anteriormente, pressupõe-se que o armazenamento agora é dimensionado corretamente e, portanto, os dados relacionados à carga do processador são válidos. Para reiterar, é essencial garantir que o afunilamento no sistema não seja o desempenho do armazenamento. Quando existe um afunilamento e o processador está aguardando, há Estados ociosos que desaparecerão quando o afunilamento for removido.  À medida que os Estados de espera do processador são removidos, por definição, a utilização da CPU aumenta, pois não é mais necessário aguardar os dados. Portanto, colete contadores de desempenho "disco lógico ( *\<unidade\>de banco de dados NTDS*) \Avg disco s/leitura" e "processo\\(Lsass)% tempo do processador". Os dados em "processo (Lsass)\\% tempo do processador" serão artificialmente baixos se "disco lógico ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/leitura" exceder de 10 a 15 ms, que é um limite geral que o suporte da Microsoft usa para solucionar problemas de desempenho relacionados ao armazenamento. Como antes, é recomendável que os intervalos de exemplo sejam 15, 30 ou 60 minutos. Qualquer coisa menor geralmente será muito volátil para boas medidas; qualquer coisa maior suavizará as exibições diárias em excesso.

### <a name="introduction"></a>Introdução

Para planejar a capacidade de planejamento de controladores de domínio, a capacidade de processamento exige mais atenção e compreensão. Ao dimensionar sistemas para garantir o máximo de desempenho, sempre há um componente que é o afunilamento e, em um controlador de domínio de tamanho adequado, esse será o processador.

Semelhante à seção de rede em que a demanda do ambiente é revisada em uma base site a site, o mesmo deve ser feito para a capacidade de computação exigida. Ao contrário da seção Networking, em que as tecnologias de rede disponíveis ultrapassam a demanda normal, preste mais atenção ao dimensionamento da capacidade da CPU.  Como qualquer ambiente de tamanho par médio; qualquer coisa em alguns milhares de usuários simultâneos pode colocar uma carga significativa na CPU.

Infelizmente, devido à grande variabilidade dos aplicativos cliente que aproveitam o AD, uma estimativa geral dos usuários por CPU é infelizmente inaplicável a todos os ambientes. Especificamente, as demandas de computação estão sujeitas ao comportamento do usuário e ao perfil do aplicativo. Portanto, cada ambiente precisa ser dimensionado individualmente.

#### <a name="target-site-behavior-profile"></a>Perfil de comportamento do site de destino

Conforme mencionado anteriormente, ao planejar a capacidade de um site inteiro, o objetivo é direcionar um design com um design de capacidade de *N* + 1, de modo que a falha de um sistema durante o período de pico permitirá a continuação do serviço em um nível razoável de qualidade. Isso significa que, em um cenário "*N*", a carga entre todas as caixas deve ser inferior a 100% (melhor ainda, menos de 80%) durante os períodos de pico.

Além disso, se os aplicativos e os clientes no site estiverem usando práticas recomendadas para localizar controladores de domínio (ou seja, usando a [função DsGetDcName](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), os clientes deverão ser relativamente distribuídos igualmente com picos transitórios pequenos devido a qualquer número de fatores.

No próximo exemplo, são feitas as seguintes suposições:

- Cada um dos cinco DCs no site tem quatro CPUs.
- O uso total de CPU de destino durante o horário comercial é de 40% em condições normais de operação ("*n* + 1") e 60% caso contrário ("*n*"). Fora do horário comercial, o uso de CPU de destino é de 80% porque o software de backup e outras manutenções devem consumir todos os recursos disponíveis.

![Gráfico de uso da CPU](media/capacity-planning-considerations-cpu-chart.png)

Analisar os dados no gráfico (utilitário de processador de informações do\% processador (_ total)) para cada um dos DCS:

- Na maior parte, a carga é igualmente distribuída, o que seria esperado quando os clientes usam o localizador de DC e têm pesquisas bem escritas. 
- Há um número de picos de cinco minutos de 10%, com um tamanho maior de 20%. Em geral, a menos que eles façam com que o destino do plano de capacidade seja excedido, investigar isso não vale a pena.  
- O período de pico de todos os sistemas está entre cerca de 8:00 AM e 9:15 AM. Com a transição suave de cerca de 5:00, a partir de aproximadamente 5:00 PM, isso geralmente indica o ciclo de negócios. Os picos mais aleatórios de uso da CPU em um cenário de caixa por caixa entre 5:00 e 4:00 está fora das preocupações de planejamento de capacidade.

  >[!NOTE]
  >Em um sistema bem gerenciado, diz que os picos podem ser o software de backup em execução, verificações completas de antivírus do sistema, inventário de hardware ou software, implantação de software ou patches e assim por diante. Como estão fora do ciclo de negócios do usuário de pico, os destinos não são excedidos.

- Como cada sistema é de cerca de 40% e todos os sistemas têm os mesmos números de CPUs, caso uma falha ou seja colocado offline, os sistemas restantes seriam executados em um% estimado de 53% (o carregamento de 40% do System D é dividido e adicionado ao sistema A e o 40% de carga existente do sistema C). Por vários motivos, essa pressuposição linear não é perfeitamente precisa, mas fornece precisão suficiente para o medidor.  

  **Cenário alternativo –** Dois controladores de domínio em execução às 40%: Um controlador de domínio falha, a CPU estimada no restante seria uma estimativa de 80%. Isso excede os limites descritos acima para o plano de capacidade e também começa a limitar severamente a quantidade de espaço de cabeça de 10% a 20% visto no perfil de carga acima, o que significa que os picos direcionariam o DC para 90% para 100% durante o cenário "*N*" e de degradação finita de capacidade de resposta.

### <a name="calculating-cpu-demands"></a>Calculando as demandas de CPU

O contador de\\objetos de desempenho "processo% tempo do processador" soma a quantidade total de tempo que todos os threads de um aplicativo gastam na CPU e dividem a quantidade total de tempo do sistema que passou. O efeito disso é que um aplicativo multi-threaded em um sistema de várias CPUs pode exceder 100% de tempo de CPU e seria interpretado de forma muito diferente do que o\\"utilitário de processador% de informações do processador". Na prática, o "processo (Lsass\\)% tempo do processador" pode ser exibido como a contagem de CPUs em execução às 100% que são necessárias para dar suporte às demandas do processo. Um valor de 200% significa que 2 CPUs, cada uma às 100%, são necessárias para dar suporte à carga de AD DS completa. Embora uma CPU em execução com a capacidade de 100% seja a mais econômica da perspectiva do dinheiro gasto em CPUs e consumo de energia e energia, por vários motivos detalhados no apêndice A, uma melhor capacidade de resposta em um sistema multi-threaded ocorre quando o sistema é Não está sendo executado às 100%.

Para acomodar picos transitórios na carga do cliente, é recomendável direcionar uma CPU de período de pico entre 40% e 60% da capacidade do sistema. Trabalhando com o exemplo acima, isso significaria que as CPUs 3,33 (60% Target) e 5 (40% Target) seriam necessárias para a carga de AD DS (processo Lsass). A capacidade adicional deve ser adicionada de acordo com as demandas do sistema operacional base e de outros agentes necessários (como antivírus, backup, monitoramento e assim por diante). Embora o impacto dos agentes precise ser avaliado em uma base por ambiente, uma estimativa entre 5% e 10% de uma única CPU pode ser feita. No exemplo atual, isso sugere que as CPUs 3,43 (60% Target) e 5,1 (40% Target) são necessárias durante períodos de pico.

A maneira mais fácil de fazer isso é usar a exibição "área empilhada" no PerfMon (monitor de desempenho e confiabilidade) do Windows, garantindo que todos os contadores sejam dimensionados da mesma forma.

Suposições:

- A meta é reduzir a quantidade de máximo de servidores possível. O ideal é que um servidor carregue a carga e um servidor adicional fosse adicionado para redundância (*N* + 1 cenário).

![Gráfico de tempo do processador para o processo do Lsass (em todos os processadores)](media/capacity-planning-considerations-proc-time-chart.png)

Conhecimento obtido dos dados no gráfico (Lsass)\\% tempo do processador):

- O dia útil começa a subir em cerca de 7:00 e diminui às 5:00 PM.
- O período de pico mais ocupado é de 9:30 às 11:00. 
  > [!NOTE]
  > Todos os dados de desempenho são históricos. O ponto de dados de pico em 9:15 indica a carga de 9:00 a 9:15.
- Há picos antes de 7:00, o que pode indicar a carga de diferentes fusos horários ou atividade de infraestrutura de segundo plano, como backups. Como o pico às 9:30 está excedendo essa atividade, ele não é relevante.
- Há três controladores de domínio no site.

Na carga máxima, o LSASS consome cerca de 485% de uma CPU ou 4,85 CPUs em execução às 100%. De acordo com a matemática anterior, isso significa que o site precisa de cerca de 12,25 CPUs para AD DS. Adicione as sugestões acima de 5% a 10% para processos em segundo plano e isso significa que a substituição do servidor hoje precisará de aproximadamente 12,30 a 12,35 CPUs para dar suporte à mesma carga. Uma estimativa ambiental para crescimento agora precisa ser fatorada em.

### <a name="when-to-tune-ldap-weights"></a>Quando ajustar pesos LDAP

Há vários cenários em que o ajuste de [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) deve ser considerado. No contexto do planejamento de capacidade, isso seria feito quando o aplicativo ou o usuário carrega não está balanceado uniformemente ou os sistemas subjacentes não são balanceados uniformemente em termos de capacidade. Os motivos para fazer isso além do planejamento de capacidade estão fora do escopo deste artigo.

Há dois motivos comuns para ajustar os pesos do LDAP:

- O emulador de PDC é um exemplo que afeta todos os ambientes para os quais o comportamento de carga de usuário ou aplicativo não é distribuído uniformemente. Como determinadas ferramentas e ações visam o emulador de PDC, como as ferramentas de gerenciamento de Política de Grupo, segunda tentativas no caso de falhas de autenticação, estabelecimento de confiança e assim por diante, os recursos de CPU no emulador de PDC podem ser mais exigentes do que em outro lugar o site.
  - Só é útil ajustar isso se houver uma diferença perceptível na utilização da CPU para reduzir a carga no emulador de PDC e aumentar a carga em outros controladores de domínio permitirá uma distribuição mais uniforme da carga.
  - Nesse caso, defina LDAPSrvWeight entre 50 e 75 para o emulador de PDC.
- Servidores com contagens diferentes de CPUs (e velocidades) em um site.  Por exemplo, digamos que existam servidores 2 8-core e o servidor 1 4-core.  O último servidor tem metade dos processadores dos outros dois servidores.  Isso significa que uma carga de cliente bem distribuída aumentará a carga de CPU média na caixa de quatro núcleos para aproximadamente duas vezes a das caixas de oito núcleos.
  - Por exemplo, as caixas 2 8-Core seriam executadas às 40% e a caixa de quatro núcleos estaria em execução às 80%.
  - Além disso, considere o impacto da perda da caixa 1 8-Core nesse cenário, especificamente o fato de que a caixa de quatro núcleos agora estaria sobrecarregada.

#### <a name="example-1---pdc"></a>Exemplo 1-PDC

| |Utilização com padrões|Novo LdapSrvWeight|Nova utilização estimada|
|-|-|-|-|
|DC 1 (emulador de PDC)|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

O problema aqui é que, se a função de emulador de PDC for transferida ou executada, particularmente para outro controlador de domínio no site, haverá um aumento considerável no novo emulador de PDC.

Usando o exemplo da seção [perfil de comportamento do site de destino](#target-site-behavior-profile), foi feita uma pressuposição de que todos os três controladores de domínio no site tinham quatro CPUs. O que deve acontecer, em condições normais, se um dos controladores de domínio tivesse oito CPUs? Haveria dois controladores de domínio à 40% de utilização e um a 20% de utilização. Embora isso não seja mau, há uma oportunidade de balancear a carga um pouco melhor. Aproveite os pesos LDAP para fazer isso.  Um cenário de exemplo seria:

#### <a name="example-2---differing-cpu-counts"></a>Exemplo 2 – diferentes contagens de CPU

| |Utilitário processador\\de informações %&nbsp;do processador (_ total)<br />Utilização com padrões|Novo LdapSrvWeight|Nova utilização estimada|
|-|-|-|-|
|4-CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

No entanto, tenha cuidado com esses cenários. Como pode ser visto acima, a matemática parece muito boa e em papel. Mas, ao longo deste artigo, o planejamento de um cenário "*N* + 1" é de extrema importância. O impacto de um DC ficar offline deve ser calculado para cada cenário. No cenário imediatamente anterior, em que a distribuição de carga é par, para garantir uma carga de 60% durante um cenário "*N*", com a carga balanceada uniformemente em todos os servidores, a distribuição será bem como as proporções se mantêm consistentes. Observando o cenário de ajuste do emulador de PDC e, em geral, qualquer cenário em que a carga de usuário ou aplicativo está desbalanceada, o efeito é muito diferente:

| |Utilização ajustada|Novo LdapSrvWeight|Nova utilização estimada|
|-|-|-|-|
|DC 1 (emulador de PDC)|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### <a name="virtualization-considerations-for-processing"></a>Considerações sobre virtualização para processamento

Há duas camadas de planejamento de capacidade que precisam ser feitas em um ambiente virtualizado. No nível do host, semelhante à identificação do ciclo de negócios descrito anteriormente para o processamento do controlador de domínio, os limites durante o período de pico precisam ser identificados. Como as entidades subjacentes são as mesmas para um computador host que está agendando threads de convidado na CPU quanto para obter AD DS threads na CPU em um computador físico, é recomendável o mesmo objetivo de 40% a 60% no host subjacente. Na próxima camada, a camada de convidado, já que as entidades de programação de thread não foram alteradas, a meta dentro do convidado permanece no intervalo de 40% a 60%.

Em um cenário mapeado direto, um convidado por host, todo o planejamento de capacidade feito para esse ponto precisa ser adicionado aos requisitos (RAM, disco, rede) do sistema operacional do host subjacente. Em um cenário de host compartilhado, o teste indica que há 10% de impacto na eficiência dos processadores subjacentes. Isso significa que se um site precisar de 10 CPUs em um destino de 40%, a quantidade recomendada de CPUs virtuais a serem alocadas em todos os "*N*" convidados seria 11. Em um site com uma distribuição mista de servidores físicos e servidores virtuais, o modificador se aplica somente às VMs. Por exemplo, se um site tiver um cenário "*N* + 1", um servidor físico ou mapeado direto com 10 CPUs seria equivalente a um convidado com 11 CPUs em um host, com 11 CPUs reservadas para o controlador de domínio.

Durante a análise e o cálculo das quantidades de CPU necessárias para dar suporte à carga de AD DS, os números de CPUs que mapeiam para o que pode ser adquirido em termos de hardware físico não são necessariamente mapeados corretamente. A virtualização elimina a necessidade de arredondar. A virtualização diminui o esforço necessário para adicionar capacidade de computação a um site, devido à facilidade com que uma CPU pode ser adicionada a uma VM. Ele não elimina a necessidade de avaliar com precisão a capacidade de computação necessária para que o hardware subjacente esteja disponível quando CPUs adicionais precisarem ser adicionadas aos convidados.  Como sempre, lembre-se de planejar e monitorar o crescimento na demanda.

### <a name="calculation-summary-example"></a>Exemplo de Resumo de cálculo

|Sistema|CPU de pico|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|DC 3|218%|
|Total de CPU em uso|485%|  
  
|Contagem de sistema (es) de destino|Largura de banda total (acima)|
|-|-|
|CPUs necessárias às 40% de destino|4,85 &divide; . 4 = 12,25|

Repetindo devido à importância deste ponto, *Lembre-se de planejar o crescimento*. Supondo um crescimento de 50% nos próximos três anos, esse ambiente precisará de 18,375 &times; CPUs (12,25 1,5) na marca de três anos. Um plano alternativo seria analisar após o primeiro ano e adicionar capacidade adicional, conforme necessário.

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Carga de autenticação de cliente de confiança cruzada para NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>Avaliando a carga de autenticação de cliente de confiança cruzada

Muitos ambientes podem ter um ou mais domínios conectados por uma relação de confiança. Uma solicitação de autenticação para uma identidade em outro domínio que não usa a autenticação Kerberos precisa atravessar uma relação de confiança usando o canal seguro do controlador de domínio para outro controlador de domínio no domínio de destino ou no próximo domínio no caminho para o domínio de destino. O número de chamadas simultâneas usando o canal seguro que um controlador de domínio pode fazer a um controlador de domínio em um domínio confiável é controlado por uma configuração conhecida como **MaxConcurrentAPI**. Para controladores de domínio, garantir que o canal seguro possa lidar com a quantidade de carga é realizado por uma das duas abordagens: ajuste **MaxConcurrentAPI** ou, em uma floresta, criando relações de confiança de atalho. Para medir o volume de tráfego em uma relação de confiança individual, consulte [como fazer o ajuste de desempenho para a autenticação NTLM usando a configuração MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

Durante a coleta de dados, isso, assim como acontece com todos os outros cenários, deve ser coletado durante os períodos de pico do dia para que os dados sejam úteis.

> [!NOTE]
> Cenários intraflorestal e entre florestas podem fazer com que a autenticação percorra várias relações de confiança e cada estágio precise ser ajustado.

#### <a name="planning"></a>Planejamento

Há vários aplicativos que usam a autenticação NTLM por padrão, ou use-os em um determinado cenário de configuração. Os servidores de aplicativos aumentam a capacidade e o serviço são um número cada vez maior de clientes ativos. Também há uma tendência de que os clientes mantenham sessões abertas por um período limitado e, em vez disso, se reconectem regularmente (como sincronização de pull por email). Outro exemplo comum de alta carga de NTLM são os servidores proxy da Web que exigem autenticação para acesso à Internet.

Esses aplicativos podem causar uma carga significativa para a autenticação NTLM, o que pode colocar uma sobrecarga significativa nos DCs, especialmente quando os usuários e os recursos estiverem em domínios diferentes.

Há várias abordagens para gerenciar a carga de confiança cruzada, que, na prática, é usada em conjunto em vez de em um cenário ou/ou exclusivo. As opções possíveis são:

- Reduza a autenticação de cliente de confiança cruzada localizando os serviços que um usuário consome no mesmo domínio no qual o usuário está residente.
- Aumente o número de canais seguros disponíveis. Isso é relevante para o tráfego intraflorestal e entre florestas e é conhecido como relações de confiança de atalho.
- Ajuste as configurações padrão para **MaxConcurrentAPI**.

Para ajustar o **MaxConcurrentAPI** em um servidor existente, a equação é:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires*  +  *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

Para obter mais informações, [consulte o artigo 2688798 do KB: Como fazer o ajuste de desempenho para a autenticação NTLM usando a configuração](http://support.microsoft.com/kb/2688798)MaxConcurrentApi.

## <a name="virtualization-considerations"></a>Considerações sobre virtualização

Nenhum, essa é uma configuração de ajuste do sistema operacional.

### <a name="calculation-summary-example"></a>Exemplo de Resumo de cálculo

|Tipo de dados|Valor|
|-|-|
|Aquisições de semáforo (mínimo)|6\.161|
|Aquisições de semáforo (máximo)|6\.762|
|Tempos limite de semáforo|0|
|Tempo médio de espera de semáforo|0, 12|
|Duração da coleta (segundos)|1:11 minutos (71 segundos)|
|Fórmula (de KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0, 12/|
|Valor mínimo para **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0, 12 &divide; 71 =. 101|

Para esse sistema durante esse período de tempo, os valores padrão são aceitáveis.

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>Monitorando a conformidade com as metas de planejamento de capacidade

Ao longo deste artigo, foi discutido que o planejamento e a escala vão para metas de utilização. Aqui está um gráfico de resumo dos limites recomendados que devem ser monitorados para garantir que os sistemas estejam operando dentro dos limites de capacidade adequados. Tenha em mente que esses não são limites de desempenho, mas limites de planejamento de capacidade. Um servidor operando em excesso desses limites funcionará, mas é hora de começar a validar que todos os aplicativos estão bem comparados. Se disse que os aplicativos estão bem comparados, é hora de começar a avaliar atualizações de hardware ou outras alterações de configuração.

|Category|Contador de desempenho|Intervalo/amostragem|Destino|Aviso|
|-|-|-|-|-|
|Processador|Informações do processador (_\\total)% Processor Utility|mínimo de 60|40%|60%|
|RAM (Windows Server 2008 R2 ou anterior)|\ MB|< 100 MB|N/D|< 100 MB|
|RAM (Windows Server 2012)|Tempo de vida de cache de espera Memory\Long-Term médio (s)|30 min|Deve ser testado|Deve ser testado|
|Rede|Interface de rede\*() \Bytes enviados/s<br /><br />Interface de rede\*() \Bytes recebidos/s|30 min|40%|60%|
|Armazenamento|LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/leitura<br /><br />LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/gravação|mínimo de 60|10 ms|15 ms|
|Serviços do AD|Tempo de\*espera do semáforo \ Netlogon ()|mínimo de 60|0|1 segundo|

## <a name="appendix-a-cpu-sizing-criteria"></a>Apêndice A: Critérios de dimensionamento da CPU

### <a name="definitions"></a>Definições

**Processador (microprocessador) –** um componente que lê e executa instruções do programa  

**CPU –** Unidade de processamento central  

**Processador de vários núcleos –** várias CPUs no mesmo circuito integrado  

Várias **CPU –** várias CPUs, não no mesmo circuito integrado  

**Processador lógico –** um mecanismo de computação lógico da perspectiva do sistema operacional  

Isso inclui o Hyper-threaded, um núcleo no processador de vários núcleos ou um processador de núcleo único.  

Como os sistemas de servidor atuais têm vários processadores, vários processadores de vários núcleos e hiperthreading, essas informações são generalizadas para abranger os dois cenários. Como tal, o termo processador lógico será usado, pois representa a perspectiva do sistema operacional e do aplicativo dos mecanismos de computação disponíveis.

### <a name="thread-level-parallelism"></a>Paralelismo de nível de thread

Cada thread é uma tarefa independente, pois cada thread tem sua própria pilha e instruções. Como AD DS é multi-threaded e o número de threads disponíveis pode ser ajustado com o uso de [como exibir e definir a política LDAP em Active Directory usando ntdsutil. exe](http://support.microsoft.com/kb/315071), ele é dimensionado bem em vários processadores lógicos.

### <a name="data-level-parallelism"></a>Paralelismo de nível de dados

Isso envolve o compartilhamento de dados entre vários threads dentro de um processo (no caso do AD DS processo sozinho) e entre vários threads em vários processos (em geral). Com a preocupação de simplificar o caso, isso significa que qualquer alteração nos dados é refletida para todos os threads em execução em todos os vários níveis de cache (L1, L2, L3) em todos os núcleos executados em threads, bem como atualização da memória compartilhada. O desempenho pode diminuir durante operações de gravação, enquanto todos os vários locais de memória são colocados consistentes antes que o processamento de instruções possa continuar.

### <a name="cpu-speed-vs-multiple-core-considerations"></a>Considerações sobre velocidade da CPU versus vários núcleos

A regra geral é que os processadores lógicos mais rápidos reduzem a duração que leva para processar uma série de instruções, enquanto mais processadores lógicos significam que mais tarefas podem ser executadas ao mesmo tempo. Essas regras de Thumb dividem-se conforme os cenários se tornam inerentemente mais complexos com considerações sobre a busca de dados de memória compartilhada, aguardando o paralelismo no nível de dados e a sobrecarga de gerenciar vários threads. Essa é também a razão pela qual a escalabilidade em sistemas de vários núcleos não é linear.

Considere as seguintes analogias nessas considerações: pense em uma rodovia, com cada thread sendo um carro individual, cada pista sendo um núcleo e o limite de velocidade sendo a velocidade do clock.

1. Se houver apenas um carro na rodovia, não importa se há duas pistas ou 12 pistas. Esse carro só será tão rápido quanto o limite de velocidade permitir.
1. Suponha que os dados que o thread precisa não estejam imediatamente disponíveis. A analogia seria que um segmento de estrada fosse desligado. Se houver apenas um carro na rodovia, não importa qual é o limite de velocidade até que a pista seja reaberta (os dados são buscados da memória).
1. À medida que aumenta o número de carros, a sobrecarga para gerenciar o número de carros aumenta. Compare a experiência de condução e a quantidade de atenção necessária quando a estrada está praticamente vazia (como a tarde da noite) versus quando o tráfego é pesado (como, por exemplo, uma hora de meados da tarde, mas não de urgência). Além disso, considere a quantidade de atenção necessária ao dirigir uma rodovia de duas pistas, em que há apenas uma outra pista para se preocupar com o que os drivers estão fazendo, em vez de uma rodovia de seis pistas, em que é preciso se preocupar com o que muitos outros drivers estão fazendo.
   > [!NOTE]
   > A analogia sobre o cenário de hora de corrida é estendida na próxima seção: Tempo de resposta/como o sistema busyness impacta o desempenho.

Como resultado, informações específicas sobre processadores mais ou mais rápidos se tornam altamente subjetivas para o comportamento do aplicativo, que no caso de AD DS é muito específico do ambiente e até mesmo varia do servidor para o servidor em um ambiente. É por isso que as referências anteriores no artigo não investem muito em serem excessivamente precisas, e uma margem de segurança é incluída nos cálculos. Ao fazer decisões de compra controladas pelo orçamento, é recomendável que a otimização do uso dos processadores em 40% (ou o número desejado para o ambiente) ocorra primeiro, antes de considerar a compra de processadores mais rápidos. A maior sincronização em mais processadores reduz o verdadeiro benefício de mais processadores da progressão linear (2&times; o número de processadores fornece menos de 2&times; recursos de computação disponíveis).

> [!NOTE]
> A lei de Amdahl e a lei de Gustafson são os conceitos relevantes aqui.

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>Tempo de resposta/como o sistema busyness impacta o desempenho

A teoria do enfileiramento é o estudo matemático de linhas de espera (filas). Na teoria do enfileiramento, a lei de utilização é representada pela equação:

*U* k = *B* &divide; *T*

Onde *U* k é a porcentagem de utilização, *B* é a quantidade de tempo ocupada e *T* é o tempo total que o sistema foi observado. Traduzido no contexto do Windows, isso significa o número de threads de intervalo de 100 a nanossegundos (ns) que estão em um estado de execução dividido por quantos intervalos de 100-NS estavam disponíveis no intervalo de tempo fornecido. Essa é exatamente a fórmula para calcular o utilitário% Processor ( [objeto de processador](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) de referência e [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

A teoria do enfileiramento também fornece a fórmula: *N* = *u* k &divide;(1 &ndash; *u* k) para estimar o número de itens em espera com base na utilização (*N* é o comprimento da fila). O gráfico em todos os intervalos de utilização fornece as estimativas a seguir de quanto tempo a fila para chegar ao processador está em qualquer carga de CPU específica.

![Comprimento da fila](media/capacity-planning-considerations-queue-length.png)

Observe que após 50% de carga de CPU, em média há sempre uma espera de um outro item na fila, com um aumento notavelmente rápido após cerca de 70% de utilização da CPU.

Retornando à analogia de condução usada anteriormente nesta seção:

- Os horários ocupados de "meados da tarde", hipotéticomente, se enquadram em um intervalo de 40% a 70%. Há tráfego suficiente, de modo que a capacidade de escolher qualquer pista não seja muito restrita, e a possibilidade de outro driver estar no caminho, embora alto, não exija o nível de esforço para "encontrar" uma lacuna segura entre outros carros em trânsito.
- Um deles perceberá que, à medida que o tráfego se aproxima de hora, o sistema de estrada se aproxima da capacidade de 100%. A alteração de pistas pode se tornar muito desafiador, pois os carros estão tão próximos que o maior cuidado deve ser exercido para fazer isso.

É por isso que as médias de longo prazo da capacidade é rebaixada de forma conservadora em 40% permite a sala de cabeça para picos anormais na carga, quer dizer picos transitórios (como consultas mal suportadas que são executadas por alguns minutos) ou intermitências anormais em carga geral (a manhã de o primeiro dia após um longo período de semana.

A instrução acima considera que o cálculo do tempo do processador é o mesmo que a lei de utilização é um pouco de simplificação para a facilidade do leitor geral. Para aqueles mais matematicamente rigorosos:  
- Convertendo o [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = o número de 100-o thread "ocioso" de intervalos NS gasta no processador lógico. A alteração na variável "*X*" no cálculo de [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *T* = o número total de intervalos de 100-NS em um determinado intervalo de tempo. A alteração na variável "*Y*" no cálculo de [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) .
  - *U* k = a porcentagem de utilização do processador lógico pelo "thread ocioso" ou% tempo ocioso.  
- Como trabalhar com a matemática:
  - *U* k = 1 –% de tempo do processador
  - % Tempo do processador = 1 – *U* k
  - % Tempo do processador = 1 – *B* / *T*
  - % Tempo do processador = 1 – *X1* – *x0* / *Y1* – *y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>Aplicando os conceitos ao planejamento de capacidade

A matemática anterior pode fazer com que as determinações sobre o número de processadores lógicos necessários em um sistema pareçam extremamente complexas. É por isso que a abordagem de dimensionamento dos sistemas concentra-se na determinação da utilização máxima de destino com base na carga atual e no cálculo do número de processadores lógicos necessários para chegar lá. Além disso, embora as velocidades de processador lógico tenham um impacto significativo no desempenho, as eficiências de cache, os requisitos de coerência de memória, o agendamento e a sincronização de threads e a carga de cliente com balanceamento de sobrecarga, todos terão impactos significativos em desempenho que variará em uma base servidor a servidor. Com o custo relativamente barato da capacidade de computação, a tentativa de analisar e determinar o número perfeito de CPUs necessárias se torna um exercício acadêmico do que fornece valor comercial.

40 por cento não é um requisito difícil e rápido, é um início razoável. Vários consumidores de Active Directory exigem vários níveis de capacidade de resposta. Pode haver situações em que os ambientes possam ser executados às 80% ou 90% de utilização como uma média sustentada, pois os tempos de espera maiores para o acesso ao processador não afetarão notavelmente o desempenho do cliente. É importante repetir a iteração de que há muitas áreas no sistema que são muito mais lentas do que o processador lógico no sistema, incluindo acesso à RAM, acesso ao disco e transmissão da resposta pela rede. Todos esses itens precisam ser ajustados em conjunto. Exemplos:

- A adição de mais processadores a um sistema executando 90%, que é o limite de disco, provavelmente não vai melhorar significativamente o desempenho. Uma análise mais profunda do sistema provavelmente identificará que há muitos threads que ainda não estão recebendo o processador porque estão aguardando a conclusão da e/s.
- Resolver os problemas de limite de disco pode significar que os threads que estavam gastando anteriormente muito tempo em um estado de espera não estarão mais em um estado de espera para e/s e haverá mais competição pelo tempo de CPU, o que significa que a utilização de 90% no anterior o exemplo vai para 100% (porque ele não pode ficar mais alto). Ambos os componentes precisam ser ajustados em conjunto.
  > [!NOTE]
  > As informações do processador (\\*)% Processor Utility pode exceder 100% com sistemas que têm um modo "Turbo".  É aí que a CPU excede a velocidade de processador nominal por períodos curtos.  Consulte a documentação dos fabricantes de CPU e a descrição do contador para obter mais informações.  

Discutir as considerações de utilização do sistema inteiro também traz os controladores de domínio de conversa como convidados virtualizados. O [tempo de resposta/como o sistema busyness impacta o desempenho](#response-timehow-the-system-busyness-impacts-performance) se aplica ao host e ao convidado em um cenário virtualizado. É por isso que em um host com apenas um convidado, um controlador de domínio (e, em geral, qualquer sistema) tem quase o mesmo desempenho que ele faz em hardware físico. A adição de convidados adicionais aos hosts aumenta a utilização do host subjacente, aumentando assim os tempos de espera para obter acesso aos processadores, conforme explicado anteriormente. Em suma, a utilização do processador lógico precisa ser gerenciada no host e nos níveis convidados.

Estendendo as analogias anteriores, deixando a rodovia como o hardware físico, a VM convidada será analogia com um barramento (um barramento expresso que vai direto para o destino que a cláusula adicional deseja). Imagine os quatro cenários a seguir:

- Ele está fora do expediente, uma cláusula é um barramento quase vazio e o barramento entra em uma estrada que também está quase vazia. Como não há tráfego a ser disparado, a cláusula adicional tem um bom recurso fácil e chega lá tão rápido quanto se a cláusula fosse baseada em seu lugar. Os tempos de viagem da cláusula adicional ainda são restritos pelo limite de velocidade.
- Está fora do horário de que o barramento está quase vazio, mas a maioria das pistas na estrada está fechada, portanto a estrada ainda está congestionada. A cláusula adicional está em um barramento quase vazio em uma estrada congestionada. Embora a cláusula adicional não tenha muita competição no barramento para onde se sentar, o tempo total de viagem ainda é ditado pelo restante do tráfego fora.
- É uma hora de urgência para que a rodovia e o barramento fiquem congestionados. Não só a viagem leva mais tempo, mas a introdução e a desativação do barramento é um pesadelo, pois as pessoas estão tentando tiracolo e a estrada não é muito melhor. Adicionar mais barramentos (processadores lógicos ao convidado) não significa que eles podem se ajustar à estrada com mais facilidade ou que a viagem será reduzida.
- O cenário final, embora possa estar aumentando a analogia, é onde o barramento está cheio, mas a estrada não está congestionada. Embora a cláusula adicional ainda tenha problemas de ativar e desativar o barramento, a viagem será eficiente depois que o barramento estiver viajando. Esse é o único cenário em que adicionar mais barramentos (processadores lógicos ao convidado) melhorará o desempenho do convidado.

A partir daí, é relativamente fácil extrapolar que há vários cenários entre o estado 0% utilizado e 100% utilizado da estrada, e o estado 0%-e 100% utilizado do barramento que têm graus variados de impacto em um mesmo grau.

A aplicação das entidades de segurança acima de 40% da CPU como destino razoável para o host, bem como o convidado, é um início razoável para o mesmo raciocínio acima, a quantidade de enfileiramento.

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>Apêndice B: Considerações sobre diferentes velocidades de processador e o efeito do gerenciamento de energia do processador em velocidades de processador

Em todas as seções na seleção do processador, é feita a suposição que o processador está sendo executado às 100% da velocidade do relógio o tempo todo que os dados estão sendo coletados e que os sistemas de substituição terão os mesmos processadores de velocidade. Apesar de ambas as suposições na prática serem falsas, particularmente com o Windows Server 2008 R2 e posterior, em que o plano de energia padrão é **equilibrado**, a metodologia ainda significa que ela é a abordagem conservadora. Embora a possível taxa de erros possa aumentar, ela aumenta apenas a margem de segurança à medida que as velocidades do processador aumentam.

- Por exemplo, em um cenário em que as CPUs de 11,25 são solicitadas, se os processadores estivessem sendo executados em meia velocidade quando os dados foram coletados, &divide; a estimativa mais precisa será 5,125 2.
- É impossível garantir que dobrar as velocidades do relógio dobraria a quantidade de processamento que ocorre por um determinado período de tempo. Isso ocorre devido ao fato da quantidade de tempo que os processadores gastam na RAM ou outros componentes do sistema podem permanecer os mesmos. O efeito líquido é que os processadores mais rápidos podem gastar uma porcentagem maior do tempo ocioso enquanto aguardam a busca de dados. Novamente, é recomendável manter o denominador comum mais baixo, ser conservador e evitar tentar calcular um nível de precisão potencialmente falso, supondo uma comparação linear entre as velocidades do processador.

Como alternativa, se as velocidades do processador no hardware de substituição forem menores do que o hardware atual, seria seguro aumentar a estimativa dos processadores necessários por um valor proporcional. Por exemplo, é calculado que 10 processadores são necessários para manter a carga em um site, e os processadores atuais estão sendo executados a 3,3 GHz e os processadores de substituição serão executados a 2,6 GHz, isso é uma redução de 21% em velocidade. Nesse caso, 12 processadores seriam o valor recomendado.

Dito isso, essa variabilidade não alteraria os destinos de utilização do processador de gerenciamento de capacidade. À medida que as velocidades do relógio do processador forem ajustadas dinamicamente com base na carga solicitada, a execução do sistema sob cargas mais altas gerará um cenário em que a CPU passa mais tempo em um estado de velocidade de clock maior, fazendo com que o objetivo final esteja à utilização de 40% em 100% Estado de velocidade do relógio no pico. Qualquer coisa menor que gerará economia de energia, pois as velocidades de CPU serão limitadas durante os cenários de pico.

> [!NOTE]
> Uma opção seria desativar o gerenciamento de energia nos processadores (definindo o plano de energia para **alto desempenho**) enquanto os dados são coletados. Isso daria uma representação mais precisa do consumo de CPU no servidor de destino.

Para ajustar as estimativas para processadores diferentes, ele costumava ser seguro, excluindo outros afunilamentos do sistema descritos acima, para assumir que dobrar velocidades de processador dobraram a quantidade de processamento que poderia ser executada.  Hoje, a arquitetura interna dos processadores é diferente o suficiente entre os processadores, que é uma maneira mais segura de medir os efeitos do uso de processadores diferentes dos quais os dados foram tirados é aproveitar o benchmark SPECint_rate2006 da avaliação de desempenho padrão Corporation.

1. Localize as pontuações de SPECint_rate2006 para o processador que estão em uso e o plano a ser usado.
    1. No site da empresa de avaliação de desempenho padrão, selecione **resultados**, realce **CPU2006**e selecione **Pesquisar todos os resultados do SPECint_rate2006**.
    1. Em **solicitação simples**, insira os critérios de pesquisa para o processador de destino, por exemplo, as correspondências de **processador E5-2630 (baselinetarget)** e as correspondências de **processador E5-2650 (linha de base)** .
    1. Localize a configuração do servidor e do processador a ser usada (ou algo próximo, se uma correspondência exata não estiver disponível) e observe o valor nas colunas **resultado** e **n º núcleos** .
1. Para determinar o modificador, use a seguinte equação:
   >((*Plataforma de destino por valor de Pontuação*de &times; núcleo) (*MHz por núcleo de plataforma de linha de base* &divide; )) ((*linha de base por valor de Pontuação de núcleo*) &times; (*MHz por núcleo de plataforma de destino*))  

    Usando o exemplo acima:
   >(35,83 &times; 2000) &divide; (33,75 &times; 2300) = 0,92
1. Multiplique o número estimado de processadores pelo modificador.  No caso acima, para passar do processador E5-2650 para o processador E5-2630, multiplique as 11,25 CPUs &times; calculadas 0,92 = 10,35 processadores necessários.

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>Apêndice C: Conceitos básicos sobre o sistema operacional interagindo com o armazenamento

Os conceitos de teoria de filas descritos em [tempo de resposta/como o sistema busyness impacta o desempenho](#response-timehow-the-system-busyness-impacts-performance) também são aplicáveis ao armazenamento. Ter uma familiaridade de como o sistema operacional manipula a e/s é necessário para aplicar esses conceitos. No sistema operacional Microsoft Windows, uma fila para manter as solicitações de e/s é criada para cada disco físico. No entanto, é necessário fazer um esclarecimento sobre o disco físico. Os controladores de matriz e as SANs apresentam agregações de eixos para o sistema operacional como discos físicos únicos. Além disso, os controladores de matriz e as SANs podem agregar vários discos em um conjunto de matriz e, em seguida, dividir essa matriz definida em várias "partições", que, por sua vez, é apresentada ao sistema operacional como vários discos físicos (Ref. figura).

![Eixos de bloco](media/capacity-planning-considerations-block-spindles.png)  

Nesta figura, os dois eixos são espelhados e divididos em áreas lógicas para armazenamento de dados (data 1 e data 2). Essas áreas lógicas são exibidas pelo sistema operacional como discos físicos separados.

Embora isso possa ser altamente confuso, a seguinte terminologia é usada em todo este apêndice para identificar as diferentes entidades:

- **Eixo –** o dispositivo que está fisicamente instalado no servidor.
- **Array –** uma coleção de eixos agregados pelo controlador.
- **Partição de matriz –** um particionamento da matriz agregada
- **LUN –** uma matriz, usada ao se referir a Sans
- **Disco –** O que o sistema operacional observa para ser um único disco físico.
- **Partição –** um particionamento lógico do que o sistema operacional percebe como um disco físico.

### <a name="operating-system-architecture-considerations"></a>Considerações sobre a arquitetura do sistema operacional

O sistema operacional cria uma fila de e/s de FIFO (primeiro a entrar/primeiro a sair) para cada disco observado; Esse disco pode estar representando um eixo, uma matriz ou uma partição de matriz. Do ponto de vista do sistema operacional, com relação ao tratamento de e/s, mais as filas ativas são melhores. Como uma fila FIFO é serializada, o que significa que todas as e/SS emitidas para o subsistema de armazenamento devem ser processadas na ordem em que a solicitação chegou. Ao correlacionar cada disco observado pelo sistema operacional com um eixo/matriz, o sistema operacional agora mantém uma fila de e/s para cada conjunto exclusivo de discos, eliminando assim a contenção de recursos de e/s escassos em discos e isolando a demanda de e/s para um único disco. Como uma exceção, o Windows Server 2008 apresenta o conceito de priorização de e/s e os aplicativos projetados para usar a prioridade "baixa" estão fora deste pedido normal e fazem um assento. Os aplicativos não codificados especificamente para aproveitar o padrão de prioridade "baixa" para "normal".

### <a name="introducing-simple-storage-subsystems"></a>Apresentando subsistemas de armazenamento simples

A partir de um exemplo simples (um único disco rígido dentro de um computador), uma análise de componente por componente será fornecida. Dividindo isso nos principais componentes do subsistema de armazenamento, o sistema consiste em:

- **1 a** 10.000 rpm Ultra Fast SCSI HD (Ultra Fast SCSI tem uma taxa de transferência de 20 MB/s)
- **1 –** Barramento SCSI (o cabo)
- **1 –** Adaptador SCSI ultra rápido
- barramento PCI de **1 a** 32 bits 33 MHz

Depois que os componentes são identificados, uma ideia de quantos dados podem transitar o sistema ou a quantidade de e/s pode ser tratada, pode ser calculada. Observe que a quantidade de e/s e a quantidade de dados que podem transitar o sistema estão correlacionadas, mas não as mesmas. Essa correlação depende se a e/s de disco é aleatória ou sequencial e o tamanho do bloco. (Todos os dados são gravados no disco como um bloco, mas diferentes aplicativos usando diferentes tamanhos de bloco.) Em uma base componente por componente:

- **O disco rígido –** O disco rígido de 10.000 RPM médio tem um tempo de busca de 7 milissegundos (MS) e um tempo de acesso de 3 ms. Tempo de busca é a quantidade média de tempo que leva o cabeçalho de leitura/gravação para mover para um local no platter. Tempo de acesso é a quantidade média de tempo que leva para ler ou gravar os dados no disco, uma vez que o cabeçalho está no local correto. Assim, o tempo médio para ler um bloco de dados exclusivo em um HD de 10.000 RPM constitui uma busca e um acesso para um total de aproximadamente 10 ms (ou .010 segundos) por bloco de dados.

  Quando cada acesso ao disco requer o movimento da cabeça para um novo local no disco, o comportamento de leitura/gravação é conhecido como "aleatório". Assim, quando toda e/s é aleatória, um HD de 10.000 RPM pode lidar com aproximadamente 100 e/s por segundo (IOPS) (a fórmula é 1000 MS por segundo dividida por 10 ms por e/s ou 1000/10 = 100 IOPS).

  Como alternativa, quando toda e/s ocorre de setores adjacentes no HD, isso é chamado de e/s sequencial. A e/s sequencial não tem tempo de busca porque quando a primeira e/s está concluída, o cabeçalho de leitura/gravação está no início de onde o próximo bloco de dados é armazenado no HD. Portanto, um HD de 10.000 RPM é capaz de lidar com aproximadamente 333 e/s por segundo (1000 MS por segundo dividida por 3 ms por e/s).

  >[!NOTE]
  >Este exemplo não reflete o cache de disco, onde os dados de um cilindro normalmente são mantidos. Nesse caso, os 10 MS são necessários na primeira e/s e o disco lê todo o cilindro. Todas as outras e/s sequenciais são satisfeitas do cache. Como resultado, os caches em disco podem melhorar O desempenho sequencial de e/s.
  
  Até agora, a taxa de transferência do disco rígido foi irrelevante. Se o disco rígido tem 20 MB/s de largura ou um Ultra3 160 MB/s, a quantidade real de IOPS que pode ser tratada pelo HD de 10.000 RPM é ~ 100 Random ou ~ 300 de e/s sequencial. À medida que os tamanhos de bloco são alterados com base na gravação do aplicativo na unidade, a quantidade de dados extraídos por e/s é diferente. Por exemplo, se o tamanho do bloco for de 8 KB, as operações de e/s de 100 serão lidas ou gravadas no disco rígido um total de 800 KB. No entanto, se o tamanho do bloco for 32 KB, 100 e/s lerá/gravará 3.200 KB (3,2 MB) no disco rígido. Desde que a taxa de transferência SCSI ultrapasse a quantidade total de dados transferidos, obter uma unidade de taxa de transferência "mais rápida" não receberá nada. Consulte as tabelas a seguir para comparação.

  | |7200 RPM 9ms Seek, acesso 4ms|10.000 RPM 7MS Seek, acesso 3MS|15.000 RPM 4ms Seek, acesso 2 MS
  |-|-|-|-|
  |E/s aleatória|80|100|150|
  |E/s sequencial|250|300|500|  
  
  |unidade RPM de 10.000|tamanho do bloco de 8 KB (Active Directory Jet)|
  |-|-|
  |E/s aleatória|800 KB/s|
  |E/s sequencial|2400 KB/s|

- **Backplane SCSI (barramento) –** Entender como o "backplane SCSI (barramento)", ou nesse cenário, o cabo da faixa de uma, afeta a taxa de transferência do subsistema de armazenamento depende do conhecimento do tamanho do bloco. Essencialmente, a pergunta seria o quanto a e/s pode ser a alça do barramento se a e/s estiver em blocos de 8 KB? Nesse cenário, o barramento SCSI é de 20 MB/s ou 20480 KB/s. 20480 KB/s divididos por 8 KB blocos gera um máximo de aproximadamente 2500 IOPS com suporte pelo barramento SCSI.

  >[!NOTE]
  >Os valores na tabela a seguir representam um exemplo. A maioria dos dispositivos de armazenamento anexados atualmente usa o PCI Express, que fornece uma taxa de transferência muito maior.  
  
  |E/s com suporte do barramento SCSI por tamanho de bloco|tamanho do bloco de 2 KB|tamanho do bloco de 8 KB (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10.000|2\.500|
  |40 MB/s|20.000|5\.000|
  |128 MB/s|65.536|16.384|
  |320 MB/s|160.000|40.000|

  Como pode ser determinado neste gráfico, no cenário apresentado, não importa qual seja o uso, o barramento nunca será um afunilamento, pois o máximo do eixo é de 100 e/s, bem abaixo de qualquer um dos limites acima.

  >[!NOTE]
  >Isso pressupõe que o barramento SCSI seja 100% eficiente.
  
- **Adaptador SCSI –** Para determinar a quantidade de e/s que isso pode manipular, as especificações do fabricante precisam ser verificadas. Direcionar solicitações de e/s para o dispositivo apropriado requer processamento de algum tipo, portanto, a quantidade de e/s que pode ser tratada depende do processador de adaptador SCSI (ou controlador de matriz).

  Neste exemplo, a suposição de que 1.000 e/s pode ser tratada será feita.

- **Barramento PCI –** Esse é um componente com frequência ignorado. Neste exemplo, esse não será o afunilamento; no entanto, à medida que os sistemas aumentam, ele pode se tornar um afunilamento. Para referência, um barramento PCI de 32 bits operando em 33Mhz pode, na teoria, transferir 133 MB/s de dados. A seguir está a equação:  
  > 32 bits &divide; 8 bits por byte &times; 33 MHz = 133 MB/s.  

  Observe que é o limite teórico; na realidade, apenas cerca de 50% do máximo é realmente atingido, embora em determinados cenários de intermitência, a eficiência de 75% pode ser obtida por períodos curtos.

  Um barramento PCI 66MHz de 64 bits pode dar suporte a um máximo teórico de ( &divide; 64 bits 8 bits &times; por byte 66 MHz) = 528 MB/s. Além disso, qualquer outro dispositivo (como o adaptador de rede, o segundo controlador SCSI e assim por diante) reduzirá a largura de banda disponível à medida que a largura de banda é compartilhada e os dispositivos irão lutar para os recursos limitados.

Após a análise dos componentes desse subsistema de armazenamento, o eixo é o fator limitante na quantidade de e/s que pode ser solicitada e, consequentemente, a quantidade de dados que podem transitar o sistema. Especificamente, em um cenário de AD DS, é 100 e/s aleatória por segundo em incrementos de 8 KB, para um total de 800 KB por segundo ao acessar o banco de dados Jet. Como alternativa, a taxa de transferência máxima para um eixo que é alocada exclusivamente para arquivos de log afetaria as seguintes limitações: a e/s sequencial de 300 por segundo em incrementos de 8 KB, para um total de 2400 KB (2,4 MB) por segundo.

Agora, tendo analisado uma configuração simples, a tabela a seguir demonstra onde o afunilamento ocorrerá, pois os componentes no subsistema de armazenamento são alterados ou adicionados.

|Observações|Análise de afunilamento|Disco|Bus|Adaptador|Barramento PCI|
|-|-|-|-|-|-|
|Essa é a configuração do controlador de domínio depois de adicionar um segundo disco. A configuração de disco representa o afunilamento em 800 KB/s.|Adicionar 1 disco (total = 2)<br /><br />E/s é aleatória<br /><br />tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM|total de 200 I/os<br />total de 800 KB/s.| | | |
|Depois de adicionar 7 discos, a configuração de disco ainda representa o afunilamento em 3200 KB/s.|**Adicionar 7 discos (total = 8)**  <br /><br />E/s é aleatória<br /><br />tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM|total de 800 I/os.<br />total de 3200 KB/s| | | |
|Depois de alterar a e/s para sequencial, o adaptador de rede torna-se o afunilamento porque está limitado a 1000 IOPS.|Adicionar 7 discos (total = 8)<br /><br />**E/s é sequencial**<br /><br />tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM| | |2400 e/s s podem ser lidas/gravadas no disco, controlador limitada a 1000 IOPS| |
|Depois de substituir o adaptador de rede por um adaptador SCSI que dá suporte a 10.000 IOPS, o afunilamento retorna à configuração do disco.|Adicionar 7 discos (total = 8)<br /><br />E/s é aleatória<br /><br />tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM<br /><br />**Atualizar o adaptador SCSI (agora dá suporte a 10.000 e/s)**|total de 800 I/os.<br />total de 3.200 KB/s| | | |
|Depois de aumentar o tamanho do bloco para 32 KB, o barramento se tornará o afunilamento, pois ele só dá suporte a 20 MB/s.|Adicionar 7 discos (total = 8)<br /><br />E/s é aleatória<br /><br />**tamanho do bloco de 32 KB**<br /><br />HD DE 10.000 RPM| |total de 800 I/os. 25.600 KB/s (25 MB/s) podem ser lidos/gravados no disco.<br /><br />O barramento só dá suporte a 20 MB/s| | |
|Depois de atualizar o barramento e adicionar mais discos, o disco permanece o afunilamento.|**Adicionar 13 discos (total = 14)**<br /><br />Adicionar segundo adaptador SCSI com 14 discos<br /><br />E/s é aleatória<br /><br />tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM<br /><br />**Atualizar para o barramento SCSI de 320 MB/s**|2800 I/os<br /><br />11.200 KB/s (10,9 MB/s)| | | |
|Depois de alterar a e/s para sequencial, o disco permanece o afunilamento.|Adicionar 13 discos (total = 14)<br /><br />Adicionar segundo adaptador SCSI com 14 discos<br /><br />**E/s é sequencial**<br /><br />tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM<br /><br />Atualizar para o barramento SCSI de 320 MB/s|8\.400 I/os<br /><br />33.600 KB\s<br /><br />(32,8 MB\s)| | | |
|Depois de adicionar discos rígidos mais rápidos, o disco permanece o afunilamento.|Adicionar 13 discos (total = 14)<br /><br />Adicionar segundo adaptador SCSI com 14 discos<br /><br />E/s é sequencial<br /><br />tamanho do bloco de 4 KB<br /><br />**HD DE 15.000 RPM**<br /><br />Atualizar para o barramento SCSI de 320 MB/s|14.000 I/os<br /><br />56.000 KB/s<br /><br />(54,7 MB/s)| | | |
|Depois de aumentar o tamanho do bloco para 32 KB, o barramento PCI torna-se o afunilamento.|Adicionar 13 discos (total = 14)<br /><br />Adicionar segundo adaptador SCSI com 14 discos<br /><br />E/s é sequencial<br /><br />**tamanho do bloco de 32 KB**<br /><br />HD DE 15.000 RPM<br /><br />Atualizar para o barramento SCSI de 320 MB/s| | | |14.000 I/os<br /><br />448.000 KB/s<br /><br />(437 MB/s) é o limite de leitura/gravação para o eixo.<br /><br />O barramento PCI dá suporte a um máximo teórico de 133 MB/s (75% eficiente na melhor das hipóteses).|

### <a name="introducing-raid"></a>Apresentando o RAID

A natureza de um subsistema de armazenamento não muda drasticamente quando um controlador de matriz é introduzido; Ele substitui apenas o adaptador SCSI nos cálculos. O que muda é o custo de leitura e gravação de dados no disco ao usar os vários níveis de matriz (como RAID 0, RAID 1 ou RAID 5).

No RAID 0, os dados são distribuídos em todos os discos no conjunto de RAID. Isso significa que durante uma operação de leitura ou de gravação, uma parte dos dados é retirada ou enviada por push para cada disco, aumentando a quantidade de dados que podem transitar o sistema durante o mesmo período de tempo. Portanto, em um segundo, em cada eixo (supondo novamente unidades de 10.000 RPM), 100 operações de e/s podem ser executadas. A quantidade total de e/s que pode ter suporte é N eixos vezes 100 e/s por segundo por eixo (produz 100 * N e/s por segundo).

![Lógica d: unidade](media/capacity-planning-considerations-logical-d-drive.png)

No RAID 1, os dados são espelhados (duplicados) em um par de eixos para redundância. Assim, quando uma operação de leitura de e/s é executada, os dados podem ser lidos de ambos os eixos no conjunto. Isso efetivamente torna a capacidade de e/s de ambos os discos disponíveis durante uma operação de leitura. A limitação é que as operações de gravação não têm vantagem de desempenho em um RAID 1. Isso ocorre porque os mesmos dados precisam ser gravados em ambas as unidades para fins de redundância. Embora não demore mais, pois a gravação de dados ocorre simultaneamente em ambos os eixos, porque ambos os eixos estão ocupados duplicando os dados, uma operação de e/s de gravação em essência impede que duas operações de leitura ocorram. Portanto, cada e/s de gravação custa duas e/s de leitura. Uma fórmula pode ser criada com base nessas informações para determinar o número total de operações de e/s que estão ocorrendo:  

> *Ler e/s* + 2 &times; *gravar e/s* = total de e/s de*disco disponível* consumida  

Quando a taxa de leituras para gravações e o número de eixos são conhecidos, a equação a seguir pode ser derivada da equação acima para identificar a e/s máxima que pode ser suportada pela matriz:  

> *IOPS máximo por fuso* &times; 2 fuso &times; [( *%lê* +  *%Escreve*) &divide; ( *%lê* + 2 &times; *%Escreve*)] = *Total IOPS*

RAID 1 + 0, comporta-se exatamente o mesmo que RAID 1 em relação às despesas de leitura e gravação. No entanto, a e/s agora é distribuída em cada conjunto espelhado. Se  

> *IOPS máximo por fuso* &times; 2 fuso &times; [( *%lê* +  *%Escreve*) &divide; ( *%lê* + 2 &times; *%Escreve*)] = *Total IOPS*  

em um conjunto de RAID 1, quando uma multiplicidade (*N*) de conjuntos RAID 1 são distribuídos, a e/s total que pode ser processada se torna &times; N e/s por conjunto RAID 1:  

> *N* &times; {*IOPS máximo por fuso* &times; 2 fuso &times; [( *%lê*  +  *%E*) &divide; ( *%lê* + 2 &times; *%Escreve*)] } = *Total IOPS*

No RAID 5, às vezes referido como *n* + 1 RAID, os dados são distribuídos entre os eixos *n* e as informações de paridade são gravadas no eixo "+ 1". No entanto, o RAID 5 é muito mais caro ao executar uma e/s de gravação do que RAID 1 ou 1 + 0. O RAID 5 executa o seguinte processo toda vez que uma e/s de gravação é enviada para a matriz:

1. Ler os dados antigos
1. Ler a paridade antiga
1. Gravar os novos dados
1. Gravar a nova paridade

Como cada solicitação de e/s de gravação enviada ao controlador de matriz pelo sistema operacional requer quatro operações de e/s a serem concluídas, as solicitações de gravação enviadas levam quatro vezes mais tempo para serem concluídas como uma única e/s de leitura. Para derivar uma fórmula para converter as solicitações de e/s da perspectiva do sistema operacional para aquela que os eixos tiveram:  

> *Ler e/s* + 4 &times; *gravação* = e/s total de e */s*  

Da mesma forma, em um conjunto de RAID 1, quando a taxa de leituras para gravações e o número de eixos são conhecidos, a equação a seguir pode ser derivada da equação acima para identificar o máximo de e/s que pode ser suportado pela matriz (Observe que o número total de eixos não inclui e a "unidade" é perdida para paridade:  

> *IOPS por fuso* &times; (*fuso* – 1) &times; [( *%lê* +  *%Escreve*) &divide; ( *%lê* + 4 &times; *%Escreve*)] = *Total IOPS*

### <a name="introducing-sans"></a>Apresentando SANs

Ao expandir a complexidade do subsistema de armazenamento, quando uma SAN é introduzida no ambiente, os princípios básicos descritos não são alterados, no entanto, o comportamento de e/s para todos os sistemas conectados à SAN precisa ser levado em conta. Como uma das principais vantagens no uso de uma SAN é uma quantidade adicional de redundância no armazenamento conectado internamente ou externamente, o planejamento de capacidade agora precisa levar em conta as necessidades de tolerância a falhas. Além disso, são introduzidos mais componentes que precisam ser avaliados. Dividindo uma SAN em partes do componente:

- Unidade de disco rígido SCSI ou Fibre Channel
- Backplane de canal da unidade de armazenamento
- Unidades de armazenamento
- Módulo do controlador de armazenamento
- Comutador (es) SAN
- HBA (s)
- O barramento PCI

Ao criar qualquer sistema para redundância, os componentes adicionais são incluídos para acomodar o potencial de falha. É muito importante, quando o planejamento de capacidade, excluir o componente redundante dos recursos disponíveis. Por exemplo, se a SAN tiver dois módulos de controlador, a capacidade de e/s de um módulo de controlador será tudo o que deve ser usado para a taxa de transferência de e/s total disponível para o sistema. Isso se deve ao fato de que, se um controlador falhar, toda a carga de e/s exigida por todos os sistemas conectados precisará ser processada pelo controlador restante. Como todo o planejamento de capacidade é feito para períodos de pico de uso, os componentes redundantes não devem ser fatorados nos recursos disponíveis e a utilização de pico planejada não deve exceder 80% de saturação do sistema (para acomodar intermitências ou sistemas anormais comportamento). Da mesma forma, o comutador de SAN redundante, a unidade de armazenamento e os eixos não devem ser fatorados nos cálculos de e/s.

Ao analisar o comportamento do disco rígido SCSI ou Fibre Channel, o método de análise do comportamento, conforme descrito anteriormente, não é alterado. Embora haja determinadas vantagens e desvantagens em cada protocolo, o fator de limitação por disco é a limitação mecânica do disco rígido.

Analisar o canal na unidade de armazenamento é exatamente o mesmo que calcular os recursos disponíveis no barramento SCSI, ou largura de banda (como 20 MB/s) dividido pelo tamanho do bloco (como 8 KB). O local em que isso se desvia do exemplo anterior simples está na agregação de vários canais. Por exemplo, se houver 6 canais, cada um com suporte para taxa de transferência máxima de 20 MB/s, a quantidade total de e/s e transferência de dados disponível é de 100 MB/s (isso está correto, não é 120 MB/s). Novamente, a tolerância a falhas é um jogador principal nesse cálculo, no caso da perda de um canal inteiro, o sistema só é deixado com 5 canais de funcionamento. Portanto, para garantir que continuem a atender às expectativas de desempenho em caso de falha, a taxa de transferência total para todos os canais de armazenamento não deve exceder 100 MB/s (isso pressupõe que a carga e a tolerância a falhas são distribuídas uniformemente entre todos os canais). Transformar isso em um perfil de e/s depende do comportamento do aplicativo. No caso de Active Directory e/s do Jet, isso correlacionaria a aproximadamente 12.500 e/s por segundo (100 MB/s &divide; 8 KB por e/s).

Em seguida, é necessário obter as especificações do fabricante para os módulos do controlador para obter uma compreensão da taxa de transferência de que cada módulo pode dar suporte. Neste exemplo, a SAN tem dois módulos de controlador que dão suporte a 7.500 e/s cada. A taxa de transferência total do sistema poderá ser de 15.000 IOPS se a redundância não for desejada. No cálculo da taxa de transferência máxima em caso de falha, a limitação é a taxa de transferência de um controlador ou IOPS de 7.500. Esse limite está abaixo do máximo de 12.500 IOPS (supondo que o tamanho do bloco de 4 KB) que pode ser suportado por todos os canais de armazenamento e, portanto, é atualmente o afunilamento na análise. Ainda para fins de planejamento, a e/s máxima desejada para ser planejada seria de 10.400 e/s.

Quando os dados saem do módulo do controlador, ele transita um Fibre Channel conexão classificada a 1 GB/s (ou 1 gigabit por segundo). Para correlacionar isso com as outras métricas, 1 GB/s se transforma em 128 MB/s (1 GB &divide; /s 8 bits/byte). Como isso excede a largura de banda total em todos os canais na unidade de armazenamento (100 MB/s), isso não causará o afunilamento do sistema. Além disso, como se trata de apenas um dos dois canais (a conexão adicional de 1 GB/s Fibre Channel a redundância), se uma conexão falhar, a conexão restante ainda terá capacidade suficiente para lidar com toda a transferência de dados exigida.

Para rotear para o servidor, os dados provavelmente funcionarão como um comutador SAN. Como o comutador SAN precisa processar a solicitação de e/s de entrada e encaminhá-la à porta apropriada, o comutador terá um limite para a quantidade de e/s que pode ser tratada. no entanto, as especificações do fabricante serão necessárias para determinar o que é esse limite. Por exemplo, se houver duas opções e cada opção puder lidar com 10.000 IOPS, a taxa de transferência total será de 20.000 IOPS. Novamente, a tolerância a falhas é uma preocupação, se um comutador falhar, a taxa de transferência total do sistema será de 10.000 IOPS. Como ele é desejado para não exceder a utilização de 80% na operação normal, usar no máximo 8000 e/s deve ser o destino.

Por fim, o HBA instalado no servidor também teria um limite para a quantidade de e/s que ele pode manipular. Normalmente, um segundo HBA é instalado para redundância, mas assim como com o comutador SAN, ao calcular o máximo de e/s que pode ser manipulado, a taxa de transferência total de *N* &ndash; 1 HBAs é o que é a escalabilidade máxima do sistema.

### <a name="caching-considerations"></a>Considerações sobre cache

Os caches são um dos componentes que podem afetar significativamente o desempenho geral em qualquer ponto do sistema de armazenamento. A análise detalhada sobre algoritmos de cache está além do escopo deste artigo; no entanto, algumas instruções básicas sobre o cache em subsistemas de disco vale a pena acender:

- O Caching aprimorou e/s de gravação sequencial sustentada, pois pode armazenar em buffer muitas operações de gravação menores em blocos de e/s maiores e despreparar para armazenamento em um tamanho menor, mas maior. Isso reduzirá a e/s aleatória total e a e/s sequencial total, fornecendo mais disponibilidade de recursos para outras e/s.
- O Caching não melhora a taxa de transferência de e/s de gravação sustentada do subsistema de armazenamento. Ele só permite que as gravações sejam armazenadas em buffer até que os eixos estejam disponíveis para confirmar os dados. Quando todas as e/s disponíveis dos eixos no subsistema de armazenamento estão saturadas por longos períodos, o cache, eventualmente, será preenchido. Para esvaziar o cache, é necessário alocar tempo suficiente entre picos ou eixos extras para fornecer e/s suficiente para permitir que o cache seja liberado.

  Caches maiores permitem que mais dados sejam armazenados em buffer. Isso significa que períodos mais longos de saturação podem ser acomodados.

  Em um subsistema de armazenamento normalmente operacional, o sistema operacional terá um desempenho de gravação aprimorado, pois os dados só precisam ser gravados no cache. Depois que a mídia subjacente for saturada com e/s, o cache será preenchido e o desempenho de gravação retornará à velocidade do disco.

- Ao armazenar em cache e/s de leitura, o cenário em que o cache é mais vantajoso é quando os dados são armazenados sequencialmente no disco, e o cache pode ser lido antecipadamente (faz a suposição de que o próximo setor contenha os dados que serão solicitados a seguir).
- Quando a e/s de leitura é aleatória, o cache no controlador da unidade é improvável de fornecer qualquer aprimoramento à quantidade de dados que podem ser lidos do disco. Qualquer aprimoramento será inexistente se o tamanho do cache baseado no aplicativo ou no sistema operacional for maior do que o tamanho do cache baseado em hardware.

  No caso de Active Directory, o cache é limitado apenas pela quantidade de RAM.

### <a name="ssd-considerations"></a>Considerações sobre SSD

SSDs são um animal completamente diferente dos discos rígidos baseados em eixo. Ainda assim, os dois critérios principais permanecem: "Quantos IOPS ele pode lidar?" e "Qual é a latência para esses IOPS?" Em comparação com discos rígidos baseados em eixo, o SSDs pode lidar com volumes maiores de e/s e pode ter latências menores. Em geral e no momento da redação deste artigo, embora o SSDs ainda seja caro em uma comparação de custo por gigabyte, eles são muito baratos em termos de custo por e/s e merecem uma consideração significativa em termos de desempenho de armazenamento.

Considerações:

- Tanto o IOPS quanto as latências são muito subjetivas para os designs de fabricantes e, em alguns casos, foram observados como um desempenho mais baixo do que as tecnologias baseadas em eixo. Em suma, é mais importante revisar e validar a unidade de especificações do fabricante por unidade e não pressupor nenhuma generalidade.
- Os tipos de IOPS podem ter números muito diferentes, dependendo se são de leitura ou gravação. AD DS serviços, em geral, sendo predominantemente baseados em leitura, serão menos afetados do que alguns outros cenários de aplicativos.
- "Write endurance" – esse é o conceito que as células SSD eventualmente esgotarão. Vários fabricantes lidam com esse desafio de maneira diferente. Pelo menos para a unidade de banco de dados, o perfil de e/s de leitura predominantemente permite downplaying o significado dessa preocupação, pois os dados não são altamente voláteis.

### <a name="summary"></a>Resumo

Uma maneira de pensar sobre o armazenamento é o encanamento doméstico Picturing. Imagine o IOPS da mídia na qual os dados são armazenados é o principal dreno doméstico. Quando isso é saturado (como raízes no pipe) ou limitado (é recolhido ou muito pequeno), todos os coletores na residência fazem backup quando muita água está sendo usada (muitos convidados). Isso é perfeitamente análogo a um ambiente compartilhado em que um ou mais sistemas estão aproveitando o armazenamento compartilhado em um SAN/NAS/iSCSI com a mesma mídia subjacente. Abordagens diferentes podem ser tomadas para resolver os diferentes cenários:

- Um dreno recolhido ou redimensionado requer uma substituição e uma correção completas de escala. Isso seria semelhante à adição em um novo hardware ou à redistribuição dos sistemas usando o armazenamento compartilhado em toda a infraestrutura.
- Um pipe "saturado" geralmente significa a identificação de um ou mais problemas problemáticos e a remoção desses problemas. Em um cenário de armazenamento, isso pode ser o armazenamento ou backups no nível do sistema, verificações de antivírus sincronizadas em todos os servidores e o software de desfragmentação sincronizado em execução durante períodos de pico.

Em qualquer design de encanamento, vários esgotamentos são alimentados no dreno principal. Se algo parar um desses esgotamentos ou um ponto de junção, somente as coisas por trás desse ponto de junção fazem backup. Em um cenário de armazenamento, isso pode ser uma opção sobrecarregada (cenário de SAN/NAS/iSCSI), problemas de compatibilidade de driver (combinação incorreta de firmware/HBA/Storport. sys) ou backup/antivírus/desfragmentação. Para determinar se o "pipe" de armazenamento é grande o suficiente, IOPS e tamanho de e/s precisam ser medidos. Em cada conjunto, adicione-os em conjunto para garantir "diâmetro de pipe" adequado.

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>Apêndice D-discussão sobre solução de problemas de armazenamento – ambientes em que fornecer pelo menos tanta RAM quanto o tamanho do banco de dados não é uma opção viável

É útil entender por que essas recomendações existem para que as alterações na tecnologia de armazenamento possam ser acomodadas. Essas recomendações existem por dois motivos. A primeira é o isolamento da e/s, de modo que problemas de desempenho (ou seja, paginação) no eixo do sistema operacional não afetem o desempenho do banco de dados e dos perfis de e/s. O segundo é que os arquivos de log para AD DS (e a maioria dos bancos de dados) são sequenciais por natureza, e os discos rígidos baseados no eixo e os caches têm um enorme benefício de desempenho quando usados com e/s sequencial em comparação com os padrões de e/s mais aleatórios do sistema operacional e quase padrões de e/s puramente aleatórios da unidade de banco de dados AD DS. Ao isolar a e/s sequencial para uma unidade física separada, a taxa de transferência pode ser aumentada. O desafio apresentado pelas opções de armazenamento de hoje é que as suposições fundamentais por trás dessas recomendações não são mais verdadeiras. Em muitos cenários de armazenamento virtualizados, como o iSCSI, SAN, NAS e arquivos de imagem de disco virtual, a mídia de armazenamento subjacente é compartilhada entre vários hosts, o que nega completamente o "isolamento de e/s" e os aspectos "otimização de e/SS sequencial". Na verdade, esses cenários adicionam uma camada adicional de complexidade, pois os outros hosts que acessam a mídia compartilhada podem degradar a capacidade de resposta para o controlador de domínio.

No planejamento do desempenho do armazenamento, há três categorias a serem consideradas: estado de cache frio, estado de cache quente e backup/restauração. O estado de cache frio ocorre em cenários como quando o controlador de domínio é inicialmente reinicializado ou o serviço de Active Directory é reiniciado e não há dados de Active Directory na RAM. Estado de cache quente é onde o controlador de domínio está em um estado estável e o banco de dados é armazenado em cache. É importante observar que eles vão gerar perfis de desempenho muito diferentes e ter RAM suficiente para armazenar em cache todo o banco de dados não ajuda o desempenho quando o cache está frio. É possível considerar o design de desempenho para esses dois cenários com a analogia a seguir, o aquecimento do cache frio é um "Sprint" e a execução de um servidor com cache quente é um "Marathon".

Para o cenário do cache frio e do cache ativo, a pergunta torna-se a rapidez com que o armazenamento pode mover os dados do disco para a memória. O aquecimento do cache é um cenário em que, ao longo do tempo, o desempenho melhora à medida que mais consultas reutilizam dados, a taxa de acesso ao cache aumenta e a frequência de necessidade de ir para o disco diminui. Como resultado, o impacto adverso no desempenho do disco diminui. Qualquer degradação no desempenho só é transitória enquanto aguarda o cache ficar quente e aumentar para o tamanho máximo permitido pelo sistema. A conversa pode ser simplificada com a rapidez com que os dados podem ser tirados do disco e é uma medida simples dos IOPS disponíveis para Active Directory, que é subjetiva para IOPS disponíveis no armazenamento subjacente. Do ponto de vista do planejamento, como o aquecimento do cache e os cenários de backup/restauração ocorrem de forma excepcional, normalmente ocorrem fora do horário e estão sujeitos à carga do DC, as recomendações gerais não existem, exceto que essas atividades sejam agendadas para horários que não são de pico.

AD DS, na maioria dos cenários, é basicamente a leitura de e/s, geralmente uma taxa de 90% de leitura/10% de gravação. A leitura de e/s geralmente tende a ser o afunilamento da experiência do usuário e com a e/s de gravação, gera desempenho de gravação para degradar. À medida que a e/s para o NTDS. dit é predominantemente aleatória, os caches tendem a fornecer um benefício mínimo para ler e/s, o que torna muito mais importante configurar o armazenamento para o perfil de e/SS de leitura corretamente.

Para condições normais de operação, a meta de planejamento de armazenamento é minimizar os tempos de espera para uma solicitação de AD DS ser retornada do disco. Isso essencialmente significa que o número de e/s pendentes e pendentes é menor ou equivalente ao número de caminhos para o disco. Há várias maneiras de medir isso. Em um cenário de monitoramento de desempenho, a recomendação geral é que o LogicalDisk ( *\<unidade\>de banco de dados NTDS*) \Avg de disco s/leitura seja inferior a 20 ms. O limite de operação desejado deve ser muito menor, preferencialmente o mais próximo da velocidade do armazenamento possível, no intervalo de 2 a 6 milissegundos (. 002 a .006 segundo), dependendo do tipo de armazenamento.

Exemplo:

![Gráfico de latência de armazenamento](media/capacity-planning-considerations-storage-latency.png)

Analisando o gráfico:

- **Oval verde à esquerda –** A latência permanece consistente em 10 ms. A carga aumenta de 800 IOPS para 2400 IOPS. Essa é a base absoluta para a rapidez com que uma solicitação de e/s pode ser processada pelo armazenamento subjacente. Isso está sujeito às especificações da solução de armazenamento.
- **Burgundy oval à direita –** A taxa de transferência permanece simples a partir da saída do círculo verde até o final da coleta de dados, enquanto a latência continua a aumentar. Isso demonstra que, quando os volumes de solicitação excedem as limitações físicas do armazenamento subjacente, mais tempo as solicitações gastam na fila aguardando para serem enviadas para o subsistema de armazenamento.

Aplicando este conhecimento:

- **Impacto em um usuário consultando a associação de um grupo grande –** Suponha que isso exija a leitura de 1 MB de dados do disco, a quantidade de e/s e o tempo necessário que podem ser avaliados da seguinte maneira:
  - Active Directory páginas de banco de dados têm 8 KB de tamanho. 
  - Um mínimo de 128 páginas precisa ser lido no disco. 
  - Supondo que nada seja armazenado em cache, no andar (10 ms), isso levará um mínimo de 1,28 segundos para carregar os dados do disco para retorná-los ao cliente. A 20 ms, em que a taxa de transferência no armazenamento tem tempo desde o esgotamento e também é o máximo recomendado, levará 2,5 segundos para obter os dados do disco, a fim de devolvê-los ao usuário final.  
- **Em que taxa o cache ficará quente –** Tomando a suposição de que a carga do cliente vai maximizar a taxa de transferência neste exemplo de armazenamento, o cache ficará quente a uma taxa &times; de 2400 IOPS de 8 KB por e/s. Ou aproximadamente 20 MB/s por segundo, carregando cerca de 1 GB de banco de dados na RAM a cada 53 segundos.

> [!NOTE]
> É normal que os curtos períodos observem a escala de latência quando os componentes lêem ou gravam agressivamente em disco, como quando o backup do sistema está sendo feito ou quando AD DS está executando a coleta de lixo. Uma sala de cabeça adicional sobre os cálculos deve ser fornecida para acomodar esses eventos periódicos. O objetivo é fornecer uma taxa de transferência suficiente para acomodar esses cenários sem afetar a função normal.

Como pode ser visto, há um limite físico com base no design de armazenamento para a rapidez com que o cache pode possivelmente quente. O que resultará no cache de solicitações de entrada do cliente até a taxa que o armazenamento subjacente pode fornecer. Executar scripts para "pré-quente" o cache durante horários de pico fornecerá à competição a carga orientada por solicitações reais de cliente. Isso pode afetar negativamente a entrega de dados que os clientes precisam primeiro porque, por design, ele gerará a competição por recursos de disco escassos, pois as tentativas artificiais do cache carregarão os dados que não são relevantes para os clientes que entram em contato com o controlador de domínio.
