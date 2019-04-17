---
title: "Estações de trabalho com privilégios de acesso"
description: "Segurança do Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/09/2016
ms.openlocfilehash: ce64974d771a11ef11257bceef1c3fde1797a7da
ms.sourcegitcommit: 3bf47cf4e25896725137d983d63b8041a53cb9a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/25/2018
---
# <a name="privileged-access-workstations"></a>Estações de trabalho com privilégios de acesso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Privilegiado estações de trabalho de acesso (patas) fornecem um sistema operacional dedicado para tarefas confidenciais protegido contra ataques de Internet e os vetores de ameaça. Separando essas tarefas confidenciais e contas de diária usar estações de trabalho e dispositivos oferece muito forte proteção contra ataques de phishing, aplicativo e sistema operacional vulnerabilidades, diversos ataques de representação e ataques de roubo de credenciais como registrar em log de pressionamento de tecla, [Pass-the-Hash](https://www.microsoft.com/en-us/download/details.aspx?id=36036), e [Pass-The-Ticket](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf).

## <a name="architecture-overview"></a>Visão geral da arquitetura
O diagrama abaixo mostra um "canal" separado para administração (uma tarefa altamente confidencial) que é criado por manter contas de administrador dedicadas separadas e estações de trabalho.

![Diagrama mostrando um "canal" separado para administração (uma tarefa altamente confidencial) que é criada por manter contas de administrador dedicadas separadas e estações de trabalho](../media/privileged-access-workstations/PAWFig1.JPG)

Essa abordagem arquitetônica complementa as proteções encontradas no Windows 10 [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx) recursos e vai além essas proteções para contas confidenciais e tarefas.

Essa metodologia é apropriada para contas com acesso aos ativos de alto valor:

-   **Privilégios administrativos** as patas fornecem maior segurança para alto impacto funções administrativas de TI e tarefas. Essa arquitetura pode ser aplicada a administração de vários tipos de sistemas entre domínios do Active Directory e florestas, locatários do Active Directory do Microsoft Azure, Office 365 locatários, redes de controle de processo (PCN), sistemas de controle de supervisão e dados de aquisição (SCADA), caixas eletrônicos (caixas eletrônicos) e dispositivos de ponto de venda (PoS).

-   **Operadores de informações de sensibilidade alto** a abordagem usada uma PATA também pode fornecer proteção para tarefas de trabalho de informações altamente confidenciais e pessoal, como aquelas que envolvem a atividade de fusão e aquisição de pré-lançamento, relatórios financeiros de pré-lançamento, presença de mídias sociais organizacional, comunicações executivas, não patenteados segredos comerciais, pesquisa confidencial ou outros dados confidenciais ou proprietários. Essa orientação não discorre sobre a configuração desses cenários de worker informações em detalhes ou incluir esse cenário nas instruções técnicas.

    > [!NOTE]
    > Microsoft IT usa patas (internamente chamadas de "estações de trabalho Administração segura", ou SAWs) para gerenciar o acesso seguro a sistemas internos de alto valor dentro da Microsoft. Essa diretriz tem detalhes adicionais abaixo sobre o uso de PATA na Microsoft na seção "Como a Microsoft usa estações de trabalho do administrador". Para mais informações detalhadas sobre essa abordagem de ambiente de ativos de alto valor, consulte o artigo [proteger ativos de alto valor com estações de trabalho Administração segura](https://msdn.microsoft.com/en-us/library/mt186538.aspx).

Este documento descreve por que essa prática é recomendada para proteger as contas de alto impacto privilegiado, aparência essas soluções PATA para proteger os privilégios administrativos e como implantar rapidamente uma solução PATA para administração de serviços de nuvem e domínio.

Este documento fornece diretrizes detalhadas para a implementação de várias configurações de PATA e inclui instruções detalhadas de implementação para começar a proteção de contas de alto impacto comuns:

-   **Fase 1 - implantação imediata para administradores do Active Directory** isso fornece uma PATA rapidamente que pode proteger no local funções de administração de domínio e da floresta

-   **Fase 2 - estender PATA para todos os administradores** Isso permite que a proteção para administradores de serviços de nuvem como o Office 365 e Azure, servidores corporativos, aplicativos corporativos e estações de trabalho

-   **Fase 3 - segurança avançada PATA** este tópico aborda as proteções adicionais e as considerações de segurança PATA

### <a name="why-a-dedicated-workstation"></a>Por que uma estação de trabalho dedicada?
O ambiente de ameaça atual para empresas é apresenta com phishing sofisticado e outros ataques de internet que criam contínuo risco de comprometimento de segurança para contas da internet exposto e estações de trabalho.

Esse ambiente ameaça requer uma organização adotar uma postura de segurança "pressupõem violação" ao projetar proteções para ativos de alto valor, como contas de administrador e os ativos corporativos confidenciais. Esses ativos de alto valor precisam ser protegidos contra tanto direta ameaças da internet, bem como ataques montada a partir de outros estações de trabalho, servidores e dispositivos no ambiente.

![Figura mostrando o risco aos ativos gerenciados, se um invasor assumir o controle de uma estação de trabalho do usuário onde as credenciais confidenciais são usadas](../media/privileged-access-workstations/PAWFig2.JPG)

Esta figura mostra risco aos ativos gerenciados se um invasor assumir o controle de uma estação de trabalho do usuário onde as credenciais confidenciais são usadas.

Um invasor no controle de um sistema operacional tem várias maneiras de obter acesso a todas as atividades na estação de trabalho e representar a conta legítima ilicitamente. Uma variedade de técnicas de ataque conhecidos e desconhecidos pode ser usada para obter esse nível de acesso. O volume crescente e a sofisticação dos cyberattacks tornaram necessários para estender esse conceito de separação para separar completamente os sistemas operacionais de cliente para contas confidenciais. Para obter mais informações sobre esses tipos de ataques, visite o [site Pass The Hash](https://www.microsoft.com/pth) informativas white papers, vídeos e muito mais.

A abordagem PATA é uma extensão da prática recomendada bem estabelecida para usar contas de usuário e da administração separada para pessoal administrativo. Esta prática usa uma conta administrativa individualmente atribuída completamente separada da conta de usuário padrão do usuário. PATA complementa essa prática de separação de conta, fornecendo uma estação de trabalho confiável para essas contas confidenciais.

> [!NOTE]
> Microsoft IT usa patas (internamente chamadas de "estações de trabalho Administração segura", ou SAWs) para gerenciar o acesso seguro a sistemas internos de alto valor dentro da Microsoft.  Essa diretriz tem detalhes adicionais sobre o uso de PATA na Microsoft na seção "Como a Microsoft usa estações de trabalho do administrador"
>
> Para mais informações detalhadas sobre essa abordagem de ambiente de ativos de alto valor, consulte o artigo [proteger ativos de alto valor com estações de trabalho Administração segura](https://msdn.microsoft.com/en-us/library/mt186538.aspx).

Essa diretriz PATA destina-se para ajudá-lo a implementar essa funcionalidade para proteger contas de alto valor, como os administradores de TI altamente privilegiado e alta sensibilidade comercial. A diretriz ajuda você a:

-   Restringir a exposição de credenciais para somente hosts confiáveis

-   Fornece uma estação de trabalho de alta segurança para os administradores para que eles podem realizar facilmente tarefas administrativas.

Restringir as contas confidenciais usando apenas patas protegidas é uma proteção simples para essas contas que seja altamente útil para administradores e muito difícil para um adversário vencer.

#### <a name="alternate-approaches---limitations-considerations-and-integration"></a>Abordagens alternativas - integração, considerações e limitações
Esta seção contém informações sobre como a segurança de abordagens alternativas se compara ao PATA e como integrar corretamente essas abordagens dentro de uma arquitetura de PATA. Todas essas abordagens executar riscos significativos quando implementada em isolamento, mas podem agregar valor para uma implementação de PATA em alguns cenários.

**O Credential Guard e o Microsoft Passport**

Introduzido no Windows 10, [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) usa segurança baseada em virtualização e hardware para atenuar ataques de roubo de credenciais comuns, como Pass-the-Hash, protegendo as credenciais derivadas. A chave privada para as credenciais usadas pelas [Microsoft Passport](http://aka.ms/passport) pode ser também seja protegida pelo hardware Trusted Platform Module (TPM).

Esses são mitigações poderosas, mas estações de trabalho ainda podem estar vulneráveis a determinados ataques mesmo se as credenciais são protegidas pelo Credential Guard ou o Passport. Ataques podem incluir o abuso de privilégios e o uso de credenciais diretamente de um dispositivo comprometido, reutilizando credenciais roubadas anteriormente, antes da habilitação do Credential Guard e o abuso de configurações de aplicativo fraca e ferramentas de gerenciamento na estação de trabalho.

A diretriz PATA nesta seção inclui o uso de várias dessas tecnologias para contas de alta sensibilidade e tarefas.

**VM administrativo**

Uma máquina virtual administrativa (VM) é um sistema operacional dedicado para tarefas administrativas hospedado em uma área de trabalho do usuário padrão. Embora essa abordagem é semelhante ao PATA em fornecer um sistema operacional dedicado para tarefas administrativas, ele tem uma falha fatal a VM administrativa é dependente na área de trabalho de usuário padrão para sua segurança.

O diagrama abaixo mostra a capacidade de invasores seguem a cadeia de controle para o objeto de destino de interesse com um administrador VM em uma estação de trabalho do usuário e que ele é difícil criar um caminho na configuração reversa.

A arquitetura PATA não permitir para hospedar um administrador VM em uma estação de trabalho do usuário, mas um usuário VM com uma imagem corporativa padrão pode ser hospedado em um host PATA para fornecer a equipe com um único computador para todas as responsabilidades.

![Diagrama da architecture PATA](../media/privileged-access-workstations/PAWFig9.JPG)

**Servidor de atalhos**

Administrativas arquiteturas de "Saltar Server" Configure um pequeno número servidores de console administrativo e restringem a equipe a usá-los para tarefas administrativas. Isso normalmente é baseado no serviços de área de trabalho remota, uma solução de virtualização de apresentação de 3ª terceiros ou uma tecnologia de Virtual Desktop Infrastructure (VDI).

Essa abordagem é frequentemente proposta para reduzir os riscos de administração e fornecem algumas garantias de segurança, mas a abordagem de servidor de atalhos por si só é vulnerável a determinados ataques, porque isso viola o ["fonte limpa" princípio](http://aka.ms/cleansource). O princípio de origem limpa exige que todas as dependências de segurança seja tão confiável quanto o objeto sendo protegido.

![Figura mostrando uma relação de controle simples](../media/privileged-access-workstations/PAWFig3.JPG)

Esta figura mostra uma relação de controle simples. Qualquer assunto no controle de um objeto é uma dependência de segurança desse objeto. Se um adversário pode controlar uma dependência de segurança de um objeto de destino (sujeito), eles podem controlar esse objeto.

A sessão administrativa no servidor atalhos depende da integridade do computador local acessá-lo. Se o computador for uma estação de trabalho do usuário sujeitos a ataques de phishing e outros vetores de ataque baseados na internet, em seguida, a sessão administrativa também está sujeita a esses riscos.

![Figura mostrando como os invasores podem seguir uma cadeia de controle estabelecidas para o objeto de destino de interesse](../media/privileged-access-workstations/PAWFig4.JPG)

A figura abaixo descreve como os invasores podem seguir uma cadeia de controle estabelecidas para o objeto de destino de interesse.

Embora alguns controles de segurança avançada, como a autenticação multifator pode aumentar a dificuldade de um invasor assumam nesta sessão administrativa da estação de trabalho do usuário, nenhum recurso de segurança totalmente podem proteger contra ataques technical quando um invasor tem acesso administrativo do computador de origem (por exemplo, injete ilícitas comandos em uma sessão legítima, processos legítimos sequestro e assim por diante.)

A configuração padrão neste guia PATA instala ferramentas administrativas na PATA, mas uma arquitetura de servidor de atalhos também pode ser adicionada, se necessário.

![Figura mostrando como inverter a relação de controle e acessar aplicativos de usuário em uma estação de trabalho do administrador não oferece o invasor nenhum caminho para o objeto de destino](../media/privileged-access-workstations/PAWFig5.JPG)

Esta figura mostra como inverter a relação de controle e acessar aplicativos de usuário em uma estação de trabalho do administrador não oferece o invasor nenhum caminho para o objeto de destino. O servidor de atalhos do usuário ainda é exposto a riscos para que controles de proteção apropriados, controles de investigação e processos de resposta ainda devem ser aplicados para aquele computador conectado à internet.

Essa configuração requer que os administradores a seguir práticas operacionais estreitamente para garantir que eles não acidentalmente firmar credenciais de administrador a sessão do usuário em sua área de trabalho.

![Figura mostrando como acessar um servidor de salto administrativo de uma PATA não adiciona nenhum caminho para o invasor nos ativos administrativos](../media/privileged-access-workstations/PAWFig6.JPG)

Esta figura mostra como acessar um servidor de salto administrativo de uma PATA não adiciona nenhum caminho para o invasor nos ativos administrativos. Um servidor de atalhos com um PATA permite neste caso consolidar o número de locais para monitorar a atividade administrativa e distribuir aplicativos administrativos e ferramentas. Isso adiciona certa complexidade do design, mas pode simplificar as atualizações de software e monitoramento de segurança se um grande número de contas e estações de trabalho é usado na implementação PATA. O servidor de salto precisaria ser criados e configurados para os padrões de segurança semelhantes como a PATA.

**Soluções de gerenciamento de privilégio**

Soluções de gerenciamento de privilegiados são aplicativos que fornecem acesso temporário privilégios discretos ou contas privilegiadas sob demanda. Soluções de gerenciamento de privilégio são um componente de uma estratégia completa para proteger o acesso privilegiado e fornecer visibilidade extremamente importante e responsabilidade de atividade administrativa extremamente valioso.

Essas soluções geralmente usam um fluxo de trabalho flexível para conceder acesso e muitos têm recursos de segurança adicionais e recursos como gerenciamento de senhas de conta de serviço e integração com servidores de salto administrativos. Existem muitas soluções no mercado que oferecem recursos de gerenciamento, uma delas é o gerenciamento de acesso do Microsoft Identity Manager (MIM) privilegiados (PAM) de privilégio.

A Microsoft recomenda usando uma PATA para soluções de gerenciamento de privilégios de acesso. Acesso a essas soluções deve ser concedido apenas para patas. A Microsoft não recomenda usando essas soluções como um substituto para uma PATA porque acessar privilégios usando essas soluções de uma área de trabalho do usuário potencialmente comprometida viola a [origem limpa](http://aka.ms/cleansource) princípio, conforme mostrado no diagrama a seguir:

![Diagrama mostrando como a Microsoft não recomenda usando essas soluções como um substituto para uma PATA porque acessando privilégios usando essas soluções de uma área de trabalho do usuário potencialmente comprometida viola o princípio de origem limpa](../media/privileged-access-workstations/PAWFig7.JPG)

Fornecer uma PATA para acessar essas soluções permite que você obtenha os benefícios de segurança de PATA e a solução de gerenciamento de privilégio, conforme mostrado neste diagrama:

![Diagrama mostrando como fornecer uma PATA para acessar essas soluções permite que você obtenha os benefícios de segurança de PATA e a solução de gerenciamento de privilégio](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> Esses sistemas devem ser classificados no nível mais alto de privilégio gerenciam e ser protegidos em ou acima esse nível de segurança. Isso normalmente são configurados para gerenciar ativos de nível 0 e soluções de nível 0 e devem ser classificados na faixa de 0.
> Para obter mais informações sobre o modelo de camada, consulte [http://aka.ms/tiermodel](http://aka.ms/tiermodel) para obter mais informações sobre os grupos de nível 0, consulte equivalência de nível 0 no [protegendo Material de referência de acesso privilegiado](../securing-privileged-access/securing-privileged-access-reference-material.md).

Para obter mais informações sobre a implantação do gerenciamento de acesso do Microsoft Identity Manager (MIM) privilegiados (PAM), consulte [http://aka.ms/mimpamdeploy](http://aka.ms/mimpamdeploy)

#### <a name="how-microsoft-is-using-admin-workstations"></a>Como a Microsoft está usando estações de trabalho do administrador
A Microsoft usa a abordagem de arquitetura PATA tanto internamente em nossos sistemas, bem como com nossos clientes. A Microsoft usa estações de trabalho administrativas internamente em um número de capacidades, incluindo administração de infraestrutura do Microsoft IT, desenvolvimento de infraestrutura de malha do Microsoft na nuvem e operações e outros ativos de alto valor.

Essa diretriz baseia-se diretamente na arquitetura de referência de estação de trabalho de acesso privilegiado (PATA) implantada por nossas equipes de serviços profissionais cybersecurity para proteger os clientes contra ataques de cybersecurity. As estações de trabalho administrativas também são um elemento fundamental da proteção mais forte para tarefas de administração de domínio, a arquitetura de referência de floresta administrativas avançado segurança administrativas ambiente (ESAE).

Para obter mais detalhes na floresta administrativa do ESAE, consulte [abordagem de Design de floresta administrativas ESAE](http://aka.ms/ESAE) seção [protegendo Material de referência de acesso privilegiado](../securing-privileged-access/securing-privileged-access-reference-material.md).

Para obter mais informações sobre envolvente serviços da Microsoft para implantar um PATA ou ESAE para seu ambiente, entre em contato com seu representante da Microsoft ou visite [nesta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="what-is-a-privileged-access-workstation-paw"></a>O que é uma estação de trabalho com privilégios de acesso (PATA)?
Em termos mais simples, uma PATA é uma estação de trabalho protegida e bloqueada projetada para fornecer garantias de alta segurança para contas confidenciais e tarefas.  Patas são recomendadas para administração de sistemas de identidade, serviços de nuvem e estrutura de nuvem privada, bem como funções comerciais confidenciais.

> [!NOTE]
> A arquitetura PATA não requer um mapeamento de 1:1 de contas para estações de trabalho, mas essa é uma configuração comum. PATA cria um ambiente de estação de trabalho confiável que pode ser usado por uma ou mais contas.

Para proporcionar a melhor segurança, patas devem sempre executar mais atualizado e seguro sistema operacional disponível: a Microsoft recomenda veementemente Windows 10 Enterprise, que inclui vários recursos de segurança adicional não está disponíveis em outras edições (em particular, [Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)).

> [!NOTE]
> As organizações sem acesso ao Windows 10 Enterprise podem usar Windows 10 Pro, que inclui muitas das tecnologias básicos críticas para patas, incluindo a inicialização confiável, o BitLocker e área de trabalho remota.  Os clientes Education podem usar o Windows 10 Education.  Windows 10 Home não deve ser usado para uma PATA.
>
> Para uma matriz de comparação das diferentes edições do Windows 10, leia [neste artigo](https://www.microsoft.com/en-us/WindowsForBusiness/Compare).

Os controles de segurança no PATA enfocam minimizando o maior impacto e provavelmente riscos de comprometimento. Isso inclui a atenuar ataques no ambiente e reduzir os riscos que podem prejudicar os controles PATA ao longo do tempo:

-   **Ataques de Internet** -a maioria dos ataques originados direta ou indiretamente de fontes de internet e usar a internet para roubar os dados e comando e controle (C2). Isolar PATA da internet aberto é um elemento fundamental para garantir a PATA não seja comprometido.

-   **O risco de usabilidade** -se um PATA for muito difícil de usar para executar suas tarefas diárias, os administradores sejam motivou para criar soluções alternativas para facilitar a seus trabalhos. Com frequência, estas soluções alternativas abra a estação de trabalho administrativa e contas a riscos de segurança significativas, portanto, é fundamental para envolver e capacitar os usuários PATA para atenuar esses problemas de usabilidade com segurança. Isso é feito com frequência por seus comentários, instalar ferramentas e scripts necessários para executar seus trabalhos, além de garantir pessoal administrativo todos estão cientes por que ele precisa usar uma PATA, quais uma PATA é e como usá-lo corretamente e com êxito.

-   **Riscos do ambiente** -porque muitos outros computadores e contas no ambiente são expostas para diretório de risco de internet ou indiretamente, uma PATA deve ser protegida contra ataques de comprometido ativos no ambiente de produção. Isso requer a limitar as ferramentas de gerenciamento e as contas que têm acesso a patas no mínimo necessário para proteger e monitorar esses especializado estações de trabalho.

-   **Forneça a adulteração de cadeia** - enquanto é impossível remover todos os possíveis riscos de adulteração na cadeia de suprimento para hardware e software, executar algumas ações principais podem atenuar os vetores de ataque críticos que estão prontamente disponíveis para os invasores. Isso inclui a validar a integridade de todas as mídias de instalação ([limpa princípio de origem](http://aka.ms/cleansource)) e o uso de um fornecedor confiável e confiável de hardware e software.

-   **Ataques físicos** -patas porque pode ser fisicamente móvel e ser usados fora instalações seguras fisicamente, eles devem ser protegidos contra ataques que aproveitam não autorizado acesso físico ao computador.

> [!NOTE]
> Uma PATA não protegerá um ambiente de um adversário que já tenha obtido acesso administrativo ao longo de uma floresta do Active Directory.
> Como muitos implementações existentes dos serviços de domínio do Active Directory tem sido operando por anos em risco de roubo de credenciais, as organizações devem assumir violação e considere a possibilidade de que eles podem ter um compromisso não detectado de credenciais de administrador do domínio ou da empresa. Uma organização que suspeitar de comprometimento de domínio deve considerar o uso dos serviços de resposta a incidentes profissional.
>
> Para obter mais informações sobre as diretrizes de resposta e recuperação, consulte o "responder às atividades suspeitas" e "Recuperar de uma violação" seções de [Mitigating Pass-the-Hash e outros roubo de credenciais](https://www.microsoft.com/pth), versão 2.
>
> Visite [resposta a incidentes e serviços de recuperação da Microsoft](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx) página para obter mais informações.

### <a name="paw-hardware-profiles"></a>Perfis de Hardware PATA
Pessoal administrativo também é usuários padrão também - precisam não apenas uma PATA, mas também uma estação de trabalho do usuário padrão para verificar emails, navegar na web e acessar corporativo aplicativos de linha de negócios.  É essencial para o sucesso de qualquer implantação PATA garantir que os administradores podem permanecer produtivo e seguro.  Uma solução segura que limita drasticamente a produtividade será abandonada pelos usuários em favor que aumenta a produtividade (mesmo que isso é feito de uma maneira segura).

Para equilibrar a necessidade de segurança com a necessidade de produtividade, a Microsoft recomenda usando um desses perfis de hardware PATA:

-   **Hardware dedicado** -separar dedicados para tarefas de usuário versus tarefas administrativas

-   **Uso simultâneo** -dispositivo única que pode executar tarefas de usuário e tarefas administrativas simultaneamente, tirando proveito da virtualização de sistema operacional ou a apresentação.

As organizações podem usar somente um perfil ou ambos. Não há nenhum preocupações de interoperabilidade entre os perfis de hardware e as organizações têm a flexibilidade de acordo com o perfil de hardware para a necessidade específica e a situação de um determinado administrador.

> [!NOTE]
> É fundamental que, em todos esses cenários, pessoal administrativo é emitido uma conta de usuário padrão que é separada do designado contas de administrador. As contas de administrador devem ser usadas somente no sistema operacional PATA administrativo.

Esta tabela resume as relativas vantagens e desvantagens de cada perfil de hardware da perspectiva do operacional facilidade de uso e a produtividade e a segurança.  As duas abordagens de hardware fornecem forte segurança para contas de administrador contra roubo de credenciais e reutilização.

|**Cenário**|**Vantagens**|**Desvantagens**|
|--------|---------|-----------|
|Hardware dedicado|-Sinal forte para sensibilidade de tarefas<br />-Mais forte separação de segurança|-Espaço em disco adicionais<br />-Peso adicional (de trabalho remoto)<br />– Custo hardware|
|Uso simultâneo|-Menor custo de hardware<br />-Experiência de dispositivo único|-Compartilhamento único via teclado/mouse cria o risco de erros inadvertidos/riscos|

Este guia contém instruções detalhadas para a configuração PATA para a abordagem de hardware dedicado. Se você tiver requisitos para os perfis de uso simultâneo de hardware, você pode adaptar as instruções com base nessa diretriz por conta própria ou contratar uma organização de serviços profissionais que a Microsoft para ajudar com ele.

**Hardware dedicado**

Nesse cenário, uma PATA é usada para administração completamente separado do computador que é usado para atividades diárias, como email, edição de documentos e trabalho de desenvolvimento. Todas as ferramentas administrativas e aplicativos instalados na PATA e todos os aplicativos de produtividade são instalados na estação de trabalho do usuário padrão. As instruções passo a passo neste guia são baseadas nesse perfil de hardware.

**Uso simultâneo - adicionando um usuário local VM**

Neste cenário de uso simultâneo, um único computador é usado para tarefas de administração e atividades diárias, como email, edição de documentos e trabalho de desenvolvimento. Nesta configuração, o sistema operacional do usuário está disponível enquanto estiver desconectado (para editar documentos e trabalhar em email armazenados em cache localmente), mas requer processos de suporte e hardware que podem acomodar esse estado desconectado.

![Diagrama mostrando o único computador em um cenário de uso simultâneo usado para tarefas de administração e atividades diárias, como email, edição de documentos e trabalho de desenvolvimento](../media/privileged-access-workstations/PAWFig10.JPG)

O hardware físico executa localmente dois sistemas operacionais:

-   **Sistema operacional Admin** -o host físico executa o Windows 10 no host PATA para tarefas administrativas

-   **Nome do SO** -convidado de máquina virtual do Windows 10 de um cliente Hyper-V é executado em uma imagem corporativa

Com o Windows 10Hyper-V, uma máquina virtual de convidado (também executa o Windows 10) pode ter uma experiência de usuário, incluindo o som, vídeo e aplicativos de comunicação de Internet, como o Skype para empresas.

Nesta configuração, o trabalho diário que não exigem privilégios administrativos é feito na máquina virtual SO de usuário que tem uma imagem corporativa do Windows 10 normal e não está sujeito às restrições aplicadas ao host PATA. Todo o trabalho administrativo é feito no administrador de sistema operacional.

Para configurar isso, siga as instruções neste guia para o host PATA, adicionar recursos do Hyper-V cliente, crie uma VM do usuário e, em seguida, instalar uma imagem corporativa do Windows 10 na VM do usuário.

Leitura [Hyper-V cliente](https://technet.microsoft.com/en-us/library/hh857623.aspx) artigo para saber mais sobre essa funcionalidade. Observe que o sistema operacional em máquinas virtuais convidadas precisa ser licenciado por [licenciamento de produtos Microsoft](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx), também descrita [aqui](https://www.microsoft.com/en-us/Licensing/learn-more/brief-windows-virtual-machine.aspx).

**Uso simultâneo - adicionando RemoteApp, RDP ou uma VDI**

Neste cenário de uso simultâneo, um único computador é usado para ambas as tarefas de administração e atividades diárias, como email, edição de documentos e desenvolvimento funcionam. Nesta configuração, os sistemas operacionais de usuário são implantados e gerenciados centralmente (na nuvem ou em seu datacenter), mas não estão disponíveis enquanto estiver desconectado.

![Figura mostrando um único computador em um cenário de uso simultâneo usado para ambas as tarefas de administração e atividades diárias, como email, edição de documentos e desenvolvimento de funcionar](../media/privileged-access-workstations/PAWFig11.JPG)

O hardware físico é executado um único sistema operacional PATA localmente para tarefas administrativas e entra em contato com um Microsoft ou 3ª parte da área de trabalho serviço remoto para aplicativos de usuário, como email, edição de documentos e aplicativos linha de negócios.

Nesta configuração, trabalho diário que não exigem privilégios administrativos é feito no OS(es) remoto e aplicativos que não estão sujeitas a restrições aplicadas ao host PATA. Todo o trabalho administrativo é feito no administrador de sistema operacional.

Para configurar isso, siga as instruções neste guia para o host PATA, permite a conectividade de rede para os serviços de área de trabalho remota e, em seguida, adicionar atalhos à área de trabalho do usuário PATA para acessar os aplicativos. Os serviços de área de trabalho remotos podem ser hospedados em muitas maneiras incluindo:

-   Um serviço de área de trabalho remota ou VDI existente (local ou na nuvem)

-   Um novo serviço que você instala no local ou na nuvem

-   O Azure RemoteApp usando modelos pré-configurados do Office 365 ou suas próprias imagens de instalação

Para obter mais informações sobre o Azure RemoteApp, visite [nesta página](https://www.remoteapp.windowsazure.com).

### <a name="paw-scenarios"></a>Cenários de PATA
Esta seção contém diretrizes sobre quais cenários essa diretriz PATA deve ser aplicado ao. Em todos os cenários, os administradores devem ser treinados usar somente patas para executar o suporte de sistemas remotos. Para incentivar o uso seguro e bem-sucedida, todos os usuários PATA deve ser também ser incentivados a fornecer comentários para melhorar a experiência de PATA e esses comentários devem ser revisados cuidadosamente para integração com o programa PATA.

Em todos os cenários, perfis de hardware diferente neste guia e proteção adicional em fases posteriores podem ser usados para atender aos requisitos de usabilidade ou a segurança das funções.

> [!NOTE]
> Observe que essa diretriz explicitamente diferencia entre exigir acesso a serviços específicos na internet (por exemplo, o Azure e Office 365 portais administrativos) e a "Internet aberta" de todos os hosts e serviços.

Consulte o [página de modelo de camada](http://aka.ms/tiermodel) para obter mais informações sobre as designações de nível.

|**Cenários**|**Usar PATA?**|**Escopo e as considerações de segurança**|
|---------|--------|---------------------|
|Administradores do Active Directory - nível 0|Sim|Uma PATA criada com as diretrizes de fase 1 é suficiente para essa função.<br /><br />-Uma floresta administrativa pode ser adicionada para fornecer a proteção mais forte para esse cenário. Para obter mais informações sobre a floresta administrativa ESAE, consulte [ESAE administrativas abordagem de Design de floresta](http://aka.ms/esae)<br />-Uma PATA pode ser usada para gerenciados várias florestas ou vários domínios.<br />-Se os controladores de domínio são hospedados em uma infraestrutura como serviço (IaaS) ou solução de virtualização de local, você deve priorizar implementando patas para os administradores dessas soluções|
|Serviços de administrador do Azure IaaS e PaaS - nível 0 ou nível 1 (veja escopo e as considerações de Design)|Sim|Uma PATA criada usando as orientações fornecidas na fase 2 é suficiente para essa função.<br /><br />-Patas devem ser usadas para pelo menos o Administrador Global e o administrador de cobrança da assinatura. Você também deve usar patas de administradores delegados de servidores críticos ou confidenciais.<br />-Patas devem ser usadas para gerenciar o sistema operacional e aplicativos que fornecem a sincronização de diretório e federação de identidade para serviços de nuvem como [Azure AD Connect](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect/) e serviços de Federação do Active Directory (ADFS).<br />-As restrições de rede de saída devem permitir que a conectividade somente aos serviços de nuvem autorizados usando a diretriz na fase 2. Sem acesso à internet aberta deve ter permissão de patas.<br />-EMET deve ser configurado para todos os navegadores usados na estação de trabalho **Observação:** uma assinatura é considerada nível 0 para uma floresta se os controladores de domínio ou outros hosts de nível 0 forem na assinatura. Uma assinatura é o nível 1 se nenhuma servidores de nível 0 são hospedados no Azure|
|Administração Office 365 locatário <br />-Nível 1|Sim|Uma PATA criada usando as orientações fornecidas na fase 2 é suficiente para essa função.<br /><br />-Patas devem ser usadas para pelo menos o administrador de cobrança de assinatura, Administrador Global, administrador do Exchange, SharePoint administrador e funções de administrador de gerenciamento de usuário. Você também altamente deve considerar o uso de patas para os administradores delegados de dados altamente confidenciais ou críticos.<br />-EMET deve ser configurado para todos os navegadores usados na estação de trabalho<br />-As restrições de rede de saída devem permitir que a conectividade apenas aos serviços da Microsoft usando a diretriz na fase 2. Sem acesso à internet aberta deve ter permissão de patas.|
|Outros IaaS ou PaaS administrador de serviço de nuvem<br />-Nível 0 ou 1 nível (veja escopo e as considerações de Design)||Uma PATA criada usando as orientações fornecidas na fase 2 é suficiente para essa função.<br /><br />-Patas devem ser usadas para qualquer função que tenha direitos administrativos em nuvem hospedada VMs incluindo a capacidade de instalar agentes, exporte arquivos no disco rígido ou acessar o armazenamento onde os discos rígidos com dados críticos de negócios, dados confidenciais ou sistemas operacionais está armazenado.<br />-As restrições de rede de saída devem permitir que a conectividade apenas aos serviços da Microsoft usando a diretriz na fase 2. Sem acesso à internet aberta deve ter permissão de patas.<br />-EMET deve ser configurado para todos os navegadores usados na estação de trabalho. **Observação:** uma assinatura é o nível 0 para uma floresta se os controladores de domínio ou outros hosts de nível 0 forem na assinatura. Uma assinatura é o nível 1 se nenhuma servidores de nível 0 são hospedados no Azure.|
|Administradores de virtualização<br />-Nível 0 ou 1 nível (veja escopo e as considerações de Design)|Sim|Uma PATA criada usando as orientações fornecidas na fase 2 é suficiente para essa função.<br /><br />-Patas devem ser usadas para qualquer função que tenha direitos administrativos em VMs incluindo a capacidade de instalar agentes, exportar arquivos de disco rígido virtual ou acessar o armazenamento onde os discos rígidos com informações do sistema operacional convidado, dados confidenciais ou dados críticos de negócios está armazenado. **Observação:** um sistema de virtualização (e seus administradores) são considerados nível 0 para uma floresta se os controladores de domínio ou outros hosts de nível 0 forem na assinatura. Uma assinatura é o nível 1 se nenhuma servidores de nível 0 são hospedados no sistema de virtualização.|
|Administradores de manutenção do servidor<br />-Nível 1|Sim|Uma PATA criada usando as orientações fornecidas na fase 2 é suficiente para essa função.<br /><br />-Uma PATA deve ser usada para os administradores que atualizam, patch e solucionar problemas de servidores da empresa e aplicativos que estejam executando o Windows server, Linux e outros sistemas operacionais.<br />-Ferramentas de gerenciamento dedicado talvez precise ser adicionados para patas manipular a escala maior desses administradores.|
|Administradores de estação de trabalho do usuário <br />-Nível 2|Sim|Uma PATA criada com orientação fornecida na fase 2 é suficiente para funções que tenham direitos administrativos em dispositivos de usuário final (como assistência técnica e em mesa têm suporte para funções).<br /><br />-Aplicativos adicionais talvez precise ser instalado em patas para habilitar o gerenciamento de tíquete e outras funções de suporte.<br />-EMET deve ser configurado para todos os navegadores usados na estação de trabalho.<br />    Ferramentas de gerenciamento dedicado talvez precise ser adicionados para patas manipular a escala maior desses administradores.|
|SQL, SharePoint, ou linha de negócios (LOB) Admin<br />-Nível 1||Uma PATA criada com as diretrizes de fase 2 é suficiente para essa função.<br /><br />-Ferramentas de gerenciamento adicionais talvez precise ser instalado em patas para permitir que os administradores gerenciem aplicativos sem a necessidade de se conectar aos servidores usando a área de trabalho remota.|
|Usuários Gerenciando a presença de mídias sociais|Parcialmente|Uma PATA criada usando as orientações fornecidas na fase 2 pode ser usada como um ponto de partida para fornecer segurança para essas funções.<br /><br />-Proteger e gerenciar contas de mídias sociais usando o Azure Active Directory (AAD) para compartilhar, proteger e controle de acesso a contas de mídias sociais.<br />    Para obter mais informações sobre essa funcionalidade leia [esta postagem no blog](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx).<br />-As restrições de rede de saída devem permitir que a conectividade com esses serviços. Isso pode ser feito, permitindo que abrir conexões de internet (muito maior risco à segurança nega muitos garantias PATA) ou permitindo que somente endereços DNS necessários para o serviço (pode ser um desafio para obter).|
|Usuários padrão|Não|Embora muitas etapas para proteger podem ser usadas para usuários padrão, PATA foi projetada para isolar contas de acesso à internet aberta que a maioria dos usuários exigem para trabalhos.|
|Convidado VDI/quiosque|Não|Embora muitas etapas para proteger podem ser usadas para um sistema de quiosque para convidados, a arquitetura PATA foi projetada para fornecer maior segurança para contas de alta sensibilidade, não maior segurança para contas de sensibilidade inferiores.|
|Usuário VIP (executivo, pesquisador, etc.)|Parcialmente|Uma PATA criada com orientação fornecida na fase 2 pode ser usada como um ponto de partida para fornecer segurança para essas funções<br /><br />-Esse cenário é semelhante a uma área de trabalho do usuário padrão, mas geralmente tem um perfil de aplicativo menores, mais simples e bem conhecido. Esse cenário normalmente requer a descoberta e proteger dados confidenciais, serviços e aplicativos (que podem ou não podem ser instalados nos desktops).<br />-Essas funções geralmente exigem um alto grau de segurança e muito alto grau de usabilidade, que exigem alterações de design para atender às preferências do usuário.|
|Sistemas de controle industriais (por exemplo, SCADA, PCN e controladores de domínio)|Parcialmente|Uma PATA criada com orientação fornecida na fase 2 pode ser usada como ponto de partida para fornecer segurança para essas funções como a maioria dos ICS consoles (incluindo esses padrões comuns como SCADA e PCN) não exigem navegar na Internet aberta e verificar emails.<br /><br />-Aplicativos usados para controlar a físico maquinário precisaria ser integrado e testado para compatibilidade e devidamente protegido|
|Sistema operacional incorporado|Não|Embora muitos etapas para proteger de PATA podem ser usadas para sistemas operacionais incorporados, uma solução personalizada precisa ser desenvolvidos para proteção neste cenário.|

> [!NOTE]
> **Cenários de combinação** alguns equipe pode ter responsabilidades administrativas que se estendem por vários cenários.
> Nesses casos, as regras importantes a serem lembrados são que as regras de modelo de camada devem ser seguidas em todos os momentos. Consulte a página de modelo de nível para obter mais informações.

> [!NOTE]
> **Dimensionamento do programa PAW** como seu programa PATA dimensiona para abranger mais administradores e funções, você precisa continuar a garantir que você mantenha a conformidade com os padrões de segurança e a usabilidade. Isso pode exigir que você atualize-lo suporte a estruturas ou cria novos para resolver PATA desafios específicos, como PATA processo de integração, gerenciamento de incidentes, gerenciamento de configuração e desafios de comentários coleta a usabilidade de endereço.  Um exemplo pode ser que sua organização decide para habilitar cenários de trabalho de casa para administradores, que necessitariam de uma mudança da área de trabalho patas para notebook patas - uma mudança que pode exigir as considerações de segurança adicionais.  Outro exemplo comum é criar ou atualizar treinamento para novos administradores - treinamento que agora deve incluir o conteúdo sobre o uso apropriado de uma PATA (incluindo por que é importante e o que um PATA é e não está).  Para mais considerações que devem ser tratadas como dimensionar seu programa PATA, consulte a fase 2 das instruções.

Este guia contém instruções detalhadas para a configuração PATA para os cenários como observado acima. Se você tiver requisitos para os outros cenários, você pode adaptar as instruções com base nessa diretriz por conta própria ou contratar uma organização de serviços profissionais que a Microsoft para ajudar com ele.

Para obter mais informações sobre envolvente serviços da Microsoft para criar uma PATA personalizada para seu ambiente, entre em contato com seu representante da Microsoft ou visite [nesta página](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).

### <a name="paw-installation-instructions"></a>Instruções de instalação PATA
Como a PATA deve fornecer uma fonte confiável e segura para administração, é essencial que o processo de compilação é seguro e confiável.  Esta seção fornece instruções detalhadas que permitirá que você crie seu próprios PATA usando princípios gerais e conceitos muito semelhantes àquelas usadas por IT da Microsoft e a Microsoft de engenharia de nuvem e organizações de gerenciamento de serviço.

As instruções são divididas em três fases que se concentram em colocando rapidamente as mitigações mais críticas no lugar e depois progressivamente aumentando e expandindo o uso de PATA para a empresa.

-   Fase 1 - implantação imediata para administradores do Active Directory

-   Fase 2 - estender PATA para todos os administradores

-   Fase 3 - PATA avançadas de segurança

É importante observar que as fases sempre devem ser executadas na ordem mesmo se eles são planejados e implementados como parte do mesmo projeto geral.

#### <a name="phase-1---immediate-deployment-for-active-directory-administrators"></a>Fase 1 - implantação imediata para administradores do Active Directory
Finalidade: Fornece uma PATA rapidamente que pode proteger funções de administração de floresta e domínio local.

Escopo: Administradores de nível 0 incluindo administradores corporativos, administradores de domínio (para todos os domínios) e os administradores de outros sistemas de identidade autoritativo.

Fase 1 enfoca os administradores que gerenciam seu domínio do Active Directory no local, que são extremamente importantes funções com frequência atacadas por invasores. Esses sistemas de identidade funcionará efetivamente para proteger esses administradores se seu Active Directory controladores de domínio () são hospedados no data center local, no Azure infraestrutura como serviço (IaaS) ou outro provedor de IaaS.

Durante essa fase, você criará a estrutura de unidade organizacional (UO) de Active Directory administrativa segura para hospedar sua estação de trabalho do acesso privilegiado (PATA), bem como implantar as patas propriamente ditos.  Essa estrutura também inclui as políticas de grupo e os grupos necessários para dar suporte a PATA.  Você criará a maior parte da estrutura usando scripts do PowerShell que estão disponíveis no [Galeria do TechNet](http://aka.ms/pawmedia).

Os scripts criará a UOs e os grupos de segurança a seguir:

-   Unidades organizacionais (UO)

    -   Seis novas OUs de nível superior: Admin; Grupos; Servidores de nível 1; Estações de trabalho; Contas de usuário; e o computador quarentena.  Cada UO de nível superior conterá um número de unidades organizacionais filho.

-   Grupos

    -   Seis novos com segurança ativada globais grupos: manutenção da replicação nível 0; Manutenção de servidor de nível 1; Operadores de suporte técnico de serviço; Manutenção de estação de trabalho; Usuários PATA; Manutenção PATA.

Você também criará um número de objetos de política de grupo: configuração PAW - computador. PAW configuração - usuário; RestrictedAdmin obrigatório - computador. Restrições de saída PATA; Restringir o Logon de estação de trabalho; Restringir o Logon no servidor.

Fase 1 inclui as seguintes etapas:

1.  Preencha os pré-requisitos

    1.  **Certifique-se de que todos os administradores usam contas separadas, individuais para atividades de administração e o usuário final** (incluindo email, navegação na Internet, aplicativos de linha de negócios e outras atividades não administrativas).  Atribuir uma conta administrativa para cada separado pessoal autorizado de sua conta de usuário padrão é fundamental para o modelo de PATA, conforme apenas determinadas contas serão permitidas conectem a PATA em si.

        > [!NOTE]
        > Cada administrador deve usar a própria conta para administração.  Não compartilhe uma conta administrativa.

    2.  **Minimizar o número de administradores de nível 0 privilegiado**.  Porque cada administrador deve usar uma PATA, reduzir o número de administradores reduz o número de patas necessárias para dar suporte a eles e os custos associados. A contagem inferior de administradores também resulta em menor exposição desses privilégios e os riscos associados. Embora seja possível para os administradores em um local para compartilhar uma PATA, os administradores em locais físicos separados exigirá patas separadas.

    3.  **Adquirir hardware de um fornecedor confiável que atenda a todos os requisitos técnicos**. A Microsoft recomenda a aquisição de hardware que atenda aos requisitos técnicos no artigo [proteger credenciais de domínio com o Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx).

        > [!NOTE]
        > PATA instalada no hardware sem esses recursos pode fornecer proteções significativas, mas os recursos de segurança avançada, como Credential Guard e o Device Guard não estarão disponíveis.  O Credential Guard e o Device Guard não são necessárias para a implantação de fase 1, mas serão altamente recomendável como parte da fase 3 (proteção avançada).

        Certifique-se de que o hardware usado para a PATA é originado um fabricante e o fornecedor cujas práticas de segurança sejam confiáveis pela organização. Isso é um aplicativo do princípio origem limpo para fornecer segurança cadeia.

        > [!NOTE]
        > Para obter mais informações sobre a importância da segurança de cadeia de suprimento, visite [este site](https://www.microsoft.com/security/cybersecurity/).

    4.  Adquirir e validar a edição necessária do Windows 10 Enterprise e o software de aplicativo. Obter o software necessário para PATA e validá-lo usando as diretrizes em [limpa origem de mídia de instalação](http://aka.ms/cleansource).

        -   Windows 10 Enterprise Edition

        -   [Ferramentas de administração de servidor remoto](https://www.microsoft.com/en-us/download/details.aspx?id=45520.) para Windows 10

        -   Windows 10 [linhas de base de segurança](http://aka.ms/win10baselines)

        -   [Kit de ferramentas de mitigação aprimorada experiência (EMET)](https://www.microsoft.com/en-us/download/details.aspx?id=49166)

            > [!NOTE]
            > A Microsoft publica MD5 hashes para todos os sistemas operacionais e aplicativos no MSDN, mas nem todos os fornecedores de software fornecem uma documentação semelhante.  Nesses casos, outras estratégias será necessárias.  Para obter informações adicionais sobre validação de software, consulte [origem limpa](http://aka.ms/cleansource) para mídia de instalação.

    5.  **Certifique-se de que você tenha o servidor WSUS disponível na intranet**. Você precisará de um servidor WSUS na intranet para baixar e instalar atualizações para PATA. Este servidor WSUS deve ser configurado para aprovar automaticamente todas as atualizações de segurança para o Windows 10 ou um pessoal administrativo deve ter responsabilidade e responsabilidade para aprovar rapidamente as atualizações de software.

        > [!NOTE]
        > Para obter mais informações, consulte a seção "Automaticamente aprovar atualizações para instalação" o [orientações de aprovação de atualizações](https://technet.microsoft.com/en-us/library/cc708458(v=ws.10).aspx).

2.  Implantar a estrutura de UO de administrador para hospedar as patas

    1.  Baixe a biblioteca de script PATA de [Galeria do TechNet](http://aka.ms/PAWmedia)

        > [!NOTE]
        > Baixe todos os arquivos e salvá-los no mesmo diretório e executá-los na ordem especificada abaixo.  Create-PAWGroups depende da estrutura de UO criada por Create-PAWOUs e Set-PAWOUDelegation depende os grupos criados por Create-PAWGroups.
        > Não modifique qualquer um dos scripts ou o arquivo de valores separados por vírgula (CSV).

    2.  **Execute o script. ps1 Create-PAWOUs**.  Esse script criará a nova estrutura de unidade organizacional (UO) no Active Directory e bloquear herança GPO as UOs novo conforme apropriado.

    3.  **Execute o script. ps1 Create-PAWGroups**.  Esse script criará os novos grupos de segurança global nas UOs apropriadas.

        > [!NOTE]
        > Embora este script cria os novos grupos de segurança, ele não preencherá-los automaticamente.

    4.  **Execute o script. ps1 Set-PAWOUDelegation**.  Esse script atribuirá permissões para os novo UOs aos grupos apropriados.

3.  **Mover contas de nível 0 à UO de 0\Accounts Admin\Tier**. Mova cada conta que é um membro do administrador de domínio, o administrador da empresa, ou 0 grupos de equivalente (incluindo associação aninhada) para essa UO de nível. Se sua organização tem seus próprios grupos são adicionados a esses grupos, você deve movê-los à UO de 0\Groups Admin\Tier.

    > [!NOTE]
    > Para obter mais informações sobre quais grupos são nível 0, consulte "Faixa de 0 equivalência" em [protegendo Material de referência de acesso privilegiado](../securing-privileged-access/securing-privileged-access-reference-material.md).

4.  Adicionar os membros apropriados para os grupos relevantes

    1.  **Os usuários PATA** - adicionar os administradores de nível 0 com domínio ou administradores de empresa de grupos que você identificou na etapa 1 da fase 1.

    2.  **Manutenção PATA** – Adicione pelo menos uma conta que será usada para manutenção PATA e tarefas de solução de problemas. São raras as contas de manutenção PAW serão usadas.

        > [!NOTE]
        > Não adicione a mesma conta de usuário ou grupo para os usuários PAW e PAW manutenção.  O modelo de segurança PATA se baseia em parte do pressuposto de que a conta de usuário PATA tem privilegiados direitos em sistemas gerenciados ou sobre a PATA em si, mas não ambos.
        >
        > -   Isso é importante para a criação de boas práticas administrativas e hábitos na fase 1.
        > -   Isso é extremamente importante para a fase 2 e muito mais para evitar o escalonamento de privilégios por meio de PATA conforme patas sendo faixas distribuí-lo.
        >
        > Idealmente, a equipe de nenhuma é atribuída a tarefas em vários níveis de impor o princípio de diferenciação de direitos, mas a Microsoft reconhece que muitas organizações limitada equipe (ou outros requisitos organizacionais) que não permitem essa diferenciação completa. Nesses casos, a equipe mesmo pode ser atribuída a ambas as funções, mas não deve usar a mesma conta para essas funções.

5.  **Crie o objeto de política de grupo (GPO) "PAW configuração – computador" e link à UO de dispositivos de nível 0** ("dispositivos" em nível 0\Admin).  Nesta seção, você criará um novo "PAW configuração – computador" GPO que fornecem proteções específicas para essas patas.

    > [!NOTE]
    > **Não adicione essas configurações para a diretiva de domínio padrão**.  Isso afetará potencialmente operações em todo o seu ambiente do Active Directory.  Somente definir essas configurações nos GPOs recém-criada descritos aqui e aplicá-los apenas à UO de PAW.

    1.  **Acesso de manutenção PATA** – essa configuração irá definir a associação dos grupos privilegiados específicos em patas a um conjunto específico de usuários. Vá para *usuários locais do computador Configuration\Preferences\Control painel* e grupos e siga as etapas abaixo:

        1.  Clique em **nova** e clique em **Grupo Local**

        2.  Selecione o **atualização** ações e selecionados "administradores (interno)" (não use o botão Procurar para selecionar o grupo Administradores de domínio).

        3.  Selecione o **excluir todos os usuários membros** e **excluir todos os grupos de membro** caixas de seleção

        4.  Adicionar manutenção PAW (pawmaint) e o administrador (novamente, não use o botão Procurar para selecionar administrador).

            > [!NOTE]
            > Não adicione o grupo usuários PAW à lista de associação de grupo local Administradores.  Para garantir que os usuários PAW não pode deliberadamente ou acidentalmente modificar configurações de segurança a PATA em si, eles não devem ser membros do grupo Administradores local.

            Para obter mais informações sobre como usar preferências da política de grupo para modificar a associação de grupo, consulte o artigo da TechNet [configurar um Item do grupo Local](https://technet.microsoft.com/en-us/library/cc732525.aspx).

    2.  **Restringir o membro do grupo Local** -essa configuração garante que os membros de grupos de administrador local na estação de trabalho sempre está vazio

        1.  Vá para o computador Configuration\Preferences\Control painel locais usuários e grupos e siga as etapas abaixo:

            1.  Clique em **nova** e clique em **Grupo Local**

            2.  Selecione o **atualização** ação e selecione "operadores de Backup (interno)" (não use o botão Procurar para selecionar o grupo Operadores de Backup do domínio).

            3.  Selecione o **excluir todos os usuários membros** e **excluir todos os grupos de membro** caixas de seleção.

            4.  Não adicione todos os membros ao grupo.  Basta clicar em **Okey**.  Atribuindo uma lista vazia, política de grupo automaticamente removerá todos os membros e certifique-se de uma lista de associação em branco que cada política de grupo de tempo é atualizada.

        2.  Conclua as etapas acima para os seguintes grupos adicionais:

            -   Operadores de criptografia

            -   Administradores do Hyper-V

            -   Operadores de configuração de rede

            -   Usuários avançados

            -   Usuários da área de trabalho remota

            -   Replicators

        3.  Restrições ao Logon do PATA - essa configuração limitará as contas que podem se conectar a PATA. Siga as etapas abaixo para configurar essa configuração:

            1.  Acesse Assignment\Allow de direitos de usuário do computador \ Diretivas \ Configurações de segurança locais logon localmente.

            2.  Selecione definir essas configurações de política e adicione "Usuários PAW" e os administradores (novamente, não use o botão Procurar para selecionar os administradores).

        4.  **Bloquear o tráfego de rede entrada** -essa configuração garante que nenhum tráfego de rede de entrada não solicitadas tem permissão para a PATA. Siga as etapas abaixo para configurar essa configuração:

            1.  Vá para configuração de computador Settings\Security Settings\Windows Firewall with Advanced Security\Windows Firewall com segurança avançada e siga as etapas abaixo:

                1.  Clique com o botão direito no Firewall do Windows com segurança avançada e selecione **importar política**.

                2.  Clique em **Sim** para aceitar que isso substituirá qualquer políticas de firewall existentes.

                3.  Navegue até PAWFirewall.wfw e selecione **abrir**.

                4.  Clique em **Okey**.

                    > [!NOTE]
                    > Você pode adicionar endereços ou sub-redes que devem atingir a PATA com tráfego não solicitado nesse ponto (por exemplo, varredura ou gerenciamento de software de segurança.
                    > As configurações no arquivo WFW habilitá-lo no modo de "Bloquear – Default" para todos os perfis de firewall, desative a mesclagem de regra e Habilitar log de pacotes removidos e bem-sucedida. Essas configurações bloqueará o tráfego unsolicitied permitindo ainda comunicação bidirecional em conexões iniciadas a partir da PATA, impedir que os usuários com acesso de administrador local Criando regras de firewall locais que podem substituir as configurações de GPO e certifique-se de que o tráfego de entrada e saída a PATA é registrado.
                    > **Abrir esse firewall expandir a superfície de ataque para a PATA e aumentar o risco de segurança. Antes de adicionar os endereços, consulte a seção Gerenciando e operando PAW neste guia**.

        5.  **Configurar o Windows Update para o WSUS** -siga as etapas abaixo para alterar as configurações para configurar o Windows Update para as patas:

            1.  Vá para atualizações do computador configuração Modelos Administrativos\Componentes do Windows e siga as etapas abaixo:

                1.  Habilitar o **diretiva configurar atualizações automáticas**.

                2.  Selecione a opção **4 - download automático das atualizações e agendar a instalação**.

                3.  Mude a opção **dia de instalação agendado** para **0 - os dias** e a opção **hora da instalação agendado** como sua preferência organizacional.

                4.  Habilite a opção **especificar o local do serviço de atualização da intranet Microsoft** política, e especifique em ambas as opções a URL do servidor ESAE WSUS.

        6.  Vincule o "PAW configuração – computador" GPO da seguinte maneira:

            |Política|Link local|
            |-----|---------|
            |Configuração de PATA - computador |Admin\Tier 0\Devices|

6.  **Crie o objeto de política de grupo (GPO) "PAW configuração – usuário" e link à UO de contas de nível 0 ("contas" em nível 0\Admin)**.  Nesta seção, você criará um novo "PAW configuração – usuário" GPO que fornecem proteções específicas para essas patas.

    > [!NOTE]
    > Não adicione essas configurações para a diretiva de domínio padrão

    1.  **Bloquear a navegação na internet** - para impedir inadvertida a navegação na internet, isso definirá um endereço de proxy de um endereço de loopback (127.0.0.1).

        1.  Acesse Settings\Registry de configuração do usuário. Clique com botão direito do registro, selecione **nova** \> **Item do registro** e defina as seguintes configurações:

            1.  Ação: substituir

            2.  Seção: HKEY_CURRENT_USER

            3.  Caminho da chave: Software\Microsoft\Windows\CurrentVersion\Internet configurações

            4.  Nome do valor: ProxyEnable

                > [!NOTE]
                > Não marque a caixa de padrão à esquerda do nome do valor.

            5.  Tipo de valor: REG_DWORD

            6.  Dados do valor: 1

                1.  um.  Clique na guia comum e selecione **remover este item quando ele não for mais aplicado**.

                2.  Na guia comum selecione **direcionamento de nível de Item** e clique em **direcionamento**.

                3.  Clique em **Novo Item** e selecione **grupo de segurança**.

                4.  Selecione o botão "…" e procure o grupo usuários PAW.

                5.  Clique em **Novo Item** e selecione **grupo de segurança**.

                6.  Selecione o botão "…" e procurar o **administradores de serviços de nuvem** grupo.

                7.  Clique no **administradores de serviços de nuvem** item e clique em **Item opções**.

                8.  Selecione **não é**.

                9. Clique em **Okey** na janela de direcionamento.

            7.  Clique em **Okey** para concluir a configuração de política de grupo ProxyServer

        2.  Acesse Settings\Registry de configuração do usuário. Clique com botão direito do registro, selecione **nova** \> **Item do registro** e defina as seguintes configurações:

            -   Ação: substituir

            -   Seção: HKEY_CURRENT_USER

            -   Caminho da chave: Software\Microsoft\Windows\CurrentVersion\Internet configurações

            -   Nome do valor: ProxyServer

                > [!NOTE]
                > Não marque a caixa de padrão à esquerda do nome do valor.

            -   Tipo de valor: REG_SZ

            -   Dados do valor: 127.0.0.1:80

                1.  Clique no **comuns** guia e selecione **remover este item quando ele não for mais aplicado**.

                2.  Sobre o **comuns** guia selecione **direcionamento de nível de Item** e clique em **direcionamento**.

                3.  Clique em **Novo Item** e grupo de segurança select.

                4.  Selecione o botão "…" e adicione o grupo usuários PAW.

                5.  Clique em **Novo Item** e grupo de segurança select.

                6.  Selecione o botão "…" e procurar o **administradores de serviços de nuvem** grupo.

                7.  Clique no **administradores de serviços de nuvem** item e clique em **Item opções**.

                8.  Selecione **não é**.

                9. Clique em **Okey** na janela de direcionamento.

        3.  Clique em **Okey** para concluir a configuração de política de grupo ProxyServer

    2.  Vá para configuração do usuário \ Modelos Administrativos\Componentes do Windows\Internet Explorer e habilitar as opções abaixo. Essas configurações impedirá que os administradores substituam manualmente as configurações de proxy.

        1.  Habilitar o **desativar a alteração de configuração automática** configurações.

        2.  Habilitar o **impedir a alteração de configurações de proxy**.

7.  **Impedir que administradores logon hosts de nível inferiores**.  Nesta seção, configuraremos políticas de grupo para impedir que contas privilegiadas administrativas logon hosts de nível inferiores.

    1.  Criar o novo **restringir o Logon de estação de trabalho** GPO - essa configuração irá restringir nível 0 e contas de administrador do nível 1 de fazer logon em estações de trabalho padrão.  Esse GPO deve ser vinculado à UO de nível superior "Estações de trabalho" e ter as seguintes configurações:

        - (i) No Assignment\Deny direitos diretivas de locais do computador \ Diretivas \ Configurações de segurança fazer logon como um trabalho em lotes, selecione **definir essas configurações de política** e adicione o nível 0 e grupos de nível 1:

            **Grupos para adicionar configurações de política:**

            Administradores corporativos

            Administradores de domínio

            Administradores de esquema

            DOMAIN\Administrators

            Operadores de conta

            Operadores de backup

            Operadores de impressão

            Operadores de servidor

            Controladores de domínio

            Read-Only Controladores de domínio

            Proprietários criadores de política de grupo

            Operadores de criptografia

            > [!NOTE]
            > Observação: Os grupos internos do nível 0, consulte equivalência de nível 0 para obter mais detalhes.

            Outros recebido grupos

            > [!NOTE]
            > Observação: Qualquer personalizado criado grupos com acesso de nível 0 eficaz, consulte equivalência de nível 0 para obter mais detalhes.

            Administradores de nível 1

            > [!NOTE]
            > Observação: Este grupo foi criado anteriormente na fase 1

        - (ii) Em Assignment\Deny direitos diretivas de locais do computador \ Diretivas \ Configurações de segurança fazer logon como um serviço, selecione **definir essas configurações de política** e adicione o nível 0 e grupos de nível 1:

            **Grupos para adicionar configurações de política:**

            Administradores corporativos

            Administradores de domínio

            Administradores de esquema

            DOMAIN\Administrators

            Operadores de conta

            Operadores de backup

            Operadores de impressão

            Operadores de servidor

            Controladores de domínio

            Read-Only Controladores de domínio

            Proprietários criadores de política de grupo

            Operadores de criptografia

            > [!NOTE]
            > Observação: Os grupos internos do nível 0, consulte equivalência de nível 0 para obter mais detalhes.

            Outros recebido grupos

            > [!NOTE]
            > Observação: Qualquer personalizado criado grupos com acesso de nível 0 eficaz, consulte equivalência de nível 0 para obter mais detalhes.

            Administradores de Teir 1

            > [!NOTE]
            > Observação: Este grupo foi criado anteriormente na fase 1

    2.  Criar o novo **restringir Logon de servidor** GPO - essa configuração irá restringir contas de administrador de nível 0 de fazer logon nos servidores de nível 1.  Esse GPO deve ser vinculado à UO de nível superior "Nível 1 servidores" e ter as seguintes configurações:

        -   (i) No Assignment\Deny direitos diretivas de locais do computador \ Diretivas \ Configurações de segurança fazer logon como um trabalho em lotes, selecione **definir essas configurações de política** e adicione os grupos de nível 0:

            **Grupos para adicionar configurações de política:**

            Administradores corporativos

            Administradores de domínio

            Administradores de esquema

            DOMAIN\Administrators

            Operadores de conta

            Operadores de backup

            Operadores de impressão

            Operadores de servidor

            Controladores de domínio

            Read-Only Controladores de domínio

            Proprietários criadores de política de grupo

            Operadores de criptografia

            > [!NOTE]
            > Observação: Os grupos internos do nível 0, consulte equivalência de nível 0 para obter mais detalhes.

            Outros recebido grupos

            > [!NOTE]
            > Observação: Qualquer personalizado criado grupos com acesso de nível 0 eficaz, consulte equivalência de nível 0 para obter mais detalhes.

        -   (ii) Em Assignment\Deny direitos diretivas de locais do computador \ Diretivas \ Configurações de segurança fazer logon como um serviço, selecione **definir essas configurações de política** e adicione os grupos de nível 0:

            **Grupos para adicionar configurações de política:**

            Administradores corporativos

            Administradores de domínio

            Administradores de esquema

            DOMAIN\Administrators

            Operadores de conta

            Operadores de backup

            Operadores de impressão

            Operadores de servidor

            Controladores de domínio

            Read-Only Controladores de domínio

            Proprietários criadores de política de grupo

            Operadores de criptografia

            > [!NOTE]
            > Observação: Os grupos internos do nível 0, consulte equivalência de nível 0 para obter mais detalhes.

            Outros recebido grupos

            > [!NOTE]
            > Observação: Qualquer personalizado criado grupos com acesso de nível 0 eficaz, consulte equivalência de nível 0 para obter mais detalhes.

        -   (iii) No logon do computador \ Diretivas \ Configurações de segurança locais diretivas direitos Assignment\Deny localmente, selecione **definir essas configurações de política** e adicione os grupos de nível 0:

            **Grupos para adicionar configurações de política:**

            Administradores corporativos

            Administradores de domínio

            Administradores de esquema

            Operadores de conta

            Operadores de backup

            Operadores de impressão

            Operadores de servidor

            Controladores de domínio

            Read-Only Controladores de domínio

            Proprietários criadores de política de grupo

            Operadores de criptografia

            > [!NOTE]
            > Observação: Os grupos internos do nível 0, consulte equivalência de nível 0 para obter mais detalhes.

            Outros recebido grupos

            > [!NOTE]
            > Observação: Qualquer personalizado criado grupos com acesso de nível 0 eficaz, consulte equivalência de nível 0 para obter mais detalhes.

8.  Implantar seu patas

    > [!NOTE]
    > Certifique-se de que o PATA estiver desconectada da rede durante o processo de compilação do sistema operacional.

    1.  Instale o Windows 10 usando a mídia de instalação limpa de origem que você obteve anteriormente.

        > [!NOTE]
        > Você pode usar o Microsoft Deployment Toolkit (MDT) ou outro sistema de implantação de imagem automatizados para automatizar a implantação de PATA, mas você deve garantir que o processo de compilação é tão confiável quanto a PATA. Adversários especificamente buscam imagens corporativas e sistemas de implantação (incluindo ISOs, pacotes de implantação, etc.) como um mecanismo de persistência para que preexistente sistemas de implantação ou imagens não devem ser usadas.
        >
        > Se você automatizar a implantação da PATA, faça o seguinte:
        >
        > -   Criar o sistema usando a mídia de instalação validada usando as diretrizes em [limpa origem de mídia de instalação](http://aka.ms/cleansource).
        > -   Certifique-se de que o sistema de implantação automatizada estiver desconectado da rede durante o processo de compilação do sistema operacional.

    2.  Defina uma senha complexa exclusiva para a conta de administrador local.  Não use uma senha que foi usada para qualquer outra conta no ambiente.

        > [!NOTE]
        > A Microsoft recomenda usar [solução de senha de Administrador Local (VOLTAS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) para gerenciar a senha de administrador local para todas as estações de trabalho, incluindo patas.  Se você usar VOLTAS, certifique-se de que você concede apenas o grupo de manutenção PAW à direita para ler as senhas gerenciadas VOLTAS para as patas.

    3.  Instale remoto Server Administration Tools para Windows 10 usando a mídia de instalação limpa de origem.

    4.  Instale avançado mitigação experiência Toolkit (EMET) usando a mídia de instalação limpa de origem.

        > [!NOTE]
        > Observe que, no momento da última atualização deste guia, EMET 5.5 ainda estava em teste beta.  Como isso é a primeira versão com suporte no Windows 10, ele é incluído aqui apesar de seu estado de beta.  Se instalar software beta na PATA exceder seu tolerância de risco, você pode ignorar esta etapa por enquanto.

    5.  Conecte a PATA à rede.  Certifique-se de que o PATA pode se conectar ao controlador de domínio pelo menos uma (DC).

    6.  Usando uma conta que seja um membro do grupo PAW manutenção, execute o seguinte comando do PowerShell da PATA recém-criado para ingressar no domínio na UO apropriada:

        `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

        Substitua as referências a *Fabrikam* com seu nome de domínio, conforme apropriado.  Se o nome de domínio se estende para vários níveis (por exemplo, child.fabrikam.com), adicione os nomes adicionais com a "DC =" identificador na ordem em que eles aparecem no nome de domínio totalmente qualificado do domínio.

        > [!NOTE]
        > Se você já tiver implantado uma [ESAE administrativa floresta](http://aka.ms/esae) (para administradores de nível 0 na fase 1) ou um [Microsoft Identity Manager (MIM) privilegiados gerenciamento de acesso (PAM)](http://aka.ms/mimpamdeploy) (para nível 1 e 2 administradores na fase 2), você pode ingressar a PATA ao domínio nesse ambiente aqui em vez do domínio de produção.

    7.  Aplica todas as atualizações críticas e importantes do Windows antes de instalar qualquer outro software (incluindo ferramentas administrativas, agentes, etc.).

    8.  Força o aplicativo de diretiva de grupo.

        1.  Abra um prompt de comando com privilégios elevados e digite o seguinte comando:

            `Gpupdate /force /sync`

        2.  Reinicie o computador

    9. **(Opcional) **Instalar ferramentas adicionais necessárias para administradores do Active Directory. Instale outras ferramentas ou scripts necessários para realizar seus trabalhos. Certifique-se para avaliar o risco de exposição de credencial nos computadores de destino com qualquer ferramenta antes de adicioná-lo a uma PATA. Acesso [nesta página](http://aka.ms/logontypes) para obter mais informações sobre como avaliar ferramentas administrativas e métodos de conexão para o risco de exposição de credenciais. Certifique-se para obter todas as mídias de instalação usando a diretriz em [limpa origem de mídia de instalação](http://aka.ms/cleansource).

        > [!NOTE]
        > Usando um servidor de atalhos para um local central para essas ferramentas pode reduzir a complexidade, mesmo se ele não servir como um limite de segurança.

    10. **(Opcional) **Baixar e instalar o software necessário acesso remoto. Se os administradores usarão a PATA remotamente para administração, instale o software de acesso remoto usando as diretrizes de segurança de seu fornecedor de solução de acesso remoto. Certifique-se para obter todas as mídias de instalação usando a diretriz na origem limpo para a mídia de instalação.

        > [!NOTE]
        > Considere cuidadosamente todos os riscos envolvidos na permitindo o acesso remoto via um PATA.  Enquanto uma PATA móvel permite vários cenários importantes, incluindo o trabalho de casa, software de acesso remoto pode estar vulnerável a ataques e usados para comprometer uma PATA.

    11. Valide a integridade do sistema PATA examinando e confirmando que todas as configurações apropriadas estão no lugar usando as etapas abaixo:

        1.  Confirmar que as políticas de grupo PATA específicas sejam aplicadas à PATA

            1.  Abra um prompt de comando com privilégios elevados e digite o seguinte comando:

                `Gpresult /scope computer /r`

            2.  Examine a lista resultante e verifique se o grupo somente as políticas que aparecem são aqueles que você criou acima.

        2.  Confirme se nenhuma conta de usuário adicionais é membros do grupo privilegiado na PATA usando as etapas abaixo:

            1.  Abrir **editar usuários e grupos locais** (lusrmgr.msc), selecione **grupos**e confirme que apenas os membros do grupo Administradores local são a conta de administrador local e o grupo de segurança global PAW manutenção.

                > [!NOTE]
                > O grupo usuários PAW não deve ser um membro do grupo Administradores local.  Os únicos membros devem ser a conta de administrador local e o grupo de segurança global PAW manutenção (e os usuários PAW não deve ser um membro desse grupo global ou).

            2.  Também usando **editar usuários e grupos locais**, certifique-se de que os grupos a seguir não têm membros:

                -   Operadores de backup

                -   Operadores de criptografia

                -   Administradores do Hyper-V

                -   Operadores de configuração de rede

                -   Usuários avançados

                -   Usuários da área de trabalho remota

                -   Replicators

    12. **(Opcional) **Se sua organização usa uma solução de gerenciamento (SIEM) do evento e informações de segurança, certifique-se de que o PATA é [configurado para encaminhar eventos para o sistema usando o encaminhamento de eventos do Windows (WEF)](http://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx) ou caso contrário, é registrado com a solução para que o SIEM ativamente está recebendo eventos e informações da PATA.  Os detalhes dessa operação varia de acordo com sua solução SIEM.

        > [!NOTE]
        > Se seu SIEM requer um agente que é executado como o sistema ou uma conta administrativa local nas patas, certifique-se de que o SIEMs são gerenciadas com o mesmo nível de confiança que seus sistemas de identidade e controladores de domínio.

    13. **(Opcional) **Se você optou por implantar VOLTAS para gerenciar a senha da conta de administrador local no seu PATA, verificar se a senha está registrada com êxito.

        -   Usando uma conta com permissões para ler senhas gerenciadas VOLTAS, abra **usuários e Active Directory computadores** (dsa.msc).  Certifique-se de que recursos avançados está habilitado e, em seguida, clique com botão direito do objeto de computador apropriado.  Selecione a guia Editor de atributo e confirme que o valor de msSVSadmPwd é preenchido com uma senha válida.

#### <a name="phase-2---extend-paw-to-all-administrators"></a>Fase 2 - estender PATA para todos os administradores
Escopo: Todos os usuários com direitos administrativos sobre aplicativos essenciais e dependências.  Isso deve incluir pelo menos os administradores dos servidores de aplicativos, operacional integridade e monitoramento de dispositivos de rede, soluções de virtualização, sistemas de armazenamento e soluções de segurança.

> [!NOTE]
> As instruções nessa fase pressupõem que fase 1 foi concluída em sua totalidade.  Não começa a fase 2 até que você concluiu todas as etapas na fase 1.

Depois de confirmar que todas as etapas foram concluídas, siga as etapas abaixo para concluir a fase 2:

1.  **(Recomendado) **Habilitar **RestrictedAdmin** modo - habilitar esse recurso em seus servidores existentes e estações de trabalho, em seguida, impor o uso desse recurso. Esse recurso exigirá que os servidores de destino esteja executando o Windows Server 2008 R2 ou posterior e estações de trabalho esteja executando o Windows 7 ou posterior de destino.

    1.  Habilitar **RestrictedAdmin** modo em seus servidores e estações de trabalho, seguindo as instruções disponíveis nesse [página](http://aka.ms/rdpra).

        > [!NOTE]
        > Antes de ativar esse recurso para servidores oposta da internet, você deve considerar o risco de adversários incapacidade de autenticar a esses servidores com um hash da senha roubada anteriormente.

    2.  Crie o objeto de política de grupo (GPO) "RestrictedAdmin necessária – computador". Esta seção cria um GPO que aplica o uso do /RestrictedAdmin alternar para saída conexões de área de trabalho remota, Protegendo contas contra roubo de credenciais nos sistemas de destino

        -   Vá para o computador configuração Templates\System\Credentials Delegation\Restrict delegação de credenciais para servidores remotos e defina **Enabled**.

    3.  Link o **RestrictedAdmin** obrigatório - o computador para o apropriado nível 1 e/ou nível 2 dispositivos usando as opções de política abaixo:

        Configuração de PATA - computador

        -> Link local: Admin\Tier 0\Devices (existente)

        PAW configuração - usuário

        -> Link local: Admin\Tier 0\Accounts

        RestrictedAdmin obrigatório - computador

        -> Admin\Tier1\Devices ou -> Admin\Tier2\Devices (ambos são opcionais)

        > [!NOTE]
        > Isso não é necessário para sistemas de nível 0 como esses sistemas já estão no controle total de todos os ativos no ambiente.

2.  Mova objetos do nível 1 para as UOs apropriadas.

    1.  Mova para o Admin\Tier 1\Groups UO de grupos de nível 1. Localize todos os grupos que conceda os seguintes direitos administrativos e movê-los para essa UO.

        -   Administrador local em mais de um servidor

        -   Acesso administrativo aos serviços de nuvem

        -   Acesso administrativo aos aplicativos corporativos

    2.  Mova contas de nível 1 à UO de 1\Accounts Admin\Tier. Mova cada conta que é um membro desses grupos de nível 1 (incluindo associação aninhada) para essa UO.

3.  Adicionar os membros apropriados para os grupos relevantes

    -   **Nível 1 administradores** -esse grupo conterá os administradores de 1 de camada que serão restritos acessem hosts de nível 2. Adicione todas as de seus grupos administrativos nível 1 que tenham privilégios administrativos em servidores ou serviços de internet.

        > [!NOTE]
        > Se pessoal administrativo tiver obrigações para gerenciar ativos em vários níveis, você precisará criar uma conta de administrador separado por nível.

4.  Habilite o Credential Guard reduzir o risco de roubo de credenciais e reutilizar.  O Credential Guard é um novo recurso do Windows 10 que restringe o acesso do aplicativo às credenciais, impedindo que ataques de roubo de credenciais (incluindo Pass-the-Hash).  O Credential Guard é completamente transparente para o usuário final e exige uma configuração mínima de tempo e esforço.  Para obter mais informações sobre o Credential Guard, incluindo as etapas de implantação e os requisitos de hardware, consulte o artigo [proteger credenciais de domínio com o Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx).

    > [!NOTE]
    > O Device Guard deve estar habilitado para configurar e usar o Credential Guard.  No entanto, você não é necessária para configurar quaisquer outras proteções de Device Guard para usar o Credential Guard.

5.  **(Opcional) **Habilitar conectividade com serviços de nuvem. Esta etapa permite o gerenciamento de serviços de nuvem como o Office 365 e o Azure com garantias de segurança adequadas. Esta etapa também é necessária para o Microsoft Intune gerenciar as patas.

    > [!NOTE]
    > Se nenhuma conectividade em nuvem é necessária para a administração de serviços de nuvem ou gerenciamento pelo Intune, ignore esta etapa.

    Estas etapas serão restringir a comunicação pela internet para somente os serviços de nuvem autorizados (mas não na internet aberta) e adicione proteções para os navegadores e outros aplicativos que processará o conteúdo da internet. Esses patas para administração nunca devem ser usadas para tarefas de usuário padrão como comunicações de internet e produtividade.

    Para habilitar a conectividade para PATA serviços sigam as etapas abaixo:

    1.  Configure PATA para permitir que apenas autorizados destinos da Internet.  Como estender a implantação PATA para habilitar a administração de nuvem, você precisa permitir o acesso aos serviços autorizados enquanto filtrando o acesso da internet aberta onde ataques mais facilmente podem ser montados contra seus administradores.

        1.  Criar **administradores de serviços de nuvem** de grupo e adicione todas as contas que exigem acesso aos serviços de nuvem na internet.

        2.  Baixe a PATA *proxy.pac* do arquivo de [Galeria do TechNet](http://aka.ms/pawmedia) e publicá-lo em um site interno.

            > [!NOTE]
            > Você precisará atualizar o *proxy.pac* arquivo após o download para garantir que ele seja atualizado e completo.  
            > A Microsoft publica todas as URLs do Azure e Office 365 de atual no escritório [Centro de suporte](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
            >
            > Talvez seja necessário adicionar outros destinos de Internet válidos para adicionar a essa lista para outro provedor de IaaS, mas não adicione produtividade, entretenimento, notícias ou sites à lista de pesquisa.
            >
            > Talvez você precise ajustar o arquivo PAC para acomodar um endereço de proxy válido para usar para esses endereços.

            > [!NOTE]
            > Você também pode restringir o acesso da PATA usando um proxy da web também para defesa aprofundada. Não recomendamos usar isso por si só sem o arquivo PAC conforme ele só irá restringir o acesso para patas enquanto estão conectados à rede corporativa.

            Essas instruções presumem que você usará o Internet Explorer (ou o Microsoft Edge) para administração do Office 365, Azure e outros serviços de nuvem. A Microsoft recomenda configurar restrições semelhantes para qualquer 3ª navegadores de terceiros que exigem para administração. Navegadores da Web em patas só devem ser usados para administração dos serviços de nuvem e nunca para navegação na web geral.

        3.  Depois que você configurou o *proxy.pac* de arquivos, atualizar a configuração PAW - GPO de usuário.

            1.  Acesse Settings\Registry de configuração do usuário. Clique com botão direito do registro, selecione **nova** \> **Item do registro** e defina as seguintes configurações:

                1.  Ação: substituir

                2.  Hive: HKEY _ CURRENT_USER

                3.  Caminho da chave: Software\Microsoft\Windows\CurrentVersion\Internet configurações

                4.  Nome do valor: AutoConfigUrl

                    > [!NOTE]
                    > Não selecione a **padrão** caixa à esquerda do nome do valor.

                5.  Tipo de valor: REG_SZ

                6.  Dados do valor: insira a URL completa para o *proxy.pac* arquivo, incluindo http:// e o nome do arquivo - por exemplo http://proxy.fabrikam.com/proxy.pac.  A URL também pode ser uma URL de rótulo único - por exemplo, http://proxy/proxy.pac

                    > [!NOTE]
                    > O arquivo PAC também pode ser hospedado em um compartilhamento de arquivos, com a sintaxe de file://server.fabrikan.com/share/proxy.pac, mas isso requer permitindo que o protocolo file://. Consulte a seção "Nota: File://-based Proxy Scripts preterido" deste [configuração de Proxy da Web compreensão](http://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx) blog para obter detalhes adicionais sobre como configurar o valor de registro necessárias.

                7.  Clique no **comuns** guia e selecione **remover este item quando ele não for mais aplicado**.

                8.  Sobre o **comuns** guia selecione **direcionamento de nível de Item** e clique em **direcionamento**.

                9. Clique em **Novo Item** e selecione **grupo de segurança**.

                10. Selecione o botão "…" e procurar o **administradores de serviços de nuvem** grupo.

                11. Clique em **Novo Item** e selecione **grupo de segurança**.

                12. Selecione o botão "…" e procurar o **PAW usuários** grupo.

                13. Clique no **PAW usuários** item e clique em **Item opções**.

                14. Selecione **não é**.

                15. Clique em **Okey** na janela de direcionamento.

                16. Clique em **Okey** para concluir o **AutoConfigUrl** configuração de política de grupo.

    2.  Aplica linhas de base de segurança do Windows 10 e o Link de acesso do serviço de nuvem as linhas de base de segurança para o Windows e para o serviço de nuvem acessem (se necessário) para as UOs corretas usando as etapas abaixo:

        1.  Extraia o conteúdo do arquivo ZIP de linhas de base de segurança do Windows 10.

        2.  Criá-los GPOs, [importar a política](https://technet.microsoft.com/en-us/library/cc753786.aspx) configurações, e [link](https://technet.microsoft.com/en-us/library/cc732979.aspx) por esta tabela. Vincular cada política para cada local e garantir a ordem seguirá a tabela (inferiores entradas na tabela devem ser aplicada mais tarde e maior prioridade):

            **Políticas:**

            |||
            |-|-|
            |CM Windows 10 - segurança de domínio|N/d - não vincular agora|
            |SCM Windows 10 TH2 - computador|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |Windows 10 de SCM TH2-BitLocker|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Windows 10 - o Credential Guard|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Internet Explorer - computador|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |Configuração de PATA - computador|Admin\Tier 0\Devices (existente)|
            ||Admin\Tier 1\Devices (Novo Link)|
            ||Admin\Tier 2\Devices (Novo Link)|
            |RestrictedAdmin obrigatório - computador|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Windows 10 - usuário|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |SCM Internet Explorer - usuário|Admin\Tier 0\Devices|
            ||Admin\Tier 1\Devices|
            ||Admin\Tier 2\Devices|
            |PAW configuração - usuário|Admin\Tier 0\Devices (existente)|
            ||Admin\Tier 1\Devices (Novo Link)|
            ||Admin\Tier 2\Devices (Novo Link)|

            > [!NOTE]
            > A "SCM Windows 10 – segurança do domínio" GPO podem estar vinculada ao domínio independentemente PATA, mas afetará todo o domínio.

6.  **(Opcional) **Instalar ferramentas adicionais necessárias para administradores do nível 1. Instale outras ferramentas ou scripts necessários para realizar seus trabalhos. Certifique-se para avaliar o risco de exposição de credencial nos computadores de destino com qualquer ferramenta antes de adicioná-lo a uma PATA. Para obter mais informações sobre como avaliar ferramentas administrativas e conexão métodos para o risco de exposição de credencial visite [nesta página](http://aka.ms/logontypes). Certifique-se para obter todas as mídias de instalação usando a diretriz na origem limpo para a mídia de instalação

7.  Identifique e obter com segurança o software e os aplicativos necessários para administração.  Isso é semelhante para o trabalho realizado na fase 1, mas com um escopo mais amplo devido o número maior de aplicativos, serviços e sistemas sejam protegidos.

    > [!NOTE]
    > Certifique-se de que você proteja esses novos aplicativos (incluindo navegadores da web), recusando-los as proteções fornecidas pelo EMET.

    Exemplos de aplicativos e software adicional incluem:

    -   PowerShell do Microsoft Azure

    -   Office 365 PowerShell (também conhecido como Microsoft Online Services Module)

    -   Aplicativo ou software de gerenciamento de serviço com base em Console de gerenciamento Microsoft

    -   Aplicativo proprietário (não MMC baseados) ou software de gerenciamento de serviço

        > [!NOTE]
        > Muitos aplicativos exclusivamente agora são gerenciados por meio de navegadores da web, incluindo muitos serviços de nuvem.  Enquanto isso reduz o número de aplicativos que precisam ser instaladas em uma PATA, ele também introduz o risco de problemas de interoperabilidade de navegador.  Talvez seja necessário implantar um navegador da web não são da Microsoft em instâncias PATA específicas para habilitar a administração de serviços específicos.  Se você implantar um navegador da web adicionais, certifique-se de que você siga todos os princípios de origem limpa e segura o navegador de acordo com diretrizes de segurança do fornecedor.

8.  **(Opcional) **Baixe e instale qualquer agentes de gerenciamento necessárias.

    > [!NOTE]
    > Se você optar por instalar agentes de gerenciamento adicionais (monitoramento, segurança, gerenciamento de configuração, etc.), é essencial que você garanta que os sistemas de gerenciamento são confiáveis no mesmo nível como controladores de domínio e sistemas de identidade. Veja a gerenciar e atualizar patas para obter orientação adicional.

9. Avalie a infraestrutura para identificar sistemas que exigem as proteções de segurança adicionais fornecidas por uma PATA.  Certifique-se de que você sabe exatamente quais sistemas devem ser protegidos.  Faça perguntas críticas sobre os recursos propriamente ditas, como:

    -   Onde estão os sistemas de destino que devem ser gerenciados?  São eles coletados em um único local físico ou conectados a uma única sub-rede bem definida

    -   Como muitos sistemas há?

    -   Esses sistemas dependem de outros sistemas (virtualização, armazenamento, etc.) e, em caso afirmativo, como são esses sistemas gerenciados?  Como os sistemas críticos expostos a essas dependências e quais são os riscos adicionais são associados com essas dependências?

    -   Quão crítica são os serviços que estão sendo gerenciados e qual é a perda esperada se esses serviços estão comprometidos?

        > [!NOTE]
        > Incluir seus serviços de nuvem nesta avaliação – invasores têm como alvo cada vez mais implantações de nuvem inseguro e é essencial que você administrar esses serviços com a segurança como faria com seus aplicativos essenciais no local.

        Usar essa avaliação para identificar os sistemas específicos que exigem proteção adicional e estender seu programa PATA para os administradores desses sistemas.  Exemplos comuns de sistemas que se beneficiam muito do administração baseada em PATA incluem SQL Server (local e SQL Azure), aplicativos de recursos humanos e software financeiro.

        > [!NOTE]
        > Se um recurso é gerenciado por um sistema Windows, podem ser gerenciado com uma PATA, mesmo se o aplicativo em si é executado em um sistema operacional diferente do Windows ou em uma plataforma de nuvem não são da Microsoft.  Por exemplo, o proprietário de uma assinatura do Amazon Web Services só deve usar uma PATA para administrar essa conta.

10. Desenvolva um método de solicitação e a distribuição para implantar patas em escala em sua organização.  Dependendo do número de patas que você escolher para implantar na fase 2, talvez seja necessário automatizar o processo.

    -   Considere a possibilidade de desenvolver uma solicitação formal e o processo de aprovação de administradores usar para obter uma PATA.  Esse processo ajudaria padronizar o processo de implantação, certifique-se responsabilidade para dispositivos PATA e ajudar a identificar lacunas na implantação PATA.

    -   Conforme afirmado anteriormente, essa solução de implantação deve ser separada de métodos de automação existente (que talvez já tenha sido comprometidos) e deve seguir os princípios descritos na fase 1.

        > [!NOTE]
        > Qualquer sistema que gerencia recursos em si gerenciado no nível de confiança igual ou superior.

11. Examine e se necessário implantar perfis de hardware PATA adicionais.  O perfil de hardware que você escolheu para implantação de fase 1 pode não ser adequado para todos os administradores.  Examine os perfis de hardware e, se apropriado selecione perfis de hardware PATA adicionais para atender às necessidades dos administradores.  Por exemplo, o perfil de Hardware dedicado (separado PATA e diárias usam estações de trabalho) pode ser inadequado para um administrador que viaja com frequência – nesse caso, você pode optar por implantar o perfil usar simultâneos (PATA com usuário VM) para esse administrador.

12. Considere as comunicações culturais e operacionais e as necessidades de treinamento que acompanhar uma implantação PATA estendida.   Uma alteração significativa para um modelo administrativo naturalmente exigirá gerenciamento de alteração em algum grau e é essencial criar que para o projeto de implantação em si.  Considere a possibilidade de no mínimo o seguinte:

    -   Como você irá se comunicar as alterações à liderança sênior para garantir seu suporte?  Qualquer projeto sem liderança sênior fazendo é provável que uma falha, ou em um mínimo luta por fundos e ampla aceitação.

    -   Como você irá documentar o novo processo para os administradores?  Essas alterações devem ser documentadas e comunicadas não apenas administradores existentes (que devem alterar seus hábitos e gerenciar recursos de maneira diferente), mas também para novos administradores (aqueles promovido dentro ou contratada de fora da organização).  É essencial que a documentação está clara e totalmente enfatiza a importância das ameaças, a função do PATA em proteger os administradores e como usar PATA corretamente.

        > [!NOTE]
        > Isso é especialmente importante para funções com alta rotatividade, incluindo, mas não se limitando a equipe de assistência técnica.

    -   Como você pode garantir a conformidade com o novo processo?  Enquanto o modelo de PATA inclui uma série de controles técnicos para evitar a exposição de credenciais com privilégios, é impossível impedir completamente todos os possíveis riscos puramente usando controles técnicos.  Por exemplo, embora seja possível impedir a logon com êxito uma área de trabalho do usuário com credenciais com privilégios de administrador, o simples ato de tentativa o logon pode expor as credenciais a malware instalada nessa área de trabalho do usuário.  Portanto, é essencial que você articular não apenas os benefícios do modelo de PATA, mas os riscos de não conformidade.  Isso deve ser complementado por auditoria e alertas para que a exposição de credenciais pode ser detectada e resolvida rapidamente.

#### <a name="phase-3-extend-and-enhance-protection"></a>Fase 3: Estender e melhorar a proteção
Escopo: Essas proteções aprimoram os sistemas compilados na fase 1, aumentando a proteção básica com recursos avançados, incluindo a autenticação multifator e regras de acesso de rede.

> [!NOTE]
> Essa fase pode ser executada a qualquer momento após a conclusão da fase 1.  Ele não é dependente a conclusão da fase 2 e, portanto, pode ser executado antes, simultâneas com ou após a fase 2.

Siga as etapas abaixo para configurar essa fase:

1.  Habilite a autenticação multifator para contas privilegiadas.  A autenticação multifator reforça a segurança da conta exigindo que o usuário forneça um token físico além de credenciais.  A autenticação multifator complementa as políticas de autenticação extremamente bem, mas ele não depende de políticas de autenticação para implantação (e então, da mesma maneira, políticas de autenticação não exigir autenticação multifator).  A Microsoft recomenda usando uma destas formas de autenticação multifator:

    -   **Cartão inteligente**: um cartão inteligente é um dispositivo físico à prova de adulterações e portátil que fornece uma segunda verificação durante o processo de logon do Windows.  Exigindo um indivíduo para possuem um cartão para logon, você pode reduzir o risco de credenciais roubadas sendo reutilizado remotamente.  Para obter detalhes sobre o logon com cartão inteligente no Windows, consulte o artigo [visão geral de cartão inteligente](https://technet.microsoft.com/en-us/library/hh831433.aspx).

    -   **Cartão inteligente virtual**: um cartão inteligente virtual fornece os mesmos benefícios de segurança físicos como cartões inteligentes, com a vantagem de ser vinculado ao hardware específico.  Para obter detalhes sobre a implantação e requisitos de hardware, consulte os artigos, [visão geral de cartão inteligente Virtual](https://technet.microsoft.com/en-us/library/dn593708.aspx) e [começar com Virtual cartões inteligentes: guia passo a passo](https://technet.microsoft.com/en-us/library/dn579260.aspx).

    -   **O Microsoft Passport**: Microsoft Passport permite que os usuários se autenticar em uma conta da Microsoft, uma conta do Active Directory, uma conta da Microsoft Azure Active Directory (Azure AD) ou serviço não Microsoft que dá suporte à autenticação Fast ID Online (FIDO). Após uma verificação inicial em duas etapas durante o registro do Microsoft Passport, um Microsoft Passport é configurado no dispositivo do usuário e o usuário define um gesto, que pode ser um PIN ou Windows Hello. As credenciais do Microsoft Passport são um par de chaves assimétricas, que pode ser gerado em ambientes isolados de Trusted Platform Modules (TPMs).
        Para obter mais informações sobre o Microsoft Passport leia [visão geral do Microsoft Passport](https://technet.microsoft.com/en-us/library/dn985839%28v=vs.85%29.aspx) artigo.

    -   **Autenticação multifator Azure**: Azure a autenticação multifator (MFA) fornece a segurança de um segundo fator de verificação, bem como proteção avançada por meio da análise de monitoramento e com base em aprendizado de máquina.  Azure MFA pode seguro não apenas administradores Azure, mas muitas outras soluções também, incluindo aplicativos web, Active Directory do Azure e soluções de local como acesso remoto e área de trabalho remota.  Para obter mais informações sobre autenticação multifator Azure, consulte o artigo [autenticação multifator](https://azure.microsoft.com/en-us/services/multi-factor-authentication).

2.  **Lista branca trusted aplicativos usando o Device Guard e/ou AppLocker**.  Ao limitar a capacidade do código não assinado ou não confiável para execução em uma PATA, reduzir ainda mais a probabilidade de atividade maliciosa e comprometimento.  O Windows inclui duas opções principais para o controle de aplicativo:

    -   **O AppLocker**: AppLocker ajuda administradores a controlarem quais aplicativos podem ser executados em um determinado sistema.  O AppLocker pode ser centralmente controlado pela política de grupo e aplicado a usuários ou grupos (por aplicativo direcionado para os usuários de patas) específicos.  Para obter mais informações sobre o AppLocker, consulte o artigo da TechNet [visão geral do AppLocker](https://technet.microsoft.com/library/hh831440.aspx).

    -   **O Device Guard**: o novo recurso do Device Guard fornece controle de aplicativo baseada em hardware avançado que, ao contrário do AppLocker, não pode ser substituído no dispositivo afetado.  Como o AppLocker, o Device Guard podem ser controlado por meio da política de grupo e direcionado a usuários específicos.  Para obter mais informações sobre como restringir o uso do aplicativo com o Device Guard, consulte o artigo da TechNet [guia de implantação do Device Guard](https://technet.microsoft.com/en-us/library/mt463091(v=vs.85).aspx).

3.  **Use usuários protegido, políticas de autenticação e Silos de autenticação para proteger ainda mais contas privilegiadas**.  Os membros dos usuários protegidos são sujeito às políticas de segurança adicional que proteger as credenciais armazenados no agente de segurança local (LSA) e consideravelmente minimizam o risco de roubo de credenciais e reutilização.  Políticas de autenticação e silos controlam usuários como privilegiados podem acessar recursos no domínio.  Coletivamente, essas proteções fortalecem consideravelmente a segurança da conta desses usuários privilegiado.  Para obter detalhes adicionais sobre esses recursos, consulte o artigo da web [como configurar contas protegido](https://technet.microsoft.com/en-us/library/dn518179.aspx).

    > [!NOTE]
    > Essas proteções servem para complementar, não substitua, existente medidas de segurança na fase 1.  Os administradores ainda devem usar contas separadas para administração e o uso geral.

### <a name="managing-and-updating-paws"></a>Gerenciar e atualizar patas
Patas devem ter recursos de antimalware e atualizações de software devem ser aplicadas rapidamente para manter a integridade dos seguintes estações de trabalho.

Gerenciamento de configuração adicional, operacionais monitoramento e gerenciamento de segurança também podem ser usados com patas, mas a integração desses deve ser considerada cuidadosamente porque cada recurso de gerenciamento também introduz o risco de comprometimento de PATA por meio dessa ferramenta. Se faz sentido para apresentar recursos avançados de gerenciamento depende de diversos fatores incluindo:

-   O estado de segurança e práticas recomendadas da funcionalidade de gerenciamento (incluindo práticas recomendadas de atualização de software para as funções administrativas, contas e ferramenta nessas funções, sistemas operacionais, a ferramenta está hospedada ou gerenciada a partir do e quaisquer outras dependências de hardware ou software essa ferramenta)

-   A frequência e a quantidade de implantações de software e atualizações em seu patas

-   Requisitos para obter informações detalhadas de inventário e configuração

-   Requisitos de monitoramento de segurança

-   Padrões organizacionais e outros fatores organizacional específica

Pelo princípio de origem limpo, todas as ferramentas usadas para gerenciar ou monitorar as patas devem ser confiáveis em ou acima do nível dos patas. Isso geralmente requer essas ferramentas para ser gerenciados a partir de uma PATA para não garantir que nenhuma dependência de segurança em estações de trabalho do menores privilégio.

Esta tabela descreve diferentes abordagens que podem ser usadas para gerenciar e monitorar as patas:

|Abordagem|Considerações sobre|
|------|---------|
|Padrão em PATA<br /><br />-Windows Server Update Services<br />-O Windows Defender|-Sem custo adicional<br />-Executa funções de segurança básica necessária<br />-Instruções incluídas neste guia|
|Gerenciar com [Intune](https://technet.microsoft.com/en-us/library/jj676587.aspx)|<ul><li>Fornece controle e visibilidade baseado em nuvem<br /><br /><ul><li>Implantação de software</li><li>Atualizações de software de gerenciar o</li><li>Gerenciamento de política de Firewall do Windows</li><li>Proteção de antimalware</li><li>Assistência remota</li><li>Gerenciamento de licenças de software.</li></ul></li><li>Nenhuma infraestrutura de servidor necessária</li><li>Exige as seguintes etapas de "Habilitar conectividade para serviços de nuvem" na fase 2</li><li>Se o computador PATA não tenha ingressado em um domínio, isso requer aplicando as linhas de base SCM para as imagens locais usando as ferramentas fornecidas no download de linha de base de segurança.</li></ul>|
|Novas instâncias do System Center para gerenciar patas|-Fornece a visibilidade e controle de configuração, implantação de software e atualizações de segurança<br />-Requer infraestrutura de servidor separada, protegê-lo ao nível de patas e pessoal habilidades para esses equipe altamente privilegiado|
|Gerenciar patas com ferramentas de gerenciamento existentes|-Cria um risco significativo de comprometer de patas, a menos que a infraestrutura de gerenciamento existente é exibida no nível de segurança de patas **Observação:** Microsoft geralmente seria inibir essa abordagem, a menos que a organização tenha um motivo específico para usá-lo. Em nossa experiência, normalmente há um custo muito alto de trazer todas essas ferramentas (e suas dependências de segurança) até o nível de segurança dos patas.<br />-A maioria destas ferramentas fornece visibilidade e controle de configuração, implantação de software e atualizações de segurança|
|Verificação de segurança ou que precisam de acesso de administrador de ferramentas de monitoramento|Inclui qualquer ferramenta que instala um agente ou requer uma conta com acesso de administrador local.<br /><br />-Requer trazendo a garantia de segurança da ferramenta até o nível de patas.<br />-Podem exigir reduzindo a postura de segurança de patas para dar suporte à funcionalidade de ferramenta (abrir portas, instale o Java ou outros middleware, etc.), criando uma decisão de compromisso de segurança,|
|Informações e o evento de gerenciamento de segurança (SIEM)|<ul><li>Se estiver sem agente SIEM<br /><br /><ul><li>Pode acessar eventos em patas sem acesso administrativo usando uma conta no **leitores de Log de eventos** grupo</li><li>Exigirão a abertura de portas de rede para permitir o tráfego de entrada dos servidores SIEM</li></ul></li><li>Se SIEM requer um agente, consulte outra linha **verificação de segurança ou que precisam de acesso de administrador de ferramentas de monitoramento**.</li></ul>|
|Encaminhamento de eventos do Windows|-Oferece um método sem agentes de encaminhamento de eventos de segurança das patas um coletor externo ou SIEM<br />-Pode acessar eventos em patas sem acesso administrativo<br />-Não requer a abertura de portas de rede para permitir o tráfego de entrada dos servidores SIEM|

#### <a name="operating-paws"></a>Operando patas
A solução PATA deve ser operada usando os padrões em [padrões operacionais](http://aka.ms/securitystandards) baseado no princípio de origem limpa.

## <a name="related-topics"></a>Tópicos relacionados
[Serviços Microsoft Cybersecurity envolvente](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)

[Conheça das principais: atenuar Pass-the-Hash e outras formas de roubo de credenciais](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft Advanced Analytics ameaça](http://aka.ms/ata)

[Proteger credenciais de domínio derivadas com o Credential Guard](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx)

[Visão geral do Device Guard](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)

[Proteger ativos de alto valor com estações de trabalho Administração segura](https://msdn.microsoft.com/en-us/library/mt186538.aspx)

[Modo de usuário isolado no Windows 10 com Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Isolado processos do modo de usuário e recursos no Windows 10 com Logan Gabriel (Channel 9)](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Mais sobre processos e recursos no modo de usuário isolado do Windows 10 com Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Minimizando o roubo de credenciais usando o Windows 10 modo de usuário isolado (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Habilitando a validação KDC rigorosa no Windows Kerberos](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Novidades na autenticação Kerberos para Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Garantia do mecanismo de autenticação para AD DS no guia passo a passo do Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Módulo de plataforma confiável](C:\sd\docs\p_ent_keep_secure\p_ent_keep_secure\trusted_platform_module_technology_overview.xml)
