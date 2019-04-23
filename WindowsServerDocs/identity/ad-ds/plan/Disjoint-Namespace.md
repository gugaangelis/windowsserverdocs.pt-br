---
ms.assetid: d92731f1-e4d8-4223-9b07-ca1f40bb0e1f
title: Namespace não contíguo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a2e911e889343b05a515c94e615d3648289f2df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883567"
---
# <a name="disjoint-namespace"></a>Namespace não contíguo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Um namespace separado ocorre quando um ou mais computadores membros do domínio tem um sufixo primário do serviço de nome de domínio (DNS) que não coincide com o nome DNS do domínio do Active Directory, dos quais os computadores são membros. Por exemplo, um computador que usa um sufixo DNS primário do corp.fabrikam.com em um domínio do Active Directory chamado na.corp.fabrikam.com membro está usando um namespace não contíguo.  
  
Um namespace não contíguo é mais complexo para administrar, manter e solucionar problemas de um namespace adjacente. Em um namespace contíguo, o sufixo DNS primário, corresponde ao nome de domínio do Active Directory. Aplicativos de rede que são gravados a pressupor que o namespace do Active Directory é idêntico ao sufixo DNS primário para todos os computadores de membro de domínio não funcionam corretamente em um namespace não contíguo.  
  
## <a name="support-for-disjoint-namespaces"></a>Suporte para namespaces separados  
Computadores membros do domínio, incluindo controladores de domínio, podem funcionar em um namespace não contíguo. Computadores membros do domínio podem registrar seu host (A) IP versão 6 (IPv6) e registro de recurso de hospedam o registro de recurso (AAAA) em um namespace DNS não contíguo. Quando computadores membros do domínio registra seus registros de recursos dessa forma, os controladores de domínio continuam registrar os registros de recursos globais e específicas do site serviço (SRV) na zona DNS que seja idêntica ao nome de domínio do Active Directory.  
  
Por exemplo, suponha que um controlador de domínio para o domínio do Active Directory chamado na.corp.fabrikam.com que usa um sufixo DNS primário do corp.fabrikam.com registra o host (A) e registros de recursos do IPv6 (AAAA) do host na zona DNS corp.fabrikam.com. O controlador de domínio continua a registrar os registros de recursos globais e específicas do site serviço (SRV) nas zonas DNS _msdcs.na.corp.fabrikam.com e na.corp.fabrikam.com, que possibilita o local do serviço.  
  
> [!IMPORTANT]  
> Embora os sistemas operacionais Windows podem dar suporte a um namespace não contíguo, os aplicativos que são gravados a pressupor que o sufixo DNS primário é o mesmo que o sufixo do domínio do Active Directory podem não funcionar em um ambiente desse tipo. Por esse motivo, você deve testar todos os aplicativos e seus respectivos sistemas operacionais com cuidado antes de implantar um namespace não contíguo.  
  
Um namespace não contíguo deve funcionar (e tem suporte) nas seguintes situações:  
  
-   Quando uma floresta com vários domínios do Active Directory usa um único namespace DNS, que também é conhecida como uma zona DNS  
  
    Um exemplo disso é uma empresa que usa domínios regionais com nomes como na.corp.fabrikam.com, sa.corp.fabrikam.com e asia.corp.fabrikam.com e usa um único namespace DNS, como corp.fabrikam.com.  
  
-   Quando um único domínio do Active Directory é dividido em namespaces separados do DNS  
  
    Um exemplo disso é uma empresa com um domínio do Active Directory de corp.contoso.com que use zonas de DNS como hr.corp.contoso.com, production.corp.contoso.com e it.corp.contoso.com.  
  
Um namespace não contíguo não funciona corretamente (e não há suporte para) nas seguintes situações:  
  
-   Um sufixo separado usado por membros do domínio corresponde a um nome de domínio do Active Directory na floresta neste ou em outro. Isso interrompe o roteamento de sufixo de nome de Kerberos.  
  
-   O mesmo sufixo separado é usado em outra floresta. Isso impede que o roteamento esses sufixos exclusivamente entre florestas.  
  
-   Quando um servidor de autoridade (CA) de certificação de membro do domínio altera seu nome de domínio totalmente qualificado (FQDN) para que ele não for mais usar o mesmo sufixo DNS primário que é usado pelos controladores de domínio do domínio ao qual o servidor de autoridade de certificação é um membro. Nesse caso, você pode ter problemas ao validar certificados de servidor da autoridade de certificação emitido, dependendo de quais nomes DNS são usados em pontos de distribuição da CRL. Mas, se você colocar um servidor de autoridade de certificação em um namespace não contíguo estável, ele funciona corretamente e é suportado.  
  
## <a name="considerations-for-disjoint-namespaces"></a>Considerações para namespaces separados  
As considerações a seguir podem ajudá-lo a decidir se deve usar um namespace não contíguo.  
  
### <a name="application-compatibility"></a>Compatibilidade com aplicativos  
Conforme mencionado anteriormente, um namespace não contíguo pode causar problemas para quaisquer aplicativos e serviços que são gravados a pressupor que um sufixo DNS primário do computador é idêntico ao nome do nome de domínio do qual ele é um membro. Antes de implantar um namespace não contíguo, você deve verificar aplicativos para problemas de compatibilidade. Além disso, certifique-se de verificar a compatibilidade de todos os aplicativos que você usa quando você executa sua análise. Isso inclui aplicativos da Microsoft e de outros desenvolvedores de software.  
  
### <a name="advantages-of-disjoint-namespaces"></a>Vantagens de namespaces separados  
Usar um namespace não contíguo pode ter as seguintes vantagens:  
  
-   Porque o sufixo DNS primário de um computador pode indicar informações diferentes, você pode gerenciar o namespace DNS separadamente do nome de domínio do Active Directory.  
  
-   Você pode separar o namespace DNS com base na estrutura de negócios ou localização geográfica. Por exemplo, você pode separar o namespace com base em nomes de unidades de negócios ou local físico, como continente, país/região ou construção.  
  
### <a name="disadvantages-of-disjoint-namespaces"></a>Desvantagens de namespaces separados  
Usar um namespace não contíguo pode ter as seguintes desvantagens:  
  
-   Você deve criar e gerenciar zonas DNS separadas para cada domínio do Active Directory na floresta que tenha computadores membros que usam um namespace não contíguo. (Ou seja, ele requer uma configuração adicional e mais complexa.)  
  
-   Você deve executar etapas manuais para modificar e gerenciar o atributo do Active Directory que permite que os membros do domínio usar sufixos DNS especificados, o primários.  
  
-   Para otimizar a resolução de nomes, você deve executar etapas manuais para modificar e manter a diretiva de grupo para configurar os computadores membros com sufixos DNS primários alternativos.  
  
    > [!NOTE]  
    > O Windows Internet nome WINS (serviço) pode ser usado para deslocar essa desvantagem Resolvendo nomes de rótulo único. Para obter mais informações sobre o WINS, consulte a referência técnica do WINS ([https://go.microsoft.com/fwlink/?LinkId=102303](https://go.microsoft.com/fwlink/?LinkId=102303)).  
  
-   Quando seu ambiente requer vários sufixos DNS primários, você deve configurar adequadamente a ordem de pesquisa de sufixo DNS para todos os domínios do Active Directory na floresta.  
  
    Para definir a ordem de pesquisa de sufixo DNS, você pode usar objetos de diretiva de grupo ou parâmetros de serviço do servidor de protocolo de configuração de Host dinâmico (DHCP). Você também pode modificar o registro.  
  
-   Você deve testar cuidadosamente todos os aplicativos para problemas de compatibilidade.  
  
Para obter mais informações sobre as etapas que você pode tomar para lidar com essas desvantagens, consulte Criar um Namespace não contíguo ([https://go.microsoft.com/fwlink/?LinkId=106638](https://go.microsoft.com/fwlink/?LinkId=106638)).  
  
### <a name="planning-a-namespace-transition"></a>Planejando uma transição de namespace  
Antes de modificar um namespace, examine as seguintes considerações se aplicam a transições de namespaces contíguos a disjunção namespaces (ou vice-versa):  
  
-   Configurado manualmente de nomes de entidade de serviço (SPNs) talvez não correspondem mais nomes DNS após uma alteração de namespace. Isso pode causar falhas de autenticação.  
  
    Para obter mais informações, consulte serviço Logons falhar devido a definir SPNs incorretamente ([https://go.microsoft.com/fwlink/?LinkId=102304](https://go.microsoft.com/fwlink/?LinkId=102304)).  
  
    -   Se você usar computadores baseados no Windows Server 2003 com a delegação restrita, esses computadores podem exigir configuração adicional para alterar os SPNs. Para obter mais informações, consulte o artigo 936628 na Base de dados de Conhecimento Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102306](https://go.microsoft.com/fwlink/?LinkId=102306)).  
  
    -   Se você deseja delegar permissões para modificar os SPNs para subordinada administradores, consulte Delegando autoridade para modificar SPNs ([https://go.microsoft.com/fwlink/?LinkId=106639](https://go.microsoft.com/fwlink/?LinkId=106639)).  
  
-   Se você usar LDAP Lightweight Directory Access Protocol () sobre SSL Secure Sockets Layer () (conhecido como LDAPS) com uma autoridade de certificação em uma implantação que tem controladores de domínio que são configuradas em um namespace não contíguo, você deve usar o nome de domínio do Active Directory apropriado e sufixo DNS primário quando você configura os certificados LDAPS.  
  
    Para obter mais informações sobre requisitos de certificado do controlador de domínio, consulte o artigo 321051 na Base de dados de Conhecimento Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102307](https://go.microsoft.com/fwlink/?LinkId=102307)).  
  
    > [!NOTE]  
    > Controladores de domínio que usam certificados para LDAPS podem exigir a reimplantar seus certificados. Quando você fizer isso, controladores de domínio não podem selecionar um certificado apropriado, até que sejam reiniciados. Para obter mais informações sobre a autenticação de LDAPS e relacionadas uma atualização para o Windows Server 2003, consulte o artigo 932834 na Base de dados de Conhecimento Microsoft ([https://go.microsoft.com/fwlink/?LinkId=102308](https://go.microsoft.com/fwlink/?LinkId=102308)).  
  
### <a name="planning-for-disjoint-namespace-deployments"></a>Planejamento para implantações de namespace não contíguo  
Se você implantar computadores em um ambiente que tem um namespace não contíguo, tome as seguintes precauções:  
  
1.  Notificar todos os fornecedores de software com quem você fazer negócios que precisam testar e dar suporte a um namespace não contíguo. Solicite a verificação de suporte de seus aplicativos em ambientes que usam namespaces separados.  
  
2.  Teste todas as versões de sistemas operacionais e aplicativos em ambientes de laboratório do namespace não contíguo. Quando você fizer isso, siga estas recomendações:  
  
    1.  Resolva todos os problemas de software antes de implantar o software em seu ambiente.  
  
    2.  Quando possível, participe de testes beta de sistemas operacionais e aplicativos que você planeja implantar nos namespaces não contíguos.  
  
3.  Certifique-se de que os administradores e a equipe de suporte técnico está ciente do namespace não contíguo e seu impacto.  
  
4.  Crie um plano que possibilita que você faça a transição de um namespace não contíguo a um namespace contíguo, se necessário.  
  
5.  Evangelizar a importância do suporte de namespace não contíguo com provedores de aplicativos e sistemas operacionais.  
  


