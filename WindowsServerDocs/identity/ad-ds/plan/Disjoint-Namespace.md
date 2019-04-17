---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Namespace separado
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac231dbfbaaeafa39199e29a1744d84d5cec23e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="disjoint-namespace"></a>Namespace separado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um namespace separado ocorre quando um ou mais computadores membros do domínio tem um sufixo primário do serviço de nome de domínio (DNS) que não coincide com o nome DNS do domínio do Active Directory dos quais os computadores são membros. Por exemplo, um computador membro que usa um sufixo DNS primário do corp.fabrikam.com em um domínio do Active Directory denominado na.corp.fabrikam.com está usando um namespace separado.  
  
Um namespace separado é mais complexo para administrar, manter e solucionar problemas de um namespace contínuo. Em um namespace contíguo, o sufixo DNS primário corresponde ao nome de domínio do Active Directory. Aplicativos de rede que são gravados pressupõem que o namespace do Active Directory é idêntico ao sufixo DNS primário para todos os computadores membros do domínio não funcionam corretamente em um namespace separado.  
  
## <a name="support-for-disjoint-namespaces"></a>Suporte para namespaces separado  
Computadores membros do domínio, incluindo controladores de domínio, podem funcionar em um namespace separado. Computadores membros do domínio podem registrar seu host (A) registro de recurso e IP versão 6 (IPv6) hospedam o registro de recurso (AAAA) em um namespace separado do DNS. Quando computadores membros do domínio registrar seus registros de recursos dessa forma, controladores de domínio continuam registrar os registros de recurso global e específica do site Serviços (SRV) na zona DNS que é idêntica ao nome de domínio do Active Directory.  
  
Por exemplo, suponha que um controlador de domínio para o domínio do Active Directory denominado na.corp.fabrikam.com que usa um sufixo DNS primário do corp.fabrikam.com registra host (A) e registros de recurso IPv6 (AAAA) do host na zona corp.fabrikam.com DNS. O controlador de domínio continua a registrar registros de recurso global e específica do site Serviços (SRV) nas msdcs. na.corp.fabrikam.com e na.corp.fabrikam.com zonas DNS, que possibilita o local do serviço.  
  
> [!IMPORTANT]  
> Embora sistemas operacionais Windows possa oferecer suporte a um namespace separado, aplicativos que são gravados pressupõem que o sufixo DNS primário é o mesmo que o sufixo de domínio do Active Directory podem não funcionar nesse ambiente. Por esse motivo, você deve testar todos os aplicativos e seus respectivos sistemas operacionais cuidadosamente antes de implantar um namespace separado.  
  
Um namespace separado deve funcionar (e é compatível) nas seguintes situações:  
  
-   Quando uma floresta com vários domínios do Active Directory usa um único namespace DNS, que também é conhecido como uma zona DNS  
  
    Um exemplo disso é uma empresa que usa domínios regionais com nomes como na.corp.fabrikam.com, sa.corp.fabrikam.com e asia.corp.fabrikam.com e usa um único namespace DNS, como corp.fabrikam.com.  
  
-   Quando um único domínio do Active Directory é dividido em um namespace separado DNS  
  
    Um exemplo disso é uma empresa com um domínio do Active Directory do corp.contoso.com que usa as zonas DNS como hr.corp.contoso.com, production.corp.contoso.com e it.corp.contoso.com.  
  
Um namespace separado não funciona corretamente (e não é suportado) nas seguintes situações:  
  
-   Um sufixo separado usado por membros do domínio corresponde a um nome de domínio do Active Directory na floresta neste ou em outro. Isso interrompe o roteamento de sufixo de nome de Kerberos.  
  
-   O mesmo sufixo separado é usado em outra floresta. Isso impede que esses sufixos de roteamento exclusivamente entre florestas.  
  
-   Quando uma autoridade de certificação de membro do domínio (CA) alterações de servidor é totalmente qualificado (FQDN) do nome de domínio para que ele não usa mais o sufixo DNS primário mesmo que é usado pelos controladores de domínio do domínio ao qual o servidor de CA é um membro. Nesse caso, você pode ter problemas de validação de certificados de servidor da CA que emitiu, dependendo de quais nomes DNS são usados para os pontos de distribuição de CRL. Mas, se você colocar um servidor de autoridade de certificação em um namespace separado estável, ele funciona corretamente e é compatível.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Considerações para namespaces separado  
As considerações a seguir podem ajudá-lo a decidir se você deve usar um namespace separado.  
  
### <a name="application-compatibility"></a>Compatibilidade de aplicativos  
Conforme mencionado anteriormente, um namespace separado pode causar problemas para aplicativos e serviços que são gravados suponha que um sufixo DNS primário do computador é idêntico ao nome do domínio do qual ele é um membro. Antes de implantar um namespace separado, você deve verificar aplicativos para os problemas de compatibilidade. Além disso, certifique-se de verificar a compatibilidade de todos os aplicativos que você usa quando você executa sua análise. Isso inclui aplicativos da Microsoft e de outros desenvolvedores de software.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Vantagens dos namespaces não contíguas.  
Usar um namespace separado pode ter as seguintes vantagens:  
  
-   Como o sufixo DNS primário de um computador pode indicar informações diferentes, você pode gerenciar o namespace DNS separadamente do nome de domínio do Active Directory.  
  
-   Você pode separar o namespace do DNS com base na estrutura de negócios ou localização geográfica. Por exemplo, você pode separar o namespace com base em nomes de unidade de negócios ou local físico como continente, país/região ou criando.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Desvantagens dos namespaces não contíguas.  
Usar um namespace separado pode ter as seguintes desvantagens:  
  
-   Você deve criar e gerenciar zonas DNS separadas para cada domínio do Active Directory na floresta que tenha computadores membro que usam um namespace separado. (Ou seja, ele requer uma configuração adicional e mais complexa.)  
  
-   Você deve executar etapas manuais para modificar e gerenciar o atributo do Active Directory que permite que os membros do domínio usar sufixos DNS primários, especificados.  
  
-   Para otimizar a resolução de nomes, você deve executar etapas manuais para modificar e manter a política de grupo para configurar computadores membro com alternativos sufixos DNS primários.  
  
    > [!NOTE]  
    > O nome do serviço (WINS) pode ser usada para deslocar essa desvantagem pela resolução de nomes de rótulo único. Para obter mais informações sobre o WINS, consulte a referência técnica WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Quando seu ambiente requer vários sufixos DNS primários, você deve configurar a ordem de pesquisa de sufixo DNS para todos os domínios do Active Directory na floresta adequadamente.  
  
    Para definir a ordem de pesquisa de sufixo DNS, você pode usar objetos de política de grupo ou parâmetros de serviço Dynamic Host Configuration servidor DHCP (protocolo). Você também pode modificar o registro.  
  
-   Você deve testar cuidadosamente todos os aplicativos para os problemas de compatibilidade.  
  
Para obter mais informações sobre as etapas que você pode tomar para lidar com essas desvantagens, consulte Criar um Namespace separado ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Planejando uma transição de namespace  
Antes de modificar um namespace, examine as seguintes considerações que se aplicam às transições dos namespaces contíguos para ramificações namespaces (ou vice-versa):  
  
-   Configurados manualmente nomes principais serviço (SPNs) podem não coincidir com nomes DNS após uma alteração de namespace. Isso pode causar falhas de autenticação.  
  
    Para saber mais, veja serviço Logons falhar devido à definir SPNs incorretamente ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Se você usar computadores baseados em Windows Server 2003 com a delegação restrita, esses computadores podem exigir configuração adicional para alterar SPNs. Para obter mais informações, consulte o artigo 936628 na Base de Conhecimento Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Se você quiser delegar permissões Modificar SPNs para subordinadas administradores, consulte Delegando autoridade para modificar SPNs ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Se você usar Lightweight Directory Access Protocol (LDAP) na segura Sockets Layer (SSL) (conhecido como LDAP) com uma autoridade de certificação em uma implantação com controladores de domínio que são definidas em um namespace separado, você deve usar o nome de domínio do Active Directory apropriado e o sufixo DNS primário quando você configura os certificados de LDAP.  
  
    Para obter mais informações sobre os requisitos de certificado de controlador de domínio, consulte o artigo 321051 na Base de Conhecimento Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > Controladores de domínio que usam certificados para LDAP podem exigir que você reimplantar seus certificados. Quando você fizer isso, controladores de domínio não podem selecionar um certificado apropriado, até que sejam reiniciados. Para obter mais informações sobre autenticação LDAP e uma atualização relacionada para o Windows Server 2003, consulte o artigo 932834 na Base de Conhecimento Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Planejamento para implantações de namespace separado  
Se você implantar computadores em um ambiente que tem um namespace separado, tome as seguintes precauções:  
  
1.  Notifica todos os fornecedores de software com os quais você faz negócios que eles devem testar e dar suporte a um namespace separado. Pergunte para verificar que dão suporte a seus aplicativos em ambientes que usam os namespaces não contíguas.  
  
2.  Teste todas as versões dos sistemas operacionais e aplicativos em ambientes de laboratório de namespace não contíguas. Quando você fizer isso, siga estas recomendações:  
  
    1.  Resolva todos os problemas de software antes de implantar o software em seu ambiente.  
  
    2.  Quando possível, participe de testes beta de sistemas operacionais e aplicativos que você planeja implantar nos namespaces não contíguas.  
  
3.  Certifique-se de que os administradores e a equipe de suporte técnico está ciente do namespace separado e seu impacto.  
  
4.  Crie um plano que torna possível para você fazer a transição de um namespace separado para um namespace contíguo, se necessário.  
  
5.  Evangelizar a importância do suporte a namespace separado com o sistema operacional e provedores de aplicativos.  
  


