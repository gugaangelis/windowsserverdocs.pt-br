---
title: "Protegendo o Material de referência de acesso privilegiado"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22ee9a77-4872-4c54-82d9-98fc73a378c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ee5769a3ed0225ebdd31645027bace38f0bff1b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="securing-privileged-access-reference-material"></a>Protegendo o Material de referência de acesso privilegiado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Esta seção contém informações de referência para proteção de acesso privilegiado incluindo:

-   [Modelo de nível administrativo do Active Directory](#ADATM_BM)

-   [Princípio de origem limpa](#CSP_BM)

-   [Abordagem de Design de floresta administrativas ESAE](#ESAE_BM)

-   [Equivalência de nível 0](#T0E_BM)

-   [Ferramentas administrativas e tipos de Logon](#ATLT_BM)

## <a name="ADATM_BM"></a>Modelo de nível administrativo do Active Directory
A finalidade desse modelo de nível é proteger sistemas de identidade usando um conjunto de zonas de buffer entre o controle total do ambiente (nível 0) e os ativos de estação de trabalho de alto risco que invasores comprometer com frequência.

![Diagrama mostrando as três camadas do modelo de nível](../media/securing-privileged-access-reference-material/PAW_RM_Fig1.JPG)

O modelo de nível é composto por três níveis e inclui apenas as contas de administrador, contas de usuário não padrão:

-   **Nível 0** -controle direto de identidades de empresa no ambiente. Nível 0 inclui contas, grupos e outros ativos com controle administrativo direto ou indireto da floresta, domínios ou controladores de domínio do Active Directory e todos os ativos dentro dele. A sensibilidade de segurança de todos os ativos de nível 0 é equivalente como se estivessem com eficiência em controle entre si.

-   **Nível 1** -controle de servidores e aplicativos corporativos. Os ativos de nível 1 incluem sistemas operacionais de servidor, os serviços de nuvem e aplicativos corporativos. Contas do nível 1 administrador com controle administrativo de uma quantidade significativa de valor de negócios que está hospedado nesses ativos. Uma função de exemplo comum é os administradores de servidor que mantêm esses sistemas operacionais com a capacidade de afetar todos os serviços corporativos.

-   **Nível 2** -controle de dispositivos e estações de trabalho do usuário. Contas de administrador 2 nível tem controle administrativo de uma quantidade significativa de valor de negócios que está hospedado em estações de trabalho do usuário e dispositivos. Os exemplos incluem suporte técnico e o computador dá suporte aos administradores porque eles podem afetar a integridade de dados praticamente qualquer usuário.

> [!NOTE]
> As camadas também servem como um mecanismo de priorização básico para proteger os ativos administrativos, mas é importante considerar que um invasor com controle de todos os ativos em qualquer nível pode acessar a maioria dos ou todos os ativos de negócios. Isso é útil como um mecanismo de priorização básico é invasor dificuldade/custo. É mais fácil para um invasor operar com controle total de todas as identidades (nível 0) ou servidores e serviços de nuvem (nível 1) que é se eles devem acessar cada estação de trabalho individual ou dispositivo do usuário (nível 2) para obter dados da sua organização.

### <a name="containment-and-security-zones"></a>Zonas de segurança e contenção
Os níveis são relativos uma zona de segurança específicos. Enquanto eles passaram por muitos nomes, zonas de segurança são uma abordagem bem estabelecida que fornecem contenção de ameaças de segurança por meio de isolamento da camada de rede entre eles. O modelo de camada complementa o isolamento fornecendo contenção de adversários dentro de uma zona de segurança em que o isolamento de rede não é eficaz. Zonas de segurança podem abranger ambos os locais e nuvem infraestrutura, como no exemplo em que os controladores de domínio e membros do domínio no mesmo domínio são hospedados no local e no Azure.

![Diagrama mostrando como zonas de segurança podem abranger ambos os locais e infraestrutura de nuvem](../media/securing-privileged-access-reference-material/PAW_RM_Fig2.JPG)

O modelo de nível impede que a elevação de privilégio, restringindo o que os administradores podem controlar e onde eles podem fazer logon (porque o logon em um computador concede controle dessas credenciais e todos os ativos gerenciados por essas credenciais).

### <a name="control-restrictions"></a>Restrições de controle
Restrições de controle são mostradas na figura a seguir:

![Diagrama de restrições de controle](../media/securing-privileged-access-reference-material/PAW_RM_Fig3.JPG)

### <a name="primary-responsibilities-and-critical-restrictions"></a>Principais responsabilidades e restrições críticas
**Administrador de nível 0** -gerenciar o repositório de identidade e um pequeno número de sistemas que estão no controle eficaz dele, e:

-   Pode gerenciar e controlar ativos em qualquer nível conforme necessário

-   Somente faça logon interativamente ou acessar ativos confiáveis no nível do nível 0

**Administrador de nível 1** -gerenciar servidores corporativos, serviços e aplicativos, e:

-   Somente pode gerenciar e controlar ativos no nível do nível 1 ou nível 2

-   Pode apenas acesso ativos (por meio de tipo de logon de rede) que são confiáveis no nível 1 ou níveis 0

-   Pode apenas logon interativamente ativos confiáveis no nível do nível 1

**Administrador de nível 2** -gerenciar os desktops corporativos, notebooks, impressoras e outros dispositivos do usuário, e:

-   Somente pode gerenciar e controlar ativos no nível de nível 2

-   Pode acessar ativos (por meio de tipo de logon de rede) em qualquer nível conforme necessário

-   Pode apenas logon interativamente ativos confiáveis no nível de nível 2

### <a name="logon-restrictions"></a>Restrições ao logon
Restrições ao logon são mostradas na figura a seguir:

![Diagrama de restrições ao logon](../media/securing-privileged-access-reference-material/PAW_RM_Fig4.JPG)

> [!NOTE]
> Observe que alguns ativos podem afetar a disponibilidade do ambiente de nível 0, mas não diretamente impacto a confidencialidade ou integridade dos ativos. Isso inclui o serviço de servidor DNS e dispositivos de rede essenciais como proxies da Internet.

## <a name="CSP_BM"></a>Princípio de origem limpa
O princípio de origem limpa exige que todas as dependências de segurança seja tão confiável quanto o objeto sendo protegido.

![Diagrama mostrando como um assunto no controle de um objeto é uma dependência de segurança desse objeto](../media/securing-privileged-access-reference-material/PAW_RM_Fig5.JPG)

Qualquer assunto no controle de um objeto é uma dependência de segurança desse objeto. Se um adversário pode controlar nada no controle efetivo de um objeto de destino, eles podem controlar esse objeto de destino. Por isso, você deve garantir que as garantias todas as dependências de segurança estão no ou acima do nível de segurança desejado do próprio objeto.

Enquanto simples no princípio, aplicar isso requer Noções básicas sobre as relações de controle de um ativo de interesse (objeto) e executar uma análise de dependência de descobrir todas as dependências de segurança (Subject(s)).

Como o controle é transitivo, este princípio tem que ser recursivamente repetida. Para controles de exemplo, se um controles B e B C, então A também indiretamente os controles C.

![Diagrama mostrando como se um controles B e B controles C, então A também indiretamente os controles C](../media/securing-privileged-access-reference-material/PAW_RM_Fig6.JPG)

Um invasor que comprometem um obtém acesso a tudo um controles (incluindo B) e tudo B controla (incluindo C). Usando a linguagem de dependências de segurança sobre este mesmo exemplo, B e A são dependências de segurança do C e tem serem protegidos no nível do C assurance desejada na ordem c ter esse nível de garantia.

Para sistemas de identidade e de infraestrutura de TI, este princípio deve ser aplicado ao meio mais comum de controle, incluindo o hardware em que os sistemas são instalados, a mídia de instalação para os sistemas, a arquitetura e configuração do sistema e operações diárias.

### <a name="clean-source-for-installation-media"></a>Limpeza de origem de mídia de instalação
![Diagrama mostrando uma fonte para a mídia de instalação limpa](../media/securing-privileged-access-reference-material/PAW_RM_CSIM.JPG)

Aplicando o princípio de origem limpa a mídia de instalação requer que você a garantir que a mídia de instalação não foi violada desde está sendo lançada pelo fabricante (da melhor forma você é capaz de determinar). Esta figura mostra um invasor usando esse caminho de comprometer um computador:

![Figura mostrando um invasor usando um caminho de comprometer um computador](../media/securing-privileged-access-reference-material/PAW_RM_Fig8.JPG)

Aplicando o princípio de origem limpa a mídia de instalação requer a validar a integridade de software em todo o ciclo possuem incluindo durante a aquisição, armazenamento e transferir para cima até que ele é usado.

#### <a name="software-acquisition"></a>Aquisição de software
A origem do software deve ser validada por meio de um da seguinte maneira:

-   Software é obtido a mídia física é conhecida por vêm do fabricante ou por uma fonte confiável, normalmente fabricada mídia enviada de um fornecedor.

-   Software é obtido da Internet e validado com hashes de arquivo fornecido pelo fornecedor.

-   Software é obtido da Internet e validado baixando e comparando duas cópias independentes:

    -   Baixar para dois hosts sem nenhuma relação de segurança (não no mesmo domínio e não gerenciados pelas mesmas ferramentas), preferencialmente de conexões de Internet separadas.

    -   Compare os arquivos baixados usando um utilitário como certutil:

        `certutil -hashfile <filename>`

Quando possível, todos os softwares de aplicativo, como instaladores de aplicativos e ferramentas devem ser assinados digitalmente e verificado usando Authenticode do Windows com o [Windows Sysinternal](https://www.microsoft.com/sysinternals)ferramenta s, *sigcheck.exe*, com a verificação de revogação. Alguns softwares podem ser necessários em que o fornecedor não pode fornecer esse tipo de assinatura digital.

#### <a name="software-storage-and-transfer"></a>Transferência e armazenamento de software
Depois de obter o software, ela deverá ser armazenada em um local protegido contra modificações, especialmente por hosts conectado à internet ou a equipe de confiável em um nível inferior que os sistemas em que o software ou o sistema operacional será instalado. Esse armazenamento pode ser mídia física ou um local seguro eletrônico.

#### <a name="software-usage"></a>Uso de software
Idealmente, o software deve ser validado no momento que é usado, como quando ele é manualmente instalado, empacotado para uma ferramenta de gerenciamento de configuração ou importado para uma ferramenta de gerenciamento de configuração.

### <a name="clean-source-for-architecture-and-design"></a>Origem limpo para a arquitetura e o design
Aplicando o princípio de origem limpo para a arquitetura do sistema requer que você a garantir que o sistema não é dependente em sistemas de confiança inferiores. Um sistema pode ser dependente em um sistema de confiança maior, mas não em um sistema de confiança inferior com os padrões de segurança mais baixas.

Por exemplo, aceitável para o Active Directory controlar uma área de trabalho do usuário padrão, mas ele é um escalonamento significativo de risco de privilégio para uma área de trabalho do usuário padrão estar no controle do Active Directory.

![Diagrama mostrando como um sistema pode ser dependente em um sistema de confiança maior, mas não em um sistema de confiança inferior com os padrões de segurança mais baixas](../media/securing-privileged-access-reference-material/PAW_RM_Fig09.JPG)

A relação de controle pode ser introduzida por meios muitos, incluindo listas de controle de acesso (ACLs) de segurança nos objetos como sistemas de arquivos, a associação ao grupo local Administradores em um computador ou agentes instalados em um computador em execução como sistema (com a capacidade de executar scripts e código arbitrário).

Um exemplo frequentemente esquecido é exposição por meio de logon, que cria uma relação de controle por meio da exposição credenciais administrativas de um sistema para outro sistema. Esse é o motivo subjacente por que ataques, como passar o hash de roubo de credenciais são tão eficiente. Quando um administrador faz logon uma área de trabalho do usuário padrão com credenciais de nível 0, eles são expondo essas credenciais para área de trabalho, colocá-lo no controle de anúncios e na criação de um escalonamento de caminho de privilégio ao AD. Para obter mais informações sobre esses ataques, consulte [nesta página](https://technet.microsoft.com/en-us/security/dn785092).

Devido ao grande número de ativos que dependem de sistemas de identidade como Active Directory, você deve minimizar o número de controladores de domínio e do Active Directory dependem de sistemas.

![Diagrama mostrando que você deve reduzir o número de sistemas do Active Directory e dependem de controladores de domínio](../media/securing-privileged-access-reference-material/PAW_RM_Fig010.JPG)

Para obter mais informações sobre como proteger os principais riscos do active directory, consulte [nesta página](http://aka.ms/hardenAD).

### <a name="OSBCS_BM"></a>Padrões operacionais baseados no princípio de origem limpa
Esta seção descreve os padrões operacionais e as expectativas por pessoal administrativo. Esses padrões são projetados para proteger o controle administrativo dos sistemas de tecnologia de informações de uma organização contra riscos que podem ser criados por processos e práticas operacionais.

![Diagrama mostrando como os padrões são projetados para proteger controle administrativo de informações de uma organização sistemas de tecnologia contra riscos que podem ser criados por processos e práticas operacionais](../media/securing-privileged-access-reference-material/PAW_RM_Fig11.JPG)

#### <a name="integrating-the-standards"></a>Integrando os padrões
Você pode integrar esses padrões em padrões e práticas recomendadas gerais da sua organização. Você pode se adaptar a eles a requisitos específicos, ferramentas disponíveis e apetite de risco da sua organização, mas recomendamos que apenas as modificações mínimas para reduzir os riscos. Recomendamos que você use os padrões neste guia como o parâmetro de comparação para seu estado final ideal e gerencia quaisquer deltas como exceções devem ser atendidas em ordem de prioridade.

As diretrizes de padrões é dividida nestas seções:

-   Suposições

-   Conselho

-   Práticas operacionais

    -   Resumo

    -   Detalhes de padrões

#### <a name="assumptions"></a>Suposições
Os padrões nesta seção pressupõem que a organização tem os seguintes atributos:

-   A maioria dos ou todos os servidores e estações de trabalho no escopo associadas ao Active Directory.

-   Todos os servidores sejam gerenciados estão executando o Windows Server 2008 R2 ou posterior e tiver habilitado o modo RestrictedAdmin RDP.

-   Todas as estações de trabalho para serem gerenciados estão executando o Windows 7 ou posterior e tem habilitado o modo RestrictedAdmin RDP.

    > [!NOTE]
    > Para habilitar o modo de RDP RestrictedAdmin, consulte [nesta página](http://aka.ms/RDPRA).

-   Cartões inteligentes estão disponíveis e emitidas administrativas todas as contas.

-   O *Builtin\Administrator* para cada domínio foi designado como uma conta de acesso de emergência

-   Uma solução de gerenciamento de identidade empresarial é implantada.

-   [VOLTAS](http://aka.ms/laps) for implantado em servidores e estações de trabalho para gerenciar a senha da conta de administrador local

-   Há uma solução de gerenciamento de acesso privilegiado, como Microsoft Identity Manager, no local, ou houver um plano para adotar uma.

-   A equipe é atribuída a monitorar os alertas de segurança e responda-a.

-   A funcionalidade technical rapidamente aplicar atualizações de segurança da Microsoft está disponível.

-   Controladores de gerenciamento da placa-base em servidores não serão usados ou seguirão a controles de segurança estrito.

-   Contas de administrador e os grupos de servidores (nível 1 administradores) e estações de trabalho (administradores de nível 2) serão gerenciados por administradores de domínio (nível 0).

-   Há uma mudança Advisory conselho (CAB) ou outra autoridade designada no lugar para aprovar alterações do Active Directory.

#### <a name="change-advisory-board"></a>Conselho
Uma alteração Advisory CAB (Conselho) é a autoridade discussão fórum e aprovação, as alterações que poderiam afetar o perfil de segurança da organização. Todas as exceções esses padrões devem ser enviadas para o arquivo CAB com uma avaliação de risco e justificativa.

Cada padrão neste documento é dividido pelo importância do padrão para um determinado nível de reunião.

![Diagrama mostrando o padrão para dado níveis](../media/securing-privileged-access-reference-material/PAW_RM_Fig12.JPG)

Todas as exceções para itens obrigatórios (marcados com octógono vermelho ou um triângulo laranja neste documento) são consideradas temporárias, e eles precisam ser aprovados pelo arquivo CAB. Diretrizes incluem:

-   A solicitação inicial requer aceitação de risco de justificação assinada por supervisor imediata da equipe, e ele expire após seis meses.

-   Renovações requerem justificativa e a aceitação de risco assinado por um diretor da unidade de negócios, e eles expirem após seis meses.

Todas as exceções para itens recomendado (marcados com um círculo amarelo neste documento) são consideradas temporárias e precisam ser aprovados pelo arquivo CAB. Diretrizes incluem:

-   A solicitação inicial requer aceitação de risco de justificação assinada por supervisor imediata da equipe, e ele expire após 12 meses.

-   Renovações requerem justificativa e a aceitação de risco assinado por um diretor da unidade de negócios, e eles expirem após 12 meses.

#### <a name="operational-practices-standards-summary"></a>Padrões de práticas operacionais resumidos
As colunas de camada nesta tabela consultem o nível da conta administrativa, o controle das quais geralmente afeta todos os ativos em que nível.

![Tabela mostrando níveis da conta admninistrative](../media/securing-privileged-access-reference-material/PAW_RM_Fig13.JPG)

Decisões operacionais que são feitas regularmente são importantes para manter a postura de segurança do ambiente. Esses padrões para processos e práticas recomendadas ajudam a garantir que um erro operacional não resulta em uma vulnerabilidade explorável operacional no ambiente.

##### <a name="administrator-enablement-and-accountability"></a>Responsabilidade e habilitação de administrador
Os administradores devem ser informados, capacitados, treinados e RESPONSABILIZADOS para operar o ambiente mais segura.

###### <a name="administrative-personnel-standards"></a>Padrões de pessoal administrativo
Deve ser examinada atribuído a equipe administrativa para garantir que eles são confiáveis e precisam de privilégios administrativos:

-   Execute verificações em segundo plano pessoal antes de atribuir privilégios administrativos.

-   Examine cada trimestre de privilégios administrativos para determinar qual equipe ainda tenha uma empresa legítima precisa de acesso administrativo.

###### <a name="administrative-security-briefing-and-accountability"></a>Resumo de segurança administrativa e de responsabilidade
Os administradores devem estar informados e responsáveis por riscos para a organização e suas funções em Gerenciando esse risco. Os administradores devem ser treinados anualmente:

-   Ambiente de ameaça gerais

    -   Determinados adversários

    -   Incluindo pass-the-hash de técnicas de ataque e roubo de credenciais

-   Os incidentes e ameaças específicas da organização

-   Funções do administrador na proteção contra ataques

    -   Gerenciando a exposição de credenciais com o modelo de nível

    -   Uso de estações de trabalho administrativas

    -   Uso do modo de RestrictedAdmin de protocolo de área de trabalho remota

-   Práticas administrativas específicas da organização

    -   Examine todas as diretrizes operacionais este padrão

    -   Implemente as seguintes regras importantes:

        -   Não use contas de administrador no não seja estações de trabalho administrativas

        -   Não desabilite ou desmonte controles de segurança em sua conta ou estações de trabalho (por exemplo, restrições ao logon ou atributos necessários para cartões inteligentes)

        -   Relatar problemas ou atividade incomum

Para fornecer responsabilidade, todas as pessoas com contas de administrador devem assinar um documento administrativas privilégio código de conduta que diz que pretendem seguem práticas de política administrativas específicas da organização.

###### <a name="provisioning-and-deprovisioning-processes-for-administrative-accounts"></a>Provisionamento e desprovisionamento processos para contas de administrador
Os padrões a seguir devem ser atendidos para atender aos requisitos de ciclo de vida.

-   Todas as contas administrativas devem ser aprovadas pela autoridade de aprovação descrito na tabela a seguir.

    -   Aprovação deve ser concedida apenas, se os funcionários tenham uma verdadeira necessidade comercial de privilégios administrativos.

    -   Aprovação de privilégios administrativos não deve exceder seis meses.

-   Acesso a privilégios administrativos deve ser desprovisionada imediatamente quando:

    -   A equipe de altera posições.

    -   A equipe saem da organização.

-   Contas devem ser desabilitadas imediatamente após a equipe de sair da organização.

-   Contas desabilitadas devem ser excluídas dentro de seis meses e o registro de sua exclusão deve ser inserido em registros de conselho de aprovação de alteração.

-   Examine todas as associações de conta com privilégios mensalmente para garantir que nenhuma permissão não autorizado foram concedida. Isso pode ser substituído por uma ferramenta automatizada que avisa sobre alterações.

|Nível de privilégio de conta|Aprovação autoridade|Frequência de análise de associação|
|--------------|------------|----------------|
|Administrador de nível 0|Conselho de aprovação|Mensais ou automatizada|
|Administrador de nível 1|Os administradores de nível 0 ou segurança|Mensais ou automatizada|
|Administrador de nível 2|Os administradores de nível 0 ou segurança|Mensais ou automatizada|

##### <a name="operationalize-least-privilege"></a>Colocar em operação privilégios mínimos
Esses padrões ajudam a atingir privilégios mínimos, reduzindo o número de administradores na função e a quantidade de tempo que eles tenham privilégios.

> [!NOTE]
> Obtenção de privilégios mínimos em sua organização exigirão a entender as funções organizacionais, seus requisitos e seus mecanismos de design para garantir que eles possam realizar suas tarefas usando privilégios mínimos. Obtenção de um estado de privilégios mínimos em um modelo administrativo frequentemente requer o uso de várias abordagens:
>
> -   Limite o número de administradores ou membros de grupos com privilégios
> -   Delegar menos privilégios para contas
> -   Fornecer tempo associado privilégios sob demanda
> -   Fornecer a capacidade de outras equipes executar tarefas (uma abordagem Assistente)
> -   Fornecer processos para acesso de emergência e cenários de uso raros

###### <a name="limit-count-of-administrators"></a>Contagem de limite de administradores
Um mínimo de dois técnicos autorizados deve ser atribuído a cada função administrativa para garantir a continuidade de negócios.

Se o número de equipe designada para qualquer função exceder dois, o conselho de aprovação deve aprovar os motivos específicos para atribuir privilégios para cada membro individual (incluindo os dois originais). A justificativa para a aprovação deve incluir:

-   Quais tarefas técnicas são executadas pelos administradores que exigem os privilégios administrativos

-   Com que frequência as tarefas são executadas

-   Motivo específico por que as tarefas não podem ser executadas por outro administrador em seu nome

-   Todas as outras abordagens alternativas conhecidas para conceder o privilégio e por que cada não é aceitável do documento

###### <a name="dynamically-assign-privileges"></a>Atribuir dinamicamente privilégios
Os administradores devem obter permissões "just-in-time" para usá-los conforme eles executam tarefas. Nenhuma permissão será permanentemente atribuído a contas de administrador.

> [!NOTE]
> Privilégios administrativos permanentemente atribuídos naturalmente criar uma estratégia de "a maioria dos privilégio" porque pessoal administrativo exige acesso rápido às permissões para manter a disponibilidade operacional se houver um problema. Permissões de just-in-time fornecem a capacidade de:
>
> -   Atribua permissões mais especificamente, obtendo mais perto de privilégios mínimos.
> -   Reduzir o tempo de exposição de privilégios
> -   Usam as permissões de rastreamento para detectar ataques ou abuso.

##### <a name="manage-risk-of-credential-exposure"></a>Gerenciar o risco de exposição de credenciais
Use as seguintes práticas para gerenciar o risco de exposição de credenciais apropriado.

###### <a name="separate-administrative-accounts"></a>Contas administrativas separadas
Todas as pessoas que estão autorizadas a possuem privilégios administrativos devem ter contas separadas para funções administrativas que são diferentes das contas de usuário.

-   **Contas de usuário padrão** -privilégios de usuário padrão para tarefas de usuário padrão, como email, navegação na web e usar aplicativos de linha de negócios. Essas contas não devem ser concedidas privilégios administrativos.

-   **Contas de administrador** -separar contas criadas por profissionais que recebem os privilégios administrativos apropriados. Um administrador que é necessário para gerenciar ativos em cada nível deve ter uma conta separada para cada nível. Essas contas devem não têm acesso a email ou à Internet pública.

###### <a name="administrator-logon-practices"></a>Práticas recomendadas de logon do administrador
Antes de um administrador pode fazer logon um host interativamente (localmente pela RDP padrão usando RunAs ou usando o console de virtualização), esse host deve atender ou exceder o padrão para o nível de conta de administrador (ou um nível superior).

Os administradores só poderá entrar em estações de trabalho do administrador com suas contas de administrador. Os administradores só fazer logon recursos gerenciados usando a tecnologia de suporte aprovadas descrita na próxima seção.

> [!NOTE]
> Isso é necessário porque o logon interativamente um host concede o controle das credenciais àquele host.
>
> Consulte o [ferramentas administrativas e tipos de Logon](http://aka.ms/admintoolsecurity) para obter detalhes sobre os tipos de logon, as ferramentas comuns de gerenciamento e exposição de credenciais.

###### <a name="use-of-approved-support-technology-and-methods"></a>Uso da tecnologia de suporte aprovadas e métodos
Os administradores que dão suporte a sistemas remotos e os usuários devem seguir estas diretrizes para impedir que um adversário no controle do computador remoto roubem suas credenciais administrativas.

-   As opções de suporte principal devem ser usadas se eles estiverem disponíveis.

-   As opções de suporte secundário devem ser usadas somente se a opção de suporte principal não estiver disponível.

-   Métodos de suporte proibidos nunca podem ser usados.

-   Sem acesso à internet de navegação ou email pode ser realizado por qualquer conta administrativa a qualquer momento.

###### <a name="tier-0-forest-domain-and-dc-administration"></a>Floresta de nível 0, domínio e administração de DC
Certifique-se de que as seguintes práticas são aplicadas para esse cenário:

-   **Suporte ao servidor remoto** -ao acessar remotamente um servidor, os administradores de nível 0 devem seguir estas diretrizes:

    -   **Principal (ferramenta de)** -remoto ferramentas logons de rede que usar (tipo 3). Para obter mais informações, consulte [ferramentas administrativas e tipos de Logon](http://aka.ms/admintoolsecurity).

    -   **Principal (interativo)** -uso RDP RestrictedAdmin ou uma sessão RDP padrão de uma estação de trabalho do administrador com uma conta de domínio

    > [!NOTE]
    > Se você tiver uma solução de gerenciamento de privilégio 0 nível, adicione "que usa permissões obtidos just-in-time uma solução de gerenciamento de acesso privilegiado."

-   **Suporte ao servidor físico** - quando presentes fisicamente no console de um servidor ou em um console da máquina virtual (Hyper-V ou VMWare tools), essas contas não têm restrições de uso nenhuma ferramenta administrativa específicas, somente as restrições gerais de tarefas de usuário padrão como emails e navegar na internet aberta.

    > [!NOTE]
    > Administração de nível 0 é diferente de administração de outros níveis porque todos os ativos de nível 0 já tem o controle direto ou indireto de todos os ativos. Por exemplo, um invasor no controle de um controlador de domínio tem sem a necessidade de roubam credenciais de logon administradores conforme eles já têm acesso a todas as credenciais de domínio no banco de dados.

###### <a name="tier-1-server-and-enterprise-application-support"></a>Servidor de nível 1 e suporte a aplicativos corporativos
Certifique-se de que as seguintes práticas são aplicadas para esse cenário:

-   **Suporte ao servidor remoto** -ao acessar remotamente um servidor, os administradores de nível 1 devem seguir estas diretrizes:

    -   **Principal (ferramenta de)** -remoto ferramentas logons de rede que usar (tipo 3). Para obter mais informações, consulte [Mitigating Pass-the-Hash e outros roubo de credenciais](https://www.microsoft.com/pth) v1 (pp 42 47).

    -   **Principal (interativo)** -uso RDP RestrictedAdmin em uma estação de trabalho do administrador com uma conta de domínio que usa as permissões obtido just-in-time de uma solução de gerenciamento de acesso privilegiado.

    -   **Secundário** -logon o servidor usando uma senha de conta local que é definida pelo VOLTAS de uma estação de trabalho do administrador.

    -   **Proibidos** -RDP padrão não pode ser usado com uma conta de domínio.

    -   **Proibidos** -usando a conta de domínio credenciais enquanto na sessão (por exemplo, usando *RunAs* ou autenticando para um compartilhamento de). Isso expõe as credenciais de logon para o risco de roubo.

-   **Suporte ao servidor físico** - quando presentes fisicamente no console de um servidor ou em um console da máquina virtual (Hyper-V ou VMWare tools), os administradores de nível 1 devem recuperar a senha de conta local do VOLTAS antes de acessar o servidor.

    -   **Principais** -recuperar a senha de conta local definida pelo VOLTAS de uma estação de trabalho do administrador antes de fazer logon no servidor.

    -   **Proibidos** -logon com uma conta de domínio não é permitida neste cenário.

    -   **Proibidos** -usando a conta de domínio credenciais enquanto na sessão (por exemplo, RunAs ou autenticação em um compartilhamento). Isso expõe as credenciais de logon para o risco de roubo.

###### <a name="tier-2-help-desk-and-user-support"></a>Nível 2 suporte técnico e suporte do usuário
Suporte técnico e usuário organizações de suporte ao executam o suporte para os usuários finais (que não exigem privilégios administrativos) e as estações de trabalho do usuário (que exigem privilégios administrativos).

**Suporte ao usuário** -tarefas incluem ajudar os usuários com executando tarefas que não exigem nenhuma modificação na estação de trabalho, com frequência mostrando como usar um recurso de aplicativo ou recurso do sistema operacional.

-   **Suporte ao lado de mesa usuário** -equipe de suporte de nível 2 a está fisicamente no espaço de trabalho do usuário.

    -   **Principais** -por"jaquetas" suporte pode ser fornecido sem nenhum ferramentas.

    -   **Proibidos** -logon com credenciais administrativas de conta de domínio não é permitido neste cenário. Alternar para o suporte de estação de trabalho do lado do suporte técnico se são necessários privilégios administrativos.

-   **Suporte ao usuário remoto** -o nível 2 a equipe de suporte é fisicamente remoto para o usuário.

    -   **Principais** -assistência remota, o Skype for Business ou o compartilhamento de tela do usuário semelhante pode ser usado. Para obter mais informações, consulte [o que é assistência remota do Windows?](https://windows.microsoft.com/en-us/windows/what-is-windows-remote-assistance)

    -   **Proibidos** -logon com credenciais administrativas de conta de domínio não é permitido neste cenário. Alternar para o suporte de estação de trabalho se são necessários privilégios administrativos.

-   **Suporte a estação de trabalho** - tarefas incluem a execução de manutenção de estação de trabalho ou solução de problemas que requer acesso a um sistema para logs de visualização, instalar um software, atualizar os drivers e assim por diante.

    -   **Suporte a estação de trabalho do lado do suporte técnico** -equipe de suporte o nível 2 está fisicamente em estação de trabalho do usuário.

        -   **Principais** -recuperar a senha de conta local definida pelo VOLTAS de uma estação de trabalho do administrador antes de se conectar a estação de trabalho do usuário.

        -   **Proibidos** -logon com credenciais administrativas de conta de domínio não é permitido neste cenário.

    -   **Suporte a estação de trabalho remota** -o nível 2 a equipe de suporte é fisicamente remoto na estação de trabalho.

        -   **Principais** -uso RDP RestrictedAdmin em uma estação de trabalho do administrador com uma conta de domínio que usa as permissões obtido just-in-time de uma solução de gerenciamento de acesso privilegiado.

        -   **Secundário** -recuperar uma senha de conta local definida pelo VOLTAS de uma estação de trabalho do administrador antes de se conectar a estação de trabalho do usuário.

        -   **Proibidos** -Use RDP padrão com uma conta de domínio.

###### <a name="no-browsing-the-public-internet-with-admin-accounts-or-from-admin-workstations"></a>Nenhuma navegar na Internet pública com contas de administrador ou de estações de trabalho do administrador
Pessoal administrativo não pode navegar na Internet aberta enquanto estiver conectado com uma conta administrativa ou enquanto estiver conectado a uma estação de trabalho administrativa. As únicas exceções autorizadas são o uso de um navegador da web para administrar um serviço baseado em nuvem, como o Microsoft Azure, Amazon Web Services, Microsoft Office 365 ou enterprise do Gmail.

###### <a name="no-accessing-email-with-admin-accounts-or-from-admin-workstations"></a>Nenhum email acessando com contas de administrador ou de estações de trabalho do administrador
Pessoal administrativo não pode acessar o email enquanto estiver conectado com uma conta administrativa ou enquanto estiver conectado a uma estação de trabalho administrativa.

###### <a name="store-service-and-application-account-passwords-in-a-secure-location"></a>Serviço de armazenamento e senhas em um local seguro de conta de aplicativo
As diretrizes a seguir devem ser usadas para a segurança física processa que controlam o acesso à senha:

-   Bloquear as senhas de conta de serviço em um cofre físico.

-   Certifique-se de que somente a equipe confiável em ou acima a classificação de nível da conta têm acesso à senha da conta.

-   Limite o número de pessoas que acessem as senhas para um número mínimo de uso de contas.

-   Certifique-se de que todo o acesso à senha é registrado, acompanhado e monitorado por um fabricante disinterested, por exemplo, um gerente treinados não executar a administração de TI.

##### <a name="strong-authentication"></a>Autenticação forte
Use as seguintes práticas para configurar a autenticação forte apropriado.

###### <a name="enforce-smartcard-multi-factor-authentication-mfa-for-all-admin-accounts"></a>Impor smartcard a autenticação multifator (MFA) para todas as contas de administrador
Nenhuma conta administrativa tem permissão para usar uma senha para autenticação. As únicas exceções autorizadas são as contas de acesso de emergência são protegidas pelos processos apropriados.

Vincular todas as contas de administrador para um cartão inteligente e habilitar o atributo "**cartão inteligente necessário para Logon interativo**."

Um script deve ser implementado para automaticamente e periodicamente redefinir o valor de hash da senha aleatória desabilitando e habilitando novamente o atributo, imediatamente "**cartão inteligente necessário para Logon interativo**."

Permitir que nenhuma exceção para contas usadas por equipe humana além as contas de acesso de emergência.

###### <a name="enforce-multi-factor-authentication-for-all-cloud-admin-accounts"></a>Impor a autenticação multifator para todas as contas de administrador de nuvem
Todas as contas com privilégios administrativos em um serviço de nuvem, como Microsoft Azure e Office 365, devem usar a autenticação multifator.

##### <a name="rare-use-emergency-procedures"></a>Procedimentos de emergência uso raros
Práticas operacionais devem dar suporte os padrões a seguir:

-   Certifique-se de interrupções podem ser resolvidas rapidamente.

-   Certifique-se de tarefas de alto privilégio raras podem ser concluídas conforme necessário.

-   Certifique-se de procedimentos de seguros são usados para proteger as credenciais e privilégios.

-   Certifique-se de processos de rastreamento e aprovação apropriados sejam seguidos.

###### <a name="correctly-follow-appropriate-processes-for-all-emergency-access-accounts"></a>Siga corretamente processos apropriados para todas as contas de acesso de emergência
Certifique-se de que cada conta de acesso de emergência tem uma folha de rastreamento no cofre.

O procedimento documentado na senha rastreamento folha deve ser seguido para cada conta, que inclui a alteração da senha depois de cada uso e registro em log sem qualquer estações de trabalho ou servidores usados após a conclusão.

Todos usam de emergência acesso contas devem ser aprovadas pelo Conselho de aprovação de alteração em Avançado ou após o ocorrido como um uso de emergência aprovado.

###### <a name="restrict-and-monitor-usage-of-emergency-access-accounts"></a>Restringir e monitorar o uso de contas de acesso de emergência
Para todos os usos de contas de acesso de emergência:

-   Administradores de domínio autorizado somente podem acessar as contas de acesso de emergência com privilégios de administrador do domínio.

-   As contas de emergência acesso podem ser usadas apenas em controladores de domínio e outros hosts de nível 0.

-   Essa conta deve ser usada apenas para:

    -   Execute a solução de problemas e correção de problemas técnicos que impedem o uso das corretos contas administrativas.

    -   Execute tarefas raras, como:

        -   Administração de esquema

        -   Tarefas de toda a floresta que exigem privilégios administrativos enterprise Observe que o gerenciamento de topologia incluindo gerenciamento de site e de sub-rede do Active Directory é delegado para limitar o uso desses privilégios.

-   Uso todos de uma dessas contas deve ter autorização por escrito pelo líder do grupo de segurança

-   O procedimento na folha de rastreamento para cada conta de acesso de emergência requer a senha seja alterada para cada uso. Um membro da equipe de segurança deve validar que isso aconteceu corretamente.

###### <a name="temporarily-assign-enterprise-admin-and-schema-admin-membership"></a>Atribua temporariamente associação de administrador esquema e da Administração empresarial
Privilégios devem ser adicionados como necessário e removido após o uso. A conta de emergência deve ter esses privilégios atribuídos para apenas a duração da tarefa para ser concluída e um máximo de 10 horas. Todos os uso e duração desses privilégios devem ser capturadas no registro do conselho de aprovação de alteração depois que a tarefa é concluída.

## <a name="ESAE_BM"></a>Abordagem de Design de floresta administrativas ESAE
Esta seção contém uma abordagem para uma floresta administrativa com base na arquitetura avançado segurança administrativas ambiente (ESAE) referência implantada por equipes de serviços profissionais cybersecurity da Microsoft para proteger os clientes contra ataques de cybersecurity.

Florestas administrativas dedicadas permitem que as organizações grupos, estações de trabalho e contas de administrador do host em um ambiente que tem controles de segurança mais fortes que o ambiente de produção.

Essa arquitetura permite uma série de controles de segurança que não são possíveis ou facilmente configurado em uma arquitetura de floresta única, até mesmo um gerenciados com privilegiados estações de trabalho de acesso (patas). Essa abordagem permite o provisionamento de contas sem privilégios usuários padrão na floresta administrativa que são altamente privilegiado no ambiente de produção, permitindo maior imposição técnica de controle. Essa arquitetura também permite o uso do recurso autenticação seletiva de confiança como um meio para restringir logons (e exposição de credenciais) apenas autorizado hosts. Em situações em que um nível maior de garantia é desejado para a floresta de produção, sem precisar pagar o custo e a complexidade de uma recompilação completa, uma floresta administrativa pode fornecer um ambiente que aumenta o nível de garantia do ambiente de produção.

Embora essa abordagem adicionar uma floresta para um ambiente do Active Directory, o custo e a complexidade são limitadas pelo design fixo, volume pequeno de hardware e software e pequeno número de usuários.

> [!NOTE]
> Essa abordagem funciona bem para administrar o Active Directory, mas muitos aplicativos não são compatíveis com administrado pela contas de uma floresta externa usando uma relação de confiança padrão.

Esta figura mostra uma floresta ESAE usada para administração de ativos de nível 0 e floresta um PRIV configurado para uso com funcionalidade de gerenciamento de acesso privilegiado do Microsoft Identity Manager. Para obter mais informações sobre como implantar uma instância de MIM PAM, consulte [privilegiados gerenciamento de identidade para os serviços de domínio do Active Directory (AD DS)](https://technet.microsoft.com/en-us/library/mt150258.aspx) artigo.

![Figura mostrando uma floresta ESAE usada para administração de ativos de nível 0 e floresta um PRIV configurado para uso com funcionalidade de gerenciamento de acesso privilegiado do Microsoft Identity Manager](../media/securing-privileged-access-reference-material/PAW_RM_Fig14.JPG)

Uma floresta administrativa dedicada é um único domínio padrão floresta do Active Directory dedicado para a função de gerenciamento do Active Directory. Domínios e florestas administrativas podem ser protegidos forma mais rigorosa de casos de uso de florestas de produção devido a limitação.

Um design de floresta administrativos deve incluir as seguintes considerações:

-   **Limitado escopo** -o valor de uma floresta de administrador principal é o alto nível de garantia de segurança e a superfície de ataque reduzida resulta em risco residual inferior. Floresta pode ser usada para armazenar aplicativos e funções de gerenciamento adicionais, mas cada aumento no escopo aumentará a superfície de ataque de floresta e seus recursos. O objetivo é limitar as funções dos usuários floresta e no administrador de dentro de manter a superfície de ataque mínima, para que cada aumento de escopo deve ser considerado cuidadosamente.

-   **Confiar em configurações** -configurar confiança de florestas gerenciadas (s) ou domínios à floresta administrativo

    -   Uma relação de confiança unidirecional é necessária do ambiente de produção de floresta do administrador. Isso pode ser uma relação de confiança do domínio ou uma relação de confiança de floresta. O domínio da floresta admin não precisa confiar em domínios/florestas gerenciadas para gerenciar o Active Directory, embora outros aplicativos podem exigir uma relação de confiança bidirecional, a validação de segurança e testes.

    -   Autenticação seletiva deve ser usada para restringir contas na floresta do administrador de logon somente para os hosts de produção apropriado. Para manter os controladores de domínio e delegação de direitos no Active Directory, isso geralmente requer concessão "Permitidos para logon" direito em controladores de domínio para contas de administrador 0 nível designadas na floresta do administrador. Consulte Definindo configurações de autenticação seletiva para obter mais informações.

-   **Proteção de domínio e privilégios** -floresta administrativa deve ser configurada para privilégios mínimos, com base nos requisitos de administração do Active Directory.

    -   Conceder direitos para administrar controladores de domínio e delegar permissões requer a adição de contas de floresta de administrador ao grupo BUILTIN\Administrators domínio local. Isso ocorre porque o grupo global Admins. do domínio não pode ter membros de um domínio externo.

    -   Advertência para usar esse grupo para conceder direitos é que eles não terão acesso administrativo aos novos objetos de diretiva de grupo por padrão. Isso pode ser mudado seguindo os procedimentos em [este artigo da base de Conhecimento](https://support.microsoft.com/kb/321476) para alterar as permissões padrão de esquema.

    -   Contas da floresta de administrador que são usadas para administrar o ambiente de produção não devem ser concedidas privilégios administrativos para floresta do administrador, domínios nele ou estações de trabalho nele.

    -   Privilégios administrativos ao longo da floresta de administração devem ser bem controlados por um processo offline para reduzir a oportunidade para um invasor ou mal-intencionado insider para apagar os logs de auditoria. Isso também ajuda a garantir que a equipe de contas de administrador de produção não pode relaxar as restrições em suas contas e aumentar risco para a organização.

    -   Floresta administrativa deve seguir as configurações de linha de base de conformidade de segurança da Microsoft (SCB) do domínio, incluindo as configurações de alta seguras de protocolos de autenticação.

    -   Todos os hosts de floresta de administrador devem ser atualizados automaticamente com atualizações de segurança. Enquanto isso pode criar risco de interromper as operações de manutenção de controlador de domínio, ele fornece uma redução significativa de risco de segurança das vulnerabilidades sem patch.

        > [!NOTE]
        > Uma instância dedicada do Windows Server Update Services pode ser configurada para aprovar automaticamente as atualizações. Para obter mais informações, consulte a seção "Automaticamente aprovar atualizações para instalação" nas atualizações de aprovação.

-   **Proteção de estação de trabalho** -compilar as estações de trabalho administrativas usando o [privilegiados estações de trabalho de acesso](../securing-privileged-access/privileged-access-workstations.md) (por meio de fase 3), mas alterar a associação ao domínio na floresta administrativas em vez de ambiente de produção.

-   **Servidor e proteção de DC** - para todos os controladores de domínio e servidores na floresta administrativo:

    -   Certifique-se de que todas as mídias é validada usando a diretriz em [limpa origem de mídia de instalação](http://aka.ms/cleansource)

    -   Certifique-se de que os servidores de floresta administrativos devem ter os sistemas operacionais mais recentes instalados, mesmo se isso não for possível em produção.

    -   Os hosts de floresta Admin devem ser atualizados automaticamente com as atualizações de segurança.

        > [!NOTE]
        > Windows Server Update Services pode ser configurado para aprovar automaticamente as atualizações. Para obter mais informações, consulte a seção "Automaticamente aprovar atualizações para instalação" nas atualizações de aprovação.

    -   Linhas de base de segurança devem ser usadas como configurações de partida.

        > [!NOTE]
        > Os clientes podem usar o Kit de ferramentas de conformidade de segurança do Microsoft (SCT) para configurar as linhas de base nos hosts administrativos.

    -   Inicialização para atenuar contra invasores ou malware tentar carregar o código não assinado no processo de inicialização segura.

        > [!NOTE]
        > Esse recurso foi introduzido no Windows 8 para aproveitar a Unified Extensible Firmware Interface (UEFI).

    -   Criptografia de volume completo para atenuar contra perda de física dos computadores, como notebooks administrativas usados remotamente.

        > [!NOTE]
        > Consulte [BitLocker](https://technet.microsoft.com/en-us/library/dn641993.aspx) para obter mais informações.

    -   Restrições de USB para se proteger contra vetores de infecção físico.

        > [!NOTE]
        > Consulte [controle de acesso de leitura ou gravação de dispositivos removíveis ou mídia](https://technet.microsoft.com/en-us/library/cc730808(v=ws.10).aspx) para obter mais informações.

    -   Isolamento de rede para se proteger contra ataques de rede e as ações de administrador inadvertida. Firewalls de host devem bloquear todas as conexões de entrada exceto aqueles explicitamente necessários e bloquear todo o acesso de saída do Internet.

    -   Antimalware para se proteger contra malware e ameaças conhecidas.

    -   Análise da superfície de ataque para evitar a introdução da nova vetores de ataque Windows durante a instalação do novo software.

        > [!NOTE]
        > Uso de ferramentas como o [analisador de superfície de ataque (ASA)](https://www.microsoft.com/en-us/download/details.aspx?id=24487) ajudará a avaliar as definições de configuração em um host e identificar os vetores de ataque introduzidos pelo software ou alterações na configuração.

-   Otimização da segurança da conta

    -   A autenticação multifator deve ser configurada para todas as contas na floresta do administrador, exceto uma conta. Pelo menos uma conta administrativa deve ser a senha com base para garantir o acesso funcionará caso a autenticação multifator processar quebras. Essa conta deve ser protegida por um processo rigorosos controle físico.

    -   Contas configuradas para a autenticação multifator devem ser configuradas para definir um novo NTLM hash em contas regularmente. Isso pode ser feito desabilitando e habilitar o atributo de conta cartão inteligente é necessário para logon interativo.

        > [!NOTE]
        > Isso pode interromper as operações em andamento usando essa conta, portanto, esse processo deve ser iniciado somente quando os administradores não estar usando a conta, como à noite ou no fim de semana.

-   Controles de investigação

    -   Controles de investigação de floresta administrativo devem ser projetados para alertar sobre anomalias na floresta do administrador. O número limitado de cenários autorizados e atividades pode ajudar a ajustar esses controles mais preciso do que o ambiente de produção.

Para saber mais atraentes sobre os serviços Microsoft para criar e implantar um ESAE para seu ambiente, veja [nesta página](https://www.microsoft.com/services).

## <a name="T0E_BM"></a>Equivalência de nível 0
A maioria das organizações controlar os membros a poderosos grupos do Active Directory de nível 0 como administradores, Admins. do domínio e administradores corporativos.  Muitas organizações desprezar o risco de outros grupos são efetivamente equivalentes em termos de privilégio em um ambiente do active directory típico. Esses grupos oferecem um caminho de escalonamento relativamente fácil para um invasor os mesmos privilégios de nível 0 explícitos usando vários métodos de ataque diferentes.

Por exemplo, um operador de servidor pode obter acesso a uma mídia de backup de um controlador de domínio e extrair todas as credenciais de arquivos em que a mídia e usá-los para aumentar privilégios.

As organizações devem controlar e monitorar a associação ao grupo todos os grupos de nível 0 (incluindo associação aninhada) incluindo:

-   Administradores corporativos

-   Administradores de domínio

-   Administrador de esquema

-   BUILTIN\Administrators

-   Operadores de conta

-   Operadores de backup

-   Operadores de impressão

-   Operadores de servidor

-   Controladores de domínio

-   Controladores de domínio somente leitura

-   Proprietários criadores de política de grupo

-   Operadores de criptografia

-   Usuários COM distribuída

-   Outros recebido grupos

    > [!NOTE]
    > "Outros grupos delegados" refere-se a grupos que podem ser criados pela sua organização para gerenciar operações de diretório que também podem ter acesso de nível 0 eficaz.

## <a name="ATLT_BM"></a>Ferramentas administrativas e tipos de Logon
Estas são as informações de referência para ajudar a identificar o risco de exposição de credenciais associada ao uso de diferentes ferramentas administrativas para administração remota.

Em um cenário de administração remota, credenciais sempre são expostas no computador de origem para que uma estação de trabalho confiável acesso privilegiado (PATA) é sempre recomendável para contas confidenciais ou alto impacto. Se as credenciais são expostas no roubo potencial no computador (remote) de destino dependem principalmente o tipo de logon do windows usado pelo método conexão.

Esta tabela inclui orientações para as ferramentas administrativas mais comuns e os métodos de conexão:

|Conexão<br />método|Tipo de logon|Credenciais reutilizáveis no destino|Comentários|
|-----------|-------|--------------------|------|
|Fazer logon no console|Interativo|v|Inclui o acesso remoto ao hardware / noturna cartões e KVMs de rede.|
|RUNAS|Interativo|v||
|RUNAS /NETWORK|NewCredentials|v|Clones sessão atual da LSA para acesso local, mas usa novas credenciais ao se conectar aos recursos de rede.|
|Área de trabalho remota (success)|RemoteInteractive|v|Se o cliente de área de trabalho remota está configurado para compartilhar dispositivos e recursos locais, aqueles podem ser comprometidas também.|
|Área de trabalho remota (falha - tipo de logon negou)|RemoteInteractive|-|Por padrão, se RDP credenciais de falha de logon são armazenadas somente muito rapidamente. Isso não pode ser o caso se o computador estiver comprometido.|
|NET use * \\\SERVER|Rede|-||
|NET use * \\\SERVER /u:user|Rede|-||
|Snap-ins do MMC para computador remoto|Rede|-|Exemplo: Gerenciador de dispositivos de gerenciamento de Visualizador de eventos, computador, serviços|
|PowerShell WinRM|Rede|-|Exemplo: PSSession insira server|
|PowerShell WinRM com CredSSP|NetworkClearText|v|Novo PSSession server<br />-Autenticação Credssp<br />-Credenciais de credencial|
|PsExec sem credenciais explícitas|Rede|-|Exemplo: PsExec \\\server cmd|
|PsExec com credenciais explícitas|Rede + interativo|v|PsExec \\\server -u usuário -p senha cmd<br />Cria várias sessões de logon.|
|Registro remoto|Rede|-||
|Gateway de área de trabalho remota|Rede|-|Autenticação de gateway de área de trabalho remota.|
|Tarefa agendada|Em lotes|v|Senha também serão salvas como segredo LSA no disco.|
|Execute ferramentas como um serviço|Serviço|v|Senha também serão salvas como segredo LSA no disco.|
|Scanners de vulnerabilidade|Rede|-|A maioria dos scanners padrão usando os logons na rede, embora alguns fornecedores podem implementar logons na rede não e apresentar mais risco de roubo de credenciais.|

Para a autenticação da web, use a referência da tabela a seguir:

|Conexão<br />método|Tipo de logon|Credenciais reutilizáveis no destino|Comentários|
|-----------|-------|--------------------|------|
|IIS "Autenticação básica"|NetworkCleartext<br />(IIS) 6.0 +<br /><br />Interativo<br />(antes do IIS 6.0)|v||
|IIS "autenticação integrada do Windows"|Rede|-|Provedores de Kerberos e NTLM.|

Definições de coluna:

-   **Tipo de logon** identifica o tipo de logon iniciado pela conexão.

-   **Credenciais reutilizáveis no destino** indica que os seguintes tipos de credencial serão armazenados na memória de processo LSASS no computador de destino onde a conta especificada está conectada localmente:

    -   Hashes do LM e NT

    -   TGTs Kerberos

    -   Senha de texto sem formatação (se aplicável).

    -

Os símbolos nesta tabela definida da seguinte maneira:

-   (-) indica quando as credenciais não são expostas.

-   (v) indica quando as credenciais são expostas.

Para aplicativos de gerenciamento que não estão nesta tabela, você pode determinar o tipo de logon do campo tipo de logon nos eventos de logon de auditoria. Para obter mais informações, consulte [eventos de logon de auditoria](https://technet.microsoft.com/en-us/library/cc787567(v=ws.10).aspx).

Em computadores baseados em Windows, todas as autenticações são processadas como um dos vários tipos de logon, independentemente de qual autenticação de protocolo ou autenticador é usado. Esta tabela inclui tipos mais comuns de logon e seus atributos em relação ao roubo de credenciais:

|Tipo de logon|#|Autenticadores aceitados|Credenciais reutilizáveis na sessão LSA|Exemplos|
|-------|---|--------------|--------------------|------|
|Interativo (também conhecida como Logon local)|2|Senha, cartão inteligente,<br />outros|Sim|Logon no console;<br />RUNAS;<br />Soluções de controle remoto de hardware (como KVM de rede ou acesso remoto / noturna cartão no servidor)<br />Autenticação básica do IIS (antes do IIS 6.0)|
|Rede|3|Senha,<br />Hash do NT,<br />Tíquete Kerberos|Não (exceto se delegação estiver ativada, Kerberos ingressos presente)|USE NET;<br />Chamadas RPC;<br />Registro remoto;<br />IIS integrado autorização do Windows;<br />SQL Windows auth;|
|Em lotes|4|Senha (geralmente armazenada como segredo LSA)|Sim|Tarefas agendadas|
|Serviço|5|Senha (geralmente armazenada como segredo LSA)|Sim|Serviços do Windows|
|NetworkCleartext|8|Senha|Sim|Autenticação básica do IIS (IIS 6.0 e versões mais recente);<br />Windows PowerShell com CredSSP|
|NewCredentials|9|Senha|Sim|RUNAS /NETWORK|
|RemoteInteractive|10|Senha, cartão inteligente,<br />outros|Sim|Área de trabalho remota (anteriormente conhecida como "Serviços de Terminal")|

Definições de coluna:

-   **Tipo de logon** é o tipo de logon solicitado.

-   **#**é o identificador numérico para o tipo de logon que é relatado em eventos de auditoria no log de eventos de segurança.

-   **Autenticadores aceitados** indica quais tipos de autenticadores são capazes de iniciar um logon desse tipo.

-   **Credenciais reutilizáveis** no LSA sessão indica se o tipo de logon resulta na sessão do LSA segurando credenciais, como senhas de texto sem formatação, hashes NT ou tíquetes Kerberos que poderiam ser usados para autenticar a outros recursos de rede.

-   **Exemplos** listar comuns cenários em que o tipo de logon é usado.

> [!NOTE]
> Para obter mais informações sobre tipos de Logon, veja [enumeração SECURITY_LOGON_TYPE](https://technet.microsoft.com/en-us/library/aa380129(VS.85).aspx).
