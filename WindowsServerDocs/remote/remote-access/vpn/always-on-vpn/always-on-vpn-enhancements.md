---
title: Aprimoramentos de VPN Always On
description: A VPN Always On tem muitos benefícios em relação às soluções de VPN do Windows do passado. As principais melhorias na integração, segurança, conectividade, controle de rede e compatibilidade são alinhadas Always On VPN com a visão em nuvem, primeiro e móvel da Microsoft.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.author: v-tea
author: Teresa-MOTIV
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: e20d59311d8bc21052855acae9fc2eb356fdff23
ms.sourcegitcommit: 717222e9efceb5964872dbf97034cad60f3c48df
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87295047"
---
# <a name="always-on-vpn-enhancements"></a>Aprimoramentos de VPN Always On

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

- [**Anterior:** Saiba mais sobre os recursos de VPN Always On](../vpn-map-da.md)
- [**Em seguida:** Saiba mais sobre a tecnologia de VPN Always On](always-on-vpn-technology-overview.md)

A VPN Always On tem muitos benefícios em relação às soluções de VPN do Windows do passado. Os principais aprimoramentos a seguir alinham Always On VPN com a visão de dispositivos móveis e primeiro em nuvem da Microsoft:

- **Integração de plataforma:** A VPN Always On melhorou a integração com o sistema operacional Windows e soluções de terceiros para fornecer uma plataforma robusta para inúmeros cenários de conexão avançados.

- **Segurança:** A VPN Always On tem recursos de segurança novos e avançados para restringir o tipo de tráfego, quais aplicativos podem usar a conexão VPN e quais métodos de autenticação você pode usar para iniciar a conexão. Quando a conexão está ativa na maioria das vezes, é especialmente importante proteger a conexão. Para obter mais detalhes, consulte [Opções de autenticação de VPN](/windows/security/identity-protection/vpn/vpn-authentication).

- **Conectividade VPN:** Always On VPN, com ou sem o túnel de dispositivo fornece o recurso de gatilho automático. Antes de Always On VPN, a capacidade de disparar uma conexão automática por meio de autenticação de usuário ou de dispositivo não era possível.  

- **Controle de rede:** Always On VPN permite que os administradores especifiquem as políticas de roteamento em um nível mais granular, até mesmo no aplicativo individual, que é perfeito para aplicativos de linha de negócios (LOB) que exigem acesso remoto especial.  Always On VPN também é totalmente compatível com o protocolo IP versão 4 (IPv4) e a versão 6 (IPv6). Ao contrário do DirectAccess, não há nenhuma dependência específica no IPv6.

  >[!NOTE]
  >Antes de começar, certifique-se de habilitar o IPv6 no servidor VPN. Caso contrário, uma conexão não pode ser estabelecida e uma mensagem de erro é exibida.

- **Configuração e compatibilidade:** A VPN Always On pode ser implantada e gerenciada de várias maneiras, o que dá Always On VPN várias vantagens em relação ao outro software cliente VPN.

## <a name="platform-integration"></a>Integração de plataforma

A Microsoft introduziu ou aprimorou os seguintes recursos de integração no Always On VPN:


| Aprimoramento da chave   |  Descrição  |
|----------------|---|
| **[Proteção de Informações do Windows (WIP)](/windows/threat-protection/windows-information-protection/protect-enterprise-data-using-wip)** | A integração com WIP permite a imposição de diretiva de rede para determinar se o tráfego tem permissão para passar pela VPN. Se o perfil do usuário estiver ativo e as políticas de WIP forem aplicadas, Always On VPN será disparada automaticamente para se conectar. Além disso, quando você usa o WIP, não é necessário especificar as regras AppTriggerList e TrafficFilterList separadamente no perfil de VPN (a menos que você queira uma configuração mais avançada), pois as políticas de WIP e as listas de aplicativos entram em vigor automaticamente. |
|**[Windows Hello for Business](/windows/access-protection/hello-for-business/hello-overview)** |Always On VPN nativamente dá suporte ao Windows Hello para empresas (no modo de autenticação baseada em certificado) para fornecer uma experiência de logon único direta para entrada no computador e conexão com a VPN. Portanto, nenhuma autenticação secundária (credenciais de usuário) é necessária para a conexão VPN, possibilitando o uso de uma conexão Always On com a autenticação do Windows Hello para empresas. |
| **[Microsoft Azure o acesso condicional](/azure/active-directory/active-directory-conditional-access-controls)**  |O cliente VPN Always On pode ser integrado à plataforma de acesso condicional do Azure para impor a MFA (autenticação multifator), a conformidade do dispositivo ou uma combinação dos dois. Quando em conformidade com as políticas de acesso condicional, o Azure Active Directory (Azure AD) emite um certificado de autenticação IPsec (segurança IP) de curta duração (por padrão, 60 minutos) que pode ser usado para autenticar o gateway de VPN. A conformidade do dispositivo usa políticas de conformidade do Configuration Manager/Intune, que podem incluir o estado do atestado de integridade do dispositivo como parte da verificação de conformidade da conexão.|
|  **MFA do Azure** |Quando combinado com serviços de serviço RADIUS (RADIUS) e a extensão do NPS (servidor de políticas de rede) para o Azure MFA, a autenticação VPN pode usar MFA forte. | **Plug-in de VPN de terceiros**  | Com o Plataforma Universal do Windows (UWP), provedores de VPN de terceiros podem criar um único aplicativo para toda a gama de dispositivos Windows 10. A UWP fornece uma camada de API principal garantida entre dispositivos, eliminando a complexidade e os problemas frequentemente associados à gravação de drivers de nível de kernel. Atualmente, os plug-ins de VPN UWP do Windows 10 existem para o [Pulse Secure](https://www.microsoft.com/p/pulse-secure/9nblggh3b0bp), o [acesso F5](https://www.microsoft.com/p/f5-access/9wzdncrdsfn0), o [Check Point cápsula VPN](https://www.microsoft.com/p/check-point-capsule-vpn/9wzdncrdjxtj), o [FortiClient](https://www.microsoft.com/p/forticlient/9wzdncrdh6mc), o [SonicWALL Mobile Connect](https://www.microsoft.com/p/sonicwall-mobile-connect/9wzdncrdsfkz)e o [GlobalProtect](https://www.microsoft.com/p/globalprotect/9nblggh6bzl3); sem dúvida, outros serão exibidos no futuro. |

## <a name="security"></a>Segurança

As principais melhorias na segurança estão nas seguintes áreas:

| Aprimoramento da chave | Descrição  |
|---|---|
| **Filtros de tráfego** | Por meio de filtros de tráfego, você pode especificar políticas do lado do cliente que determinam qual tráfego é permitido para a rede corporativa. Dessa forma, os administradores podem aplicar restrições de aplicativo ou de tráfego na interface VPN, limitando seu uso a fontes específicas, portas de destino e endereços IP. Dois tipos de regras de filtragem estão disponíveis:<ul><li>**Regras baseadas em aplicativo.** As regras de firewall baseadas em aplicativo são baseadas em uma lista de aplicativos especificados para que somente o tráfego originado desses aplicativos tenha permissão para passar pela interface VPN.</li><li>**Regras baseadas em tráfego.** As regras de firewall baseadas em tráfego são baseadas em requisitos de rede, como portas, endereços e protocolos. Use estas regras somente para o tráfego que corresponde a essas condições específicas que têm permissão para passar pela interface VPN.<p><p>***Observação***:<br>Essas regras se aplicam somente ao tráfego de saída do dispositivo. O uso de filtros de tráfego bloqueia o tráfego de entrada da rede corporativa para o cliente. </li></ul> |**VPN por aplicativo**|A VPN por aplicativo é como ter um filtro de tráfego baseado em aplicativo, mas vai além de combinar gatilhos de aplicativo com um filtro de tráfego baseado em aplicativo para que a conectividade de VPN seja restrita a um aplicativo específico, em oposição a todos os aplicativos no cliente VPN. O recurso inicia automaticamente quando o aplicativo é iniciado.|
|  **Suporte para algoritmos de criptografia IPsec personalizados**   |  Always On VPN dá suporte ao uso de algoritmos criptográficos personalizados baseados em RSA e na criptografia de curva elíptica para atender a rigorosas políticas de segurança governamentais ou organizacionais.|
| **Suporte nativo a EAP (Extensible Authentication Protocol)** |Always On VPN nativamente dá suporte ao EAP, que permite que você use um conjunto diversificado de tipos de EAP da Microsoft e de terceiros como parte do fluxo de trabalho de autenticação. O EAP fornece autenticação segura com base nos seguintes tipos de autenticação:<ul><li>Nome de usuário e senha</li><li>Cartão inteligente (físico e virtual)</li><li>Certificados do usuário</li><li>Windows Hello for Business</li><li>Suporte a MFA por meio da integração de RADIUS do EAP</li></ul>O fornecedor de aplicativos controla os métodos de autenticação de plug-in de VPN UWP de terceiros, embora eles tenham uma matriz de opções disponíveis, incluindo tipos de credenciais personalizadas e suporte a OTP.|

## <a name="vpn-connectivity"></a>Conectividade VPN

A seguir estão os principais aprimoramentos na conectividade de VPN Always On:

|  Aprimoramento da chave  | Descrição |
|---|---|
|                    **Always On**                    |                                                                                          Always On é um recurso do Windows 10 que permite que o perfil VPN ativo se conecte automaticamente e permaneça conectado com base em gatilhos — ou seja, entrada do usuário, alteração de estado da rede ou tela do dispositivo ativa. O Always On também é integrado à experiência de espera conectada para maximizar a vida útil da bateria.                                                                                           |
|             **Gatilho de aplicativo**              |                                                                                                                                          Você pode configurar perfis VPN no Windows 10 para se conectar automaticamente na inicialização de um conjunto de aplicativos especificado. Você pode configurar aplicativos de área de trabalho e UWP para disparar uma conexão VPN.                                                                                                                                          |
|              **Gatilho baseado em nome**              |                                                                                                            Com a VPN Always On, você pode definir regras para que as consultas de nome de domínio específicas disparem a conexão VPN. O Windows 10 agora dá suporte ao disparo baseado em nome para computadores ingressados no domínio e não relacionados ao domínio (anteriormente, apenas computadores não ingressados no domínio eram suportados).                                                                                                            |
|            **Detecção de rede confiável**            |                                                                                    Always On VPN inclui esse recurso para garantir que a conectividade VPN não seja disparada se um usuário estiver conectado a uma rede confiável dentro do limite corporativo. Você pode combinar esse recurso com qualquer um dos métodos de disparo mencionados anteriormente para fornecer uma experiência de usuário direta "somente conectar quando necessário".                                                                                     |
| **[Túnel de dispositivo](../vpn-device-tunnel-config.md)** | Always On VPN oferece a capacidade de criar um perfil VPN dedicado para o dispositivo ou computador. Ao contrário do *encapsulamento do usuário*, que se conecta somente depois que um usuário faz logon no dispositivo ou computador, o *túnel de dispositivo* permite que a VPN estabeleça conectividade antes de entrar no usuário. O túnel de dispositivo e o túnel de usuário operam de forma independente com seus perfis de VPN, podem ser conectados ao mesmo tempo e podem usar diferentes métodos de autenticação e outras definições de configuração de VPN, conforme apropriado. |

## <a name="networking"></a>Rede

A seguir estão alguns dos aprimoramentos de rede no Always On VPN:


|              Aprimoramento da chave              |                                                                                                                                                                                                                Descrição                                                                                                                                                                                                                |
|-------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Suporte de pilha dupla para IPv4 e IPv6**  | Always On VPN dá suporte nativo ao uso de IPv4 e IPv6 em uma abordagem de pilha dupla. Ele não tem dependência específica em um protocolo, o que permite a compatibilidade máxima de aplicativos IPv4/IPv6 combinada com suporte para futuras necessidades de rede IPv6.<p>***Observação:*** Antes de começar, certifique-se de habilitar o IPv6 no servidor VPN. Caso contrário, uma conexão não pode ser estabelecida e uma mensagem de erro é exibida. |
| **Políticas de roteamento específicas do aplicativo** |                            Além de definir políticas de roteamento de conexão VPN globais para separação de tráfego de intranet e Internet, é possível adicionar políticas de roteamento para controlar o uso de túnel dividido ou configurações de túnel forçado em uma base por aplicativo. Essa opção proporciona um controle mais granular sobre quais aplicativos têm permissão para interagir com quais recursos por meio do túnel VPN.                             |
|           **Rotas de exclusão**            |                 Always On VPN dá suporte à capacidade de especificar rotas de exclusão que controlam especificamente o comportamento de roteamento para definir qual tráfego deve atravessar a VPN apenas e não passar pela interface de rede física.<p><p>***Observações:***<br>-As rotas de exclusão atualmente funcionam para o tráfego na mesma sub-rede que o cliente, por exemplo, LinkLocal.<br>-As rotas de exclusão funcionam apenas em uma configuração de túnel dividido.                  |

## <a name="configuration-and-compatibility"></a>Configuração e compatibilidade 

A seguir estão alguns dos aprimoramentos de configuração e compatibilidade no Always On VPN:


|                 Aprimoramento da chave                  |                                                                                                                                                                                                                                                                                                                       Descrição                                                                                                                                                                                                                                                                                                                       |
|--------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    **Compatibilidade de gateway de VPN de terceiros**     | O cliente VPN Always On não requer o uso de um gateway de VPN baseado em Microsoft para operar. Por meio do suporte do protocolo IKEv2, o cliente facilita a interoperabilidade com gateways de VPN de terceiros que dão suporte a esse tipo de encapsulamento padrão da indústria. Você também pode obter interoperabilidade com gateways de VPN de terceiros usando um plug-in de VPN UWP combinado com um tipo de túnel personalizado sem sacrificar Always On recursos e benefícios da plataforma VPN.<p><p>***Observação***:<br>Consulte seu gateway ou fornecedor de dispositivo de back-end de terceiros sobre configurações e compatibilidade com Always On VPN e túnel de dispositivo usando IKEv2. |
| **Suporte ao protocolo VPN IKEv2 padrão do setor** |                                                                                                                                                                                                                              O cliente VPN Always On dá suporte a IKEv2, um dos protocolos de encapsulamento padrão mais amplamente usados do setor. Essa compatibilidade maximiza a interoperabilidade com gateways de VPN de terceiros.                                                                                                                                                                                                                               |
|               **Suporte a plataforma**               |                                                                                                                                                                                                           Always On VPN dá suporte a dispositivos ingressados no domínio, não ingressados no domínio (grupo de trabalho) ou no Azure AD para permitir cenários de ambas as empresas e BYOD (Traga seu próprio dispositivo). Além disso, Always On VPN está disponível em todas as edições do Windows.                                                                                                                                                                                                           |
| **Diferentes mecanismos de gerenciamento e implantação** |                                                                                                                                 Você pode usar muitos mecanismos de gerenciamento e implantação para gerenciar as configurações de VPN (chamadas de *perfil de VPN*), incluindo o Windows PowerShell, o Microsoft Endpoint Configuration Manager, o Intune ou a ferramenta de MDM (gerenciamento de dispositivo móvel) de terceiros e o designer de configuração do Windows. Essas opções simplificam a configuração de Always On VPN independentemente das ferramentas de gerenciamento de cliente que você usa.                                                                                                                                 |
|     **Definição de perfil de VPN padronizada**      |                                                                                                                                                                                                                                  Always On VPN dá suporte à configuração usando um perfil XML padrão (ProfileXML), fornecendo um formato de modelo de configuração padrão que a maioria dos conjuntos de ferramentas de gerenciamento e implantação usam.                                                                                                                                                                                                                                   |

## <a name="next-steps"></a>Próximas etapas

- [Saiba mais sobre alguns dos recursos avançados de VPN Always On](deploy/always-on-vpn-adv-options.md)

- [Saiba mais sobre a tecnologia de VPN Always On](always-on-vpn-technology-overview.md)

- [Iniciar o planejamento de sua implantação de VPN Always On](deploy/always-on-vpn-deploy-deployment.md)
