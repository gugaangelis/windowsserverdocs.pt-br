---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: Contas atraentes de roubo de credenciais
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae1bfe017e8b21c3abfcdc137153b0fd379053fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="attractive-accounts-for-credential-theft"></a>Contas atraentes de roubo de credenciais

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ataques de roubo de credenciais são aqueles em que um invasor obtiver inicialmente mais alto de privilégio (raiz, o administrador ou sistema, dependendo do sistema operacional em uso) acesso a um computador em uma rede e, em seguida, usa disponível gratuitamente ferramentas para extrair as credenciais de sessões de outras contas de logon. Dependendo da configuração do sistema, essas credenciais podem ser extraídas na forma de hashes, tíquetes ou até mesmo texto sem formatação senhas. Se qualquer uma das credenciais coletadas são para contas locais que têm probabilidade de existir em outros computadores na rede (por exemplo, contas de administrador no Windows ou contas de raiz em OSX, UNIX ou Linux), o invasor apresenta as credenciais para outros computadores na rede para propagar comprometimento em outros computadores e tente obter as credenciais dos dois tipos específicos de contas :  

1.  Contas de domínio privilegiados com privilégios de amplas e profundos (ou seja, as contas que têm privilégios de administrador em vários computadores e no Active Directory). Essas contas não podem ser membros de qualquer um dos grupos de privilégio mais alto no Active Directory, mas eles podem ter recebidos privilégios de nível de administrador em diversos servidores e estações de trabalho no domínio ou floresta, o que torna efetivamente tão potente como membros de grupos privilegiados no Active Directory. Na maioria dos casos, a contas que foram concedidas altos níveis de privilégio entre faixas amplas da infraestrutura do Windows são contas de serviço para que contas de serviço sempre devem ser avaliadas para amplitude e profundidade de privilégio.  

2.  "Pessoa muito importante" contas de domínio (VIP). No contexto deste documento, uma conta de VIP é qualquer conta que tenha acesso às informações que deseja que um invasor (direitos de propriedade intelectual e outras informações confidenciais) ou qualquer conta que pode ser usada para conceder o acesso do invasor a essas informações. Exemplos dessas contas de usuário:  

    1.  Executivos cujas contas têm acesso a informações confidenciais corporativas  

    2.  Contas para a equipe de suporte técnico que é responsável por manter os computadores e aplicativos usados por executivos  

    3.  Contas de membro da equipe legais que tenham acesso à oferta e contrato documentos de uma organização, se os documentos são para sua própria organização ou organizações de cliente  

    4.  Pipeline planejadores de produto que têm acesso às especificações e planos de produtos no desenvolvimento de uma empresa, independentemente dos tipos de produtos que a empresa faz  

    5.  Pesquisadores cujas contas são usadas para acessar dados de estudo, fórmulas de produto ou qualquer outra pesquisa de interesse para um invasor  

Como contas altamente privilegiadas no Active Directory podem ser usadas para propagar comprometimento e manipular VIP contas ou os dados que eles podem acessar, as contas mais úteis para ataques de roubo de credenciais são contas que são membros dos grupos Administradores, Admins. do domínio e administradores de empresa no Active Directory.  

Como controladores de domínio são os repositórios do banco de dados do AD DS e controladores de domínio têm acesso completo a todos os dados no Active Directory, controladores de domínio também são direcionados para comprometimento, se em paralelo com ataques de roubo de credenciais, ou após um ou mais altamente privilegiado do Active Directory contas sejam comprometidas. Embora várias publicações (e muitos invasores) concentram os membros do grupo Admins. do domínio ao descrever os ataques de roubo de credencial pass-the-hash e outras (conforme descrito em [reduzindo a superfície de ataque Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)), uma conta que seja um membro de qualquer um dos grupos listados aqui pode ser usada para comprometer a instalação do AD DS inteira.  

> [!NOTE]  
> Para obter informações completas sobre pass-the-hash e outros ataques de roubo de credenciais, consulte o [ataques Mitigating Pass-the-Hash (PTH) e outras técnicas de roubo de credenciais](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf) whitepaper listado em [apêndice m documento Links e leitura recomendada](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md). Para obter mais informações sobre ataques por determinados adversários, que são, às vezes, conhecido como "ameaças persistentes avançadas" APTs (), consulte [determinados adversários e ataques direcionados](https://www.microsoft.com/download/details.aspx?id=34793).  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>Atividades que aumentam a probabilidade de comprometimento  
Como o destino de roubo de credenciais é geralmente VIP contas e contas de domínio altamente privilegiado, é importante para os administradores sejam consciente de atividades que aumentam a probabilidade de sucesso de um ataque de roubo de credenciais. Embora os invasores também contas-alvo VIP, se VIPs não recebem altos níveis de privilégio em sistemas ou no domínio, o roubo de suas credenciais requer outros tipos de ataques, como o VIP para fornecer informações secretas de engenharia social. Ou, o invasor deve obter primeiro acesso privilegiado para um sistema no qual VIP credenciais são armazenadas em cache. Por isso, as atividades que aumentam a probabilidade de roubo de credenciais descrito aqui concentram-se principalmente sobre como evitar a aquisição de credenciais administrativas altamente privilegiadas. Essas atividades são mecanismos comuns por meio do qual os invasores conseguem comprometer sistemas para obter credenciais com privilégios.  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>Fazer logon em computadores não protegidos com contas privilegiadas  
A vulnerabilidade principal que permite que os ataques de roubo de credenciais ter sucesso é o ato de fazer logon nos computadores que não são seguros com contas que são amplamente e profundamente privilegiado em todo o ambiente. Esses logons pode ser o resultado de várias configurações incorretas descrito aqui.  

#### <a name="not-maintaining-separate-administrative-credentials"></a>Não manter separadas credenciais administrativas  
Embora isso seja muito comum, na avaliação várias instalações do AD DS, descobrimos funcionários de TI usando uma única conta para todos os seus trabalhos. A conta é um membro de pelo menos um dos grupos mais altamente privilegiados no Active Directory e é a mesma conta que os funcionários usam para fazer logon suas estações de trabalho da manhã, verificar seus emails, navegar em sites de Internet e baixar conteúdo em seus computadores. Quando os usuários executam com contas que são concedidas permissões e direitos de administrador local, eles expõem o computador local para concluir o comprometimento. Quando essas contas também são membros dos grupos mais privilegiados no Active Directory, eles expõem toda a floresta de comprometer, tornando muito fácil para um invasor obter controle total do ambiente do Active Directory e do Windows.  

Da mesma forma, em alguns ambientes, descobrimos que os mesmos nomes de usuário e senhas sejam usadas para contas de raiz em computadores que não são do Windows que são usados no ambiente do Windows, que permite que os invasores estender o comprometimento de sistemas UNIX ou Linux para sistemas Windows e vice-versa.

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>Logons comprometidas estações de trabalho ou servidores membro com contas privilegiadas  
Quando uma conta de domínio altamente privilegiado é usada para fazer logon interativamente para uma estação de trabalho comprometida ou um servidor membro, esse computador comprometido pode obter credenciais de qualquer conta que faz logon no sistema.  

#### <a name="unsecured-administrative-workstations"></a>Estações de trabalho administrativas inseguro  
Em muitas organizações, a equipe de TI usa várias contas. Uma conta é usada para fazer logon estação de trabalho do funcionário, e como eles são a equipe de TI, eles geralmente têm direitos de administrador local em suas estações de trabalho. Em alguns casos, o UAC é permanece habilitado para que o usuário recebe pelo menos um acesso de divisão token no logon e deve elevem quando são necessários privilégios. Quando esses usuários estão realizando atividades de manutenção, eles geralmente usar ferramentas de gerenciamento instalados localmente e forneça as credenciais para suas contas de domínio privilégios, selecionando o **executar como administrador** opção ou por fornecer as credenciais quando solicitado. Embora essa configuração pode parecer apropriada, ela expõe o ambiente de comprometer porque:  

-   A conta de usuário "normal" que o funcionário usa para fazer logon suas estações de trabalho com direitos de administrador locais, o computador é vulnerável a [download drive-by](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) ataques em que o usuário está convencido a instalar um malware.  

-   O malware está instalado no contexto de uma conta administrativa, o computador agora pode ser usado para capturar pressionamentos de tecla, conteúdo da área de transferência, capturas de tela e credenciais de residentes na memória, que pode resultar em exposição das credenciais de uma conta de domínio potente.  

Os problemas neste cenário têm dois lados. Primeiro, embora contas separadas são usadas para administração locais e de domínio, o computador é protegido e não protege contra o roubo de contas. Segundo, a conta de usuário normal e a conta administrativa receberam excessivos direitos e permissões.  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>Navegando na Internet com uma conta altamente privilegiada  
Os usuários que fizerem logon em computadores com contas que são membros do grupo Administradores local no computador ou membros dos grupos privilegiados no Active Directory e quem e procure Internet (ou uma intranet comprometida) expõem o computador local e o diretório de comprometer.  

Acessar um site mal-intencionado com um navegador em execução com privilégios administrativos pode permitir que um invasor depositar código malicioso no computador local no contexto do usuário privilegiado. Se o usuário tiver direitos de administrador local no computador, os invasores podem levar o usuário baixando o código mal-intencionado ou abrir anexos de email que aproveitam vulnerabilidades do aplicativo e aproveitam os privilégios do usuário para extrair localmente em cache as credenciais para todos os usuários ativos no computador. Se o usuário tiver direitos administrativos no diretório pela associação ao grupo Administradores corporativos, Admins. do domínio, ou grupos de administradores no Active Directory, o invasor pode extrair as credenciais de domínio e usá-los para comprometer a todo o domínio AD DS ou floresta, sem a necessidade de comprometer a qualquer outro computador na floresta.  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>Configurando contas privilegiadas Local com as mesmas credenciais em sistemas  
Configurando o mesmo nome de conta de administrador local e a senha em vários ou todos os computadores permite que as credenciais roubadas do banco de dados SAM em um computador para ser usados para comprometer todos os outros computadores que usam as mesmas credenciais. No mínimo, você deve usar senhas diferentes para contas de administrador locais em cada sistema de domínio. Contas de administrador local também podem ser chamadas com exclusividade, mas o uso de senhas diferentes para contas locais com privilégios de cada sistema é suficiente para garantir que as credenciais não podem ser usadas em outros sistemas.  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation e o uso excessivo de grupos de domínio privilegiado  
Conceder a associação as grupos EA, DA ou BA em um domínio cria um destino para os invasores. Quanto maior o número de membros desses grupos, maior a probabilidade de que um usuário privilegiado pode inadvertidamente uso indevido as credenciais e expô-los para roubo de credenciais ataques. Cada estação de trabalho ou servidor no qual um usuário de domínio privilegiados efetua logon apresenta um possível mecanismo pelo qual as credenciais do usuário privilegiado podem ser coletadas e usadas para comprometer a floresta e domínio AD DS.  

### <a name="poorly-secured-domain-controllers"></a>Controladores de domínio protegidos mal  
Controladores de domínio colocar uma réplica de banco de dados do domínio AD DS. No caso de controladores de domínio somente leitura, a réplica local do banco de dados contém as credenciais para apenas um subconjunto das contas no diretório, nenhum dos quais são as contas de domínio privilegiados por padrão. Em controladores de domínio de leitura-gravação, cada controlador de domínio mantém uma réplica completa do banco de dados AD DS, incluindo as credenciais não somente para usuários privilegiados como administradores de domínio, mas contas privilegiadas, como contas de controlador de domínio ou conta Krbtgt do domínio, que é a conta que está associada com o KDC serviço em controladores de domínio. Se outros aplicativos que não sejam necessários para a funcionalidade de controlador de domínio são instalados em controladores de domínio, ou se controladores de domínio não são severamente invasores corrigidos e seguros, podem comprometê-los por meio de vulnerabilidades sem patch, ou eles podem aproveitar outros vetores de ataque para instalar um software mal-intencionado diretamente para eles.  

## <a name="privilege-elevation-and-propagation"></a>Propagação e elevação de privilégio  
Independentemente dos métodos de ataque usados, Active Directory sempre é direcionado quando um ambiente do Windows for atacado, porque ele controla o acesso em última análise, para que os invasores desejado. Isso não significa que todo o diretório destina-se, no entanto. Contas específicas, servidores e componentes de infraestrutura geralmente são os principais alvos de ataques contra do Active Directory. Essas contas são descritas a seguir.  

### <a name="permanent-privileged-accounts"></a>Contas privilegiadas permanentes  
Porque a introdução do Active Directory, foi possível uso altamente privilegiado contas para compilar floresta do Active Directory e, em seguida, delegar direitos e permissões necessárias para executar a administração cotidiana às contas com menos privilégios. A associação ao grupo os grupos Administradores, Admins. do domínio ou administradores de empresa no Active Directory é necessária apenas temporariamente e com pouca frequência em um ambiente que implementa abordagens de privilégios mínimos para administração diária.  

Contas privilegiadas permanentes são contas que foram colocadas em grupos privilegiados e lá esquerdas do dia a dia. Caso a organização coloque cinco contas no grupo Admins. do domínio para um domínio, esses cinco contas podem ser direcionadas 24 horas ao dia, 7 dias por semana. No entanto, a necessidade real de usar contas com privilégios de administradores de domínio é normalmente somente para configuração específica de todo o domínio e por períodos curtos.  

### <a name="vip-accounts"></a>Contas VIP  
Um destino de muitas vezes negligenciado no violações do Active Directory é as contas de "pessoas muito importantes" (ou VIPs) em uma organização. Contas privilegiadas são selecionadas porque essas contas podem conceder acesso aos invasores, que permite que eles comprometer ou até mesmo destruir sistemas de destino, conforme descrito anteriormente nesta seção.  

### <a name="privilege-attached-active-directory-accounts"></a>Contas do Active Directory "Anexados privilégio"  
"Anexados privilégio" contas do Active Directory são contas de domínio que não foram feitas membros de qualquer um dos grupos que têm os mais altos níveis de privilégio no Active Directory, mas em vez disso foram concedidos altos níveis de privilégio em vários servidores e estações de trabalho no ambiente. Essas contas são a maioria das contas frequentemente baseado em domínio que estão configurados para executar serviços em sistemas ingressados em domínio, normalmente para aplicativos executados em grandes seções da infraestrutura. Embora essas contas sem privilégios no Active Directory, se eles são concedidos alto privilégio em um número grande de sistemas, eles podem ser usados para comprometer ou até mesmo destruir grandes segmentos da infraestrutura, atingir o mesmo efeito de comprometimento de uma conta do Active Directory privilegiado.  
