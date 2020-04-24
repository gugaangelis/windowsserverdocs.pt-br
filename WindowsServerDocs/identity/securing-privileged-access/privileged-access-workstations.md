---
title: Por que Estações de Trabalho com Acesso Privilegiado podem ajudar a proteger sua organização
description: Como uma PAW pode ampliar a postura de segurança de sua organização
ms.prod: windows-server
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
ms.date: 03/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: 36988b8ed3d61fe97f4e1018b4ec1b6340cb4fae
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855139"
---
# <a name="privileged-access-workstations"></a>Estações de trabalho com acesso privilegiado

>Aplica-se a: Windows Server

As PAWs (Privileged Access Workstations, Estações de Trabalho com Acesso Privilegiado) fornecem um sistema operacional dedicado para tarefas confidenciais, que é protegido contra ataques da Internet e vetores de ameaça. A separação dessas tarefas e contas confidenciais das estações de trabalho e dispositivos de uso diário proporciona uma sólida proteção contra ataques de phishing, vulnerabilidades do sistema operacional e de aplicativos, diferentes ataques de representação e ataques de roubo de credenciais, como registro em log do pressionamento de teclas, [Pass-the-Hash](https://aka.ms/pth) e Pass-The-Ticket.

## <a name="what-is-a-privileged-access-workstation"></a>O que é uma Estação de Trabalho com Acesso Privilegiado?

Em termos simples, uma PAW é uma estação de trabalho robusta e bloqueada, projetada para fornecer garantias de alta segurança para contas e tarefas confidenciais.  Recomenda-se as PAWs para a administração de sistemas de identidade, serviços de nuvem, malha de nuvem privada e funções corporativas confidenciais.

> [!NOTE]
> A arquitetura da PAW não exige um mapeamento 1:1 de contas para as estações de trabalho, embora seja uma configuração comum. A PAW cria um ambiente de estação de trabalho confiável que pode ser usado por uma ou mais contas.

Para fornecer a maior segurança possível, as PAWs sempre devem executar o sistema operacional mais atualizado e seguro disponível: A Microsoft recomenda enfaticamente o Windows 10 Enterprise, que inclui vários recursos de segurança adicionais indisponíveis em outras edições (em particular, o [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) e o [Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)).

> [!NOTE]
> As organizações sem acesso ao Windows 10 Enterprise podem usar o Windows 10 Pro, que inclui várias tecnologias de base essenciais para as PAWs, incluindo a Inicialização Confiável, o BitLocker e a Área de Trabalho Remota.  Clientes da área de educação podem usar o Windows 10 Education.  O Windows 10 Home não deve ser usado para uma PAW.
>
> Para uma comparação entre as diferentes edições do Windows 10, leia [este artigo](https://www.microsoft.com/WindowsForBusiness/Compare).

Os controles de segurança da PAW concentram-se em reduzir o alto impacto e os riscos da alta probabilidade de comprometimento. Isso inclui a redução de ataques no ambiente e de riscos que podem reduzir a eficácia dos controles da PAW ao longo do tempo:

* **Ataques da Internet**: a maioria dos ataques são provenientes direta ou indiretamente de fontes na Internet e usa a Internet para transferência de dados e comando e controle (C2). Isolar a PAW da Internet aberta é essencial para garantir que não seja comprometida.
* **Risco de uso**: se for muito difícil usar uma PAW para tarefas diárias, os administradores serão motivados a criar soluções para facilitar seu trabalho. Frequentemente, essas soluções alternativas deixam as contas e a estação de trabalho administrativas vulneráveis a riscos consideráveis de segurança, portanto é fundamental envolver e capacitar os usuários da PAW para atenuar esses problemas de uso com segurança. Isso pode ser feito escutando os comentários, instalando as ferramentas e os scripts necessários para realizar seus trabalhos e garantindo que todo o pessoal administrativo esteja ciente do motivo de precisarem usar uma PAW, do que é uma PAW e de como usá-la corretamente e com sucesso.
* **Riscos de ambiente**: como muitos outros computadores e outras contas do ambiente estão expostos ao risco da Internet, direta ou indiretamente, uma PAW precisa ser protegida contra os ataques de ativos comprometidos no ambiente de produção. Isso exige minimizar o uso de ferramentas de gerenciamento e contas que têm acesso às PAWs a fim de proteger e monitorar essas estações de trabalho especializadas.
* **Adulteração da cadeia de suprimentos**: embora seja impossível remover todos os riscos possíveis de adulteração da cadeia de suprimentos de hardware e software, algumas ações importantes podem mitigar os vetores de ataque críticos que estão prontamente disponíveis para os invasores. Isso inclui a validação da integridade de todas as mídias de instalação ([princípio da origem limpa](https://aka.ms/cleansource)) e o uso de um fornecedor confiável e respeitável de hardware e software.
* **Ataques físicos**: como as PAWs podem ser fisicamente móveis e podem ser usadas fora das instalações físicas seguras, devem ser protegidas contra ataques que utilizam o acesso físico não autorizado ao computador.

> [!NOTE]
> Uma PAW não protegerá um ambiente de um adversário que já obteve acesso administrativo em uma Floresta do Active Directory.
> Como muitas implementações existentes dos Active Directory Domain Services já estão em operação há anos sob o risco de roubo de credenciais, as organizações devem presumir a violação e considerar a possibilidade de que exista um comprometimento despercebido de credenciais do administrador corporativo ou de domínio. Uma organização que suspeita do comprometimento do domínio deve considerar o uso de serviços profissionais de resposta a incidentes.
>
> Para saber mais sobre diretrizes de resposta e recuperação, consulte as seções "Responder a atividades suspeitas" e "Recuperar-se de uma violação" em [Atenuar a Passagem de Hash e outros roubos de credenciais](https://aka.ms/pth), versão 2.
>
> Visite os [serviços de Recuperação e Resposta a incidentes da Microsoft](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx) para saber mais.

### <a name="paw-hardware-profiles"></a>Perfis de hardware da PAW

A equipe administrativa também é formada por usuários padrão, que não precisam apenas de uma PAW, mas também de uma estação de trabalho de usuário padrão para verificar email, navegar na Web e acessar aplicativos corporativos de linha de negócio.  É essencial para o sucesso de qualquer implantação de PAW garantir que os administradores possam permanecer produtivos e seguros.  Uma solução segura que limita drasticamente a produtividade será abandonada pelos usuários em detrimento de uma que aprimora a produtividade (mesmo que isso seja feito de uma maneira insegura).

Para equilibrar a necessidade de segurança com a necessidade de produtividade, a Microsoft recomenda o uso de um destes perfis de hardware de PAW:

* **Hardware dedicado**: separe dispositivos dedicados para tarefas de usuário versus tarefas administrativas.
* **Uso simultâneo**: um único dispositivo que pode executar tarefas administrativas e tarefas do usuário simultaneamente, tirando proveito da virtualização do sistema operacional ou da apresentação.

As organizações podem usar apenas um perfil ou ambos. Não há preocupações quanto à interoperabilidade entre os perfis de hardware, e as organizações têm a flexibilidade de combinar o perfil de hardware com a necessidade específica e a situação de um determinado administrador.

> [!NOTE]
> É fundamental que, em todos esses cenários, a equipe administrativa receba uma conta de usuário padrão separada das contas administrativas designadas. As contas administrativas devem ser usadas apenas no sistema operacional administrativo da PAW.

Esta tabela resume as vantagens e desvantagens relativas de cada perfil de hardware da perspectiva de facilidade de uso operacional e produtividade e segurança.  As duas abordagens de hardware fornecem uma segurança sólida para contas administrativas contra roubo de credenciais e reutilização.

|**Cenário**|**Vantagens**|**Desvantagens**|
|--------|---------|-----------|
|Hardware dedicado|-   Sinal forte para a confidencialidade das tarefas<br />-   Separação de segurança mais sólida|-   Espaço em disco adicional<br />-   Peso adicional (para trabalho remoto)<br />-   Custo de hardware|
|Uso simultâneo|-   Custo de hardware menor<br />-   Experiência de dispositivo único|-   Compartilhamento de teclado/mouse único cria o risco de erros/riscos acidentais|

Este guia contém instruções detalhadas para a configuração da PAW para a abordagem de hardware dedicado. Se houver requisitos de uso simultâneo dos perfis de hardware, você poderá adaptar por conta própria as instruções com base neste guia ou contratar uma organização de serviços profissionais, como a Microsoft, para auxiliar nessa tarefa.

#### <a name="dedicated-hardware"></a>Hardware dedicado

Nesse cenário, uma PAW é usada para a administração, e é completamente separada do PC usado para atividades diárias, como email, edição de documentos e trabalho de desenvolvimento. Todos os aplicativos e ferramentas de administração são instalados na PAW, e todos os aplicativos de produtividade são instalados na estação de trabalho de usuário padrão. As instruções passo a passo deste guia têm base nesse perfil de hardware.

#### <a name="simultaneous-use---adding-a-local-user-vm"></a>Uso simultâneo: adicionar uma VM de usuário local

Neste cenário de uso simultâneo, um único PC é usado para tarefas de administração e atividades diárias, como email, edição de documentos e trabalho de desenvolvimento. Nessa configuração, o sistema operacional do usuário fica disponível enquanto estiver desconectado (para edição de documentos e trabalho em emails armazenados localmente em cache), mas exige processos de suporte e de hardware que podem acomodar esse estado desconectado.

![Diagrama que mostra que, neste cenário de uso simultâneo, um único PC é usado para tarefas de administração e atividades diárias, como email, edição de documentos e trabalho de desenvolvimento](../media/privileged-access-workstations/PAWFig10.JPG)

O hardware físico executa dois sistemas operacionais localmente:

* **Sistema operacional de administração**: o host físico executa o Windows 10 no host da PAW para Tarefas administrativas
* **Sistema operacional do usuário**: uma máquina virtual Hyper-V do cliente do Windows 10 executa uma imagem corporativa

Com o Hyper-V do Windows 10, uma máquina virtual de convidado (que também executa o Windows 10) pode ter uma ótima experiência de usuário, incluindo som, vídeo e aplicativos de comunicação da Internet, como o Skype for Business.

Nessa configuração, o trabalho diário que não exige privilégios administrativos é realizado na máquina virtual de SO do usuário que possui uma imagem corporativa regular do Windows 10 e não está sujeita às restrições aplicadas ao host da PAW. Todo o trabalho administrativo é feito no sistema operacional do administrador.

Para configurar isso, siga as instruções neste guia para o host da PAW, adicione recursos do Hyper-V do cliente, crie uma VM de usuário e instale uma imagem corporativa do Windows 10 na VM do usuário.

Leia o artigo [Cliente Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index) para saber mais sobre esse recurso. Observe que o sistema operacional em máquinas virtuais de convidado precisará ser licenciado por [Licenciamento de produtos da Microsoft](https://www.microsoft.com/Licensing/product-licensing/products.aspx), também descrito [aqui](https://download.microsoft.com/download/9/8/D/98D6A56C-4D79-40F4-8462-DA3ECBA2DC2C/Licensing_Windows_Desktop_OS_for_Virtual_Machines.pdf).

#### <a name="simultaneous-use---adding-remoteapp-rdp-or-a-vdi"></a>Uso simultâneo: adicionar RemoteApp, RDP ou uma VDI

Neste cenário de uso simultâneo, um único PC é usado para tarefas de administração e atividades diárias, como email, edição de documentos e trabalho de desenvolvimento. Nessa configuração, os sistemas operacionais de usuário são implantados e gerenciados centralmente (na nuvem ou em seu datacenter), mas não ficarão disponíveis enquanto estiverem desconectados.

![Figura que mostra que, neste cenário de uso simultâneo, um único PC é usado para tarefas de administração e atividades diárias, como email, edição de documentos e trabalho de desenvolvimento](../media/privileged-access-workstations/PAWFig11.JPG)

O hardware físico executa um único sistema operacional de PAW localmente para tarefas administrativas e entra em contato com um serviço de área de trabalho remota da Microsoft ou de terceiros para aplicativos de usuário, como email, edição de documento e aplicativos de linha de negócios.

Nessa configuração, o trabalho diário que não exige privilégios administrativos é realizado nos SOs e aplicativos remotos, que não estão sujeitos às restrições aplicadas ao host da PAW. Todo o trabalho administrativo é feito no sistema operacional do administrador.

Para configurar isso, siga as instruções neste guia para o host da PAW, permita a conectividade de rede com os serviços de Área de Trabalho Remota e, em seguida, adicione atalhos à área de trabalho do usuário na PAW a fim de acessar os aplicativos. Os serviços de área de trabalho remota podem ser hospedados de várias maneiras, incluindo:

* Um serviço existente de Área de Trabalho Remota ou VDI (local ou na nuvem)
* Um novo serviço instalado localmente ou na nuvem
* RemoteApp do Azure usando modelos pré-configurados do Office 365 ou suas próprias imagens de instalação

Para saber mais sobre o RemoteApp do Azure, visite [esta página](https://www.remoteapp.windowsazure.com).

## <a name="how-microsoft-is-using-administrative-workstations"></a>Como a Microsoft está usando estações de trabalho administrativas

A Microsoft usa a abordagem de arquitetura de PAW internamente, em nossos sistemas, e com nossos clientes. A Microsoft usa as estações de trabalho administrativas internamente para várias atividades, incluindo a administração de infraestrutura de TI da Microsoft, desenvolvimento e operações de infraestrutura de malha de nuvem da Microsoft, além de outros ativos de alto valor.

Este guia é fundamentado diretamente na arquitetura de referência de PAW (Estação de Trabalho com Acesso Privilegiado) implantada por nossas equipes de serviços profissionais de segurança cibernética para proteger os clientes contra ataques de segurança cibernética. As estações de trabalho administrativas também são um elemento essencial da proteção mais forte para tarefas de administração de domínio, a arquitetura de referência de floresta administrativa do ESAE (Ambiente administrativo de segurança avançada).

Para saber mais sobre a floresta administrativa de ESAE, consulte a seção *Abordagem de design de floresta administrativa de ESAE* no [Material de referência sobre proteção de acesso privilegiado](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach).

## <a name="architecture-overview"></a>Visão geral da arquitetura

O diagrama a seguir ilustra um "canal" separado para administração (uma tarefa altamente confidencial), que é criado pela manutenção de contas administrativas e estações de trabalho dedicadas e separadas.

![O diagrama a seguir ilustra um "canal" separado para administração (uma tarefa altamente confidencial) criado pela manutenção de contas administrativas e estações de trabalho dedicadas e separadas](../media/privileged-access-workstations/PAWFig1.JPG)

Essa abordagem de arquitetura amplia as proteções encontradas nos recursos [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) e [Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx) do Windows 10 e vai além dessas proteções para contas e tarefas confidenciais.

Essa metodologia é apropriada para contas que têm acesso a ativos de alto valor:

* **Privilégios administrativos**: as PAWs fornecem mais segurança para funções e tarefas administrativas de alto impacto na TI. Essa arquitetura pode ser aplicada à administração de vários tipos de sistemas, incluindo Domínios e Florestas do Active Directory, locatários do Microsoft Azure Active Directory, locatários do Office 365, PCN (Redes de controle de processo), sistemas SCADA (Aquisição de dados e controle de supervisão), caixas eletrônicos e dispositivos para pontos de venda.
* **Trabalhadores com informações altamente confidenciais**: a abordagem usada em uma PAW também pode fornecer proteção para tarefas de trabalhadores com informações altamente confidenciais, como as que envolvem atividades de fusão e aquisição antes de qualquer anúncio, relatórios financeiros pré-lançamento, presença organizacional em redes sociais, comunicações executivas, segredos comerciais não patenteados, pesquisa confidencial ou outros dados confidenciais ou proprietários. Este guia não discute detalhadamente as configurações desses cenários de trabalhador de informações, nem inclui esse cenário nas instruções técnicas.

    > [!NOTE]
    > A TI da Microsoft usa PAWs (chamadas internamente de "estações de trabalho de administração seguras" ou SAWs) para gerenciar o acesso seguro aos sistemas internos de alto valor dentro da Microsoft. Este guia oferece detalhes adicionais sobre o uso de PAW na Microsoft na seção "Como a Microsoft usa as estações de trabalho de administração". Para saber mais sobre essa abordagem de ambiente com ativos de alto valor, consulte o artigo [Proteger ativos de alto valor com estações de trabalho de administração seguras](https://msdn.microsoft.com/library/mt186538.aspx).

Este documento descreverá o motivo de essa prática ser recomendada para proteger contas privilegiadas de alto impacto, a aparência dessas soluções de PAW para proteção de privilégios administrativos e como implantar rapidamente uma solução de PAW para administração de domínio e serviços de nuvem.

Este documento fornece orientações detalhadas para implementar várias configurações de PAW e inclui instruções detalhadas de implementação para ajudar com a proteção de suas contas de alto impacto comuns:

* [**Fase 1: implantação imediata para Administradores do Active Directory**](#phase-1-immediate-deployment-for-active-directory-administrators) fornece rapidamente uma PAW que pode proteger funções de administração de domínio e floresta locais
* [**Fase 2: estender a PAW a todos os administradores**](#phase-2-extend-paw-to-all-administrators) habilita a proteção para os administradores de serviços de nuvem, como o Office 365 e o Azure, servidores, aplicativos e estações de trabalho corporativos
* [**Fase 3: a segurança avançada da PAW**](#phase-3-extend-and-enhance-protection) aborda proteções e considerações adicionais sobre segurança da PAW

### <a name="why-dedicated-workstations"></a>Por que usar estações de trabalho dedicadas?

O ambiente atual de ameaças para as organizações está repleto de phishing sofisticado e outros ataques da Internet que geram riscos contínuos de comprometimento da segurança para contas e estações de trabalho expostas na Internet.

Esse ambiente de ameaça exige que as organizações adotem uma postura de segurança de "suposição de violação" ao criar proteções para ativos de alto valor, como contas administrativas e ativos corporativos confidenciais. Esses ativos de alto valor precisam ser protegidos contra ameaças diretas da Internet e contra ataques vindos de outras estações de trabalho, servidores e dispositivos no ambiente.

![Figura que mostra o risco aos ativos gerenciados se um invasor assumir o controle de uma estação de trabalho de usuário na qual credenciais confidenciais são usadas](../media/privileged-access-workstations/PAWFig2.JPG)

Essa figura retrata o risco aos ativos gerenciados se um invasor assumir o controle de uma estação de trabalho do usuário na qual credenciais confidenciais são usadas.

Um invasor no controle de um sistema operacional conta com várias maneiras de obter acesso ilícito a todas as atividades na estação de trabalho e de representar a conta legítima. Diversas técnicas de ataques conhecidas e desconhecidas podem ser usadas para obter esse nível de acesso. O volume e a sofisticação cada vez maiores dos ciberataques exigiram a extensão desse conceito de separação a fim de desligar completamente os sistemas operacionais de clientes das contas confidenciais. Para saber mais sobre esses tipos de ataques, visite o [Site de passagem de hash](https://www.microsoft.com/pth) e consulte os white papers e vídeos informativos, além de mais referências.

A abordagem PAW é uma extensão da prática recomendada bem estabelecida de uso de contas de usuário e de administrador separadas para a equipe administrativa. Esta prática usa uma conta administrativa atribuída individualmente e completamente separada da conta padrão do usuário. A PAW amplia essa prática de separação de conta, fornecendo uma estação de trabalho confiável para essas contas confidenciais.

Esta orientação sobre PAW tem como objetivo ajudá-lo a implementar essa funcionalidade a fim de proteger contas de alto valor, como administradores de TI altamente privilegiados e contas corporativas de alta confidencialidade. O guia ajudará você a:

* Restringir a exposição de credenciais somente aos hosts confiáveis
* Fornecer uma estação de trabalho altamente segura a administradores, para que eles possam executar com facilidade as tarefas administrativas.

Restringir as contas confidenciais apenas às PAWs seguras é uma proteção bem simples para essas contas, pois elas são altamente úteis para os administradores e dificultam muito a vitória de um adversário.

### <a name="alternate-approaches"></a>Abordagens alternativas

Esta seção compara a segurança das abordagens alternativas às PAWs e contém informações sobre como integrar corretamente essas abordagens dentro de uma arquitetura de PAW. Todas essas abordagens apresentam riscos consideráveis quando implementadas sozinhas, mas podem agregar valor à implementação da PAW em alguns cenários.

#### <a name="credential-guard-and-windows-hello-for-business"></a>Credential Guard e Windows Hello para Empresas

Apresentado no Windows 10, o [Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx) usa segurança baseada em virtualização e em hardware para atenuar ataques comuns de roubo de credenciais, como Passagem de Hash, protegendo as credenciais derivadas. A chave privada para as credenciais usadas pelo [Windows Hello for Business](https://aka.ms/passport) também pode ser protegida pelo hardware do TPM (Trusted Platform Module).

São atenuações eficientes, mas as estações de trabalho ainda poderão ficar vulneráveis a determinados ataques, mesmo se as credenciais forem protegidas pelo Credential Guard ou pelo Windows Hello for Business. Os ataques podem incluir abuso de privilégios e uso de credenciais diretamente de um dispositivo comprometido, reutilização de credenciais roubadas anteriormente antes da habilitação do Credential Guard e abuso de ferramentas de gerenciamento e configurações de aplicativos fracos na estação de trabalho.

A orientação sobre PAW nesta seção inclui o uso de várias dessas tecnologias para contas e tarefas de alta confidencialidade.

#### <a name="administrative-vm"></a>VM administrativa

Uma máquina virtual administrativa (VM de Admin) é um sistema operacional dedicado para tarefas administrativas hospedado em um desktop de usuário padrão. Embora essa abordagem seja semelhante à PAW no fornecimento de um sistema operacional dedicado para tarefas administrativas, tem uma falha fatal: a VM administrativa depende do desktop de usuário padrão para sua segurança.

O diagrama a seguir ilustra a capacidade de os invasores seguirem a cadeia de controle até o objeto de destino de interesse com uma VM de administrador em uma Estação de trabalho de usuário, e isso dificulta a criação de um caminho na configuração inversa.

A arquitetura de PAW não permite a hospedagem de uma VM Admin em uma Estação de Trabalho do Usuário, mas uma VM de Usuário com uma imagem corporativa padrão pode ser hospedada em uma PAW Admin a fim de fornecer à equipe um único PC para todas as responsabilidades.

![Diagrama da arquitetura de PAW](../media/privileged-access-workstations/PAWFig9.JPG)

#### <a name="shielded-vm-based-paws"></a>PAWs baseadas em VM blindada

Uma variação segura do modelo de VM administrativa é o uso de [máquinas virtuais blindadas](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) para hospedar uma ou mais VMs de administrador com uma VM de usuário.
As VMs blindadas são projetadas para executar cargas de trabalho seguras em um ambiente onde usuários ou código potencialmente não confiáveis podem estar na área de trabalho do usuário padrão do computador físico.
Uma VM blindada tem um TPM virtual que permite criptografar seus próprios dados em repouso e vários controles administrativos, como o acesso de console básico, o PowerShell Direct e a capacidade de depurar a VM, são desabilitados para isolar ainda mais a VM da área de trabalho do usuário padrão e de outras VMs.
As chaves de uma VM blindada são armazenadas em um servidor de gerenciamento de chaves confiável, o que exige que o dispositivo físico ateste sua identidade e integridade antes de liberar uma chave para iniciar a VM.
Isso garante que as VMs blindadas só possam iniciar nos dispositivos pretendidos e que esses dispositivos estejam executando configurações de software conhecidas e confiáveis.

Como as VMs blindadas são isoladas umas das outras e da área de trabalho do usuário padrão, é aceitável executar várias VMs com PAW blindadas em um único host, mesmo quando essas VMs gerenciam diferentes camadas.

Consulte a seção [Implantar PAWs usando uma Malha Protegida](#deploy-paws-using-a-guarded-fabric) abaixo para obter mais informações.

#### <a name="jump-server"></a>Servidor de salto

Arquiteturas administrativas de "Servidor de salto" definem um pequeno número de servidores de console administrativos e restringem seu uso para tarefas administrativas. Isso normalmente tem base em serviços de área de trabalho remota, em uma solução de virtualização de apresentação de terceiros ou em uma tecnologia de VDI (Virtual Desktop Infrastructure).

Essa abordagem é proposta com frequência para reduzir os riscos à administração e fornecer algumas garantias de segurança, mas a abordagem de servidor de salto por si só é vulnerável a certos ataques, pois viola o [princípio de "origem limpa"](../securing-privileged-access/securing-privileged-access-reference-material.md#clean-source-principle). O princípio de origem limpa exige que todas as dependências de segurança sejam tão confiáveis quanto o objeto que está sendo protegido.

![Figura que mostra uma relação de controle simples](../media/privileged-access-workstations/PAWFig3.JPG)

Esta figura mostra uma relação de controle simples. Qualquer pessoa que tem controle de um objeto é uma dependência de segurança desse objeto. Se um adversário puder controlar uma dependência de segurança de um objeto alvo (entidade), ele poderá controlar esse objeto.

A sessão administrativa no servidor de salto depende da integridade do computador local que a acessa. Se esse computador for uma estação de trabalho de usuário sujeita a ataques de phishing e outros vetores de ataque baseados na Internet, a sessão administrativa também estará sujeita a esses riscos.

![Figura que mostra como os invasores podem seguir uma cadeia de controle estabelecida até o objeto de destino de interesse](../media/privileged-access-workstations/PAWFig4.JPG)

A figura acima mostra como os invasores podem seguir uma cadeia de controle estabelecida até o objeto de destino de interesse.

Embora alguns controles avançados de segurança, como a autenticação multifator, possam aumentar a dificuldade de um invasor assumir o controle dessa sessão administrativa a partir da estação de trabalho de usuário, nenhum recurso de segurança pode proteger totalmente contra ataques técnicos quando um invasor tiver acesso administrativo do computador de origem (por exemplo, injetando comandos ilícitos em uma sessão legítima, sequestrando processos legítimos, etc.)

A configuração padrão neste guia sobre PAW instala ferramentas administrativas na PAW, mas também é possível adicionar uma arquitetura de servidor de salto, se for necessário.

![Figura que mostra como a inversão da relação de controle e o acesso a aplicativos de usuário em uma estação de trabalho administrativa não proporcionam ao invasor um caminho até o objeto de destino](../media/privileged-access-workstations/PAWFig5.JPG)

Essa figura mostra como a inversão da relação de controle e o acesso a aplicativos de usuário em uma estação de trabalho administrativa não proporcionam ao invasor um caminho até o objeto de destino. O servidor de salto do usuário ainda está exposto a riscos, portanto os controles de proteção adequados, os controles de investigação e os processos de resposta ainda devem ser aplicados nesse computador voltado para a Internet.

Essa configuração exige que os administradores sigam de perto as práticas operacionais a fim de assegurar que não insiram acidentalmente credenciais de administrador na sessão de usuário em seus desktops.

![Figura que mostra como o acesso a um servidor de salto administrativo de uma PAW não adiciona um caminho do invasor até os ativos administrativos](../media/privileged-access-workstations/PAWFig6.JPG)

Essa figura mostra como o acesso a um servidor de salto administrativo a partir de uma PAW não adiciona um caminho até os recursos administrativos para o invasor. Um servidor de salto com uma PAW permite, neste caso, a consolidação do número de locais para monitoramento da atividade administrativa e a distribuição de ferramentas e aplicativos administrativos. Isso adiciona certa complexidade ao design, mas pode simplificar o monitoramento de segurança e as atualizações de software se uma grande quantidade de contas e estações de trabalho forem usadas em sua implementação da PAW. Seria necessário criar e configurar o servidor de salto com padrões de segurança semelhantes aos da PAW.

#### <a name="privilege-management-solutions"></a>Soluções de gerenciamento de privilégios

As soluções de Gerenciamento de privilégios são aplicativos que fornecem acesso temporário a privilégios discretos ou a contas privilegiadas sob demanda. As soluções de Gerenciamento de privilégios são um componente extremamente valioso em uma estratégia completa de proteção do acesso privilegiado, e fornecem visibilidade e responsabilidade fundamentais das atividades administrativas.

Essas soluções normalmente usam um fluxo de trabalho flexível para conceder acesso, e muitas têm recursos de segurança adicionais, como gerenciamento de senhas de contas de serviço e integração com servidores de salto administrativos. Há muitas soluções no mercado que oferecem recursos de gerenciamento de privilégios. Uma delas é o PAM (gerenciamento de acesso privilegiado) do MIM (Microsoft Identity Manager).

A Microsoft recomenda o uso de uma PAW para acessar soluções de gerenciamento de privilégios. O acesso a essas soluções deve ser concedido somente às PAWs. A Microsoft não recomenda o uso dessas soluções como substituição para uma PAW, pois o acesso de privilégios usando essas soluções em um desktop de usuário potencialmente comprometido viola o princípio da [origem limpa](https://aka.ms/cleansource), conforme ilustrado no diagrama a seguir:

![Diagrama que mostra que a Microsoft não recomenda o uso dessas soluções como substituição para uma PAW, pois o acesso de privilégios usando essas soluções em um desktop de usuário potencialmente comprometido viola o princípio da origem limpa](../media/privileged-access-workstations/PAWFig7.JPG)

O fornecimento de uma PAW para acessar essas soluções permite que você obtenha os benefícios de segurança da PAW e da solução de gerenciamento de privilégios, conforme ilustrado neste diagrama:

![Diagrama que mostra que o fornecimento de uma PAW para acessar essas soluções permite que você obtenha os benefícios de segurança da PAW e da solução de gerenciamento de privilégios](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> Esses sistemas devem ser classificados com a camada mais alta de privilégio que eles gerenciam, e devem ser protegidos nesse nível de segurança ou acima dele. Eles são configurados normalmente para gerenciar soluções e ativos de Camada 0, e devem ser classificados na Camada 0.
> Para saber mais sobre o modelo de camadas, consulte [https://aka.ms/tiermodel](https://aka.ms/tiermodel). Para saber mais sobre grupos no Nível 0, consulte a equivalência de Nível 0 no [Material de referência da proteção de acesso privilegiado](../securing-privileged-access/securing-privileged-access-reference-material.md).

Para saber mais sobre como implantar o PAM (Privileged Access Management) do MIM (Microsoft Identity Manager), confira [https://aka.ms/mimpamdeploy](https://aka.ms/mimpamdeploy)

## <a name="paw-scenarios"></a>Cenários de PAW

Esta seção contém orientações sobre os cenários nos quais este guia de PAW deve ser aplicado. Em todos os cenários, os administradores devem ser treinados para usar as PAWs apenas para executar o suporte de sistemas remotos. Para incentivar o uso seguro e bem-sucedido, todos os usuários de PAW também devem ser incentivados a fornecer comentários a fim de melhorar a experiência da PAW, e esses comentários devem ser examinados com atenção para proporcionar integração com seu programa de PAW.

Em todos os cenários, talvez seja necessário ampliar a proteção em fases mais avançadas e usar perfis de hardware diferentes para atender aos requisitos de segurança ou uso das funções.

> [!NOTE]
> Esta orientação diferencia explicitamente a necessidade entre o acesso a serviços específicos na Internet (como portais administrativos do Azure e do Office 365) e à "Internet aberta" de todos os hosts e serviços.

Consulte a [página de Modelo de camadas](https://aka.ms/tiermodel) para saber mais sobre as designações de camadas.

|**Cenários**|**Usar a PAW?**|**Considerações de escopo e segurança**|
|---------|--------|---------------------|
|Administradores do Active Directory: Camada 0|Sim|Uma PAW criada com orientação de Fase 1 é suficiente para esta função.<p>-   É possível adicionar uma floresta administrativa para fornecer a proteção mais forte para este cenário. Para saber mais sobre a floresta administrativa de ESAE, consulte [Abordagem de design de floresta administrativa de ESAE](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach)<br />- Uma PAW pode ser usada para administrar vários domínios ou florestas.<br />-  Se os Controladores de domínio estiverem hospedados em uma IaaS (Infraestrutura como serviço) ou em uma solução de virtualização local, você deverá priorizar a implementação das PAWs para os administradores dessas soluções.|
|Serviços de administração de IaaS e PaaS do Azure: Camada 1 ou Camada 0 (consulte Considerações de escopo e design)|Sim|Uma PAW criada usando as diretrizes fornecidas na Fase 2 é suficiente para essa função.<p>-   As PAWs devem ser usadas pelo menos para o Administrador global e o Administrador de cobrança da assinatura. Você também deve usar PAWs para administradores delegados de servidores críticos ou confidenciais.<br />-   As PAWs devem ser usadas para gerenciar o sistema operacional e aplicativos que fornecem Sincronização de Diretório e Federação de Identidade para serviços de nuvem, como [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect/) e Serviços de Federação do Active Directory (AD FS).<br />-   As restrições para a rede de saída devem permitir a conectividade apenas a serviços de nuvem autorizados usando as diretrizes da Fase 2. Não deve haver acesso à Internet aberta a partir das PAWs.<br />- O Windows Defender Exploit Guard deve ser configurado na estação de trabalho **Observação:**     uma assinatura será considerada de Camada 0 para uma Floresta se os Controladores de domínio ou outros hosts de Camada 0 estiverem na assinatura. Uma assinatura será de Camada 1 se não houver servidores de Camada 0 hospedados no Azure.|
|Administrador do locatário do Office 365 <br />- Camada 1|Sim|Uma PAW criada usando as diretrizes fornecidas na Fase 2 é suficiente para essa função.<p>-   As PAWs devem ser usadas pelo menos para o Administrador de cobrança da assinatura, o Administrador global, o Administrador do Exchange, o Administrador do SharePoint e funções de Administrador de gerenciamento de usuário. Considere também o uso de PAWs para os administradores delegados de dados altamente críticos ou confidenciais.<br />- O Windows Defender Exploit Guard deve ser configurado na estação de trabalho.<br />-   As restrições para a rede de saída devem permitir a conectividade apenas a serviços da Microsoft usando as diretrizes da Fase 2. Não deve haver acesso à Internet aberta a partir das PAWs.|
|Outros administradores de serviço de nuvem IaaS ou PaaS<br />- Camada 0 ou Camada1 (consulte Considerações de escopo e design)|Sim|Uma PAW criada usando as diretrizes fornecidas na Fase 2 é suficiente para essa função.<p>-   As PAWs devem ser usadas para qualquer função que tenha direitos administrativos sobre VMs hospedadas na nuvem, incluindo a capacidade para instalar agentes, exportar arquivos de disco rígido ou acessar o armazenamento no qual os discos rígidos com sistemas operacionais, dados confidenciais ou dados comerciais críticos são armazenados.<br />-   As restrições para a rede de saída devem permitir a conectividade apenas a serviços da Microsoft usando as diretrizes da Fase 2. Não deve haver acesso à Internet aberta a partir das PAWs.<br />- O Windows Defender Exploit Guard deve ser configurado na estação de trabalho. **Observação**: uma assinatura será de Camada 0 para uma Floresta se os Controladores de domínio ou outros hosts de Camada 0 estiverem na assinatura. Uma assinatura será de Camada 1 se não houver servidores de Camada 0 hospedados no Azure.|
|Administradores de virtualização<br />- Camada 0 ou Camada1 (consulte Considerações de escopo e design)|Sim|Uma PAW criada usando as diretrizes fornecidas na Fase 2 é suficiente para essa função.<p>-   As PAWs devem ser usadas para qualquer função que tenha direitos administrativos sobre VMs, incluindo a capacidade para instalar agentes, exportar arquivos de disco rígido virtual ou acessar o armazenamento no qual os discos rígidos com informações de sistemas operacionais convidados, dados confidenciais ou dados comerciais críticos são armazenados. **Observação**: um sistema de virtualização (e seus administradores) são considerados de Camada 0 para uma Floresta se os Controladores de domínio ou outros hosts de Camada 0 estiverem na assinatura. Uma assinatura será de Camada 1 se não houver servidores de Camada 0 hospedados no sistema de virtualização.|
|Administradores de manutenção do servidor<br />- Camada 1|Sim|Uma PAW criada usando as diretrizes fornecidas na Fase 2 é suficiente para essa função.<p>-   Uma PAW deverá ser usada para os administradores que atualizam, aplicam patches e solucionam problemas de aplicativos e servidores corporativos executando o servidor Windows, Linux e outros sistemas operacionais.<br />-   Talvez seja necessário adicionar ferramentas de gerenciamento dedicadas às PAWs a fim de lidar com a escala maior desses administradores.|
|Administradores de estação de trabalho do usuário <br />- Camada 2|Sim|Uma PAW criada usando as orientações fornecidas na Fase 2 é suficiente para as funções que têm direitos administrativos em dispositivos de usuário final (como funções de suporte técnico e deskside).<p>-   Talvez seja necessário instalar aplicativos adicionais em PAWs a fim de habilitar o gerenciamento de tíquetes e outras funções de suporte.<br />- O Windows Defender Exploit Guard deve ser configurado na estação de trabalho.<br />    Talvez seja necessário adicionar ferramentas de gerenciamento dedicadas às PAWs a fim de lidar com a escala maior desses administradores.|
|Administradores de SQL, SharePoint ou LOB (linha de negócios)<br />- Camada 1|Sim|Uma PAW criada com as orientações da Fase 2 é suficiente para esta função.<p>-   Talvez seja necessário instalar ferramentas de gerenciamento adicionais nas PAWs a fim de permitir que os administradores gerenciem aplicativos sem precisarem se conectar a servidores usando a Área de Trabalho Remota.|
|Usuários que gerenciam presença em redes sociais|Parcialmente|Uma PAW criada usando as diretrizes fornecidas na Fase 2 pode ser usada como ponto de partida para fornecer segurança a essas funções.<p>-   Proteger e gerenciar contas de redes sociais usando o Azure Active Directory (AAD) para compartilhamento, proteção e controle de acesso a contas de redes sociais.<br />    Para saber mais sobre esse recurso, leia [esta postagem de blog](https://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx).<br />-   As restrições para a rede de saída devem permitir a conectividade com esses serviços. Isso pode ser feito permitindo que conexões com a Internet aberta (risco de segurança muito maior que elimina muitas garantias da PAW) ou permitindo somente endereços DNS necessários para o serviço (pode ser difícil de obter).|
|Usuários padrão|Não|Embora muitas etapas de proteção possam ser usadas para usuários padrão, a PAW foi criada para isolar contas do acesso à Internet aberta que a maioria dos usuários precisa para funções de trabalho.|
|VDI/Quiosque de convidado|Não|Embora muitas etapas de proteção possam ser usadas para um sistema de quiosque para convidados, a arquitetura de PAW foi projetada para fornecer maior segurança para contas de alta confidencialidade, e não uma segurança maior para contas de confidencialidade inferior.|
|Usuário VIP (executivos, pesquisadores, etc.)|Parcialmente|Uma PAW criada usando as diretrizes fornecidas na Fase 2 pode ser usada como ponto de partida para fornecer segurança a essas funções.<p>-   Esse cenário é semelhante a uma área de trabalho de usuário padrão, mas normalmente tem um perfil de aplicativo menor, mais simples e bem conhecido. Normalmente, esse cenário exige o descobrimento e a proteção de dados, serviços e aplicativos confidenciais (que podem ou não ser instalados em desktops).<br />-   Essas funções geralmente exigem um alto grau de segurança e um grau muito alto de facilidade de uso, o que exige alterações de design a fim de atender às preferências do usuário.|
|Sistemas de controle industriais (por exemplo, SCADA, PCN e DCS)|Parcialmente|Uma PAW criada usando a orientação fornecida na Fase 2 pode ser usada como ponto de partida para fornecer segurança para essas funções, pois a maioria dos consoles ICS (incluindo padrões comuns como SCADA e PCN) não exige a navegação na Internet aberta e a verificação de emails.<p>-  Aplicativos usados para controlar máquinas físicas precisariam ser integrados e testados quanto à compatibilidade, além de protegidos adequadamente.|
|Sistema operacional incorporado|Não|Apesar de muitas etapas de proteção da PAW poderem ser usadas para sistemas operacionais incorporados, uma solução personalizada precisaria ser desenvolvida para a proteção nesse cenário.|

> [!NOTE]
> **Cenários de combinação** Alguns funcionários podem ter responsabilidades administrativas que abrangem vários cenários.
> Nesses casos, o principal a lembrar é que as regras de modelo de Camada sempre devem ser seguidas. Consulte a página Modelo de camadas para saber mais.

> [!NOTE]
> **Dimensionamento do programa de PAW** À medida que seu programa de PAW aumenta para abranger mais administradores e funções, você precisa continuar assegurando a aderência aos padrões de segurança e uso. Isso pode exigir a atualização das estruturas de seu suporte de TI ou a criação de novas estruturas para resolver desafios específicos da PAW, como o processo de integração, gerenciamento de incidentes, gerenciamento de configuração e coleta de comentários para resolver desafios de uso.  Por exemplo, sua organização decide permitir cenários de home office para administradores, o que exigiria uma mudança de PAWs de desktop para PAWs de laptop, uma mudança que pode precisar de considerações de segurança adicionais.  Outro exemplo comum é a criação ou atualização do treinamento para novos administradores, treinamento que agora deve incluir o conteúdo sobre o uso apropriado de uma PAW (incluindo porque uma PAW é importante e sua definição).  Para obter mais considerações que devem ser abordadas ao dimensionar seu programa de PAW, consulte a Fase 2 das instruções.

Este guia contém instruções detalhadas para a configuração da PAW para os cenários indicados acima. Se houver requisitos para os outros cenários, você poderá adaptar por conta própria as instruções com base neste guia ou contratar uma organização de serviços profissionais, como a Microsoft, para auxiliar nessa tarefa.

Para saber mais sobre como usar os serviços da Microsoft para projetar uma PAW personalizada para o seu ambiente, entre em contato com seu representante da Microsoft ou visite [esta página](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

## <a name="paw-phased-implementation"></a>Implementação da PAW em fases

Como a PAW deve fornecer uma fonte confiável e segura para administração, é essencial que o processo de compilação seja seguro e confiável.  Esta seção fornecerá instruções detalhadas que permitirão a você criar suas próprias PAWs usando princípios e conceitos gerais muito semelhantes aos usados pela TI da Microsoft e pela engenharia de nuvem da Microsoft e organizações de gerenciamento de serviço.

As instruções são divididas em três fases que se concentram em adotar rapidamente as atenuações mais importantes e, em seguida, aumentar e expandir progressivamente o uso da PAW na empresa.

* [Fase 1: implantação imediata para administradores do Active Directory](#phase-1-immediate-deployment-for-active-directory-administrators)
* [Fase 2: extensão da PAW para todos os administradores](#phase-2-extend-paw-to-all-administrators)
* [Fase 3: segurança avançada da PAW](#phase-3-extend-and-enhance-protection)

É importante observar que as fases sempre devem ser realizadas em ordem, mesmo se forem planejadas e implementadas como parte do mesmo projeto geral.

### <a name="phase-1-immediate-deployment-for-active-directory-administrators"></a>Fase 1: implantação imediata para administradores do Active Directory

Finalidade: fornecer rapidamente uma PAW que pode proteger funções de administração de domínio e floresta local.

Escopo: administradores de Camada 0, incluindo administradores de empresa, de domínio (para todos os domínios) e administradores de outros sistemas de identidade autoritativos.

A Fase 1 concentra-se nos administradores que gerenciam seu domínio do Active Directory local, que são funções extremamente importantes e frequentemente alvo de invasores. Esses sistemas de identidade funcionarão com eficiência para proteger esses administradores se os seus Controladores de Domínios do Active Directory estiverem hospedados em datacenters locais, em IaaS (Infraestrutura como serviço) do Azure ou outro provedor de IaaS.

Durante essa fase, você criará a estrutura administrativa e protegida de unidade organizacional do Active Directory para hospedar sua PAW (Estação de Trabalho com Acesso Privilegiado), bem como implantar as próprias PAWs.  Essa estrutura também inclui as políticas de grupo e grupos necessários para dar suporte à PAW.  Você criará a maioria da estrutura usando scripts do PowerShell que estão disponíveis na [Galeria do TechNet](https://aka.ms/pawmedia).

Os scripts criarão as seguintes unidades organizacionais e grupos de segurança:

* Unidades organizacionais (UO)
   * Seis novas UOs de nível superior:  Administrador, Grupos, Servidores de Camada 1, Estações de Trabalho, Contas de Usuário e Quarentena do Computador.  Cada UO de nível superior conterá várias UOs filhas.
* Grupos
   * Seis novos grupos globais habilitados para segurança:  Manutenção de Replicação de Camada 0, Manutenção de Servidor de Camada 1, Operadores de Suporte Técnico, Manutenção de Estação de Trabalho, Usuários da PAW e Manutenção da PAW.

Você também criará vários objetos de política de grupo: Configuração da PAW – Computador; Configuração da PAW – Usuário; RestrictedAdmin Obrigatório – Computador; Restrições de Saída da PAW; Restringir Logon da Estação de Trabalho; Restringir Logon do Servidor.

A Fase 1 inclui as seguintes etapas:

#### <a name="complete-the-prerequisites"></a>Completar os pré-requisitos

1. **Certifique-se de que todos os administradores usem contas individuais e separadas para atividades de administração e de usuário final** (incluindo email, navegação na Internet, aplicativos de linha de negócios e outras atividades não administrativas).  Atribuir uma conta administrativa para cada funcionário autorizado separada de sua conta de usuário padrão é fundamental para o modelo da PAW, pois somente determinadas contas terão permissão para fazer logon na PAW.

   > [!NOTE]
   > Cada administrador deve usar sua própria conta para administração.  Não compartilhe uma conta administrativa.

2. **Minimize o número de administradores privilegiados de Camada 0**.  Como cada administrador deve usar uma PAW, reduzir o número de administradores reduz o número de PAWs necessárias para dar suporte a eles e os custos associados. A contagem inferior de administradores também resulta em uma exposição menor desses privilégios e riscos associados. Embora seja possível para administradores em um local compartilharem uma PAW, os administradores em locais físicos separados exigirão PAWs separadas.
3. **Adquira hardware de um fornecedor confiável que atenda a todos os requisitos técnicos**. A Microsoft recomenda a aquisição de hardware que atende aos requisitos técnicos no artigo [Proteger as credenciais de domínio com o Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx).

   > [!NOTE]
   > A PAW instalada no hardware sem esses recursos pode fornecer proteções significativas, mas os recursos de segurança avançados, como o Credential Guard e o Device Guard não estarão disponíveis.  O Credential Guard e o Device Guard não são necessários para a implantação da Fase 1, mas são altamente recomendados como parte da Fase 3 (proteção avançada).
   >
   > Certifique-se de que o hardware usado para a PAW tenha origem de um fabricante e fornecedor cujas práticas de segurança sejam confiáveis para a organização. Essa é uma aplicação do princípio de origem limpa com o objetivo de fornecer segurança para a cadeia de suprimentos.
   >
   > Para saber mais sobre a importância da segurança na cadeia de suprimentos, visite [este site](https://www.microsoft.com/security/cybersecurity/).

4. **Obtenha e valide o Windows 10 Enterprise Edition e o software de aplicativo exigidos**. Obtenha o software necessário para a PAW e valide-o usando a orientação em [Origem limpa para mídia de instalação](https://aka.ms/cleansource).

   * Windows 10 Enterprise Edition
   * [Ferramentas de Administração de Servidor Remoto](https://www.microsoft.com/download/details.aspx?id=45520) para Windows 10
   * [Linhas de base de segurança do Windows 10](https://aka.ms/win10baselines)

      > [!NOTE]
      > A Microsoft publica os hashes MD5 para todos os sistemas operacionais e aplicativos no MSDN, mas nem todos os fornecedores de software fornecem uma documentação semelhante.  Nesses casos, é necessário recorrer a outras estratégias.  Para saber mais sobre a validação de software, consulte [Origem limpa](https://aka.ms/cleansource) para mídia de instalação.

5. **Certifique-se de que o servidor do WSUS esteja disponível na Intranet**. Você precisará de um servidor do WSUS na Intranet para baixar e instalar atualizações para PAW. Este servidor do WSUS deve ser configurado para aprovar automaticamente todas as atualizações de segurança para Windows 10, ou a equipe administrativa deverá ter responsabilidade para aprovar rapidamente as atualizações de software.

   > [!NOTE]
   > Para saber mais, veja a seção “Aprovar automaticamente atualizações para instalação” em [Guia sobre aprovação de atualizações](https://technet.microsoft.com/library/cc708458(v=ws.10).aspx).

#### <a name="deploy-the-admin-ou-framework-to-host-the-paws"></a>Implantar a estrutura de UO de administração para hospedar as PAWs

1. Baixe a biblioteca de scripts de PAW na [Galeria do TechNet](https://aka.ms/PAWmedia)

   > [!NOTE]
   > Baixe todos os arquivos e salve-os no mesmo diretório, depois execute-os na ordem especificada abaixo.  Create-PAWGroups depende da estrutura de UO criada por Create-PAWOUs, e Set-PAWOUDelegation depende dos grupos criados por Create-PAWGroups.
   > Não modifique os scripts ou o arquivo de valores separados por vírgulas (CSV).

2. **Execute o script Create-PAWOUs.ps1**.  Esse script criará a nova estrutura da unidade organizacional (UO) no Active Directory e bloqueará a herança de GPO em novas UOs, conforme apropriado.
3. **Execute o script Create-PAWGroups.ps1**.  Esse script criará novos grupos de segurança globais nas UOs apropriadas.

   > [!NOTE]
   > Embora esse script crie novos grupos de segurança, ele não os preencherá automaticamente.

4. **Execute o script Set-PAWOUDelegation.ps1**.  Esse script atribuirá permissões às novas UOs para os grupos apropriados.

#### <a name="move-tier-0-accounts-to-the-admintier-0accounts-ou"></a>Mova as contas da Camada 0 para a UO Admin\Camada 0\Contas

Mova cada conta que seja membro do grupo Administrador de Domínio, Administrador de Empresa ou de grupos equivalentes à Camada 0 (incluindo associação aninhada) para esta UO. Se sua organização tiver seus próprios grupos que foram adicionados a esses grupos, você deverá movê-los para a UO de Admin\Camada 0\Grupos.

   > [!NOTE]
   > Para saber mais sobre quais grupos são de Camada 0, consulte "Equivalência à Camada 0" em [Proteção do material de referência de acesso privilegiado](../securing-privileged-access/securing-privileged-access-reference-material.md).

#### <a name="add-the-appropriate-members-to-the-relevant-groups"></a>Adicionar os membros apropriados aos grupos relevantes

1. **Usuários da PAW**: adicione administradores de Camada 0 com os grupos Administradores de Domínio ou de Empresa que você identificou na Etapa 1 da Fase 1.
2. **Manutenção de PAW**: adicione pelo menos uma conta a ser usada para manutenção da PAW e tarefas de solução de problemas. As Contas de Manutenção de PAW raramente serão usadas.

   > [!NOTE]
   > Não adicione a mesma conta de usuário ou grupo a Usuários da PAW e Manutenção da PAW.  O modelo de segurança da PAW tem base parcialmente na suposição de que a conta de usuário da PAW tem direitos privilegiados em sistemas gerenciados ou sobre a própria PAW, mas não em ambos.
   >
   > * Isso é importante para a criação de boas práticas e hábitos administrativos na Fase 1.
   > * Isso é extremamente importante para a Fase 2 e fases seguintes para evitar o escalonamento de privilégios por meio da PAW, pois as PAWs envolvem Camadas.
   >
   > De forma ideal, nenhuma equipe recebe tarefas em várias camadas a fim de aplicar o princípio de segregação de tarefas, mas a Microsoft reconhece que muitas organizações podem ter uma equipe limitada (ou outros requisitos organizacionais) que não permitem essa segregação completa. Nesses casos, a mesma equipe pode receber ambas as funções, mas não deve usar a mesma conta para essas funções.

#### <a name="create-paw-configuration---computer-group-policy-object-gpo"></a>Criar GPO (objeto de política de grupo) "Configuração da PAW – Computador"

Nesta seção, você criará um GPO "Configuração da PAW – Computador" que fornece proteções específicas para essas PAWs e o vinculará à UO de Dispositivos de Camada 0 ("Dispositivos" em Camada 0\Admin).

   > [!NOTE]
   > **Não adicione essas configurações à Política de Domínio Padrão**.  Isso potencialmente afetará as operações em todo o seu ambiente do Active Directory.  Apenas defina essas configurações nos GPOs recém-criados descritos aqui e aplique-as apenas à UO da PAW.

1. **Acesso de Manutenção da PAW**: essa configuração definirá a associação de grupos privilegiados específicos nas PAWs a um conjunto específico de usuários. Acesse *Configuração do Computador\Preferências\Configurações de Painel de Controle\Usuários e Grupos Locais* e execute as etapas abaixo:
   1. Clique em **Novo** e em **Grupo Local**
   2. Selecione a ação **Atualizar** e selecione "Administradores (interno)" (não use o botão Procurar para selecionar o grupo Administradores do domínio).
   3. Marque as caixas de seleção **Excluir todos os usuários membros** e **Excluir todos os grupos membros**
   4. Adicione Manutenção da PAW (pawmaint) e Administrador (novamente, não use o botão Procurar para selecionar o administrador).

      > [!NOTE]
      > Não adicione o grupo Usuários da PAW à lista de membros para o grupo local de Administradores.  Para garantir que os Usuários da PAW não possam, acidental ou deliberadamente, modificar configurações de segurança da própria PAW, eles não devem ser membros dos grupos locais de Administradores.
      >
      > Para saber mais sobre como usar as Preferências de Política de Grupo para modificar a associação de grupo, consulte o artigo da TechNet [Configurar um Item de Grupo Local](https://technet.microsoft.com/library/cc732525.aspx).

2. **Restringir a Associação no Grupo Local** - essa configuração garantirá que a associação dos grupos de administradores locais na estação de trabalho esteja sempre vazia
   1. Acesse Configuração do Computador\Preferências\Configurações de Painel de Controle\Usuários e Grupos Locais e execute as etapas abaixo:
      1. Clique em **Novo** e em **Grupo Local**
      2. Selecione a ação **Atualizar** e selecione "Operadores de Backup (interno)" (não use o botão Procurar para selecionar o grupo Operadores de Backup de domínio).
      3. Marque as caixas de seleção **Excluir todos os usuários membros** e **Excluir todos os grupos de membros**.
      4. Não adicione nenhum membro ao grupo.  Simplesmente clique em **OK**.  Ao atribuir uma lista vazia, a política de grupo automaticamente removerá todos os membros e garantirá uma lista de membros vazia sempre que cada política de grupo for atualizada.
   2. Conclua as etapas acima para os seguintes grupos adicionais:
      * Operadores criptográficos
      * Administradores do Hyper-V
      * Operadores de configuração de rede
      * Usuários avançados
      * Usuários da Área de Trabalho Remota
      * Replicadores
   3. **Restrições de Logon na PAW**: essa configuração limita as contas que podem se conectar à PAW. Execute as etapas abaixo para definir essa configuração:
      1. Acesse Configuração do Computador\Políticas\Configurações do Windows\Configurações de Segurança\Políticas locais\Atribuição de direitos de usuário\ Permitir logon localmente.
      2. Selecione Definir estas configurações de política e adicione "Usuários de PAW" e Administradores (novamente, não use o botão Procurar para selecionar Administradores).
   4. **Bloquear Tráfego de Rede de Entrada** - essa configuração garante que nenhum tráfego de rede de entrada não solicitado tenha permissão na PAW. Execute as etapas abaixo para definir essa configuração:
      1. Acesse Configuração do Computador\Políticas\Configurações do Windows\Configurações de Segurança\Firewall do Windows com Segurança Avançada\Firewall do Windows com Segurança Avançada e execute estas etapas:
         1. Clique com o botão direito do mouse em Firewall do Windows com Segurança Avançada e selecione **Importar política**.
         2. Clique em **Sim** para aceitar que isso substituirá quaisquer políticas de firewall existentes.
         3. Navegue até PAWFirewall.wfw e selecione **Abrir**.
         4. Clique em **OK**.

            > [!NOTE]
            > Você pode adicionar endereços ou sub-redes que precisam acessar a PAW com tráfego não solicitado neste momento (por exemplo, software de verificação ou gerenciamento de segurança).
            > As configurações no arquivo WFW permitirão o firewall no modo "Bloquear – Padrão" para todos os perfis de firewall, desativarão a mesclagem de regra e habilitarão o registro em log de pacotes descartados e bem-sucedidos. Essas configurações bloqueiam o tráfego não solicitado enquanto ainda permitem a comunicação bidirecional em conexões iniciadas da PAW, impedem que usuários com acesso administrativo local criem regras de firewall local que substituiriam as configurações de GPO e garantiriam o registro em log do tráfego de entrada e saída da PAW.
            > **Abrir esse firewall expandirá a superfície de ataque da PAW e aumentará o risco de segurança. Antes de adicionar os endereços, consulte a seção Gerenciar e operar a PAW neste guia**.

   5. **Configurar o Windows Update para o WSUS** - execute as etapas abaixo para alterar as configurações a fim de configurar o Windows Update para as PAWs:
      1. Acesse Configuração do Computador\Políticas\Modelos Administrativos\Componentes do Windows\Windows Update e execute as etapas abaixo:
         1. Habilite a política **Configurar Atualizações Automáticas**.
         2. Selecione a opção **4 - Baixar automaticamente e agendar a instalação**.
         3. Altere a opção **Dia agendado para a instalação** para **0 - Todos os dias** e a opção **Hora agendada para a instalação** para algo de sua preferência.
         4. Habilite a opção **Especificar local do serviço Microsoft Update na Intranet** e especifique em ambas as opções a URL do servidor WSUS ESAE.
   6. Vincule o GPO Configuração de PAW – Computador da seguinte maneira:

         |Política|Local do link|
         |-----|---------|
         |Configuração da PAW - Computador |Admin\Camada 0\Dispositivos|

#### <a name="create-paw-configuration---user-group-policy-object-gpo"></a>Criar GPO (objeto de política de grupo) "Configuração da PAW – Usuário"

Nesta seção, você criará um novo GPO "Configuração da PAW – Usuário" que fornece proteções específicas para essas PAWs e o vinculará à UO de Contas de Camada 0 ("Contas" em Camada 0\Admin).

   > [!NOTE]
   > Não adicione essas configurações à Política de Domínio Padrão

1. **Bloquear navegação na Internet** - Para deter a navegação acidental na Internet, isso definirá um endereço de proxy de um endereço de loopback (127.0.0.1).
   1. Vá para User Configuration\Preferences\Windows Settings\Registry. Clique com o botão direito do mouse em Registro, selecione **Novo** > **Item de Registro** e defina as seguintes configurações:
      1. Ação:  Substituir
      2. Hive: HKEY_CURRENT_USER
      3. Caminho da Chave:  Software\Microsoft\Windows\CurrentVersion\Internet Settings
      4. Nome do valor: ProxyEnable

         > [!NOTE]
         > Não selecione a caixa Padrão à esquerda do nome do Valor.

      5. Tipo de valor: REG_DWORD
      6. Dados do valor: 1
         1. Clique na guia Comum e selecione **Remover este item quando ele não for mais aplicado**.
         2. Na guia Comum selecione **Direcionamento de nível de item** e clique em **Direcionamento**.
         3. Clique em **Novo Item** e selecione **Grupo de segurança**.
         4. Selecione o botão "..." e navegue até o grupo Usuários da PAW.
         5. Clique em **Novo Item** e selecione **Grupo de segurança**.
         6. Selecione o botão "..." e navegue até o grupo **Administradores de Serviços de Nuvem**.
         7. Clique no item **Administradores de Serviços de Nuvem** e clique em **Opções de Item**.
         8. Selecione **Não é**.
         9. Clique em **OK** na janela de direcionamento.
      7. Clique em **OK** para concluir a configuração de política de grupo ProxyServer
   2. Vá para User Configuration\Preferences\Windows Settings\Registry. Clique com o botão direito do mouse em Registro, selecione **Novo** > **Item de Registro** e defina as seguintes configurações:

      * Ação: Substituir
      * Hive: HKEY_CURRENT_USER
      * Caminho da Chave: Software\Microsoft\Windows\CurrentVersion\Internet Settings
         * Nome do valor: ProxyServer

            > [!NOTE]
            > Não selecione a caixa Padrão à esquerda do nome do Valor.

         * Tipo de valor: REG_SZ
         * Dados do valor: 127.0.0.1:80
            1. Clique na guia **Comum** e selecione **Remover este item quando ele não for mais aplicado**.
            2. Na guia **Comum** selecione **Direcionamento de nível de item** e clique em **Direcionamento**.
            3. Clique em **Novo Item** e selecione Grupo de segurança.
            4. Selecione o botão "..." e adicione o grupo Usuários da PAW.
            5. Clique em **Novo Item** e selecione Grupo de segurança.
            6. Selecione o botão "..." e navegue até o grupo **Administradores de Serviços de Nuvem**.
            7. Clique no item **Administradores de Serviços de Nuvem** e clique em **Opções de Item**.
            8. Selecione **Não é**.
            9. Clique em **OK** na janela de direcionamento.

   3. Clique em **OK** para concluir a configuração de política de grupo ProxyServer,
2. Acesse Configuração do Usuário\Políticas\Modelos Administrativos\Componentes do Windows\Internet Explorer\ e habilite as opções a seguir. Essas configurações impedirão que os administradores substituam as configurações de proxy manualmente.
   1. Habilite **Desativar a alteração das definições de Configuração Automática**.
   2. Habilite **Impedir alteração das configurações de proxy**.

#### <a name="restrict-administrators-from-logging-onto-lower-tier-hosts"></a>Impedir que administradores efetuem logon em hosts de camada inferior

Nesta seção, configuraremos as políticas de grupo para impedir que contas com privilégios administrativos façam logon em hosts de camadas inferiores.

1. Crie o novo GPO **Restringir Logon na Estação de Trabalho** essa configuração restringirá o logon de contas de administrador de Camada 0 e Camada 1 em estações de trabalho padrão.  Esse GPO deve ser vinculado à UO de nível superior das "Estações de Trabalho" e ter as seguintes configurações:
   * Em Configuração do Computador\Políticas\Configurações de do Windows\Configurações de Segurança\Atribuição de Direitos de Usuário\Negar logon como um trabalho em lotes, selecione **Definir essas configurações de política** e adicione os grupos de Camada 1 e Camada 0:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > Grupos Internos de Camada 0, consulte a equivalência de Camada 0 para saber mais.

         Other Delegated Groups

     > [!NOTE]
     > quaisquer grupos personalizados com acesso efetivo de Camada 0, consulte Equivalência de Camada 0 para saber mais.

         Tier 1 Admins

     > [!NOTE]
     > este grupo foi criado anteriormente na Fase 1.

   * (i) Em Configuração do Computador\Políticas\Configurações de do Windows\Configurações de Segurança\Atribuição de Direitos de Usuário\Negar logon como um serviço, selecione **Definir essas configurações de política** e adicione os grupos de Camada 1 e Camada 0:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > Observação: Grupos Internos de Camada 0, consulte a equivalência de Camada 0 para saber mais.

         Other Delegated Groups

     > [!NOTE]
     > Observação: quaisquer grupos personalizados com acesso efetivo de Camada 0, consulte Equivalência de Camada 0 para saber mais.

         Tier 1 Admins

     > [!NOTE]
     > Observação: este grupo foi criado anteriormente na Fase 1

2. Crie o GPO **Restringir Logon no Servidor** – essa configuração restringirá o logon de contas de administrador de Camada 0 em servidores de Camada 1.  Esse GPO deve ser vinculado à UO de nível superior das "Servidores de Camada 1" e ter as seguintes configurações:
   * Em Configuração do Computador\Políticas\Configurações do Windows\Configurações de Segurança\Políticas Locais\Atribuição de Direitos de Usuário\Negar logon como um trabalho em lotes, selecione **Definir essas configurações de política** e adicione os grupos de Camada 0:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > Grupos Internos de Camada 0, consulte a equivalência de Camada 0 para saber mais.

         Other Delegated Groups

     > [!NOTE]
     > quaisquer grupos personalizados com acesso efetivo de Camada 0, consulte Equivalência de Camada 0 para saber mais.

   * Em Configuração do Computador\Políticas\Configurações do Windows\Configurações de Segurança\Políticas Locais\Atribuição de Direitos de Usuário\Negar logon como um serviço, selecione **Definir essas configurações de política** e adicione os grupos de Camada 0:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > Grupos Internos de Camada 0, consulte a equivalência de Camada 0 para saber mais.

         Other Delegated Groups

     > [!NOTE]
     > quaisquer grupos personalizados com acesso efetivo de Camada 0, consulte Equivalência de Camada 0 para saber mais.

   * Em Configuração do Computador\Políticas\Configurações do Windows\Configurações de Segurança\Políticas Locais\Atribuição de Direitos de Usuário\Negar logon localmente, selecione **Definir essas configurações de política** e adicione os grupos de Camada 0:
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     BUILTIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > Observação: Grupos Internos de Camada 0, consulte a equivalência de Camada 0 para saber mais.

         Other Delegated Groups

     > [!NOTE]
     > Observação: quaisquer grupos personalizados com acesso efetivo de Camada 0, consulte Equivalência de Camada 0 para saber mais.

#### <a name="deploy-your-paws"></a>Implantar sua(s) PAW(s)

   > [!NOTE]
   > Certifique-se de que a PAW esteja desconectada da rede durante o processo de compilação do sistema operacional.

1. Instale o Windows 10 usando a mídia de instalação de origem limpa que você obteve anteriormente.

   > [!NOTE]
   > Você pode usar o MDT (Microsoft Deployment Toolkit) ou outro sistema de implantação de imagem automatizado para automatizar a implantação da PAW, mas é necessário garantir que o processo de compilação seja tão confiável quanto a PAW. Os adversários procuram especificamente por imagens corporativas e sistemas de implantação (incluindo ISOs, pacotes de implantação etc.) como um mecanismo de persistência, então sistemas de implantação ou imagens preexistentes não devem ser usados.
   >
   > Se você automatizar a implantação da PAW, faça o seguinte:
   >
   > * Compile o sistema usando a mídia de instalação validada seguindo a orientação em [Origem limpa para a mídia de instalação](https://aka.ms/cleansource).
   > * Certifique-se de que o sistema de implantação automatizado esteja desconectado da rede durante o processo de compilação do sistema operacional.

2. Defina uma senha complexa e exclusiva para a conta de administrador local.  Não use uma senha que foi usada para qualquer outra conta no ambiente.

   > [!NOTE]
   > A Microsoft recomenda usar a [LAPS (Solução de Senha de Administrador Local)](https://www.microsoft.com/download/details.aspx?id=46899) para gerenciar a senha de administrador local para todas as estações de trabalho, incluindo as PAWs.  Se você usar a LAPS, conceda somente ao grupo Manutenção da PAW o direito de ler senhas gerenciadas pela LAPS para as PAWs.

3. Instale as Ferramentas de Administração de Servidor Remoto para Windows 10 usando a mídia de instalação de origem limpa.
4. Configurar o Windows Defender Exploit Guard

   > [!NOTE]
   > Diretrizes de configuração a seguir

5. Conectar a PAW à rede.  Certifique-se de que a PAW possa conectar-se a pelo menos um Controlador de Domínio (DC).
6. Usando uma conta membro do grupo Manutenção da PAW, execute o seguinte comando do PowerShell na PAW recém-criada a fim de ingressar o domínio na UO adequada:

   `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

   Substitua as referências a *Fabrikam* por seu nome de domínio, conforme apropriado.  Se o seu nome de domínio se estender a vários níveis (por exemplo, child.fabrikam.com), adicione os outros nomes com o identificador "DC =" na ordem em que aparecem no nome de domínio totalmente qualificado do domínio.

   > [!NOTE]
   > Se você implantou uma [Floresta Administrativa ESAE](https://aka.ms/esae) (para administradores de Camada 0 na Fase 1) ou um [PAM (gerenciamento de acesso privilegiado) do MIM (Microsoft Identity Manager)](https://aka.ms/mimpamdeploy) (para administradores de Camada 1 e 2 na Fase 2), você ingressaria a PAW ao domínio nesse ambiente, em vez do domínio de produção.

7. Aplique todas as atualizações críticas e importantes do Windows antes de instalar qualquer outro software (incluindo ferramentas administrativas, agentes etc.).
8. Force a aplicação da Política de Grupo.
   1. Abra um prompt de comandos com privilégios elevados e digite o seguinte comando: `Gpupdate /force /sync`
   2. Reinicie o computador

9. (Opcional) Instale as ferramentas adicionais necessárias para os Administradores do Active Directory. Instale outras ferramentas ou scripts necessários para realizar seus trabalhos. Avalie o risco de exposição de credencial nos computadores de destino com alguma ferramenta antes de adicioná-los a uma PAW. Acesse [esta página](https://aka.ms/logontypes) para obter mais informações sobre como avaliar as ferramentas administrativas e os métodos de conexão para o risco de exposição de credencial. Obtenha todas as mídias de instalação usando as diretrizes de [Origem limpa para a mídia de instalação](https://aka.ms/cleansource).

   > [!NOTE]
   > Usar um servidor de salto como local central para essas ferramentas pode reduzir a complexidade, mesmo que não sirva como um limite de segurança.

10. (Opcional) Baixe e instale o software de acesso remoto necessário. Se os administradores usarem a PAW remotamente para administração, instale o software de acesso remoto usando as diretrizes de segurança de seu fornecedor de solução de acesso remoto. Obtenha todas as mídias de instalação usando as diretrizes de Origem limpa para a mídia de instalação.

    > [!NOTE]
    > Considere cuidadosamente todos os riscos envolvidos na permissão de acesso remoto via uma PAW.  Embora uma PAW móvel permita vários cenários importantes, incluindo home office, o software de acesso remoto pode ficar vulnerável a ataques e ser usado para comprometer uma PAW.

11. Valide a integridade do sistema da PAW revisando e confirmando que todas as configurações apropriadas foram aplicadas usando as etapas abaixo:
    1. Confirme se apenas as políticas de grupo específicas às PAWs foram aplicadas à PAW
       1. Abra um prompt de comandos com privilégios elevados e digite o seguinte comando: `Gpresult /scope computer /r`
       2. Examine a lista resultante e certifique-se de que as políticas de grupo que aparecem são aqueles que você criou logo acima.
    2. Confirme que nenhuma conta de usuário adicional é membro dos grupos privilegiados na PAW usando as etapas abaixo:
       1. Abra **Editar Usuários e Grupos Locais** (lusrmgr.msc), selecione **Grupos** e confirme que os únicos membros do grupo local Administradores são a conta de Administrador local e o grupo de segurança global Manutenção da PAW.

          > [!NOTE]
          > O grupo Usuários da PAW não deve ser membro do grupo local Administradores.  Os únicos membros devem ser a conta local de Administrador e o grupo de segurança global Manutenção da PAW (e Usuários da PAW não deve ser um membro desse grupo global).

       2. Além disso, usando **Editar Usuários e Grupos Locais**, verifique se os seguintes grupos têm algum membro: Operadores de Backup, Operadores de Criptografia, Administradores do Hyper-V, Operadores de Configuração de Rede, Usuários Avançados, Usuários da Área de Trabalho Remota, Replicadores

12. (Opcional) Se sua organização usa uma solução SIEM (Gerenciamento de informações e eventos de segurança), a PAW deverá estar [configurada para encaminhar eventos para o sistema usando o WEF (Windows Event Forwarding)](https://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx) ou registrada com a solução para que a SIEM receba ativamente eventos e informações da PAW.  Os detalhes dessa operação poderão variar com base em sua solução SIEM.

    > [!NOTE]
    > Se a SIEM exigir um agente executado como uma conta de sistema ou de administração local nas PAWs, certifique-se de que as SIEMs sejam gerenciadas com o mesmo nível de confiança que os controladores de domínio e sistemas de identidade.

13. (Opcional) Se você optar por implantar LAPS para gerenciar a senha da conta local de Administrador em sua PAW, verifique se a senha foi registrada com êxito.

    * Usando uma conta com permissões para ler senhas gerenciadas por LAPS, abra **Usuários e Computadores do Active Directory** (dsa.msc).  Verifique se Recursos avançados está habilitado e, em seguida, clique com o botão direito do mouse no objeto do computador apropriado.  Selecione a guia Editor de Atributos e confirme se o valor para msSVSadmPwd foi preenchido com uma senha válida.

### <a name="phase-2-extend-paw-to-all-administrators"></a>Fase 2: extensão da PAW a todos os administradores

Escopo: todos os usuários com direitos administrativos sobre aplicativos e dependências de missão crítica.  Isso deve incluir pelo menos os administradores de servidores de aplicativos, soluções de monitoramento de integridade w segurança operacional, soluções de virtualização, sistemas de armazenamento e dispositivos de rede.

> [!NOTE]
> As instruções nesta fase presumem que a Fase 1 foi concluída totalmente.  Não comece a Fase 2 até concluir todas as etapas na Fase 1.

Depois de confirmar que todas as etapas foram executadas, execute as etapas abaixo para concluir a Fase 2:

#### <a name="recommended-enable-restrictedadmin-mode"></a>(Recomendado) Habilitar o modo **RestrictedAdmin**

Habilite esse recurso nos servidores e estações de trabalho existentes e, em seguida, imponha o uso desse recurso. Esse recurso exigirá a execução dos servidores de destino no Windows Server 2008 R2 ou posterior e nas estações de trabalho de destino executando o Windows 7 ou posterior.

1. Habilite o modo **RestrictedAdmin** nos servidores e estações de trabalho seguindo as instruções disponíveis nesta [página](https://aka.ms/rdpra).

   > [!NOTE]
   > Antes de habilitar esse recurso para servidores voltados à Internet, considere o risco de os adversários serem capazes de autenticar nesses servidores com um hash de senha roubada anteriormente.

2. Crie o GPO (Objeto de Política de Grupo) "RestrictedAdmin Required – Computer". Esta seção cria um GPO que impõe o uso da opção /RestrictedAdmin para conexões de Área de Trabalho Remota de saída, protegendo contas contra o roubo de credenciais nos sistemas de destino
   * Acesse Configuração do Computador\Políticas\Modelos Administrativos\Sistema\Delegação de Credenciais\Restringir delegação de credenciais para servidores remotos e defina como **Habilitado**.
3. Vincule o **RestrictedAdmin** Requires - Computador aos dispositivos apropriados da Camada 1 e/ou Camada 2 usando as opções de Política a seguir:
   * Configuração da PAW - Computador
      * -> Local do link: Admin\Camada 0\Dispositivos (existente)
   * Configuração da PAW - Usuário
      * -> Local do link: Admin\Camada 0\Contas
   * RestrictedAdmin Required - Computador
      * --> Admin\Camada1\Dispositivos ou --> Admin\Camada2\Dispositivos (ambos opcionais)

   > [!NOTE]
   > Isso não é necessário para sistemas de Camada 0, pois esses sistemas já estão no controle total de todos os ativos no ambiente.

#### <a name="move-tier-1-objects-to-the-appropriate-ous"></a>Mova objetos da Camada 1 para as UOs apropriadas

1. Mova grupos da Camada 1 para a UO Admin\Camada 1\Grupos. Localize todos os grupos que concedem os seguintes direitos administrativos e mova-os para essa UO.
   * Administrador local em mais de um servidor
      * Acesso Administrativo aos serviços de nuvem
      * Acesso Administrativo aos aplicativos corporativos
2. Mova as contas da Camada 1 para a UO Admin\Camada 1\Contas. Mova cada conta membro dos grupos de Camada 1 (incluindo associação aninhada) para essa UO.
3. Adicionar os membros apropriados aos grupos relevantes
   * **Administradores de Camada 1** - Esse grupo conterá os Administradores de Camada 1 que não poderão efetuar logon em hosts de Camada 2. Adicione todos os seus grupos administrativos de Camada 1 que tenham privilégios administrativos em servidores ou serviços de Internet.

      > [!NOTE]
      > Se a equipe administrativa tiver obrigações de gerenciamento de ativos em várias camadas, será necessário criar uma conta de administrador separada por camada.

4. Habilite o Credential Guard para reduzir o risco de roubo e reutilização de credenciais.  O Credential Guard é um novo recurso do Windows 10 que restringe o acesso do aplicativos às credenciais, impedindo ataques de roubo de credenciais (incluindo Passagem de Hash).  O Credential Guard é completamente transparente para o usuário final e exige o mínimo de configuração.  Para saber mais sobre o Credential Guard, incluindo etapas de implantação e requisitos de hardware, consulte o artigo [Proteger as credenciais de domínio com o Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx).

   > [!NOTE]
   > O Device Guard deve ser habilitado para configurar e usar o Credential Guard.  No entanto, não é necessário configurar quaisquer outras proteções do Device Guard para usar o Credential Guard.

5. (Opcional) Habilite a conectividade com os Serviços de Nuvem. Esta etapa permite o gerenciamento de serviços de nuvem como o Azure e o Office 365 com as garantias de segurança apropriadas. Esta etapa também é necessária para o Microsoft Intune, a fim de gerenciar as PAWs.

   > [!NOTE]
   > Ignore esta etapa se não houver a necessidade de conectividade de nuvem para a administração dos serviços de nuvem ou para o gerenciamento pelo Intune.

   * Essas etapas restringirão a comunicação pela Internet somente aos serviços de nuvem autorizados (mas não a Internet aberta) e acrescentarão proteções aos navegadores e a outros aplicativos que processarão o conteúdo da Internet. Esses PAWs para administração nunca devem ser usadas para tarefas de usuário padrão, como comunicação e produtividade pela Internet.
   * Para habilitar a conectividade para os serviços da PAW, execute as etapas abaixo:

   1. Configure a PAW para permitir somente destinos da Internet autorizados.  À medida que você estender sua implantação PAW para habilitar a administração de nuvem, será necessário permitir o acesso a serviços autorizados durante a filtragem de acesso da Internet aberta, onde os ataques contra seu administradores podem ocorrer com mais facilidade.

      1. Crie o grupo **Administradores de Serviços de Nuvem** e adicione todas as contas que exigem acesso aos serviços de nuvem na Internet.
      2. Baixe o arquivo *proxy.pac* da PAW na [Galeria do TechNet](https://aka.ms/pawmedia) e publique-o em um site interno.

         > [!NOTE]
         > Será necessário atualizar o arquivo *proxy.pac* após o download, para garantir que ele esteja atualizado e completo.  
         > A Microsoft publica todas as URLs atuais do Office 365 e do Azure [Centro de Suporte do Office](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US). Essas instruções pressupõem que você usará o Internet Explorer (ou Microsoft Edge) para a administração do Office 365, Azure e outros serviços de nuvem. A Microsoft recomenda a configuração de restrições semelhantes para qualquer navegador de terceiros que você precise usar para a administração. Só use navegadores da Web nas PAWs para a administração de serviços de nuvem, nunca para navegação geral.
         >
         > Talvez seja necessário adicionar outros destinos da Internet válidos para aumentar essa lista para outro provedor de IaaS, mas não adicione sites de produtividade, entretenimento, notícias ou pesquisa a esta lista.
         >
         > Talvez você precise ajustar o arquivo PAC para acomodar um endereço de proxy válido a ser usado para esses endereços.
         >
         > Você também pode restringir o acesso da PAW usando um proxy da Web para uma defesa mais profunda. Não recomendamos isso sem o arquivo PAC, pois ele apenas restringirá o acesso às PAWs enquanto estiver conectado à rede corporativa.

      3. Após a configuração do arquivo *proxy.pac*, atualize o GPO Configuração da PAW - Usuário.
         1. Vá para User Configuration\Preferences\Windows Settings\Registry. Clique com o botão direito do mouse em Registro, selecione **Novo** > **Item de Registro** e defina as seguintes configurações:
            1. Ação: Substituir
            2. Hive: HKEY_ CURRENT_USER
            3. Caminho da Chave: Software\Microsoft\Windows\CurrentVersion\Internet Settings
            4. Nome do valor: AutoConfigUrl

               > [!NOTE]
               > Não selecione a caixa **Padrão** à esquerda do nome do Valor.

            5. Tipo de valor: REG_SZ
            6. Dados do valor: insira a URL completa para o arquivo *proxy.pac*, incluindo http:// e o nome do arquivo – por exemplo, http://proxy.fabrikam.com/proxy.pac.  A URL também pode ser uma URL de rótulo único, por exemplo, http://proxy/proxy.pac

               > [!NOTE]
               > O arquivo PAC também pode estar hospedado em um compartilhamento de arquivos, com a sintaxe file://server.fabrikan.com/share/proxy.pac, mas isso exige a permissão do protocolo file://. Confira a seção "OBSERVAÇÃO: scripts de proxy com base em File:// estão obsoletos desta postagem de blog [Noções básicas sobre configuração de proxy da Web](https://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx) para saber mais sobre como configurar o valor de Registro necessário.

            7. Clique na guia **Comum** e selecione **Remover este item quando ele não for mais aplicado**.
            8. Na guia **Comum** selecione **Direcionamento de nível de item** e clique em **Direcionamento**.
            9. Clique em **Novo Item** e selecione **Grupo de segurança**.
            10. Selecione o botão "..." e navegue até o grupo **Administradores de Serviços de Nuvem**.
            11. Clique em **Novo Item** e selecione **Grupo de segurança**.
            12. Selecione o botão "..." e navegue até o grupo **Usuários da PAW**.
            13. Clique no item **Usuários de PAW** e clique em **Opções de Item**.
            14. Selecione **Não é**.
            15. Clique em **OK** na janela de direcionamento.
            16. Clique em **OK** para concluir a configuração de política de grupo **AutoConfigUrl**.
   2. Aplique as Linhas de base de segurança do Windows 10 e o Link de acesso do serviço de nuvem das linhas de base de segurança para Windows e para o acesso ao serviço de nuvem (se for necessário) às UOs corretas usando as etapas abaixo:
      1. Extraia o conteúdo do arquivo ZIP de Linhas de base de segurança do Windows 10.
      2. Crie esses GPOs, [importe as configurações da política](https://technet.microsoft.com/library/cc753786.aspx) e [vincule](https://technet.microsoft.com/library/cc732979.aspx) de acordo com esta tabela. Vincule cada política a cada local e garante que a ordem siga a tabela (entradas inferiores na tabela devem ser aplicadas posteriormente e com maior prioridade):

         **Políticas:**

         |||
         |-|-|
         |CM Windows 10 - Segurança de Domínio|N/A - Não Vincular Agora|
         |SCM Windows 10 TH2 - Computador|Admin\Camada 0\Dispositivos|
         ||Admin\Camada 1\Dispositivos|
         ||Admin\Camada 2\Dispositivos|
         |SCM Windows 10 TH2- BitLocker|Admin\Camada 0\Dispositivos|
         ||Admin\Camada 1\Dispositivos|
         ||Admin\Camada 2\Dispositivos|
         |SCM Windows 10 - Credential Guard|Admin\Camada 0\Dispositivos|
         ||Admin\Camada 1\Dispositivos|
         ||Admin\Camada 2\Dispositivos|
         |SCM Internet Explorer - Computador|Admin\Camada 0\Dispositivos|
         ||Admin\Camada 1\Dispositivos|
         ||Admin\Camada 2\Dispositivos|
         |Configuração da PAW - Computador|Admin\Camada 0\Dispositivos (existente)|
         ||Admin\Camada 1\Dispositivos (novo link)|
         ||Admin\Camada 2\Dispositivos (novo link)|
         |RestrictedAdmin Required - Computador|Admin\Camada 0\Dispositivos|
         ||Admin\Camada 1\Dispositivos|
         ||Admin\Camada 2\Dispositivos|
         |SCM Windows 10 - Usuário|Admin\Camada 0\Dispositivos|
         ||Admin\Camada 1\Dispositivos|
         ||Admin\Camada 2\Dispositivos|
         |SCM Internet Explorer - Usuário|Admin\Camada 0\Dispositivos|
         ||Admin\Camada 1\Dispositivos|
         ||Admin\Camada 2\Dispositivos|
         |Configuração da PAW - Usuário|Admin\Camada 0\Dispositivos (existente)|
         ||Admin\Camada 1\Dispositivos (novo link)|
         ||Admin\Camada 2\Dispositivos (novo link)|

         > [!NOTE]
         > O "SCM Windows 10 - Segurança" GPO pode ser vinculado ao domínio independentemente da PAW, mas afetará todo o domínio.

6. (Opcional) Instale as ferramentas adicionais necessárias para administradores da Camada 1. Instale outras ferramentas ou scripts necessários para realizar seus trabalhos. Avalie o risco de exposição de credencial nos computadores de destino com alguma ferramenta antes de adicioná-los a uma PAW. Para saber mais sobre como avaliar as ferramentas administrativas e os métodos de conexão com relação ao risco de exposição de credencial visite [esta página](https://aka.ms/logontypes). Obtenha todas as mídias de instalação usando as diretrizes de Origem limpa para a mídia de instalação
7. Identifique e obtenha com segurança o software e os aplicativos necessários para administração.  Isso é semelhante ao trabalho realizado na Fase 1, mas com um escopo mais amplo devido à quantidade maior de aplicativos, serviços e sistemas que estão sendo protegidos.

   > [!NOTE]
   > Proteja esses novos aplicativos (incluindo navegadores da Web) aceitando as proteções fornecidas pelo Windows Defender Exploit Guard.

   Exemplos de aplicativos e softwares adicionais:

      * Microsoft Azure PowerShell
      * Office 365 PowerShell (também conhecido como Módulo do Microsoft Online Services)
      * Software de gerenciamento de aplicativo ou de serviço com base no Console de Gerenciamento Microsoft
      * Software de gerenciamento de aplicativo ou de serviço proprietário (não baseado em MMC)

         > [!NOTE]
         > Agora, muitos aplicativos são gerenciados exclusivamente em navegadores da Web, incluindo muitos serviços de nuvem.  Embora isso reduza a quantidade de aplicativos que precisam ser instalados em uma PAW, isso também introduz o risco de problemas de interoperabilidade do navegador.  Talvez seja necessário implantar um navegador da Web que não seja da Microsoft em instâncias específicas da PAW a fim de habilitar a administração de serviços específicos.  Se você implantar um navegador da Web adicional, siga todos os princípios de origem limpa e proteja o navegador de acordo com as diretrizes de segurança do fornecedor.

8. (Opcional) Baixe e instale os agentes de gerenciamento necessários.

   > [!NOTE]
   > Se você optar por instalar agentes de gerenciamento adicionais (monitoramento, segurança, gerenciamento de configuração etc.), é vital que você verifique se os sistemas de gerenciamento são confiáveis no mesmo nível que os controladores de domínio e sistemas de identidade. Consulte Gerenciar e atualizar PAWs para obter orientação adicional.

9. Avalie a infraestrutura para identificar sistemas que exigem as proteções de segurança adicionais fornecidas por uma PAW.  Saiba exatamente quais sistemas devem ser protegidos.  Faça perguntas importantes sobre os próprios recursos, como:

   * Onde estão os sistemas de destino que devem ser gerenciados?  Eles são coletados em um único local físico, ou conectados a uma única sub-rede bem definida?
   * Quantos sistemas existem?
   * Esses sistemas dependem de outros sistemas (virtualização, armazenamento etc.) e, em caso afirmativo, como esses sistemas são gerenciados?  Como os sistemas críticos são expostos a essas dependências, e quais são os riscos adicionais associados a essas dependências?
   * Qual a importância dos serviços gerenciados, e quais são as perdas esperada se esses serviços forem comprometidos?

      > [!NOTE]
      > Inclua seus serviços de nuvem nesta avaliação, os invasores visam cada vez mais as implantações de nuvem desprotegidas, e é essencial que você administre esses serviços com a segurança que você usaria com seus aplicativos de missão crítica locais.

        Use esta avaliação para identificar os sistemas específicos que exigem proteção adicional, e estender seu programa PAW para os administradores desses sistemas.  Entre os exemplos comuns de sistemas que se beneficiam muito da administração baseada em PAW estão o SQL Server (local e SQL Azure), aplicativos de recursos humanos e software financeiro.

        > [!NOTE]
        > Se um recurso for gerenciado de um sistema Windows, ele poderá ser gerenciado com uma PAW, mesmo se o próprio aplicativo for executado em um sistema operacional diferente do Windows ou em uma plataforma de nuvem que não seja da Microsoft.  Por exemplo, o proprietário de uma assinatura do Amazon Web Services só deve usar uma PAW para administrar essa conta.

10. Desenvolva um método de solicitação e a distribuição para implantar as PAWs em larga escala em sua organização.  Dependendo do número de PAWs que você optar por implantar na Fase 2, talvez seja necessário automatizar o processo.

    * Considere o desenvolvimento de um processo de solicitação e aprovação formal para que os administradores usem a fim de obter uma PAW.  Esse processo ajudaria a padronizar o processo de implantação, assegurar a responsabilidade para dispositivos PAW e ajudar a identificar lacunas na implantação da PAW.
    * Conforme mencionado anteriormente, essa solução de implantação deve ser separada dos métodos de automação existentes (que talvez já estejam comprometidos) e deve seguir os princípios descritos na Fase 1.

        > [!NOTE]
        > Qualquer sistema que gerencie os recursos deve ser ele próprio gerenciado no mesmo nível de confiança ou superior.

11. Revise e, se for necessário, implante perfis de hardware de PAW adicionais.  Talvez o perfil de hardware que você escolheu para implantação da Fase 1 não seja adequado para todos os administradores.  Revise os perfis de hardware e, se for apropriado, selecione perfis de hardware de PAW adicionais, a fim de atender às necessidades dos administradores.  Por exemplo, o perfil de Hardware dedicado (separar PAW das estações de trabalho de uso diário) pode ser inadequado para um administrador que viaja frequentemente. Nesse caso, você pode optar por implantar o perfil de Uso simultâneo (PATA com VM de usuário) para esse administrador.
12. Considere as necessidades culturais, operacionais, comunicações e de treinamento que acompanham uma implantação de PAW estendida.   Uma alteração considerável em um modelo administrativo naturalmente exigirá um certo nível de gerenciamento de mudanças, e é essencial agregar isso ao próprio projeto de implantação.  Considere no mínimo o seguinte:

    * Como você comunicará as alterações à liderança sênior para garantir o apoio dela?  Qualquer projeto sem o apoio da liderança sênior provavelmente falhará ou, no máximo, terá dificuldades de financiamento e ampla aceitação.
    * Como você documentará o novo processo para administradores?  Essas alterações devem ser documentadas e comunicadas não apenas aos administradores existentes (que devem mudar seus hábitos e gerenciar os recursos de forma diferente), mas também para novos administradores (aqueles promovidos de dentro ou contratados de fora da organização).  É essencial que a documentação esteja clara e enfatize totalmente a importância das ameaças, a função da PAW na proteção dos administradores e como usar a PAW corretamente.

      > [!NOTE]
      > Isso é especialmente importante para funções com alta rotatividade, incluindo, mas sem limitação, a equipe de suporte técnico.

    * Como você garantirá a conformidade com o novo processo?  Embora o modelo PAW inclua diversos controles técnicos para evitar a exposição de credenciais com privilégios, é impossível evitar totalmente toda exposição possível usando unicamente controles técnicos.  Por exemplo, embora seja possível evitar que um administrador faça logon em um desktop de usuário com credenciais privilegiadas, o simples ato de tentar o logon pode expor as credenciais a algum malware instalado desse desktop de usuário.  Portanto, é essencial que você descreva, além dos benefícios do modelo da PAW, os riscos da não conformidade.  Isso deve ser complementado por auditoria e alertas, para que a exposição de credencial possa ser detectada e resolvida rapidamente.

### <a name="phase-3-extend-and-enhance-protection"></a>Fase 3: Estender e aprimorar a proteção

Escopo: essas proteções aprimoram os sistemas desenvolvidos na Fase 1, reforçando a proteção básica com recursos avançados, incluindo autenticação multifator e regras de acesso de rede.

> [!NOTE]
> Essa fase pode ser executada a qualquer momento após a conclusão da Fase 1.  Ela não depende da conclusão da Fase 2 e, portanto, pode ser realizada antes, simultaneamente ou após a Fase 2.

Execute as etapas abaixo para configurar essa fase:

1. **Habilite a autenticação multifator para contas privilegiadas.**  A autenticação multifator reforça a segurança da conta exigindo que o usuário forneça um token físico além das credenciais.  A autenticação multifator complementa muito bem as políticas de autenticação, mas não depende de políticas de autenticação para a implantação (e, da mesma forma, as políticas de autenticação não exigem autenticação multifator).  A Microsoft recomenda o uso de uma destas formas de autenticação multifator:

   * **Cartão inteligente**: um cartão inteligente é um dispositivo físico portátil e inviolável que fornece uma segunda verificação durante o processo de logon do Windows.  Ao exigir que um indivíduo possua um cartão para o logon, você pode reduzir o risco de reutilização remota das credenciais roubadas.  Para saber mais sobre o logon com cartão inteligente no Windows, consulte o artigo [Visão geral do cartão inteligente](https://technet.microsoft.com/library/hh831433.aspx).
   * **Cartão inteligente virtual**:  um cartão inteligente virtual fornece os mesmos benefícios de segurança dos cartões inteligentes físicos, com a vantagem adicional de ser vinculado ao hardware específico.  Para saber mais sobre a implantação e os requisitos de hardware, consulte os artigos [Visão geral do cartão inteligente virtual](https://technet.microsoft.com/library/dn593708.aspx) e [Introdução aos cartões inteligentes virtuais: guia passo a passo](https://technet.microsoft.com/library/dn579260.aspx).
   * **Windows Hello para Empresas**: o Microsoft Hello para Empresas permite a autenticação de usuários para uma conta da Microsoft, uma conta do Active Directory, uma conta do Active Directory (Azure AD) do Microsoft Azure ou um serviço que não seja da Microsoft que oferece suporte à FIDO (Autenticação Fast ID Online). Após uma verificação inicial em duas etapas durante o registro do Windows Hello para Empresas, um Windows Hello para Empresas é configurado no dispositivo do usuário e o usuário define um gesto, que pode ser um PIN ou Windows Hello. As credenciais do Windows Hello para Empresas são um par de chaves assimétricas, que podem ser geradas em ambientes isolados do Trusted Platform Modules (TPMs).
      Para obter mais informações sobre o Windows Hello para Empresas, leia o artigo [Windows Hello para Empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification).
   * **Autenticação Multifator do Azure**:  a MFA (Autenticação multifator) do Azure fornece a segurança de um segundo fator de verificação, bem como proteção aprimorada por meio de monitoramento e de análise baseada em aprendizado de máquina.  A MFA do Azure pode proteger não só os administradores do Azure, mas muitas outras soluções, incluindo aplicativos Web, Azure Active Directory e soluções locais, como o acesso remoto e Área de Trabalho Remota.  Para saber mais sobre a autenticação multifator do Azure, consulte o artigo [Autenticação multifator](https://azure.microsoft.com/services/multi-factor-authentication).

2. **Aplicativos confiáveis da lista de permissões usando o Controle de Aplicativos do Windows Defender e/ou do AppLocker**.  Ao limitar a capacidade do código não confiável ou não assinado executar em uma PAW, você reduz ainda mais a probabilidade de atividade mal-intencionada e comprometimento.  O Windows inclui duas opções principais para controle de aplicativo:

   * **AppLocker**:  o AppLocker ajuda os administradores a controlarem quais aplicativos e arquivos podem ser executados em um certo sistema.  O AppLocker pode ser controlado centralmente pela política de grupo, e aplicado a usuários ou grupos específicos (para aplicativos direcionados a usuários de PAWs).  Para saber mais sobre como o AppLocker, consulte o artigo da TechNet [Visão geral do AppLocker](https://technet.microsoft.com/library/hh831440.aspx).
   * **Controle de Aplicativos do Windows Defender**: o novo recurso de Controle de Aplicativos do Windows Defender fornece controle aprimorado de aplicativos com base em hardware que, ao contrário do AppLocker, não pode ser substituído no dispositivo afetado.  Assim como o AppLocker, o Controle de Aplicativos do Windows Defender pode ser controlado por meio da política de grupo e direcionado a usuários específicos.  Para obter mais informações sobre como restringir o uso do aplicativo com o Controle de Aplicativos do Windows Defender, confira [Guia de Implantação do Controle de Aplicativos do Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).

3. **Use Usuários Protegidos, Políticas de Autenticação e Silos de Autenticação para proteger ainda mais as contas privilegiadas**.  Os membros de Usuários Protegidos estão sujeitos a políticas de segurança adicionais que protegem as credenciais armazenadas no agente de segurança local (LSA) e reduz bastante o risco de roubo e reutilização de credenciais.  Políticas e silos de autenticação controlam como os usuários privilegiados podem acessar recursos no domínio.  Coletivamente, essas proteções fortalecem consideravelmente a segurança da conta desses usuários privilegiados.  Para saber mais sobre esses recursos, consulte o artigo da Web [Como configurar contas protegidas](https://technet.microsoft.com/library/dn518179.aspx).

   > [!NOTE]
   > Essas proteções são complementares e não substituem as medidas existentes de segurança na Fase 1.  Os administradores ainda devem usar contas separadas para administração e uso geral.

## <a name="managing-and-updating"></a>Gerenciamento e atualização

As PAWs devem ter recursos antimalware, e as atualizações de software devem ser aplicadas rapidamente a fim de manter a integridade dessas estações de trabalho.

Gerenciamento de configuração adicional, monitoramento operacional e gerenciamento de segurança também podem ser usados com PAWs, mas a integração deles deve ser considerada com cuidado, pois cada recurso de gerenciamento também introduz o risco de comprometimento da PAW por meio dessa ferramenta. Se faz sentido introduzir recursos avançados de gerenciamento depende de inúmeros fatores, incluindo:

* O estado de segurança e práticas do recurso de gerenciamento (incluindo práticas de atualização de software para a ferramenta, contas e funções administrativas nessas funções, sistemas operacionais nos quais ferramenta está hospedada ou dos quais é gerenciada e quaisquer outras dependências de hardware ou software dessa ferramenta)
* A frequência e a quantidade de implantações de software e atualizações em suas PAWs
* Requisitos para obter informações detalhadas de inventário e configuração
* Requisitos de monitoramento de segurança
* Padrões organizacionais e outros fatores organizacionais específicos

De acordo com o princípio da origem limpa, todas as ferramentas usadas para gerenciar ou monitorar as PAWs devem ser confiáveis no nível das PAWs, ou acima dele. Isso geralmente exige que essas ferramentas sejam gerenciadas de uma PAW para garantir a ausência de dependência de segurança de estações de trabalho com privilégios inferiores.

Esta tabela descreve as diferentes abordagens que podem ser usadas para gerenciar e monitorar as PAWs:

|Abordagem|Considerações|
|------|---------|
|Padrão na PAW<p>-   Windows Server Update Services<br />-   Windows Defender|-   Sem custo adicional<br />-   Realiza funções básicas de segurança necessárias<br />-   Instruções incluídas neste guia|
|Gerenciar com o [Intune](https://technet.microsoft.com/library/jj676587.aspx)|<ul><li>Fornece controle e visibilidade com base na nuvem<p><ul><li>Implantação de software</li><li>o   Gerenciar atualizações de software</li><li>Gerenciamento de política de firewall do Windows</li><li>Proteção antimalware</li><li>Assistência Remota</li><li>Gerenciamento de licenças de software.</li></ul></li><li>Nenhuma infraestrutura de servidor é necessária</li><li>Exige as seguintes etapas de "Habilitar a conectividade para serviços de nuvem" na Fase 2</li><li>Se o computador PAW não tiver ingressado em um domínio, será necessário aplicar as linhas de base de SCM às imagens locais usando as ferramentas fornecidas no download de linha de base de segurança.</li></ul>|
|Novas instâncias do System Center para gerenciar PAWs|-   Oferece visibilidade e controle de configuração, implantação de software e atualizações de segurança<br />-   Exige infraestrutura de servidor separada, protegendo-a no nível das PAWs e qualificações de equipe para os funcionários altamente privilegiados|
|Gerenciar PAWs com ferramentas de gerenciamento existentes|- Cria um risco significativo de comprometer as PAWs, a menos que a infraestrutura de gerenciamento existente tenha o nível de segurança das PAWs **Observação:**     a Microsoft geralmente não incentiva essa abordagem, a menos que sua organização tenha um motivo específico para usá-la. De acordo com a nossa experiência, normalmente há um custo muito alto para colocar todas essas ferramentas (e suas dependências de segurança) no nível de segurança das PAWs.<br />-   A maioria dessas ferramentas oferece visibilidade e controle de configuração, implantação de software e atualizações de segurança|
|Ferramentas de Verificação de Segurança ou monitoramento precisam de acesso de administrador|Inclui qualquer ferramenta que instala um agente ou exige uma conta com acesso administrativo local.<p>-   Exige que a garantia de segurança da ferramenta seja levada até o nível das PAWs.<br />-   Pode exibir uma postura de segurança inferior das PAWs a fim de oferecer suporte à funcionalidade da ferramenta (abrir portas, instalar Java e outro middleware etc.), criando uma decisão de compensação de segurança,|
|SIEM (Gerenciamento de informações e eventos de segurança)|<ul><li>Se a SIEM não tiver um agente<p><ul><li>Pode acessar eventos nas PAWs sem acesso administrativo usando uma conta do grupo **Leitores de Log de Eventos**</li><li>Exigirá a abertura das portas de rede a fim de permitir o tráfego de entrada dos servidores SIEM</li></ul></li><li>Se a SIEM exigir um agente, consulte outra linha: **Ferramentas de Verificação de Segurança ou monitoramento que precisam de acesso administrativo**.</li></ul>|
|Windows Event Forwarding|-   Fornece um método sem agente de encaminhamento de eventos de segurança das PAWs para um coletor externo ou SIEM<br />-  Pode acessar eventos em PAWs sem acesso administrativo<br />-   Não exige a abertura das portas de rede a fim de permitir o tráfego de entrada dos servidores SIEM|

## <a name="operating-paws"></a>Operação das PAWs

A solução PAW deve ser operada usando os padrões em [Padrões operacionais](https://aka.ms/securitystandards) com base no Princípio da origem limpa.

## <a name="deploy-paws-using-a-guarded-fabric"></a>Implantar PAWs usando uma Malha Protegida

Uma [malha protegida](https://aka.ms/shieldedvms) pode ser usada para executar cargas de trabalho da PAW em uma máquina virtual blindada em um laptop ou servidor de salto.
A adoção dessa abordagem requer infraestrutura extra e etapas operacionais extra, mas pode facilitar a reimplantação de imagens da PAW em intervalos regulares e permite consolidar várias PAWs em diferentes camadas (ou classificações) em máquinas virtuais em execução lado a lado em um único dispositivo.
Para obter uma explicação completa da topologia e das promessas de segurança da malha protegida, confira a [documentação da malha protegida](https://aka.ms/shieldedvms).

### <a name="changes-to-the-paw-gpos"></a>Alterações nos GPOs da PAW

Ao usar PAWs baseadas em VMs blindadas, as [configurações de GPO recomendadas](#create-paw-configuration---computer-group-policy-object-gpo) definidas acima precisarão ser modificadas para dar suporte ao uso de máquinas virtuais.

1. Crie uma nova UO para os hosts de PAW físicas. PAWs físicas e virtuais têm requisitos de segurança diferentes e devem ser separadas no Active Directory da maneira adequada.
2. O GPO de Computador da PAW deve ser vinculado às UOs da PAW física e virtual.
3. Crie um novo GPO para a PAW física para adicionar os usuários da PAW ao grupo de Administradores do Hyper-V. Isso é necessário para permitir que os administradores se conectem às VMs de administração e as ativem/desativem conforme necessário. É importante que o usuário que faz logon na PAW física não tenha direitos de administrador, acesso à Internet ou a capacidade de copiar dados de máquinas virtuais mal-intencionadas de compartilhamentos de rede ou dispositivos de armazenamento externos para a PAW física.
4. Crie um novo GPO para as VMs de administração para adicionar usuários da PAW ao grupo de Usuários da Área de Trabalho Remota. Isso permitirá que os usuários usem as Sessões de Console Avançadas do Hyper-V, que oferecem uma melhor experiência do usuário e habilitam a passagem do cartão inteligente para a VM.

### <a name="set-up-the-host-guardian-service"></a>Configurar o Serviço Guardião de Host

O Serviço Guardião de Host é responsável por atestar a identidade e a integridade de um dispositivo de PAW física.
Somente os computadores que são conhecidos como HGS e que executam uma [política de integridade de código](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) confiável têm permissão para iniciar as VMs blindadas.
Isso ajuda a proteger as VMs blindadas, que executam cargas de trabalho confiáveis para gerenciar recursos em camadas, contra ameaças do ambiente de área de trabalho do usuário.

Como o HGS é responsável por determinar quais dispositivos podem executar VMs da PAW, ele é considerado um recurso de Camada 0.
Ele deve ser implantado junto outros recursos da Camada 0 e protegido contra acesso físico e lógico não autorizado.
O HGS é uma função clusterizada, facilitando a expansão para implantações de qualquer tamanho.
A regra geral é planejar 1 servidor HGS para cada 1.000 dispositivos que você tem, com um mínimo de 3 nós.

1. Para instalar seu primeiro servidor HGS, comece com o artigo [Instalar o HGS – Floresta do Bastion](../../security/guarded-fabric-shielded-vm/guarded-fabric-install-hgs-in-a-bastion-forest.md) e faça a junção do HGS com seu domínio de Camada 0.

2. Em seguida, [crie certificados para o HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs.md) usando sua autoridade de certificação corporativa.
Qualquer pessoa em posse dos certificados de criptografia e autenticação do HGS pode descriptografar uma VM blindada, portanto, se você tiver acesso a um módulo de segurança de hardware para proteger chaves privadas, é recomendável que você gere esses certificados usando um HSM.
Para ter uma segurança mais sólida, selecione um tamanho de chave maior ou igual a 4096 bits.

3. Por fim, siga as etapas para [inicializar o servidor HGS](../../security/guarded-fabric-shielded-vm/guarded-fabric-initialize-hgs-tpm-mode-bastion.md) no **Modo TPM**.
A inicialização configura os serviços Web de atestado e proteção de chave usados por suas PAWs.
O HGS deve ser [configurado com um certificado TLS](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-https.md) para proteger essas comunicações e somente a porta 443 deve ser aberta de redes não confiáveis para o HGS.

4. Siga as etapas para [adicionar mais nós](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-additional-hgs-nodes.md) para o segundo, o terceiro e os nós adicionais do HGS.

5. Se seu servidor HGS estiver executando o Windows Server 2019 ou posterior, você poderá habilitar um recurso opcional para armazenar em cache as chaves para VMs blindadas nas PAWs para que elas possam ser usadas offline. As chaves são lacradas para a configuração de segurança atual do sistema para impedir que alguém use chaves armazenadas em cache em outro computador ou no mesmo computador em um estado inseguro. Essa poderá ser uma solução útil se os usuários do PAW viajam sem acesso à Internet, mas ainda precisam fazer logon em suas VMs da PAW. Para usar esse recurso, execute o seguinte comando em qualquer servidor HGS:

      ```powershell
      Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
      ```

### <a name="set-up-the-physical-paw-device"></a>Configurar o dispositivo de PAW física

O dispositivo de PAW física é considerado não confiável por padrão na solução de malha protegida.
Ele pode provar que é confiável durante o processo de atestado, após o qual pode obter as chaves necessárias para iniciar uma VM de administrador blindada.
O dispositivo deve ser capaz de executar o Hyper-V e ter a Inicialização Segura e um TPM 2.0 habilitado para atender aos [pré-requisitos de host protegido](../../security/guarded-fabric-shielded-vm/guarded-fabric-guarded-host-prerequisites.md).
A versão mínima do sistema operacional para dar suporte a todas as funcionalidades da PAW é o **Windows 10 versão 1803**.

A PAW física deve ser configurada como qualquer outra, com a exceção de que qualquer usuário da PAW precisará ser Administrador do Hyper-V para poder ativar a VM do administrador e conectar-se a ela.
Em seu ambiente de sala limpa, você precisará criar uma configuração de ouro para cada combinação exclusiva de hardware/software que estiver implantando como hosts protegidos para VMs de administração.
Em cada configuração de ouro, conclua as seguintes tarefas:

1. Instale as atualizações mais recentes para o Windows, drivers e firmwares no computador, bem como agentes de gerenciamento ou monitoramento de terceiros.
2. [Capture as informações de linha de base necessárias](../../security/guarded-fabric-shielded-vm/guarded-fabric-tpm-trusted-attestation-capturing-hardware.md), incluindo o identificador exclusivo do TPM (chave de endosso), as medidas de inicialização (log do TCG) e a política de integridade de código para o computador.
3. Copie esses artefatos para um servidor HGS e execute os comandos de atestado HGS no artigo anterior para registrar o host. Se todos os seus hosts usarem a mesma política de integridade de código e/ou a mesma configuração de hardware, você só precisará registrar a política de integridade de código/log de TCG uma vez.

### <a name="create-the-signed-template-disk"></a>Criar o disco de modelo assinado

As VMs blindadas são criadas usando discos de modelo assinados.
A assinatura é verificada no momento da implantação para verificar a integridade e a autenticidade do disco antes de liberar segredos, como a senha do administrador, para a VM.

Para criar um disco de modelo assinado, siga as etapas de implantação da fase 1 em uma máquina virtual comum de geração 2.
Esta máquina se tornará a imagem de ouro para uma VM de administrador.
Você pode criar mais de um disco de modelo para ter ferramentas especializadas disponíveis em diferentes contextos.

Quando a VM estiver configurada como desejado, execute `C:\Windows\System32\sysprep\sysprep.exe` e opte por **Generalizar** o disco. **Desligue** o sistema operacional quando a generalização for concluída.

Por fim, execute o [Assistente de Disco de Modelo](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-template.md) no arquivo VHDX da VM para instalar os componentes do BitLocker e gerar a assinatura do disco.

#### <a name="create-the-shielding-data-file"></a>Criar o arquivo de dados de blindagem

O disco de modelo generalizado é emparelhado com um arquivo de dados de blindagem, que contém os segredos necessários para provisionar uma VM blindada.
O arquivo de dados de blindagem inclui:
   - Uma lista de guardiões, que definem as malhas em que a VM pode ser executada. Cada cluster HGS é um guardião para os dispositivos PAW que ele protege.
   - Uma lista de assinaturas de disco confiáveis para implantação. Os arquivos de dados de blindagem só liberarão seus segredos para as VMs criadas usando mídias de origem autorizada.
   - Uma política de segurança que determina se proteções extra devem ser colocadas em vigor para proteger a VM do host e se a VM tem permissão para ser movida para outra máquina. As VMs de administrador da PAW sempre devem ser totalmente blindadas.
   - O arquivo de especialização unattend.xml, que permite que o Windows conclua a instalação automaticamente e inclui segredos como a senha do administrador local.
   - Arquivos adicionais, como certificados RDP ou VPN.

Consulte o [artigo sobre o arquivo de dados de blindagem](../../security/guarded-fabric-shielded-vm/guarded-fabric-tenant-creates-shielding-data.md) para obter as etapas de como criar um arquivo de dados de blindagem.

As chaves do proprietário para VMs blindadas são extremamente confidenciais e devem ser mantidas em um HSM ou armazenadas offline em um local seguro.
Elas podem ser usadas em um cenário de interrupção de emergência para inicializar uma VM blindada sem a presença do HGS.

É altamente recomendável que a blindagem de dados para VMs de administração da PAW inclua a configuração para bloquear uma VM para o primeiro host físico em que ela é inicializada.
Isso impedirá que alguém mova uma VM de administrador de um PAW para outra no mesmo ambiente.
Para usar esse recurso, crie o arquivo de dados de blindagem com o PowerShell e inclua o parâmetro **BindToHostTpm**:

```powershell
New-ShieldingDataFile -Policy Shielded -BindToHostTpm [...]
```

#### <a name="deploy-an-admin-vm"></a>Implantar uma VM de administrador

Depois que o disco de modelo e o arquivo de dados de blindagem estiverem prontos, você poderá implantar uma VM de administrador em qualquer PAW que tenha sido registrada no HGS.

1. Copie o disco de modelo (.vhdx) e o arquivo de dados de blindagem (.pdk) para um dispositivo PAW confiável.
2. Siga as instruções para [implantar uma nova VM blindada usando o PowerShell](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-using-powershell.md)
3. Conclua as etapas restantes na fase 1 do processo de implantação para proteger o sistema operacional da VM e configurá-lo para a função pretendida, conforme apropriado.


## <a name="related-topics"></a>Tópicos relacionados

[Usar serviços de segurança cibernética da Microsoft](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx)

[Taste of Premier: como reduzir a Pass-the-Hash e outras formas de roubo de credenciais](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft Advanced Threat Analytics](https://aka.ms/ata)

[Proteger as credenciais de domínio derivadas com o Credential Guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)

[Visão geral do Device Guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)

[Proteger ativos de alto valor com estações de trabalho de administração seguras](https://msdn.microsoft.com/library/mt186538.aspx)

[Modo de Usuário Isolado no Windows 10 com Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Processos e recursos do Modo de Usuário Isolado no Windows 10 com Logan Gabriel (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[Mais sobre processos e recursos no Modo de Usuário Isolado do Windows 10 com Dave Probert (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[Minimizando o roubo de credenciais usando o Modo de Usuário Isolado do Windows 10 (Channel 9)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[Habilitando a validação KDC rigorosa no Windows Kerberos](https://www.microsoft.com/download/details.aspx?id=6382)

[Novidades na autenticação Kerberos para Windows Server 2012](https://technet.microsoft.com/library/hh831747.aspx)

[Guia passo a passo da garantia do mecanismo de autenticação para AD DS no Windows Server 2008 R2](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[Trusted Platform Module](C:/sd/docs/p_ent_keep_secure/p_ent_keep_secure/trusted_platform_module_technology_overview.xml)
