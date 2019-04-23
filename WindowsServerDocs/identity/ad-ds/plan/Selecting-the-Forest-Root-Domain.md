---
ms.assetid: ef4ef4a9-8969-4ad0-bd17-b2bb24f36ef6
title: Selecionando o domínio raiz da floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c669a5c8103fba52140147957823db978f8b6955
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871347"
---
# <a name="selecting-the-forest-root-domain"></a>Selecionando o domínio raiz da floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O primeiro domínio que você implanta em uma floresta do Active Directory é chamado de domínio raiz da floresta. Este domínio permanece o domínio raiz da floresta para o ciclo de vida da implantação do AD DS.  
  
Domínio raiz da floresta contém os grupos Administradores corporativos e administradores de esquema. Esses grupos de administrador de serviço são usados para gerenciar operações de nível de floresta, como a adição e remoção de domínios e a implementação de alterações no esquema.  
  
Selecionar o domínio raiz da floresta consiste em determinar se um dos domínios do Active Directory em seu design de domínio pode funcionar como o domínio raiz da floresta ou se você precisa implantar um domínio raiz de floresta dedicada.  
  
Para obter informações sobre como implantar um domínio raiz da floresta, consulte [implantação de um domínio raiz de floresta do Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  
## <a name="choosing-a-regional-or-dedicated-forest-root-domain"></a>Escolher um domínio raiz de floresta dedicada ou regionais

Se você estiver aplicando um modelo de domínio único, o único domínio funciona como o domínio raiz da floresta. Se você estiver aplicando um modelo de domínio múltiplo, você pode optar por implantar um domínio raiz de floresta dedicada ou selecione um domínio regional para funcionar como o domínio raiz da floresta.  
  
### <a name="dedicated-forest-root-domain"></a>Domínio de raiz de floresta dedicada

Um domínio raiz de floresta dedicada é um domínio que é criado especificamente para funcionar como a raiz da floresta. Ele não contém quaisquer contas de usuário que não sejam as contas de administrador de serviço para o domínio raiz da floresta. Além disso, ele não representa qualquer região geográfica em sua estrutura de domínio. Todos os outros domínios na floresta são filhos do domínio raiz da floresta dedicada.  

Usando uma raiz de floresta dedicada fornece as seguintes vantagens:  

- Separação operacional floresta de administradores de serviço de administradores de serviços de domínio. Em um ambiente de domínio único, os membros dos grupos de administradores internos e Admins. do domínio podem usar ferramentas padrão e os procedimentos para se tornarem membros dos grupos Administradores corporativos e administradores de esquema. Em uma floresta que usa um domínio raiz de floresta dedicada, os membros dos grupos Administradores embutidos nos domínios regionais e Admins. do domínio não podem se tornar membros de grupos de administradores de serviço de nível de floresta usando procedimentos e ferramentas padrão.  
- Proteção contra alterações operacionais em outros domínios. Um domínio raiz de floresta dedicada não representa uma determinada região geográfica em sua estrutura de domínio. Por esse motivo, ele não é afetado por reorganizações ou outras alterações que resultam na renomeação ou reestruturação de domínios.  
- Serve como uma raiz neutra para que nenhum país ou região parece ser subordinado a outra região. Algumas organizações podem preferir evitar a aparência que um país ou região é subordinada a outro país ou região no namespace. Quando você usa um domínio raiz de floresta dedicada, todos os domínios regionais podem estejam no mesmo nível na hierarquia de domínios.  

Em um ambiente de vários domínios de regional em que uma raiz de floresta dedicada é usada, a replicação de domínio raiz da floresta tem impacto mínimo sobre a infraestrutura de rede. Isso ocorre porque a raiz da floresta apenas hospeda as contas de administrador de serviço. A maioria das contas de usuário na floresta e outros dados específicos de domínio são armazenados nos domínios regionais.  
  
Uma desvantagem de usar um domínio raiz de floresta dedicada é que ele cria a sobrecarga de gerenciamento adicional para dar suporte o domínio adicional.  
  
### <a name="regional-domain-as-a-forest-root-domain"></a>Domínio regional, como um domínio raiz da floresta

Se você optar por não implantar um domínio raiz de floresta dedicada, você deve selecionar um domínio regional para funcionar como o domínio raiz da floresta. Este domínio é o domínio pai de todos os outros domínios regionais e será o primeiro domínio que você implanta. Domínio raiz da floresta contém contas de usuário e é gerenciado da mesma maneira que outros domínios regionais são gerenciados. A principal diferença é que ele também inclui os grupos Administradores corporativos e administradores de esquema.  
  
Cria a vantagem da seleção de um domínio regional para a função como domínio raiz da floresta é que ele não cria a sobrecarga de gerenciamento adicionais que manter um domínio adicional. Selecione um domínio regional apropriado para ser a raiz da floresta, como o domínio que representa sua sede ou a região que tem as conexões de rede mais rápidas. Se você tiver dificuldades para sua organização selecionar um domínio regional para ser o domínio raiz da floresta, você pode optar por usar um modelo de raiz de floresta dedicada em vez disso.  
  
## <a name="assigning-the-forest-root-domain-name"></a>Atribuir o nome de domínio de raiz da floresta

O nome de domínio de raiz da floresta também é o nome da floresta. O nome de raiz da floresta é um nome de sistema de nome de domínio (DNS) que consiste em um prefixo e um sufixo na forma de prefix.suffix. Por exemplo, uma organização pode ter o corp.contoso.com de nome de raiz de floresta. Neste exemplo, corp é o prefixo e contoso.com é o sufixo.  
  
Selecione o sufixo de uma lista dos nomes existentes em sua rede. Para o prefixo, selecione um novo nome não foi usado em sua rede anteriormente. Ao anexar um novo prefixo a um sufixo existente, você deve criar um namespace exclusivo. Criando um novo namespace para os serviços de domínio do Active Directory (AD DS) garante que toda a infra-estrutura DNS existente não precisa ser modificado para acomodar o AD DS.  
  
### <a name="selecting-a-suffix"></a>Selecionando um sufixo

Para selecionar um sufixo para o domínio raiz da floresta:  
  
1. Entre em contato com o proprietário do DNS para a organização para obter uma lista de sufixos DNS registrados que estão em uso na rede que hospedará o AD DS. Observe que os sufixos usados na rede interna podem ser diferentes os sufixos usados externamente. Por exemplo, uma organização pode usar contosopharma.com na Internet e contoso.com na rede corporativa interna.  
  
2. Consulte o proprietário do DNS para selecionar um sufixo para uso com o AD DS. Não se existir nenhum sufixo adequado, registre um novo nome com uma autoridade de cadastramento na Internet.  
  
É recomendável que você use nomes DNS que são registrados com uma autoridade de Internet no namespace do Active Directory. Somente os nomes registrados têm garantia de ser globalmente exclusivo. Se outra organização posteriormente registra o mesmo nome de domínio DNS (ou se mescla com sua organização, adquire ou é adquirida por outra empresa que usa o mesmo nome DNS), duas infraestruturas não podem interagir entre si.  
  
> [!CAUTION]  
> Não use nomes DNS de rótulo único. Para obter mais informações, consulte informações sobre como configurar o Windows para domínios com nomes DNS de rótulo único ([https://go.microsoft.com/fwlink/?LinkId=106631](https://go.microsoft.com/fwlink/?LinkId=106631)). Além disso, não recomendamos usar sufixos de cancelar o registro, como. local.  
  
### <a name="selecting-a-prefix"></a>Selecionando um prefixo

Se você escolher um sufixo registrado que já está em uso na rede, selecione um prefixo para o nome de domínio de raiz de floresta usando as regras de prefixo na tabela a seguir. Adicione um prefixo que não está atualmente em uso para criar um novo nome subordinado. Por exemplo, se seu nome DNS de raiz for contoso.com, você pode criar concorp.contoso.com de nome de domínio de raiz para floresta do Active Directory se o namespace concorp.contoso.com não ainda estiver em uso na rede. Essa nova branch do namespace será dedicado ao AD DS e pode ser integrado facilmente com a implementação de DNS existente.  
  
Se você tiver selecionado um domínio regional para funcionar como um domínio raiz da floresta, você precisa selecionar um novo prefixo para o domínio. Como o nome de domínio de raiz de floresta afeta todos os outros nomes de domínio na floresta, um nome regionalmente pode não ser apropriado. Se você estiver usando um novo sufixo que não está atualmente em uso na rede, você pode usá-lo como o nome de domínio de raiz de floresta sem escolher um prefixo adicional.  
  
A tabela a seguir lista as regras para a seleção de um prefixo para um nome DNS registrado.  
  
|Regra|Explicação|  
|--------|---------------|  
|Selecione um prefixo que não provavelmente fiquem desatualizados.|Evite nomes como uma linha de produto ou sistema operacional que pode ser alterado no futuro. É recomendável usar nomes genéricos como corp ou ds.|  
|Selecione um prefixo que inclui apenas a caracteres de padrão de Internet.|A-Z, a-z, 0 a 9 e (-), mas não totalmente numérica.|  
|Incluir 15 caracteres ou menos no prefixo.|Se você escolher um tamanho de prefixo de 15 caracteres ou menos, o nome NetBIOS é o mesmo que o prefixo.|  
  
É importante para o proprietário do DNS do Active Directory para trabalhar com o proprietário do DNS para a organização para obter a propriedade do nome que será usado para o namespace do Active Directory. Para obter mais informações sobre como projetar uma infraestrutura de DNS para dar suporte ao AD DS, consulte [criar um Design de infraestrutura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).  
  
## <a name="documenting-the-forest-root-domain-name"></a>Documentando o nome de domínio de raiz da floresta

Documente o prefixo DNS e o sufixo que você selecionar para o domínio raiz da floresta. Neste ponto, identifique qual domínio será a raiz da floresta. Você pode adicionar as informações de nome de domínio de raiz de floresta para a planilha "Planejando a domínio" que você criou para documentar seu plano para domínios de novos e atualizados e seus nomes de domínio. Para abri-lo, baixe Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip partir [trabalho auxílios para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) e abra "Planejamento de domínio" (DSSLOGI_5.doc).
