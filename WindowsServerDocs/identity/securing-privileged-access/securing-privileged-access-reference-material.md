---
title: Material de referência de proteção de acesso privilegiado
description: Controles de segurança operacional para domínios de Active Directory do Windows Server
ms.topic: article
ms.assetid: 22ee9a77-4872-4c54-82d9-98fc73a378c0
ms.date: 02/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: d2e7d7a064806646ccc075c96fff2ba20acfe005
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766209"
---
# <a name="active-directory-administrative-tier-model"></a>Modelo de camadas administrativas do Active Directory

>Aplica-se a: Windows Server

A finalidade desse modelo de camadas é proteger os sistemas de identidade usando um conjunto de zonas de buffer entre o controle total do ambiente (Camada 0) e os ativos de estação de trabalho de alto risco que os invasores comprometem com frequência.

![Diagrama que mostra as três camadas do modelo de Camadas](../media/securing-privileged-access-reference-material/PAW_RM_Fig1.JPG)

O modelo de Camadas é composto por três níveis e inclui somente contas administrativas, e contas de usuário não padrão:

- **Camada 0** -Controle direto de identidades corporativas no ambiente. A Camada 0 inclui contas, grupos e outros recursos que têm controle administrativo direto ou indireto sobre as florestas, domínios ou controladores de domínio do Active Directory, e todos os ativos que eles contêm. A sensibilidade da segurança de todos os ativos de Camada 0 é equivalente, pois todos estão controlando uns aos outros.
- **Camada 1** -Controle de servidores e aplicativos corporativos. Entre os ativos de Camada 1 estão sistemas operacionais de servidor, serviços de nuvem e aplicativos corporativos. Contas de administrador de Camada 1 têm controle administrativo sobre uma quantidade considerável de conteúdo corporativo hospedada nesses ativos. Uma função de exemplo comum é a de administradores de servidor, que proporciona a esses sistemas operacionais a capacidade de afetar todos os serviços corporativos.
- **Camada 2**: controle de dispositivos e estações de trabalho do usuário. Contas de administrador de Camada 2 têm controle administrativo sobre uma quantidade considerável de conteúdo corporativo hospedada em estações de trabalho e dispositivos do usuário. Entre os exemplos estão Suporte técnico e administradores de suporte ao computador, pois podem afetar a integridade de quase todos os dados do usuário.

> [!NOTE]
> As camadas também servem como um mecanismo básico de priorização para proteger ativos administrativos, mas é importante considerar que um invasor com controle de todos os ativos em qualquer camada pode acessar a maioria ou todos os ativos corporativos. O motivo disso ser útil como um mecanismo básico de priorização é a dificuldade/custo para o invasor. É mais fácil para um invasor operar com controle total de todas as identidades (Camada 0) ou de servidores e serviços em nuvem (Camada 1) do que precisar acessar cada estação de trabalho ou dispositivo de usuário individual (Camada 2) para obter dados da sua organização.

## <a name="containment-and-security-zones"></a>Zonas de segurança e contenção

As camadas são relativas a uma zona de segurança específica. Embora tenham muitos nomes, as zonas de segurança são uma abordagem bem estabelecida que proporcionam confinamento de ameaças de segurança por meio do isolamento de camada de rede entre elas. O modelo de camadas complementa o isolamento fornecendo o confinamento dos adversários dentro de uma zona de segurança em que o isolamento da rede não é eficaz. As zonas de segurança podem abranger a infraestrutura local e na nuvem, como no exemplo onde os Controladores de Domínio e membros de domínio no mesmo domínio são hospedados localmente e no Azure.

![Diagrama que mostra como as zonas de segurança podem abranger a infraestrutura de nuvem e a local](../media/securing-privileged-access-reference-material/PAW_RM_Fig2.JPG)

O modelo de camadas impede o escalonamento de privilégios, restringindo o que os administradores podem controlar e onde eles podem fazer logon (pois o logon em um computador concede controle a essas credenciais e a todos os recursos gerenciados por essas credenciais).

### <a name="control-restrictions"></a>Restrições de controle

As restrições de controle são exibidas na figura abaixo:

![Diagrama de restrições de controle](../media/securing-privileged-access-reference-material/PAW_RM_Fig3.JPG)

### <a name="primary-responsibilities-and-critical-restrictions"></a>Principais responsabilidades e restrições importantes

**Administrador de Camada 0**: gerenciar o armazenamento de identidade e uma pequena quantidade de sistemas que estão em controle efetivo dele, e:

- Pode gerenciar e controlar ativos em qualquer nível, conforme o necessário
- Pode fazer logon somente de forma interativa ou acessar ativos confiáveis no nível da Camada 0

**Administrador de nível 1**: gerenciar servidores, serviços e aplicativos corporativos, e:

- Pode gerenciar e controlar somente os ativos na Camada 1 ou Camada 2
- Pode acessar somente os ativos (por meio do tipo de logon de rede) que são confiáveis nos níveis de Camada 1 ou Camada 0
- Pode fazer logon somente de forma interativa em ativos confiáveis no nível da Camada 1

**Administrador de Camada 2**: gerenciar desktops, laptops, impressoras e outros dispositivos de usuário corporativos, e:

- Pode gerenciar e controlar somente os ativos na Camada 2
- Pode acessar ativos (por meio do tipo de logon de rede) em qualquer nível, conforme o necessário
- Pode fazer logon somente de forma interativa em ativos confiáveis no nível da Camada 2

### <a name="logon-restrictions"></a>Restrições de logon

As restrições de logon são exibidas na figura abaixo:

![Diagrama de restrições de logon](../media/securing-privileged-access-reference-material/PAW_RM_Fig4.JPG)

> [!NOTE]
> Observe que alguns ativos podem ter um impacto de Camada 0 na disponibilidade do ambiente, mas não afetam diretamente a confidencialidade ou a integridade dos ativos. Entre eles estão o serviço do Servidor DNS e dispositivos de rede críticos como proxies da Internet.

## <a name="clean-source-principle"></a>Princípio da origem limpa

O princípio de origem limpa exige que todas as dependências de segurança sejam tão confiáveis quanto o objeto que está sendo protegido.

![Diagrama que mostra como uma pessoa que tem controle de um objeto é uma dependência de segurança desse objeto](../media/securing-privileged-access-reference-material/PAW_RM_Fig5.JPG)

Qualquer pessoa que tem controle de um objeto é uma dependência de segurança desse objeto. Se um adversário puder controlar qualquer coisa que esteja em controle efetivo de um objeto alvo, ele poderá controlar esse objeto alvo. Por isso, você deve assegurar que as garantias para todas as dependências de segurança estejam no nível de segurança desejado do próprio objeto, ou acima dele.

Embora seja simples em princípio, isso exige a compreensão das relações de controle de um ativo de interesse (Objeto) e a execução de uma análise de dependência para descobrir todas as dependências de segurança (Entidade(s)).

Como o controle é transitivo, esse princípio deve ser repetido recursivamente. Por exemplo, se A controla B e B controla C, então A também controla C indiretamente.

![Diagrama que mostra que, se A controla B e B controla C, então A também controla C indiretamente](../media/securing-privileged-access-reference-material/PAW_RM_Fig6.JPG)

Um invasor que compromete A obtém acesso a tudo o que A controla (incluindo B), e a tudo o que B controla (incluindo C). Usando a linguagem de dependências de segurança nesse mesmo exemplo, B e A são dependências de segurança de C e precisam ser protegidos no nível de garantia desejado de C, para que C tenha esse nível de garantia.

Para sistemas de identidade e de infraestrutura de TI, esse princípio deve ser aplicado aos meios de controle mais comuns, incluindo o hardware no qual os sistemas estão instalados, a mídia de instalação para os sistemas, a arquitetura e a configuração do sistema e as operações diárias.

## <a name="clean-source-for-installation-media"></a>Fonte limpa da mídia de instalação

![Diagrama que mostra uma fonte limpa da mídia de instalação](../media/securing-privileged-access-reference-material/PAW_RM_CSIM.JPG)

A aplicação do princípio de origem limpa à mídia de instalação exige que você certifique-se de que a mídia de instalação não tenha sido violada desde o lançamento pelo fabricante (até onde você sabe). Esta figura mostra um invasor usando esse caminho para comprometer um computador:

![Figura que mostra um invasor usando um caminho para comprometer um computador](../media/securing-privileged-access-reference-material/PAW_RM_Fig8.JPG)

A aplicação do princípio de origem limpa à mídia de instalação exige a validação da integridade do software em todo o ciclo, desde a aquisição, o armazenamento e a transferência até a utilização.

### <a name="software-acquisition"></a>Aquisição de software

A origem do software deve ser validada usando um dos seguintes meios:

- O software é obtido de uma mídia física proveniente comprovadamente do fabricante ou de uma fonte com boa reputação, normalmente fabricada e enviada de um fornecedor.
- O software é obtido da Internet e validado com hashes de arquivo fornecidos pelo fornecedor.
- O software é obtido da Internet e validado baixando e comparando duas cópias independentes:
   - Baixe em dois hosts sem relação de segurança (não no mesmo domínio e não gerenciados pelas mesmas ferramentas), preferencialmente de conexões de Internet separadas.
   - Compare os arquivos baixados usando um utilitário como o certutil: `certutil -hashfile <filename>`

Quando possível, todos os softwares de aplicativo, como instaladores de aplicativos e ferramentas devem ser assinados digitalmente e verificados usando o Authenticode do Windows com a ferramenta [Windows Sysinternal](https://www.microsoft.com/sysinternals), *sigcheck.exe*, verificação de revogação. Nos casos em que o fornecedor não fornece esse tipo de assinatura digital, talvez alguns softwares sejam exigidos.

### <a name="software-storage-and-transfer"></a>Transferência e armazenamento de software

Depois de obter o software, ele deve ser armazenado em um local protegido contra modificação, especialmente por hosts conectados à internet ou de confiança da equipe, em um nível inferior ao dos sistemas nos quais o sistema operacional ou o software será instalado. Esse armazenamento pode ser uma mídia física ou um local eletrônico seguro.

### <a name="software-usage"></a>Uso de software

Idealmente, o software deve ser validado no momento que é usado, por exemplo, quando ele é instalado manualmente, empacotado para uma ferramenta de gerenciamento de configuração ou importado para uma ferramenta de gerenciamento de configuração.

## <a name="clean-source-for-architecture-and-design"></a>Origem limpa para arquitetura e design

A aplicação do princípio de origem limpa para a arquitetura do sistema exige que você verifique se o sistema não é dependente de sistemas de confiança inferiores. Um sistema pode ser dependente de um sistema de confiança superior, mas não de um sistema de confiança inferior com padrões de segurança inferiores.

Por exemplo, é aceitável para o Active Directory controlar um desktop de usuário padrão, mas é um escalonamento considerável de risco de privilégio um desktop de usuário padrão estar no controle do Active Directory.

![Diagrama que mostra como um sistema pode ser dependente de um sistema de confiança superior, mas não de um sistema de confiança inferior com padrões de segurança inferiores](../media/securing-privileged-access-reference-material/PAW_RM_Fig09.JPG)

A relação de controle pode ser introduzida de várias maneiras, incluindo ACLs (Listas de controle de acesso) de segurança em objetos como sistemas de arquivos, associação no grupo local de administradores em um computador ou agentes instalados em um computador executando como Sistema (com a habilidade de executar código e scripts aleatórios).

Um exemplo frequentemente negligenciado é a exposição pelo logon, que cria uma relação de controle expondo as credenciais administrativas de um sistema para outro sistema. Esse é o motivo subjacente de os ataques de roubo de credencial, como Passagem de hash, serem tão poderosos. Quando um administrador faz logon em um desktop de usuário padrão com credenciais de Camada 0, eles expõem essas credenciais a esse desktop, colocando-o em controle do AD e criando um escalonamento do caminho do privilégio ao AD. Para saber mais sobre esses ataques, consulte [esta página](/previous-versions/dn785092(v=msdn.10)).

Devido a grande quantidade de ativos que dependem de sistemas de identidade como o Active Directory, você deve minimizar o número de sistemas dos quais os Controladores de domínio e o Active Directory dependem.

![Diagrama que mostra que você deve reduzir a quantidade de sistemas de que o Active Directory e os controladores de domínio dependem](../media/securing-privileged-access-reference-material/PAW_RM_Fig010.JPG)

Para saber mais sobre como se proteger dos principais riscos do Active Directory, consulte [esta página](../ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface.md).

## <a name="operational-standards-based-on-clean-source-principle"></a>Padrões operacionais com base no princípio de origem limpa

Esta seção descreve os padrões operacionais e as expectativas para a equipe administrativa. Esses padrões são projetados para proteger o controle administrativo dos sistemas de tecnologia da informação de uma organização contra riscos que podem ser criados por processos e práticas operacionais.

![Diagrama que mostra como os padrões são projetados para proteger o controle administrativo dos sistemas de tecnologia da informação de uma organização contra riscos que podem ser criados por processos e práticas operacionais](../media/securing-privileged-access-reference-material/PAW_RM_Fig11.JPG)

### <a name="integrating-the-standards"></a>Integração dos padrões

Você pode integrar esses padrões em padrões nas práticas e padrões gerais de sua organização. Você pode adaptá-los aos requisitos específicos, às ferramentas disponíveis e ao apetite de risco de sua organização, mas recomendamos somente modificações mínimas a fim de reduzir o risco. Recomendamos que você use os padrões neste guia como referência para seu estado final ideal e gerenciar quaisquer deltas como exceções a serem resolvidas em ordem de prioridade.

O guia de padrões é organizado nas seguintes seções:

- Suposições
- Comitê de aconselhamento sobre mudanças
- Práticas operacionais
   - Resumo
   - Detalhes dos padrões

### <a name="assumptions"></a>Suposições

Os padrões nesta seção pressupõem que a organização tem os seguintes atributos:

- A maioria dos servidores e estações de trabalho, ou todos eles, no escopo é associada ao Active Directory.
- Todos os servidores a serem gerenciados estão executando o Windows Server 2008 R2 ou posterior e têm o modelo RDP RestrictedAdmin habilitado.
- Todas as estações de trabalho a serem gerenciadas estão executando o Windows 7 ou posterior e têm o modelo RDP RestrictedAdmin habilitado.

   > [!NOTE]
   > Para habilitar o modo RDP RestrictedAdmin, consulte [esta página](https://aka.ms/RDPRA).

- Os cartões inteligentes estão disponíveis e são emitidos para todas as contas administrativas.
- O *Builtin\Administrador* para cada domínio foi designado como uma conta de acesso de emergência
- Uma solução de gerenciamento de identidade corporativa é implantada.
- [LAPS](https://aka.ms/laps) foi implantado em servidores e estações de trabalho para gerenciar a senha da conta de administrador local
- Há uma solução de gerenciamento de acesso privilegiado, como o Microsoft Identity Manager, implementada, ou há um plano para adotar uma.
- A equipe é designada para monitorar alertas de segurança e responder a eles.
- O recurso técnico para aplicar rapidamente as atualizações de segurança da Microsoft está disponível.
- Os controladores de gerenciamento da placa-base em servidores não serão usados ou seguirão controles rígidos de segurança.
- Contas de administrador e grupos de servidores (administradores de Camada 1) e estações de trabalho (administradores de Camada 2) serão gerenciados por administradores de domínio (Camada 0).
- Há um CAB (Comitê de aconselhamento sobre mudanças) ou outra autoridade designada implementada para aprovação de alterações no Active Directory.

### <a name="change-advisory-board"></a>Comitê de aconselhamento sobre mudanças

Um CAB (Comitê de aconselhamento sobre mudanças) é o fórum de discussão e a autoridade de aprovação para alterações que poderiam afetar o perfil de segurança da organização. Quaisquer exceções a esses padrões devem ser enviadas ao CAB com uma avaliação de risco e a justificativa.

Cada padrão neste documento é dividido de acordo com o nível de importância em atender ao padrão de um determinado nível de Camada.

![Diagrama que mostra o padrão para os níveis de camada fornecidos](../media/securing-privileged-access-reference-material/PAW_RM_Fig12.JPG)

Todas as exceções para os itens Obrigatórios (marcados com octógono vermelho ou com um triângulo laranja neste documento) são consideradas temporárias e precisam ser aprovadas pelo CAB. As diretrizes incluem:

- A solicitação inicial exige a aceitação do risco e a justificativa assinadas pelo supervisor imediato da equipe, e expira após seis meses.
- Renovações exigem justificativa e aceitação de risco assinadas por um diretor da unidade de negócios, e expiram depois de seis meses.

Todas as exceções para os itens Recomendados (marcados com um círculo amarelo neste documento) são consideradas temporárias e precisam ser aprovadas pelo CAB. As diretrizes incluem:

- A solicitação inicial exige a aceitação do risco e a justificativa assinadas pelo supervisor imediato da equipe, e expira após 12 meses.
- Renovações exigem justificativa e aceitação de risco assinadas por um diretor da unidade de negócios, e expiram depois de 12 meses.

### <a name="operational-practices-standards-summary"></a>Resumo dos padrões de práticas operacionais

As colunas de Camadas nessa tabela referem-se ao nível de Camada da conta administrativa, da qual o controle geralmente afeta todos os ativos na camada.

![Tabela que mostra os níveis de camada da conta administrativa](../media/securing-privileged-access-reference-material/PAW_RM_Fig13.JPG)

Decisões operacionais feitas com frequência são importantes para manter a postura de segurança do ambiente. Esses padrões para processos e práticas recomendadas ajudam a garantir que um erro operacional não resulte em uma vulnerabilidade operacional explorável no ambiente.

#### <a name="administrator-enablement-and-accountability"></a>Responsabilidade e habilitação do administrador

Os administradores devem ser informados, capacitados, treinados e responsabilizados pela operação do ambiente da forma mais segura possível.

##### <a name="administrative-personnel-standards"></a>Padrões da equipe administrativa

A equipe administrativa atribuída precisa ser verificada a fim de garantir que seja confiável e que precisa de privilégios administrativos:

- Execute verificações de histórico na equipe antes de atribuir privilégios administrativos.
- Revise os privilégios administrativos a cada trimestre para determinar qual equipe ainda tem uma necessidade legítima de acesso administrativo.

##### <a name="administrative-security-briefing-and-accountability"></a>Responsabilidade e resumo de segurança administrativa

Os administradores devem ser informados e responsabilizados pelos riscos à organização e pela função de gerenciamento desse risco. Os administradores devem ser treinados anualmente em:

- Ambiente geral de ameaças
   - Adversários determinados
   - Técnicas de ataque, incluindo Passagem de Hash e roubo de credenciais
- Ameaças e incidentes específicos à organização
- Funções de administrador na proteção contra ataques
   - Gerenciamento de exposição de credencial com o modelo de Camadas
   - Uso de estações de trabalho administrativas
   - Uso do modo RestrictedAdmin do protocolo RDP
- Práticas administrativas específicas à organização
   - Examinar todas as diretrizes operacionais nesse padrão
   - Implemente as seguintes regras principais:
      - Não use contas administrativas em nada que não seja estações de trabalho administrativas
      - Não desative ou desmonte controles de segurança em sua conta ou estações de trabalho (por exemplo, restrições de logon ou atributos necessários para cartões inteligentes)
      - Relatar problemas ou atividades incomuns

Para proporcionar responsabilidade, todos os funcionários com contas administrativas devem assinar um documento sobre Código de conduta para privilégio administrativo afirmando que pretendem seguir práticas de política administrativa específicas da organização.

##### <a name="provisioning-and-deprovisioning-processes-for-administrative-accounts"></a>Processos de provisionamento e desprovisionamento para contas administrativas

Deve-se seguir estes padrões a fim de atender aos requisitos do ciclo de vida.

- Todas as contas administrativas devem ser aprovadas pela Autoridade de Aprovação descrita na tabela a seguir.
   - Deve-se conceder aprovação apenas se a equipe tiver uma verdadeira necessidade comercial de privilégios administrativos.
   - A aprovação de privilégios administrativos não deve exceder seis meses.
- O acesso a privilégios administrativos deve ser imediatamente desprovisionado quando:
   - O funcionário muda de cargo.
   - O funcionário sai da organização.
- As contas devem ser desabilitadas imediatamente após o funcionário sair da organização.
- Contas desativadas devem ser excluídas em até seis meses, e o registro da exclusão deve ser inserido nos registros do comitê de aprovação de mudança.
- Examine mensalmente todas as associações de conta privilegiada a fim de garantir que nenhum permissão não autorizada seja concedida. Isso pode ser substituído por uma ferramenta automatizada que alerta sobre as mudanças.

|Nível de privilégio da conta|Autoridade de aprovação|Frequência de revisão de associação|
|--------------|------------|----------------|
|Administrador de Camada 0|Comitê de aprovação de mudança|Mensal ou automatizado|
|Administrador de Camada 1|Administradores ou segurança de Camada 0|Mensal ou automatizado|
|Administrador de Camada 2|Administradores ou segurança de Camada 0|Mensal ou automatizado|

#### <a name="operationalize-least-privilege"></a>Colocar em operação com o privilégio mínimo

Esses padrões ajudam a obter os privilégios mínimos reduzindo o número de administradores na função e a quantidade de tempo durante o qual eles têm privilégios.

> [!NOTE]
> A obtenção dos privilégios mínimos em sua organização exigirá a compreensão das funções organizacionais, seus requisitos e seus mecanismos de criação, a fim de garantir que eles sejam capazes de realizar seu trabalho usando o privilégio mínimo. O alcance de um estado de privilégio mínimo em um modelo administrativo exige com frequência o uso de várias abordagens:
>
> - Limitar a contagem de administradores ou membros de grupos privilegiados
> - Delegar menos privilégios às contas
> - Fornecer privilégios com limite de tempo sob demanda
> - Fornecer a capacidade de outras equipes realizarem tarefas (uma abordagem concierge)
> - Fornecer processos para acesso de emergência e cenários de uso raros

##### <a name="limit-count-of-administrators"></a>Limitar a contagem de administradores

No mínimo duas pessoas qualificadas deve ser atribuídas a cada função administrativa a fim de garantir a continuidade dos negócios.

Se o número de funcionários atribuídos a qualquer função exceder dois, o comitê de aprovação de mudanças deve aprovar os motivos específicos da atribuição dos privilégios a cada membro (incluindo os dois originais). A justificativa para a aprovação deve incluir:

- Quais tarefas técnicas são executadas pelos administradores que exigem privilégios administrativos
- Com que frequência as tarefas são executadas
- Motivo específico de as tarefas não poderem ser executadas por outro administrador em nome dele
- Documente todas as outras abordagens alternativas conhecidas para conceder o privilégio e por que cada uma delas não é aceitável

##### <a name="dynamically-assign-privileges"></a>Atribuir privilégios dinamicamente

Os administradores precisam obter permissões "just-in-time" a fim de usá-las conforme realizam as tarefas. Nenhuma permissão será permanentemente atribuída às contas administrativas.

> [!NOTE]
> Privilégios administrativos permanentemente atribuídos naturalmente criam uma estratégia de "a maioria dos privilégios", pois a equipe administrativa exige acesso rápido às permissões a fim de manter a disponibilidade operacional se houver algum problema. Permissões just-in-time fornecem a capacidade de:
>
> - Atribuir permissões de forma mais granular, chegando mais perto do privilégio mínimo.
> - Reduzir o tempo de exposição dos privilégios
> - Controlar o uso das permissões para detectar abuso ou ataques.

#### <a name="manage-risk-of-credential-exposure"></a>Gerenciar o risco de exposição de credencial

Use as práticas a seguir para gerenciar o risco de exposição de credencial apropriado.

##### <a name="separate-administrative-accounts"></a>Separar contas administrativas

Todas as pessoas com autorização para ter privilégios administrativos devem ter contas separadas para funções administrativas diferentes das contas de usuário.

- **Contas de usuário padrão** privilégios de usuário padrão concedidos para tarefas de usuário padrão, como email, navegação na Web e uso de aplicativos de linha de negócios. Essas contas não devem receber privilégios administrativos.
- **Contas administrativas**: contas separadas, criadas para funcionários que recebem os privilégios administrativos apropriados. Um administrador que precisa gerenciar ativos em cada Camada deve ter uma conta separada para cada Camada. Essas contas não devem ter acesso a email ou à Internet pública.

##### <a name="administrator-logon-practices"></a>Práticas de logon de administrador

Antes que um administrador possa fazer logon em um host de forma interativa (localmente por RDP padrão, usando Executar como ou por meio do console de virtualização), esse host deve atender ou exceder ao padrão para a Camada de conta de administrador (ou uma Camada superior).

Os administradores só podem entrar nas estações de trabalho de administrador com suas contas administrativas. Os administradores fazem logon nos recursos gerenciados usando a tecnologia de suporte aprovada descrita na próxima seção.

> [!NOTE]
> Isso é necessário porque fazer logon interativamente em um host concede controle das credenciais nesse host.
>
> Consulte [Ferramentas administrativas e tipos de logon]() para obter detalhes sobre os tipos de logon, ferramentas de gerenciamento comuns e exposição de credencial.

##### <a name="use-of-approved-support-technology-and-methods"></a>Uso da tecnologia e métodos de suporte aprovados

Os administradores que oferecem suporte a usuários e sistemas remotos devem seguir estas diretrizes a fim de impedir que um adversário no controle do computador remoto roube suas credenciais administrativas.

- As principais opções de suporte devem ser usadas se estiverem disponíveis.
- As opções de suporte secundárias devem ser usadas somente se a opção de suporte principal não estiver disponível.
- Métodos de suporte proibidos nunca devem ser usados.
- O acesso a emails ou a navegação sem internet pode ser realizada por qualquer conta de administrador, a qualquer momento.

###### <a name="tier-0-forest-domain-and-dc-administration"></a>Floresta, domínio e administração do controlador de domínio de Camada 0

Certifique-se de que as práticas a seguir sejam aplicadas neste cenário:

- **Suporte de servidor remoto**: ao acessar remotamente um servidor, os administradores de Camada 0 devem seguir estas diretrizes:
  - **Primário (ferramenta)** ferramentas remotas que usam logons de rede (tipo 3). Para saber mais, consulte [Ferramentas administrativas e tipos de logon]().
  - **Primário (interativo)** use RDP RestrictedAdmin ou uma sessão RDP padrão de uma estação de trabalho do administrador com uma conta de domínio

    > [!NOTE]
    > Se você tiver uma solução de gerenciamento de privilégio de Camada 0, adicione "que usa permissões obtidas just-in-time de uma solução de gerenciamento de acesso privilegiado."

- **Suporte do servidor físico** Quando estiverem fisicamente presentes em um console do servidor ou em um console de máquina virtual (ferramentas Hyper-V ou VMWare), essas contas não terão restrições específicas de uso de ferramentas administrativas, somente as restrições gerais das tarefas de usuário padrão, como email e navegação na internet aberta.

   > [!NOTE]
   > A administração de Camada 0 é diferente da administração de outras camadas, pois todos os ativos de Camada 0 já tem controle direto ou indireto de todos os ativos. Por exemplo, um invasor no controle de um DC não precisa roubar as credenciais de administradores conectados, já que têm acesso a todas as credenciais de domínio no banco de dados.

###### <a name="tier-1-server-and-enterprise-application-support"></a>Suporte a aplicativo corporativo e de servidor de Camada 1

Certifique-se de que as práticas a seguir sejam aplicadas neste cenário:

- **Suporte de servidor remoto**: ao acessar remotamente um servidor, os administradores de Camada 1 devem seguir estas diretrizes:
   - **Primário (ferramenta)** ferramentas remotas que usam logons de rede (tipo 3). Para saber mais, consulte [Minimizar a Passagem o Hash e outros roubos de credenciais](https://www.microsoft.com/pth) v1 (páginas 47 a 42).
   - **Principal (interativo)** : use RDP RestrictedAdmin de uma estação de trabalho administrativa com uma conta de domínio que usa permissões just-in-time obtidas de uma solução de gerenciamento de acesso privilegiado.
   - **Secundário**: faça logon no servidor usando uma senha de conta local definida por LAPS em uma estação de trabalho de administrador.
   - **Proibido**: não use RDP padrão com uma conta de domínio.
   - **Proibido**: o uso de credenciais de conta de domínio durante a sessão (por exemplo, usando *RunAs* ou autenticando em um compartilhamento). Isso expõe as credenciais de logon ao risco de roubo.
- **Suporte de servidor físico**: quando estiverem fisicamente presentes em um console do servidor ou em um console de máquina virtual (ferramentas Hyper-V ou VMWare), os administradores da Camada 1 devem recuperar a senha da conta local no LAPS antes de acessar o servidor.
   - **Primário**: recupere a senha da conta local definida por LAPS em uma estação de trabalho de administração antes de fazer logon no servidor.
   - **Proibido**: o logon com uma conta de domínio não é permitido neste cenário.
   - **Proibido**: o uso de credenciais de conta de domínio durante a sessão (por exemplo, RunAs ou autenticando em um compartilhamento). Isso expõe as credenciais de logon ao risco de roubo.

###### <a name="tier-2-help-desk-and-user-support"></a>Suporte técnico e suporte ao usuário da Camada 2

O suporte técnico e organizações de suporte ao usuário executam suporte para usuários finais (o que não exige privilégios administrativos) e para as estações de trabalho do usuário (o que exige privilégios administrativos).

**Suporte ao usuário**: entre as tarefas estão o auxílio a usuários na execução de tarefas que não exigem modificação na estação de trabalho, mostrando frequentemente como usar um recurso de aplicativo ou recurso do sistema operacional.

- **Suporte ao usuário no escritório**: a equipe de suporte de Camada 2 está fisicamente presente no workspace do usuário.
   - **Primário**: suporte "Over the shoulder" (por cima do ombro) pode ser fornecido sem qualquer ferramenta.
   - **Proibido**: o logon com credenciais administrativas de uma conta de domínio não é permitido neste cenário. Alterne para o suporte de estação de trabalho no escritório se houver a necessidade de privilégios administrativos.
- **Suporte remoto ao usuário**: a equipe de suporte de Camada 2 é fisicamente remota com relação ao usuário.
   - **Primário**: é possível usar Assistência Remota, Skype for Business ou um compartilhamento de tela do usuário semelhante. Para saber mais, consulte [O que é a Assistência Remota do Windows?](https://windows.microsoft.com/windows/what-is-windows-remote-assistance)
   - **Proibido**: o logon com credenciais administrativas de uma conta de domínio não é permitido neste cenário. Alterne para o suporte de estação de trabalho se houver a necessidade de privilégios administrativos.
- **Suporte de estação de trabalho**: entre as tarefas estão a manutenção ou solução de problemas da estação de trabalho, que exigem acesso a um sistema para exibição de logs, instalação de software, atualização de drivers e assim por diante.
   - **Suporte à estação de trabalho no escritório**: a equipe de suporte de Camada 2 está fisicamente presente na estação de trabalho do usuário.
      - **Primário**: recupere a senha da conta local definida por LAPS em uma estação de trabalho de administração antes de conectar-se à estação de trabalho do usuário.
      - **Proibido**: o logon com credenciais administrativas de uma conta de domínio não é permitido neste cenário.
   - **Suporte remoto à estação de trabalho**: a equipe de suporte de Camada 2 é fisicamente remota com relação à estação de trabalho do usuário.
      - **Primário**: use RDP RestrictedAdmin de uma estação de trabalho administrativa com uma conta de domínio que usa permissões obtidas just-in-time de uma solução de gerenciamento de acesso privilegiado.
      - **Secundário**: recupere uma senha da conta local definida por LAPS em uma estação de trabalho de administração antes de conectar-se à estação de trabalho do usuário.
      - **Proibido**: use o RDP padrão com uma conta de domínio.

###### <a name="no-browsing-the-public-internet-with-admin-accounts-or-from-admin-workstations"></a>Sem navegação na Internet pública com contas de administrador ou de estações de trabalho do administrador

A equipe administrativa não pode navegar pela Internet aberta enquanto estiver conectada com uma conta administrativa ou a uma estação de trabalho administrativa. As únicas exceções autorizadas são o uso de um navegador da Web para administrar um serviço baseado em nuvem.

###### <a name="no-accessing-email-with-admin-accounts-or-from-admin-workstations"></a>Não há acesso ao email com contas de administrador ou de estações de trabalho do administrador

A equipe administrativa não pode acessar email enquanto estiver conectada com uma conta administrativa ou a uma estação de trabalho administrativa.

###### <a name="store-service-and-application-account-passwords-in-a-secure-location"></a>Armazene as senhas de conta de aplicativo e de serviço em um local seguro

As diretrizes a seguir devem ser usadas para processos de segurança física que controlam o acesso à senha:

- Guarde as senhas de conta de serviço em um cofre físico.
- Garanta que apenas funcionários confiáveis que estejam dentro da classificação de Camada da conta, ou acima dela, tenham acesso à senha da conta.
- Limite o número de pessoas que acessam as senhas para o mínimo a fim de preservar a responsabilidade .
- Certifique-se de que todo acesso à senha seja conectado, controlado e monitorado por um terceiro desinteressado, como um gerente, que não foi treinado para realizar a administração de TI.

##### <a name="strong-authentication"></a>Autenticação forte

Use as práticas a seguir para configurar apropriadamente a autenticação forte.

###### <a name="enforce-smartcard-multi-factor-authentication-mfa-for-all-admin-accounts"></a>Imponha a MFA (autenticação multifator) de cartão inteligente para todas as contas de administrador

Nenhuma conta administrativa tem permissão para usar uma senha para autenticação. As únicas exceções autorizadas são as contas de acesso de emergência protegidas pelo processo apropriado.

Vincule todas as contas administrativas a um cartão inteligente e habilite o atributo "**Cartão inteligente exigido para Logon Interativo**."

Um script deve ser implementado para redefinir periódica e automaticamente o valor de hash de senha aleatória desabilitando e reabilitando imediatamente o atributo "**Cartão inteligente exigido para Logon Interativo**."

Não permita exceções para contas usadas por funcionários humanos além das contas de acesso de emergência.

###### <a name="enforce-multi-factor-authentication-for-all-cloud-admin-accounts"></a>Impor a autenticação multifator a todas as contas de administrador na nuvem

Todas as contas com privilégios administrativos em um serviço de nuvem, como o Microsoft Azure e o Office 365, devem usar a autenticação multifator.

##### <a name="rare-use-emergency-procedures"></a>Procedimentos de emergência para uso raro

Práticas operacionais devem oferecer suporte aos padrões a seguir:

- Certifique-se de que as interrupções possam ser resolvidas rapidamente.
- Certifique-se de que tarefas raras de alto privilégio possam ser concluídas conforme o necessário.
- Certifique-se de que os procedimentos seguros sejam usados para proteger as credenciais e os privilégios.
- Certifique-se de que os processos de aprovação e controle apropriados sejam seguidos.

###### <a name="correctly-follow-appropriate-processes-for-all-emergency-access-accounts"></a>Siga corretamente os processos adequados para todas as contas de acesso de emergência

Certifique-se de que cada conta de acesso de emergência tenha uma planilha de controle armazenada em segurança.

O procedimento documentado na planilha de controle de senha deve ser seguido para cada conta, o que inclui a alteração da senha após cada uso e a desconexão de qualquer estação de trabalho ou servidor após a conclusão.

Qualquer utilização das contas de acesso de emergência devem ser aprovadas pelo Comitê de aprovação de mudança ou após o ocorrido como um uso de emergência aprovado.

###### <a name="restrict-and-monitor-usage-of-emergency-access-accounts"></a>Restringir e monitorar o uso de contas de acesso de emergência

Para qualquer utilização das contas de acesso de emergência:

- Somente administradores de domínio autorizados podem acessar as contas de acesso de emergência com privilégios de administrador de domínio.
- As contas de acesso de emergência podem ser usadas somente em controladores de domínio e outros hosts de Camada 0.
- Esta conta deve ser usada somente para:
  - Executar a solução de problemas e correção de problemas técnicos que estão impedindo o uso das contas administrativas corretas.
  - Execute tarefas raras, como:
    - Administração de esquema
    - Tarefas em toda a floresta que exigem privilégios administrativos corporativos

      > [!NOTE]
      > O gerenciamento de topologia, incluindo o gerenciamento de site e sub-rede do Active Directory, é delegado a fim de limitar o uso desses privilégios.

- Qualquer utilização de uma dessas contas deve ter autorização por escrito do líder do grupo de segurança
- O procedimento na folha de controle para cada conta de acesso de emergência exige que a senha seja alterada em cada uso. Um membro da equipe de segurança deve validar se isso aconteceu corretamente.

###### <a name="temporarily-assign-enterprise-admin-and-schema-admin-membership"></a>Atribuir temporariamente a associação de administração corporativa e de esquema de administrador

Os privilégios devem ser adicionados conforme o necessário e removidos após o uso. A conta de emergência deve receber esses privilégios somente durante a execução da tarefa e por no máximo 10 horas. Qualquer utilização e duração desses privilégios devem ser capturados no Comitê de aprovação de mudança após a conclusão da tarefa.

## <a name="esae-administrative-forest-design-approach"></a>Abordagem de Design de Floresta Administrativa ESAE

Esta seção contém uma abordagem para uma floresta administrativa baseada na arquitetura de referência ESAE (Ambiente administrativo de segurança aprimorada) implantada por equipes de serviços profissionais de segurança cibernética da Microsoft a fim de proteger os clientes contra ataques de segurança cibernética.

Florestas administrativas dedicadas permitem que as organizações hospedem contas administrativas, estações de trabalho e grupos em um ambiente com controles de segurança mais fortes do que nos ambientes de produção.

Essa arquitetura permite uma série de controles de segurança que não são possíveis, nem facilmente configurados, em uma arquitetura de floresta única, mesmo um gerenciado com PAWS (Estações de trabalho com acesso privilegiado). Essa abordagem permite o provisionamento de contas como usuários padrão sem privilégios na floresta administrativa que tem altos níveis de privilégio no ambiente de produção, possibilitando uma maior imposição técnica de controle. Essa arquitetura também permite o uso do recurso de autenticação seletiva de uma relação de confiança como um meio de restringir logons (e a exposição de credenciais) apenas para hosts autorizados. Em situações em que se deseje um nível maior de segurança para a floresta de produção sem incorrer em custos e complexidade de uma recompilação completa, uma floresta administrativa pode fornecer um ambiente que aumente o nível de garantia do ambiente de produção.

Embora essa abordagem adicione uma floresta a um ambiente do Active Directory, o custo e a complexidade são limitados pelo projeto fixo, superfície pequena de hardware/software e pequena quantidade de usuários.

> [!NOTE]
> Essa abordagem funciona bem para a administração do Active Directory, mas muitos aplicativos não são compatíveis com a administração por contas de uma floresta externa usando uma relação de confiança padrão.

Esta figura mostra uma floresta ESAE usada para administração de ativos de Camada 0 e uma floresta PRIV configurada para ser usada com o recurso Gerenciamento de Acesso Privilegiado do Microsoft Identity Manager. Para saber mais sobre como implantar uma instância de PAM do MIM, consulte o artigo [Privileged Identity Management para Active Directory Domain Services](/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services).

![Figura que mostra uma floresta ESAE usada para administração de ativos de Camada 0 e uma floresta PRIV configurada para ser usada com o recurso Gerenciamento de Acesso Privilegiado do Microsoft Identity Manager](../media/securing-privileged-access-reference-material/PAW_RM_Fig14.JPG)

Uma floresta administrativa dedicada é uma floresta do Active Directory de domínio único padrão, dedicada à função de gerenciamento do Active Directory. Domínios e florestas administrativas podem ser protegidos de forma mais rigorosa do que as florestas de produção, devido aos casos de uso limitados.

Um design de floresta administrativa deve incluir as seguintes considerações:

- **Escopo limitado** – O valor primário de uma floresta de administrador é o alto nível de garantia de segurança e a redução da superfície de ataque, resultando em menor risco residual. A floresta pode ser usada para hospedar aplicativos e funções de gerenciamento adicionais, mas cada aumento no escopo aumenta a superfície de ataque da floresta e de seus recursos. O objetivo é limitar as funções da floresta e dos usuários administradores internos para manter a superfície de ataque mínima, para que cada aumento de escopo seja considerado com cautela.
- **Configurações de relação de confiança** – Configure a relação de confiança da(s) floresta(s) ou do(s) domínio(s) gerenciado(s) para a floresta administrativa
   - Uma relação de confiança unidirecional é exigida pelo ambiente de produção para a floresta administrativa. Isso pode ser uma relação de confiança de domínio ou uma relação de confiança de floresta. O domínio/floresta de administrador não precisa confiar nos domínios/florestas gerenciadas para gerenciar o Active Directory, embora outros aplicativos possam exigir uma relação de confiança bidirecional, validação de segurança e testes.
   - A autenticação seletiva deve ser usada para restringir contas na floresta de administrador para fazer logon apenas nos hosts de produção apropriados. Para manter os controladores de domínio e direitos de delegação no Active Directory, isso geralmente exige a concessão do direito “Autorizado a fazer logon” de controladores de domínio a contas de administrador da Camada 0 designadas na floresta de administrador. Consulte Definir as configurações de autenticação seletiva para saber mais.
- **Privilégios e proteção do domínio**: a floresta administrativa deve ser configurada para o privilégio mínimo com base nos requisitos de administração do Active Directory.
   - A concessão de direitos para administrar controladores de domínio e delegar permissões exige a adição de contas de floresta de administrador ao grupo de domínio local BUILTIN\Administradores. Isso ocorre porque o grupo global de Administradores de Domínio não pode ter membros de um domínio externo.
   - Uma limitação ao uso desse grupo para concessão de direitos é que eles não têm acesso administrativo a novos objetos de política de grupo por padrão. Isso pode ser alterado seguindo o procedimento [neste artigo da base de dados de conhecimento](https://support.microsoft.com/kb/321476) para alteração das permissões do esquema padrão.
   - Contas da floresta de administrador que são usadas para administrar o ambiente de produção não devem receber privilégios administrativos para a floresta de administração nem para domínios ou estações de trabalho nela.
   - Privilégios administrativos sobre a floresta de administrador devem ser controlados rigorosamente por um processo offline, a fim de reduzir a oportunidade de um invasor ou funcionário mal-intencionado em posse de informações privilegiadas apagar os logs de auditoria. Isso também ajuda a garantir que a equipe em posse das contas de administrador de produção não reduza as restrições sobre suas contas e aumente o risco para a organização.
   - A floresta administrativa deve seguir as configurações da SCB (Microsoft Security Compliance Baseline) para o domínio, inclusive configurações fortes para protocolos de autenticação.
   - Todos os hosts da floresta de administrador devem ser atualizados automaticamente com atualizações de segurança. Embora isso possa criar um risco de interromper as operações de manutenção do controlador de domínio, isso fornece uma redução significativa dos riscos de segurança de vulnerabilidades sem patch.

      > [!NOTE]
      > Uma instância dedicada do Windows Server Update Services pode ser configurada para aprovar automaticamente as atualizações. Para saber mais, consulte a seção “Aprovar automaticamente atualizações para instalação” em Aprovar atualizações.

- **Proteção da estação de trabalho**: compile as estações de trabalho administrativas usando as [Estações de trabalho com acesso privilegiado](../securing-privileged-access/privileged-access-workstations.md) (por meio da Fase 3), mas altere a associação de domínio na floresta administrativa em vez do ambiente de produção.
- **Proteção do servidor e do DC**: para todos os controladores de domínio e servidores na floresta administrativa:
   - Certifique-se de que todas as mídias sejam validadas usando as diretrizes em [Origem limpa para a mídia de instalação]()
   - Certifique-se de que os servidores de floresta administrativa tenham os sistemas operacionais mais recentes instalados, mesmo que isso não seja viável em produção.
   - Os hosts da floresta de administrador devem ser atualizados automaticamente com atualizações de segurança.

      > [!NOTE]
      > O Windows Server Update Services pode ser configurado para aprovar automaticamente as atualizações. Para saber mais, consulte a seção “Aprovar automaticamente atualizações para instalação” em Aprovar atualizações.

   - Linhas de base de segurança devem ser usadas como configurações iniciais.

      > [!NOTE]
      > Os clientes podem usar o SCT (Microsoft Security Compliance Toolkit) para configurar as linhas de base nos hosts administrativos.

   - Inicialização Segura para reduzir tentativas de ataques ou de malware que poderiam carregar um código não assinado no processo de inicialização.

      > [!NOTE]
      > Esse recurso foi introduzido no Windows 8 para utilizar a UEFI (Unified Extensible Firmware Interface).

   - Criptografia de volume completo para reduzir a perda física de computadores, como laptops administrativos usados remotamente.

      > [!NOTE]
      > Consulte [BitLocker](/previous-versions/windows/it-pro/windows-8.1-and-8/dn641993(v=ws.11))para saber mais.

   - Restrições de USB para proteger contra vetores de infecção física.

      > [!NOTE]
      > Consulte [Controlar o acesso de leitura ou gravação para dispositivos ou mídias removíveis](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730808(v=ws.10)) para saber mais.

   - Isolamento de rede para proteger contra ataques de rede e ações acidentais do administrador. Os firewalls de host devem bloquear todas as conexões de entrada, exceto aquelas explicitamente necessárias, e bloquear todo o acesso à Internet de saída.

   - Antimalware para proteger contra ameaças conhecidas e malware.

   - Análise da superfície de ataque para evitar a introdução de novos vetores de ataque no Windows durante a instalação de novo software.

      > [!NOTE]
      > O uso de ferramentas como o [ASA (Attack Surface Analyzer)](https://www.microsoft.com/download/details.aspx?id=24487) ajudará a avaliar as definições de configuração em um host e a identificar os vetores de ataque introduzidos por software ou alterações na configuração.

- Proteção de conta
   - A Autenticação multifator deve ser configurada para todas as contas na floresta administrativa, exceto uma conta. Pelo menos uma conta administrativa deve ter base em senha para garantir que o acesso funcionará no caso de quebra de processos da autenticação multifator. Essa conta deve ser protegida por um processo rigoroso de controle físico.
   - Contas configuradas para autenticação multifator devem ser configuradas para definir regularmente um novo hash NTLM nas contas. Isso pode ser feito desabilitando e habilitando o atributo de conta Cartão inteligente necessário para logon interativo.

      > [!NOTE]
      > Isso pode interromper as operações em andamento que estão usando essa conta. Portanto, esse processo deve ser iniciado apenas quando os administradores não forem usar a conta, por exemplo, à noite ou nos fins de semana.

- Controles de detecção
   - Os controles de detecção para a floresta administrativa devem ser desenvolvidos para alertar sobre anomalias na floresta de administrador. O número limitado de cenários e atividades autorizadas pode ajudar no ajuste desses controles de forma mais precisa do que o ambiente de produção.

Para saber mais sobre os serviços da Microsoft para criar e implantar um ESAE em seu ambiente, consulte [esta página](https://www.microsoft.com/services).

## <a name="tier-0-equivalency"></a>Equivalência de camada 0

A maioria das organizações controla a associação a grupos poderosos do Active Directory de Camada 0 como Administradores, Administradores de Domínio e Administradores de Empresa.  Muitas organizações ignoram o risco de outros grupos efetivamente equivalentes em privilégio em um ambiente comum do Active Directory. Esses grupos oferecem um caminho de escalonamento relativamente fácil para um invasor aos mesmos privilégios explícitos de Camada 0 usando vários métodos diferentes de ataque.

Por exemplo, um operador de servidor pode obter acesso a uma mídia de backup de um controlador de domínio e extrair todas as credenciais dos arquivos nessa mídia, e usá-los para escalonar privilégios.

As organizações devem controlar e monitorar a associação em todos os grupos de Camada 0 (incluindo a associação aninhada) incluindo:

- Administrador corporativo
- Administradores do domínio
- Administradores de Esquema
- BUILTIN\Administrators
- Opers. de contas
- Operadores de cópia
- Operadores de Impressão
- Opers. de servidores
- Controladores de Domínio
- Controladores de Domínio somente leitura
- Proprietários criadores de política de grupo
- Operadores criptográficos
- Usuários COM Distribuídos
- Outros grupos delegados – grupos personalizados que podem ser criados por sua organização para gerenciar operações de diretório que também podem ter acesso efetivo de Camada 0.

## <a name="administrative-tools-and-logon-types"></a>Ferramentas administrativas e tipos de logon

Essas são informações de referência para ajudar a identificar o risco de exposição de credencial associada ao uso de diferentes ferramentas administrativas para administração remota.

Em um cenário de administração remota, as credenciais sempre são expostas no computador de origem. Por isso, recomendamos sempre uma PAW (Estação de trabalho com acesso privilegiado) confiável para contas confidenciais ou de alto impacto. A exposição das credenciais a possíveis roubos no computador de destino (remoto) dependem principalmente do tipo de logon do Windows usado pelo método de conexão.

Esta tabela inclui orientações para as ferramentas administrativas e métodos de conexão mais comuns:

|Método de conexão|Tipo de logon|Credenciais reutilizáveis no destino|Comentários|
|-----------|-------|--------------------|------|
|Logon no console|Interactive (Interativo)|v|Inclui o acesso remoto do hardware/placas lights-out e KVMs de rede.|
|RUNAS|Interactive (Interativo)|v||
|RUNAS /NETWORK|NewCredentials|v|Clona a sessão atual do LSA para acesso local, mas usa novas credenciais ao se conectar aos recursos da rede.|
|Área de Trabalho Remota (sucesso)|RemoteInteractive|v|Se o cliente de Área de Trabalho Remota estiver configurado para compartilhar dispositivos e recursos locais, esses últimos podem ficar comprometidos também.|
|Área de Trabalho Remota (falha: tipo de logon negado)|RemoteInteractive|-|Por padrão, se o logon do RDP falhar, as credenciais serão armazenadas apenas rapidamente. Esse pode não ser o caso se o computador estiver comprometido.|
|Uso da rede * \\\SERVER|Rede|-||
|Uso da rede * \\\SERVER /u:user|Rede|-||
|Snap-ins do MMC para um computador remoto|Rede|-|Exemplo: Gerenciamento de Computador, Visualizador de Eventos, Gerenciador de Dispositivos, Serviços|
|PowerShell WinRM|Rede|-|Exemplo: Enter-PSSession server|
|PowerShell WinRM com CredSSP|NetworkClearText|v|New-PSSession server<br />-Credssp de autenticação<br />-Credencial cred|
|PsExec sem credenciais explícitas|Rede|-|Exemplo: PsExec \\\server cmd|
|PsExec com credenciais explícitas|Rede + interativo|v|PsExec \\\server -u user -p pwd cmd<br />Cria várias sessões de logon.|
|Registro Remoto|Rede|-||
|Gateway de Área de Trabalho Remota|Rede|-|Autenticação em Gateway de Área de Trabalho Remota.|
|Tarefa agendada|Batch|v|A senha também será salva como um segredo LSA em disco.|
|Executar ferramentas como um serviço|Serviço|v|A senha também será salva como um segredo LSA em disco.|
|Scanners de vulnerabilidade|Rede|-|A maioria dos scanners assumem o uso padrão de logons de rede, embora alguns fornecedores possam implementar logons de fora da rede e apresentar mais risco de roubo de credenciais.|

Para autenticação na Web, use a referência da tabela abaixo:

|Método de conexão|Tipo de logon|Credenciais reutilizáveis no destino|Comentários|
|-----------|-------|--------------------|------|
|"Autenticação básica" de IIS|NetworkCleartext<br />(IIS 6.0+)<p>Interactive (Interativo)<br />(antes do IIS 6.0)|v||
|“Autenticação Integrada do Windows” do IIS|Rede|-|Provedores de Kerberos e NTLM.|

Definições de coluna:

- **Tipo de logon** identifica o tipo de logon iniciado pela conexão.
- **Credenciais reutilizáveis no destino** indica que os seguintes tipos de credencial serão armazenados na memória do processo LSASS no computador de destino onde a conta especificada está conectada localmente:
   - Hashes LM e NT
   - TGTs de Kerberos
   - Senha de texto sem formatação (se for aplicável).

Os símbolos nesta tabela são definidos da seguinte forma:

- (-) indica quando as credenciais não são expostas.
- (v) indica quando as credenciais são expostas.

Para os aplicativos de gerenciamento que não estão nessa tabela, você pode determinar o tipo de logon no campo tipo de logon nos eventos de logon de auditoria. Para saber mais, consulte [Eventos de logon de auditoria](/previous-versions/windows/it-pro/windows-server-2003/cc787567(v=ws.10)).

Em computadores baseados em Windows, todas as autenticações são processadas como um dos vários tipos de logon, independentemente de qual protocolo de autenticação ou autenticador é usado. Essa tabela inclui os tipos mais comuns de logon e seus atributos em relação ao roubo de credenciais:

|Tipo de logon|#|Autenticadores aceitos|Credenciais reutilizáveis na sessão de LSA|Exemplos|
|-------|---|--------------|--------------------|------|
|Interativo (também conhecida como Logon local)|2|Senha, Cartão inteligente,<br />outros|Sim|Logon no console;<br />RUNAS;<br />Soluções de controle remoto de hardware (como KVM de rede ou Acesso remoto/Cartão lights-out no servidor)<br />Autenticação básica do IIS (antes do IIS 6.0)|
|Rede|3|Senha,<br />Hash de NT,<br />Tíquete Kerberos|Não (exceto se a delegação estiver habilitada, nesse caso, os tíquetes do Kerberos estarão presentes)|NET USE;<br />Chamadas RPC;<br />Registro Remoto;<br />Autenticação Windows integrada ao IIS;<br />Autenticação Windows do SQL;|
|Batch|4|Senha (normalmente armazenada como segredo de LSA)|Sim|Tarefas Agendadas|
|Serviço|5|Senha (normalmente armazenada como segredo de LSA)|Sim|Serviços Windows|
|NetworkCleartext|8|Senha|Sim|Autenticação básica do IIS (IIS 6.0 e mais recente);<br />Windows PowerShell com CredSSP|
|NewCredentials|9|Senha|Sim|RUNAS /NETWORK|
|RemoteInteractive|10|Senha, Cartão inteligente,<br />outros|Sim|Área de Trabalho Remota (anteriormente conhecida como "Serviços de Terminal")|

Definições de coluna:

- **Tipo de logon** é o tipo de logon solicitado.
- **#** é o identificador numérico para o tipo de logon informado em eventos de auditoria no log de eventos de Segurança.
- **Autenticadores aceitos** indica quais tipos de autenticadores são capazes de iniciar um logon desse tipo.
- **Credenciais reutilizáveis** na sessão LSA indica se o tipo de logon resulta no armazenamento de credenciais pela sessão LSA, como senhas de texto sem formatação, hashes de NT ou tíquetes Kerberos que podem ser usados para autenticar em outros recursos de rede.
- **Exemplos** lista cenários comuns nos quais o tipo de logon é usado.

> [!NOTE]
> Para saber mais sobre tipos de logon, consulte [Enumeração SECURITY_LOGON_TYPE](/windows/win32/api/ntsecapi/ne-ntsecapi-security_logon_type).