---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Resumo executivo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c218a4d66e8208cf627bc93be50bf11ea2fbf862
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="executive-summary"></a>Resumo executivo

>Aplica-se a: Windows Server 2012

>[!IMPORTANT] 
>A documentação a seguir foi escrita em 2013 e é fornecida apenas para fins históricos.  Atualmente, podemos examinar esta documentação e ele está sujeito a alterações.  Ele pode não refletir as práticas recomendadas atuais.

Nenhuma organização com uma infraestrutura de informática informações é imune contra ataques, mas se os controles, processos e políticas apropriadas estão implementados para proteger chaves segmentos de infraestrutura de computação da organização, talvez seja possível para evitar que um evento de violação crescendo atacado comprometer o ambiente de computação.  
  
Esse resumo executivo destina-se para ser útil como um documento autônomo resumindo o conteúdo do documento, que contém recomendações que podem ajudar as organizações em aprimora a segurança de suas instalações do Active Directory. Ao implementar essas recomendações, as organizações poderão identificar e priorizar as atividades de segurança, proteger chaves segmentos de infraestrutura de computação da organização e criar controles que reduzem a probabilidade de ataques bem-sucedidos contra componentes essenciais do ambiente de TI significativamente.  
  
Embora este documento discute os ataques contra o Active Directory e contramedidas para reduzir a superfície de ataque mais comuns, ele também contém recomendações para a recuperação no caso de comprometimento completo. A maneira somente se a recuperação caso haja algum comprometimento completo do Active Directory é estar preparados para o comprometimento antes que ela ocorra.  
  
As principais seções deste documento são:  
  
-   Caminhos para comprometer  
  
-   Reduzir a superfície de ataque do Active Directory  
  
-   O monitoramento do Active Directory de sinais de comprometimento  
  
-   Planejamento de comprometimento  
  
## <a name="avenues-to-compromise"></a>Caminhos para comprometer  
Esta seção fornece informações sobre algumas das vulnerabilidades utilizou mais comumente usadas por invasores para comprometer infraestruturas dos clientes. Ele contém categorias gerais de vulnerabilidades e como eles são usados para inicialmente penetrar infraestruturas dos clientes, propagar comprometimento em sistemas adicionais e eventualmente destinados ao Active Directory e controladores de domínio para obter controle completo de florestas das organizações. Ele não fornece recomendações detalhadas sobre como lidar com cada tipo de vulnerabilidade, principalmente nas áreas em que as vulnerabilidades não são usadas para direcionar diretamente do Active Directory. No entanto, para cada tipo de vulnerabilidade, fornecemos links para informações adicionais para usar para desenvolver contramedidas e reduzir a superfície de ataque da organização.  
  
Incluídas estão as seguintes assuntos:  
  
-   **Inicial destinos de violação** -a maioria das violações de segurança das informações começar com o comprometimento de pequenas partes de uma organização infraestrutura geralmente um ou dois sistemas de cada vez. Esses eventos iniciais ou pontos de entrada à rede, muitas vezes exploram vulnerabilidades que poderiam ter foi corrigidas, mas não foram. Vulnerabilidades comuns são:  
  
    -   Lacunas em implantações de antivírus e antimalware  
  
    -   Aplicação de patch incompleta  
  
    -   Sistemas operacionais e aplicativos desatualizados  
  
    -   Definição incorreta  
  
    -   Falta de práticas de desenvolvimento de aplicativos seguros  
  
-   **Contas atraentes de roubo de credenciais** -ataques de roubo de credenciais são aqueles em que um invasor inicialmente obtém acesso privilegiado para um computador em uma rede e, em seguida, usa ferramentas disponíveis gratuitamente para extrair as credenciais de sessões de outras contas de logon.   
    Incluído nesta seção são as seguintes:  
  
    -   **Atividades que aumentam a probabilidade de comprometimento** - como o destino de roubo de credenciais é geralmente contas de domínio altamente privilegiado e "pessoa muito importante" contas (VIP), é importante que os administradores sejam consciente de atividades que aumentam a probabilidade de sucesso de um ataque de roubo de credenciais. Essas atividades são:  
  
        -   Fazer logon em computadores não protegidos com contas privilegiadas  
  
        -   Navegando na Internet com uma conta altamente privilegiada  
  
        -   Configurando contas privilegiadas locais com as mesmas credenciais em sistemas  
  
        -   Overpopulation e o uso excessivo de grupos de domínio privilegiado  
  
        -   Gerenciamento insuficiente da segurança dos controladores de domínio.  
  
    -   **Elevação e propagação de privilégio** -contas específicas, servidores e componentes de infraestrutura geralmente são os principais alvos de ataques contra do Active Directory. Essas contas são:  
  
        -   Contas privilegiadas permanentemente  
  
        -   Contas VIP  
  
        -   "Anexados privilégio" contas do Active Directory  
  
        -   Controladores de domínio  
  
        -   Outros serviços de infraestrutura que afetam o gerenciamento de configuração, acesso e identidade, como servidores de infraestrutura de chave pública (PKI) e servidores de gerenciamento de sistemas  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Reduzir a superfície de ataque do Active Directory  
Esta seção se concentra em controles técnicos para reduzir a superfície de ataque de uma instalação do Active Directory. Incluído nesta seção são os seguintes assuntos:  
  
-   O **privilegiados contas e grupos no Active Directory** seção discute os contas privilegiadas mais e os grupos no Active Directory e os mecanismos que contas privilegiadas estão protegidas. No Active Directory, três grupos internos são os grupos de privilégio mais altos na pasta (administradores corporativos, Admins. do domínio e administradores), embora um número de contas e grupos adicionais também deve ser protegido.  
  
-   O **Implementando modelos administrativos de privilégios mínimos** seção concentra-se nos identificando o risco de que o uso de altamente privilegiado contas para administração cotidiana apresenta, além para fornecer recomendações para reduzir esse risco.  
  
Privilégios excessivos apenas não for encontrado no Active Directory em ambientes comprometidos. Quando uma organização desenvolveu o hábito de concessão mais privilégios do que é necessário, geralmente é encontrada em toda a infraestrutura:  
  
-   No Active Directory  
  
-   Em servidores membro  
  
-   Em estações de trabalho  
  
-   Em aplicativos  
  
-   Em repositórios de dados  
  
-   O **implementando seguro Hosts administrativas** seção descreve seguros hosts administrativos, que são os computadores que estão configurados para dar suporte a administração do Active Directory e sistemas conectados. Esses hosts são dedicados a funcionalidade administrativa e não executar software, como aplicativos de email, navegadores da web ou software de produtividade (como o Microsoft Office).  
  
Incluído nesta seção são as seguintes:  
  
-   **Princípios para criar segura Hosts administrativas** -são os princípios gerais a serem lembrados:  
  
    -   Nunca administrar um sistema confiável de um host menos confiável.  
  
    -   Não confie em um fator de autenticação único ao realizar atividades privilegiadas.  
  
    -   Não se esqueça de segurança física ao projetar e implementar hosts administrativos seguros.  
  
-   **Protegendo controladores de domínio contra ataques** -se um usuário mal-intencionado obtém acesso privilegiado para um controlador de domínio, se o usuário pode modificar, corromper e destruir o banco de dados do Active Directory e por extensão, todos os sistemas e contas que são gerenciadas pelo Active Directory.  
  
Incluído nesta seção são os seguintes assuntos:  
  
-   **Segurança física para controladores de domínio** -contém recomendações para fornecer segurança física para controladores de domínio em data centers, filiais e locais remotos.  
  
-   **Sistemas de operacionais do controlador de domínio** -contém recomendações para proteger sistemas operacionais do controlador de domínio.  
  
-   **Configuração de controladores de domínio seguros** -ferramentas de configuração nativa e disponível gratuitamente e as configurações podem ser usadas para criar linhas de base de configuração para controladores de domínio que posteriormente podem ser aplicadas por objetos de política de grupo (GPOs) de segurança.  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>O monitoramento do Active Directory de sinais de comprometimento  
Esta seção fornece informações sobre auditoria herdados categorias e subcategorias de política de auditoria (que foram introduzidas no Windows Vista e Windows Server 2008) e auditoria política avançada (que foi introduzida no Windows Server 2008 R2). Também fornecido é informações sobre eventos e objetos para monitorar que podem indicar tentativas de comprometer o ambiente e alguns referências adicionais que podem ser usadas para construir uma política de auditoria abrangente para o Active Directory.  
  
Incluído nesta seção são os seguintes assuntos:  
  
-   **Política de auditoria do Windows** - logs de eventos de segurança do Windows têm categorias e subcategorias que determinam quais eventos de segurança são controladas e gravadas.  
  
-   **Recomendações de política de auditoria** -esta seção descreve as configurações de política de auditoria do Windows padrão, as configurações de política são recomendadas pela Microsoft e recomendações mais agressivas para organizações para usar a auditoria críticos servidores e estações de trabalho de auditoria.  
  
## <a name="planning-for-compromise"></a>Planejamento de comprometimento  
Esta seção contém recomendações que ajudam as organizações a preparar para um comprometimento antes que ela ocorra, implemente controles que podem detectar um evento de comprometimento antes que ocorreu uma violação inteira e fornecem diretrizes de resposta e recuperação para casos em que um comprometimento completo do diretório é obtido por invasores. Incluído nesta seção são os seguintes assuntos:  
  
-   **Repensando a abordagem** -contém os princípios e as diretrizes para criar ambientes seguros no qual uma organização pode colocar os ativos mais críticos. Estas diretrizes são as seguintes:  
  
    -   Identificando princípios para separar e proteger os ativos críticos  
  
    -   Definindo um plano de migração limitado, com base em risco  
  
    -   Aproveitando migrações "nonmigratory" onde necessário  
  
    -   Implementando "destruição criativa"  
  
    -   Isolando aplicativos e sistemas herdados  
  
    -   Simplificando a segurança para os usuários finais  
  
-   **Manter um ambiente mais seguro** -contém recomendações de alto nível para ser usado como diretrizes para usar em desenvolvendo não só de segurança efetiva, mas o gerenciamento de ciclo de vida eficaz. Incluído nesta seção são os seguintes assuntos:  
  
    -   **Criando centradas em empresas práticas de segurança para o Active Directory** - para efetivamente gerenciar o ciclo de vida dos usuários, dados, aplicativos e sistemas gerenciados pelo Active Directory, siga estes princípios.  
  
        -   **Atribua uma propriedade de negócios para dados do Active Directory** -atribuir a propriedade de componentes de infraestrutura para IT; para dados que são adicionados para serviços de domínio Active Directory (AD DS) para dar suporte aos negócios, por exemplo, novos funcionários, novos aplicativos e novos repositórios de informações, uma unidade de negócios designado ou o usuário deve ser associado aos dados.  
  
        -   **Implementar o gerenciamento de ciclo de vida de Business-Driven** -gerenciamento de ciclo de vida deve ser implementado para dados no Active Directory.  
  
        -   **Classificar todos os dados do Active Directory** -empresários devem fornecer a classificação dos dados no Active Directory. No modelo de classificação de dados, a classificação para os seguintes dados do Active Directory deve ser incluída:  
  
            -   **Sistemas** -classificar população de servidor, o sistema operacional suas funções, os aplicativos em execução no-los e o ele e empresários de registro.  
  
            -   **Aplicativos** -classificar aplicativos pela funcionalidade, base de usuários e seu sistema operacional.  
  
            -   **Os usuários** -as contas em instalações do Active Directory que têm mais probabilidade de ser atacado por invasores devem ser marcadas e monitoradas.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Resumo das práticas recomendadas para proteger os serviços de domínio do Active Directory  
A tabela a seguir fornece um resumo das recomendações fornecidos neste documento para assegurar uma instalação do AD DS. Algumas práticas recomendadas são estratégicas por natureza e exigem planejamento abrangente e projetos de implementação; outros são táticos e focada em componentes específicos do Active Directory e infraestrutura relacionada.  
  
Práticas recomendadas são listadas na ordem aproximada de prioridade, ou seja, números menores indicam prioridade mais alta. Onde aplicáveis, melhores práticas são identificadas como preventivas ou detetive por natureza. Todas essas recomendações devem ser completamente testadas e modificadas conforme necessário para características e os requisitos da sua organização.  
  
  
|-|-|-|-|  
||**Prática recomendada**|**táticos ou estratégica**|**preventivas ou detetive**|  
| 1 | Aplicativos de patch. | Táticos | Preventivas |  
| 2 | Sistemas operacionais de patch. | Táticos | Preventivas |  
| 3 | Implantar e atualizar imediatamente o software antivírus e antimalware em todos os sistemas e monitorar tentativas remover ou desativá-lo. | Táticos | Ambos |  
| 4 | Monitorar confidenciais objetos do Active Directory para tentativas de modificação e o Windows para eventos que possam indicar a tentativa de comprometimento. | Táticos | Detetive |  
| 5 | Proteger e monitorar contas para os usuários que têm acesso a dados confidenciais | Táticos | Ambos |  
| 6 | Impedir que contas potentes sendo usada em sistemas não autorizados. | Táticos | Preventivas |  
| 7 | Elimine permanente participação em grupos altamente privilegiados. | Táticos | Preventivas |  
| 8 | Implementar controles para conceder temporária participação em grupos privilegiados quando necessário. | Táticos | Preventivas |  
| 9 | Implementar hosts administrativos seguros. | Táticos | Preventivas |  
| 10 | Use a lista de exceções do aplicativo em controladores de domínio, hosts administrativos e outros sistemas confidenciais. | Táticos | Preventivas |  
| 11 | Identificar os ativos críticos e priorizar sua segurança e monitoramento. | Táticos | Ambos |  
| 12 | Implementar controles de acesso de privilégios mínimos, baseada em função de administração de diretório, infraestrutura de apoio de TI e sistemas ingressados em domínio. | Estratégica | Preventivas |  
| 13 | Isolar sistemas herdados e aplicativos. | Táticos | Preventivas |  
| 14 | Desativação de aplicativos e sistemas herdados. | Estratégica | Preventivas |  
| 15 | Implemente desenvolvimento seguro de programas de ciclo de vida para aplicativos personalizados. | Estratégica | Preventivas |  
| 16 | Implementar o gerenciamento de configuração, examine conformidade regularmente e avaliar configurações com cada nova versão de hardware ou software. | Estratégica | Preventivas |  
| 17 | Migrar os ativos críticos para florestas impecáveis com segurança rigorosas e requisitos de monitoramento. | Estratégica | Ambos |  
| 18 | Simplifique a segurança para os usuários finais. | Estratégica | Preventivas |  
| 19 | Use firewalls com base em host para o controle e comunicação segura. | Táticos | Preventivas |  
| 20 | Patch de dispositivos. | Táticos | Preventivas |  
| 21 | Implementar o gerenciamento de ciclo de vida centradas em empresas para ativos de TI. | Estratégica | N/D |  
| 22 | Criar ou atualizar os planos de recuperação incidente. | Estratégica | N/D |  
  


