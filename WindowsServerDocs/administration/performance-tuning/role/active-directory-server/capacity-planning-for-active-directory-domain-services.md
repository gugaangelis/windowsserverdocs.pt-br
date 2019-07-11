---
title: Planejamento de capacidade para os serviços de domínio do Active Directory
description: Discussão detalhada sobre os fatores a considerar durante o planejamento de capacidade do AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 5a9e2d39d4eedd1e8fdb4bfeaf267ad4cb4c596a
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799828"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Planejamento de capacidade para os serviços de domínio do Active Directory

Este tópico é criado originalmente por Ken Brumfield, chefe de campo sênior na Microsoft e fornece recomendações para planejamento de capacidade para os serviços de domínio do Active Directory (AD DS).

## <a name="goals-of-capacity-planning"></a>Metas do planejamento de capacidade

Planejamento de capacidade não é o mesmo que a solução de problemas de incidentes de desempenho. Eles estão intimamente relacionados, mas é bastante diferente. Os objetivos do planejamento de capacidade são:  

- Implementar e operar um ambiente corretamente 
- Minimize os tempo gasto Solucionando problemas de desempenho.  
  
No planejamento de capacidade, uma organização pode ter um destino de linha de base % a 40% de utilização do processador durante períodos de pico para atender às necessidades de desempenho do cliente e acomodar o tempo necessário para atualizar o hardware no datacenter. Por outro lado, para ser notificado de incidentes de desempenho anormal, um limite de alerta de monitoramento pode ser definido em 90% durante um intervalo de 5 minutos.

A diferença é que, quando um limite de gerenciamento de capacidade é continuamente excedido (um evento único não é uma preocupação), adicionando capacidade (que é, adicionando nos processadores mais rápidos ou mais) seria uma solução ou o serviço de dimensionamento em vários servidores, seria um solução. Limites de alerta de desempenho indicam que atualmente está apresentando problemas a experiência do cliente e imediatas etapas são necessárias para resolver o problema.

Como uma analogia: gerenciamento de capacidade é sobre como evitar um acidente de carro (defensiva dirigir, tornando-se de que os freios estão funcionando corretamente, e assim por diante), enquanto a solução de problemas de desempenho é o que fazem a polícia, departamento de incêndio e profissionais de médicos de emergências Após um acidente. Trata-se "dirigindo defensiva" estilo de diretório Active Directory.

Nos últimos anos, as diretrizes de planejamento de capacidade para sistemas de expansão mudou drasticamente. As seguintes alterações nas arquiteturas de sistema desafiaram fundamentais suposições sobre como criar e dimensionar um serviço:

- plataformas de servidor de 64 bits  
- Virtualização  
- Maior atenção para o consumo de energia  
- Armazenamento SSD  
- Cenários de nuvem  

Além disso, a abordagem está mudando de uma exercício para um exercício de planejamento de capacidade baseada em serviços de planejamento de capacidade baseada em servidor. Active Directory Domain Services (AD DS), um serviço distribuído maduro que muitos produtos da Microsoft e produtos de terceiros usam como um back-end se torna um os produtos mais importantes ao planejar corretamente garantir a capacidade necessária para a execução de outros aplicativos.

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>Requisitos de linha de base para a orientação de planejamento de capacidade

Ao longo deste artigo, os seguintes requisitos de linha de base são esperados:

- Leitores de ter lido e está familiarizados com [desempenho ajustando as diretrizes do Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- A plataforma do Windows Server é um x64 com base em arquitetura. Mas mesmo se seu ambiente do Active Directory é instalado no Windows Server 2003 x86 (agora além do fim do ciclo de vida de suporte) e tem uma diretório informações DIT (árvore) que é menos 1,5 GB de tamanho e que pode facilmente ser mantido na memória, as diretrizes deste artigo ainda são aplicáveis.
- Planejamento de capacidade é um processo contínuo, e você deve examinar regularmente o quão bem o ambiente está atendendo às expectativas.
- Otimização será ocorrer durante vários ciclos de vida do hardware, como alteração de custos de hardware. Por exemplo, memória se torna mais barata, diminui o custo por núcleo ou o preço do armazenamento de diferente opções de alteração.
- Plano para o período de pico ocupado do dia. É recomendável examinar isso em intervalos de 30 minutos ou horas. Qualquer valor maior pode ocultar os picos reais e qualquer coisa que menor poderá ser distorcida pelo "picos transitórios."
- Planeje o crescimento ao longo do ciclo de vida do hardware para a empresa. Isso pode incluir uma estratégia de atualização ou adição de hardware de maneira irregular ou uma atualização completa a cada três a cinco anos. Cada um exigirá um "adivinhar" como crescerá muito a carga no Active Directory. Os dados históricos, se coletados, ajudará com essa avaliação. 
- Planejar para tolerância a falhas. Uma vez uma estimativa *N* é derivado, o plano para cenários que incluem *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Adicione servidores adicionais conforme a necessidade de organização para garantir que a perda de um único ou vários servidores não exceda as estimativas de capacidade máxima de pico.
  - Além disso, considere que o plano de crescimento e o plano de tolerância a falhas precisam ser integrados. Por exemplo, se um controlador de domínio é necessária para suportar a carga, mas a previsão é que a carga será ser duplicada no próximo ano e exigem dois controladores de domínio totais, não haja capacidade suficiente para dar suporte à tolerância a falhas. A solução seria começar com três controladores de domínio. Um também pode planejar adicionar o controlador de domínio de terceiro após 3 ou 6 meses se orçamentos apertados.

    >[!NOTE]
    >Adicionar aplicativos compatíveis com o Active Directory pode ter um impacto significativo sobre a carga do controlador de domínio, se a carga é proveniente de servidores de aplicativos ou clientes.

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>Processo de três etapas para o ciclo de planejamento de capacidade

No planejamento de capacidade, primeiro decida quais qualidade de serviço é necessária. Por exemplo, um datacenter core dá suporte a um nível mais alto de simultaneidade e requer experiência mais consistente para usuários e aplicativos, que exige a maior atenção para redundância de consumo e minimizando os gargalos do sistema e da infraestrutura. Por outro lado, um local de satélite com alguns usuários não precisa de mesmo nível de simultaneidade ou tolerância a falhas. Assim, o escritório satélite talvez não seja necessário muita atenção para otimizar o hardware subjacente e a infraestrutura, que pode levar a economias de custo. Todas as recomendações e diretrizes aqui são para otimizar o desempenho e podem ser relaxados seletivamente para cenários com menos exigentes requisitos.

A próxima pergunta é: virtualizado ou físico? De uma perspectiva de planejamento de capacidade, não há nenhuma resposta certa ou errada; Há apenas um outro conjunto de variáveis para trabalhar com. Cenários de virtualização descer para uma das duas opções:

- "Mapeamento direto" com um convidado por host (onde a virtualização existe exclusivamente para abstrair o hardware físico do servidor)
- "Host compartilhado"

Cenários de teste e produção indicam que o cenário de "mapeamento direto" pode ser tratado de forma idêntica para um host físico. No entanto, "Compartilhada host," apresenta várias considerações, descrito em mais detalhes posteriormente. O cenário de "host compartilhado" significa que o AD DS também estejam competindo pelos recursos, e há penalidades e considerações de ajustes para fazer isso.

Com essas considerações em mente, o ciclo de planejamento de capacidade é um processo iterativo de três etapas:

1. Medir o ambiente existente, determinar onde os gargalos do sistema no momento estão e obtém Noções básicas de ambientais necessárias para planejar a quantidade da capacidade necessária.
1. Determine o hardware necessário de acordo com os critérios descritos na etapa 1.
1. Monitorar e validar se a infraestrutura conforme implementado está operando dentro das especificações. Alguns dados coletados nesta etapa se torna a linha de base para o próximo ciclo de planejamento de capacidade.

### <a name="applying-the-process"></a>Aplicando o processo

Para otimizar o desempenho, verifique se esses componentes principais corretamente são selecionadas e ajustados para o aplicativo for carregado:

1. Memória
1. Rede
1. Armazenamento
1. Processador
1. Logon de Rede

Os requisitos de armazenamento básico do AD DS e o comportamento geral do software cliente bem escrito permitem ambientes com até 10.000 para 20.000 usuários a deixarem de investimento pesado em com relação a um hardware físico, como quase qualquer servidor moderno de planejamento de capacidade sistema de classe irá manipular a carga. Dito isso, a tabela a seguir resume como avaliar um ambiente existente para selecionar o hardware correto. Cada componente é analisado em detalhes nas seções subsequentes para ajudar os administradores do AD DS a avaliar sua infraestrutura usando as recomendações de linha de base e entidades de segurança específicas do ambiente.

Em geral:

- Qualquer dimensionamento com base nos dados atuais apenas serão preciso para o ambiente atual.
- Para qualquer estimativas, esperamos por demanda para aumentar o ciclo de vida do hardware.
- Determinar se a ultrapassam hoje e crescer no ambiente de maior ou adicionar capacidade ao longo do ciclo de vida.
- Para virtualização de todas as mesmas entidades de segurança e as metodologias de planejamento de capacidade se aplicam, exceto que a sobrecarga da virtualização precisa ser adicionado a qualquer coisa domínio relacionado.
- Planejamento de capacidade, como qualquer coisa que tenta prever, não é uma ciência precisa. Não esperar que as coisas para calcular perfeitamente e com precisão de 100%. A orientação aqui é a recomendação leanest; adicionar capacidade para fornecer segurança adicional e validar continuamente o que o ambiente permaneça no destino.

### <a name="data-collection-summary-tables"></a>Tabelas de resumo de coleta de dados

#### <a name="new-environment"></a>Novo ambiente

| Componente | Estimativas |
|-|-|
|Tamanho do banco de dados/armazenamento|40 KB a 60 KB para cada usuário|
|RAM|Tamanho do Banco de Dados<br />Recomendações de sistema operacional base<br />Aplicativos de terceiros|
|Rede|1 GB|
|CPU|1\.000 usuários simultâneos para cada núcleo|

#### <a name="high-level-evaluation-criteria"></a>Critérios de avaliação de alto nível

| Componente | Critérios de avaliação | Considerações de planejamento |
|-|-|-|
|Tamanho do banco de dados/armazenamento|A seção intitulada "para ativar o registro em log de espaço em disco que é liberado pela desfragmentação" em [limites de armazenamento](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Armazenamento / desempenho de banco de dados|<ul><li>"Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg disco s/leitura," "disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg disco s/gravação," " Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg de disco s/transferência "</li><li>"Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Reads/sec," "disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Writes/sec," "disco lógico (  *\<Unidade de banco de dados NTDS\>* ) \Transfers/sec "</li></ul>|<ul><li>O armazenamento tem duas preocupações para endereço<ul><li>Espaço disponível, que, com o tamanho do eixo com base e SSD de hoje, com base em armazenamento é irrelevante para a maioria dos ambientes do AD.</li> <li>Operações de entrada/saída (e/s) disponíveis – em muitos ambientes, isso geralmente é esquecido. Mas é importante avaliar apenas a ambientes onde não há RAM suficiente para carregar o banco de dados NTDS inteiro na memória.</li></ul><li>O armazenamento pode ser um tópico complexo e deve envolver a experiência de fornecedores de hardware para o dimensionamento apropriado. Especialmente com cenários mais complexos, como, cenários de iSCSI, SAN e NAS. No entanto, em geral, o custo por Gigabyte de armazenamento é geralmente em oposição direta ao custo por e/s:<ul><li>RAID 5 tem o menor custo por Gigabyte que o Raid 1, mas Raid 1 tem menor custo por e/s</li><li>Unidades de disco rígido com base no eixo tem menor custo por Gigabyte, mas SSDs têm um custo menor por e/s</li></ul><li>Após a reinicialização do computador ou o serviço do Active Directory Domain Services, o cache do mecanismo de armazenamento extensível (ESE) está vazio e o desempenho será vinculado enquanto warms o cache de disco.</li><li>Na maioria dos ambientes AD é intensa e/s em um padrão aleatório para discos, eliminando grande parte do benefício de cache de leitura e leitura estratégias de otimização.  Além disso, o AD possui uma forma maior cache na memória que o sistema de armazenamento a maioria dos caches.</li></ul>
|RAM|<ul><li>Tamanho do banco de dados</li><li>Recomendações de sistema operacional base</li><li>Aplicativos de terceiros</li></ul>|<ul><li>O armazenamento é o componente mais lento em um computador. Quanto mais que pode ser residentes na memória RAM, menos é necessário ir para o disco.</li><li>Certifique-se de RAM suficiente é alocado para armazenar o sistema operacional, agentes (antivírus, backup, monitoramento), banco de dados NTDS e crescimento ao longo do tempo.</li><li>Para ambientes em que maximizar a quantidade de RAM não é custo efetivo (por exemplo, um locais de satélite) ou não é viável (DIT é muito grande), a seção de armazenamento para garantir que o armazenamento é dimensionada adequadamente de referência.</li></ul>|
|Rede|<ul><li>"Interface de rede (\*) \Bytes recebidos/s"</li><li>"Interface de rede (\*) \Bytes enviados/s"|<ul><li>Em geral, o tráfego enviado de um controlador de domínio agora excede o tráfego enviado para um controlador de domínio.</li><li>Como uma conexão de Ethernet comutada é full-duplex, tráfego de rede de entrada e saída precisam ser dimensionadas de forma independente.</li><li>Consolidar o número de controladores de domínio aumentarão a quantidade de largura de banda usada para enviar respostas para solicitações de cliente para cada controlador de domínio, mas serão bastante linear para o site como um todo.</li><li>Se remover o local de satélite controladores de domínio, não se esqueça de adicionar a largura de banda para o controlador de domínio de satélite para o hub de controladores de domínio, bem como usá-lo para avaliar será quanto tráfego de WAN.</li></ul>|
|CPU"Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg de disco s/leitura"descripti"Process(lsass)\\' tempo do processador"tors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>Depois de eliminar o armazenamento como um gargalo, abordar a quantidade de potência de computação necessário.</li><li>Embora não perfeitamente linear, o número de núcleos de processador consumida por todos os servidores em um escopo específico (como um site) pode ser usado para medir a quantos processadores são necessários para suportar a carga total do cliente. Adicione o mínimo necessário para manter o atual nível de serviço em todos os sistemas dentro do escopo.</li><li>Alterações na velocidade do processador, incluindo o gerenciamento de energia relacionados a alterações, derivados do ambiente atual de números de impacto. Em geral, é impossível avaliar com precisão como vai de um processador de 2,5 GHz um processador de 3 GHz reduzirá o número de CPUs necessários.</li></ul>|
|Logon de Rede|<ul><li>"Netlogon (\*) \Semaphore adquire"</li><li>"Netlogon (\*) tempos limite de \Semaphore"</li><li>"Netlogon (\*) \Average semáforo mantenha tempo"</li></ul>|<ul><li>Canal de segurança de Logon do NET/MaxConcurrentAPI afeta somente a ambientes com autenticações NTLM e/ou validação de PAC. Validação de PAC é ativado por padrão nas versões de sistema operacional anterior ao Windows Server 2008. Isso é um configuração do cliente, portanto, os controladores de domínio serão afetados até que ele está desligado em todos os sistemas cliente.</li><li>Ambientes com a autenticação de confiança significativo, o que inclui relações de confiança entre florestas, têm o maior risco se não for dimensionado corretamente.</li><li>Consolidações de servidor aumentará a simultaneidade de relação de confiança entre autenticação.</li><li>As explosões precisam ser levado em conta, como failovers de cluster, conforme os usuários autenticam novamente em massa para o novo nó de cluster.</li><li>Sistemas de clientes individuais (por exemplo, um cluster) talvez seja necessário ajustar muito.</li></ul>|

## <a name="planning"></a>Planejamento

Por um longo tempo, a recomendação da comunidade para dimensionar o AD DS tem sido "colocar em RAM como o tamanho do banco de dados". Na maior parte, que a recomendação é tudo o que a maioria dos ambientes precisava se preocupar. Mas, o ecossistema de consumo de AD DS ficou muito maior, como tem os ambientes do AD DS por conta própria, desde sua apresentação em 1999. Embora o aumento no poder de computação e o comutador do x86 arquiteturas para x64 arquiteturas fez os aspectos mais específico de dimensionamento para tem desempenho irrelevante para um conjunto maior de clientes que executam o AD DS no hardware físico, o crescimento da virtualização reintroduzido as preocupações de ajustes para um público maior do que antes.

As diretrizes a seguir, portanto, são sobre como determinar e planejar as demandas do Active Directory como um serviço independentemente de ele é implantado em um cenário puramente virtualizado, uma mistura de virtuais/físicas ou físico. Como tal, dividiremos a avaliação para cada um dos quatro componentes principais: processador, memória, rede e armazenamento. Em resumo, para maximizar o desempenho no AD DS, o objetivo é obter o mais próximo processador associado possível.

## <a name="ram"></a>RAM

Simplesmente, quanto mais que podem ser armazenados em cache na RAM, menos é necessário ir para o disco. Para maximizar a escalabilidade do servidor a quantidade mínima de RAM deve ser a soma do tamanho atual do banco de dados, o tamanho total do SYSVOL, o sistema operacional recomendada quantidade e as recomendações do fornecedor para os agentes (antivírus, monitoramento, backup e assim por diante ). Uma quantidade adicional deve ser adicionada para acomodar o crescimento durante o tempo de vida do servidor. Isso será ambientalmente subjetivo com base em estimativas de crescimento do banco de dados com base nas alterações ambientais.

Para ambientes em que maximizar a quantidade de RAM não é custo efetivo (por exemplo, um locais de satélite) ou não é viável (DIT é muito grande), a seção de armazenamento para garantir que o armazenamento está corretamente projetada de referência.

Um corolário que aparece no contexto geral na memória de dimensionamento é de dimensionamento do arquivo de paginação. No mesmo contexto de todo o resto memória relacionada, a meta é minimizar vai o muito mais lenta do disco. Portanto, a pergunta deve ir de, "como o arquivo de página sejam dimensionado?" para "A quantidade de RAM é necessário para minimizar a paginação?" A resposta para a última pergunta é descrita no restante desta seção. Isso deixa a maioria da discussão para dimensionar o arquivo de paginação para o realm de recomendações gerais do sistema operacional e a necessidade de configurar o sistema para despejos de memória, que não estão relacionados ao desempenho do AD DS.

### <a name="evaluating"></a>Avaliar o

A quantidade de RAM que precisa de um controlador de domínio (DC) é, na verdade, um exercício complexo por esses motivos:

- Alto potencial de erro ao tentar usar um sistema existente para medir a quantidade de RAM é necessária pois LSASS removerá sob condições de pressão de memória, deflating artificialmente a necessidade.
- O fato subjetivo que um controlador de domínio individual só precisa armazenar em cache o que é "interessante" a seus clientes. Isso significa que os dados que precisam ser armazenados em cache em um controlador de domínio em um site com apenas um servidor do Exchange será muito diferentes dos dados que precisam ser armazenados em cache em um controlador de domínio que autentica apenas os usuários.
- O trabalho a ser avaliada de RAM para cada controlador de domínio em uma base por caso é proibitivo e muda conforme as alterações de ambiente.
- Os critérios por trás de recomendação ajudará a tomar decisões conscientes: 
- Quanto mais que podem ser armazenados em cache na RAM, menos é necessário ir para o disco. 
- De longe, o armazenamento é o componente mais lento de um computador. Acesso a dados com base no eixo e SSD mídia de armazenamento é de 1.000.000 vezes mais lenta do que o acesso aos dados na RAM.

Portanto, para maximizar a escalabilidade do servidor, a quantidade mínima de RAM é a soma do tamanho atual do banco de dados, o tamanho total do SYSVOL, o sistema operacional recomendada quantidade e as recomendações do fornecedor para os agentes (antivírus, monitoramento, backup e e assim por diante). Adicione valores adicionais para acomodar o crescimento durante o tempo de vida do servidor. Isso será ambientalmente subjetivo com base em estimativas de crescimento do banco de dados. No entanto, para locais de satélite com um pequeno conjunto de usuários finais, esses requisitos podem ser reduzidos conforme esses sites não precisarão cache tanto para a maioria das solicitações de serviço.

Para ambientes em que maximizar a quantidade de RAM não é custo efetivo (por exemplo, um locais de satélite) ou não é viável (DIT é muito grande), a seção de armazenamento para garantir que o armazenamento é dimensionada adequadamente de referência.

> [!NOTE]
> Um corolário ao dimensionar a memória é de dimensionamento do arquivo de paginação. Como a meta é minimizar vai o muito mais lenta do disco, a pergunta passa de "como o arquivo de página sejam dimensionado?" para "A quantidade de RAM é necessário para minimizar a paginação?" A resposta para a última pergunta é descrita no restante desta seção. Isso deixa a maioria da discussão para dimensionar o arquivo de paginação para o realm de recomendações gerais do sistema operacional e a necessidade de configurar o sistema para despejos de memória, que não estão relacionados ao desempenho do AD DS.

### <a name="virtualization-considerations-for-ram"></a>Considerações sobre virtualização de RAM

Evite a confirmação de excesso de memória no host. O objetivo fundamental por trás de otimizar a quantidade de RAM é minimizar a quantidade de tempo gasto para o disco. Em cenários de virtualização, o conceito de confirmação de excesso de memória existe onde mais RAM alocado para os convidados, em seguida, existe no computador físico. Isso por si só não é um problema. Ele se torna um problema quando a memória total ativamente usada por todos os convidados excede a quantidade de RAM no host e o host subjacente inicia a paginação. Desempenho fica limitado ao disco em casos onde o controlador de domínio será o NTDS. dit para obter dados, o controlador de domínio será o arquivo de paginação para obter dados ou o host vai para o disco para obter os dados que considera o convidado na RAM.

### <a name="calculation-summary-example"></a>Exemplo de resumo de cálculo

|Componente|Memória estimada (exemplo)|
|-|-|
|Sistema operacional de base recomendada de RAM (Windows Server 2008)|2 GB|
|Tarefas internas de LSASS|200 MB|
|Agente de monitoramento|100 MB|
|Antivírus|100 MB|
|Banco de dados (Catálogo Global)|8,5 GB tem certeza???|
|Amortecedor de backup para ser executado, os administradores façam logon sem impacto|1 GB|
|Total|12 GB|

**Recomendado: 16 GB**

Ao longo do tempo, a suposição pode ser feita que mais dados serão adicionados ao banco de dados e provavelmente será o servidor de produção para 3 a 5 anos. Com base em uma estimativa do crescimento de 33%, 16 GB seria uma quantidade razoável de RAM para colocar em um servidor físico. Em uma máquina virtual, dada a facilidade com que as configurações podem ser modificadas e RAM pode ser adicionado à VM, começando em 12 GB com o plano para monitorar e atualizar no futuro é razoável.

## <a name="network"></a>Rede

### <a name="evaluating"></a>Avaliar o
Esta seção é menor sobre como avaliar as demandas sobre o tráfego de replicação, o que é voltada para o tráfego que passa a WAN e é abordado detalhadamente na [tráfego de replicação do Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), que é sobre como avaliar o total largura de banda e capacidade de rede necessária, incluindo consultas de cliente, aplicativos de diretiva de grupo e assim por diante. Para os ambientes existentes, isso pode ser coletado usando contadores de desempenho "Interface de rede (\*) \Bytes recebidos/s" e "adaptador de rede (\*) \Bytes enviados/s." Intervalos de amostragem para os contadores de Interface de rede em 15, 30 ou 60 minutos. Qualquer coisa menor geralmente será muito volátil para boas medidas; qualquer valor maior será suavizar espia diária excessivamente.

> [!NOTE]
> Em geral, a maioria do tráfego de rede em um controlador de domínio é de saída como o controlador de domínio responde a consultas de cliente. Esse é o motivo para manter o foco no tráfego de saída, embora ele seja recomendado para avaliar cada ambiente para o tráfego de entrada também. As mesmas abordagens podem ser usadas para resolver e examine os requisitos de tráfego de rede de entrada. Para obter mais informações, consulte o artigo de Base de dados de Conhecimento [929851: O intervalo de porta dinâmica para TCP/IP mudou no Windows Vista e no Windows Server 2008](http://support.microsoft.com/kb/929851).

### <a name="bandwidth-needs"></a>Necessidades de largura de banda

Planejamento para escalabilidade de rede abrange duas categorias distintas: a quantidade de tráfego e a CPU de carga do tráfego de rede. Cada um desses cenários é simples em comparação com alguns dos outros tópicos neste artigo.

Avaliar a quantidade de tráfego deve ter suporte, há duas categorias exclusivas de planejamento de capacidade do AD DS em termos de tráfego de rede. A primeira é o tráfego de replicação que atravessa entre controladores de domínio e é abordado detalhadamente na referência [tráfego de replicação do Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) e ainda é relevante para as versões atuais do AD DS. O segundo é o tráfego de cliente-servidor intrassite. Um dos cenários mais simples de planejar, tráfego intra-site predominantemente recebe pequenas solicitações de clientes em relação as grandes quantidades de dados enviadas de volta para os clientes. 100 MB geralmente será adequada em ambientes de até 5.000 usuários por servidor, em um site. Usando um adaptador de rede de 1 GB e o RSS Receive Side Scaling () e o suporte é recomendado para qualquer coisa acima de 5.000 usuários. Para validar esse cenário, especialmente no caso de cenários de consolidação de servidor, examine a Interface de rede (\*) \Bytes/sec entre todos os controladores de domínio em um site, adicioná-los juntos e divida pelo número de controladores de domínio para garantir que há destino é a capacidade adequada. A maneira mais fácil de fazer isso é usar a exibição de "Área empilhada" no Monitor de desempenho (anteriormente conhecido como Perfmon) e a confiabilidade do Windows, certificando-se de que todos os contadores são dimensionados o mesmo.

Considere o exemplo a seguir (também conhecido como uma verdade, realmente complexa maneira de validar que a regra geral é aplicável em um ambiente específico). As seguintes suposições são feitas:

- O objetivo é reduzir a superfície para servidores o menor número possível. O ideal é que um servidor trará a carga e um servidor adicional é implantado para redundância (*N* + 1 cenário). 
- Nesse cenário, o adaptador de rede atual dá suporte a apenas 100 MB e está em um ambiente comutado.  
  A utilização de largura de banda de rede de destino máximo é de 60% em um cenário de N (perda de um controlador de domínio).
- Cada servidor tem cerca de 10.000 clientes conectados a ele.

Conhecimento adquirido a partir dos dados no gráfico (Interface de rede (\*) \Bytes enviados/s):

1. O dia inicia aumentando cerca de 5:30 e ventos para baixo em 7:00 PM.
1. O período mais ocupado do horário de pico é de às 8H até 15 8H, com mais de 25 Bytes enviados/s no controlador de domínio mais ocupado.  
   > [!NOTE]
   > Todos os dados de desempenho são históricos. Portanto, o ponto de dados de pico em 8:15 indica a carga de 8:00 para 8:15.
1. Há picos antes de 4:00 AM, com mais de 20 Bytes enviados/s no controlador de domínio mais ocupado, que pode indicar qualquer carga de fusos horários diferentes ou atividade de infraestrutura, como backups em segundo plano. Uma vez que o pico às 8H excede essa atividade, não é relevante.
1. Há cinco controladores de domínio no site.
1. A carga máxima é de aproximadamente 5.5 MB/s por controlador de domínio, que representa a 44% da conexão de 100 MB. Usando esses dados, ele pode ser estimado que a largura de banda total necessária entre às 8H e 8H: 15 é 28 MB/s.
   >[!NOTE]
   >Tenha cuidado com o fato de que os contadores enviados/recebidos do adaptador de rede estão em bytes e largura de banda de rede é medida em bits. 100 MB &divide; 8 = 12,5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusões:

1. Esse ambiente atual atende o nível de 1 N + de tolerância a falhas em 60% de utilização de destino. Colocar um sistema offline mudará a largura de banda por servidor de cerca de 5,5 MB/s (44%) cerca de 7 MB/s (56%).
1. Com base na meta de consolidação de um servidor mencionada anteriormente, isso excede a utilização máxima de destino e, teoricamente, a utilização de possíveis de uma conexão de 100 MB.
1. Com uma conexão de 1 GB, isso representará 22% da capacidade total.
1. Em normal operacional condições na *N* + 1 cenário, carga de cliente será distribuída igualmente a cerca de 14 MB/s por servidor ou 11% da capacidade total.
1. Para garantir que a capacidade é adequada durante a indisponibilidade de um controlador de domínio, os destinos de operação normais por servidor seria de aproximadamente 30% de rede utilização ou 38 MB/s por servidor. Destinos de failover seria 60% da utilização de rede ou 72 MB/s por servidor.  
  
Em resumo, a implantação final dos sistemas deve ter um adaptador de rede de 1 GB e estar conectada a uma infraestrutura de rede que dão suporte a tal carga. Uma observação adicional é que considerando a quantidade de tráfego de rede gerado, a carga da CPU de comunicações de rede pode ter um impacto significativo e limitar a escalabilidade máxima do AD DS. Esse mesmo processo pode ser usado para estimar a quantidade de comunicação de entrada para o controlador de domínio. Mas, considerando a predominância de tráfego de saída em relação ao tráfego de entrada, é um exercício acadêmico para a maioria dos ambientes. Garantir que o suporte de hardware para o RSS é importante em ambientes com mais de 5.000 usuários por servidor. Para cenários com alto tráfego de rede, o balanceamento de carga de interrupção pode ser um gargalo. Isso pode ser detectado pelo processador (\*)\% tempo de interrupção que está sendo distribuído sem uniformidade entre CPUs. O RSS habilitado NICs pode reduzir essa limitação e aumentar a escalabilidade.

> [!NOTE]
> Uma abordagem semelhante pode ser usada para estimar a capacidade adicional necessária ao consolidar centros de dados, ou desativar um controlador de domínio em um local de satélite. Coletar apenas o tráfego de entrada e saído para os clientes e que será a quantidade de tráfego que agora estará presente nos links WAN.  
>  
> Em alguns casos, você pode experimentar mais tráfego do que o esperado, pois o tráfego é mais lento, como quando a verificação de certificado não atender a contundentes na WAN. Por esse motivo, a utilização e dimensionamento WAN devem ser um processo iterativo e em andamento.

### <a name="virtualization-considerations-for-network-bandwidth"></a>Considerações sobre a virtualização para largura de banda de rede

É fácil fazer recomendações para um servidor físico: 1 GB para servidores que dão suporte a maior que 5.000 usuários. Depois que diversas convidadas comece a compartilhar uma infra-estrutura subjacente do comutador virtual, atenção adicional é necessária para garantir que o host tem largura de banda de rede adequada para dar suporte a todos os convidados no sistema e, portanto, requer o rigor adicional. Isso não é nada mais do que uma extensão de garantir a infraestrutura de rede na máquina host. Isso é independentemente se a rede é inclui o controlador de domínio em execução como um convidado de máquina virtual em um host com o tráfego de rede ultrapassou um comutador virtual ou se conectado diretamente a um comutador físico. O comutador virtual é apenas um componente mais onde o uplink precisa dar suporte a quantidade de dados sendo transmitidos. Vinculado ao comutador do adaptador de rede física do host físico, portanto, deve ser capaz de dar suporte a carga do controlador de domínio além de todos os outros convidados compartilhando o comutador virtual conectado ao adaptador de rede física.

### <a name="calculation-summary-example"></a>Exemplo de resumo de cálculo

|Sistema|Largura de banda de pico|
|-|-|
DC 1|6.5 MB/s|
O CONTROLADOR DE DOMÍNIO 2|6,25 MB/s|
|O CONTROLADOR DE DOMÍNIO 3|6,25 MB/s|
|O CONTROLADOR DE DOMÍNIO 4|5,75 MB/s|
|O CONTROLADOR DE DOMÍNIO 5|4,75 MB/s|
|Total|28,5 MB/s|

**Recomendado: MB/s 72** (28.5 MB/s dividida em 40%)

|Contagem de sistema (s) de destino|Largura de banda total (acima)|
|-|-|
|2|28,5 MB/s|
|Comportamento resultante normal|28,5 &divide; 2 = 14.25 MB/s|

Como sempre, ao longo do tempo a suposição pode ser feita que aumentará a carga do cliente e esse crescimento deve ser planejado como melhores. A quantidade recomendada planejar permitiria que um crescimento estimado em tráfego de rede de 50%.

## <a name="storage"></a>Armazenamento

Planejamento de armazenamento constitui dois componentes:

- Capacidade ou tamanho de armazenamento
- Desempenho

Uma grande quantidade de tempo e a documentação é gasto no planejamento de capacidade, deixando completamente muitas vezes negligenciado do desempenho. Com os custos de hardware atual, a maioria dos ambientes não são grandes o suficiente que qualquer uma delas é realmente uma preocupação e a recomendação para "put na RAM como o tamanho do banco de dados" normalmente abrange o restante, embora possa ser um exagero para locais de satélite no maiores ambientes.

### <a name="sizing"></a>Dimensionamento

#### <a name="evaluating-for-storage"></a>Avaliando para armazenamento

Em comparação com 13 anos atrás quando o Active Directory foi introduzido, uma vez quando as unidades de 4 GB e 9 GB foram os tamanhos de unidade mais comuns, o dimensionamento do Active Directory não é mesmo uma consideração para todos os mas maiores ambientes. Com o menor disco rígido disponível tamanhos no intervalo de 180 GB, a todo o sistema operacional, SYSVOL e o NTDS. dit podem se encaixar facilmente em uma unidade. Como tal, é recomendável substituir grandes investimentos nessa área.

A recomendação única para consideração é garantir que 110% do tamanho do arquivo Ntds. dit está disponível para permitir a desfragmentação. Além disso, para o crescimento durante a vida útil do hardware deve ser adaptado.

A primeira e mais importante consideração é avaliar o quão grande o NTDS. dit e SYSVOL estarão. Essas medidas serão levar a alocação de RAM e disco de tamanho fixo. Devido à () custo relativamente baixo desses componentes, a matemática não precisam ser rigorosas e precisas. Conteúdo sobre como avaliar isso para ambientes novos e existentes podem ser encontradas na [armazenamento de dados](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) série de artigos. Especificamente, consulte os seguintes artigos:

- **Para ambientes existentes &ndash;**  a seção intitulada "para ativar o registro em log de espaço em disco que é liberado pela desfragmentação" no artigo [limites de armazenamento](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **Para novos ambientes &ndash;**  o artigo intitulado [estimativas de crescimento para usuários do Active Directory e unidades organizacionais](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > Os artigos são com base em estimativas de tamanho de dados feitas no momento da versão do Active Directory no Windows 2000. Use os tamanhos de objeto que refletem o tamanho real dos objetos em seu ambiente.

Ao revisar os ambientes existentes com vários domínios, pode haver variações em tamanhos de banco de dados. Em que isso é verdadeiro, use o menor catálogo global (GC) e os tamanhos de não-GC.  

O tamanho do banco de dados pode variar entre versões do sistema operacional. Controladores de domínio que executem sistemas operacionais anteriores, como o Windows Server 2003 tem um tamanho de banco de dados menor do que um controlador de domínio que executa um sistema operacional posterior como o Windows Server 2008 R2, especialmente quando a Lixeira de tal do Active Directory de recursos Bin ou a mobilidade de credenciais estão habilitados.

> [!NOTE]  
  >
>- Para novos ambientes, observe que as estimativas em estimativas de crescimento para usuários do Active Directory e unidades organizacionais indicam que 100.000 usuários (no mesmo domínio) consomem cerca de 450 MB de espaço. Observe que os atributos preenchidos podem ter um grande impacto na quantidade total. Atributos serão preenchidos em muitos objetos por terceiros e produtos da Microsoft, incluindo o Microsoft Exchange Server e do Lync. Uma avaliação com base no portfólio de produtos no ambiente é preferível, mas o exercício de detalhando a matemática e teste para os ambientes maiores, mas estimativas precisas para todas as não pode ser realmente vale a pena o esforço e tempo significativo.
>- Certifique-se de que 110% do arquivo Ntds. dit tamanho está disponível como espaço livre para permitir off-line de desfragmentação e planejar o crescimento ao longo de um tempo de vida do hardware de três a cinco anos. Dado que quão barato armazenamento é, estimar o armazenamento em 300%, o tamanho da DIT como alocação de armazenamento é segura acomodar o crescimento e a necessidade potencial de off-line de desfragmentação.

#### <a name="virtualization-considerations-for-storage"></a>Considerações sobre a virtualização de armazenamento

Em um cenário onde vários arquivos de disco rígido Virtual (VHD) estão sendo alocados em um único volume fixo de usar disco de tamanho de pelo menos % 210 (100% da DIT + 110% de espaço livre) o tamanho da DIT para garantir que haja espaço suficiente reservado.  

#### <a name="calculation-summary-example"></a>Exemplo de resumo de cálculo

|Dados coletados da fase de avaliação| |
|-|-|
|NTDS dit|35 GB|
|Desfragmentação de modificador para permitir off-line|2.1|
|Armazenamento total necessário|EM 73,5 GB|

> [!NOTE]
> Esse armazenamento necessário é além do armazenamento necessário para SYSVOL, sistema operacional, arquivo de paginação, arquivos temporários, dados armazenados em cache locais (como arquivos do instalador) e aplicativos.

### <a name="storage-performance"></a>Desempenho de armazenamento

#### <a name="evaluating-performance-of-storage"></a>Avaliando o desempenho de armazenamento

Como o componente mais lento em qualquer computador, o armazenamento pode ter o maior impacto adverso na experiência do cliente. Para esses ambientes grandes suficiente para o qual as recomendações de dimensionamento de RAM não forem viáveis, as consequências de esquecendo de planejamento de armazenamento para o desempenho podem ser devastadores.  Além disso, as complexidades e variedades de tecnologia de armazenamento adicional aumentarão o risco de falha conforme a relevância de longa data de práticas recomendadas de "put sistema operacional, logs e banco de dados" em discos físicos separados é limitada em cenários úteis de TI.  Isso ocorre porque a prática recomendada de longa data se baseia na suposição de que é que um "disco" é um eixo dedicado e isso permitiu a e/s sejam isolados.  Este suposições que tornam isso true não são mais relevantes com a introdução de:

- Novos tipos de armazenamento e cenários de armazenamento virtualizado e compartilhada
- Compartilhado eixos em uma rede de área de armazenamento (SAN)
- Arquivo VHD em uma SAN ou armazenamento conectado à rede
- Unidades de estado sólido
- Arquiteturas de armazenamento em camadas (ou seja, camada de armazenamento SSD cache maior armazenamento de eixo com base)

Em resumo, o objetivo final de todos os esforços de desempenho de armazenamento, independentemente da arquitetura de armazenamento e design, subjacentes é garantir que a quantidade necessária de operações de entrada/saída por segundo (IOPS) estão disponíveis e que os IOPS acontecer dentro de um período de tempo aceitável . Esta seção aborda como avaliar quais demandas de AD DS do armazenamento subjacente para garantir soluções de armazenamento são projetadas corretamente.  Devido às variações de tecnologias de armazenamento de hoje, é melhor trabalhar com os fornecedores de armazenamento para garantir que o IOPS suficiente.  Para esses cenários com armazenamento anexado localmente, fazer referência a Apêndice C para os conceitos básicos como cenários de armazenamento local tradicional de design.  Este entidades de segurança são geralmente aplicável para as camadas de armazenamento mais complexas e também ajudará na caixa de diálogo com os fornecedores que dão suporte a soluções de armazenamento de back-end.

- Considerando a ampla variedade de opções de armazenamento disponíveis, é recomendável para envolver a experiência de equipes de suporte de hardware ou de fornecedores para garantir que a solução específica atende às necessidades do AD DS. Os números a seguir estão as informações que seriam fornecidas para os especialistas de armazenamento.

Para ambientes em que o banco de dados é muito grande para ser mantido em RAM, use os contadores de desempenho para determinar quanto a e/s precisa ser suportados:

- Disco lógico (\*) \Avg disco s/leitura (por exemplo, se o NTDS. dit é armazenado na unidade d: / unidade, o caminho completo seria \Avg LogicalDisk (d:) de disco s/leitura)
- Disco lógico (\*) \Avg de disco s/gravação
- Disco lógico (\*) \Avg de disco s/transferência
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

Eles devem ser testados de intervalos de 30/15/60 minutos para submeter a benchmark as demandas do ambiente atual.

#### <a name="evaluating-the-results"></a>Avaliar os resultados

> [!NOTE]
> O foco é em Leituras do banco de dados, como isso normalmente é o componente mais exigente, a mesma lógica pode ser aplicada a gravações no arquivo de log, substituindo o disco lógico ( *\<Log NTDS\>* ) \Avg disco s/gravação e o disco lógico ( *\<Log NTDS\>* ) \Writes/sec):
>  
> - Disco lógico ( *\<NTDS\>* ) \Avg disco s/leitura indica se o armazenamento atual está dimensionado adequadamente.  Se os resultados são aproximadamente iguais a hora de acesso do disco para o tipo de disco, disco lógico ( *\<NTDS\>* ) \Reads/sec é uma medida válida.  Verifique as especificações do fabricante para o armazenamento de back-end, mas os intervalos de BOM para o disco lógico ( *\<NTDS\>* ) \Avg disco s/leitura aproximadamente seria:
>   - 7200 – 9 para 12,5 milissegundos (ms)
>   - 10.000 – 6 a 10 ms
>   - 15.000 – 4 a 6 ms  
>   - SSD – 1 a 3 ms  
>   - >[!NOTE]
>     >Recomendações existem informando que o desempenho de armazenamento está degradado em 15 ms a 20 ms (dependendo do código-fonte).  A diferença entre os valores acima e as outras diretrizes é que os valores acima são o intervalo de operação normal.  As outras recomendações estiver solucionando problemas de orientação para identificar quando experiência do cliente, degrada significativamente e fica perceptível.  Referência Apêndice C para obter uma explicação mais profunda.
> - Disco lógico ( *\<NTDS\>* ) \Reads/sec é a quantidade de e/s que está sendo executada.
>   - Se LogicalDisk ( *\<NTDS\>* ) \Avg disco s/leitura está dentro do intervalo ideal para o armazenamento de back-end, disco lógico ( *\<NTDS\>* ) \Reads/ s podem ser usado diretamente para o armazenamento de tamanho.
>   - Se LogicalDisk ( *\<NTDS\>* ) \Avg disco s/leitura não está dentro do intervalo ideal para o armazenamento de back-end, e/s adicional é necessária de acordo com a seguinte fórmula:
>     > (Disco lógico ( *\<NTDS\>* ) \Avg de disco s/leitura) &divide; (tempo de acesso de disco de mídia física) &times; (disco lógico ( *\<NTDS\>* ) \Avg disco s/leitura)

Considerações:

- Observe que, se o servidor está configurado com uma quantidade ideal de RAM, esses valores serão imprecisas para fins de planejamento.  Eles serão erroneamente no lado superior e ainda podem ser usados como um cenário de pior caso.
- Adicionando/otimizando RAM especificamente aumentará uma redução na quantidade de e/s de leitura (disco lógico ( *\<NTDS\>* ) \Reads/Sec.  Isso significa que a solução de armazenamento não pode ter que ser tão eficiente quanto o inicialmente calculado.  Infelizmente, nada mais específico que essa instrução geral é ecologicamente dependente da carga do cliente e diretrizes gerais não pode ser fornecido.  É a melhor opção Ajustar o dimensionamento do armazenamento após a otimização de memória RAM.

#### <a name="virtualization-considerations-for-performance"></a>Considerações sobre a virtualização para desempenho

Assim como todas as discussões anteriores de virtualização, a chave aqui é garantir que a infra-estrutura compartilhada subjacente pode dar suporte a carga do controlador de domínio além de outros recursos usando subjacente compartilhado mídia e todos os caminhos a ele. Isso é verdadeiro se um controlador de domínio físico está compartilhando a mesma mídia subjacente em uma infra-estrutura de iSCSI, SAN ou NAS como outros servidores ou aplicativos, seja ele um convidado usando a passagem por meio do acesso para uma infraestrutura de SAN, NAS ou iSCSI que compartilha o subjacente a mídia, ou se o convidado está usando um arquivo VHD que reside em mídia compartilhada localmente ou em uma infraestrutura de SAN, NAS ou iSCSI. O exercício de planejamento é tudo sobre certificando-se de que a mídia subjacente pode dar suporte a carga total de todos os consumidores.

Além disso, de uma perspectiva de convidado, como há caminhos de código adicional que devem ser percorridos, há um impacto no desempenho ao ter que passar por um host para qualquer armazenamento de acesso. Não surpreende que o teste de desempenho de armazenamento indica que a virtualização tem um impacto na taxa de transferência é subjetiva para a utilização do processador do sistema do host (consulte o Apêndice a: Critério de dimensionamento da CPU), que obviamente é influenciado pelos recursos do host exigida pelo convidado. Isso contribui para as considerações de virtualização sobre necessidades de processamento em um cenário virtualizado (consulte [considerações de virtualização para o processamento](#virtualization-considerations-for-processing)).

Tornando isso mais complexo é o que há uma variedade de opções de armazenamento diferentes que estão disponíveis para que todos possuem diferentes impactos no desempenho. Como uma estimativa segura durante a migração do físico para virtual, use um multiplicador de 1,10 para ajustar para opções de armazenamento diferentes para convidados virtualizados no Hyper-V, como o armazenamento de passagem, adaptador SCSI ou IDE. Os ajustes que precisam ser feitas durante a transferência entre os cenários de armazenamento diferentes são irrelevantes sobre se o armazenamento é local, SAN, NAS ou iSCSI.

#### <a name="calculation-summary-example"></a>Exemplo de resumo de cálculo

Determinando a quantidade de e/s necessária para um sistema íntegro sob condições normais de operação:

- Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Transfers/sec durante o período de pico 15 período minuto 
- Para determinar a quantidade de e/s necessária para o armazenamento em que a capacidade de armazenamento subjacente for excedida:
  >*Necessário de IOPS* = (disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg de disco s/leitura &divide; *\<média de disco s/leitura de destino\>* ) &times; Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Read/sec

|Contador|Valor|
|-|-|
|LogicalDisk real ( *\<unidade de banco de dados NTDS\>* ) \Avg de disco s/transferência|0,02 segundos (20 milissegundos)|
|Disco lógico de destino ( *\<unidade de banco de dados NTDS\>* ) \Avg de disco s/transferência|.01 segundos|
|Multiplicador para a alteração na e/s disponível|0.02 &divide; 0.01 = 2|  
  
|Nome do valor|Valor|
|-|-|
|Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Transfers/sec|400|
|Multiplicador para a alteração na e/s disponível|2|
|Total de IOPS necessários durante o período de pico|800|

Para determinar a taxa em que o cache for desejado para ser prontas:

- Determine o tempo máximo aceitável para quente o cache. Ele é a quantidade de tempo que deve levar para carregar o banco de dados inteiro do disco ou para cenários em que o banco de dados inteiro não pode ser carregado na RAM, isso seria o tempo máximo para preencher a RAM.
- Determine o tamanho do banco de dados, excluindo espaços em branco.  Para obter mais informações, consulte [avaliar para armazenamento](#evaluating-for-storage).  
- Dividir o tamanho do banco de dados por 8 KB; Esse será o total de IOs necessário para carregar o banco de dados.
- Divida os IOs total pelo número de segundos no período de tempo definido.

Observe que a taxa calculada, enquanto precisos, não ser exata porque páginas carregadas anteriormente são removidas se o ESE não está configurado para ter um tamanho de cache fixo e tamanho de cache variável usa o AD DS por padrão.

|Pontos de dados para coletar|Valores
|-|-|
|Tempo máximo aceitável para a quente|10 minutos (600 segundos)
|Tamanho do banco de dados|2 GB|  
  
|Etapa de cálculo|Fórmula|Resultado|
|-|-|-|
|Calcule o tamanho do banco de dados em páginas|(2 GB &times; 1024 &times; 1024) = *tamanho do banco de dados em KB*|2\.097.152 KB|
|Calcular o número de páginas no banco de dados|KB 2.097.152 &divide; 8 KB = *número de páginas*|262.144 páginas|
|Calcular o IOPS necessário morna totalmente o cache|262.144 páginas &divide; 600 segundos = *IOPS necessários*|437 IOPS|

## <a name="processing"></a>Processando

### <a name="evaluating-active-directory-processor-usage"></a>Avaliar o uso do processador do Active Directory

Para a maioria dos ambientes, depois que a rede, RAM e armazenamento são ajustados corretamente conforme descrito na seção de planejamento, gerenciar a quantidade de capacidade de processamento será o componente que merece mais atenção. Há dois desafios para avaliar a capacidade de CPU necessária:

- Se ou não os aplicativos no ambiente estão sendo bem comportados em uma infraestrutura de serviços compartilhados e é abordado na seção intitulada "Controle cara e ineficiente pesquisas" no artigo criar mais eficiente Microsoft Active Directory Aplicativos habilitados por diretório ou migrar do SAM de nível inferior chamadas para chamadas de LDAP.  
  
  Em ambientes maiores, o motivo pelo qual que isso é importante é que aplicativos codificados mal podem unidade volatilidade na carga de CPU, "roubar" uma quantidade excessiva de tempo de CPU de outros aplicativos, artificialmente aumentar as necessidades de capacidade e sem uniformidade distribuir a carga em relação a os controladores de domínio.  
- Como o AD DS é um ambiente distribuído com uma grande variedade de clientes potenciais, estimar o custo de um "cliente único" é subjetiva ambientalmente devido a padrões de uso e o tipo ou a quantidade de aplicativos que usam o AD DS. Em resumo, assim como a seção de rede, aplicabilidade amplo, isso é melhor abordou da perspectiva de avaliar a capacidade total necessária no ambiente.

Para os ambientes existentes, como o dimensionamento de armazenamento foi discutido anteriormente, a suposição é feita que armazenamento agora está dimensionado adequadamente e, portanto, os dados em relação à carga do processador são válidos. Para reiterar, é essencial para garantir que o gargalo no sistema não é o desempenho do armazenamento. Quando existe um gargalo e o processador está esperando, há estados ociosos desaparecerão depois que o gargalo é removido.  Como os estados de espera de processador são removidos, por definição, a utilização da CPU aumenta à medida que ele não tem mais de espera nos dados. Assim, coletar contadores de desempenho "disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg disco s/leitura" e "Process(lsass)\\% Processor Time". Os dados em "Process(lsass)\\% Processor Time" será artificialmente baixa se "disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg disco s/leitura" excede 10 a 15 ms, que é um limite geral que O suporte da Microsoft usa para solução de problemas de desempenho relacionados ao armazenamento. Como antes, é recomendável que os intervalos de amostra ser 15, 30 ou 60 minutos. Qualquer coisa menor geralmente será muito volátil para boas medidas; qualquer valor maior será suavizar espia diária excessivamente.

### <a name="introduction"></a>Introdução

Para planejar o planejamento de capacidade para controladores de domínio, a capacidade de processamento requer mais atenção e Noções básicas sobre. Quando o dimensionamento de sistemas para garantir o máximo desempenho, sempre há um componente que é o gargalo e um controlador de domínio corretamente dimensionada, esse será o processador.

Semelhante à seção de rede onde a demanda do ambiente é examinada em uma base de site a site, o mesmo deve ser feito para a capacidade de computação exigida. Ao contrário a seção de rede, onde as tecnologias de rede disponíveis excediam muito a demanda normal, mais atenção à capacidade de CPU de dimensionamento.  Como todos os ambientes de tamanho moderado mesmo; algo sobre alguns milhares de usuários simultâneos pode colocar uma carga significativa na CPU.

Infelizmente, devido à grande variabilidade de aplicativos cliente que aproveite o AD, uma estimativa geral de usuários por CPU está lamentavelmente inaplicável a todos os ambientes. Especificamente, as demandas de computação estão sujeitos a perfil de aplicativo e o comportamento do usuário. Portanto, cada ambiente precisa ser dimensionado individualmente.

#### <a name="target-site-behavior-profile"></a>Perfil de comportamento do site de destino

Como mencionado anteriormente, ao planejar a capacidade para um site inteiro, o objetivo é direcionar um design com uma *N* + design de capacidade de 1, o, de modo que a falha de um sistema durante o período de pico permitirá a continuação do serviço em uma razoável nível de qualidade. Isso significa que em um "*N*" cenário, a carga em todas as caixas de deve ser inferior a 100% (melhor ainda, menos de 80%) durante os períodos de pico.

Além disso, se os aplicativos e clientes no site usando as práticas recomendadas para a localização de controladores de domínio (ou seja, usando o [DsGetDcName função](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), os clientes devem ser igualmente distribuídos com minor picos transitórios devido a qualquer número de fatores.

No exemplo a seguir, as seguintes suposições são feitas:

- Cada um dos cinco DCs no site tem quatro CPUs.
- Uso da CPU durante o horário comercial de destino total é 40% em condições normais de operação ("*N* + 1") e 60% caso contrário ("*N*"). Durante o horário comercial, o uso de CPU de destino é 80% porque o software de backup e outros tipos de manutenção devem consumir todos os recursos disponíveis.

![Gráfico de uso de CPU](media/capacity-planning-considerations-cpu-chart.png)

Analisar os dados no gráfico (processador Information(_Total)\% utilitário processador) para cada um dos controladores de domínio:

- Na maior parte, a carga é igualmente distribuída que é o que seria esperado quando os clientes usam o localizador do controlador de domínio e também tem escrito pesquisas. 
- Há um número de cinco minutos picos de 10%, com algumas como grandes como 20%. Em geral, a menos que eles fazem com que o destino do plano de capacidade de ser excedido, investigar esses não é interessante.  
- O período de pico para todos os sistemas é entre aproximadamente às 8H e 15 9H. Com a transição suave de aproximadamente 5:00 AM por meio de aproximadamente às 17H, isso é geralmente uma indicação do ciclo de negócios. Os mais aleatórios picos de uso da CPU em um cenário de caixa por caixa entre às 17H e 4:00 AM seria fora as questões de planejamento de capacidade.

  >[!NOTE]
  >Em um sistema bem gerenciado, picos ditas são pode ser em execução do software de backup, verificações de antivírus completa do sistema, inventário de hardware ou software, implantação de software ou patches e assim por diante. Porque eles estão fora o ciclo de negócios do usuário de pico, os destinos não são excedidos.

- Como cada sistema é cerca de 40%, e todos os sistemas têm os mesmos números de CPUs, em caso de um falhar ou ficar offline, os sistemas restantes seriam executados em um 53% estimado (carga de 40% do sistema D é uniformemente dividir e adicionada ao sistema A e do sistema C % a 40% de carga existentes). Por uma série de motivos, essa suposição linear não é perfeitamente precisa, mas fornece a precisão suficiente para medir.  

  **Cenário alternativo –** dois controladores de domínio em execução em 40%: Um domínio controlador falhar, estimado de CPU em um restante seria uma cerca de 80%. Agora isso excede os limites descritos acima para planejamento de capacidade e também começa a gravemente limitar a quantidade de espaço principal para o 10 a 20% visto no perfil de carga acima, que significa que os picos controlariam o controlador de domínio para 90% a 100% durante o "*N*"cenário e definitivamente prejudicar a capacidade de resposta.

### <a name="calculating-cpu-demands"></a>Calculando as demandas de CPU

O "processo\\% Processor Time" contadores de desempenho do objeto soma a quantidade total de tempo que gastam por todos os threads de um aplicativo na CPU e divide pelo valor total do sistema de tempo que passou. O efeito disso é que um aplicativo multi-threaded em um sistema com várias CPUs pode exceder 100% de tempo de CPU e poderia ser interpretado de forma muito diferente de "informações do processador\\% utilitário do processador". Na prática a "Process(lsass)\\% Processor Time" pode ser visto como a contagem de CPUs em execução em 100% que são necessários para dar suporte a demandas do processo. Um valor de 200% significa que 2 CPUs, cada uma a 100%, são necessários para suportar a carga total do AD DS. Embora seja uma CPU em execução com capacidade de 100% mais econômico da perspectiva de dinheiro gasto em CPUs e consumo de energia, por uma série de motivos detalhadas no Apêndice A, melhor capacidade de resposta em um sistema com multithread ocorre quando o sistema é não em execução em 100%.

Para acomodar picos transitórios na carga do cliente, é recomendável uma CPU de período de pico de entre 40% e 60% da capacidade do sistema de destino. Trabalhando com o exemplo acima, isso significaria que entre 3,33 (60% de destino) e 5 (40% de destino) CPUs seriam necessários para a carga de (processo lsass) do AD DS. Capacidade adicional deve ser adicionada no acordo com as exigências de sistema operacional de base e outros agentes necessários (como antivírus, backup, monitoramento e assim por diante). Embora o impacto dos agentes precisa ser avaliado em uma base por ambiente, uma estimativa de entre 5% e 10% de uma única CPU pode ser feita. No exemplo atual, isso seria sugerem que entre 3.43 (60% de destino) e 5.1 (40% de destino) CPUs são necessárias durante períodos de pico.

A maneira mais fácil de fazer isso é usar a exibição de "Área empilhada" no Monitor de desempenho (perfmon) e confiabilidade do Windows, certificando-se de que todos os contadores são dimensionados o mesmo.

Suposições:

- Meta é reduzir o volume para servidores o menor número possível. O ideal é que um servidor levaria com a carga e um servidor adicional adicionado para redundância (*N* + 1 cenário).

![Gráfico de tempo do processador para o processo lsass (ao longo de todos os processadores)](media/capacity-planning-considerations-proc-time-chart.png)

Conhecimento adquirido a partir dos dados no gráfico (Process(lsass)\\% tempo do processador):

- O dia inicia aumentando cerca de 7:00 e diminui em às 17H.
- O período mais ocupado do horário de pico é de 9H30 a 11 horas. 
  > [!NOTE]
  > Todos os dados de desempenho são históricos. O ponto de dados de pico no 9:15 indica a carga de 9:00:15 de 9.
- Há picos antes 19h, que pode indicar a carga de fusos horários diferentes ou atividade de infraestrutura em segundo plano, como backups. Porque o pico em 9H30 excede essa atividade, não é relevante.
- Há três controladores de domínio no site.

Em carga máxima, o lsass consome cerca de 485% de uma CPU ou 4.85 CPUs em execução em 100%. De acordo com a matemática anteriormente, isso significa que o site precisa de cerca de 12,25 CPUs para AD DS. Adicionar as sugestões acima de 5 a 10% para processos em segundo plano, e isso significa que substituir o servidor hoje precisaria de aproximadamente 12.30 para 12,35 CPUs para dar suporte a mesma carga. Uma estimativa ambiental para o crescimento agora precisa ser levado em conta.

### <a name="when-to-tune-ldap-weights"></a>Quando ajustar os pesos LDAP

Há vários cenários em que o ajuste [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) devem ser consideradas. Dentro do contexto do planejamento de capacidade, isso seria feito quando as cargas de usuário ou aplicativo não são balanceadas igualmente ou sistemas subjacentes não são balanceados igualmente em termos de recursos. Motivos para fazer isso, além do planejamento de capacidade estão fora do escopo deste artigo.

Há duas razões comuns para ajustar os pesos de LDAP:

- O emulador PDC é um exemplo que afeta todos os ambientes para qual usuário ou aplicativo comportamento de carregamento não é distribuído uniformemente. Como determinadas ferramentas e ações direcionar o emulador PDC, como a ferramentas de gerenciamento de diretiva de grupo, a segunda tentativas no caso de falhas de autenticação, estabelecimento de confiança e assim por diante, recursos de CPU no emulador de PDC podem ser mais intensamente requeridos do que em outros lugares no o site.
  - Isso é útil somente ajustar isso, se houver uma diferença perceptível na utilização da CPU para reduzir a carga no emulador PDC e aumentar a carga nos outros controladores de domínio permitirá uma distribuição mais uniforme de carga.
  - Nesse caso, defina LDAPSrvWeight entre 50 e 75 para o emulador PDC.
- Servidores com diferentes contagens de CPUs (e velocidade) em um site.  Por exemplo, digamos que há dois servidores de oito núcleos e um servidor de quatro núcleos.  O último servidor tem metade os processadores de dois servidores.  Isso significa que uma carga de cliente distribuída também aumentará a carga média da CPU na caixa de quatro núcleos para aproximadamente duas vezes em que as caixas de oito núcleos.
  - Por exemplo, as duas caixas de oito núcleos estaria em execução em 40% e a caixa de quatro núcleos estaria em execução em 80%.
  - Além disso, considere o impacto da perda de uma caixa de oito núcleos neste cenário, especificamente o fato de que a caixa de quatro núcleos deve agora ser sobrecarregada.

#### <a name="example-1---pdc"></a>Exemplo 1 - PDC

| |Utilização com padrões|Novo LdapSrvWeight|Nova utilização estimada|
|-|-|-|-|
|1 controlador de domínio (emulador PDC)|53%|57|40%|
|O CONTROLADOR DE DOMÍNIO 2|33%|100|40%|
|O CONTROLADOR DE DOMÍNIO 3|33%|100|40%|

O problema aqui é que se a função de emulador PDC é transferida ou tomada de controle, particularmente para outro controlador de domínio no site, haverá um aumento significativo no novo emulador PDC.

Usando o exemplo da seção [perfil de comportamento do site de destino](#target-site-behavior-profile), foi feita uma suposição todos os três controladores de domínio no site tinham quatro CPUs. O que deve acontecer, em condições normais, se um dos controladores de domínio tiver oito CPUs? Deve haver dois controladores de domínio a 40% de utilização e uma a 20% de utilização. Embora isso não é ruim, há uma oportunidade para balancear a carga de um pouco melhor. Aproveite os pesos LDAP para fazer isso.  Um cenário de exemplo seria:

#### <a name="example-2---differing-cpu-counts"></a>Contagens de CPU diferentes do exemplo 2:

| |Informações do processador\\ %&nbsp;Utility(_Total) do processador<br />Utilização com padrões|Novo LdapSrvWeight|Nova utilização estimada|
|-|-|-|-|
|4-CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

Tenha muito cuidado com esses cenários entanto. Como pode ser visto acima, a matemática se parece muito bom e bonito em papel. Mas ao longo deste artigo, planejamento de uma "*N* + 1" cenário é de suma importância. O impacto de um controlador de domínio ficar offline deve ser calculada para cada cenário. No cenário imediatamente anterior em que a distribuição de carga for par, para garantir uma 60% de carga durante um "*N*" cenário, com a carga balanceada uniformemente entre todos os servidores, a distribuição esteja correto conforme as taxas de permanecem consistente. Procurando no emulador do PDC cenário de ajuste e, em geral qualquer cenário em que a carga de usuário ou aplicativo é desbalanceada, o efeito é muito diferente:

| |Utilização ajustada|Novo LdapSrvWeight|Nova utilização estimada|
|-|-|-|-|
|1 controlador de domínio (emulador PDC)|40%|85|47%|
|O CONTROLADOR DE DOMÍNIO 2|40%|100|53%|
|O CONTROLADOR DE DOMÍNIO 3|40%|100|53%|

### <a name="virtualization-considerations-for-processing"></a>Considerações de virtualização para o processamento

Há duas camadas do planejamento de capacidade que precisam ser feitas em um ambiente virtualizado. No nível do host, semelhante à identificação de ciclo de negócios descrita para o controlador de domínio processamento anteriormente, os limites durante o período de pico devem ser identificados. Como as entidades de segurança subjacentes são o mesmo para um computador host agendamento de threads de convidado na CPU para a introdução de threads do AD DS na CPU em um computador físico, a mesma meta de 40 a 60% no host subjacente são recomendados. Na próxima camada, a camada de convidado, desde as entidades de agendamento de thread não foram alterados, a meta dentro do convidado permanece no intervalo de 40 a 60%.

Em um cenário direto mapeado, um convidado por host, todo o planejamento de capacidade feito até o momento precisa ser adicionada aos requisitos (RAM, disco, rede) do sistema operacional do host subjacente. Em um cenário de host compartilhado, indicam que não há impacto de 10% na eficiência dos processadores subjacentes. Isso significa que se um site precisa de 10 CPUs em um destino de 40%, a quantidade recomendada de CPUs virtuais para alocar em todos os "*N*" convidados seria 11. Em um site com uma distribuição misto de servidores virtuais e servidores físicos, o modificador só se aplica às VMs. Por exemplo, se um site tem um "*N* + 1" cenário, um servidor físico ou mapeado em direct com 10 CPUs seria sobre equivalente a um convidado com 11 CPUs em um host, com 11 CPUs reservados para o controlador de domínio.

Em toda a análise e o cálculo das quantidades de CPU necessários para dar suporte à carga do AD DS, os números de CPUs que são mapeados para o que pode ser comprado em hardware físico de termos não necessariamente mapeiam corretamente. Virtualização elimina a necessidade de arredondar para cima. A virtualização reduz o esforço necessário para adicionar capacidade de computação a um site, dada a facilidade com que uma CPU pode ser adicionada a uma VM. Ele elimina a necessidade para avaliar com precisão a capacidade de computação necessária para que o hardware subjacente está disponível quando precisam de mais CPUs a ser adicionado para os convidados.  Como sempre, lembre-se de planejar e monitorar para aumento na demanda.

### <a name="calculation-summary-example"></a>Exemplo de resumo de cálculo

|Sistema|CPU de pico|
|-|-|-|
|DC 1|120%|
|O CONTROLADOR DE DOMÍNIO 2|147%|
|Dc 3|218%|
|Total de CPU que está sendo usado|485%|  
  
|Contagem de sistema (s) de destino|Largura de banda total (acima)|
|-|-|
|CPUs necessários no destino de 40%|4.85 &divide; .4 = 12.25|

Devido a importância desse ponto de repetição *Lembre-se de planejar o crescimento*. Supondo que o crescimento de 50% nos próximos três anos, esse ambiente precisará 18.375 CPUs (12,25 &times; 1.5) na marca de três anos. Um plano alternativo seria examinar após o primeiro ano e adicione em capacidade adicional conforme necessário.

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>Carga de autenticação de cliente de confiança entre para NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>Avaliar a carga de autenticação de cliente de confiança entre

Muitos ambientes podem ter um ou mais domínios conectados por uma relação de confiança. Uma solicitação de autenticação para uma identidade em outro domínio que não usa a autenticação Kerberos precisa atravessar uma relação de confiança usando o canal de segurança do controlador de domínio para outro controlador de domínio no domínio de destino ou o próximo domínio no caminho para o domínio de destino. O número de chamadas simultâneas usando o canal seguro de que um controlador de domínio pode fazer em um controlador de domínio em um domínio confiável é controlado por uma configuração conhecida como **MaxConcurrentAPI**. Para controladores de domínio, garantir que o canal seguro pode lidar com a quantidade de carga é feito por uma das duas abordagens: ajuste **MaxConcurrentAPI** ou dentro de uma floresta, criar relações de confiança de atalho. Para medir o volume de tráfego em uma relação de confiança individual, consulte [como fazer o ajuste de desempenho para a autenticação NTLM, usando a configuração MaxConcurrentApi](https://support.microsoft.com/kb/2688798).

Durante a coleta de dados, isso, assim como acontece com todos os outros cenários, deve ser coletado durante os períodos de pico ocupado do dia para os dados para serem úteis.

> [!NOTE]
> Cenários entre florestas e intrafloresta podem fazer com que a autenticação percorrer várias relações de confiança e cada estágio precisaria ser ajustado.

#### <a name="planning"></a>Planejamento

Há um número de aplicativos que usam a autenticação NTLM por padrão, ou usá-lo em um determinado cenário de configuração. Servidores de aplicativos crescem em capacidade e aumentar o número de clientes ativos do serviço. Também há uma tendência de que os clientes Manter sessões abertas por tempo limitado e reconectar-se em vez disso, de forma regular (como sincronização de recepção de email). Outro exemplo comum para alta carga NTLM é servidores proxy da web que exigem a autenticação para acesso à Internet.

Esses aplicativos podem causar uma carga significativa para a autenticação NTLM, que pode colocar tensão significativa nos controladores de domínio, especialmente quando os usuários e recursos estão em domínios diferentes.

Há várias abordagens para a carga de relação de confiança entre gerenciamento, que, na prática, são usadas em conjunto em vez de em um recurso exclusivo ou / ou cenário. As opções possíveis são:

- Reduza a autenticação de cliente de confiança entre Localizando os serviços que um usuário consome no mesmo domínio que o usuário seja residente no.
- Aumente o número de seguro canais disponíveis. Isso é relevante para o tráfego de entre florestas e intrafloresta e são conhecidas como relações de confiança de atalho.
- Ajustar as configurações padrão para **MaxConcurrentAPI**.

Para ajustar **MaxConcurrentAPI** em um servidor existente, a equação é:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time transferências*) &times; *average_semaphore_ hold_time* &divide; *time_collection_length*

Para obter mais informações, consulte [2688798 artigo da KB: Como fazer o ajuste de desempenho para a autenticação NTLM, usando a configuração MaxConcurrentApi](http://support.microsoft.com/kb/2688798).

## <a name="virtualization-considerations"></a>Considerações sobre virtualização

Nenhum, isso é um sistema operacional configuração de ajuste.

### <a name="calculation-summary-example"></a>Exemplo de resumo de cálculo

|Tipo de dados|Valor|
|-|-|
|Adquire de semáforo (mínimo)|6,161|
|Adquire de semáforo (máximo)|6,762|
|Tempos limite de semáforo|0|
|Tempo de espera médio de semáforo|0.012|
|Duração da coleta (segundos)|1:11 minutos (71 segundos)|
|Fórmula (de KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Valor mínimo para **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

Para esse sistema para esse período de tempo, os valores padrão são aceitáveis.

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>Monitoramento de conformidade com as metas de planejamento de capacidade

Ao longo deste artigo, ele foi discutido planejamento e dimensionamento ir para destinos de utilização. Aqui está um gráfico de resumo dos limites recomendados que devem ser monitorados para garantir que os sistemas estiverem operando dentro dos limites de capacidade adequada. Tenha em mente que esses não são os limites de desempenho, mas os limites de planejamento de capacidade. Um servidor operacional que excede esses limites funcionará, mas é o momento de iniciar a validação de todos os aplicativos também são se comportavam. Se disse que os aplicativos são bem comportados, é hora de começar a avaliar as atualizações de hardware ou outras alterações de configuração.

|Category|Contador de desempenho|Intervalo/amostragem|Destino|Aviso|
|-|-|-|-|-|
|Processador|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 or earlier)|Memory\Available MB|< 100 MB|N/A|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long-Term Average Standby Cache Lifetime(s)|30 min|Must be tested|Must be tested|
|Network|Network Interface(\*)\Bytes Sent/sec<br /><br />Network Interface(\*)\Bytes Received/sec|30 min|40%|60%|
|Storage|LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read<br /><br />LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write|60 min|10 ms|15 ms|
|AD Services|Netlogon(\*)\Average Semaphore Hold Time|60 min|0|1 second|

## Appendix A: CPU sizing criteria

### Definitions

**Processor (microprocessor) –** a component that reads and executes program instructions  

**CPU –** Central Processing Unit  

**Multi-Core processor –** multiple CPUs on the same integrated circuit  

**Multi-CPU –** multiple CPUs, not on the same integrated circuit  

**Logical Processor –** one logical computing engine from the perspective of the operating system  

This includes hyper-threaded, one core on multi-core processor, or a single core processor.  

As today’s server systems have multiple processors, multiple multi-core processors, and hyper-threading, this information is generalized to cover both scenarios. As such, the term logical processor will be used as it represents the operating system and application perspective of the available computing engines.

### Thread-level parallelism

Each thread is an independent task, as each thread has its own stack and instructions. Because AD DS is multi-threaded and the number of available threads can be tuned by using [How to view and set LDAP policy in Active Directory by using Ntdsutil.exe](http://support.microsoft.com/kb/315071), it scales well across multiple logical processors.

### Data-level parallelism

This involves sharing data across multiple threads within one process (in the case of the AD DS process alone) and across multiple threads in multiple processes (in general). With concern to over-simplifying the case, this means that any changes to data are reflected to all running threads in all the various levels of cache (L1, L2, L3) across all cores running said threads as well as updating shared memory. Performance can degrade during write operations while all the various memory locations are brought consistent before instruction processing can continue.

### CPU speed vs. multiple-core considerations

The general rule of thumb is faster logical processors reduce the duration it takes to process a series of instructions, while more logical processors means that more tasks can be run at the same time. These rules of thumb break down as the scenarios become inherently more complex with considerations of fetching data from shared-memory, waiting on data-level parallelism, and the overhead of managing multiple threads. This is also why scalability in multi-core systems is not linear.

Consider the following analogies in these considerations: think of a highway, with each thread being an individual car, each lane being a core, and the speed limit being the clock speed.

1. If there is only one car on the highway, it doesn’t matter if there are two lanes or 12 lanes. That car is only going to go as fast as the speed limit will allow.
1. Assume that the data the thread needs is not immediately available. The analogy would be that a segment of road is shutdown. If there is only one car on the highway, it doesn’t matter what the speed limit is until the lane is reopened (data is fetched from memory).
1. As the number of cars increase, the overhead to manage the number of cars increases. Compare the experience of driving and the amount of attention necessary when the road is practically empty (such as late evening) versus when the traffic is heavy (such as mid-afternoon, but not rush hour). Also, consider the amount of attention necessary when driving on a two-lane highway, where there is only one other lane to worry about what the drivers are doing, versus a six-lane highway where one has to worry about what a lot of other drivers are doing.
   > [!NOTE]
   > The analogy about the rush hour scenario is extended in the next section: Response Time/How the System Busyness Impacts Performance.

As a result, specifics about more or faster processors become highly subjective to application behavior, which in the case of AD DS is very environmentally specific and even varies from server to server within an environment. This is why the references earlier in the article do not invest heavily in being overly precise, and a margin of safety is included in the calculations. When making budget-driven purchasing decisions, it is recommended that optimizing usage of the processors at 40% (or the desired number for the environment) occurs first, before considering buying faster processors. The increased synchronization across more processors reduces the true benefit of more processors from the linear progression (2&times; the number of processors provides less than 2&times; available additional compute power).

> [!NOTE]
> Amdahl’s Law and Gustafson’s Law are the relevant concepts here.

### Response time/How the system busyness impacts performance

Queuing theory is the mathematical study of waiting lines (queues). In queuing theory, the Utilization Law is represented by the equation:

*U* k = *B* &divide; *T*

Where *U* k is the utilization percentage, *B* is the amount of time busy, and *T* is the total time the system was observed. Translated into the context of Windows, this means the number of 100-nanosecond (ns) interval threads that are in a Running state divided by how many 100-ns intervals were available in given time interval. This is exactly the formula for calculating % Processor Utility (reference [Processor Object](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) and [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Queuing theory also provides the formula: *N* = *U* k &divide; (1 &ndash; *U* k) to estimate the number of waiting items based on utilization ( *N* is the length of the queue). Charting this over all utilization intervals provides the following estimates to how long the queue to get on the processor is at any given CPU load.

![Queue length](media/capacity-planning-considerations-queue-length.png)

It is observed that after 50% CPU load, on average there is always a wait of one other item in the queue, with a noticeably rapid increase after about 70% CPU utilization.

Returning to the driving analogy used earlier in this section:

- The busy times of “mid-afternoon” would, hypothetically, fall somewhere into the 40% to 70% range. There is enough traffic such that one’s ability to pick any lane is not majorly restricted, and the chance of another driver being in the way, while high, does not require the level of effort to “find” a safe gap between other cars on the road.
- One will notice that as traffic approaches rush hour, the road system approaches 100% capacity. Changing lanes can become very challenging because cars are so close together that increased caution must be exercised to do so.

This is why the long term averages for capacity conservatively estimated at 40% allows for head room for abnormal spikes in load, whether said spikes transitory (such as poorly coded queries that run for a few minutes) or abnormal bursts in general load (the morning of the first day after a long weekend).

The above statement regards % Processor Time calculation being the same as the Utilization Law is a bit of a simplification for the ease of the general reader. For those more mathematically rigorous:  
- Translating the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = The number of 100-ns intervals “Idle” thread spends on the logical processor. The change in the “*X*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation
  - *T* = the total number of 100-ns intervals in a given time range. The change in the “*Y*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation.
  - *U* k = The utilization percentage of the logical processor by the “Idle Thread” or % Idle Time.  
- Working out the math:
  - *U* k = 1 – %Processor Time
  - %Processor Time = 1 – *U* k
  - %Processor Time = 1 – *B* / *T*
  - %Processor Time = 1 – *X1* – *X0* / *Y1* – *Y0*

### Applying the concepts to capacity planning

The preceding math may make determinations about the number of logical processors needed in a system seem overwhelmingly complex. This is why the approach to sizing the systems is focused on determining maximum target utilization based on current load and calculating the number of logical processors required to get there. Additionally, while logical processor speeds will have a significant impact on performance, cache efficiencies, memory coherence requirements, thread scheduling and synchronization, and imperfectly balanced client load will all have significant impacts on performance that will vary on a server-by-server basis. With the relatively cheap cost of compute power, attempting to analyze and determine the perfect number of CPUs needed becomes more an academic exercise than it does provide business value.

Forty percent is not a hard and fast requirement, it is a reasonable start. Various consumers of Active Directory require various levels of responsiveness. There may be scenarios where environments can run at 80% or 90% utilization as a sustained average, as the increased wait times for access to the processor will not noticeably impact client performance. It is important to re-iterate that there are many areas in the system that are much slower than the logical processor in the system, including access to RAM, access to disk, and transmitting the response over the network. All of these items need to be tuned in conjunction. Examples:

- Adding more processors to a system running 90% that is disk-bound is probably not going to significantly improve performance. Deeper analysis of the system will probably identify that there are a lot of threads that are not even getting on the processor because they are waiting on I/O to complete.
- Resolving the disk-bound issues potentially means that threads that were previously spending a lot of time in a waiting state will no longer be in a waiting state for I/O and there will be more competition for CPU time, meaning that the 90% utilization in the previous example will go to 100% (because it can not go higher). Both components need to be tuned in conjunction.
  > [!NOTE]
  > Processor Information(*)\\% Processor Utility can exceed 100% with systems that have a "Turbo" mode.  This is where the CPU exceeds the rated processor speed for short periods.  Reference CPU manufacturers documentation and description of the counter for greater insight.  

Discussing whole system utilization considerations also brings into the conversation domain controllers as virtualized guests. [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) applies to both the host and the guest in a virtualized scenario. This is why in a host with only one guest, a domain controller (and generally any system) has near the same performance it does on physical hardware. Adding additional guests to the hosts increases the utilization of the underlying host, thereby increasing the wait times to get access to the processors as explained previously. In short, logical processor utilization needs to be managed at both the host and at the guest levels.

Extending the previous analogies, leaving the highway as the physical hardware, the guest VM will be analogized with a bus (an express bus that goes straight to the destination the rider wants). Imagine the following four scenarios:

- It is off hours, a rider gets on a bus that is nearly empty, and the bus gets on a road that is also nearly empty. As there is no traffic to contend with, the rider has a nice easy ride and gets there just as fast as if the rider had driven instead. The rider’s travel times are still constrained by the speed limit.
- It is off hours so the bus is nearly empty but most of the lanes on the road are closed, so the highway is still congested. The rider is on an almost-empty bus on a congested road. While the rider does not have a lot of competition in the bus for where to sit, the total trip time is still dictated by the rest of the traffic outside.
- It is rush hour so the highway and the bus are congested. Not only does the trip take longer, but getting on and off the bus is a nightmare because people are shoulder to shoulder and the highway is not much better. Adding more busses (logical processors to the guest) does not mean they can fit on the road any more easily, or that the trip will be shortened.
- The final scenario, though it may be stretching the analogy a little, is where the bus is full, but the road is not congested. While the rider will still have trouble getting on and off the bus, the trip will be efficient after the bus is on the road. This is the only scenario where adding more busses (logical processors to the guest) will improve guest performance.

From there it is relatively easy to extrapolate that there are a number of scenarios in between the 0%-utilized and the 100%-utilized state of the road and the 0%- and 100%-utilized state of the bus that have varying degrees of impact.

Applying the principals above of 40% CPU as reasonable target for the host as well as the guest is a reasonable start for the same reasoning as above, the amount of queuing.

## Appendix B: Considerations regarding different processor speeds, and the effect of processor power management on processor speeds

Throughout the sections on processor selection the assumption is made that the processor is running at 100% of clock speed the entire time the data is being collected and that the replacement systems will have the same speed processors. Despite both assumptions in practice being false, particularly with Windows Server 2008 R2 and later, where the default power plan is **Balanced**, the methodology still stands as it is the conservative approach. While the potential error rate may increase, it only increases the margin of safety as processor speeds increase.

- For example, in a scenario where 11.25 CPUs are demanded, if the processors were running at half speed when the data was collected, the more accurate estimate might be 5.125 &divide; 2.
- It is impossible to guarantee that doubling the clock speeds would double the amount of processing that happens for a given time period. This is due to the fact the amount of time that the processors spend waiting on RAM or other system components could stay the same. The net effect is that the faster processors might spend a greater percentage of the time idle while waiting on data to be fetched. Again, it is recommended to stick with the lowest common denominator, being conservative, and avoid trying to calculate a potentially false level of accuracy by assuming a linear comparison between processor speeds.

Alternatively, if processor speeds in replacement hardware are lower than current hardware, it would be safe to increase the estimate of processors needed by a proportionate amount. For example, it is calculated that 10 processors are needed to sustain the load in a site, and the current processors are running at 3.3 Ghz and replacement processors will run at 2.6 Ghz, this is a 21% decrease in speed. In this case, 12 processors would be the recommended amount.

That said, this variability would not change the Capacity Management processor utilization targets. As processor clock speeds will be adjusted dynamically based on the load demanded, running the system under higher loads will generate a scenario where the CPU spends more time in a higher clock speed state, making the ultimate goal to be at 40% utilization in a 100% clock speed state at peak. Anything less than that will generate power savings as CPU speeds will be throttled back during off peak scenarios.

> [!NOTE]
> An option would be to turn off power management on the processors (setting the power plan to **High Performance**) while data is collected. That would give a more accurate representation of the CPU consumption on the target server.

To adjust estimates for different processors, it used to be safe, excluding other system bottlenecks outlined above, to assume that doubling processor speeds doubled the amount of processing that could be performed.  Today, the internal architecture of processors is different enough between processors, that a safer way to gauge the effects of using different processors than data was taken from is to leverage the SPECint_rate2006 benchmark from Standard Performance Evaluation Corporation.

1. Find the SPECint_rate2006 scores for the processor that are in use and that plan to be used.
    1. On the website of the Standard Performance Evaluation Corporation, select **Results**, highlight **CPU2006**, and select **Search all SPECint_rate2006 results**.
    1. Under **Simple Request**, enter the search criteria for the target processor, for example **Processor Matches E5-2630 (baselinetarget)** and **Processor Matches E5-2650 (baseline)**.
    1. Find the server and processor configuration to be used (or something close, if an exact match is not available) and note the value in the **Result** and **# Cores** columns.
1. To determine the modifier use the following equation:
   >((*Target platform per-core score value*) &times; (*MHz per-core of baseline platform*)) &divide; ((*Baseline per-core score value*) &times; (*MHz per-core of target platform*))  

    Using the above example:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multiply the estimated number of processors by the modifier.  In the above case to go from the E5-2650 processor to the E5-2630 processor multiply the calculated 11.25 CPUs &times; 0.92 = 10.35 processors needed.

## Appendix C: Fundamentals regarding the operating system interacting with storage

The queuing theory concepts outlined in [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) are also applicable to storage. Having a familiarity of how the operating system handles I/O is necessary to apply these concepts. In the Microsoft Windows operating system, a queue to hold the I/O requests is created for each physical disk. However, a clarification on physical disk needs to be made. Array controllers and SANs present aggregations of spindles to the operating system as single physical disks. Additionally, array controllers and SANs can aggregate multiple disks into one array set and then split this array set into multiple “partitions”, which is in turn presented to the operating system as multiple physical disks (ref. figure).

![Block spindles](media/capacity-planning-considerations-block-spindles.png)  

In this figure the two spindles are mirrored and split into logical areas for data storage (Data 1 and Data 2). These logical areas are viewed by the operating system as separate physical disks.

Although this can be highly confusing, the following terminology is used throughout this appendix to identify the different entities:

- **Spindle –** the device that is physically installed in the server.
- **Array –** a collection of spindles aggregated by controller.
- **Array partition –** a partitioning of the aggregated array
- **LUN –** an array, used when referring to SANs
- **Disk –** What the operating system observes to be a single physical disk.
- **Partition –** a logical partitioning of what the operating system perceives as a physical disk.

### Operating system architecture considerations

The operating system creates a First In/First Out (FIFO) I/O queue for each disk that is observed; this disk may be representing a spindle, an array, or an array partition. From the operating system perspective, with regard to handling I/O, the more active queues the better. As a FIFO queue is serialized, meaning that all I/Os issued to the storage subsystem must be processed in the order the request arrived. By correlating each disk observed by the operating system with a spindle/array, the operating system now maintains an I/O queue for each unique set of disks, thereby eliminating contention for scarce I/O resources across disks and isolating I/O demand to a single disk. As an exception, Windows Server 2008 introduces the concept of I/O prioritization, and applications designed to use the “Low” priority fall out of this normal order and take a back seat. Applications not specifically coded to leverage the “Low” priority default to “Normal.”

### Introducing simple storage subsystems

Starting with a simple example (a single hard drive inside a computer) a component-by-component analysis will be given. Breaking this down into the major storage subsystem components, the system consists of:

- **1 –** 10,000 RPM Ultra Fast SCSI HD (Ultra Fast SCSI has a 20 MB/s transfer rate)
- **1 –** SCSI Bus (the cable)
- **1 –** Ultra Fast SCSI Adapter
- **1 –** 32-bit 33 MHz PCI bus

Once the components are identified, an idea of how much data can transit the system, or how much I/O can be handled, can be calculated. Note that the amount of I/O and quantity of data that can transit the system is correlated, but not the same. This correlation depends on whether the disk I/O is random or sequential and the block size. (All data is written to the disk as a block, but different applications using different block sizes.) On a component-by-component basis:

- **The hard drive –** The average 10,000-RPM hard drive has a 7-millisecond (ms) seek time and a 3 ms access time. Seek time is the average amount of time it takes the read/write head to move to a location on the platter. Access time is the average amount of time it takes to read or write the data to disk, once the head is in the correct location. Thus, the average time for reading a unique block of data in a 10,000-RPM HD constitutes a seek and an access, for a total of approximately 10 ms (or .010 seconds) per block of data.

  When every disk access requires movement of the head to a new location on the disk, the read/write behavior is referred to as “random.” Thus, when all I/O is random, a 10,000-RPM HD can handle approximately 100 I/O per second (IOPS) (the formula is 1000 ms per second divided by 10 ms per I/O or 1000/10=100 IOPS).

  Alternatively, when all I/O occurs from adjacent sectors on the HD, this is referred to as sequential I/O. Sequential I/O has no seek time because when the first I/O is complete, the read/write head is at the start of where the next block of data is stored on the HD. Thus a 10,000-RPM HD is capable of handling approximately 333 I/O per second (1000 ms per second divided by 3 ms per I/O).

  >[!NOTE]
  >This example does not reflect the disk cache, where the data of one cylinder is typically kept. In this case, the 10 ms are needed on the first I/O and the disk reads the whole cylinder. All other sequential I/O is satisfied from the cache. As a result, in-disk caches might improve sequential I/O performance.
  
  So far, the transfer rate of the hard drive has been irrelevant. Whether the hard drive is 20 MB/s Ultra Wide or an Ultra3 160 MB/s, the actual amount of IOPS the can be handled by the 10,000-RPM HD is ~100 random or ~300 sequential I/O. As block sizes change based on the application writing to the drive, the amount of data that is pulled per I/O is different. For example, if the block size is 8 KB, 100 I/O operations will read from or write to the hard drive a total of 800 KB. However, if the block size is 32 KB, 100 I/O will read/write 3,200 KB (3.2 MB) to the hard drive. As long as the SCSI transfer rate is in excess of the total amount of data transferred, getting a “faster” transfer rate drive will gain nothing. See the following tables for comparison.

  | |7200 RPM 9ms seek, 4ms access|10,000 RPM 7ms seek, 3ms access|15,000 RPM 4ms seek, 2ms access
  |-|-|-|-|
  |Random I/O|80|100|150|
  |Sequential I/O|250|300|500|  
  
  |10,000 RPM drive|8 KB block size (Active Directory Jet)|
  |-|-|
  |Random I/O|800 KB/s|
  |Sequential I/O|2400 KB/s|

- **SCSI backplane (bus) –** Understanding how the “SCSI backplane (bus)”, or in this scenario the ribbon cable, impacts throughput of the storage subsystem depends on knowledge of the block size. Essentially the question would be, how much I/O can the bus handle if the I/O is in 8 KB blocks? In this scenario, the SCSI bus is 20 MB/s, or 20480 KB/s. 20480 KB/s divided by 8 KB blocks yields a maximum of approximately 2500 IOPS supported by the SCSI bus.

  >[!NOTE]
  >The figures in the following table represent an example. Most attached storage devices currently use PCI Express, which provides much higher throughput.  
  
  |I/O supported by SCSI bus per block size|2 KB block size|8 KB block size (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  As can be determined from this chart, in the scenario presented, no matter what the use, the bus will never be a bottleneck, as the spindle maximum is 100 I/O, well below any of the above thresholds.

  >[!NOTE]
  >This assumes that the SCSI bus is 100% efficient.
  
- **SCSI adapter –** For determining the amount of I/O that this can handle, the manufacturer’s specifications need to be checked. Directing I/O requests to the appropriate device requires processing of some sort, thus the amount of I/O that can be handled is dependent on the SCSI adapter (or array controller) processor.

  In this example, the assumption that 1,000 I/O can be handled will be made.

- **PCI bus –** This is an often overlooked component. In this example, this will not be the bottleneck; however as systems scale up, it can become a bottleneck. For reference, a 32 bit PCI bus operating at 33Mhz can in theory transfer 133 MB/s of data. Following is the equation:  
  > 32 bits &divide; 8 bits per byte &times; 33 MHz = 133 MB/s.  

  Note that is the theoretical limit; in reality only about 50% of the maximum is actually reached, although in certain burst scenarios, 75% efficiency can be obtained for short periods.

  A 66Mhz 64-bit PCI bus can support a theoretical maximum of (64 bits &divide; 8 bits per byte &times; 66 Mhz) = 528 MB/sec. Additionally, any other device (such as the network adapter, second SCSI controller, and so on) will reduce the bandwidth available as the bandwidth is shared and the devices will contend for the limited resources.

After analysis of the components of this storage subsystem, the spindle is the limiting factor in the amount of I/O that can be requested, and consequently the amount of data that can transit the system. Specifically, in an AD DS scenario, this is 100 random I/O per second in 8 KB increments, for a total of 800 KB per second when accessing the Jet database. Alternatively, the maximum throughput for a spindle that is exclusively allocated to log files would suffer the following limitations: 300 sequential I/O per second in 8 KB increments, for a total of 2400 KB (2.4 MB) per second.

Now, having analyzed a simple configuration, the following table demonstrates where the bottleneck will occur as components in the storage subsystem are changed or added.

|Notes|Bottleneck analysis|Disk|Bus|Adapter|PCI bus|
|-|-|-|-|-|-|
|This is the domain controller configuration after adding a second disk. The disk configuration represents the bottleneck at 800 KB/s.|Add 1 disk (Total=2)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|200 I/Os total<br />800 KB/s total.| | | |
|After adding 7 disks, the disk configuration still represents the bottleneck at 3200 KB/s.|**Add 7 disks (Total=8)**  <br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|800 I/Os total.<br />3200 KB/s total| | | |
|After changing I/O to sequential, the network adapter becomes the bottleneck because it is limited to 1000 IOPS.|Add 7 disks (Total=8)<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD| | |2400 I/O sec can be read/written to disk, controller limited to 1000 IOPS| |
|After replacing the network adapter with a SCSI adapter that supports 10,000 IOPS, the bottleneck returns to the disk configuration.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade SCSI adapter (now supports 10,000 I/O)**|800 I/Os total.<br />3,200 KB/s total| | | |
|After increasing the block size to 32 KB, the bus becomes the bottleneck because it only supports 20 MB/s.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />**32 KB block size**<br /><br />10,000 RPM HD| |800 I/Os total. 25,600 KB/s (25 MB/s) can be read/written to disk.<br /><br />The bus only supports 20 MB/s| | |
|After upgrading the bus and adding more disks, the disk remains the bottleneck.|**Add 13 disks (Total=14)**<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade to 320 MB/s SCSI bus**|2800 I/Os<br /><br />11,200 KB/s (10.9 MB/s)| | | |
|After changing I/O to sequential, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI Adapter with 14 disks<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus|8,400 I/Os<br /><br />33,600 KB\s<br /><br />(32.8 MB\s)| | | |
|After adding faster hard drives, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />4 KB block size<br /><br />**15,000 RPM HD**<br /><br />Upgrade to 320 MB/s SCSI bus|14,000 I/Os<br /><br />56,000 KB/s<br /><br />(54.7 MB/s)| | | |
|After increasing the block size to 32 KB, the PCI bus becomes the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />**32 KB block size**<br /><br />15,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus| | | |14,000 I/Os<br /><br />448,000 KB/s<br /><br />(437 MB/s) is the read/write limit to the spindle.<br /><br />The PCI bus supports a theoretical maximum of 133 MB/s (75% efficient at best).|

### Introducing RAID

The nature of a storage subsystem does not change dramatically when an array controller is introduced; it just replaces the SCSI adapter in the calculations. What does change is the cost of reading and writing data to the disk when using the various array levels (such as RAID 0, RAID 1, or RAID 5).

In RAID 0, the data is striped across all the disks in the RAID set. This means that during a read or a write operation, a portion of the data is pulled from or pushed to each disk, increasing the amount of data that can transit the system during the same time period. Thus, in one second, on each spindle (again assuming 10,000-RPM drives), 100 I/O operations can be performed. The total amount of I/O that can be supported is N spindles times 100 I/O per second per spindle (yields 100*N I/O per second).

![Logical d: drive](media/capacity-planning-considerations-logical-d-drive.png)

In RAID 1, the data is mirrored (duplicated) across a pair of spindles for redundancy. Thus, when a read I/O operation is performed, data can be read from both of the spindles in the set. This effectively makes the I/O capacity from both disks available during a read operation. The caveat is that write operations gain no performance advantage in a RAID 1. This is because the same data needs to be written to both drives for the sake of redundancy. Though it does not take any longer, as the write of data occurs concurrently on both spindles, because both spindles are occupied duplicating the data, a write I/O operation in essence prevents two read operations from occurring. Thus, every write I/O costs two read I/O. A formula can be created from that information to determine the total number of I/O operations that are occurring:  

> *Read I/O* + 2 &times; *Write I/O* = *Total available disk I/O consumed*  

When the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array:  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total IOPS*

RAID 1+ 0, behaves exactly the same as RAID 1 regarding the expense of reading and writing. However, the I/O is now striped across each mirrored set. If  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total I/O*  

in a RAID 1 set, when a multiplicity (*N*) of RAID 1 sets are striped, the Total I/O that can be processed becomes N &times; I/O per RAID 1 set:  

> *N* &times; {*Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] } = *Total IOPS*

In RAID 5, sometimes referred to as *N* + 1 RAID, the data is striped across *N* spindles and parity information is written to the “+ 1” spindle. However, RAID 5 is much more expensive when performing a write I/O than RAID 1 or 1 + 0. RAID 5 performs the following process every time a write I/O is submitted to the array:

1. Read the old data
1. Read the old parity
1. Write the new data
1. Write the new parity

As every write I/O request that is submitted to the array controller by the operating system requires four I/O operations to complete, write requests submitted take four times as long to complete as a single read I/O. To derive a formula to translate I/O requests from the operating system perspective to that experienced by the spindles:  

> *Read I/O* + 4 &times; *Write I/O* = *Total I/O*  

Similarly in a RAID 1 set, when the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array (Note that total number of spindles does not include the “drive” lost to parity):  

> *IOPS per spindle* &times; (*Spindles* – 1) &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 4 &times; *%Writes*)] = *Total IOPS*

### Introducing SANs

Expanding the complexity of the storage subsystem, when a SAN is introduced into the environment, the basic principles outlined do not change, however I/O behavior for all of the systems connected to the SAN needs to be taken into account. As one of the major advantages in using a SAN is an additional amount of redundancy over internally or externally attached storage, capacity planning now needs to take into account fault tolerance needs. Also, more components are introduced that need to be evaluated. Breaking a SAN down into the component parts:

- SCSI or Fibre Channel hard drive
- Storage unit channel backplane
- Storage units
- Storage controller module
- SAN switch(es)
- HBA(s)
- The PCI bus

When designing any system for redundancy, additional components are included to accommodate the potential of failure. It is very important, when capacity planning, to exclude the redundant component from available resources. For example, if the SAN has two controller modules, the I/O capacity of one controller module is all that should be used for total I/O throughput available to the system. This is due to the fact that if one controller fails, the entire I/O load demanded by all connected systems will need to be processed by the remaining controller. As all capacity planning is done for peak usage periods, redundant components should not be factored into the available resources and planned peak utilization should not exceed 80% saturation of the system (in order to accommodate bursts or anomalous system behavior). Similarly, the redundant SAN switch, storage unit, and spindles should not be factored into the I/O calculations.

When analyzing the behavior of the SCSI or Fibre Channel hard drive, the method of analyzing the behavior as outlined previously does not change. Although there are certain advantages and disadvantages to each protocol, the limiting factor on a per disk basis is the mechanical limitation of the hard drive.

Analyzing the channel on the storage unit is exactly the same as calculating the resources available on the SCSI bus, or bandwidth (such as 20 MB/s) divided by block size (such as 8 KB). Where this deviates from the simple previous example is in the aggregation of multiple channels. For example, if there are 6 channels, each supporting 20 MB/s maximum transfer rate, the total amount of I/O and data transfer that is available is 100 MB/s (this is correct, it is not 120 MB/s). Again, fault tolerance is a major player in this calculation, in the event of the loss of an entire channel, the system is only left with 5 functioning channels. Thus, to ensure continuing to meet performance expectations in the event of failure, total throughput for all of the storage channels should not exceed 100 MB/s (this assumes load and fault tolerance is evenly distributed across all channels). Turning this into an I/O profile is dependent on the behavior of the application. In the case of Active Directory Jet I/O, this would correlate to approximately 12,500 I/O per second (100 MB/s &divide; 8 KB per I/O).

Next, obtaining the manufacturer’s specifications for the controller modules is required in order to gain an understanding of the throughput each module can support. In this example, the SAN has two controller modules that support 7,500 I/O each. The total throughput of the system may be 15,000 IOPS if redundancy is not desired. In calculating maximum throughput in the case of failure, the limitation is the throughput of one controller, or 7,500 IOPS. This threshold is well below the 12,500 IOPS (assuming 4 KB block size) maximum that can be supported by all of the storage channels, and thus, is currently the bottleneck in the analysis. Still for planning purposes, the desired maximum I/O to be planned for would be 10,400 I/O.

When the data exits the controller module, it transits a Fibre Channel connection rated at 1 GB/s (or 1 Gigabit per second). To correlate this with the other metrics, 1 GB/s turns into 128 MB/s (1 GB/s &divide; 8 bits/byte). As this is in excess of the total bandwidth across all channels in the storage unit (100 MB/s), this will not bottleneck the system. Additionally, as this is only one of the two channels (the additional 1 GB/s Fibre Channel connection being for redundancy), if one connection fails, the remaining connection still has enough capacity to handle all the data transfer demanded.

En route to the server, the data will most likely transit a SAN switch. As the SAN switch has to process the incoming I/O request and forward it out the appropriate port, the switch will have a limit to the amount of I/O that can be handled, however, manufacturers specifications will be required to determine what that limit is. For example, if there are two switches and each switch can handle 10,000 IOPS, the total throughput will be 20,000 IOPS. Again, fault tolerance being a concern, if one switch fails, the total throughput of the system will be 10,000 IOPS. As it is desired not to exceed 80% utilization in normal operation, using no more than 8000 I/O should be the target.

Finally, the HBA installed in the server would also have a limit to the amount of I/O that it can handle. Usually, a second HBA is installed for redundancy, but just like with the SAN switch, when calculating maximum I/O that can be handled, the total throughput of *N* &ndash; 1 HBAs is what the maximum scalability of the system is.

### Caching considerations

Caches are one of the components that can significantly impact the overall performance at any point in the storage system. Detailed analysis about caching algorithms is beyond the scope of this article; however, some basic statements about caching on disk subsystems are worth illuminating:

- Caching does improved sustained sequential write I/O as it can buffer many smaller write operations into larger I/O blocks and de-stage to storage in fewer, but larger block sizes. This will reduce total random I/O and total sequential I/O, thus providing more resource availability for other I/O.
- Caching does not improve sustained write I/O throughput of the storage subsystem. It only allows for the writes to be buffered until the spindles are available to commit the data. When all the available I/O of the spindles in the storage subsystem is saturated for long periods, the cache will eventually fill up. In order to empty the cache, enough time between bursts, or extra spindles, need to be allotted in order to provide enough I/O to allow the cache to flush.

  Larger caches only allow for more data to be buffered. This means longer periods of saturation can be accommodated.

  In a normally operating storage subsystem, the operating system will experience improved write performance as the data only needs to be written to cache. Once the underlying media is saturated with I/O, the cache will fill and write performance will return to disk speed.

- When caching read I/O, the scenario where the cache is most advantageous is when the data is stored sequentially on the disk, and the cache can read-ahead (it makes the assumption that the next sector contains the data that will be requested next).
- When read I/O is random, caching at the drive controller is unlikely to provide any enhancement to the amount of data that can be read from the disk. Any enhancement is non-existent if the operating system or application-based cache size is greater than the hardware-based cache size.

  In the case of Active Directory, the cache is only limited by the amount of RAM.

### SSD considerations

SSDs are a completely different animal than spindle-based hard disks. Yet the two key criteria remain: “How many IOPS can it handle?” and “What is the latency for those IOPS?” In comparison to spindle-based hard disks, SSDs can handle higher volumes of I/O and can have lower latencies. In general and as of this writing, while SSDs are still expensive in a cost-per-Gigabyte comparison, they are very cheap in terms of cost-per-I/O and deserve significant consideration in terms of storage performance.

Considerations:

- Both IOPS and latencies are very subjective to the manufacturer designs and in some cases have been observed to be poorer performing than spindle based technologies. In short, it is more important to review and validate the manufacturer specs drive by drive and not assume any generalities.
- IOPS types can have very different numbers depending on whether it is read or write. AD DS services, in general, being predominantly read-based, will be less affected than some other application scenarios.
- “Write endurance” – this is the concept that SSD cells will eventually wear out. Various manufacturers deal with this challenge different fashions. At least for the database drive, the predominantly read I/O profile allows for downplaying the significance of this concern as the data is not highly volatile.

### Summary

One way to think about storage is picturing household plumbing. Imagine the IOPS of the media that the data is stored on is the household main drain. When this is clogged (such as roots in the pipe) or limited (it is collapsed or too small), all the sinks in the household back up when too much water is being used (too many guests). This is perfectly analogous to a shared environment where one or more systems are leveraging shared storage on an SAN/NAS/iSCSI with the same underlying media. Different approaches can be taken to resolve the different scenarios:

- A collapsed or undersized drain requires a full scale replacement and fix. This would be similar to adding in new hardware or redistributing the systems using the shared storage throughout the infrastructure.
- A “clogged” pipe usually means identification of one or more offending problems and removal of those problems. In a storage scenario this could be storage or system level backups, synchronized antivirus scans across all servers, and synchronized defragmentation software running during peak periods.

In any plumbing design, multiple drains feed into the main drain. If anything stops up one of those drains or a junction point, only the things behind that junction point back up. In a storage scenario, this could be an overloaded switch (SAN/NAS/iSCSI scenario), driver compatibility issues (wrong driver/HBA Firmware/storport.sys combination), or backup/antivirus/defragmentation. To determine if the storage “pipe” is big enough, IOPS and I/O size needs to be measured. At each joint add them together to ensure adequate “pipe diameter.”

## Appendix D - Discussion on storage troubleshooting - Environments where providing at least as much RAM as the database size is not a viable option

It is helpful to understand why these recommendations exist so that the changes in storage technology can be accommodated. These recommendations exist for two reasons. The first is isolation of IO, such that performance issues (that is, paging) on the operating system spindle do not impact performance of the database and I/O profiles. The second is that log files for AD DS (and most databases) are sequential in nature, and spindle-based hard drives and caches have a huge performance benefit when used with sequential I/O as compared to the more random I/O patterns of the operating system and almost purely random I/O patterns of the AD DS database drive. By isolating the sequential I/O to a separate physical drive, throughput can be increased. The challenge presented by today’s storage options is that the fundamental assumptions behind these recommendations are no longer true. In many virtualized storage scenarios, such as iSCSI, SAN, NAS, and Virtual Disk image files, the underlying storage media is shared across multiple hosts, thus completely negating both the “isolation of IO” and the “sequential I/O optimization” aspects. In fact these scenarios add an additional layer of complexity in that other hosts accessing the shared media can degrade responsiveness to the domain controller.

In planning storage performance, there are three categories to consider: cold cache state, warmed cache state, and backup/restore. The cold cache state occurs in scenarios such as when the domain controller is initially rebooted or the Active Directory service is restarted and there is no Active Directory data in RAM. Warm cache state is where the domain controller is in a steady state and the database is cached. These are important to note as they will drive very different performance profiles, and having enough RAM to cache the entire database does not help performance when the cache is cold. One can consider performance design for these two scenarios with the following analogy, warming the cold cache is a “sprint” and running a server with a warm cache is a “marathon.”

For both the cold cache and warm cache scenario, the question becomes how fast the storage can move the data from disk into memory. Warming the cache is a scenario where, over time, performance improves as more queries reuse data, the cache hit rate increases, and the frequency of needing to go to disk decreases. As a result the adverse performance impact of going to disk decreases. Any degradation in performance is only transient while waiting for the cache to warm and grow to the maximum, system-dependent allowed size. The conversation can be simplified to how quickly the data can be gotten off of disk, and is a simple measure of the IOPS available to Active Directory, which is subjective to IOPS available from the underlying storage. From a planning perspective, because warming the cache and backup/restore scenarios happen on an exceptional basis, normally occur off hours, and are subjective to the load of the DC, general recommendations do not exist except in that these activities be scheduled for non-peak hours.

AD DS, in most scenarios, is predominantly read IO, usually a ratio of 90% read/10% write. Read I/O often tends to be the bottleneck for user experience, and with write IO, causes write performance to degrade. As I/O to the NTDS.dit is predominantly random, caches tend to provide minimal benefit to read IO, making it that much more important to configure the storage for read I/O profile correctly.

For normal operating conditions, the storage planning goal is minimize the wait times for a request from AD DS to be returned from disk. This essentially means that the number of outstanding and pending I/O is less than or equivalent to the number of pathways to the disk. There are a variety of ways to measure this. In a performance monitoring scenario, the general recommendation is that LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read be less than 20 ms. The desired operating threshold must be much lower, preferably as close to the speed of the storage as possible, in the 2 to 6 millisecond (.002 to .006 second) range depending on the type of storage.

Example:

![Storage latency chart](media/capacity-planning-considerations-storage-latency.png)

Analyzing the chart:

- **Green oval on the left –** The latency remains consistent at 10 ms. The load increases from 800 IOPS to 2400 IOPS. This is the absolute floor to how quickly an I/O request can be processed by the underlying storage. This is subject to the specifics of the storage solution.
- **Burgundy oval on the right –** The throughput remains flat from the exit of the green circle through to the end of the data collection while the latency continues to increase. This is demonstrating that when the request volumes exceed the physical limitations of the underlying storage, the longer the requests spend sitting in the queue waiting to be sent out to the storage subsystem.

Applying this knowledge:

- **Impact to a user querying membership of a large group –** Assume this requires reading 1 MB of data from the disk, the amount of I/O and how long it takes can be evaluated as follows:
  - Active Directory database pages are 8 KB in size. 
  - A minimum of 128 pages need to be read in from disk. 
  - Assuming nothing is cached, at the floor (10 ms) this is going to take a minimum 1.28 seconds to load the data from disk in order to return it to the client. At 20 ms, where the throughput on storage has long since maxed out and is also the recommended maximum, it will take 2.5 seconds to get the data from disk in order to return it to the end user.  
- **At what rate will the cache be warmed –** Making the assumption that the client load is going to maximize the throughput on this storage example, the cache will warm at a rate of 2400 IOPS &times; 8 KB per IO. Or, approximately 20 MB/s per second, loading about 1 GB of database into RAM every 53 seconds.

> [!NOTE]
> It is normal for short periods to observe the latencies climb when components aggressively read or write to disk, such as when the system is being backed up or when AD DS is running garbage collection. Additional head room on top of the calculations should be provided to accommodate these periodic events. The goal being to provide enough throughput to accommodate these scenarios without impacting normal function.

As can be seen, there is a physical limit based on the storage design to how quickly the cache can possibly warm. What will warm the cache are incoming client requests up to the rate that the underlying storage can provide. Running scripts to “pre-warm” the cache during peak hours will provide competition to load driven by real client requests. That can adversely affect delivering data that clients need first because, by design, it will generate competition for scarce disk resources as artificial attempts to warm the cache will load data that is not relevant to the clients contacting the DC.Processador Information(_Total)\\' Utilitário de processador Domain Services
description: Detailed discussion of the factors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>After eliminating storage as a bottleneck, address the amount of compute power needed.</li><li>While not perfectly linear, the number of processor cores consumed across all servers within a specific scope (such as a site) can be used to gauge how many processors are necessary to support the total client load. Add the minimum necessary to maintain the current level of service across all the systems within the scope.</li><li>Changes in processor speed, including power management related changes, impact numbers derived from the current environment. Generally, it is impossible to precisely evaluate how going from a 2.5 GHz processor to a 3 GHz processor will reduce the number of CPUs needed.</li></ul>|
|NetLogon|<ul><li>“Netlogon(\*)\Semaphore Acquires”</li><li>“Netlogon(\*)\Semaphore Timeouts”</li><li>“Netlogon(\*)\Average Semaphore Hold Time”</li></ul>|<ul><li>Net Logon Secure Channel/MaxConcurrentAPI only affects environments with NTLM authentications and/or PAC Validation. PAC Validation is on by default in operating system versions before Windows Server 2008. This is a client setting, so the DCs will be impacted until this is turned off on all client systems.</li><li>Environments with significant cross trust authentication, which includes intra-forest trusts, have greater risk if not sized properly.</li><li>Server consolidations will increase concurrency of cross-trust authentication.</li><li>Surges need to be accommodated, such as cluster fail-overs, as users re-authenticate en masse to the new cluster node.</li><li>Individual client systems (such as a cluster) might need tuning too.</li></ul>|

## Planning

For a long time, the community’s recommendation for sizing AD DS has been to “put in as much RAM as the database size.” For the most part, that recommendation is all that most environments needed to be concerned about. But the ecosystem consuming AD DS has gotten much bigger, as have the AD DS environments themselves, since its introduction in 1999. Although the increase in compute power and the switch from x86 architectures to x64 architectures has made the subtler aspects of sizing for performance irrelevant to a larger set of customers running AD DS on physical hardware, the growth of virtualization has reintroduced the tuning concerns to a larger audience than before.

The following guidance is thus about how to determine and plan for the demands of Active Directory as a service regardless of whether it is deployed in a physical, a virtual/physical mix, or a purely virtualized scenario. As such, we will break down the evaluation to each of the four main components: storage, memory, network, and processor. In short, in order to maximize performance on AD DS, the goal is to get as close to processor bound as possible.

## RAM

Simply, the more that can be cached in RAM, the less it is necessary to go to disk. To maximize the scalability of the server the minimum amount of RAM should be the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). An additional amount should be added to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth based on environmental changes.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage  section to ensure that storage is properly designed.

A corollary that comes up in the general context in sizing memory is sizing of the page file. In the same context as everything else memory related, the goal is to minimize going to the much slower disk. Thus the question should go from, “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Evaluating

The amount of RAM that a domain controller (DC) needs is actually a complex exercise for these reasons:

- High potential for error when trying to use an existing system to gauge how much RAM is needed as LSASS will trim under memory pressure conditions, artificially deflating the need.
- The subjective fact that an individual DC only needs to cache what is “interesting” to its clients. This means that the data that needs to be cached on a DC in a site with only an Exchange server will be very different than the data that needs to be cached on a DC that only authenticates users.
- The labor to evaluate RAM for each DC on a case-by-case basis is prohibitive and changes as the environment changes.
- The criteria behind the recommendation will help to make informed decisions: 
- The more that can be cached in RAM, the less it is necessary to go to disk. 
- Storage is by far the slowest component of a computer. Access to data on spindle-based and SSD storage media is on the order of 1,000,000x slower than access to data in RAM.

Thus, in order to maximize the scalability of the server, the minimum amount of RAM is the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). Add additional amounts to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth. However, for satellite locations with a small set of end users, these requirements can be relaxed as these sites will not need to cache as much to service most of the requests.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.

> [!NOTE]
> A corollary while sizing memory is sizing of the page file. Because the goal is to minimize going to the much slower disk, the question goes from “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Virtualization considerations for RAM

Avoid memory over-commit at the host. The fundamental goal behind optimizing the amount of RAM is to minimize the amount of time spent going to disk. In virtualization scenarios, the concept of memory over-commit exists where more RAM is allocated to the guests then exists on the physical machine. This in and of itself is not a problem. It becomes a problem when the total memory actively used by all the guests exceeds the amount of RAM on the host and the underlying host starts paging. Performance becomes disk-bound in cases where the domain controller is going to the NTDS.dit to get data, or the domain controller is going to the page file to get data, or the host is going to disk to get data that the guest thinks is in RAM.

### Calculation summary example

|Component|Estimated memory (example)|
|-|-|
|Base operating system recommended RAM (Windows Server 2008)|2 GB|
|LSASS internal tasks|200 MB|
|Monitoring agent|100 MB|
|Antivirus|100 MB|
|Database (Global Catalog)|8.5 GB are you sure ???|
|Cushion for backup to run, administrators to log on without impact|1 GB|
|Total|12 GB|

**Recommended: 16 GB**

Over time, the assumption can be made that more data will be added to the database and the server will probably be in production for 3 to 5 years. Based on an estimate of growth of 33%, 16 GB would be a reasonable amount of RAM to put in a physical server. In a virtual machine, given the ease with which settings can be modified and RAM can be added to the VM, starting at 12 GB with the plan to monitor and upgrade in the future is reasonable.

## Network

### Evaluating
This section is less about evaluating the demands regarding replication traffic, which is focused on traffic traversing the WAN and is thoroughly covered in [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), than it is about evaluating total bandwidth and network capacity needed, inclusive of client queries, Group Policy applications, and so on. For existing environments, this can be collected by using performance counters “Network Interface(\*)\Bytes Received/sec,” and “Network Interface(\*)\Bytes Sent/sec.” Sample intervals for Network Interface counters in either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

> [!NOTE]
> Generally, the majority of network traffic on a DC is outbound as the DC responds to client queries. This is the reason for the focus on outbound traffic, though it is recommended to evaluate each environment for inbound traffic also. The same approaches can be used to address and review inbound network traffic requirements. For more information, see Knowledge Base article [929851: The default dynamic port range for TCP/IP has changed in Windows Vista and in Windows Server 2008](http://support.microsoft.com/kb/929851).

### Bandwidth needs

Planning for network scalability covers two distinct categories: the amount of traffic and the CPU load from the network traffic. Each of these scenarios is straight-forward compared to some of the other topics in this article.

In evaluating how much traffic must be supported, there are two unique categories of capacity planning for AD DS in terms of network traffic. The first is replication traffic that traverses between domain controllers and is covered thoroughly in the reference [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) and is still relevant to current versions of AD DS. The second is the intrasite client-to-server traffic. One of the simpler scenarios to plan for, intrasite traffic predominantly receives small requests from clients relative to the large amounts of data sent back to the clients. 100 MB will generally be adequate in environments up to 5,000 users per server, in a site. Using a 1 GB network adapter and Receive Side Scaling (RSS) support is recommended for anything above 5,000 users. To validate this scenario, particularly in the case of server consolidation scenarios, look at Network Interface(\*)\Bytes/sec across all the DCs in a site, add them together, and divide by the target number of domain controllers to ensure that there is adequate capacity. The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (formerly known as Perfmon), making sure all of the counters are scaled the same.

Consider the following example (also known as, a really, really complex way to validate that the general rule is applicable to a specific environment). The following assumptions are made:

- The goal is to reduce the footprint to as few servers as possible. Ideally, one server will carry the load and an additional server is deployed for redundancy (*N* + 1 scenario). 
- In this scenario, the current network adapter supports only 100 MB and is in a switched environment.  
  The maximum target network bandwidth utilization is 60% in an N scenario (loss of a DC).
- Each server has about 10,000 clients connected to it.

Knowledge gained from the data in the chart (Network Interface(\*)\Bytes Sent/sec):

1. The business day starts ramping up around 5:30 and winds down at 7:00 PM.
1. The peak busiest period is from 8:00 AM to 8:15 AM, with greater than 25 Bytes sent/sec on the busiest DC.  
   > [!NOTE]
   > All performance data is historical. So the peak data point at 8:15 indicates the load from 8:00 to 8:15.
1. There are spikes before 4:00 AM, with more than 20 Bytes sent/sec on the busiest DC, which could indicate either load from different time zones or background infrastructure activity, such as backups. Since the peak at 8:00 AM exceeds this activity, it is not relevant.
1. There are five Domain Controllers in the site.
1. The max load is about 5.5 MB/s per DC, which represents 44% of the 100 MB connection. Using this data, it can be estimated that the total bandwidth needed between 8:00 AM and 8:15 AM is 28 MB/s.
   >[!NOTE]
   >Be careful with the fact that Network Interface sent/receive counters are in bytes and network bandwidth is measured in bits. 100 MB &divide; 8 = 12.5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusions:

1. This current environment does meet the N+1 level of fault tolerance at 60% target utilization. Taking one system offline will shift the bandwidth per server from about 5.5 MB/s (44%) to about 7 MB/s (56%).
1. Based on the previously stated goal of consolidating to one server, this both exceeds the maximum target utilization and theoretically the possible utilization of a 100 MB connection.
1. With a 1 GB connection this will represent 22% of the total capacity.
1. Under normal operating conditions in the *N* + 1 scenario, client load will be relatively evenly distributed at about 14 MB/s per server or 11% of total capacity.
1. To ensure that capacity is adequate during unavailability of a DC, the normal operating targets per server would be about 30% network utilization or 38 MB/s per server. Failover targets would be 60% network utilization or 72 MB/s per server.  
  
In short, the final deployment of systems must have a 1 GB network adapter and be connected to a network infrastructure that will support said load. A further note is that given the amount of network traffic generated, the CPU load from network communications can have a significant impact and limit the maximum scalability of AD DS. This same process can be used to estimate the amount of inbound communication to the DC. But given the predominance of outbound traffic relative to inbound traffic, it is an academic exercise for most environments. Ensuring hardware support for RSS is important in environments with greater than 5,000 users per server. For scenarios with high network traffic, balancing of interrupt load can be a bottleneck. This can be detected by Processor(\*)\% Interrupt Time being unevenly distributed across CPUs. RSS enabled NICs can mitigate this limitation and increase scalability.

> [!NOTE]
> A similar approach can be used to estimate the additional capacity necessary when consolidating data centers, or retiring a domain controller in a satellite location. Simply collect the outbound and inbound traffic to clients and that will be the amount of traffic that will now be present on the WAN links.  
>  
> In some cases, you might experience more traffic than expected because traffic is slower, such as when certificate checking fails to meet aggressive time-outs on the WAN. For this reason, WAN sizing and utilization should be an iterative, ongoing process.

### Virtualization considerations for network bandwidth

It is easy to make recommendations for a physical server: 1 GB for servers supporting greater than 5000 users. Once multiple guests start sharing an underlying virtual switch infrastructure, additional attention is necessary to ensure that the host has adequate network bandwidth to support all the guests on the system, and thus requires the additional rigor. This is nothing more than an extension of ensuring the network infrastructure into the host machine. This is regardless whether the network is inclusive of the domain controller running as a virtual machine guest on a host with the network traffic going over a virtual switch, or whether connected directly to a physical switch. The virtual switch is just one more component where the uplink needs to support the amount of data being transmitted. Thus the physical host physical network adapter linked to the switch should be able to support the DC load plus all other guests sharing the virtual switch connected to the physical network adapter.

### Calculation summary example

|System|Peak bandwidth|
|-|-|
DC 1|6.5 MB/s|
DC 2|6.25 MB/s|
|DC 3|6.25 MB/s|
|DC 4|5.75 MB/s|
|DC 5|4.75 MB/s|
|Total|28.5 MB/s|

**Recommended: 72 MB/s** (28.5 MB/s divided by 40%)

|Target system(s) count|Total bandwidth (from above)|
|-|-|
|2|28.5 MB/s|
|Resulting normal behavior|28.5 &divide; 2 = 14.25 MB/s|

As always, over time the assumption can be made that client load will increase and this growth should be planned for as best as possible. The recommended amount to plan for would allow for an estimated growth in network traffic of 50%.

## Storage

Planning storage constitutes two components:

- Capacity, or storage size
- Performance

A great amount of time and documentation is spent on planning capacity, leaving performance often completely overlooked. With current hardware costs, most environments are not large enough that either of these is actually a concern, and the recommendation to “put in as much RAM as the database size” usually covers the rest, though it may be overkill for satellite locations in larger environments.

### Sizing

#### Evaluating for storage

Compared to 13 years ago when Active Directory was introduced, a time when 4 GB and 9 GB drives were the most common drive sizes, sizing for Active Directory is not even a consideration for all but the largest environments. With the smallest available hard drive sizes in the 180 GB range, the entire operating system, SYSVOL, and NTDS.dit can easily fit on one drive. As such, it is recommended to deprecate heavy investment in this area.

The only recommendation for consideration is to ensure that 110% of the NTDS.dit size is available in order to enable defrag. Additionally, accommodations for growth over the life of the hardware should be made.

The first and most important consideration is evaluating how large the NTDS.dit and SYSVOL will be. These measurements will lead into sizing both fixed disk and RAM allocation. Due to the (relatively) low cost of these components, the math does not need to be rigorous and precise. Content about how to evaluate this for both existing and new environments can be found in the [Data Storage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) series of articles. Specifically, refer to the following articles:

- **For existing environments &ndash;** The section titled “To activate logging of disk space that is freed by defragmentation” in the article [Storage Limits](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **For new environments &ndash;** The article titled [Growth Estimates for Active Directory Users and Organizational Units](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > The articles are based on data size estimates made at the time of the release of Active Directory in Windows 2000. Use object sizes that reflect the actual size of objects in your environment.

When reviewing existing environments with multiple domains, there may be variations in database sizes. Where this is true, use the smallest global catalog (GC) and non-GC sizes.  

The database size can vary between operating system versions. DCs that run earlier operating systems such as Windows Server 2003 has a smaller database size than a DC that runs a later operating system such as Windows Server 2008 R2, especially when features such Active Directory Recycle Bin or Credential Roaming are enabled.

> [!NOTE]  
  >
>- For new environments, notice that the estimates in Growth Estimates for Active Directory Users and Organizational Units indicate that 100,000 users (in the same domain) consume about 450 MB of space. Please note that the attributes populated can have a huge impact on the total amount. Attributes will be populated on many objects by both third-party and Microsoft products, including Microsoft Exchange Server and Lync. An evaluation based on the portfolio of the products in the environment is preferred, but the exercise of detailing out the math and testing for precise estimates for all but the largest environments may not actually be worth significant time and effort.
>- Ensure that 110% of the NTDS.dit size is available as free space in order to enable offline defrag, and plan for growth over a three to five year hardware lifespan. Given how cheap storage is, estimating storage at 300% the size of the DIT as storage allocation is safe to accommodate growth and the potential need for offline defrag.

#### Virtualization considerations for storage

In a scenario where multiple Virtual Hard Disk (VHD) files are being allocated on a single volume use a fixed sized disk of at least 210% (100% of the DIT + 110% free space) the size of the DIT to ensure that there is adequate space reserved.  

#### Calculation summary example

|Data collected from evaluation phase| |
|-|-|
|NTDS.dit size|35 GB|
|Modifier to allow for offline defrag|2.1|
|Total storage needed|73.5 GB|

> [!NOTE]
> This storage needed is in addition to the storage needed for SYSVOL, operating system, page file, temporary files, local cached data (such as installer files), and applications.

### Storage performance

#### Evaluating performance of storage

As the slowest component within any computer, storage can have the biggest adverse impact on client experience. For those environments large enough for which the RAM sizing recommendations are not feasible, the consequences of overlooking planning storage for performance can be devastating.  Also, the complexities and varieties of storage technology further increase the risk of failure as the relevance of long standing best practices of “put operating system, logs, and database” on separate physical disks is limited in it’s useful scenarios.  This is because the long standing best practice is based on the assumption that is that a “disk” is a dedicated spindle and this allowed I/O to be isolated.  This assumptions that make this true are no longer relevant with the introduction of:

- New storage types and virtualized and shared storage scenarios
- Shared spindles on a Storage Area Network (SAN)
- VHD file on a SAN or network-attached storage
- Solid State Drives
- Tiered storage architectures (i.e. SSD storage tier caching larger spindle based storage)

In short, the end goal of all storage performance efforts, regardless of underlying storage architecture and design, is to ensure the needed amount of Input/Output Operations per Second (IOPS) are available and that those IOPS happen within an acceptable time frame. This section covers how to evaluate what AD DS demands of the underlying storage in order to ensure storage solutions are properly designed.  Given the variability of today’s storage technologies, it is best to work with the storage vendors to ensure adequate IOPS.  For those scenarios with locally attached storage, reference the Appendix C for the basics in how to design traditional local storage scenarios.  This principals are generally applicable to more complex storage tiers and will also help in dialog with the vendors supporting backend storage solutions.

- Given the wide breadth of storage options available, it is recommended to engage the expertise of hardware support teams or vendors to ensure that the specific solution meets the needs of AD DS. The following numbers are the information that would be provided to the storage specialists.

For environments where the database is too large to be held in RAM, use the performance counters to determine how much I/O needs to be supported:

- LogicalDisk(\*)\Avg Disk sec/Read (for example, if NTDS.dit is stored on the D:/ drive, the full path would be LogicalDisk(D:)\Avg Disk sec/Read)
- LogicalDisk(\*)\Avg Disk sec/Write
- LogicalDisk(\*)\Avg Disk sec/Transfer
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

These should be sampled in 15/30/60 minute intervals to benchmark the demands of the current environment.

#### Evaluating the results

> [!NOTE]
> The focus is on reads from the database as this is usually the most demanding component, the same logic can be applied to writes to the log file by substituting LogicalDisk(*\<NTDS Log\>*)\Avg Disk sec/Write and LogicalDisk(*\<NTDS Log\>*)\Writes/sec):
>  
> - LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read indicates whether or not the current storage is adequately sized.  If the results are roughly equal to the Disk Access Time for the disk type, LogicalDisk(*\<NTDS\>*)\Reads/sec is a valid measure.  Check the manufacturer specifications for the storage on the back end, but good ranges for LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read would roughly be:
>   - 7200 – 9 to 12.5 milliseconds (ms)
>   - 10,000 – 6 to 10 ms
>   - 15,000 – 4 to 6 ms  
>   - SSD – 1 to 3 ms  
>   - >[!NOTE]
>     >Recommendations exist stating that storage performance is degraded at 15ms to 20ms (depending on source).  The difference between the above values and the other guidance is that the above values are the normal operating range.  The other recommendations are troubleshooting guidance to identify when client experience significantly degrades and becomes noticeable.  Reference Appendix C for a deeper explanation.
> - LogicalDisk(*\<NTDS\>*)\Reads/sec is the amount of I/O that is being performed.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is within the optimal range for the backend storage, LogicalDisk(*\<NTDS\>*)\Reads/sec can be used directly to size the storage.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is not within the optimal range for the backend storage, additional I/O is needed according to the following formula:
>     > (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read) &divide; (Physical Media Disk Access Time) &times; (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read)

Considerations:

- Note that if the server is configured with a sub-optimal amount of RAM, these values will be inaccurate for planning purposes.  They will be erroneously on the high side and can still be used as a worst case scenario.
- Adding/Optimizing RAM specifically will drive a decrease in the amount of read I/O (LogicalDisk(*\<NTDS\>*)\Reads/Sec.  This means the storage solution may not have to be as robust as initially calculated.  Unfortunately, anything more specific than this general statement is environmentally dependent on client load and general guidance cannot be provided.  The best option is to adjust storage sizing after optimizing RAM.

#### Virtualization considerations for performance

Similar to all of the preceding virtualization discussions, the key here is to ensure that the underlying shared infrastructure can support the DC load plus the other resources using the underlying shared media and all pathways to it. This is true whether a physical domain controller is sharing the same underlying media on a SAN, NAS, or iSCSI infrastructure as other servers or applications, whether it is a guest using pass through access to a SAN, NAS, or iSCSI infrastructure that shares the underlying media, or if the guest is using a VHD file that resides on shared media locally or a SAN, NAS, or iSCSI infrastructure. The planning exercise is all about making sure that the underlying media can support the total load of all consumers.

Also, from a guest perspective, as there are additional code paths that must be traversed, there is a performance impact to having to go through a host to access any storage. Not surprisingly, storage performance testing indicates that the virtualizing has an impact on throughput that is subjective to the processor utilization of the host system (see Appendix A: CPU Sizing Criteria), which is obviously influenced by the resources of the host demanded by the guest. This contributes to the virtualization considerations regarding processing needs in a virtualized scenario (see [Virtualization considerations for processing](#virtualization-considerations-for-processing)).

Making this more complex is that there are a variety of different storage options that are available that all have different performance impacts. As a safe estimate when migrating from physical to virtual, use a multiplier of 1.10 to adjust for different storage options for virtualized guests on Hyper-V, such as pass-through storage, SCSI Adapter, or IDE. The adjustments that need to be made when transferring between the different storage scenarios are irrelevant as to whether the storage is local, SAN, NAS, or iSCSI.

#### Calculation summary example

Determining the amount of I/O needed for a healthy system under normal operating conditions:

- LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec during the peak period 15 minute period 
- To determine the amount of I/O needed for storage where the capacity of the underlying storage is exceeded:
  >*Needed IOPS* = (LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read &divide; *\<Target Avg Disk sec/Read\>*) &times; LogicalDisk(*\<NTDS Database Drive\>*)\Read/sec

|Counter|Value|
|-|-|
|Actual LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.02 seconds (20 milliseconds)|
|Target LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.01 seconds|
|Multiplier for change in available IO|0.02 &divide; 0.01 = 2|  
  
|Value name|Value|
|-|-|
|LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec|400|
|Multiplier for change in available IO|2|
|Total IOPS needed during peak period|800|

To determine the rate at which the cache is desired to be warmed:

- Determine the maximum acceptable time to warm the cache. It is either the amount of time that it should take to load the entire database from disk, or for scenarios where the entire database cannot be loaded in RAM, this would be the maximum time to fill RAM.
- Determine the size of the database, excluding white space.  For more information, see [Evaluating for storage](#evaluating-for-storage).  
- Divide the database size by 8 KB; that will be the total IOs necessary to load the database.
- Divide the total IOs by the number of seconds in the defined time frame.

Note that the rate calculated, while accurate, will not be exact because previously loaded pages are evicted if ESE is not configured to have a fixed cache size, and AD DS by default uses variable cache size.

|Data points to collect|Values
|-|-|
|Maximum acceptable time to warm|10 minutes (600 seconds)
|Database size|2 GB|  
  
|Calculation step|Formula|Result|
|-|-|-|
|Calculate size of database in pages|(2 GB &times; 1024 &times; 1024) = *Size of database in KB*|2,097,152 KB|
|Calculate number of pages in database|2,097,152 KB &divide; 8 KB = *Number of pages*|262,144 pages|
|Calculate IOPS necessary to fully warm the cache|262,144 pages &divide; 600 seconds = *IOPS needed*|437 IOPS|

## Processing

### Evaluating Active Directory processor usage

For most environments, after storage, RAM, and networking are properly tuned as described in the Planning section, managing the amount of processing capacity will be the component that deserves the most attention. There are two challenges in evaluating CPU capacity needed:

- Whether or not the applications in the environment are being well-behaved in a shared services infrastructure, and is discussed in the section titled “Tracking Expensive and Inefficient Searches” in the article Creating More Efficient Microsoft Active Directory-Enabled Applications or migrating away from down-level SAM calls to LDAP calls.  
  
  In larger environments, the reason this is important is that poorly coded applications can drive volatility in CPU load, “steal” an inordinate amount of CPU time from other applications, artificially drive up capacity needs, and unevenly distribute load against the DCs.  
- As AD DS is a distributed environment with a large variety of potential clients, estimating the expense of a “single client” is environmentally subjective due to usage patterns and the type or quantity of applications leveraging AD DS. In short, much like the networking section, for broad applicability, this is better approached from the perspective of evaluating the total capacity needed in the environment.

For existing environments, as storage sizing was discussed previously, the assumption is made that storage is now properly sized and thus the data regarding processor load is valid. To reiterate, it is critical to ensure that the bottleneck in the system is not the performance of the storage. When a bottleneck exists and the processor is waiting, there are idle states that will go away once the bottleneck is removed.  As processor wait states are removed, by definition, CPU utilization increases as it no longer has to wait on the data. Thus, collect performance counters “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” and “Process(lsass)\\% Processor Time”. The data in “Process(lsass)\\% Processor Time” will be artificially low if “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” exceeds 10 to 15 ms, which is a general threshold that Microsoft support uses for troubleshooting storage-related performance issues. As before, it is recommended that sample intervals be either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

### Introduction

In order to plan capacity planning for domain controllers, processing power requires the most attention and understanding. When sizing systems to ensure maximum performance, there is always a component that is the bottleneck and in a properly sized Domain Controller this will be the processor.

Similar to the networking section where the demand of the environment is reviewed on a site-by-site basis, the same must be done for the compute capacity demanded. Unlike the networking section, where the available networking technologies far exceed the normal demand, pay more attention to sizing CPU capacity.  As any environment of even moderate size; anything over a few thousand concurrent users can put significant load on the CPU.

Unfortunately, due to the huge variability of client applications that leverage AD, a general estimate of users per CPU is woefully inapplicable to all environments. Specifically, the compute demands are subject to user behavior and application profile. Therefore, each environment needs to be individually sized.

#### Target site behavior profile

As mentioned previously, when planning capacity for an entire site, the goal is to target a design with an *N* + 1 capacity design, such that failure of one system during the peak period will allow for continuation of service at a reasonable level of quality. That means that in an “*N*” scenario, load across all the boxes should be less than 100% (better yet, less than 80%) during the peak periods.

Additionally, if the applications and clients in the site are using best practices for locating domain controllers (that is, using the [DsGetDcName function](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), the clients should be relatively evenly distributed with minor transient spikes due to any number of factors.

In the next example, the following assumptions are made:

- Each of the five DCs in the site has four of CPUs.
- Total target CPU usage during business hours is 40% under normal operating conditions (“*N* + 1”) and 60% otherwise (“*N*”). During non-business hours, the target CPU usage is 80% because backup software and other maintenance are expected to consume all available resources.

![CPU usage chart](media/capacity-planning-considerations-cpu-chart.png)

Analyzing the data in the chart (Processor Information(_Total)\% Processor Utility) for each of the DCs:

- For the most part, the load is relatively evenly distributed which is what would be expected when clients use DC locator and have well written searches. 
- There are a number of five-minute spikes of 10%, with some as large as 20%. Generally, unless they cause the capacity plan target to be exceeded, investigating these is not worthwhile.  
- The peak period for all systems is between about 8:00 AM and 9:15 AM. With the smooth transition from about 5:00 AM through about 5:00 PM, this is generally indicative of the business cycle. The more randomized spikes of CPU usage on a box-by-box scenario between 5:00 PM and 4:00 AM would be outside of the capacity planning concerns.

  >[!NOTE]
  >On a well-managed system, said spikes are might be backup software running, full system antivirus scans, hardware or software inventory, software or patch deployment, and so on. Because they fall outside the peak user business cycle, the targets are not exceeded.

- As each system is about 40% and all systems have the same numbers of CPUs, should one fail or be taken offline, the remaining systems would run at an estimated 53% (System D's 40% load is evenly split and added to System A's and System C’s existing 40% load). For a number of reasons, this linear assumption is NOT perfectly accurate, but provides enough accuracy to gauge.  

  **Alternate scenario –** Two domain controllers running at 40%: One domain controller fails, estimated CPU on the remaining one would be an estimated 80%. This far exceeds the thresholds outlined above for capacity plan and also starts to severely limit the amount of head room for the 10% to 20% seen in the load profile above, which means that the spikes would drive the DC to 90% to 100% during the “*N*” scenario and definitely degrade responsiveness.

### Calculating CPU demands

The “Process\\% Processor Time” performance object counter sums the total amount of time that all of the threads of an application spend on the CPU and divides by the total amount of system time that has passed. The effect of this is that a multi-threaded application on a multi-CPU system can exceed 100% CPU time, and would be interpreted VERY differently than “Processor Information\\% Processor Utility”. In practice the “Process(lsass)\\% Processor Time” can be viewed as the count of CPUs running at 100% that are necessary to support the process’s demands. A value of 200% means that 2 CPUs, each at 100%, are needed to support the full AD DS load. Although a CPU running at 100% capacity is the most cost efficient from the perspective of money spent on CPUs and power and energy consumption, for a number of reasons detailed in Appendix A, better responsiveness on a multi-threaded system occurs when the system is not running at 100%.

To accommodate transient spikes in client load, it is recommended to target a peak period CPU of between 40% and 60% of system capacity. Working with the example above, that would mean that between 3.33 (60% target) and 5 (40% target) CPUs would be needed for the AD DS (lsass process) load. Additional capacity should be added in according to the demands of the base operating system and other agents required (such as antivirus, backup, monitoring, and so on). Although the impact of agents needs to be evaluated on a per environment basis, an estimate of between 5% and 10% of a single CPU can be made. In the current example, this would suggest that between 3.43 (60% target) and 5.1 (40% target) CPUs are necessary during peak periods.

The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (perfmon), making sure all of the counters are scaled the same.

Assumptions:

- Goal is to reduce footprint to as few servers as possible. Ideally, one server would carry the load and an additional server added for redundancy (*N* + 1 scenario).

![Processor time chart for lsass process (over all processors)](media/capacity-planning-considerations-proc-time-chart.png)

Knowledge gained from the data in the chart (Process(lsass)\\% Processor Time):

- The business day starts ramping up around 7:00 and decreases at 5:00 PM.
- The peak busiest period is from 9:30 AM to 11:00 AM. 
  > [!NOTE]
  > All performance data is historical. The peak data point at 9:15 indicates the load from 9:00 to 9:15.
- There are spikes before 7:00 AM which could indicate either load from different time zones or background infrastructure activity, such as backups. Because the peak at 9:30 AM exceeds this activity, it is not relevant.
- There are three domain controllers in the site.

At maximum load, lsass consumes about 485% of one CPU, or 4.85 CPUs running at 100%. As per the math earlier, this means the site needs about 12.25 CPUs for AD DS. Add in the above suggestions of 5% to 10% for background processes and that means replacing the server today would need approximately 12.30 to 12.35 CPUs to support the same load. An environmental estimate for growth now needs to be factored in.

### When to tune LDAP weights

There are several scenarios where tuning [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) should be considered. Within the context of capacity planning, this would be done when the application or user loads are not evenly balanced, or the underlying systems are not evenly balanced in terms of capability. Reasons to do so beyond capacity planning are outside of the scope of this article.

There are two common reasons to tune LDAP Weights:

- The PDC emulator is an example that affects every environment for which user or application load behavior is not evenly distributed. As certain tools and actions target the PDC emulator, such as the Group Policy management tools, second attempts in the case of authentication failures, trust establishment, and so on, CPU resources on the PDC emulator may be more heavily demanded than elsewhere in the site.
  - It is only useful to tune this if there is a noticeable difference in CPU utilization in order  to reduce the load on the PDC emulator and increase the load on other domain controllers will allow a more even distribution of load.
  - In this case, set LDAPSrvWeight between 50 and 75 for the PDC emulator.
- Servers with differing counts of CPUs (and speeds) in a site.  For example, say there are two eight-core servers and one four-core server.  The last server has half the processors of the other two servers.  This means that a well distributed client load will increase the average CPU load on the four-core box to roughly twice that of the eight-core boxes.
  - For example, the two eight-core boxes would be running at 40% and the four-core box would be running at 80%.
  - Also, consider the impact of loss of one eight-core box in this scenario, specifically the fact that the four-core box would now be overloaded.

#### Example 1 - PDC

| |Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

The catch here is that if the PDC emulator role is transferred or seized, particularly to another domain controller in the site, there will be a dramatic increase on the new PDC emulator.

Using the example from the section [Target site behavior profile](#target-site-behavior-profile), an assumption was made that all three domain controllers in the site had four CPUs. What should happen, under normal conditions, if one of the domain controllers had eight CPUs? There would be two domain controllers at 40% utilization and one at 20% utilization. While this is not bad, there is an opportunity to balance the load a little bit better. Leverage LDAP weights to accomplish this.  An example scenario would be:

#### Example 2 - Differing CPU counts

| |Processor Information\\ %&nbsp;Processor Utility(_Total)<br />Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|4-CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

Be very careful with these scenarios though. As can be seen above, the math looks really nice and pretty on paper. But throughout this article, planning for an “*N* + 1” scenario is of paramount importance. The impact of one DC going offline must be calculated for every scenario. In the immediately preceding scenario where the load distribution is even, in order to ensure a 60% load during an “*N*” scenario, with the load balanced evenly across all servers, the distribution will be fine as the ratios stay consistent. Looking at the PDC emulator tuning scenario, and in general any scenario where user or application load is unbalanced, the effect is very different:

| |Tuned Utilization|New LdapSrvWeight|Estimated New Utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### Virtualization considerations for processing

There are two layers of capacity planning that need to be done in a virtualized environment. At the host level, similar to the identification of the business cycle outlined for the domain controller processing previously, thresholds during the peak period need to be identified. Because the underlying principals are the same for a host machine scheduling guest threads on the CPU as for getting AD DS threads on the CPU on a physical machine, the same goal of 40% to 60% on the underlying host are recommended. At the next layer, the guest layer, since the principals of thread scheduling have not changed, the goal within the guest remains in the 40% to 60% range.

In a direct mapped scenario, one guest per host, all the capacity planning done to this point needs to be added to the requirements (RAM, disk, network) of the underlying host operating system. In a shared host scenario, testing indicates that there is 10% impact on the efficiency of the underlying processors. That means if a site needs 10 CPUs at a target of 40%, the recommended amount of virtual CPUs to allocate across all the “*N*” guests would be 11. In a site with a mixed distribution of physical servers and virtual servers, the modifier only applies to the VMs. For example, if a site has an “*N* + 1” scenario, one physical or direct-mapped server with 10 CPUs would be about equivalent to one guest with 11 CPUs on a host, with 11 CPUs reserved for the domain controller.

Throughout the analysis and calculation of the CPU quantities necessary to support AD DS load, the numbers of CPUs that map to what can be purchased in terms physical hardware do not necessarily map cleanly. Virtualization eliminates the need to round up. Virtualization decreases the effort necessary to add compute capacity to a site, given the ease with which a CPU can be added to a VM. It does not eliminate the need to accurately evaluate the compute power needed so that the underlying hardware is available when additional CPUs need to be added to the guests.  As always, remember to plan and monitor for growth in demand.

### Calculation summary example

|System|Peak CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|Total CPU being used|485%|  
  
|Target system(s) count|Total bandwidth (from above)|
|-|-|
|CPUs needed at 40% target|4.85 &divide; .4 = 12.25|

Repeating due to the importance of this point, *remember to plan for growth*. Assuming 50% growth over the next three years, this environment will need 18.375 CPUs (12.25 &times; 1.5) at the three-year mark. An alternate plan would be to review after the first year and add in additional capacity as needed.

### Cross-trust client authentication load for NTLM

#### Evaluating cross-trust client authentication load

Many environments may have one or more domains connected by a trust. An authentication request for an identity in another domain that does not use Kerberos authentication needs to traverse a trust using the domain controller’s secure channel to another domain controller either in the destination domain or the next domain in the path to the destination domain. The number of concurrent calls using the secure channel that a domain controller can make to a domain controller in a trusted domain is controlled by a setting known as **MaxConcurrentAPI**. For domain controllers, ensuring that the secure channel can handle the amount of load is accomplished by one of two approaches: tuning **MaxConcurrentAPI** or, within a forest, creating shortcut trusts. To gauge the volume of traffic across an individual trust, refer to [How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](https://support.microsoft.com/kb/2688798).

During data collection, this, as with all the other scenarios, must be collected during the peak busy periods of the day for the data to be useful.

> [!NOTE]
> Intraforest and interforest scenarios may cause the authentication to traverse multiple trusts and each stage would need to be tuned.

#### Planning

There are a number of applications that use NTLM authentication by default, or use it in a certain configuration scenario. Application servers grow in capacity and service an increasing number of active clients. There is also a trend that clients keep sessions open for a limited time and rather reconnect on a regular basis (such as email pull sync). Another common example for high NTLM load is web proxy servers that require authentication for Internet access.

These applications can cause a significant load for NTLM authentication, which can put significant stress on the DCs, especially when users and resources are in different domains.

There are multiple approaches to managing cross-trust load, which in practice are used in conjunction rather than in an exclusive either/or scenario. The possible options are:

- Reduce cross-trust client authentication by locating the services that a user consumes in the same domain that the user is resident in.
- Increase the number of secure-channels available. This is relevant to intraforest and cross-forest traffic and are known as shortcut trusts.
- Tune the default settings for **MaxConcurrentAPI**.

For tuning **MaxConcurrentAPI** on an existing server, the equation is:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

For more information, see [KB article 2688798: How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](http://support.microsoft.com/kb/2688798).

## Virtualization considerations

None, this is an operating system tuning setting.

### Calculation summary example

|Data type|Value|
|-|-|
|Semaphore Acquires (Minimum)|6,161|
|Semaphore Acquires (Maximum)|6,762|
|Semaphore Timeouts|0|
|Average Semaphore Hold Time|0.012|
|Collection Duration (seconds)|1:11 minutes (71 seconds)|
|Formula (from KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Minimum value for **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

For this system for this time period, the default values are acceptable.

## Monitoring for compliance with capacity planning goals

Throughout this article, it has been discussed that planning and scaling go towards utilization targets. Here is a summary chart of the recommended thresholds that must be monitored to ensure the systems are operating within adequate capacity thresholds. Keep in mind that these are not performance thresholds, but capacity planning thresholds. A server operating in excess of these thresholds will work, but is time to start validating that all the applications are well behaved. If said applications are well behaved, it is time to start evaluating hardware upgrades or other configuration changes.

|Category|Performance counter|Interval/Sampling|Target|Warning|
|-|-|-|-|-|
|Processor|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 ou anterior)|MB de memória|< 100 MB|N/D|< 100 MB|
|RAM (Windows Server 2012)|Média de termo Memory\Long Lifetime(s) de Cache em espera|30 min|Deve ser testada|Deve ser testada|
|Rede|Interface de rede (\*) \Bytes enviados/s<br /><br />Interface de rede (\*) \Bytes recebidos/s|30 min|40%|60%|
|Armazenamento|Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg de disco s/leitura<br /><br />Disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg de disco s/gravação|60 min|10 ms|15 ms|
|Serviços do AD|Logon de rede (\*) \Average semáforo mantenha o tempo|60 min|0|1 segundo|

## <a name="appendix-a-cpu-sizing-criteria"></a>Apêndice a: Critério de dimensionamento de CPU

### <a name="definitions"></a>Definições

**Processador (microprocessador) –** um componente que lê e executa instruções de programa  

**CPU –** unidade de processamento Central  

**Com vários núcleo processador –** várias CPUs no mesmo circuito integrado  

**Com várias CPUs –** várias CPUs, não no mesmo circuito integrado  

**Processador lógico –** um mecanismo de computação lógico da perspectiva do sistema operacional  

Isso inclui a tecnologia hyper-threading, um principal em um processador de núcleo único ou vários núcleo processador.  

Como os sistemas de servidor atuais têm o hyperthreading, vários processadores de vários núcleos e vários processadores, essa informação é generalizada para cobrir ambos os cenários. Como tal, o processador lógico de termo será usado porque ele representa o sistema operacional e dos mecanismos de computação disponíveis da perspectiva do aplicativo.

### <a name="thread-level-parallelism"></a>Paralelismo em nível de thread

Cada thread é uma tarefa independente, pois cada thread tem sua própria pilha e instruções. Porque o AD DS é multi-threaded e o número de threads disponíveis pode ser ajustado por meio [como exibir e definir a política LDAP no Active Directory usando Ntdsutil.exe](http://support.microsoft.com/kb/315071), ele dimensiona bem em vários processadores lógicos.

### <a name="data-level-parallelism"></a>Paralelismo de nível de dados

Isso envolve a compartilhar dados entre vários threads dentro de um processo (no caso do processo de AD DS apenas) e em vários threads em vários processos (em geral). Com a preocupação em excesso, simplificar o caso, isso significa que as alterações nos dados são refletidas para todos os threads em execução em todos os vários níveis de cache (L1, L2, L3) entre todos os núcleos ditas threads em execução, bem como a atualização de memória compartilhada. Pode prejudicar o desempenho durante operações de gravação enquanto os vários locais de memória são ficarem consistentes, antes de continuar o processamento de instrução.

### <a name="cpu-speed-vs-multiple-core-considerations"></a>Velocidade de CPU versus considerações de vários núcleos

A regra geral é mais rápido processadores lógicos reduzem a duração necessária para processar uma série de instruções enquanto significa processadores mais lógica que mais tarefas podem ser executadas ao mesmo tempo. Essas regras de bolso dividir, conforme os cenários se tornam inerentemente mais complexos com considerações de busca de dados de memória compartilhada, aguardando o paralelismo em nível de dados e a sobrecarga de gerenciamento de vários threads. Isso também é por que não é linear de escalabilidade em sistemas de vários núcleos.

Considere as seguintes analogias essas considerações: processamento de uma estrada, com cada thread que está sendo um carro individual, cada Raia sendo um núcleo e o limite de velocidade que está sendo a velocidade do relógio.

1. Se houver apenas um carro na rodovia, não importa se houver duas pistas ou 12 pistas. Esse carro é só vai tão rápido quanto permitirá que o limite de velocidade.
1. Suponha que os dados que o thread precisa não estão imediatamente disponíveis. A analogia seria um segmento de estrada for desligada. Se houver apenas um carro na rodovia, não importa o que é o limite de velocidade até que a pista é reaberta (os dados são buscados da memória).
1. À medida que aumenta o número de carros, aumenta a sobrecarga para gerenciar o número de carros. Compare a experiência de condução e a quantidade de atenção necessária quando o caminho está praticamente vazio (por exemplo, como tarde da noite) versus quando o tráfego é pesado (como o intermediário tarde, mas não as horas de pico). Além disso, considere a quantidade de atenção necessária ao conduzir na estrada pista de dois, em que há apenas uma outra pista de se preocupar sobre o que os drivers estão fazendo, em vez de uma estrada de pista de seis onde é preciso se preocupar sobre quais muitos outros drivers estão fazendo.
   > [!NOTE]
   > A analogia sobre o cenário de hora do rush é estendida na próxima seção: Tempo de resposta/como a sistema agenda afeta o desempenho.

Como resultado, informações específicas sobre o mais rápidos ou mais processadores se tornar altamente subjetivas ao comportamento do aplicativo, que, no caso do AD DS, é muito ambientalmente específico e até mesmo varia de um servidor em um ambiente. É por isso as referências no início do artigo não investir pesadamente no que está sendo excessivamente precisos, e uma margem de segurança está incluída nos cálculos. Ao tomar decisões de compras orientadas a orçamento, é recomendável que otimiza o uso dos processadores em 40% (ou o número desejado para o ambiente) ocorre pela primeira vez, antes de considerar a compra de processadores mais rápidos. A sincronização maior entre processadores mais reduz o verdadeiro benefício dos processadores mais do que a progressão linear (2&times; o número de processadores fornece menor que 2&times; potência de computação adicionais disponíveis).

> [!NOTE]
> Lei de Amdahl e Gustafson estão aqui os conceitos relevantes.

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>Tempo de resposta/como a agenda do sistema afeta o desempenho

Teoria de enfileiramento de mensagens é o estudo de matemático de linhas de espera (filas). Em teoria de enfileiramento de mensagens, a lei de utilização é representada pela equação:

*U* k = *B* &divide; *T*

Em que *U* k é a porcentagem de utilização *B* é a quantidade de tempo ocupado, e *T* é o tempo total que o sistema foi observado. Convertido para o contexto do Windows, isso significa que o número de 100 nanossegundos (ns) threads de intervalo que estão em um estado de execução dividido por quantos intervalos de 100 ns estavam disponíveis em um determinado intervalo de tempo. Isso é exatamente a fórmula para calcular o utilitário do processador de % (referência [objeto processador](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) e [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Teoria de enfileiramento de mensagens também fornece a fórmula: *N* = *U* k &divide; (1 &ndash; *U* k) para estimar o número de itens de espera com base na utilização ( *N* é o comprimento da fila). Criação de gráficos isso em todos os intervalos de utilização fornece as estimativas a seguir para a fila para obter no processador quanto está em qualquer determinada carga de CPU.

![Comprimento da fila](media/capacity-planning-considerations-queue-length.png)

Ele é observado que após a carga de CPU de 50%, em média, sempre há uma espera de um outro item na fila, com um rápido visivelmente aumentar após cerca de 70% de utilização da CPU.

Retornando para a condução analogia usada anteriormente nesta seção:

- As épocas do "tarde intermediário", Hipoteticamente, estejam em algum lugar dentro do intervalo de 40% a 70%. Há tráfego suficiente, de modo que a capacidade de um para escolher qualquer pista não está restrita majorly e a possibilidade de outro driver sendo da forma, ao mesmo tempo alta, não exigem o nível de esforço para "localizar" uma lacuna segura entre a outros carros em trânsito.
- Um observará conforme o tráfego se aproxima de horas de pico, o sistema de estrada se aproxima 100% da capacidade. Alterar pistas pode se tornar muito difíceis porque carros são tão perto juntos que maior cuidado deve ser tomado para fazê-lo.

Isso é por isso que calcula a média de longo prazo para capacidade de forma prudente estimada em 40% permite a sala principal anormal picos de carga, não importando se disse a picos transitórios (como consultas mal codificadas que são executados por alguns minutos) ou anormais em geral intermitências de carga (da manhã de o primeiro dia após um longo final de semana).

A instrução acima considera o cálculo de tempo do processador de % sendo as mesmas, conforme a lei de utilização é um pouco de uma simplificação a facilidade do leitor geral. Para esses matematicamente mais rigorosos:  
- Convertendo o [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = o número de intervalos de 100 ns "Ocioso" thread gasta no processador lógico. A alteração no valor de "*X*" variável na [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) cálculo
  - *T* = o número total de intervalos de 100 ns em um intervalo de tempo determinado. A alteração no valor de "*Y*" variável na [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) cálculo.
  - *U* k = a porcentagem de utilização do processador lógico pelo "Thread ocioso" ou % tempo ocioso.  
- Estimar a matemática:
  - *U* k = 1-% tempo do processador
  - % Tempo do processador = 1 – *U* k
  - % Tempo do processador = 1 – *B* / *T*
  - % Tempo do processador = 1 – *X1* – *X0* / *Y1* – *Y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>Aplicando os conceitos para planejamento de capacidade

O cálculo anterior pode fazer determinações sobre o número de processadores lógicos necessários em um sistema ser terrivelmente complexas. Isso ocorre porque a abordagem para os sistemas de dimensionamento destina-se sobre como determinar a utilização máxima de destino com base no atual carga e calcular o número de processadores lógicos necessárias para chegar lá. Além disso, enquanto as velocidades de processador lógico terá um impacto significativo no desempenho, a eficiência do cache, requisitos de coerência de memória, agendamento de threads e sincronização e o cliente de modo imperfeito com balanceamento de carga terão um impacto significativo desempenho varia em uma base de servidor a servidor. Com o custo relativamente barato de computação, a tentativa de analisar e determinar o número perfeito de CPUs necessário se torna mais um exercício acadêmico que ele fornecer valor comercial.

Quarenta por cento não é um requisito definitiva e cabal, é um começo razoável. Vários consumidores do Active Directory exigem vários níveis de capacidade de resposta. Pode haver situações em que os ambientes podem executado a 80% ou 90% da utilização como uma média sustentada, enquanto os tempos de espera maior para o acesso para o processador não afetará visivelmente o desempenho do cliente. É importante reiterar o que há muitas áreas do sistema que são muito mais lento do que o processador lógico no sistema, incluindo o acesso a RAM, acesso a disco e transmitir a resposta pela rede. Todos esses itens precisam ser ajustados em conjunto. Exemplos:

- Adicionar mais processadores para um sistema que executa a 90% é limitado ao disco é provavelmente não vai melhorar significativamente o desempenho. Uma análise mais profunda do sistema provavelmente será identificar que há muitos threads que estão nem mesmo se no processador porque estão aguardando e/s para concluir.
- Resolução de problemas de disco associado potencialmente significa que os threads que anteriormente estavam gastando muito tempo em um estado de espera não poderão mais ser em um estado de espera de e/s e haverá mais competição por tempo de CPU, o que significa que a 90% da utilização na anterior exemplo irá para 100% (porque ele não pode ir mais alta). Os dois componentes precisam ser ajustados em conjunto.
  > [!NOTE]
  > Processador Information(*)\\% utilitário processador pode exceder 100% com sistemas que têm um modo de "Turbo".  Isso é onde a CPU excede a velocidade do processador classificados por curtos períodos.  Documentação do fabricante da CPU de referência e uma descrição do contador para uma visão mais clara.  

Também é discutir considerações de utilização de todo o sistema coloca nos controladores de domínio de conversa como convidados virtualizados. [Tempo de resposta/como a agenda do sistema afeta o desempenho](#response-timehow-the-system-busyness-impacts-performance) aplica-se ao host e convidado em um cenário virtualizado. Isso é por que, em um host com apenas um convidado, um controlador de domínio (e geralmente qualquer sistema) tem quase o mesmo desempenho em hardware físico. Adicionar convidados adicionais para os hosts aumenta a utilização do host subjacente, aumentando assim os tempos de espera para obter acesso aos processadores, conforme explicado anteriormente. Em resumo, a utilização do processador lógico precisa ser gerenciado em ambos os host e nos níveis de convidado.

Estendendo as analogias anteriores, deixando a via expressa como o hardware físico, o convidado VM será analogized com barramento (quer um barramento express que vai direto para o destino o rider). Imagine quatro cenários a seguir:

- É fora do horário comercial, obtém de uma cláusula adicional em um barramento que está quase vazio e o barramento obtém em uma estrada que também está praticamente vazia. Pois não há nenhum tráfego para enfrentar, a cláusula adicional tem uma BOM jornada fácil e obtém existe apenas mais rápido como se a cláusula adicional havia dirigido em vez disso. Tempos de viagem do rider ainda são restringidos pelo limite de velocidade.
- Ele é fora do horário comercial para que o barramento é quase vazio, mas a maioria das pistas em trânsito está fechada, portanto, rodovia ainda estiver congestionada. A cláusula adicional é em um barramento quase vazio em uma estrada congestionada. Enquanto o rider não tem muita concorrência no barramento de para onde ficam, o tempo de viagem total ainda é determinado pelo resto do tráfego de fora.
- É hora do rush rodovia e o barramento estiverem congestionados. Não apenas faz o processamento levar mais tempo, mas Obtendo e desativar o barramento é um pesadelo porque as pessoas estão shoulder para cima do ombro e rodovia não é muito melhor. Adicionar mais barramentos (processadores lógicos para o convidado) não significa que eles se adaptam na estrada qualquer um com mais facilidade, ou que será reduzida a viagem.
- O cenário final, embora talvez ele ser alongando a analogia um pouco, é onde o barramento é completo, mas o caminho não está congestionado. Enquanto o rider ainda tenha problemas para obter e desativar o barramento, a viagem será eficiente depois que o barramento estiver em trânsito. Isso é o único cenário em que a adição de mais barramentos (processadores lógicos para o convidado) irá melhorar o desempenho do convidado.

A partir daí é relativamente fácil extrapolar o que há vários cenários entre os utilizados de 0% e o estado utilizados para 100% do caminho e o estado 0% e 100% utilizados do barramento que possuem níveis variados de impacto.

Aplicar os princípios acima de 40% de CPU como destino razoável para o host, bem como o convidado é um começo razoável para o mesmo raciocínio como acima, a quantidade de enfileiramento de mensagens.

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>Apêndice B: Considerações sobre velocidades de processador diferentes e o efeito de gerenciamento de energia do processador em velocidades de processador

Em todo as seções na seleção do processador é feita a suposição que o processador estiver executando em 100% da velocidade de clock de todo o tempo que os dados estão sendo coletados e que os sistemas de substituição terão os mesmos processadores de velocidade. Apesar de ambas as suposições na prática, sendo falso, particularmente com o Windows Server 2008 R2 e versões posteriores, onde é o plano de energia padrão **equilibrado**, a metodologia ainda significa que é a abordagem conservadora. Enquanto a taxa de erros potenciais pode aumentar, ele só aumenta a margem de segurança como aumento de velocidade do processador.

- Por exemplo, em um cenário em que as 11,25 CPUs são exigidas, se os processadores estavam executando a meia velocidade quando os dados foram coletados, a estimativa mais precisa pode ser 5.125 &divide; 2.
- É impossível garantir que dobrar as velocidades de relógio seria o dobro da quantidade de processamento que ocorre para um determinado período de tempo. Isso é devido ao fato do período de tempo que os processadores gastam aguardando a RAM ou outros componentes do sistema podem permanecer o mesmo. O efeito líquido é que os processadores mais rápidos podem gastar uma porcentagem maior do tempo ocioso enquanto estiver aguardando dados a ser obtido. Novamente, é recomendável escolher o menor no denominador comum sendo conservadora, e evite tentar calcule um nível potencialmente false de precisão, supondo que uma comparação linear entre velocidades de processador.

Como alternativa, se as velocidades de processador em hardware de substituição são menores do que o hardware atual, seria seguro aumentar a estimativa de processadores necessários para uma quantidade proporcional. Por exemplo, ele é calculado que 10 processadores são necessários para sustentar a carga em um site e os processadores atuais estão sendo executadas em 3,3 Ghz e os processadores de substituição serão executados a 2.6 Ghz, isso é uma diminuição de 21% na velocidade. Nesse caso, 12 processadores seria a quantidade recomendada.

Dito isso, essa variação não alteraria os destinos de utilização do processador de gerenciamento de capacidade. Conforme as velocidades de clock de processador serão ajustadas dinamicamente com base na carga exigida, executando o sistema sob cargas maiores irá gerar um cenário onde a CPU efetivamente gasta mais tempo em um estado de velocidade de clock mais alta, fazendo o objetivo final seja a 40% de utilização em 100% estado de velocidade do relógio no pico. Qualquer coisa menor do que irá gerar economia de energia como velocidades de CPU será reduzida durante o logoff cenários de pico.

> [!NOTE]
> Seria uma opção Desativar o gerenciamento de energia nos processadores (definindo o plano de energia como **de alto desempenho**) enquanto os dados são coletados. Isso daria uma representação mais precisa de consumo da CPU no servidor de destino.

Para ajustar as estimativas de processadores diferentes, que costumava ser seguro, excluindo outros gargalos do sistema descritos acima, assuma que dobrar velocidades de processador dobrou a quantidade de processamento que poderia ser executada.  Hoje, a arquitetura interna de processadores é diferente entre os processadores, o que é de uma maneira mais segura para medir os efeitos de usar diferentes processadores de dados foi retirados do utilizar o parâmetro de comparação SPECint_rate2006 da avaliação de desempenho padrão Corporation.

1. Encontre as pontuações de SPECint_rate2006 para o processador que plano a ser usado e que estão em uso.
    1. No site do Standard Performance Evaluation Corporation, selecione **resultados**, realce **CPU2006**e selecione **pesquisar todos os resultados SPECint_rate2006**.
    1. Sob **simples de solicitação**, insira os critérios de pesquisa para o processador de destino, por exemplo **E5-2630 (baselinetarget) do processador correspondências** e **processador correspondências E5-2650 (linha de base)** .
    1. Localizar a configuração de servidor e processador a ser usado (ou algo fechar, se uma correspondência exata não estiver disponível) e observe o valor na **resultado** e **# núcleos** colunas.
1. Para determinar o modificador de usar a seguinte equação:
   >((*Valor de pontuação de-core de plataforma de destino*) &times; (*MHz por núcleo da plataforma de linha de base*)) &divide; ((*linha de base de valor de pontuação por núcleo*) &times; (*MHz por núcleo da plataforma de destino*))  

    Usando o exemplo acima:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multiplique o número estimado de processadores pelo modificador.  No caso acima para ir do E5-2650 processador para o processador de 2630 E5 multiplicar as 11,25 CPUs calculadas &times; 0,92 = 10,35 processadores necessário.

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>Apêndice C: Conceitos básicos sobre o sistema operacional interagindo com o armazenamento

Os conceitos de teoria de enfileiramento de mensagens descritas [tempo de resposta/como a agenda do sistema afeta o desempenho](#response-timehow-the-system-busyness-impacts-performance) também são aplicáveis para o armazenamento. Tendo uma familiaridade de como os identificadores do sistema operacional e/s é necessária para aplicar esses conceitos. No sistema operacional Microsoft Windows, uma fila para reter as solicitações de e/s é criada para cada disco físico. No entanto, um esclarecimento no disco físico precisa ser feita. Controladores de matriz e SANs apresentam agregações de eixos para o sistema operacional como uma única discos físicos. Além disso, os controladores de matriz e SANs podem agregar vários discos em uma matriz conjunto e, em seguida, dividir essa matriz definida em várias "partições", que por sua vez é apresentado para o sistema operacional como vários discos físicos (Figura ref.).

![Bloquear eixos](media/capacity-planning-considerations-block-spindles.png)  

Nesta figura dois eixos são espelhados e divididos em áreas lógicas para o armazenamento de dados (Data 1 e 2 de dados). Essas áreas lógicas são exibidas pelo sistema operacional, como discos físicos separados.

Embora isso possa ser altamente confuso, a seguinte terminologia é usada em todo este apêndice para identificar as diferentes entidades:

- **Eixo –** o dispositivo que está fisicamente instalado no servidor.
- **Matriz –** uma coleção de eixos agregados pelo controlador.
- **Matriz de partição –** um particionamento da matriz agregada
- **LUN –** uma matriz, usada para se referir a SANs
- **Disco –** que observa o sistema operacional para ser um único disco físico.
- **Partição –** um particionamento lógico do qual o sistema operacional percebe como um disco físico.

### <a name="operating-system-architecture-considerations"></a>Considerações de arquitetura do sistema operacional

O sistema operacional cria uma fila In/First PEPS (primeiro) e/s para cada disco que é observado; Esse disco pode ser que representa um eixo, uma matriz ou uma partição de matriz. Da perspectiva do sistema operacional, com relação ao tratamento de e/s, quanto mais ativo enfileira melhor. Como uma fila FIFO é serializada, que significa que todas as e/SS emitidas para o subsistema de armazenamento deve ser processada na ordem a solicitação chegou. Correlacionando cada disco observado pelo sistema operacional com um eixo/matriz, o sistema operacional agora mantém uma fila de e/s para cada conjunto exclusivo de discos, assim eliminando a contenção de recursos escassos de e/s entre discos e isolar a demanda de e/s para um único disco. Como uma exceção, o Windows Server 2008 introduz o conceito de priorização de e/s, e aplicativos projetados para usar a prioridade de "Baixa" deixam de manter essa ordem normal e tomar relegadas a segundo plano. Aplicativos codificados não especificamente para aproveitar o padrão "Baixa" prioridade "Normal".

### <a name="introducing-simple-storage-subsystems"></a>Apresentando os subsistemas de armazenamento simples

Começando com um exemplo simples (um único disco rígido dentro de um computador) uma análise de componente a componente será oferecida. Dividindo isso os componentes do subsistema de armazenamento principal, o sistema consiste em:

- **1 –** 10.000 RPM Ultra Fast SCSI HD (Ultra SCSI rápida tem uma taxa de transferência de 20 MB/s)
- **1 –** barramento SCSI (o cabo)
- **1 –** adaptador SCSI rápidas ultra
- **1 –** barramento PCI 33 MHz 32 bits

Depois que os componentes são identificados, uma ideia de quantos dados podem transportar o sistema ou a quantidade e/s pode ser tratada, pode ser calculada. Observe que a quantidade de e/s e a quantidade de dados que podem transportar o sistema estão correlacionados, mas não o mesmo. Essa correlação depende se a e/s de disco é aleatória ou sequencial e o tamanho do bloco. (Todos os dados são gravados no disco como um bloco, mas diferentes aplicativos que usam diferentes tamanhos de bloco). Em uma base de componente a componente:

- **A unidade de disco rígido** Média 10.000 RPM rígido tem um milissegundo de 7 (ms) tempo de busca e uma hora de acesso 3 ms. Busca de tempo é a quantidade média de tempo que leva a cabeça de leitura/gravação para passar para um local na superfície. Tempo de acesso é a quantidade média de tempo que leva para ler ou gravar os dados em disco, depois que o cabeçalho está no local correto. Portanto, o tempo médio para a leitura de um único bloco de dados em um HD de 10.000 RPM constitui um acesso, para um total de aproximadamente 10 ms (ou.010 segundos) e uma busca por um bloco de dados.

  Quando cada acesso ao disco requer a movimentação do cabeçote de para um novo local no disco, o comportamento de leitura/gravação é conhecido como "aleatórios". Assim, quando todas as e/s é aleatória, um HD de 10.000 RPM pode lidar com aproximadamente 100 e/s por segundo (IOPS) (a fórmula é 1000 ms por segundo dividido por 10 ms por e/s ou 1000/10 = 100 IOPS).

  Como alternativa, quando todas as e/s ocorre em setores adjacentes no HD, isso é chamado como e/s sequencial. E/s sequencial não tem nenhum tempo de busca porque quando a primeira e/s for concluída, a cabeça de leitura/gravação no início de onde o próximo bloco de dados é armazenado no HD. Assim, um HD de 10.000 RPM é capaz de lidar com aproximadamente 333 e/s por segundo (1000 ms por segundo dividido por 3 ms por e/s).

  >[!NOTE]
  >Este exemplo não reflete o cache de disco, em que os dados de um cilindro normalmente são mantidos. Nesse caso, 10 ms são necessários na e/s primeiro e o disco lê o cilindro inteiro. Todos os outros e/s sequencial será satisfeita do cache. Como resultado, os caches em disco podem melhorar o desempenho de e/s sequencial.
  
  Até agora, a taxa de transferência do disco rígido tiver sido irrelevante. Se o disco rígido é 20 MB/s Ultra largo ou uma Ultra3 160 MB/s, a quantidade real de IOPS o pode ser tratada pelo HD de 10.000 RPM é aproximadamente 100 aleatório ou e/s sequencial de ~ 300. Como alterar de tamanhos de bloco com base no aplicativo gravar na unidade, a quantidade de dados que são obtidos por e/s é diferente. Por exemplo, se o tamanho do bloco é de 8 KB, 100 operações de e/s ler ou gravar no disco rígido de um total de 800 KB. No entanto, se o tamanho do bloco é 32 KB, 100 e/s será leitura/gravação KB 3.200 (3,2 MB) para o disco rígido. Desde que a taxa de transferência de SCSI é superior ao valor total dos dados transferidos, obter uma transferência "mais rápida" unidade de taxa obterá nada. Consulte as tabelas a seguir para comparação.

  | |Busca de 7200 RPM 9ms, 4ms de acesso|Busca de 10.000 RPM 7ms, 3ms de acesso|Busca de 15.000 RPM 4ms, acesso a 2 MS
  |-|-|-|-|
  |E/s aleatória|80|100|150|
  |E/s sequencial|250|300|500|  
  
  |Unidade de 10.000 RPM|Tamanho do bloco de 8 KB (Active Directory Jet)|
  |-|-|
  |E/s aleatória|800 KB/s|
  |E/s sequencial|2400 KB/s|

- **Backplane do SCSI (barramento) –** Noções básicas sobre como o "backplane SCSI (barramento)", ou neste cenário, o cabo de faixa de opções, taxa de transferência de impactos de subsistema de armazenamento depende do conhecimento do tamanho do bloco. Essencialmente, a pergunta seria, quanto a e/s pode o identificador de barramento, se a e/s está em blocos de 8 KB? Nesse cenário, o barramento SCSI é 20 MB/s ou 20480 KB/s. 20480 KB/s dividida por blocos de 8 KB rendem um máximo de aproximadamente IOPS de 2500 compatíveis com o barramento SCSI.

  >[!NOTE]
  >Os números na tabela a seguir representam um exemplo. Mais dispositivos de armazenamento conectados no momento, usam PCI Express, que fornece a taxa de transferência mais alta.  
  
  |Com suporte pelo barramento SCSI por tamanho do bloco de e/s|Tamanho do bloco de 2 KB|Tamanho do bloco de 8 KB (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  Como pode ser determinada neste gráfico, no cenário apresentado, independentemente do uso, o barramento nunca mais poderá ser um gargalo, como o eixo máximo é 100 e/s, bem abaixo de qualquer um dos limites acima.

  >[!NOTE]
  >Isso pressupõe que o barramento SCSI é 100% eficiente.
  
- **Adaptador de SCSI –** para determinar a quantidade de e/s que isso pode manipular, as especificações do fabricante precisam ser verificados. Direcionamento das solicitações de e/s para o dispositivo apropriado requer processamento de algum tipo, assim, a quantidade de e/s que podem ser manipulados depende o adaptador SCSI (ou controlador de matriz) processador.

  Neste exemplo, a suposição de que 1.000 pode de e/s ser manipulados será feita.

- **Barramento PCI –** este é um componente que é frequentemente desconsiderado. Neste exemplo, isso não será o gargalo; No entanto, sistemas de escalar verticalmente, ele pode se tornar um gargalo. Para referência, um operando em 33Mhz 32 bits de barramento PCI pode na transferência de teoria 133 MB/s de dados. A seguir está a equação:  
  > 32 bits &divide; 8 bits por byte &times; 33 MHz = 133 MB/s.  

  Observe que o limite teórico; Na verdade apenas cerca de 50% do máximo seja alcançado, embora a eficiência de 75% em determinados cenários de intermitente, pode ser obtida por curtos períodos.

  Um barramento PCI 66 Mhz 64-bit pode dar suporte a uma velocidade máxima teórica de (64 bits &divide; 8 bits por byte &times; 66 Mhz) = 528 MB/s. Além disso, qualquer outro dispositivo (como o adaptador de rede, segundo controlador SCSI e assim por diante) reduz a largura de banda disponível como a largura de banda é compartilhada e os dispositivos serão disputam os recursos limitados.

Após a análise dos componentes desse subsistema de armazenamento, o eixo é o fator limitante na quantidade de e/s que pode ser solicitada e, consequentemente a quantidade de dados que podem transportar o sistema. Especificamente, em um cenário de AD DS, isso é 100 e/s aleatória por segundo em incrementos de 8 KB para um total de 800 KB por segundo quando acessar o banco de dados Jet. Como alternativa, a taxa de transferência máxima para um eixo que é alocada exclusivamente para arquivos de log seria afetado as seguintes limitações: 300 sequencial e/s por segundo em incrementos de 8 KB para um total de KB 2400 (2,4 MB) por segundo.

Agora, tendo analisados uma configuração simples, a tabela a seguir demonstra onde o gargalo ocorrerá como componentes no subsistema de armazenamento são alterados ou adicionados.

|Observações|Análise de afunilamento|Disco|Bus|Adaptador|Barramento PCI|
|-|-|-|-|-|-|
|Isso é a configuração do controlador de domínio depois de adicionar um segundo disco. A configuração de disco representa o afunilamento a 800 KB/s.|Adicione 1 disco (Total = 2)<br /><br />E/s é aleatória<br /><br />Tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM|200 e/SS total<br />Total de 800 KB/s.| | | |
|Depois de adicionar 7 discos, a configuração de disco ainda representa o afunilamento a 3200 KB/s.|**Adicionar discos de 7 (Total = 8)**  <br /><br />E/s é aleatória<br /><br />Tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM|800 e/SS total.<br />Total do 3200 KB/s| | | |
|Depois de alterar a e/s para sequencial, o adaptador de rede se torna o gargalo porque ele é limitado a 1000 IOPS.|Adicionar discos de 7 (Total = 8)<br /><br />**E/s é sequencial**<br /><br />Tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM| | |S de e/s 2400 podem ser lidos/gravados em disco, controlador limitadas a 1000 IOPS| |
|Depois de substituir o adaptador de rede com um adaptador SCSI que dá suporte a 10.000 IOPS, o gargalo retorna para a configuração de disco.|Adicionar discos de 7 (Total = 8)<br /><br />E/s é aleatória<br /><br />Tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM<br /><br />**Atualizar adaptador SCSI agora dá suporte a 10.000 e/s)**|800 e/SS total.<br />Total de 3.200 KB/s| | | |
|Depois de aumentar o tamanho do bloco para 32 KB, o barramento se torna o gargalo pois suporta apenas 20 MB/s.|Adicionar discos de 7 (Total = 8)<br /><br />E/s é aleatória<br /><br />**Tamanho do bloco de 32 KB**<br /><br />HD DE 10.000 RPM| |800 e/SS total. 25,600 KB/s (25 MB/s) podem ser lidos/gravados em disco.<br /><br />O barramento só oferece suporte a 20 MB/s| | |
|Depois de atualizar o barramento e adicionar mais discos, o disco permanece o gargalo.|**Adicionar 13 discos (Total = 14)**<br /><br />Adicionar um segundo adaptador de SCSI com 14 discos<br /><br />E/s é aleatória<br /><br />Tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM<br /><br />**Atualizar para o barramento de SCSI de MB/s 320**|2800 I/Os<br /><br />11,200 KB/s (10.9 MB/s)| | | |
|Depois de alterar a e/s para sequencial, o disco permanece o gargalo.|Adicionar 13 discos (Total = 14)<br /><br />Adicionar um segundo adaptador de SCSI com 14 discos<br /><br />**E/s é sequencial**<br /><br />Tamanho do bloco de 4 KB<br /><br />HD DE 10.000 RPM<br /><br />Atualizar para o barramento de SCSI de MB/s 320|E/8,400 SS<br /><br />33.600 KB\s<br /><br />(32,8 MB\s)| | | |
|Depois de adicionar discos rígidos com mais rapidez, o disco permanece o gargalo.|Adicionar 13 discos (Total = 14)<br /><br />Adicionar um segundo adaptador de SCSI com 14 discos<br /><br />E/s é sequencial<br /><br />Tamanho do bloco de 4 KB<br /><br />**HD DE 15.000 RPM**<br /><br />Atualizar para o barramento de SCSI de MB/s 320|E/14.000 SS<br /><br />56.000 KB/s<br /><br />(54.7 MB/s)| | | |
|Depois de aumentar o tamanho do bloco para 32 KB, o barramento PCI se torna o gargalo.|Adicionar 13 discos (Total = 14)<br /><br />Adicionar um segundo adaptador de SCSI com 14 discos<br /><br />E/s é sequencial<br /><br />**Tamanho do bloco de 32 KB**<br /><br />HD DE 15.000 RPM<br /><br />Atualizar para o barramento de SCSI de MB/s 320| | | |E/14.000 SS<br /><br />448.000 KB/s<br /><br />(437 MB/s) é o limite de leitura/gravação para o eixo.<br /><br />O barramento PCI dá suporte a uma velocidade máxima teórica de 133 MB/s (75% eficiente na melhor das hipóteses).|

### <a name="introducing-raid"></a>Introdução ao RAID

A natureza de um subsistema de armazenamento não altera drasticamente quando um controlador de matriz é introduzido; Ele substitui apenas o adaptador SCSI nos cálculos. O que é alterado é o custo de leitura e gravação de dados para o disco ao usar os vários níveis de matriz (como RAID 0, RAID 5 ou RAID 1).

RAID 0, os dados são distribuídos em todos os discos de RAID definidos. Isso significa que, durante uma leitura ou uma operação de gravação, uma parte dos dados é extraída de ou enviado por push para cada disco, aumentar a quantidade de dados que podem transportar o sistema durante o mesmo período de tempo. Assim, em um segundo, em cada eixo (novamente supondo que as unidades de 10.000 RPM), 100 operações de e/s podem ser executadas. A quantidade total de e/s que podem ser suportados é eixos N vezes 100 e/s por segundo por eixo (produz 100 * N e/s por segundo).

![Unidade d lógico:](media/capacity-planning-considerations-logical-d-drive.png)

No RAID 1, os dados são espelhados (duplicado) entre um par de eixos para redundância. Assim, quando uma operação de e/s de leitura é executada, os dados podem ser lidos para ambos os eixos no conjunto. Isso torna a capacidade de e/s de ambos os discos disponíveis durante uma operação de leitura. A limitação é que não escrever o ganho de operações nenhuma vantagem de desempenho em um RAID 1. Isso ocorre porque os mesmos dados precisam ser escrito para ambas as unidades para fins de redundância. Embora ela não leva mais tempo, à medida que a gravação de dados ocorre simultaneamente em ambos os eixos, porque ambos os eixos estiverem ocupados duplicar os dados, uma operação de e/s de gravação impede que basicamente duas operações de leitura ocorra. Portanto, todos os custos de e/s de gravação dois leitura e/s. Uma fórmula pode ser criada a partir dessas informações para determinar o número total de operações de e/s que estão ocorrendo:  

> *E/s de leitura* + 2 &times; *e/s de gravação* = *Total de e/s de disco disponível consumido*  

Quando a taxa de leituras para gravações e o número de eixos são conhecidos, a equação a seguir pode ser derivada da equação acima para identificar a e/s máximo que pode ser compatível com a matriz:  

> *IOPS máximo por eixo* &times; 2 eixos &times; [( *% de leituras* +  *% de gravações*) &divide; ( *% de leituras* + 2 &times; *% de gravações*)] = *IOPS Total*

RAID 1 + 0, se comporta exatamente como RAID 1 em relação a despesa de leitura e gravação. No entanto, a e/s agora é distribuído em cada conjunto espelhado. Se  

> *IOPS máximo por eixo* &times; 2 eixos &times; [( *% de leituras* +  *% de gravações*) &divide; ( *% de leituras* + 2 &times; *% de gravações*)] = *Total de e/s*  

em um conjunto RAID 1, quando uma multiplicidade (*N*) do RAID 1 conjuntos são distribuídos, a e/s Total que pode ser processado se torna N &times; e/s por conjunto RAID 1:  

> *N* &times; {*IOPS máximo por eixo* &times; 2 eixos &times; [( *% de leituras* +  *% de gravações*) &divide; ( *% De leituras* + 2 &times; *% de gravações*)]} = *IOPS Total*

RAID 5, às vezes, conhecido como *N* + 1 RAID, os dados são distribuídos entre *N* eixos e paridade informações são gravadas para o eixo "+ 1". No entanto, o RAID 5 é muito mais caro ao executar uma e/s de gravação que RAID 1 ou 1 + 0. RAID 5 executa o processo a seguir sempre que uma e/s de gravação é enviado para a matriz:

1. Ler os dados antigos
1. Leia a paridade antiga
1. Gravar os novos dados
1. Gravar a nova paridade

Como cada solicitação de e/s de gravação que é enviada para o controlador de matriz pelo sistema operacional exige quatro operações de e/s para concluir, as solicitações de gravação enviadas levarem quatro vezes para concluir uma única leitura de e/s. Para derivar uma fórmula para traduzir as solicitações de e/s da perspectiva do sistema operacional para que experimentada pelos eixos:  

> *E/s de leitura* + 4 &times; *e/s de gravação* = *Total de e/s*  

Da mesma forma em um conjunto RAID 1, quando a taxa de leituras, gravações e o número de eixos são conhecidos, a equação a seguir pode ser derivada da equação acima para identificar a e/s máximo que pode ser compatível com a matriz (Observe que não inclui o número total de eixos e "unidade" são perdidas para paridade):  

> *IOPS por eixo* &times; (*eixos* – 1) &times; [( *% de leituras* +  *% de gravações*) &divide; ( *% De leituras* + 4 &times; *% de gravações*)] = *IOPS Total*

### <a name="introducing-sans"></a>Introdução ao SANs

Expandindo a complexidade do subsistema de armazenamento, quando uma rede SAN é introduzida no ambiente, os princípios básicos descritos não alteram, no entanto o comportamento de e/s para todos os sistemas conectados à SAN precisam ser levada em conta. Como uma das principais vantagens no uso de uma SAN é uma quantidade adicional de redundância em relação ao armazenamento anexada interna ou externamente, planejamento de capacidade agora precisa levar em conta com suas necessidades de tolerância. Além disso, são apresentados os mais componentes que precisam ser avaliadas. Dividir uma rede SAN em partes componentes:

- Disco rígido SCSI ou Fibre Channel
- Backplane de canal de unidade de armazenamento
- Unidades de armazenamento
- Módulo do controlador de armazenamento
- Switches de SAN
- HBA(s)
- O barramento PCI

Durante a criação de qualquer sistema para redundância, os componentes adicionais estão incluídos para acomodar o potencial de falha. É muito importante, ao planejamento de capacidade, para excluir o componente com redundância de recursos disponíveis. Por exemplo, se a SAN tem dois módulos de controlador, a capacidade de e/s do módulo de um controlador é tudo o que deve ser usado para transferência de e/s total disponível para o sistema. Isso é devido ao fato de que se um controlador falhar, a carga inteira de e/s exigida por todos os sistemas conectados precisará ser processada pelo controlador de restantes. Como todo o planejamento de capacidade é feito para períodos de pico de uso, com redundância de componentes não devem ser separados em recursos disponíveis e planejadas de pico de utilização não deve exceder 80% de saturação do sistema (a fim de acomodar picos ou anômalo do sistema comportamento). Da mesma forma, o comutador com redundância de SAN, unidade de armazenamento e as eixos não devem ser decompostos em cálculos de e/s.

Ao analisar o comportamento do disco rígido SCSI ou Fibre Channel, o método de analisar o comportamento conforme descrito anteriormente não é alterada. Embora existam certas vantagens e desvantagens para cada protocolo, o fator limitante em uma base por disco é a limitação mecânica do disco rígido.

Analisar o canal na unidade de armazenamento é exatamente o mesmo que calcular os recursos disponíveis no barramento SCSI ou largura de banda (por exemplo, 20 MB/s) dividido pelo tamanho do bloco (por exemplo, 8 KB). Onde isso se desvia do exemplo anterior simple é a agregação de vários canais. Por exemplo, se houver 6 canais, cada um oferecendo suporte a taxa de transferência máxima do 20 MB/s, a quantidade total de e/s e transferência de dados que está disponível é 100 MB/s (isso está correto, ele é não 120 MB/s). Novamente, tolerância a falhas é muito importante nesse cálculo, no caso de perda de um canal inteiro, o sistema é esquerda apenas com 5 canais está funcionando. Portanto, para garantir que continua a atender às expectativas de desempenho no caso de falha, taxa de transferência total para todos os canais de armazenamento não deve exceder 100 MB/s (Isso pressupõe carga e tolerância a falhas é distribuída igualmente entre todos os canais). Transformar isso em um perfil de e/s é dependente do comportamento do aplicativo. No caso do Active Directory Jet e/s, isso estaria correlacionado a aproximadamente 12,500 e/s por segundo (100 MB/s &divide; 8 KB por e/s).

Em seguida, obter as especificações do fabricante para os módulos do controlador é necessária para obter uma compreensão da taxa de transferência que pode dar suporte a cada módulo. Neste exemplo, a SAN tem dois módulos do controlador que suporte 7.500 e/s cada. A taxa de transferência total do sistema pode ser 15.000 IOPS se redundância não for desejada. Calcular a taxa de transferência máxima em caso de falha, a limitação é a taxa de transferência de um controlador ou 7.500 IOPS. Esse limite está abaixo do máximo IOPS (supondo que o tamanho do bloco de 4 KB) 12.500 que pode ter suporte por todos os canais de armazenamento e assim, no momento é o gargalo na análise. Ainda, para fins de planejamento, a e/s máximo desejado para ser planejado seria 10.400 e/s.

Quando os dados é encerrado para o módulo do controlador, que atravessa uma conexão de Fibre Channel classificada como 1 GB/s (ou 1 Gigabit por segundo). Para correlacionar isso com as outras métricas, 1 GB/s se transforma em 128 MB/s (1 GB/s &divide; Mbps)*(8). Como esse é o que excede a largura de banda total em todos os canais na unidade de armazenamento (100 MB/s), isso será não causar um gargalo no sistema. Além disso, pois essa é apenas um dos dois canais (o mais 1 GB/s Fibre Channel conexão sendo para redundância), se uma conexão falhar, a conexão restante ainda tem capacidade suficiente para lidar com a transferência de dados exigida.

No caminho para o servidor, os dados provavelmente será um comutador de rede SAN de trânsito. Como o comutador de rede SAN tem que processar a solicitação de e/s de entrada e encaminhá-lo a porta apropriada, o comutador terão um limite para a quantidade de e/s que podem ser manipulados, no entanto, as especificações de fabricantes será necessárias para determinar o que é que o limite. Por exemplo, se há dois switches e cada comutador pode lidar com 10.000 IOPS, taxa de transferência total será 20.000 IOPS. Tolerância a que está sendo uma preocupação, novamente, falhas se um comutador falhar, a taxa de transferência total do sistema serão 10.000 IOPS. Como ele é desejado não deve exceder 80% da utilização na operação normal, usando não mais de 8000 e/s deve ser o destino.

Por fim, o HBA instalado no servidor também teria um limite para a quantidade de e/s que pode manipular. Geralmente, um segundo HBA é instalado para redundância, mas assim como ocorre com a opção de SAN, ao calcular a e/s máximo que pode ser tratada, a taxa de transferência total *N* &ndash; HBAs 1 é o que é a escalabilidade máxima do sistema.

### <a name="caching-considerations"></a>Considerações sobre o armazenamento em cache

Caches são um dos componentes que podem afetar significativamente o desempenho geral em qualquer ponto no sistema de armazenamento. Uma análise detalhada sobre os algoritmos de cache está além do escopo deste artigo. No entanto, algumas instruções básicas sobre o cache de subsistemas de disco valem a pena de ilumina:

- Armazenamento em cache faz e/s de gravação sequencial prolongada como ela pode muitas operações de gravação menores em blocos maiores de e/s do buffer e cancelar o preparo para o armazenamento em tamanhos de bloco de menos, porém maiores. Isso reduzirá a e/s aleatória total e total e/s sequencial, fornecendo assim mais disponibilidade de recursos para outra e/s.
- O cache não melhorar a taxa de transferência de e/s de gravação prolongada do subsistema de armazenamento. Ele permite apenas para as gravações seja armazenado em buffer até que os eixos estão disponíveis para confirmar os dados. Quando o disponíveis e/s dos eixos no subsistema de armazenamento está saturado por longos períodos, o cache eventualmente será preenchida. Para esvaziar o cache, tempo suficiente entre os picos ou eixos extras, precisa ser alocado para fornecer o suficiente e/s para permitir que o cache a liberar.

  Caches maiores permitem apenas mais dados a serem armazenados em buffer. Isso significa que podem ser acomodados períodos mais longos de saturação.

  Em um subsistema de armazenamento operacional, o sistema operacional terão melhor desempenho de gravação conforme os dados só precisam ser gravados em cache. Depois que a mídia subjacente está saturada com e/s, preencherá o cache e o desempenho de gravação será retornado para a velocidade do disco.

- Quando o cache de leitura e/s, o cenário em que o cache é mais vantajoso é quando os dados são armazenados em sequência no disco e o cache pode leitura antecipada (faz a suposição de que o setor de Avançar contém os dados que serão solicitados em seguida).
- Ao ler e/s é aleatória, armazenamento em cache no controlador de unidade é improvável fornecer qualquer melhoria para a quantidade de dados que podem ser lidos do disco. Qualquer melhoria é inexistente e se o sistema operacional ou o tamanho de cache baseada em aplicativo for maior que o tamanho do cache com base em hardware.

  No caso do Active Directory, o cache é limitado apenas pela quantidade de RAM.

### <a name="ssd-considerations"></a>Considerações do SSD

SSDs são um animal completamente diferente que os discos rígidos com base no eixo. E ainda permanecem duas principais critérios: "O número de IOPS pode ele manipular?" e "Qual é a latência para os IOPS?" Em comparação com o eixo com base em discos rígidos, o SSDs pode lidar com volumes maiores de e/s e pode ter latências menores. Em geral e a partir da redação deste artigo, enquanto o SSDs ainda são caro em uma comparação de custo por Gigabyte, elas são muito baratas em termos de custo-por-de e/S e merecem consideração importante em termos de desempenho de armazenamento.

Considerações:

- IOPS e latências são muito subjetivas para os designs de fabricante e em alguns casos foram observadas ser desempenho pior do que tecnologias com base do eixo. Em resumo, é mais importante para revisar e validar as especificações do fabricante por unidade e não suponha que qualquer generalidades.
- Tipos IOPS podem ter números muito diferentes dependendo se ele é leitura ou gravação. Serviços do AD DS, em geral, sendo predominantemente leitura com base em, será menos riscos do que alguns outros cenários de aplicativo.
- "Durabilidade de gravação" – Esse é o conceito que células SSD serão eventualmente desgaste precoce. Vários fabricantes de lidam com esse desafio formas diferentes. Pelo menos para a unidade de banco de dados, o perfil de e/s de leitura predominantemente permite downplaying a significância dessa preocupação, pois os dados não são altamente voláteis.

### <a name="summary"></a>Resumo

Uma maneira de pensar sobre o armazenamento é para representar a canalização doméstica. Imagine o IOPS do que os dados são armazenados na mídia é o principal que consome doméstico. Quando isso é saturado (como raízes no pipe) ou limitado (é recolhido ou muito pequeno), todos os coletores na casa, fazer backup quando muita água está sendo usado (convidados demais). Isso é análogo perfeitamente em um ambiente compartilhado em que um ou mais sistemas estão utilizando o armazenamento compartilhado em um SAN/NAS/iSCSI com a mesma mídia subjacente. Abordagens diferentes podem ser tomadas para resolver os diferentes cenários:

- Uma drenagem recolhida ou subdimensionada requer uma substituição completa de escala e a correção. Isso seria semelhante a adição de novo hardware ou redistribuir os sistemas usando o armazenamento compartilhado em toda a infra-estrutura.
- Um pipe "obstruído" geralmente significa que a identificação de um ou mais problemas transgressor e remoção desses problemas. Em um cenário de armazenamento pode ser armazenamento ou sistema de nível backups, verificações antivírus sincronizados em todos os servidores e execução de software de desfragmentação sincronizado durante períodos de pico.

Em qualquer design de detalhes técnicos, anda vários alimentar o principal que consome. Se nada parar backup desses anda ou um ponto de junção, apenas as coisas por trás dessa junção ponto de backup. Em um cenário de armazenamento, isso poderia ser um comutador sobrecarregado (cenário de SAN/NAS/iSCSI), problemas de compatibilidade de driver errado driver/HBA (combinação Firmware/storport.sys) ou antivírus/backup/desfragmentação. Para determinar se o "pipe" do armazenamento é grande o suficiente, o tamanho de e/s e IOPS precisa ser medido. Em cada conjunto adicioná-los juntos para garantir adequado "diâmetro de barra vertical".

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>Apêndice D - discussão sobre como solucionar problemas de armazenamento - ambientes em que pelo menos tanto fornecendo RAM conforme o tamanho do banco de dados não é uma opção viável

É útil entender por que essas recomendações existem para que as alterações na tecnologia de armazenamento podem ser acomodadas. Essas recomendações existem por dois motivos. A primeira é o isolamento de e/s, que problemas de desempenho (isto é, de paginação) o eixo de sistema operacional não afetam o desempenho do banco de dados e perfis de e/s. O segundo é que os arquivos de log do AD DS (e a maioria dos bancos de dados) são sequenciais por natureza e caches e unidades de disco rígido com base no eixo tem um benefício de desempenho enorme quando usado com e/s sequencial em comparação com os padrões de e/s mais aleatórios do sistema operacional e quase padrões de e/s puramente aleatórios do AD DS unidade de banco de dados. Ao isolar a e/s sequencial para uma unidade física separada, taxa de transferência pode ser aumentada. O desafio apresentado por opções de armazenamento de hoje é que as suposições fundamentais por trás dessas recomendações não forem verdadeiras. Em muitos virtualizados cenários de armazenamento, como iSCSI, os arquivos de imagem de disco Virtual de SAN e NAS, a mídia é compartilhada entre vários hosts de armazenamento subjacente e, portanto, eliminando completamente "isolamento de e/s" e os aspectos de "otimização de e/s sequencial". Na verdade esses cenários de adicionar uma camada adicional de complexidade que outros hosts acessando a mídia compartilhada podem prejudicar a capacidade de resposta para o controlador de domínio.

No planejamento do desempenho de armazenamento, há três categorias a serem consideradas: o estado de cache frio, estado do cache prontas e backup/restauração. O estado do cache frio ocorre em cenários como quando o controlador de domínio inicialmente é reinicializado ou o serviço do Active Directory é reiniciado e não há nenhum dado do Active Directory na RAM. Estado do cache passiva é onde o controlador de domínio está em um estado estável e o banco de dados é armazenado em cache. Eles são importantes para observar como eles influenciam a perfis de desempenho muito diferentes e ter RAM suficiente para armazenar em cache todo o banco de dados não ajuda desempenho quando o cache está frio. É possível considerar design de desempenho para esses dois cenários com a analogia a seguir, o cache frio de aquecimento é "sprint" e executar um servidor com um cache passiva é "marathon".

Para o cache frio e o cenário de cache quente, a pergunta se torna rapidez o armazenamento pode mover os dados do disco na memória. Preparando o cache é um cenário em que, ao longo do tempo, o desempenho melhora conforme mais consultas reutilize os dados, aumentos de taxa de ocorrências do cache e diminui a frequência de precisar ir até o disco. Como resultado, o impacto adverso no desempenho do disco vai diminui. Qualquer degradação no desempenho apenas é transitória enquanto aguarda o cache a quente e aumentar o tamanho permitido máximo, dependente do sistema. A conversa pode ser simplificada para o quão rapidamente os dados podem ser obtidos fora do disco e é uma medida simple do IOPS disponível para o Active Directory, que é subjetiva para IOPS disponível do armazenamento subjacente. Da perspectiva do planejamento, porque o aquecimento os cenários de cache e backup/restauração acontecerem excepcionalmente, normalmente ocorrem fora do horário comercial e são subjetiva a carga do controlador de domínio, as recomendações gerais não existem, exceto em que essas atividades ser agendada para o horário de pico.

AD DS, na maioria dos cenários, é predominantemente de leitura e/s, geralmente uma taxa de gravação read/10% 90%. E/s de leitura frequentemente tende a ser o gargalo para experiência do usuário e com e/s de gravação, faz com que escrever degradação de desempenho. Como e/s para o NTDS. dit é predominantemente aleatória, caches tendem a fornecer o mínimo de benefício para leitura e/s, tornando-o que muito mais importante para configurar o armazenamento para ler o perfil de e/s corretamente.

Para condições normais de operação, a meta do planejamento de armazenamento é minimizar os tempos de espera de uma solicitação do AD DS para ser retornado do disco. Essencialmente, isso significa que o número de pendentes e e/s pendentes é menor ou equivalente ao número de caminhos para o disco. Há várias maneiras para medir a isso. Em um cenário de monitoramento de desempenho, a recomendação geral é desse disco lógico ( *\<unidade de banco de dados NTDS\>* ) \Avg disco s/leitura ser menos de 20 ms. O limite operacional desejado deve ser muito menor, de preferência como aproxima da velocidade do armazenamento possível no intervalo de milissegundo de 2 a 6 (.002 para.006 segundo), dependendo do tipo de armazenamento.

Exemplo:

![Gráfico de latência de armazenamento](media/capacity-planning-considerations-storage-latency.png)

Analisando o gráfico:

- **Elipse verde à esquerda –** a latência permanece consistente em 10 ms. A carga aumenta de IOPS de 800 a 2400 IOPS. Esse é o local absoluto para quão rapidamente uma solicitação de e/s pode ser processada pelo armazenamento subjacente. Isso está sujeito às especificidades da solução de armazenamento.
- **Vinho elipse à direita –** a taxa de transferência permanece simples de saída do círculo verde por meio de até o final da coleta de dados, enquanto a latência continua a aumentar. Isso é demonstrar que, quando os volumes de solicitação excederem as limitações físicas do armazenamento subjacente, as solicitações mais tempo gastam colocada em fila aguardando para ser enviada para o subsistema de armazenamento.

Aplicar esse conhecimento:

- **Impacto em uma associação de consulta de usuário de um grande grupo –** supor que isso requer a leitura de 1 MB de dados do disco, a quantidade de e/s e quanto tempo demora pode ser avaliada da seguinte maneira:
  - Páginas de banco de dados do Active Directory são 8 KB de tamanho. 
  - Um mínimo de 128 páginas precisam ser lidas do disco. 
  - Supondo que nada é armazenado em cache, no chão (10 ms) isso vai levar um mínimo 1,28 segundos para carregar os dados do disco para retorná-lo ao cliente. Em 20 ms, em que a taxa de transferência de armazenamento tem maximizadas muito tempo e também é o máximo recomendado, levará 2,5 segundos, para obter os dados do disco para retorná-lo para o usuário final.  
- **A qual taxa o cache ser prontas –** supondo que o cliente vai para maximizar a taxa de transferência neste exemplo de armazenamento, o cache será morna uma taxa de IOPS 2400 &times; 8 KB por e/s. Ou, aproximadamente 20 MB/s por segundo, o carregamento cerca de 1 GB de banco de dados para a RAM cada 53 segundos.

> [!NOTE]
> É normal por curtos períodos observar as latências subir quando componentes agressivamente ler ou gravar no disco, como quando o sistema está sendo feito backup ou ao AD DS é executar a coleta de lixo. Sala de cabeçalho adicional sobre os cálculos deve ser fornecida para acomodar esses eventos periódicos. O objetivo é fornecer a taxa de transferência suficiente para acomodar esses cenários sem afetar a função normal.

Como pode ser visto, há um limite físico com base no design à rapidez o cache pode, possivelmente, verificação de armazenamento. O que será morna cache são recebidas solicitações de cliente até a taxa em que o armazenamento subjacente pode fornecer. Execução de scripts à "pré-verificação de" cache durante o horário de pico fornecerá competição carregar orientado por solicitações de clientes reais. Que pode afetar adversamente o fornecimento de dados que os clientes precisam primeiro porque, por design, ele gerará competição por recursos escassos disco conforme artificial tenta morna cache carregará os dados que não não relevantes para os clientes entrando em contato com o controlador de domínio.
