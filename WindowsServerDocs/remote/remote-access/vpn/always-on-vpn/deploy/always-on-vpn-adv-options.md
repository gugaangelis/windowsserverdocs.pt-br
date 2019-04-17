---
title: Recursos avançados de VPN Always On
description: Além do cenário de implantação fornecido nesta implantação, você pode adicionar outros recursos avançados de VPN para melhorar a segurança e a disponibilidade de sua conexão VPN.
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: a544ac3c1a121874170a2fc78a34bd401b8bebe1
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066997"
---
# Recursos avançados de VPN Always On

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Anterior:** Saiba mais sobre a tecnologia de VPN Always On](../always-on-vpn-technology-overview.md)<br>
& #187; [ **Próximo:** começar a planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md)

Além de cenários de implantação fornecidos, você pode adicionar outros recursos avançados de VPN para melhorar a segurança e a disponibilidade de sua conexão VPN. Por exemplo, esses componentes podem ajudar a garantir que o cliente está saudável antes de permitir que uma conexão.


## Alta disponibilidade

Estes são opções adicionais para alta disponibilidade.


|Opção  |Descrição  |
|---------|---------|
|A resiliência de servidor e balanceamento de carga     |Em ambientes que exigem alta disponibilidade ou suporte grande número de solicitações, você pode aumentar o desempenho e a resiliência de acesso remoto usando o balanceamento de carga entre vários servidores que estão executando o servidor de política de rede (NPS) e ativando remota Clustering de servidor de acesso.<p>Documentos relacionados:<ul><li>[Balanceamento de carga do servidor de Proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[Implantar o acesso remoto em um cluster](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|Resiliência local geográfico     |Para IP com base em localização geográfica, você pode usar Global Traffic Manager com o DNS no Windows Server 2016. Para balanceamento de carga geográfica mais robusta, você pode usar soluções de Global servidor balanceamento de carga, como o Gerenciador de tráfego do Microsoft Azure.<p>Documentos relacionados:<ul><li>[Visão geral do Gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Gerenciador de tráfego do Microsoft Azure](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

---

## Autenticação avançada

Estes são opções adicionais para autenticação.


|Opção  |Descrição  |
|---------|---------|
|Windows Hello para Empresas     |No Windows 10, o Windows Hello para Empresas substitui senhas por autenticação forte de dois fatores em computadores e dispositivos móveis. Essa autenticação consiste em um novo tipo de credencial de usuário que é vinculado a um dispositivo e usa um biométrico ou número de identificação pessoal (PIN).<p>O cliente VPN do Windows 10 é compatível com o Windows Hello para empresas. Depois que o usuário faz com um gesto, a conexão VPN usa o Windows Hello para o certificado de negócios para autenticação baseada em certificado.<p>Documentos relacionados:<ul><li>[Windows Hello para Empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>Estudo de caso técnico: [permitindo acesso remoto com o Windows Hello para empresas no Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Autenticação multifator do Azure (MFA)     |Azure MFA tem na nuvem e versões que você pode se integrar com o mecanismo de autenticação de VPN do Windows local.<p>Para obter mais informações sobre como esse mecanismo funciona, consulte [autenticação RADIUS integrar com o servidor de autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius).         |

---

## Recursos avançados de VPN

Estes são opções adicionais para recursos avançados.


|Opção  |Descrição  |
|---------|---------|
|Filtragem de tráfego     |Se você precisar impor quais aplicativos VPN que os clientes podem acessar, você pode habilitar filtros de tráfego de VPN.<p>Para obter mais informações, consulte [os recursos de segurança VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features).         |
|VPN disparada por aplicativo     |Você pode configurar perfis VPN para se conectar automaticamente ao iniciar determinados aplicativos ou tipos de aplicativos.<p>Para obter mais informações sobre isso e outras opções de disparo, consulte as [Opções de perfil disparadas automaticamente por VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile).         |
|Acesso condicional VPN   |Conformidade de dispositivo e acesso condicional pode exigir que os dispositivos gerenciados para atender aos padrões antes que possam se conectar à VPN. Um dos recursos avançados de acesso condicional VPN permite restringir as conexões VPN para somente àqueles onde o certificado de autenticação de cliente contém o 'dos acesso condicional AAD OID de ' 1.3.6.1.4.1.311.87'.<p>Para restringir as conexões VPN, você precisa:<ol><li>No servidor NPS, abra o snap-in do **Servidor de políticas de rede** .</li><li>Expanda **diretivas** > **políticas de rede**.</li><li>Clique com botão direito a política de rede de **conexões de rede Virtual privada (VPN)** e selecione **Propriedades**.</li><li>Selecione a guia **configurações** .</li><li>Selecione **Específico do fornecedor** e clique em **Adicionar**.</li><li>Selecione a opção **Permitidos de certificado OID** e, em seguida, clique em **Adicionar**.</li><li>Cole o OID de acesso condicional AAD de **1.3.6.1.4.1.311.87** como o valor de atributo e, em seguida, clique duas vezes em **Okey** .</li><li>Clique em **Close** e, em seguida, **Aplicar**.<p>Agora, quando clientes VPN tentam se conectar usando qualquer certificado que não seja o certificado de curta duração de nuvem, a conexão falhará.</li></ol>Para obter mais informações sobre o acesso condicional, consulte [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access).   |

---


## Proteção adicional

**Atestado de chave do Trusted Platform Module (TPM)**<p>
Um certificado de usuário com uma chave atestados TPM oferece maior garantia de segurança, salvo em backup, não a capacidade de exportação, anti-hammering e isolamento de chaves fornecidas pelo TPM.

Para obter mais informações sobre o atestado de chave TPM no Windows 10, consulte [Atestado de chave do TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation).

## Próximas etapas
[Começar a planejar a implantação de VPN Always On](always-on-vpn-deploy-planning.md): antes de instalar a função de servidor de acesso remoto no computador que você estiver planejando usar como um servidor VPN, execute as seguintes tarefas. Após o planejamento adequado, você pode implantar VPN Always On e, opcionalmente, configurar o acesso condicional para conectividade VPN usando o Azure AD.  

---

## Tópicos relacionados
- [Balanceamento de carga de servidor Proxy NPS](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md): clientes remoto Authentication Dial-In User Service (RADIUS), que são servidores de acesso à rede, como servidores de rede virtual privada (VPN) e pontos de acesso sem fio, criar solicitações de conexão e enviá-las para RADIUS servidores como NPS. Em alguns casos, um servidor NPS talvez receba muitas solicitações de conexão ao mesmo tempo, resultando em degradação do desempenho ou uma sobrecarga.

- [Visão geral do Gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview): Este tópico fornece uma visão geral do Azure tráfego Manager, que permite que você controle a distribuição do tráfego de usuário para pontos de extremidade de serviço. Gerenciador de tráfego usa o sistema de nome de domínio (DNS) para direcionar as solicitações para o ponto de extremidade mais apropriado com base em um método de roteamento de tráfego e a integridade dos pontos de extremidade. 

- [Windows Hello para empresas](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification): Este tópico fornece os pré-requisitos, como implantações somente na nuvem e implantações híbridas.  Este tópico também lista perguntas frequentes sobre o Windows Hello para empresas.

- [Estudo de caso técnico: habilitar o acesso remoto com o Windows Hello para empresas no Windows 10](https://msdn.microsoft.com/library/mt728163.aspx): Este estudo de caso técnico você aprende como Microsoft implementa o acesso remoto com o Windows Hello para empresas.  Windows Hello para empresas é uma abordagem de autenticação baseada em certificado para empresas e consumidores que vai além de senhas ou chave privada/pública. Essa forma de autenticação depende de credenciais de par de chaves que podem substituir senhas e são resistente a violações, thefts e phishing. 

- [Autenticação de RADIUS integrar com o servidor de autenticação multifator Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius): Este tópico o orienta por meio de adicionar e configurar uma autenticação de cliente RADIUS com o servidor de autenticação multifator do Azure. RADIUS é um protocolo padrão para aceitar solicitações de autenticação e processar as solicitações. O servidor de autenticação multifator Azure pode atuar como um servidor RADIUS. 

- [Recursos de segurança VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features): Este tópico fornece diretrizes de segurança VPN para LockDown VPN, integração de proteção de informações do Windows (WIP) com a VPN, e filtros de tráfego. 

- [Opções de perfil disparadas automaticamente por VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile): Este tópico fornece opções de perfil disparadas automaticamente por VPN, como disparo por aplicativo, com base em nome do gatilho e sempre ativado.

- [VPN e acesso condicional](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access): Este tópico fornece uma visão geral da plataforma de acesso condicional baseado em nuvem para fornecer uma opção de conformidade do dispositivo para clientes remotos. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD). 

- [Atestado de chave do TPM](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation): Este tópico fornece uma visão geral do Trusted Platform Module (TPM) e etapas para implantar o TPM atestado de chave. Você também pode encontrar informações de solução de problemas e etapas para resolver problemas. 

---