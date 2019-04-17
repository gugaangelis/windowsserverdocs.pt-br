---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: "Selecionando o domínio raiz da floresta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d87273df1e42e1fa88df09e0b86d4c86ea75ee3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="selecting-the-forest-root-domain"></a>Selecionando o domínio raiz da floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O primeiro domínio que você implantar em uma floresta do Active Directory é chamado de domínio raiz da floresta. Neste domínio permanece domínio raiz da floresta para o ciclo de vida da implantação do AD DS.  
  
Domínio raiz da floresta contém os grupos de administradores de empresa e administradores de esquema. Esses grupos de administrador de serviço são usados para gerenciar operações de nível de floresta, como a adição e remoção de domínios e a implementação de alterações no esquema.  
  
Selecionar o domínio raiz da floresta envolve a determinar se um dos domínios do Active Directory no design do seu domínio pode funcionar como domínio raiz da floresta ou se você precisa para implantar um domínio raiz da floresta dedicado.  
  
Para saber mais sobre a implantação de um domínio raiz da floresta, veja [Implantando um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Escolhendo um domínio raiz da floresta regionais ou dedicado  
Se você estiver aplicando um modelo de domínio único, o único domínio funciona como domínio raiz da floresta. Se você estiver aplicando um modelo de domínio múltiplo, você pode optar por implantar um domínio raiz da floresta dedicada ou selecione um domínio regional para funcionar como domínio raiz da floresta.  
  
### <a name="dedicated-forest-root-domain"></a>Domínio raiz da floresta dedicado  
Um domínio raiz da floresta dedicado é um domínio que é criado especificamente para funcionar como a raiz da floresta. Ele não contém nenhuma conta de usuário que não sejam as contas de administrador de serviço para o domínio raiz da floresta. Além disso, ele não representam qualquer região geográfica na sua estrutura de domínio. Todos os outros domínios na floresta são filhos do domínio raiz da floresta dedicado.  
  
Usar uma raiz de floresta dedicada fornece as seguintes vantagens:  
  
-   Operacional separação dos administradores de serviços de floresta de administradores de serviços de domínio. Em um ambiente de domínio único, membros dos grupos de administradores internos e administradores de domínio podem usar as ferramentas de padrão e procedimentos para se tornarem membros dos grupos Administradores corporativos e administradores de esquema. Em uma floresta que usa um domínio raiz da floresta dedicado, membros dos grupos Administradores internos nos domínios regionais e administradores de domínio não podem fazer propriamente ditos membros dos grupos de administrador de serviço de nível de floresta usando procedimentos e as ferramentas padrão.  
  
-   Proteção contra alterações operacionais em outros domínios. Um domínio raiz da floresta dedicada não representam uma determinada região geográfica na sua estrutura de domínio. Por esse motivo, ele não é afetado por reorganizações ou outras alterações que resultam na renomeação ou reestruturação de domínios.  
  
-   Serve como uma raiz neutra para que nenhum país ou região parece ser subordinadas a outra região. Algumas organizações podem preferir para evitar a aparência que um país ou região é subordinada para outro país ou região no namespace. Quando você usa um domínio raiz da floresta dedicado, todos os domínios regionais podem ser pares na hierarquia de domínios.  
  
Em um ambiente de vários domínios de regionais no qual uma raiz de floresta dedicada é usada, a replicação de domínio raiz da floresta tem um impacto mínimo sobre a infraestrutura de rede. Isso ocorre porque a raiz da floresta só hospeda as contas de administrador de serviço. A maioria das contas de usuário na floresta e outros dados específicos de domínio são armazenados nos domínios regionais.  
  
Uma desvantagem usando um domínio raiz da floresta dedicado é que ele cria uma sobrecarga adicional de gerenciamento para dar suporte ao domínio adicional.  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>Domínio regional como um domínio raiz da floresta  
Se você optar por não implantar um domínio raiz da floresta dedicado, você deve selecionar um domínio regional funcionar como domínio raiz da floresta. Neste domínio é o domínio pai de todos os outros domínios regionais e será o primeiro domínio que você implantar. Domínio raiz da floresta contém contas de usuário e é gerenciado da mesma forma que os outros domínios regionais são gerenciados. A principal diferença é que ele também inclui os grupos de administradores de empresa e administradores de esquema.  
  
A vantagem de selecionar um domínio regional à função como domínio raiz da floresta é que ele não cria a sobrecarga de gerenciamento adicionais que mantém um domínio adicional cria. Selecione um domínio regional apropriado para ser a raiz da floresta, como o domínio que representa sua sede ou a região que tem as conexões de rede mais rápidas. Se você tiver dificuldades para sua organização selecionar um domínio regional para ser domínio raiz da floresta, você pode optar por usar um modelo de raiz de floresta dedicada em vez disso.  
  
## <a name="assigning-the-forest-root-domain-name"></a>Atribuindo o nome de domínio de raiz da floresta  
O nome de domínio de raiz da floresta também é o nome da floresta. O nome raiz da floresta é um nome de sistema de nome de domínio (DNS) que consiste em um prefixo e um sufixo na forma de prefix.suffix. Por exemplo, uma organização pode ter o nome raiz da floresta corp.contoso.com. Neste exemplo, corp é o prefixo e contoso.com é o sufixo.  
  
Selecione o sufixo de uma lista de nomes existentes em sua rede. Para o prefixo, selecione um novo nome que não tenha sido usado em sua rede anteriormente. Anexando um novo prefixo a um sufixo existente, você deve criar um namespace exclusivo. Criando um novo namespace para os serviços de domínio do Active Directory (AD DS) garante que qualquer infraestrutura de DNS não precisa ser modificado para acomodar o AD DS.  
  
### <a name="selecting-a-suffix"></a>Selecionando um sufixo  
Para selecionar um sufixo de domínio raiz da floresta:  
  
1.  Contate o proprietário do DNS para a organização para obter uma lista de sufixos DNS registrados que estão em uso na rede que hospedará o AD DS. Observe que os sufixos usados na rede interna podem ser diferentes os sufixos usados externamente. Por exemplo, uma organização pode usar contosopharma.com na Internet e em contoso.com na rede corporativa interna.  
  
2.  Consulte o proprietário do DNS para selecionar um sufixo para uso com o AD DS. Não se existir nenhum sufixo adequado, registre um novo nome com uma autoridade de cadastramento na Internet.  
  
Recomendamos que você use nomes DNS que são registrados com uma autoridade de Internet no namespace do Active Directory. Apenas os nomes registrados têm garantia de estar globalmente exclusivo. Se outra organização mais tarde registra o mesmo nome de domínio DNS (ou se sua organização mescla com, adquire ou é adquirida por outra empresa que usa o mesmo nome DNS), as dois infraestruturas não podem interagir com um ao outro.  
  
> [!CAUTION]  
> Não use nomes DNS de rótulo único. Para obter mais informações, consulte informações sobre como configurar o Windows para domínios com nomes DNS de rótulo único ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)). Além disso, não recomendamos usar sufixos cancelados, como. local.  
  
### <a name="selecting-a-prefix"></a>Selecionando um prefixo  
Se você optar por um sufixo registrado que já está em uso na rede, selecione um prefixo para o nome de domínio de raiz da floresta usando as regras de prefixo na tabela a seguir. Adicione um prefixo que não está sendo usado para criar um novo nome subordinado. Por exemplo, se seu nome de raiz DNS é contoso.com, você pode criar o concorp.contoso.com de nome de domínio do Active Directory floresta raiz se concorp.contoso.com o namespace não estiver em uso na rede. Essa nova ramificação do namespace ser dedicada ao AD DS e pode ser integrada facilmente com a implementação de DNS existente.  
  
Se você selecionou um domínio regional funcionar como um domínio raiz da floresta, talvez seja necessário selecionar um prefixo de novo para o domínio. Como o nome de domínio de raiz da floresta afeta todos os outros nomes de domínio na floresta, um nome regionalmente pode não ser apropriado. Se você estiver usando um novo sufixo que não está atualmente em uso na rede, você pode usá-lo como o nome de domínio de raiz da floresta sem escolhendo um prefixo adicional.  
  
A tabela a seguir lista as regras para selecionar um prefixo para um nome DNS registrado.  
  
|Regra|Explicação|  
|--------|---------------|  
|Selecione um prefixo que não é provável que se tornar desatualizados.|Evite nomes como uma linha de produto ou um sistema operacional que pode ser alterada no futuro. Recomendamos usar nomes genéricos, como corp ou ds.|  
|Selecione um prefixo que inclua apenas a caracteres de padrão de Internet.|À Z, à z, 0-9 e (-), mas não inteiramente numérica.|  
|Incluir 15 caracteres ou menos no prefixo.|Se você escolher um comprimento de prefixo de 15 caracteres ou menos, o nome NetBIOS é o mesmo que o prefixo.|  
  
É importante que o proprietário de DNS do Active Directory trabalhar com o proprietário do DNS para a organização para obter a propriedade do nome que será usado para o namespace do Active Directory. Para obter mais informações sobre a criação de uma infraestrutura DNS para suporte ao AD DS, consulte [criando um Design de infraestrutura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).  
  
## <a name="documenting-the-forest-root-domain-name"></a>Documentar o nome de domínio de raiz da floresta  
Documente os prefixos DNS e sufixos que você selecionar para domínio raiz da floresta. Neste ponto, identifique quais domínio será a raiz da floresta. Você pode adicionar informações de nome de domínio de raiz da floresta à planilha "Domínio planejamento" que você criou para documentar o plano de domínios novos e atualizados e seus nomes de domínio. Para abri-lo, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip do trabalho Aids para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) e abra "Domínio planejamento" (DSSLOGI_5.doc).  
  


