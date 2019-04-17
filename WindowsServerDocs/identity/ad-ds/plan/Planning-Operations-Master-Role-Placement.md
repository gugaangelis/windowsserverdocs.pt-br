---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: "Planejar o posicionamento da função mestre de operações"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d271ed3a54e78106aed0d4dbfdb610cc8b9a460
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-operations-master-role-placement"></a>Planejar o posicionamento da função mestre de operações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os serviços de domínio do Active Directory (AD DS) dá suporte a vários mestres replicação de dados de diretório, o que significa que qualquer controlador de domínio pode aceitar alterações no diretório e replicar as alterações para todos os controladores de domínio. No entanto, determinados alterado, como modificações de esquema é não podem ser executadas de maneira vários mestres. Por esse motivo determinados controladores de domínio, conhecidos como mestres de operações, segure funções responsável para aceitar solicitações de determinadas mudanças específicas.  
  
> [!NOTE]  
> Os titulares de função mestre de operações devem ser capazes de gravar algumas informações para o banco de dados do Active Directory. Devido à natureza somente leitura do banco de dados do Active Directory em um controlador de domínio somente leitura (RODC), RODCs não podem atuar como titulares de função mestre de operações.  
  
Funções mestre de operações de três (operações mestre únicas também conhecido como flexíveis ou FSMO) existem em cada domínio:  
  
-   Mestre de operações do emulador do domínio primário (PDC) do controlador processa todas as atualizações de senha.  
  
-   O mestre de operações de ID (RID) relativo mantém o pool RID global para o domínio e aloca pools RIDs locais para todos os controladores de domínio para garantir que todas as entidades de segurança criadas no domínio tem um identificador exclusivo.  
  
-   O mestre de operações de infraestrutura para um determinado domínio mantém uma lista dos objetos de segurança de outros domínios que são membros de grupos em seu domínio.  
  
Além das funções mestre de operações de nível de domínio três, funções mestre de operações de dois existem em cada floresta:  
  
-   O mestre de operações de esquema rege alterações no esquema.  
  
-   O mestre de operações de nomeação de domínio adiciona e remove domínios e outras partições de diretório (por exemplo, partições do sistema de nome de domínio (DNS) application) para e da floresta.  
  
Coloque os controladores de domínio que hospeda essas funções mestre de operações em áreas onde a confiabilidade da rede for alta e certifique-se de que o emulador do PDC e o mestre RID estão consistentemente disponíveis.  
  
Os titulares de função mestre de operações são atribuídos automaticamente quando o primeiro controlador de domínio em um determinado domínio é criado. As duas funções de nível de floresta (mestre de esquema e mestre de nomeação de domínio) são atribuídas ao controlador de domínio primeiro criado em uma floresta. Além disso, as três funções de nível de domínio (RID mestre, mestre de infraestrutura e emulador do PDC) são atribuídas ao controlador de domínio primeiro criado em um domínio.  
  
> [!NOTE]  
> Atribuições de titular de funções mestre de operações automática são feitas somente quando um novo domínio é criado e quando uma proprietário atual da função é rebaixada. Todas as outras alterações para proprietários de função precisam ser iniciadas por um administrador.  
  
Essas atribuições de função mestre de operações automático podem causar muito alto uso da CPU no controlador de domínio primeiro criado na floresta ou domínio. Para evitar isso, atribua as operações de (transferência) funções mestre para vários controladores de domínio no domínio ou floresta. Coloque os controladores de domínio que host mestre de operações em áreas onde a rede é confiável e onde os mestres de operações podem ser acessados por todos os controladores de domínio na floresta.  
  
Você também deve designar operações de modo de espera (alternativa) mestres de operações de todas as funções de mestre. Os mestres de operações no modo de espera são controladores de domínio ao qual você pode transferir as funções mestre de operações em caso de falham dos titulares de função original. Certifique-se de que os mestres de operações no modo de espera são parceiros de replicação direta dos mestres de operações real.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Planejar o posicionamento de emulador do PDC  
O emulador do PDC processa alterações de senha do cliente. Somente um controlador de domínio atua como o emulador do PDC em cada domínio na floresta.  
  
Mesmo que todos os controladores de domínio são atualizados para o Windows 2000, Windows Server 2003 e Windows Server 2008 e o domínio está operando com o nível funcional nativo do Windows 2000, o emulador do PDC recebe a duplicação preferencial de alterações de senha executadas por outros controladores de domínio no domínio. Se uma senha tiver sido alterada recentemente, essa alteração leva tempo para ser replicada para todos os controladores de domínio no domínio. Se a autenticação de logon falhar em outro controlador de domínio por causa de uma senha incorreta, esse controlador de domínio encaminha a solicitação de autenticação para o emulador do PDC antes de decidir se deseja aceitar ou rejeitar a tentativa de logon.  
  
Coloque o emulador do PDC em um local que contém um grande número de usuários de domínio senha encaminhando operações se necessário. Além disso, certifique-se de que a localização está bem conectada para outros locais para minimizar a latência da replicação.  
  
Para uma planilha para ajudá-lo a documentar as informações sobre onde você planeja colocar emuladores PDC e o número de usuários para cada domínio que é representado em cada local, consulte trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558) ), baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abrir (DSSTOPO_4.doc) posicionamento do controlador de domínio.  
  
Você precisa fazer referência às informações sobre locais em que você precisa colocar emuladores PDC quando você implanta domínios regionais. Para obter mais informações sobre como implantar domínios regionais, consulte [implantação do Windows Server 2008 regionais domínios](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Requisitos para posicionamento mestre de infraestrutura  

O mestre de infraestrutura atualiza os nomes de entidades de segurança de outros domínios que são adicionados a grupos em seu próprio domínio. Por exemplo, se um usuário de um domínio seja um membro de um grupo em um domínio de segundo e o nome do usuário é alterado no primeiro domínio, o segundo domínio não é notificado que o nome do usuário deve ser atualizado na lista de membros do grupo. Como controladores de domínio em um domínio não duplicam entidades de segurança para os controladores de domínio em outro domínio, o segundo domínio nunca se torna ciente da alteração na ausência do mestre de infraestrutura.  
  
O mestre de infraestrutura constantemente monitores agrupar associações, procurando por entidades de segurança de outros domínios. Se ele encontrar um, ele verifica com o domínio da entidade de segurança para verificar se as informações estão atualizadas. Se as informações estiverem desatualizadas, o mestre de infraestrutura executa a atualização e, em seguida, duplica as alterações para os outros controladores de domínio no domínio.  
  
Aplicam duas exceções a essa regra. Primeiro, se todos os controladores de domínio são servidores de catálogo global, o controlador de domínio que hospeda a função mestre de infraestrutura é insignificante porque catálogos globais replicam as informações atualizadas, independentemente do domínio ao qual eles pertencem. Em segundo lugar, se a floresta tem apenas um domínio, o controlador de domínio que hospeda a função mestre de infraestrutura é insignificante porque não existem entidades de segurança de outros domínios.  
  
Não coloque o mestre de infraestrutura em um controlador de domínio que também é um servidor de catálogo global. Se o mestre de infraestrutura e catálogo global estiverem no mesmo controlador de domínio, o mestre de infraestrutura não funcionará. O mestre de infraestrutura nunca vai encontrar dados que estão desatualizados; Portanto, nunca replicará todas as alterações para os outros controladores de domínio no domínio.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Posicionamento para redes de mestre de operações com conectividade limitada  
Lembre-se de que, se o seu ambiente tem um local central ou site de hub no qual você pode colocar os titulares de função mestre de operações, certas operações de controlador de domínio que dependem da disponibilidade dessas operações mestre função titulares podem ser afetados.  
  
Por exemplo, suponha que uma organização cria sites A, B, C, e links de Site D. existem entre A e B, entre B e C e entre C e D. conectividade de rede exatamente reflete a conectividade de rede dos links de sites. Neste exemplo, todas as operações funções mestre são colocadas no site uma e a opção de **ponte todos os links de sites** não é selecionado.  
  
Embora essa configuração resulta em bem-sucedida replicação entre todos os sites, as funções de função mestre de operações têm as seguintes limitações:  
  
-   Controladores de domínio em sites C e D não podem acessar o emulador do PDC no site para atualizar uma senha ou verificá-la para uma senha que foi atualizada recentemente.  
  
-   Controladores de domínio em sites C e D não podem acessar o mestre de RID no site para obter um pool RID inicial após a instalação do Active Directory e atualizar pools RID como eles se tornam descarregados.  
  
-   Controladores de domínio em sites C e D não é possível adicionar ou remover diretório, DNS ou partições de aplicativo personalizado.  
  
-   Controladores de domínio em sites C e D não podem fazer alterações no esquema.  
  
Para uma planilha para ajudá-lo a planejar o posicionamento de funções mestre de operações, consulte orientações para [Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de baixar e abrir (DSSTOPO_4.doc) posicionamento do controlador de domínio.  
  
Você precisará fazer referência a essas informações quando você cria o domínio raiz da floresta e domínios regionais. Para obter mais informações sobre a implantação do domínio raiz da floresta, consulte Implantando uma [Implantando um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx). Para obter mais informações sobre como implantar domínios regionais, consulte [implantação do Windows Server 2008 regionais domínios](https://technet.microsoft.com/library/cc755118.aspx).  
  


