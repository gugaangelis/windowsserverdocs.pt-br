---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: Contas atraentes para roubo de credenciais
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e5d2b01f73127f84f700dab3518b66687f0294f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863097"
---
# <a name="attractive-accounts-for-credential-theft"></a>Contas atraentes para roubo de credenciais

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ataques de roubo de credenciais são aqueles em que um invasor inicialmente obtém mais alto de privilégio (raiz, administrador ou sistema, dependendo do sistema operacional em uso) acesso a um computador em uma rede e, em seguida, usa livremente as ferramentas para extrair as credenciais disponíveis das sessões de outras contas de logon. Dependendo da configuração do sistema, essas credenciais podem ser extraídas na forma de hashes, tíquetes ou até mesmo texto sem formatação senhas. Se qualquer uma das credenciais coletadas são para contas locais que podem existir em outros computadores na rede (por exemplo, contas de administrador no Windows ou contas de raiz no OSX, UNIX ou Linux), o invasor apresenta as credenciais para outros computadores em a rede para propagar o comprometimento em computadores adicionais e para tentar obter as credenciais de dois tipos específicos de contas:  

1.  Contas de domínio com privilégios com amplas e profundos privilégios (ou seja, contas que têm privilégios de administrador em vários computadores e no Active Directory). Essas contas não podem ser membros de qualquer um dos grupos de privilégio mais alto no Active Directory, mas eles podem ter privilégio de nível de administrador entre vários servidores e estações de trabalho no domínio ou floresta, tornando-os efetivamente tão poderoso quanto membros de grupos privilegiados no Active Directory. Na maioria dos casos, as contas que receberam altos níveis de privilégio em amplas faixas de infraestrutura do Windows são contas de serviço para que as contas de serviço sempre devem ser avaliadas para a amplitude e profundidade de privilégio.  

2.  "Muito importante" Person"contas de domínio (VIP). No contexto deste documento, uma conta de VIP é qualquer conta que tenha acesso às informações que deseja que um invasor (propriedade intelectual e outras informações confidenciais) ou qualquer conta que pode ser usada para conceder o invasor acesse essas informações. Exemplos dessas contas de usuário:  

    1.  Executivos cujas contas têm acesso a informações corporativas confidenciais  

    2.  Contas para ajudar a equipe de suporte técnico que são responsável por manter os computadores e aplicativos usados pelos executivos  

    3.  Contas para as equipes jurídica que têm acesso a documentos da organização bid e contrato, se os documentos são para sua própria organização ou organizações do cliente  

    4.  Planejadores de produtos que têm acesso às especificações e planos para produtos no desenvolvimento de uma empresa de pipeline, independentemente dos tipos de produtos que faz com que a empresa  

    5.  Pesquisadores cujas contas são usadas para acessar dados de estudo, formulações de produto ou qualquer outra pesquisa de interesse de um invasor  

Como contas altamente privilegiadas no Active Directory podem ser usadas para propagar o comprometimento e manipular contas VIP ou os dados que eles podem acessar, as contas mais úteis para ataques de roubo de credenciais são contas que são membros de administradores de empresa Admins. do domínio e administradores de grupos no Active Directory.  

Como controladores de domínio são os repositórios para o banco de dados do AD DS e controladores de domínio têm acesso completo a todos os dados no Active Directory, controladores de domínio também são direcionados para comprometimento, seja em paralelo com ataques de roubo de credenciais ou após uma ou mais contas altamente privilegiadas do Active Directory tem sido comprometidas. Embora várias publicações (e vários invasores) enfocar os membros do grupo Admins. do domínio ao descrever os ataques de roubo de credenciais pass-the-hash e outros (conforme descrito em [reduzindo a superfície de ataque do Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)) , uma conta que seja um membro de qualquer um dos grupos listados aqui pode ser usada para comprometer toda a instalação do AD DS.  

> [!NOTE]  
> Para obter informações abrangentes sobre pass-the-hash e outros ataques de roubo de credenciais, consulte o [ataques Mitigating Pass-the-Hash (PTH) e outras técnicas de roubo de credencial](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf) white paper listado no [Apêndice M : Links de documentos e leitura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md). Para obter mais informações sobre ataques de adversários determinados, que é chamado de "ameaças persistentes avançadas" (APTs), consulte [adversários determinados e ataques direcionados](https://www.microsoft.com/download/details.aspx?id=34793).  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>Atividades que aumentam a probabilidade de comprometimento  
Como o destino de roubo de credenciais é geralmente contas de domínio altamente privilegiados e contas de VIP, é importante para os administradores de estar consciente das atividades que aumentam a probabilidade de sucesso de um ataque de roubo de credenciais. Embora os invasores também VIP contas de destinam, se VIPs não recebem altos níveis de privilégio em sistemas ou no domínio, o roubo de suas credenciais requer outros tipos de ataques, como o VIP para fornecer informações secretas de engenharia social. Ou, o invasor deve primeiro obter acesso privilegiado em um sistema no qual VIP credenciais são armazenadas em cache. Por isso, as atividades que aumentam a probabilidade de roubo de credenciais descrito aqui são voltadas principalmente impedindo a aquisição de credenciais administrativas altamente privilegiadas. Essas atividades são mecanismos comuns pelos quais os invasores são capazes de comprometer sistemas para obter as credenciais com privilégios.  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>Fazendo logon em computadores não protegidos com as contas privilegiadas  
A vulnerabilidade de núcleo que permite que os ataques de roubo de credencial seja bem-sucedida é o ato de fazer logon em computadores que não são seguros com as contas que são amplamente e profundamente com privilégios em todo o ambiente. Esses logons pode ser o resultado de várias configurações incorretas descrito aqui.  

#### <a name="not-maintaining-separate-administrative-credentials"></a>Não manter credenciais administrativas separadas  
Embora isso seja relativamente incomum, avaliar várias instalações do AD DS, descobrimos os funcionários de TI usando uma única conta para todos os seus trabalhos. A conta é membro de pelo menos um dos grupos no Active Directory mais altamente privilegiados e é a mesma conta que os funcionários usam para fazer logon suas estações de trabalho pela manhã, verificar seus emails, procurar sites da Internet e baixar conteúda para seus computadores. Quando os usuários executam com contas que são concedidas permissões e direitos de administrador local, elas expõem o computador local para concluir o comprometimento. Quando essas contas também são membros dos grupos no Active Directory mais privilegiados, elas expõem toda a floresta comprometimento, tornando muito mais fácil para um invasor obtenha controle total do ambiente do Active Directory e o Windows.  

Da mesma forma, em alguns ambientes, descobrimos que os mesmos nomes de usuário e senhas são usadas para contas de raiz em computadores não Windows pois são usados no ambiente do Windows, que permite que os invasores estender o comprometimento de sistemas UNIX ou Linux para sistemas Windows e vice-versa.

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>Logons em estações de trabalho comprometidas ou servidores de membro com contas privilegiadas  
Quando uma conta de domínio altamente privilegiado é usada para fazer logon interativamente em um servidor de membro ou estação de trabalho comprometida, esse computador comprometido pode coletar credenciais de qualquer conta que faça logon no sistema.  

#### <a name="unsecured-administrative-workstations"></a>Estações de trabalho administrativas não seguras  
Em muitas organizações, a equipe de TI usa várias contas. Uma conta é usada para fazer logon da estação de trabalho do funcionário e como eles são a equipe de TI, eles geralmente têm direitos de administrador local em suas estações de trabalho. Em alguns casos, o UAC é esquerda habilitada para que o usuário receber pelo menos um acesso de divisão de token no logon e devem elevar quando são necessários privilégios. Quando esses usuários estão executando as atividades de manutenção, normalmente usam as ferramentas de gerenciamento instalado localmente e forneça as credenciais para suas contas de domínio com privilégios, selecionando o **executar como administrador** opção ou pelo fornecer as credenciais quando solicitado. Embora essa configuração pode parecer apropriada, ela apresenta o ambiente de serem comprometidos porque:  

-   A conta de usuário "normal" que o funcionário usa para fazer logon sua estação de trabalho tem direitos de administrador local, o computador estará vulnerável a [download drive-by](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) ataques em que o usuário é convencido instalar malware.  

-   O malware está instalado no contexto de uma conta administrativa, o computador agora pode ser usado para capturar os pressionamentos de tecla, conteúdo da área de transferência, capturas de tela e credenciais de residentes na memória, qualquer uma delas pode resultar em exposição das credenciais de um domínio poderosa conta.  

Os problemas neste cenário são duplos. Em primeiro lugar, embora contas separadas são usadas para administração local e de domínio, o computador não for seguro e não protege contra roubo de contas. Em segundo lugar, a conta de usuário regular e a conta administrativa receberam permissões e direitos em excesso.  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>Navegando na Internet com uma conta altamente privilegiada  
Os usuários que fizerem logon em computadores com as contas que são membros do grupo Administradores local no computador, ou membros de grupos privilegiados no Active Directory e que, em seguida, navegue na Internet (ou uma intranet comprometida) expõem o computador local e o diretório de comprometimento.  

Acesso a um site mal-intencionado com um navegador em execução com privilégios administrativos pode permitir que um invasor depositar o código mal-intencionado no computador local no contexto do usuário com privilégios. Se o usuário tem direitos de administrador local no computador, os invasores podem levar o usuário fazer o download de código mal-intencionado ou abrir anexos de email que aproveitam as vulnerabilidades de aplicativos e aproveitam os privilégios do usuário para extrair armazenada em cache localmente credenciais para todos os usuários ativos no computador. Se o usuário tem direitos administrativos no diretório pela associação na administradores de empresa, Admins. do domínio ou grupos de administradores no Active Directory, o invasor pode extrair as credenciais de domínio e usá-los para comprometer a floresta, ou a todo o domínio de AD DS sem a necessidade de comprometer a qualquer outro computador na floresta.  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>Configurando contas com privilégios locais com as mesmas credenciais em todos os sistemas  
Configurando o mesmo nome de conta de administrador local e a senha em muitas ou todas as credenciais permite que computadores roubadas do banco de dados SAM em um computador para ser usado para comprometer todos os outros computadores que usam as mesmas credenciais. No mínimo, você deve usar senhas diferentes para contas de administrador locais em cada sistema ingressado no domínio. Contas de administrador local também podem ser nomeadas exclusivamente, mas o uso de senhas diferentes para contas com privilégios de local de cada sistema é suficiente para garantir que as credenciais não podem ser usadas em outros sistemas.  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation e uso excessivo de grupos de domínio com privilégios  
Conceder a associação nos grupos do EA, acelerador de desenvolvimento ou BA em um domínio cria um destino para os invasores. Quanto maior o número de membros desses grupos, maior a probabilidade de um usuário com privilégios pode usar indevidamente as credenciais e expô-los para roubo de credenciais inadvertidamente os ataques. Cada estação de trabalho ou o servidor no qual um usuário de domínio com privilégios efetua logon apresenta um possível mecanismo pelo qual as credenciais do usuário com privilégios podem ser coletadas e usadas para comprometer o domínio do AD DS e a floresta.  

### <a name="poorly-secured-domain-controllers"></a>Controladores de domínio fraca  
Controladores de domínio hospedam uma réplica de banco de dados do domínio AD DS. No caso de controladores de domínio somente leitura, a réplica local do banco de dados contém as credenciais para apenas um subconjunto das contas no diretório, nenhum dos quais são as contas de domínio com privilégios por padrão. Em controladores de domínio de leitura / gravação, cada controlador de domínio mantém uma réplica completa do banco de dados AD DS, incluindo as credenciais não apenas para usuários privilegiados como Admins. do domínio, mas as contas privilegiadas, como contas de controlador de domínio ou Krbtgt de domínio conta, que é a conta que está associada com o serviço do KDC em controladores de domínio. Se aplicativos adicionais que não são necessários para a funcionalidade de controlador de domínio são instalados em controladores de domínio, ou se os controladores de domínio não forma rigorosa sido corrigidos e protegidos, os invasores podem comprometê-los por meio de vulnerabilidades sem patch ou eles pode aproveitar outros vetores de ataque para instalar o software mal-intencionado diretamente neles.  

## <a name="privilege-elevation-and-propagation"></a>Propagação e elevação de privilégio  
Os métodos de ataque usados, independentemente do Active Directory é destinado sempre quando um ambiente do Windows for atacado, porque, por fim, ele controla o acesso a tudo o que os invasores deseja que. Isso não significa que todo o diretório é afetado, no entanto. Contas específicas, servidores e componentes da infraestrutura geralmente são os principais alvos de ataques no Active Directory. Essas contas são descritas da seguinte maneira.  

### <a name="permanent-privileged-accounts"></a>Contas com privilégios permanentes  
Porque a introdução do Active Directory, era possível às contas altamente privilegiada de uso para criar a floresta do Active Directory e, em seguida, para delegar direitos e permissões necessárias para executar a administração cotidiana para contas com menos privilégios. Associação nos grupos de administradores, Admins. do domínio ou administradores de empresa no Active Directory é necessária apenas temporariamente e com pouca frequência em um ambiente que implementa abordagens com menos privilégios para administração diária.  

Contas com privilégios permanentes são contas que foram colocadas em grupos privilegiados e deixadas lá no dia a dia. Se sua organização colocar cinco contas no grupo Admins. do domínio para um domínio, as cinco contas podem ser alvo de 24 horas por dia, sete dias por semana. No entanto, a necessidade real de usar contas com privilégios de administradores de domínio normalmente é apenas para configuração de todo o domínio específica e por curtos períodos de tempo.  

### <a name="vip-accounts"></a>Contas de VIP  
Um destino que é frequentemente desconsiderado em violações de Active Directory é as contas de "pessoas muito importantes" (ou VIPs) em uma organização. Contas privilegiadas são alvo, pois essas contas podem conceder acesso para invasores, que permite que eles comprometer ou destruir até mesmo os sistemas de destino, conforme descrito anteriormente nesta seção.  

### <a name="privilege-attached-active-directory-accounts"></a>Contas do Active Directory "Anexado ao privilégio"  
"Anexado ao privilégio" contas do Active Directory são contas de domínio que não foram feitas membros de qualquer um dos grupos que têm os mais altos níveis de privilégio no Active Directory, mas em vez disso, ter altos níveis de privilégio em vários servidores e estações de trabalho no ambiente. Essas contas são a maioria das contas geralmente baseados em domínio que são configurados para executar serviços em sistemas ingressados em domínio, normalmente para aplicativos executados em grandes seções da infraestrutura. Embora essas contas não têm privilégios no Active Directory, se elas forem concedidas com privilégios elevados em um grande número de sistemas, pode ser usados para comprometer ou destruir até mesmo grandes segmentos da infraestrutura de alcançar o mesmo efeito que o comprometimento de um conta do Active Directory com privilégios.  
