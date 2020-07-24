---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Namespace não contíguo
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 6743a5fb902509d24f8f30d42b919e65fa40ccc0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965688"
---
# <a name="disjoint-namespace"></a>Namespace não contíguo

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um namespace separado ocorre quando um ou mais computadores membros do domínio têm um sufixo de DNS (serviço de nomes de domínio) primário que não corresponde ao nome DNS do domínio de Active Directory do qual os computadores são membros. Por exemplo, um computador membro que usa um sufixo DNS primário de corp.fabrikam.com em um domínio Active Directory chamado na.corp.fabrikam.com está usando um namespace não contíguo.

Um namespace não contíguo é mais complexo de administrar, manter e solucionar problemas do que um namespace contíguo. Em um namespace contíguo, o sufixo DNS primário corresponde ao nome de domínio Active Directory. Aplicativos de rede que são escritos para pressupor que o namespace de Active Directory é idêntico ao sufixo DNS primário para todos os computadores membros do domínio não funcionam corretamente em um namespace não contíguo.

## <a name="support-for-disjoint-namespaces"></a>Suporte para namespaces não contíguos

Os computadores membros do domínio, incluindo controladores de domínio, podem funcionar em um namespace não contíguo. Os computadores membros do domínio podem registrar seu registro de recurso de host (A) e o registro de recurso de host (AAAA) do IP versão 6 (IPv6) em um namespace DNS não contíguo. Quando os computadores membros do domínio registram seus registros de recursos dessa forma, os controladores de domínio continuam a registrar os registros de recurso SRV (serviços globais e específicos do site) na zona DNS que é idêntica à Active Directory nome de domínio.

Por exemplo, suponha que um controlador de domínio para o domínio Active Directory chamado na.corp.fabrikam.com que usa um sufixo DNS primário de corp.fabrikam.com registra os registros de recurso host (A) e IPv6 (AAAA) na zona DNS corp.fabrikam.com. O controlador de domínio continua a registrar registros de recursos de serviço (SRV) globais e específicos do site nas zonas DNS _msdcs. na. Corp. fabrikam. com e na.corp.fabrikam.com, o que torna possível o local do serviço.

> [!IMPORTANT]
> Embora os sistemas operacionais Windows possam dar suporte a um namespace não contíguo, os aplicativos que são escritos para pressupor que o sufixo DNS primário é o mesmo que o Active Directory sufixo de domínio pode não funcionar nesse ambiente. Por esse motivo, você deve testar todos os aplicativos e seus respectivos sistemas operacionais cuidadosamente antes de implantar um namespace não contíguo.

Um namespace não contíguo deve funcionar (e tem suporte) nas seguintes situações:

- Quando uma floresta com vários domínios de Active Directory usa um único namespace DNS, que também é conhecido como uma zona DNS

    Um exemplo disso é uma empresa que usa domínios regionais com nomes como na.corp.fabrikam.com, sa.corp.fabrikam.com e asia.corp.fabrikam.com e usa um único namespace DNS, como corp.fabrikam.com.

- Quando um único domínio de Active Directory é dividido em namespaces DNS separados

    Um exemplo disso é uma empresa com um Active Directory domínio de corp.contoso.com que usa zonas DNS, como hr.corp.contoso.com, production.corp.contoso.com e it.corp.contoso.com.

Um namespace não contíguo não funciona corretamente (e não tem suporte) nas seguintes situações:

- Um sufixo não contíguo usado por membros do domínio corresponde a um Active Directory nome de domínio nesta ou em outra floresta. Isso interrompe o roteamento de sufixo de nome Kerberos.

- O mesmo sufixo não contíguo é usado em outra floresta. Isso impede o roteamento desses sufixos exclusivamente entre florestas.

- Quando um servidor de autoridade de certificação (CA) membro do domínio altera seu FQDN (nome de domínio totalmente qualificado) para que ele não use mais o mesmo sufixo DNS primário usado pelos controladores de domínio do domínio ao qual o servidor de CA é membro. Nesse caso, você pode ter problemas ao validar certificados que o servidor de CA emitiu, dependendo de quais nomes DNS são usados nos pontos de distribuição da CRL. Mas se você posicionar um servidor de AC em um namespace não contíguo estável, ele funcionará corretamente e terá suporte.

## <a name="considerations-for-disjoint-namespaces"></a>Considerações para namespaces não contíguos

As considerações a seguir podem ajudá-lo a decidir se deve usar um namespace não contíguo.

### <a name="application-compatibility"></a>Compatibilidade de aplicativos

Como mencionado anteriormente, um namespace não contíguo pode causar problemas para aplicativos e serviços que são escritos para assumir que um sufixo DNS primário do computador é idêntico ao nome do nome de domínio do qual ele é membro. Antes de implantar um namespace não contíguo, você deve verificar os aplicativos quanto a problemas de compatibilidade. Além disso, certifique-se de verificar a compatibilidade de todos os aplicativos que você usa ao executar a análise. Isso inclui aplicativos da Microsoft e de outros desenvolvedores de software.

### <a name="advantages-of-disjoint-namespaces"></a>Vantagens de namespaces não contíguos

O uso de um namespace não contíguo pode ter as seguintes vantagens:

- Como o sufixo DNS primário de um computador pode indicar informações diferentes, você pode gerenciar o namespace DNS separadamente do nome de domínio Active Directory.

- Você pode separar o namespace DNS com base na estrutura de negócios ou na localização geográfica. Por exemplo, você pode separar o namespace com base nos nomes de unidades de negócios ou no local físico, como continente, país/região ou compilação.

### <a name="disadvantages-of-disjoint-namespaces"></a>Desvantagens de namespaces não contíguos

O uso de um namespace não contíguo pode ter as seguintes desvantagens:

- Você deve criar e gerenciar zonas DNS separadas para cada domínio Active Directory na floresta que tem computadores membros que usam um namespace não contíguo. (Ou seja, requer uma configuração adicional e mais complexa.)

- Você deve executar etapas manuais para modificar e gerenciar o atributo Active Directory que permite que os membros do domínio usem sufixos DNS primários especificados.

- Para otimizar a resolução de nomes, você deve executar etapas manuais para modificar e manter Política de Grupo para configurar computadores membros com sufixos DNS primários alternativos.

> [!NOTE]
> O WINS (serviço de cadastramento na Internet do Windows) pode ser usado para deslocar essa desvantagem resolvendo nomes de rótulo único. Para obter mais informações sobre o WINS, consulte a [referência técnica do WINS](/previous-versions/windows/it-pro/windows-server-2003/cc736411(v=ws.10)).

- Quando seu ambiente requer vários sufixos DNS primários, você deve configurar a ordem de pesquisa de sufixo DNS para todos os domínios de Active Directory na floresta adequadamente.

    Para definir a ordem de pesquisa de sufixo DNS, você pode usar Política de Grupo objetos ou os parâmetros de serviço do servidor DHCP (protocolo de configuração dinâmica de hosts). Você também pode modificar o registro.

- Você deve testar cuidadosamente todos os aplicativos para problemas de compatibilidade.

Para obter mais informações sobre as etapas que você pode tomar para resolver essas desvantagens, consulte [criar um namespace não contíguo](/previous-versions/windows/it-pro/windows-server-2003/cc755926(v=ws.10)).

### <a name="planning-a-namespace-transition"></a>Planejando uma transição de namespace

Antes de modificar um namespace, examine as seguintes considerações, que se aplicam a transições de namespaces contíguos para namespaces separados (ou o inverso):

- Os SPNs (nomes da entidade de serviço) configurados manualmente podem não corresponder mais aos nomes DNS após uma alteração no namespace. Isso pode causar falhas de autenticação.

    Para obter mais informações, consulte [falhas de logons de serviço devido a SPNs incorretamente definidos](/previous-versions/windows/it-pro/windows-server-2003/cc772897(v=ws.10)).

    - Se você usar computadores baseados no Windows Server 2003 com delegação restrita, esses computadores poderão exigir configuração adicional para alterar os SPNs. Para obter mais informações, consulte o artigo 936628 na base de dados de conhecimento Microsoft, [o SPN não aparece na lista de serviços que podem ser delegados a uma conta quando você tenta configurar a delegação restrita em um computador que esteja executando o Windows Server 2003](https://support.microsoft.com/help/936628) (404).

    - Se você quiser delegar permissões para modificar os SPNs para administradores subordinados, consulte [delegando autoridade para modificar SPNs](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770439(v=ws.10)).

- Se você usar o protocolo LDAP (Lightweight Directory Access Protocol) sobre protocolo SSL (SSL) (conhecido como LDAPs) com uma CA em uma implantação que tem controladores de domínio configurados em um namespace não contíguo, deverá usar o nome de domínio Active Directory apropriado e o sufixo DNS primário ao configurar os certificados LDAPs.

    Para obter mais informações sobre os requisitos de certificado do controlador de domínio, consulte o artigo 321051 na base de dados de conhecimento Microsoft, [como habilitar LDAP sobre SSL com uma autoridade de certificação](https://support.microsoft.com/help/321051/)de terceiros.

    > [!NOTE]
    > Os controladores de domínio que usam certificados para LDAPs podem exigir que você reimplante seus certificados. Quando você fizer isso, os controladores de domínio poderão não selecionar um certificado apropriado até que sejam reiniciados. Para obter mais informações sobre a autenticação LDAP (Lightweight Directory Access Protocol) sobre protocolo SSL (LDAP)) para o Windows Server 2003, consulte o artigo 938703 na base de dados de conhecimento Microsoft, [como solucionar problemas de conexão LDAP sobre SSL](https://support.microsoft.com/help/938703/).

### <a name="planning-for-disjoint-namespace-deployments"></a>Planejando implantações de namespace não contíguos

Tome as seguintes precauções se implantar computadores em um ambiente que tenha um namespace não contíguo:

1. Notifique todos os fornecedores de software com os quais você faz negócios que eles devem testar e dar suporte a um namespace não contíguo. Peça-lhes para verificar se eles dão suporte a seus aplicativos em ambientes que usam namespaces não contíguos.

2. Testar todas as versões de sistemas operacionais e aplicativos em ambientes de laboratório de namespace não contíguos. Ao fazer isso, siga estas recomendações:

    1. Resolva todos os problemas de software antes de implantar o software em seu ambiente.

    2. Quando possível, participe de testes beta de sistemas operacionais e aplicativos que você planeja implantar em namespaces não contíguos.

3. Verifique se os administradores e a equipe de assistência técnica estão cientes do namespace não contíguo e seu impacto.

4. Crie um plano que torna possível a transição de um namespace separado para um namespace contíguo, se necessário.

5. Evangelizar a importância do suporte a namespaces não contíguos com o sistema operacional e os provedores de aplicativos.
