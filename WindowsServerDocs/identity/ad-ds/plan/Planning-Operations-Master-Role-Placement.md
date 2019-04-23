---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Planejando posicionamento da função de mestre das operações
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a3fbe76302199888dce19f845b1c838c07facb82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870887"
---
# <a name="planning-operations-master-role-placement"></a>Planejando posicionamento da função de mestre das operações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os serviços de domínio do Active Directory (AD DS) dá suporte a replicação multimestre de dados de diretório, o que significa que qualquer controlador de domínio pode aceitar as alterações de diretório e replicar as alterações para todos os outros controladores de domínio. No entanto, determinadas alterações, como modificações de esquema são impraticáveis a realização de uma maneira de vários mestres. Por esse motivo determinados controladores de domínio, conhecidos como mestres de operações, mantenha as funções responsável por aceitar solicitações para determinadas alterações específicas.  
  
> [!NOTE]  
> Proprietários da função de mestre de operações devem ser capazes de gravar algumas informações no banco de dados do Active Directory. Devido à natureza somente leitura do banco de dados do Active Directory em um controlador de domínio somente leitura (RODC), **RODCs não podem atuar como proprietários da função de mestre de operações**.  
  
Funções de mestre de operações de três (também conhecidas como operações de mestre único ou FSMO) existem em cada domínio:  
  
- O mestre de operações de emulador PDC (controlador) de domínio primário processa todas as atualizações de senha.  

- O mestre de operações do ID (RID) relativo mantém o pool RID global para o domínio e aloca pools de RIDs locais em todos os controladores de domínio para garantir que todas as entidades de segurança criadas no domínio tenham um identificador exclusivo.  
- O mestre de operações de infraestrutura para um determinado domínio mantém uma lista das entidades de segurança de outros domínios que são membros de grupos em seu domínio.  

Além das funções de mestre de operações de nível de domínio de três, funções de mestre de operações de dois existirem em cada floresta:  
  
- O mestre de operações de esquema controla as alterações no esquema.  
- O mestre de operações de nomeação de domínio adiciona e remove os domínios e outras partições de diretório (por exemplo, partições de aplicativo de sistema de nome de domínio (DNS)) e para a floresta.  
  
Coloque os controladores de domínio que hospeda essas funções de mestre de operações nas áreas em que a confiabilidade da rede é alta e certifique-se de que o emulador do PDC e o mestre de RID estejam consistentemente disponíveis.  
  
Proprietários da função de mestre de operações são atribuídos automaticamente quando o primeiro controlador de domínio em um determinado domínio é criado. As duas funções de nível de floresta (mestre de esquema e mestre de nomeação de domínio) são atribuídas ao primeiro controlador de domínio criado em uma floresta. Além disso, as três funções de nível de domínio (mestre RID, mestre de infraestrutura e o emulador PDC) são atribuídas ao primeiro controlador de domínio criado em um domínio.  
  
> [!NOTE]  
> Atribuições de proprietário de função de mestre de operações automática são feitas somente quando um novo domínio é criado e quando um detentor de função atual é rebaixado. Todas as outras alterações para proprietários de função precisa ser iniciado por um administrador.  
  
Essas atribuições de função de mestre de operações automático podem causar muito alto uso da CPU no primeiro controlador de domínio criado na floresta ou domínio. Para evitar isso, atribua operações (transferência) funções de mestre em vários controladores de domínio no seu domínio ou floresta. Coloque os controladores de domínio que host mestre de operações nas áreas em que a rede é confiável e em que os mestres de operações podem ser acessados por todos os outros controladores de domínio na floresta.  
  
Você também deve designar as operações em espera (alternativa) funções de mestres para todas as operações de mestre. Os mestres de operações em espera são controladores de domínio ao qual você pode transferir as funções de mestre de operações em caso de falham os detentores de função original. Certifique-se de que os mestres de operações em espera são parceiros de replicação direta dos mestres de operações real.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Planejando o posicionamento do emulador PDC

O emulador do PDC processa alterações de senha do cliente. Somente um controlador de domínio atua como o emulador PDC em cada domínio na floresta.  
  
Mesmo que todos os controladores de domínio são atualizados para o Windows 2000, Windows Server 2003 e Windows Server 2008 e o domínio está operando no nível funcional nativo do Windows 2000, o emulador do PDC recebe replicação preferencial de alterações de senha executadas por outros controladores de domínio no domínio. Se uma senha foi alterada recentemente, essa alteração leva tempo para replicar a cada controlador de domínio no domínio. Se a autenticação de logon falhar no outro controlador de domínio devido a uma senha incorreta, esse controlador de domínio encaminha a solicitação de autenticação para o emulador do PDC antes de decidir se deseja aceitar ou rejeitar a tentativa de logon.  
  
Coloque o emulador do PDC em um local que contém um grande número de usuários nesse domínio senha operações de encaminhamento, se necessário. Além disso, certifique-se de que o local também está conectado a outros locais para minimizar a latência de replicação.  
  
Para uma planilha ajudar a documentar as informações sobre onde você planeja colocar emuladores PDC e o número de usuários para cada domínio que é representado em cada local, consulte o trabalho auxílios para Windows Server 2003 Deployment Kit ([ https://go.microsoft.com/fwlink/?LinkID=102558 ](https://go.microsoft.com/fwlink/?LinkID=102558)), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abra o posicionamento de controlador de domínio (DSSTOPO_4.doc).  
  
Você precisa fazer referência às informações sobre os locais em que você precisa colocar os emuladores PDC quando você implantar domínios regionais. Para obter mais informações sobre como implantar domínios regionais, consulte [implantar o Windows Server 2008 domínios regionais](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Requisitos para o posicionamento de mestre de infraestrutura  

O mestre de infraestrutura atualiza os nomes de entidades de segurança de outros domínios que são adicionados aos grupos em seu próprio domínio. Por exemplo, se um usuário de um domínio é um membro de um grupo em um domínio de segundo e o nome do usuário é alterado no primeiro domínio, o segundo domínio não é notificado que o nome do usuário deve ser atualizado na lista de membros do grupo. Porque os controladores de domínio em um domínio não são replicadas para entidades de segurança para controladores de domínio em outro domínio, o segundo domínio nunca fica ciente de que a alteração na ausência de mestre de infraestrutura.  
  
O mestre de infraestrutura constantemente monitora as associações de grupo, procurando por entidades de segurança de outros domínios. Se ele encontrar um, ele verifica com o domínio da entidade de segurança para verificar se as informações são atualizadas. Se as informações estiverem desatualizadas, o mestre de infra-estrutura executa a atualização e, depois, replica a alteração para os outros controladores de domínio em seu domínio.  
  
Duas exceções se aplicam a essa regra. Primeiro, se todos os controladores de domínio são servidores de catálogo global, o controlador de domínio que hospeda a função de mestre de infraestrutura é insignificante, porque os catálogos globais replicam as informações atualizadas, independentemente do domínio ao qual pertencem. Em segundo lugar, se a floresta tiver apenas um domínio, o controlador de domínio que hospeda a função de mestre de infraestrutura é insignificante, porque não existem objetos de segurança de outros domínios.  
  
Não coloque o mestre de infraestrutura em um controlador de domínio que também é um servidor de catálogo global. Se o mestre de infraestrutura e o catálogo global estiverem no mesmo controlador de domínio, o mestre de infraestrutura não funcionará. O mestre de infraestrutura nunca encontrará dados que estão desatualizados; Portanto, ele nunca replicará as alterações para os outros controladores de domínio no domínio.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Posicionamento para redes de mestre de operações com conectividade limitada

Lembre-se de que se seu ambiente tiver um local central ou site do hub no qual você pode colocar os detentores de função de mestre de operações, determinadas operações de controlador de domínio que dependem da disponibilidade dessas operações função de mestre reservados podem ser afetados.  
  
Por exemplo, suponha que uma organização cria sites A, B, C, e links de Site D. existem entre A e B, entre B e C e entre C e D. conectividade de rede exatamente espelha a conectividade de rede dos links de sites. Neste exemplo, todas as operações de funções de mestre são colocadas no site A e a opção para **ponte para todos os links de site** não estiver selecionada.  
  
Embora essa configuração resulta em êxito da replicação entre todos os sites, as funções da função de mestre de operações têm as seguintes limitações:  
  
- Controladores de domínio em sites de C e D não é possível acessar o emulador do PDC no site para atualizar uma senha ou fazer o check-uma senha que foi atualizado recentemente.  
- Controladores de domínio em sites de C e D não é possível acessar o mestre de RID no site para obter um pool RID inicial após a instalação do Active Directory e para atualizar pools RID conforme eles se esgotar.  
- Controladores de domínio em sites de C e D não é possível adicionar ou remover o diretório, DNS ou partições de aplicativo personalizado.  
- Controladores de domínio em sites de C e D não é possível fazer alterações de esquema.  
  
Para uma planilha ajudar a planejar o posicionamento de função de mestre de operações, consulte [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de baixar e abrir Posicionamento de controlador de domínio (DSSTOPO_4.doc).  
  
Você precisará consultar essas informações quando você cria o domínio raiz da floresta e domínios regionais. Para obter mais informações sobre a implantação do domínio raiz da floresta, consulte Implantando uma [implantação de um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Para obter mais informações sobre como implantar domínios regionais, consulte [implantar o Windows Server 2008 domínios regionais](https://technet.microsoft.com/library/cc755118.aspx).  

## <a name="next-steps"></a>Próximas etapas

Informações adicionais sobre o posicionamento da função FSMO podem ser encontradas no tópico de suporte [FSMO posicionamento e otimização em controladores de domínio do Active Directory](https://support.microsoft.com/help/223346)
