---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: Planejando posicionamento da função de mestre das operações
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ad4e89be7eeb6190d27ee0e15e370bcaa1806cb8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959368"
---
# <a name="planning-operations-master-role-placement"></a>Planejando posicionamento da função de mestre das operações

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) dá suporte à replicação multimestre de dados de diretório, o que significa que qualquer controlador de domínio pode aceitar alterações de diretório e replicar as alterações para todos os outros controladores de domínio. No entanto, determinadas alterações, como modificações de esquema, são impraticável de serem executadas em uma maneira de vários mestres. Por esse motivo, determinados controladores de domínio, conhecidos como mestres de operações, mantêm as funções responsáveis por aceitar solicitações para determinadas alterações específicas.

> [!NOTE]
> Os detentores de função do mestre de operações devem ser capazes de gravar algumas informações no banco de dados Active Directory. Devido à natureza somente leitura do banco de dados de Active Directory em um RODC (controlador de domínio somente leitura), **os RODCs não podem atuar como detentores de função de mestre de operações**.

Três funções de mestre de operações (também conhecidas como operações de mestre único flexíveis ou FSMO) existem em cada domínio:

- O mestre de operações do emulador do controlador de domínio primário (PDC) processa todas as atualizações de senha.

- O mestre de operações de RID (ID relativa) mantém o pool de RID global para o domínio e aloca pools de RIDs locais para todos os controladores de domínio para garantir que todas as entidades de segurança criadas no domínio tenham um identificador exclusivo.
- O mestre de operações de infraestrutura para um determinado domínio mantém uma lista das entidades de segurança de outros domínios que são membros de grupos dentro de seu domínio.

Além das três funções de mestre de operações no nível de domínio, existem duas funções de mestre de operações em cada floresta:

- O mestre de operações de esquema controla as alterações no esquema.
- O mestre de operações de nomeação de domínio adiciona e remove domínios e outras partições de diretório (por exemplo, partições de aplicativo DNS) de e para a floresta.

Coloque os controladores de domínio que hospedam essas funções de mestre de operações em áreas em que a confiabilidade de rede está alta e verifique se o emulador PDC e o mestre RID estão consistentemente disponíveis.

Os detentores de função de mestre de operações são atribuídos automaticamente quando o primeiro controlador de domínio em um determinado domínio é criado. As duas funções de nível de floresta (mestre de esquema e mestre de nomeação de domínio) são atribuídas ao primeiro controlador de domínio criado em uma floresta. Além disso, as três funções de nível de domínio (mestre de RID, mestre de infraestrutura e emulador de PDC) são atribuídas ao primeiro controlador de domínio criado em um domínio.

> [!NOTE]
> As atribuições do detentor da função mestre de operações automáticas são feitas somente quando um novo domínio é criado e quando um detentor da função atual é rebaixado. Todas as outras alterações nos proprietários da função precisam ser iniciadas por um administrador.

Essas atribuições da função mestre de operações automáticas podem causar um uso muito alto da CPU no primeiro controlador de domínio criado na floresta ou no domínio. Para evitar isso, atribua (transfira) funções de mestre de operações a vários controladores de domínio em sua floresta ou domínio. Coloque os controladores de domínio que hospedam funções de mestre de operações em áreas em que a rede é confiável e onde os mestres de operações podem ser acessados por todos os outros controladores de domínio na floresta.

Você também deve designar mestres de operações em espera (alternativa) para todas as funções de mestre de operações. Os mestres de operações em espera são controladores de domínio para os quais você pode transferir as funções de mestre de operações caso os detentores de função originais falhem. Verifique se os mestres de operações em espera são parceiros de replicação direta dos mestres de operações reais.

## <a name="planning-the-pdc-emulator-placement"></a>Planejando o posicionamento do emulador de PDC

O emulador de PDC processa as alterações de senha do cliente. Somente um controlador de domínio atua como o emulador de PDC em cada domínio na floresta.

Mesmo se todos os controladores de domínio forem atualizados para o Windows 2000, o Windows Server 2003 e o Windows Server 2008 e o domínio estiver operando no nível funcional nativo do Windows 2000, o emulador PDC receberá a replicação preferencial de alterações de senha executadas por outros controladores de domínio no domínio. Se uma senha tiver sido alterada recentemente, essa alteração levará tempo para ser replicada para cada controlador de domínio no domínio. Se a autenticação de logon falhar em outro controlador de domínio devido a uma senha inadequada, esse controlador de domínio encaminha a solicitação de autenticação para o emulador de PDC antes de decidir se deseja aceitar ou rejeitar a tentativa de logon.

Coloque o emulador de PDC em um local que contenha um grande número de usuários desse domínio para operações de encaminhamento de senha, se necessário. Além disso, verifique se o local está bem conectado a outros locais para minimizar a latência de replicação.

Para uma planilha para ajudá-lo a documentar as informações sobre onde você planeja colocar emuladores de PDC e o número de usuários para cada domínio representado em cada local, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abrir o posicionamento do controlador de domínio (DSSTOPO_4.doc).

Você precisa consultar as informações sobre os locais nos quais você precisa inserir emuladores de PDC ao implantar domínios regionais. Para obter mais informações sobre a implantação de domínios regionais, consulte [implantando domínios regionais do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10)).

## <a name="requirements-for-infrastructure-master-placement"></a>Requisitos para o posicionamento do mestre de infraestrutura

O mestre de infraestrutura atualiza os nomes das entidades de segurança de outros domínios que são adicionados aos grupos em seu próprio domínio. Por exemplo, se um usuário de um domínio for membro de um grupo em um segundo domínio e o nome do usuário for alterado no primeiro domínio, o segundo domínio não será notificado de que o nome do usuário deve ser atualizado na lista de associação do grupo. Como os controladores de domínio em um domínio não replicam entidades de segurança para controladores de domínio em outro domínio, o segundo domínio nunca se reconhece da alteração na ausência do mestre de infraestrutura.

O mestre de infraestrutura monitora constantemente as associações de grupo, procurando entidades de segurança de outros domínios. Se encontrar uma, ele verificará o domínio da entidade de segurança para verificar se as informações estão atualizadas. Se as informações estiverem desatualizadas, o mestre de infraestrutura executará a atualização e replicará a alteração para os outros controladores de domínio em seu domínio.

Duas exceções se aplicam a essa regra. Primeiro, se todos os controladores de domínio forem servidores de catálogo global, o controlador de domínio que hospeda a função mestre de infraestrutura é insignificante, pois os catálogos globais replicam as informações atualizadas, independentemente do domínio ao qual pertencem. Em segundo lugar, se a floresta tiver apenas um domínio, o controlador de domínio que hospeda a função mestre de infraestrutura será insignificante porque as entidades de segurança de outros domínios não existem.

Não coloque o mestre de infraestrutura em um controlador de domínio que também seja um servidor de catálogo global. Se o mestre de infraestrutura e o catálogo global estiverem no mesmo controlador de domínio, o mestre de infraestrutura não funcionará. O mestre de infraestrutura nunca encontrará dados desatualizados; Portanto, ele nunca replicará nenhuma alteração para os outros controladores de domínio no domínio.

## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Posicionamento do mestre de operações para redes com conectividade limitada

Lembre-se de que, se o seu ambiente tiver um local central ou um site de Hub no qual você possa posicionar os detentores de função do mestre de operações, determinadas operações do controlador de domínio que dependem da disponibilidade desses detentores de função do mestre de operações poderão ser afetadas.

Por exemplo, suponha que uma organização crie os sites A, B, C e D. os links de site existem entre A e B, entre B e C e entre C e D. a conectividade de rede espelha exatamente a conectividade de rede dos links de sites. Neste exemplo, todas as funções de mestre de operações são colocadas no site A e a opção de **ponte de todos os links de site** não está selecionada.

Embora essa configuração resulte em uma replicação bem-sucedida entre todos os sites, as funções da função mestre de operações têm as seguintes limitações:

- Os controladores de domínio nos sites C e D não podem acessar o emulador de PDC no local a para atualizar uma senha ou verificá-lo em busca de uma senha que tenha sido atualizada recentemente.
- Os controladores de domínio nos sites C e D não podem acessar o mestre RID no site A para obter um pool RID inicial após a instalação do Active Directory e atualizar os pools RID à medida que eles ficarem esgotados.
- Os controladores de domínio nos sites C e D não podem adicionar ou remover diretórios, DNS ou partições de aplicativo personalizadas.
- Os controladores de domínio nos sites C e D não podem fazer alterações de esquema.

Para obter uma planilha para ajudá-lo a planejar o posicionamento da função mestre de operações, consulte [ajudas de trabalho para o Windows Server 2003 Deployment Kit](https://microsoft.com/download/details.aspx?id=9608), baixar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e abrir o posicionamento do controlador de domínio (DSSTOPO_4.doc).

Você precisará consultar essas informações ao criar o domínio raiz da floresta e os domínios regionais. Para obter mais informações sobre como implantar o domínio raiz da floresta, consulte Implantando um [domínio raiz da floresta do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731174(v=ws.10)). Para obter mais informações sobre a implantação de domínios regionais, consulte [implantando domínios regionais do Windows Server 2008](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755118(v=ws.10)).

## <a name="next-steps"></a>Próximas etapas

Informações adicionais sobre o posicionamento da função FSMO podem ser encontradas no tópico de suporte [posicionamento e otimização do FSMO em controladores de domínio Active Directory](https://support.microsoft.com/help/223346)
