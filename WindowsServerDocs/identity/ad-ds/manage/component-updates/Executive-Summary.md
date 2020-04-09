---
ms.assetid: 85ca191c-0cc7-4453-a72c-42060ddf2ea2
title: Síntese
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 75b98e3f8078f33098512c8ecd01d3bb49a1c8ed
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823039"
---
# <a name="executive-summary"></a>Síntese

>Aplica-se a: Windows Server 2012

>[!IMPORTANT] 
>A documentação a seguir foi escrita em 2013 e é fornecida apenas para fins de histórico.  No momento, estamos analisando esta documentação e ela está sujeita a alterações.  Ele pode não refletir as práticas recomendadas atuais.

Nenhuma organização com uma infra-estrutura de ti (tecnologia da informação) está imune contra ataques, mas se as políticas, os processos e os controles apropriados forem implementados para proteger os principais segmentos da infraestrutura de computação de uma organização, talvez seja possível evitar que um evento de violação aumente para um comprometimento atacadista do ambiente computacional.  
  
Este resumo executivo destina-se a ser útil como um documento autônomo resumindo o conteúdo do documento, que contém recomendações que ajudarão as organizações a aprimorar a segurança de suas instalações de Active Directory. Ao implementar essas recomendações, as organizações poderão identificar e priorizar as atividades de segurança, proteger os principais segmentos da infraestrutura de computação da organização e criar controles que diminuem significativamente a probabilidade de ataques bem-sucedidos contra os componentes críticos do ambiente de ti.  
  
Embora este documento discuta os ataques mais comuns contra Active Directory e contramedidas para reduzir a superfície de ataque, ele também contém recomendações para recuperação no caso de um comprometimento completo. A única maneira de se recuperar no caso de um comprometimento completo do Active Directory é estar preparado para o comprometimento antes de acontecer.  
  
As principais seções deste documento são:  
  
-   Alternativas ao comprometimento  
  
-   Redução da superfície de ataque do Active Directory  
  
-   Monitorar o Active Directory em busca de sinais de comprometimento  
  
-   Planejar para comprometimento  
  
## <a name="avenues-to-compromise"></a>Alternativas ao comprometimento  
Esta seção fornece informações sobre algumas das vulnerabilidades mais utilizadas usadas pelos invasores para comprometer as infraestruturas dos clientes. Ele contém categorias gerais de vulnerabilidades e como elas são usadas para penetrar inicialmente nas infraestruturas dos clientes, propagar o comprometimento entre sistemas adicionais e, eventualmente, direcionar Active Directory e controladores de domínio para obter o controle total das florestas das organizações. Ele não fornece recomendações detalhadas sobre como abordar cada tipo de vulnerabilidade, particularmente nas áreas em que as vulnerabilidades não são usadas para direcionar diretamente Active Directory. No entanto, para cada tipo de vulnerabilidade, fornecemos links para informações adicionais a serem usadas para desenvolver medidas defensivas e reduzir a superfície de ataque da organização.  
  
Estão incluídos os seguintes assuntos:  
  
-   **Destinos de violação iniciais** -a maioria das violações de segurança de informações começa com o comprometimento de pequenas partes da infraestrutura de uma organização – geralmente um ou dois sistemas de cada vez. Esses eventos iniciais, ou pontos de entrada na rede, geralmente exploram vulnerabilidades que poderiam ter sido corrigidas, mas não foram. As vulnerabilidades comumente vistas são:  
  
    -   Falhas em implantações de antivírus e antimalware  
  
    -   Aplicação de patch incompleta  
  
    -   Aplicativos e sistemas operacionais desatualizados  
  
    -   Configuração incorreta  
  
    -   Falta de práticas seguras de desenvolvimento de aplicativos  
  
-   **Contas atraentes para roubo de credenciais** – ataques de roubo de credenciais são aqueles em que um invasor recebe inicialmente acesso privilegiado a um computador em uma rede e, em seguida, usa ferramentas gratuitas disponíveis para extrair credenciais das sessões de outras contas conectadas.   
    As seguintes opções estão incluídas nesta seção:  
  
    -   **Atividades que aumentam a probabilidade de comprometimento** -como o alvo do roubo de credenciais geralmente são contas de domínio altamente privilegiadas e contas de "pessoa muito importante" (VIP), é importante que os administradores estejam preocupados com as atividades que aumentam a probabilidade de um ataque de roubo de credencial. Essas atividades são:  
  
        -   Fazendo logon em computadores não seguros com contas com privilégios  
  
        -   Navegando pela Internet com uma conta altamente privilegiada  
  
        -   Configurando contas com privilégios locais com as mesmas credenciais entre sistemas  
  
        -   Sobrepopulação e uso excessivo de grupos de domínio com privilégios  
  
        -   Gerenciamento insuficiente da segurança dos controladores de domínio.  
  
    -   A **elevação de privilégio e** contas específicas de propagação, servidores e componentes de infraestrutura geralmente são os principais destinos de ataques contra Active Directory. Essas contas são:  
  
        -   Contas com privilégios permanentes  
  
        -   Contas VIP  
  
        -   Contas de Active Directory "anexadas por privilégio"  
  
        -   Controladores de domínio  
  
        -   Outros serviços de infraestrutura que afetam o gerenciamento de identidade, acesso e configuração, como servidores PKI (infraestrutura de chave pública) e servidores de gerenciamento de sistemas  
  
## <a name="reducing-the-active-directory-attack-surface"></a>Redução da superfície de ataque do Active Directory  
Esta seção se concentra em controles técnicos para reduzir a superfície de ataque de uma instalação de Active Directory. As seguintes entidades estão incluídas nesta seção:  
  
-   A seção **contas e grupos com privilégios no Active Directory** discute as contas e os grupos com privilégios mais altos no Active Directory e os mecanismos pelos quais as contas privilegiadas são protegidas. Em Active Directory, três grupos internos são os grupos de privilégios mais altos no diretório (administradores corporativos, administradores de domínio e administradores), embora um número de grupos e contas adicionais também deva ser protegido.  
  
-   A seção **implementando modelos administrativos de privilégios mínimos** concentra-se na identificação do risco de que o uso de contas altamente privilegiadas para os presentes na administração diária, além de fornecer recomendações para reduzir esse risco.  
  
O privilégio excessivo não é encontrado apenas em Active Directory em ambientes comprometidos. Quando uma organização desenvolveu o hábito de conceder mais privilégios do que o necessário, normalmente ele é encontrado em toda a infraestrutura:  
  
-   Em Active Directory  
  
-   Em servidores membro  
  
-   Em estações de trabalho  
  
-   Em aplicativos  
  
-   Em repositórios de dados  
  
-   A seção **implementando hosts administrativos seguros** descreve os hosts administrativos seguros, que são computadores configurados para dar suporte à administração de Active Directory e sistemas conectados. Esses hosts são dedicados à funcionalidade administrativa e não executam software como aplicativos de email, navegadores da Web ou software de produtividade (como Microsoft Office).  
  
As seguintes opções estão incluídas nesta seção:  
  
-   **Princípios para a criação de hosts administrativos seguros** -os princípios gerais a serem considerados são:  
  
    -   Nunca administre um sistema confiável de um host menos confiável.  
  
    -   Não confie em um único fator de autenticação ao executar atividades privilegiadas.  
  
    -   Não se esqueça da segurança física ao projetar e implementar hosts administrativos seguros.  
  
-   **Protegendo os controladores de domínio contra ataques** – se um usuário mal-intencionado obtiver acesso privilegiado a um controlador de domínio, esse usuário poderá modificar, corromper e destruir o banco de dados Active Directory e, por extensão, todos os sistemas e contas que são gerenciados pelo Active Directory.  
  
As seguintes entidades estão incluídas nesta seção:  
  
-   **Segurança física para controladores de domínio** – contém recomendações para fornecer segurança física para controladores de domínio em data centers, filiais e locais remotos.  
  
-   **Sistemas operacionais do controlador de domínio** -contém recomendações para proteger os sistemas operacionais do controlador de domínio.  
  
-   **Configuração segura de controladores de domínio** -as configurações e as ferramentas de configuração disponíveis e gratuitas podem ser usadas para criar linhas de base de configuração de segurança para controladores de domínio que podem ser subsequentemente impostas por objetos de política de grupo (GPOs).  
  
## <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitorar o Active Directory em busca de sinais de comprometimento  
Esta seção fornece informações sobre categorias de auditoria herdadas e subcategorias de política de auditoria (que foram introduzidas no Windows Vista e no Windows Server 2008) e política de auditoria avançada (que foi introduzida no Windows Server 2008 R2). Também são fornecidas informações sobre eventos e objetos para monitorar que podem indicar tentativas de comprometer o ambiente e algumas referências adicionais que podem ser usadas para construir uma política de auditoria abrangente para Active Directory.  
  
As seguintes entidades estão incluídas nesta seção:  
  
-   **Política de auditoria do Windows** – os logs de eventos de segurança do Windows têm categorias e subcategorias que determinam quais eventos de segurança são rastreados e registrados.  
  
-   **Recomendações de política de auditoria** – esta seção descreve as configurações da política de auditoria padrão do Windows, as configurações de política de auditoria recomendadas pela Microsoft e recomendações mais agressivas para que as organizações usem para auditar servidores e estações de trabalho críticos.  
  
## <a name="planning-for-compromise"></a>Planejar para comprometimento  
Esta seção contém recomendações que ajudarão as organizações a se preparar para um comprometimento antes de acontecerem, implementar controles que possam detectar um evento de comprometimento antes que ocorra uma violação completa e fornecer diretrizes de resposta e recuperação para casos em que um comprometimento completo do diretório seja obtido por invasores. As seguintes entidades estão incluídas nesta seção:  
  
-   **Reformular a abordagem** -contém princípios e diretrizes para criar ambientes seguros nos quais uma organização pode posicionar seus ativos mais críticos. Essas diretrizes são as seguintes:  
  
    -   Identificar princípios para separar e proteger ativos críticos  
  
    -   Definindo um plano de migração limitado baseado em risco  
  
    -   Aproveitando as migrações "nonmigratory" quando necessário  
  
    -   Implementando a "destruição criativa"  
  
    -   Isolando sistemas e aplicativos herdados  
  
    -   Simplificando a segurança para usuários finais  
  
-   **Mantendo um ambiente mais seguro** -contém recomendações de alto nível destinadas a serem usadas como diretrizes para uso no desenvolvimento não apenas em uma segurança efetiva, mas o gerenciamento eficiente do ciclo de vida. As seguintes entidades estão incluídas nesta seção:  
  
    -   **Criando práticas de segurança centradas nos negócios para o Active Directory** -para gerenciar com eficiência o ciclo de vida dos usuários, dados, aplicativos e sistemas gerenciados pelo Active Directory, siga estes princípios.  
  
        -   **Atribuir uma propriedade corporativa a Active Directory dados** -atribuir propriedade de componentes de infraestrutura a ela; para dados que são adicionados ao Active Directory Domain Services (AD DS) para dar suporte à empresa, por exemplo, novos funcionários, novos aplicativos e novos repositórios de informações, uma unidade de negócios ou um usuário designado deve ser associado aos dados.  
  
        -   **Implementar gerenciamento de ciclo de vida voltado para negócios** -o gerenciamento do ciclo de vida deve ser implementado para dados em Active Directory.  
  
        -   **Classificar todos os Active Directory dados-os** proprietários de negócios devem fornecer classificação de dados em Active Directory. Dentro do modelo de classificação de dados, a classificação para os seguintes dados de Active Directory deve ser incluída:  
  
            -   **Sistemas** -classificar populações de servidor, seu sistema operacional, sua função, os aplicativos em execução e os proprietários de ti e de negócios do registro.  
  
            -   **Aplicativos** – classifique aplicativos por funcionalidade, base de usuários e seu sistema operacional.  
  
            -   **Usuários** – as contas nas instalações Active Directory com maior probabilidade de serem direcionadas por invasores devem ser marcadas e monitoradas.  
  
## <a name="summary-of-best-practices-for-securing-active-directory-domain-services"></a>Resumo das práticas recomendadas para proteger Active Directory Domain Services  
A tabela a seguir fornece um resumo das recomendações fornecidas neste documento para proteger uma instalação do AD DS. Algumas práticas recomendadas são estratégicas por natureza e exigem projetos abrangentes de planejamento e implementação; outras são táticas e concentradas em componentes específicos de Active Directory e infraestrutura relacionada.  
  
As práticas são listadas em ordem aproximada de prioridade, ou seja, números menores indicam prioridade mais alta. Quando aplicável, as práticas recomendadas são identificadas como preventiva ou de detecção por natureza. Todas essas recomendações devem ser exaustivamente testadas e modificadas conforme necessário para as características e os requisitos da sua organização.  
  
  
||**Prática recomendada**|**Tático ou estratégico**|**Preventiva ou de detecção**|  
|-|-|-|-|  
|1|Aplicativos de patch.|Táticas|Preventivo|  
|2|Sistemas operacionais de patch.|Táticas|Preventivo|  
|3|Implante e atualize imediatamente software antivírus e antimalware em todos os sistemas e monitore as tentativas de removê-lo ou desabilitá-lo.|Táticas|Ambos|  
|4|Monitore objetos Active Directory confidenciais para tentativas de modificação e para eventos do Windows que podem indicar a tentativa de comprometimento.|Táticas|Detetive|  
|5|Proteger e monitorar contas para usuários que têm acesso a dados confidenciais|Táticas|Ambos|  
|6|Impedir que contas poderosas sejam usadas em sistemas não autorizados.|Táticas|Preventivo|  
|7|Elimine a associação permanente em grupos altamente privilegiados.|Táticas|Preventivo|  
|8|Implemente controles para conceder a Associação temporária em grupos privilegiados quando necessário.|Táticas|Preventivo|  
|9|Implementar hosts administrativos seguros.|Táticas|Preventivo|  
|10|Use a lista de permissões de aplicativos em controladores de domínio, hosts administrativos e outros sistemas confidenciais.|Táticas|Preventivo|  
|11|Identificar ativos críticos e priorizar a segurança e o monitoramento.|Táticas|Ambos|  
|12|Implemente controles de acesso com privilégios mínimos e baseados em função para administração do diretório, sua infraestrutura de suporte e sistemas ingressados no domínio.|Estratégicas|Preventivo|  
|13|Isole os sistemas e aplicativos herdados.|Táticas|Preventivo|  
|14|Descomissionar sistemas e aplicativos herdados.|Estratégicas|Preventivo|  
|15|Implemente programas de ciclo de vida de desenvolvimento seguro para aplicativos personalizados.|Estratégicas|Preventivo|  
|16|Implemente o gerenciamento de configuração, examine a conformidade regularmente e avalie as configurações com cada nova versão de hardware ou software.|Estratégicas|Preventivo|  
|17|Migre ativos críticos para florestas original com requisitos rigorosos de segurança e monitoramento.|Estratégicas|Ambos|  
|18|Simplifique a segurança para os usuários finais.|Estratégicas|Preventivo|  
|19|Use firewalls baseados em host para controlar e proteger as comunicações.|Táticas|Preventivo|  
|20|Dispositivos de patch.|Táticas|Preventivo|  
|21|Implemente o gerenciamento de ciclo de vida centrado nos negócios para ativos de ti.|Estratégicas|{1&gt;N/A&lt;1}|  
|22|Criar ou atualizar planos de recuperação de incidentes.|Estratégicas|{1&gt;N/A&lt;1}|  
  


