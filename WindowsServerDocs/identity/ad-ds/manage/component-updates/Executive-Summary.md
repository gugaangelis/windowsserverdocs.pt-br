---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Síntese
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3eecec9d47f91bb6a9ba549abc3bf62482b2f49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863277"
---
# <a name="executive-summary"></a>Síntese

>Aplica-se a: Windows Server 2012

>[!IMPORTANT] 
>A documentação a seguir foi escrita em 2013 e é fornecida meramente para fins históricos.  No momento estivermos examinando esta documentação e ela está sujeita a alterações.  Ele pode não refletir as práticas recomendadas atuais.

Nenhuma organização com uma infra-estrutura de TI (tecnologia) de informações é imune a ataques, mas se as políticas adequadas, processos e controles são implementados para proteger segmentos chave da infraestrutura de computação da organização, pode ser possível impedir que um evento de violação aumento atacado comprometer o ambiente de computação.  
  
Este resumo executivo destina-se a ser útil como um documento autônomo resumir o conteúdo do documento, que contém as recomendações que ajudarão as organizações em aprimorar a segurança de suas instalações do Active Directory. Ao implementar essas recomendações, as organizações poderão identificar e priorizar as atividades de segurança, proteger segmentos chave da infraestrutura de computação da organização e criar controles que reduzem significativamente a probabilidade de ataques com êxito em componentes críticos do ambiente de TI.  
  
Embora este documento aborda os ataques mais comuns em relação ao Active Directory e contramedidas para reduzir a superfície de ataque, ele também contém recomendações para a recuperação em caso de comprometimento completo. A única maneira de recuperar em caso de comprometimento completo do Active Directory deve estar preparado para comprometimento antes que elas ocorram.  
  
As seções principais deste documento são:  
  
-   Alternativas ao comprometimento  
  
-   Redução da superfície de ataque do Active Directory  
  
-   Monitorar o Active Directory em busca de sinais de comprometimento  
  
-   Planejar para comprometimento  
  
## <a name="avenues-to-compromise"></a>Alternativas ao comprometimento  
Esta seção fornece informações sobre algumas das vulnerabilidades utilizou mais comumente usadas pelos invasores para comprometer infraestruturas dos clientes. Ele contém categorias gerais de vulnerabilidades e como eles são usados para penetrar em infra-estruturas de clientes, propagar comprometimento entre sistemas adicionais e, eventualmente, Active Directory e controladores de domínio para obter completa de destino controle de florestas de organizações. Ele fornece recomendações detalhadas sobre como tratar cada tipo de vulnerabilidade, especialmente nas áreas em que as vulnerabilidades não são usadas para destinar diretamente o Active Directory. No entanto, para cada tipo de vulnerabilidade, fornecemos links para informações adicionais para usar para desenvolver contramedidas e reduzir a superfície de ataque da organização.  
  
Estão incluídos os seguintes assuntos:  
  
-   **Destinos de violação de inicial** -a maioria das violações de segurança de informações é iniciar com o comprometimento de pequenas partes de uma organização infraestrutura geralmente um ou dois sistemas em um momento. Esses eventos inicias ou pontos de entrada na rede, geralmente exploram vulnerabilidades que poderiam ter sido corrigidas, mas não foram. Vulnerabilidades comuns são:  
  
    -   Lacunas em implantações de antivírus e antimalware  
  
    -   Aplicação de patch incompleta  
  
    -   Sistemas operacionais e aplicativos desatualizados  
  
    -   configuração incorreta  
  
    -   Falta de práticas de desenvolvimento de aplicativo seguro  
  
-   **Contas atraentes para roubo de credenciais** -ataques de roubo de credenciais são aqueles em que um invasor inicialmente obtém acesso privilegiado em um computador em uma rede e, em seguida, usa as ferramentas disponíveis gratuitamente para extrair as credenciais das sessões de outros contas de logon.   
    A seguir estão incluídos nesta seção:  
  
    -   **As atividades que aumentam a probabilidade de comprometimento** – porque o destino de roubo de credenciais é geralmente contas de domínio altamente privilegiadas e "person muito importante" contas (VIP), é importante que os administradores conhecer atividades que aumentam a probabilidade de sucesso de um ataque de roubo de credenciais. Essas atividades são:  
  
        -   Fazendo logon em computadores não protegidos com as contas privilegiadas  
  
        -   Navegando na Internet com uma conta altamente privilegiada  
  
        -   Configurando contas com privilégios locais com as mesmas credenciais em todos os sistemas  
  
        -   Overpopulation e uso excessivo de grupos de domínio com privilégios  
  
        -   Gerenciamento insuficiente da segurança dos controladores de domínio.  
  
    -   **Propagação e elevação de privilégio** -contas específicas, servidores e componentes da infraestrutura geralmente são os principais alvos de ataques no Active Directory. Essas contas são:  
  
        -   Contas privilegiadas permanentemente  
  
        -   Contas de VIP  
  
        -   "Anexado ao privilégio" contas do Active Directory  
  
        -   Controladores de domínio  
  
        -   Outros serviços de infraestrutura que afetam o gerenciamento de identidade, acesso e configuração, como servidores de infraestrutura de chave pública (PKI) e servidores de gerenciamento de sistemas  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Redução da superfície de ataque do Active Directory  
Esta seção se concentra nos controles técnicos para reduzir a superfície de ataque de uma instalação do Active Directory. Incluídos nesta seção são os seguintes assuntos:  
  
-   O **contas privilegiadas e grupos no Active Directory** seção discute as contas com privilégios mais altos e os grupos no Active Directory e os mecanismos pelos quais contas privilegiadas são protegidas. Dentro do Active Directory, os três grupos internos são os grupos de privilégio mais altos no diretório (administradores de empresa, Admins. do domínio e administradores), embora um número de contas e grupos adicionais também deve ser protegido.  
  
-   O **Implementando modelos administrativos com menos privilégios** seção se concentra na identificação de riscos que apresenta o uso de contas com altos privilégios para administração cotidiana, além de fornecer recomendações para Reduza o risco.  
  
Privilégios excessivos apenas não for encontrado no Active Directory em ambientes comprometidos. Quando uma organização desenvolveu o hábito de concedendo mais privilégios do que é necessário, ele é normalmente encontrado em toda a infra-estrutura:  
  
-   No Active Directory  
  
-   Em servidores membros  
  
-   Em estações de trabalho  
  
-   Em aplicativos  
  
-   Em repositórios de dados  
  
-   O **implementar Hosts administrativos seguros** seção descreve hosts administrativos seguros, que são os computadores que estão configurados para dar suporte à administração de sistemas conectados e do Active Directory. Esses hosts são dedicados a funcionalidade administrativa e não executam o software como aplicativos de email, navegadores da web ou software de produtividade (como o Microsoft Office).  
  
A seguir estão incluídos nesta seção:  
  
-   **Princípios para a criação de proteger Hosts administrativos** -são os princípios gerais para ter em mente:  
  
    -   Nunca administrar um sistema confiável de um host de menos confiáveis.  
  
    -   Não confie em um único fator de autenticação ao executar atividades privilegiadas.  
  
    -   Não se esqueça de segurança física ao projetar e implementar hosts administrativos seguros.  
  
-   **Protegendo controladores de domínio contra ataque** -se um usuário mal-intencionado obtiver acesso privilegiado a um controlador de domínio, que o usuário pode modificar, corromper e destruir o banco de dados do Active Directory e, por extensão, todos os sistemas e as contas que são gerenciado pelo Active Directory.  
  
Incluídos nesta seção são os seguintes assuntos:  
  
-   **Segurança física dos controladores de domínio** -contém recomendações para fornecer segurança física para controladores de domínio em locais remotos, filiais e datacenters.  
  
-   **Sistemas operacionais do controlador de domínio** -contém recomendações para proteger sistemas operacionais do controlador de domínio.  
  
-   **Proteger a configuração de controladores de domínio** -configurações e ferramentas de configuração disponível gratuitamente e nativo podem ser usadas para criar linhas de base de configuração para controladores de domínio que posteriormente podem ser impostas pela (objetos de diretiva de grupo de segurança GPOs).  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitorar o Active Directory em busca de sinais de comprometimento  
Esta seção fornece informações sobre categorias de auditoria herdadas e as subcategorias de política de auditoria (que foram introduzidas no Windows Vista e Windows Server 2008) e auditoria de política avançada (que foi introduzido no Windows Server 2008 R2). Também forneceu informações sobre eventos e objetos para monitorar o que podem indicar tentativas de comprometer o ambiente e algumas referências adicionais que podem ser usadas para construir uma política de auditoria abrangente para o Active Directory.  
  
Incluídos nesta seção são os seguintes assuntos:  
  
-   **Política de auditoria do Windows** - logs de eventos de segurança do Windows tenham categorias e subcategorias que determinam quais eventos de segurança são controladas e registradas.  
  
-   **Recomendações de política de auditoria** -esta seção descreve as configurações de política de auditoria do Windows padrão, as configurações de política são recomendadas pela Microsoft e recomendações mais agressivas para as organizações a usar a auditoria de servidores críticos e estações de trabalho.  
  
## <a name="planning-for-compromise"></a>Planejar para comprometimento  
Esta seção contém recomendações que ajudam as organizações a se preparar para um comprometimento antes que elas ocorram, implementar controles que podem detectar um evento de comprometimento antes que ocorreu uma violação completa e fornecem diretrizes de resposta e recuperação para casos em que um comprometimento completo do diretório é obtido por invasores. Incluídos nesta seção são os seguintes assuntos:  
  
-   **Repensando a abordagem** -contém os princípios e diretrizes para criar ambientes seguros, nos quais uma organização pode colocar seus ativos mais importantes. Essas diretrizes são da seguinte maneira:  
  
    -   Identificando os princípios para separar e proteger ativos críticos  
  
    -   Definindo um plano de migração limitado, com base em risco  
  
    -   Aproveitando "nonmigratory" migrações onde for necessário  
  
    -   Implementação de "destruição creative"  
  
    -   Isolando aplicativos e sistemas herdados  
  
    -   Simplificando a segurança para os usuários finais  
  
-   **Manter um ambiente mais seguro** -contém recomendações de alto nível deve ser usado como diretrizes para usar no desenvolvimento de não apenas a segurança em vigor, mas o gerenciamento de ciclo de vida em vigor. Incluídos nesta seção são os seguintes assuntos:  
  
    -   **Criando centrados no negócio práticas de segurança do Active Directory** – para gerenciar o ciclo de vida dos usuários, dados, aplicativos e sistemas gerenciados pelo Active Directory com eficiência, siga esses princípios.  
  
        -   **Atribuir a uma propriedade de negócios a dados do Active Directory** -atribuir a propriedade dos componentes de infraestrutura para IT; para dados que são adicionados ao Active Directory Domain Services (AD DS) para dar suporte aos negócios, por exemplo, novos funcionários, novos aplicativos, e novos repositórios de informações, uma unidade de negócios designado ou o usuário deve ser associado aos dados.  
  
        -   **Implementar o gerenciamento de ciclo de vida de Business-Driven** -gerenciamento de ciclo de vida deve ser implementado para os dados no Active Directory.  
  
        -   **Classificar todos os dados do Active Directory** -os proprietários de negócios devem fornecer classificação de dados no Active Directory. Dentro do modelo de classificação de dados, a classificação para os seguintes dados do Active Directory deve ser incluída:  
  
            -   **Sistemas** -classificar populações de servidor, o sistema operacional sua função, os aplicativos executados no-los e a TI e os proprietários de negócios de registro.  
  
            -   **Aplicativos** -classificar os aplicativos por funcionalidade, da Base de dados de usuário e seu sistema operacional.  
  
            -   **Os usuários** -as contas nas instalações do Active Directory que têm mais probabilidade de ser alvo de invasores, devem ser marcadas e monitoradas.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Resumo de práticas recomendadas para proteger o Active Directory Domain Services  
A tabela a seguir fornece um resumo das recomendações fornecidas neste documento para proteger a instalação do AD DS. Algumas práticas recomendadas são estratégicas por natureza e exigem um planejamento abrangente e projetos de implementação; outros são táticas e se em componentes específicos do Active Directory e a infraestrutura relacionada.  
  
Práticas recomendadas são listadas em ordem aproximada de prioridade, ou seja, números mais baixos indicam a prioridade mais alta. Onde aplicáveis, práticas recomendadas são identificadas como preventivas ou detetive por natureza. Todas essas recomendações devem ser completamente testadas e modificadas conforme necessário para as características e os requisitos da sua organização.  
  
  
||**Prática recomendada**|**Táticas ou estratégicas**|**Preventivas ou detetive**|  
|-|-|-|-|  
|1|Aplicativos de patch.|Táticas|Preventivas|  
|2|Patch em sistemas operacionais.|Táticas|Preventivas|  
|3|Implantar e atualizar imediatamente o software antivírus e antimalware em todos os sistemas e monitorar tentativas de remover ou desabilitá-lo.|Táticas|Ambos|  
|4|Monitorar confidenciais objetos do Active Directory para tentativas de modificação e o Windows para eventos que podem indicar a tentativa de comprometimento.|Táticas|Detetive|  
|5|Proteger e monitorar contas de usuários que têm acesso a dados confidenciais|Táticas|Ambos|  
|6|Impedir que contas potentes que está sendo usado em sistemas não autorizados.|Táticas|Preventivas|  
|7|Elimine associação permanente em grupos altamente privilegiados.|Táticas|Preventivas|  
|8|Implementar controles para conceder associação temporária em grupos privilegiados quando necessário.|Táticas|Preventivas|  
|9|Implemente hosts administrativos seguros.|Táticas|Preventivas|  
|10|Use a lista de permissões de aplicativo em controladores de domínio, hosts administrativos e outros sistemas confidenciais.|Táticas|Preventivas|  
|11|Identificar ativos críticos e priorizar sua segurança e monitoramento.|Táticas|Ambos|  
|12|Implementar controles de acesso baseado em função com menos privilégios para administração de diretório, sua infraestrutura de suporte e sistemas ingressados em domínio.|Estratégicas|Preventivas|  
|13|Isole aplicativos e sistemas herdados.|Táticas|Preventivas|  
|14|Desativar aplicativos e sistemas herdados.|Estratégicas|Preventivas|  
|15|Implemente programas de ciclo de vida de desenvolvimento seguro para aplicativos personalizados.|Estratégicas|Preventivas|  
|16|Implementar o gerenciamento de configuração, examine a conformidade regularmente e avaliar as configurações com cada nova versão de hardware ou software.|Estratégicas|Preventivas|  
|17|Migre ativos críticos para florestas puro com requisitos de monitoramento e de segurança rigorosa.|Estratégicas|Ambos|  
|18|Simplifique a segurança para os usuários finais.|Estratégicas|Preventivas|  
|19|Use firewalls baseados em host para o controle e comunicações seguras.|Táticas|Preventivas|  
|20|Dispositivos de patch.|Táticas|Preventivas|  
|21|Implementar o gerenciamento de ciclo de vida centrados no negócio para ativos de TI.|Estratégicas|N/D|  
|22|Criar ou atualizar planos de recuperação de incidentes.|Estratégicas|N/D|  
  


