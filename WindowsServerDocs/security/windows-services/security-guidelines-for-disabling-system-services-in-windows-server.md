---
title: Diretrizes de segurança para serviços do sistema no Windows Server 2016
description: Diretrizes de segurança para desabilitar serviços no Windows Server 2016 com Experiência Desktop
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/26/2018
ms.assetid: b886b2fd-3567-4f0a-8aa3-4ba7923d2d21
author: nirb
ms.author: nirb
ms.openlocfilehash: 323985cf316bda2fa6ab6a1721e2b6316450391a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844467"
---
## <a name="guidance-on-disabling-system-services-on-windows-server-2016-with-desktop-experience"></a>Orientação sobre como desativar os serviços do sistema no Windows Server 2016 com experiência Desktop

Aplica-se a: Windows Server 2016

O sistema operacional Windows inclui muitos serviços do sistema que fornecem funcionalidade importante. Serviços diferentes têm políticas de inicialização padrão diferente: algumas são iniciadas por padrão (automática), alguns quando necessário (manuais), e alguns estão desabilitados por padrão e devem ser habilitados explicitamente antes de serem executados. Esses padrões foram escolhidos cuidadosamente para cada serviço equilibrar o desempenho, a funcionalidade e a segurança para clientes típicos.

No entanto, alguns clientes corporativos talvez prefira um equilíbrio mais focado em segurança para seus computadores Windows e servidores, o que reduz seu ataque de superfície para o mínimo absoluto e, portanto, talvez queira desabilitar totalmente todos os serviços que não são necessárias em seus específicos ambientes. Para os clientes, Microsoft® está fornecendo a orientação que acompanha este artigo em relação a quais serviços com segurança podem ser desabilitados para essa finalidade.

As diretrizes são apenas para Windows Server 2016 com experiência Desktop (a menos que usado como uma substituição da área de trabalho para os usuários finais). Começando com o Windows Server 2019, essas diretrizes são configuradas por padrão. Cada serviço no sistema é categorizado da seguinte maneira:

-   **Desabilite:** Uma empresa focado em segurança preferirão desabilitar esse serviço e antecipar sua funcionalidade (veja os detalhes adicionais abaixo).
- **Okey para desabilitar o:** Esse serviço fornece a funcionalidade que é útil para alguns, mas nem todas as empresas e empresas focado em segurança que não usá-la com segurança podem desabilitá-lo.
- **Não desabilite:** Desabilitar esse serviço afeta a funcionalidade essencial ou impedir que as funções específicas ou recursos funcionando corretamente. Portanto, não deve ser desabilitada.
-  **(Nenhuma orientação):** O impacto da desabilitação desses serviços não foi totalmente avaliado. Portanto, a configuração padrão desses serviços não deve ser alterada.


Os clientes podem configurar seus PCs do Windows e servidores para desabilitar os serviços selecionados usando os modelos de segurança em suas diretivas de grupo ou usando a automação do PowerShell. Em alguns casos, a orientação inclui configurações de diretiva de grupo específicas que desabilitar a funcionalidade do serviço diretamente, como uma alternativa para desabilitar o serviço em si.

A Microsoft recomenda que os clientes desabilitar os seguintes serviços e as respectivas tarefas agendadas no Windows Server 2016 com experiência Desktop:

Serviços: 
1. Gerenciador de autenticação ao vivo do Xbox
2. Salvar jogo do Xbox Live

Tarefas agendadas: 
1. \Microsoft\XblGameSave\XblGameSaveTask
2. \Microsoft\XblGameSave\XblGameSaveTaskLogon

(Você também pode acessar as informações sobre todos os serviços detalhado neste artigo, exibindo a planilha do Microsoft Excel anexada: [Orientação sobre como desativar os serviços do sistema no Windows Server 2016 com experiência Desktop](https://msdnshared.blob.core.windows.net/media/2017/05/Service-management-WS2016.xlsx))

<br />

### <a name="disabling-services-not-installed-by-default"></a>Desabilitando serviços não instalados por padrão

A Microsoft recomenda em relação a aplicação de políticas para desabilitar os serviços que não são instalados por padrão.
-  O serviço geralmente é necessário se o recurso está instalado. Instalar o serviço ou o recurso requer direitos administrativos. Não permitir a instalação do recurso, não a inicialização do serviço.
-  Bloqueando o serviço do Microsoft Windows não para um administrador (ou não-administrador em alguns casos) de instalar um equivalente de terceiros semelhante, talvez uma com um risco mais alto de segurança.
-  Uma linha de base ou um parâmetro de comparação que desabilita um serviço do Windows não padrão (por exemplo, W3SVC) fornecerá o alguns auditores a impressão incorreta que a tecnologia (por exemplo, o IIS) é inerentemente insegura e nunca deve ser usada.
-  Se o recurso (e o serviço) nunca são instalados, isso apenas adiciona em massa desnecessária para a linha de base e o trabalho de verificação.

<br />
Para todos os serviços de sistema listados neste documento, as duas tabelas a seguirem oferecem uma explicação das colunas e as recomendações da Microsoft para ativar e desativar os serviços do sistema no Windows Server 2016 com experiência Desktop: 
 
<br />

### <a name="explanation-of-columns"></a>Explicação das colunas

| | |
|---|---|
|**Descrição do serviço**|   Descrição do serviço, de sc.exe qdescription.|
|**Nome** |Nome da chave (interno) do serviço|
|**Instalação** |Sempre instalados: Serviço está no servidor com experiência Desktop e Server Core  <br /> Apenas com a experiência Desktop: Serviço está no Windows Server 2016 com experiência Desktop, mas está ***não*** no Server Core |
|**StartType**  |Tipo de início de serviço no Windows Server 2016|
|**Recomendação** |Microsoft recomendação/conselhos sobre como desabilitar esse serviço no Windows Server 2016 em uma implantação corporativa típica e bem gerenciada e em que o servidor não está sendo usado como uma substituição de área de trabalho do usuário final.|
|**Comentários** |Explicação adicional|

<br />

### <a name="explanation-of-microsoft-recommendations"></a>Explicação sobre as recomendações da Microsoft

| | |
|---|---|
|**Não desabilite** |Esse serviço não deve ser desabilitado|
|**Okey para desabilitar**| Esse serviço pode ser desabilitado se o recurso que oferece suporte a ele não estiver sendo usado.|
|**Já desativado**|  Esse serviço é desabilitado por padrão. Não há necessidade de impor a política|
|**Deve ser desabilitado** |Esse serviço nunca deve ser habilitado em um sistema corporativo bem gerenciada.|

<br />

As tabelas a seguir oferecem diretrizes da Microsoft sobre a desabilitação de serviços do sistema no Windows Server 2016 com experiência Desktop:

<br />

##  <a name="activex-installer-axinstsv"></a>ActiveX Installer (AxInstSV)

| | |
|---|---|
|   **Descrição do serviço** |   Fornece validação de controle de conta de usuário para a instalação de controles ActiveX na Internet e permite o gerenciamento de instalação de controles ActiveX com base nas configurações de diretiva de grupo. Esse serviço é iniciado sob demanda e se desabilitada a instalação de controles ActiveX será se comportam de acordo com as configurações do navegador padrão.    |
|   **Nome do serviço**    |   AxInstSV    |
|   **Instalação**    |   Com a experiência Desktop    |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Okey para desabilitar   |
|   **Comentários**    |   Okey para desabilitar o recurso não necessárias |


<br />

## <a name="alljoyn-router-service"></a>AllJoyn Router Service   
| | |
|---|---|
|   **Descrição do serviço** |   Encaminha mensagens de AllJoyn para AllJoyn clientes locais. Se esse serviço for interrompido, os clientes AllJoyn que não têm seus próprios roteadores agrupados será não é possível executar. |
|   **Nome do serviço**    |   AJRouter    |
|   **Instalação**    |   Com a experiência Desktop    |
|   **StartType**   |   Manual  |
|   **Recomendação**  | Nenhuma diretriz       |
|   **Comentários**    |       |
| | |

<br />

## <a name="app-readiness"></a>Preparação do aplicativo
| | |
|---|---|
**Descrição do serviço** |   Obtém os aplicativos prontos para uso na primeira vez que um usuário fizer logon neste computador e ao adicionar novos aplicativos.
**Nome do serviço**    |   AppReadiness
**Instalação**    |   Com a experiência Desktop
**StartType**   |   Manual
**Recomendação**  |   Não desabilite
**Comentários**    |   
| | |

<br />

##  <a name="application-identity"></a>Identidade do aplicativo
| | |       
|---|---|   
**Descrição do serviço** |   Determina e verifica a identidade de um aplicativo. Desabilitar esse serviço impedirá que o AppLocker que está sendo imposta.
**Nome do serviço**    |   AppIDSvc
**Instalação**    |   Sempre instalado
**StartType**   |   Manual
**Recomendação**  |Nenhuma diretriz    
**Comentários**    |   
|||     

<br />

##  <a name="application-information"></a>Informações do aplicativo 
| | |       
|---|---|   
|   **Descrição do serviço** |   Facilita a execução de aplicativos interativos com privilégios administrativos adicionais.  Se esse serviço for interrompido, os usuários poderão iniciar aplicativos com os privilégios administrativos adicionais que precisam para realizar tarefas de usuário desejado.
|   **Nome do serviço**    |   AppInfo
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   Dá suporte a elevação do UAC mesma área de trabalho
|||     

<br />

##  <a name="application-layer-gateway-service"></a>Serviço de Gateway de camada de aplicativo       
| | |           
|---|---|           
|   **Descrição do serviço** |   Fornece suporte para plug-ins de protocolo de terceiros para o compartilhamento de Conexão com a Internet
|   **Nome do serviço**    |   ALG
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |Nenhuma diretriz    
|   **Comentários**    |   
|||     

<br />

##  <a name="application-management"></a>Gerenciamento de aplicativos      
| | |           
|---|---|       
|   **Descrição do serviço** |   Processa as solicitações de instalação, remoção e enumeração para o software implantado por meio da diretiva de grupo. Se o serviço estiver desabilitado, os usuários poderão instalar, remover ou enumerar o software implantado por meio da diretiva de grupo. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   AppMgmt
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="appx-deployment-service-appxsvc"></a>Serviço de implantação de AppX (AppXSVC)       
| | |           
|---|---|
|   **Descrição do serviço** |   Fornece suporte de infraestrutura para implantação de aplicativos da Store. Esse serviço é iniciado sob demanda e, se desabilitado aplicativos da Store não serão implantados no sistema e podem não funcionam corretamente.
|   **Nome do serviço**    |   AppXSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="auto-time-zone-updater"></a>Atualizador de fuso horário automático           
| | |           
|---|---|           
|   **Descrição do serviço** |   Define automaticamente o fuso horário do sistema.
|   **Nome do serviço**    |   tzautoupdate
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   
|||         
            
<br />          

## <a name="background-intelligent-transfer-service"></a>BITS          
| | |           
|---|---|   
|   **Descrição do serviço** |   Transferências de arquivos em segundo plano usando a largura de banda de rede ociosa. Se o serviço estiver desabilitado, todos os aplicativos que dependem do BITS, como o Windows Update ou MSN Explorer, será possível baixar automaticamente programas e outras informações.
|   **Nome do serviço**    |   BITS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          


## <a name="background-tasks-infrastructure-service"></a>Serviço de infraestrutura de tarefas em segundo plano      
| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço de infraestrutura do Windows que controla quais tarefas em segundo plano pode ser executado no sistema.
|   **Nome do serviço**    |   BrokerInfrastructure
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="base-filtering-engine"></a>Mecanismo de filtragem básica            
| | |           
|---|---|       
|   **Descrição do serviço** |   A base de dados de filtragem BFE (mecanismo) é um serviço que gerencia o firewall e as políticas do Internet Protocol security (IPsec) e implementa a filtragem do modo de usuário. Interromper ou desabilitar o serviço BFE reduz significativamente a segurança do sistema. Ele também resultará em comportamento imprevisível em gerenciamento de IPsec e aplicativos de firewall.
|   **Nome do serviço**    |   BFE
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="bluetooth-support-service"></a>Serviço de suporte do Bluetooth            
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço Bluetooth dá suporte à descoberta e associação de dispositivos remotos de Bluetooth.  Interromper ou desabilitar esse serviço pode causar já instalados dispositivos Bluetooth não funcionar corretamente e impedir que novos dispositivos descobertos ou associados.
|   **Nome do serviço**    |   bthserv
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Okey para desabilitar se não usado. Outro mecanismo de desabilitação: https://technet.microsoft.com/library/dd252791.aspx
|||         
            
<br />          


## <a name="cdpusersvc"></a>CDPUserSvc           
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço de usuário é usado para cenários de plataformas de dispositivos conectados
|   **Nome do serviço**    |   CDPUserSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          


##  <a name="certificate-propagation"></a>Propagação de certificado     
| | |           
|---|---|
|   **Descrição do serviço** |   Copia os certificados de raiz e certificados de usuário de cartões inteligentes no repositório de certificados do usuário atual, detecta quando um cartão inteligente é inserido em um leitor de cartão inteligente e, se necessário, instala o minidriver de Plug and Play do cartão inteligente.
|   **Nome do serviço**    |   CertPropSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="client-license-service-clipsvc"></a>Serviço de licença de cliente (ClipSVC)        
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece suporte de infraestrutura para a Microsoft Store. Esse serviço é iniciado sob demanda e, se compraram desabilitados aplicativos usar o Microsoft Store não se comportará corretamente.
|   **Nome do serviço**    |   ClipSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="cng-key-isolation"></a>Isolamento de chave CNG
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço de isolamento de chave CNG é hospedado no processo de LSA. O serviço fornece isolamento de processos principais para chaves privadas e operações criptográficas associadas conforme exigido pelos critérios comuns. O serviço armazena e usa chaves de longa duração em um processo seguro de estar em conformidade com requisitos de critérios comuns.
|   **Nome do serviço**    |   KeyIso
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="com-event-system"></a>Sistema de eventos COM+       
| | |           
|---|---|       
|   **Descrição do serviço** |   Dá suporte ao sistema evento de notificação de serviço (SENS), que fornece a distribuição automática de eventos para inscrição de componentes do modelo de objeto de componente (COM). Se o serviço for interrompido, o SENS será fechada e não poderá fornecer notificações de logon e logoff. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   EventSystem
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="com-system-application"></a>Aplicativo de sistema COM+     
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia a configuração e o rastreamento dos componentes de + com base no modelo de objeto de componente (COM). Se o serviço for interrompido, a maioria dos COM+-componentes baseados em não funcionará corretamente. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   COMSysApp
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="computer-browser"></a>Pesquisador de computadores        
| | |           
|---|---|       
|   **Descrição do serviço** |   Mantém uma lista atualizada dos computadores na rede e fornece essa lista para os computadores designados como localizadores. Se esse serviço for interrompido, essa lista não será atualizada ou mantida. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   Navegador
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   
|||         
            
<br />          

## <a name="connected-devices-platform-service"></a>Serviço de plataforma de dispositivos conectados       
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço é usado para cenários de dispositivos conectados e vidro Universal
|   **Nome do serviço**    |   CDPSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="connected-user-experiences-and-telemetry"></a>Experiências de usuário conectado e telemetria     
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de telemetria e experiências de usuário conectado permite que os recursos que dão suporte a experiências de usuário no aplicativo e conectado. Além disso, esse serviço gerencia a coleção orientada a eventos e a transmissão de informações de diagnóstico e de uso (usadas para melhorar a experiência e a qualidade da plataforma do Windows) quando o diagnóstico e as configurações de opção de privacidade de uso são habilitadas em Comentários e diagnósticos.
|   **Nome do serviço**    |   DiagTrack
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="contact-data"></a>Dados de contato        
| | |           
|---|---|       
|   **Descrição do serviço** |   Dados de contato de índices para a pesquisa rápida de contato. Se você parar ou desabilitar esse serviço, os contatos podem estar ausentes dos resultados da pesquisa.
|   **Nome do serviço**    |   PimIndexMaintenanceSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          

## <a name="coremessaging"></a>CoreMessaging            
| | |           
|---|---|           
|   **Descrição do serviço** |   Gerencia a comunicação entre componentes do sistema.
|   **Nome do serviço**    |   CoreMessagingRegistrar
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="credential-manager"></a>Gerenciador de Credenciais           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece armazenamento seguro e recuperação de credenciais de usuários, aplicativos e pacotes de serviço de segurança.
|   **Nome do serviço**    |   VaultSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="cryptographic-services"></a>Serviços de criptografia           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece três serviços de gerenciamento: Serviço de banco de dados de catálogo, que confirma as assinaturas dos arquivos do Windows e permite que os novos programas a serem instalados; Serviço de raiz protegida, que adiciona e remove os certificados de autoridade de certificação raiz confiável do computador; e o serviço de atualização automática do certificado raiz, que recupera os certificados raiz do Windows Update e habilitar cenários como o SSL. Se esse serviço for interrompido, esses serviços de gerenciamento não funcionará corretamente. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   CryptSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="data-sharing-service"></a>Serviço de compartilhamento de dados         
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece dados intermediar entre aplicativos.
|   **Nome do serviço**    |   DsSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="datacollectionpublishingservice"></a>DataCollectionPublishingService          
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço do DCP (coleta de dados e publicação) dá suporte a aplicativos de terceiros para carregar dados para a nuvem.
|   **Nome do serviço**    |   DcpSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="dcom-server-process-launcher"></a>Iniciador de processo do servidor DCOM         
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço DCOMLAUNCH inicia servidores COM e DCOM em resposta a solicitações de ativação do objeto. Se esse serviço for interrompido ou desabilitado, programas usando COM ou DCOM não funcionará corretamente. É altamente recomendável que você tenha o DCOMLAUNCH serviço em execução.
|   **Nome do serviço**    |   DcomLaunch
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |Nenhuma diretriz    
|   **Comentários**    |   
|||         
            
<br />

##  <a name="device-association-service"></a>Serviço de associação de dispositivo      
| | |           
|---|---|       
|   **Descrição do serviço** |   Habilita o emparelhamento entre os sistemas e dispositivos com ou sem fio.
|   **Nome do serviço**    |   DeviceAssociationService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          
        
##  <a name="device-install-service"></a>Serviço de instalação do dispositivo      
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que um computador reconhecer e se adaptar às alterações de hardware com pouca ou nenhuma intervenção do usuário. Interromper ou desabilitar esse serviço resultará em instabilidade no sistema.
|   **Nome do serviço**    |   DeviceInstall
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="device-management-enrollment-service"></a>Serviço de registro de gerenciamento de dispositivo        
| | |           
|---|---|       
|   **Descrição do serviço** |   Executa as atividades de registro de dispositivo para gerenciamento de dispositivo
|   **Nome do serviço**    |   DmEnrollmentSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="device-setup-manager"></a>Gerenciador de instalação do dispositivo         
| | |           
|---|---|       
|   **Descrição do serviço** |   Habilita a detecção, download e instalação de software relacionados ao dispositivo. Se esse serviço for desabilitado, dispositivos podem ser configurados com o software desatualizado e podem não funcionar corretamente.
|   **Nome do serviço**    |   DsmSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="devquery-background-discovery-broker"></a>Agente de descoberta de plano de fundo DevQuery         
| | |           
|---|---|           
|   **Descrição do serviço** |   Permite que os aplicativos descobrir dispositivos com uma tarefa de fundo
|   **Nome do serviço**    |   DevQueryBroker
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="dhcp-client"></a>Cliente DHCP          
| | |           
|---|---|       
|   **Descrição do serviço** |   Registra e atualiza endereços IP e registros DNS para este computador. Se esse serviço for interrompido, este computador não receberá atualizações de DNS e endereços IP dinâmicos. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   DHCP
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="diagnostic-policy-service"></a>Serviço de diretiva de diagnóstico            
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de diretiva de diagnóstico permite a detecção de problemas, solução de problemas e resolução para componentes do Windows.  Se esse serviço for interrompido, diagnóstico deixará de funcionar.
|   **Nome do serviço**    |   DPS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="diagnostic-service-host"></a>Host de serviço de diagnóstico     
| | |           
|---|---|       
|   **Descrição do serviço** |   O Host de serviço de diagnóstico é usado pelo serviço de diretiva de diagnóstico para o diagnóstico de host que precisam ser executados em um contexto de serviço Local.  Se esse serviço for interrompido, qualquer diagnóstico que depende dele deixará de funcionar.
|   **Nome do serviço**    |   WdiServiceHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="diagnostic-system-host"></a>Host do sistema de diagnóstico           
| | |           
|---|---|       
|   **Descrição do serviço** |   O Host do sistema de diagnóstico é usado pelo serviço de diretiva de diagnóstico para o diagnóstico de host que precisam ser executados em um contexto de sistema Local.  Se esse serviço for interrompido, qualquer diagnóstico que depende dele deixará de funcionar.
|   **Nome do serviço**    |   WdiSystemHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="distributed-link-tracking-client"></a>Cliente de controle de Link distribuído            
| | |           
|---|---|   
|   **Descrição do serviço** |   Mantém vínculos entre arquivos NTFS em um computador ou em computadores em uma rede.
|   **Nome do serviço**    |   TrkWks
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="distributed-transaction-coordinator"></a>Coordenador de Transações Distribuídas     
| | |           
|---|---|   
|   **Descrição do serviço** |   Coordena as transações que abrangem vários gerenciadores de recursos, como bancos de dados, filas de mensagens e sistemas de arquivos. Se esse serviço for interrompido, essas transações falhará. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   MSDTC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />  

##  <a name="dmwappushsvc"></a>dmwappushsvc        
| | |           
|---|---|       
|   **Descrição do serviço** |   Serviço de roteamento de mensagem do WAP por Push
|   **Nome do serviço**    |   dmwappushservice
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Serviço necessário em dispositivos de cliente do Intune, MDM e tecnologias de gerenciamento semelhantes e para o filtro de gravação unificado. Não é necessária para o servidor.
|||         
            
<br />      

##  <a name="dns-client"></a>Cliente DNS      
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço do Cliente DNS (dnscache) faz o cache dos nomes DNS (Sistema de Nomes de Domínio) e registra o nome completo do computador nesse computador. Se o serviço for interrompido, nomes DNS continuarão sendo resolvidos. No entanto, os resultados de consultas de nomes DNS não serão armazenados e o nome do computador não será registrado. Se o serviço for desabilitado, outros serviços que dependem explicitamente dele não serão iniciados.
|   **Nome do serviço**    |   DnsCache
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="downloaded-maps-manager"></a>Gerenciador de mapas baixado     
| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço do Windows para acesso a aplicativos para mapas baixados. Esse serviço é iniciado sob demanda pelo aplicativo acessando baixado mapas. A desabilitação desse serviço impedirá que aplicativos acessem mapas.
|   **Nome do serviço**    |   MapsBroker
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Desabilitar quebras aplicativos que dependem do serviço; Okey desabilitar se os aplicativos de terceira parte confiável não nele
|||         
            
<br />          

## <a name="embedded-mode"></a>Modo inserido            
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de modo incorporado permite cenários relacionados a aplicativos em segundo plano.  A desabilitação desse serviço será impedir que aplicativos em segundo plano que está sendo ativado.
|   **Nome do serviço**    |   embeddedmode
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="encrypting-file-system-efs"></a>EFS (Encrypting File System, sistema de arquivos com criptografia)
| | |                   
|---|---|           
|   **Descrição do serviço** | Fornece a tecnologia de criptografia de arquivos principal usada para armazenar arquivos criptografados em volumes de sistema de arquivos NTFS. Se esse serviço for interrompido ou desabilitado, aplicativos poderão acessar arquivos criptografados.            
|   **Nome do serviço**  |  EFS            
|   **Instalação**  |  Sempre instalado           
|   **StartType**   |  Manual           
|   **Recomendação**  | Nenhuma diretriz           
|   **Comentários**   |
|||                 
                            
<br />  

## <a name="enterprise-app-management-service"></a>Serviço de gerenciamento de aplicativo empresarial            
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite o gerenciamento de aplicativos corporativos.
|   **Nome do serviço**    |   EntAppSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="extensible-authentication-protocol"></a>Protocolo de autenticação extensível           
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço de protocolo EAP (Extensible Authentication) fornece autenticação de rede em situações como 802.1 x com fio e sem fio, VPN e proteção de acesso de rede (NAP).  EAP também fornece programação interfaces de aplicativo (APIs) que são usados por clientes de acesso de rede, incluindo clientes VPN e sem fio durante o processo de autenticação.  Se você desabilitar esse serviço, esse computador é impedido de acessar redes que exigem autenticação EAP.
|   **Nome do serviço**    |   EapHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="function-discovery-provider-host"></a>Host de Provedor da Descoberta de Função         
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço FDPHOST hospeda os provedores de descoberta de rede Function Discovery (FD). Esses provedores de FD fornecem serviços de descoberta de rede para o simples Services Discovery Protocol (SSDP) e serviços da Web - protocolo Discovery (WS-D). Interromper ou desabilitar o serviço FDPHOST desabilitará a descoberta de rede para esses protocolos ao usar FD. Quando esse serviço não estiver disponível, usando FD e contar com esses protocolos de descoberta de serviços de rede poderão encontrar dispositivos de rede ou recursos.
|   **Nome do serviço**    |   fdPHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="function-discovery-resource-publication"></a>Publicação de Recursos da Descoberta de Função      
| | |           
|---|---|       
|   **Descrição do serviço** |   Publica este computador e recursos anexados a este computador para que possam ser descobertos na rede.  Se esse serviço for interrompido, recursos de rede não serão publicados e eles não serão descobertos por outros computadores na rede.
|   **Nome do serviço**    |   FDResPub
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="geolocation-service"></a>Serviço de Geolocalização          
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço monitora o local atual do sistema e gerencia os limites geográficos (uma localização geográfica com eventos associados).  Se você desativar esse serviço, aplicativos poderão usar ou receber notificações para a localização geográfica ou limites geográficos.
|   **Nome do serviço**    |   lfsvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Desabilitar quebras aplicativos que dependem do serviço; Okey desabilitar se os aplicativos de terceira parte confiável não nele
|||         
            
<br />          

##  <a name="group-policy-client"></a>Cliente de Diretiva de Grupo     
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço é responsável por aplicar as configurações definidas por administradores para o computador e os usuários por meio do componente de diretiva de grupo. Se o serviço estiver desabilitado, as configurações não serão aplicadas e aplicativos e componentes não poderão ser gerenciados por meio da diretiva de grupo. Quaisquer componentes ou aplicativos que dependem do componente de diretiva de grupo talvez não seja funcionais se o serviço está desabilitado.
|   **Nome do serviço**    |   gpsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          


## <a name="human-interface-device-service"></a>Serviço de Dispositivos de Interface Humana           
| | |           
|---|---|       
|   **Descrição do serviço** |   Ativa e mantém o uso de botões ativados em teclados, controles remotos e outros dispositivos de multimídia. É recomendável que você mantenha esse serviço está em execução.
|   **Nome do serviço**    |   hidserv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="hv-host-service"></a>Serviço de Host do HV     
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece uma interface para que o hipervisor Hyper-V fornecer os contadores de desempenho por partição para o sistema operacional do host.
|   **Nome do serviço**    |   HvHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Aperfeiçoadores de desempenho para VMs convidadas. Não foi usada hoje, exceto para explicitamente preenchido VMs, mas será usado no Application Guard
|||         
            
<br />          

## <a name="hyper-v-data-exchange-service"></a>Serviço de Troca de Dados do Hyper-V        
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece um mecanismo para trocar dados entre a máquina virtual e o sistema operacional executado no computador físico.
|   **Nome do serviço**    |   vmickvpexchange
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />      

## <a name="hyper-v-guest-service-interface"></a>Interface de Serviço de Convidado do Hyper-V          
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece uma interface para o host do Hyper-V interagir com serviços específicos em execução dentro da máquina virtual.
|   **Nome do serviço**    |   vmicguestinterface
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />  

## <a name="hyper-v-guest-shutdown-service"></a>Serviço de Desligamento de Convidado do Hyper-V           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um mecanismo para desligar o sistema operacional dessa máquina virtual de interfaces de gerenciamento do computador físico.
|   **Nome do serviço**    |   vmicshutdown
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          
    
## <a name="hyper-v-heartbeat-service"></a>Serviço de Pulsação do Hyper-V            
| | |           
|---|---|           
|   **Descrição do serviço** |   Monitora o estado dessa máquina virtual relatando uma pulsação em intervalos regulares. Esse serviço ajuda você a identificar as máquinas virtuais em execução que pararam de responder.
|   **Nome do serviço**    |   vmicheartbeat
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-powershell-direct-service"></a>Serviço do PowerShell Direct do Hyper-V            
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um mecanismo para gerenciar a máquina virtual, com o PowerShell por meio da sessão VM sem uma rede virtual.
|   **Nome do serviço**    |   vmicvmsession
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-remote-desktop-virtualization-service"></a>Serviço de Virtualização de Área de Trabalho Remota do Hyper-V            
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece uma plataforma para a comunicação entre a máquina virtual e o sistema operacional executado no computador físico.
|   **Nome do serviço**    |   vmicrdv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-time-synchronization-service"></a>Serviço de Sincronização de Data/Hora do Hyper-V         
| | |           
|---|---|       
|   **Descrição do serviço** |   Sincroniza a hora do sistema dessa máquina virtual com a hora do sistema do computador físico.
|   **Nome do serviço**    |   vmictimesync
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="hyper-v-volume-shadow-copy-requestor"></a>Solicitante de Cópia de Sombra de Volume do Hyper-V         
| | |           
|---|---|           
|   **Descrição do serviço** |   Coordena as comunicações que são necessários para usar o serviço de cópias de sombra de Volume para fazer backup de dados e aplicativos nessa máquina virtual do sistema operacional no computador físico.
|   **Nome do serviço**    |   vmicvss
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Consulte HvHost
|||         
            
<br />          

## <a name="ike-and-authip-ipsec-keying-modules"></a>Módulos de Chave IKE e AuthIP IPsec          
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço IKEEXT hospeda o Exchange IKE (Internet Key) e a Internet AuthIP (protocolo Authenticated) módulos de chave. Esses módulos são usados para autenticação e troca de chaves na Internet Protocol security (IPsec). Interromper ou desabilitar o serviço IKEEXT desabilitará a troca de chaves IKE e AuthIP com computadores de mesmo nível. IPsec é normalmente configurado para usar IKE ou AuthIP; Portanto, interromper ou desabilitar o serviço IKEEXT pode resultar em falha de IPsec e pode comprometer a segurança do sistema. É altamente recomendável que você tenha a execução do serviço IKEEXT.
|   **Nome do serviço**    |   IKEEXT
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |    
|||         
            
<br />          

## <a name="interactive-services-detection"></a>Detecção de Serviços Interativos           
| | |           
|---|---|   
|   **Descrição do serviço** |   Habilita a notificação de usuário do usuário de entrada para serviços interativos, que permite o acesso a caixas de diálogo criados por serviços interativos quando eles são exibidos. Se esse serviço for interrompido, notificações de caixas de diálogo Novo serviço interativo deixará de funcionar e talvez não haja acesso a caixas de diálogo do serviço interativo. Se esse serviço for desabilitado, as notificações do e acesso a caixas de diálogo Novo serviço interativo deixará de funcionar.
|   **Nome do serviço**    |   UI0Detect
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />  

## <a name="internet-connection-sharing-ics"></a>ICS (Compartilhamento de Conexão com a Internet)            
| | |           
|---|---|           
|   **Descrição do serviço** |   Fornece conversão de endereços de rede, endereçamento, serviços de prevenção de resolução e/ou intrusão nomes para uma rede doméstica ou de pequena empresa.
|   **Nome do serviço**    |   SharedAccess
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Necessário para clientes usados como pontos de acesso Wi-Fi e também em ambas as extremidades de projeção Miracast. ICS podem ser bloqueadas com configuração de GPO, "Proíbem o uso de compartilhamento de Conexão de Internet na rede do domínio DNS"
|||         
            
<br />          

## <a name="ip-helper"></a>Auxiliar de IP            
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece conectividade de túnel usando tecnologias de transição IPv6 (6to4, Teredo, ISATAP e porta Proxy) e IP-HTTPS. Se esse serviço for interrompido, o computador não terá os benefícios de uma conectividade ampliada essas tecnologias oferecem.
|   **Nome do serviço**    |   iphlpsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          


##  <a name="ipsec-policy-agent"></a>Agente de Política IPsec      
| | |           
|---|---|       
|   **Descrição do serviço** |   Internet Protocol security (IPsec) oferece suporte à autenticação de par no nível de rede, autenticação de origem de dados, integridade de dados, confidencialidade (criptografia) e proteção contra repetição.  Esse serviço impõe diretivas IPsec criadas por meio do snap-in de diretivas de segurança IP ou a ferramenta de linha de comando "netsh ipsec".  Se você interromper esse serviço, você poderá ter problemas de conectividade de rede se sua política requer que as conexões usem IPsec.  Além disso, o gerenciamento remoto do Firewall do Windows não está disponível quando esse serviço for interrompido.
|   **Nome do serviço**    |   PolicyAgent
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />

##  <a name="kdc-proxy-server-service-kps"></a>Serviço de servidor Proxy do KDC (KPS)      
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço servidor de Proxy do KDC é executado em servidores de borda para o proxy Kerberos mensagens de protocolo para controladores de domínio na rede corporativa.
|   **Nome do serviço**    |   KPSSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz    
|   **Comentários**    |   
|||         
            
<br />          

## <a name="ktmrm-for-distributed-transaction-coordinator"></a>KtmRm para Coordenador de Transações Distribuídas            
| | |           
|---|---|       
|   **Descrição do serviço** |   Coordena as transações entre o coordenador de transações distribuídas (MSDTC) e o Gerenciador de KTM (Kernel Transaction). Se não for necessária, é recomendável que esse serviço persiste interrompido. Se ela for necessária, MSDTC e KTM iniciará esse serviço automaticamente. Se esse serviço for desabilitado, qualquer transação MSDTC interagindo com um Gerenciador de recursos do Kernel falhará e todos os serviços que dependam explicitamente dele falharão ao iniciar.
|   **Nome do serviço**    |   KtmRm
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />

##  <a name="link-layer-topology-discovery-mapper"></a>Mapeador de descoberta de topologia de camada de link        
| | |       
|---|---|       
|   **Descrição do serviço** |   Cria um mapa de rede, consistindo em PC e informações de topologia (conectividade) do dispositivo e metadados que descrevem cada PC e dispositivo.  Se esse serviço for desabilitado, o mapa de rede não funcionará corretamente.
|   **Nome do serviço**    |   lltdsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Okey desabilitar se nenhuma dependência no mapa de rede
|||         
            
<br />

## <a name="local-session-manager"></a>Gerenciador de sessão local                    
| | |                   
|---|---|   
|   **Descrição do serviço** |   Serviço Windows Core que gerencia as sessões de usuário local. Interromper ou desabilitar esse serviço resultará em instabilidade no sistema.    
|   **Nome do serviço**    |   LSM |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Automática   |
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||                 
                    
<br />                  

## <a name="microsoft-r-diagnostics-hub-standard-collector"></a>Coletor Microsoft (R) diagnóstico Hub padrão         
| | |           
|---|---|           
|   **Descrição do serviço** |   Serviço Windows Core que gerencia as sessões de usuário local. Interromper ou desabilitar esse serviço resultará em instabilidade no sistema.
|   **Nome do serviço**    |   LSM
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          
            
## <a name="microsoft-account-sign-in-assistant"></a>Assistente de conexão de conta da Microsoft          
| | |           
|---|---|       
|   **Descrição do serviço** |   Habilita a entrada do usuário por meio de serviços de identidade de conta Microsoft. Se esse serviço for interrompido, os usuários não poderão fazer logon no computador com sua conta da Microsoft.
|   **Nome do serviço**    |   wlidsvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Accounts da Microsoft são n/a no Windows Server
|||         
            
<br />          

##  <a name="microsoft-app-v-client"></a>Cliente Microsoft App-V      
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia os usuários do App-V e aplicativos virtuais
|   **Nome do serviço**    |   AppVClient
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   
|||         
            
<br />          

## <a name="microsoft-iscsi-initiator-service"></a>Serviço iniciador Microsoft iSCSI            
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia as sessões de Internet SCSI (iSCSI) deste computador para dispositivos de destino iSCSI remoto. Se esse serviço for interrompido, este computador não poderão fazer logon ou acessar destinos iSCSI. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   MSiSCSI
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Nossos dados de diagnóstico indicam que isso é usado no cliente, bem como o servidor. Nenhum benefício para desativá-lo.
|||         
            
<br />          

## <a name="microsoft-passport"></a>Microsoft Passport           
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece isolamento de processo para as chaves criptográficas usadas para autenticar para provedores de identidade associado de um usuário. Se esse serviço for desabilitado, todos os usos e gerenciamento dessas chaves não estará disponíveis, que inclui o logon de máquina e logon único para aplicativos e sites. Esse serviço inicia e para automaticamente. É recomendável que você não reconfigurar esse serviço.
|   **Nome do serviço**    |   NgcSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Necessário para logons PIN/Hello, que não têm suporte no servidor
|||         
            
<br />          

## <a name="microsoft-passport-container"></a>Contêiner do Microsoft Passport         
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia chaves de identidade de usuário local usadas para autenticar o usuário para provedores de identidade, bem como os cartões inteligentes virtuais TPM. Se esse serviço for desabilitado, as chaves de identidade de usuário local e cartões inteligentes virtuais TPM não será acessíveis. É recomendável que você não reconfigurar esse serviço.
|   **Nome do serviço**    |   NgcCtnrSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="microsoft-software-shadow-copy-provider"></a>Provedor de cópias de sombra de Software da Microsoft          
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia cópias de sombra de volume com base em software criadas pelo serviço de cópias de sombra de Volume. Se esse serviço for interrompido, as cópias de sombra de volume com base em software não podem ser gerenciadas. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   swprv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="microsoft-storage-spaces-smp"></a>SMP de espaços de armazenamento da Microsoft         
| | |           
|---|---|       
|   **Descrição do serviço** |   Serviço de host para o provedor de gerenciamento de espaços de armazenamento do Microsoft. Se esse serviço for interrompido ou desabilitado, os espaços de armazenamento não pode ser gerenciados.
|   **Nome do serviço**    |   smphost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Gerenciamento de armazenamento APIs falharem sem esse serviço. Exemplo: "Get-WmiObject -class MSFT_Disk -Namespace Root\Microsoft\Windows\Storage".
|||         
            
<br />          

## <a name="nettcp-port-sharing-service"></a>Serviço de compartilhamento de porta NET. TCP         
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece a capacidade de compartilhar portas TCP no protocolo NET. TCP.
|   **Nome do serviço**    |   NetTcpPortSharing
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   
|||         
            
<br />          

## <a name="netlogon"></a>Netlogon         
| | |           
|---|---|           
|   **Descrição do serviço** |   Mantém um canal seguro entre esse computador e o controlador de domínio para autenticar usuários e serviços. Se esse serviço for interrompido, o computador não pode autenticar usuários e serviços e o controlador de domínio não é possível registrar registros DNS. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   Netlogon
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="network-connection-broker"></a>Agente de Conexão de rede            
| | |           
|---|---|       
|   **Descrição do serviço** |   Conexões de agentes que permitir que os aplicativos do Microsoft Store receber notificações da internet.
|   **Nome do serviço**    |   NcbService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="network-connections"></a>Conexões de Rede         
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia objetos na pasta Conexões Dial-Up e de rede, na qual você pode exibir as conexões remotas e rede local.
|   **Nome do serviço**    |   Netman
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="network-connectivity-assistant"></a>Assistente de conectividade de rede      
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece notificação de status do DirectAccess para componentes de interface do usuário
|   **Nome do serviço**    |   NcaSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />  

##  <a name="network-list-service"></a>Serviço de lista de rede        
| | |           
|---|---|   
|   **Descrição do serviço** |   Identifica as redes ao qual o computador tiver se conectado, coleta e armazena as propriedades para essas redes e notifica os aplicativos quando essas propriedades forem alteradas.
|   **Nome do serviço**    |   netprofm
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="network-location-awareness"></a>Reconhecimento de local de rede           
| | |           
|---|---|       
|   **Descrição do serviço** |   Coleta e armazena informações de configuração para a rede e notifica os programas quando essas informações são modificadas. Se esse serviço for interrompido, as informações de configuração podem ficar indisponíveis. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   NlaSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="network-setup-service"></a>Configurar o serviço de rede       
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço de configuração de rede gerencia a instalação de drivers de rede e permite a definição das configurações de rede de baixo nível.  Se esse serviço for interrompido, as instalações de driver que estão em andamento podem ser canceladas.
|   **Nome do serviço**    |   NetSetupSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="network-store-interface-service"></a>Serviço de Interface de rede Store      
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço fornece notificações de rede (por exemplo, interface adição/exclusão de etc) para clientes do modo de usuário. A interrupção do serviço causará a perda de conectividade de rede. Se esse serviço for desabilitado, quaisquer outros serviços que dependam desse serviço falhará ao iniciar.
|   **Nome do serviço**    |   nsi
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="offline-files"></a>Arquivos Offline            
| | |           
|---|---|       
|   **Descrição do serviço** |   Os arquivos Offline, o serviço executa as atividades de manutenção do cache de arquivos off-line, responde a eventos de logon e logoff do usuário, implementa a parte interna da API pública e expede eventos interessantes para aqueles interessados em arquivos Offline, atividades e alterações no estado do cache.
|   **Nome do serviço**    |   CscService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   
|||         
            
<br />          

## <a name="optimize-drives"></a>Otimizar unidades          
| | |           
|---|---|   
|   **Descrição do serviço** |   Ajuda o computador a executar com mais eficiência ao otimizar arquivos em unidades de armazenamento.
|   **Nome do serviço**    |   defragsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />

## <a name="performance-counter-dll-host"></a>Host DLL do contador de desempenho         
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que usuários remotos e processos de 64 bits consultar contadores de desempenho fornecidos por DLLs de 32 bits. Se esse serviço for parado, apenas usuários locais e os processos de 32 bits será capazes de consultar os contadores de desempenho fornecidos por DLLs de 32 bits.
|   **Nome do serviço**    |   PerfHost
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz    
|   **Comentários**    |   
|||         
            
<br />          

## <a name="performance-logs--alerts"></a>Alertas e Logs de desempenho            
| | |           
|---|---|   
|   **Descrição do serviço** |   Dados de desempenho de coleta de alertas e Logs de desempenho de computadores locais ou remotos com base em parâmetros pré-configurados, em seguida, grava os dados em um log ou dispara um alerta. Se esse serviço for interrompido, as informações de desempenho não serão coletadas. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   CCP
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="phone-service"></a>Serviço de telefone       
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia o estado de telefonia no dispositivo
|   **Nome do serviço**    |   PhoneSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Usado por aplicativos modernos de VoIP
|||         
            
<br />          

##      <a name="plug-and-play"></a>Plug and Play       
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que um computador reconhecer e se adaptar às alterações de hardware com pouca ou nenhuma intervenção do usuário. Interromper ou desabilitar esse serviço resultará em instabilidade no sistema.
|   **Nome do serviço**    |   PlugPlay
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="portable-device-enumerator-service"></a>Serviço de enumerador de dispositivo portátil           
| | |           
|---|---|       
|   **Descrição do serviço** |   Impõe a política de grupo para dispositivos removíveis de armazenamento em massa. Permite que os aplicativos, como o Windows Media Player e o Assistente de importação de imagem para transferir e sincronizar o conteúdo usando dispositivos removíveis de armazenamento em massa.
|   **Nome do serviço**    |   WPDBusEnum
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="power"></a>Potência            
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia a política de energia e entrega de notificação da política de energia.
|   **Nome do serviço**    |   Potência
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="print-spooler"></a>Spooler de impressão            
| | |           
|---|---|   
|   **Descrição do serviço** |   Este serviço coloca no spool de trabalhos de impressão e trata a interação com a impressora.  Se você desativar esse serviço, você não conseguirá imprimir ou ver suas impressoras.
|   **Nome do serviço**    |   Spooler
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Okey para desabilitar se não for um servidor de impressão ou um controlador de domínio
|   **Comentários**    |   Em um controlador de domínio, a instalação da função de controlador de domínio adiciona um thread para o serviço de spooler é responsável por executar a remoção de impressa – remoção dos objetos de fila de impressão obsoletos do Active Directory.  Se o serviço de spooler não está em execução em pelo menos um controlador de domínio em cada site, em seguida, o AD não tem meios a remoção de filas antigas que não existem mais. https://blogs.technet.microsoft.com/askperf/2008/11/18/disabling-unnecessary-services-a-word-to-the-wise/
|||         
            
<br />          

##  <a name="printer-extensions-and-notifications"></a>Notificações e extensões de impressora        
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço abre caixas de diálogo de impressora personalizados e manipula as notificações de um servidor de impressão remoto ou uma impressora. Se você desativar esse serviço, você não será capaz de ver as extensões de impressora ou notificações.
|   **Nome do serviço**    |   PrintNotify
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar se não for um servidor de impressão
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="problem-reports-and-solutions-control-panel-support"></a>Relatórios de problemas e suporte ao painel de controle de soluções     
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço fornece suporte para exibição, o envio e a exclusão de relatórios de problemas de nível de sistema do painel de controle relatórios de problemas e soluções.
|   **Nome do serviço**    |   wercplsupport
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="program-compatibility-assistant-service"></a>Serviço auxiliar de compatibilidade de programa     
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço oferece suporte para o Assistente de compatibilidade de programa (PCA).  O PCA monitora os programas instalados e executados pelo usuário e detecta problemas de compatibilidade conhecidos. Se esse serviço for interrompido, o PCA não funcionará corretamente.
|   **Nome do serviço**    |   PcaSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="quality-windows-audio-video-experience"></a>Quality Windows Audio-Video Experience      
| | |           
|---|---|   
|   **Descrição do serviço** |   Quality Windows Audio Video Experience (qWave) é uma plataforma de rede para áudio vídeo (AV) streaming de aplicativos em redes domésticas IP. qWave aprimora a transmissão de desempenho e confiabilidade, garantindo a rede qualidade de serviço (QoS) para aplicativos de AV de AV. Ele fornece mecanismos para controle de admissão, executar o monitoramento em tempo e imposição, comentários sobre aplicativos e priorização de tráfego.
|   **Nome do serviço**    |   QWAVE
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Serviço de QoS do lado do cliente
|||         
            
<br />          

##      <a name="radio-management-service"></a>Serviço de gerenciamento de rádio        
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerenciamento de rádio e serviço de modo de avião
|   **Nome do serviço**    |   RmSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="remote-access-auto-connection-manager"></a>Gerenciador de Conexão de acesso remoto automático            
| | |           
|---|---|   
|   **Descrição do serviço** |   Cria uma conexão a uma rede remota sempre que um programa faz referência a um nome DNS ou NetBIOS ou o endereço remoto.
|   **Nome do serviço**    |   RasAuto
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="remote-access-connection-manager"></a>Gerenciador de Conexão de acesso remoto         
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia conexões discadas e rede virtual privada (VPN) deste computador à Internet ou outras redes remotas. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   RasMan
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="remote-desktop-configuration"></a>Configuração da Área de Trabalho Remota         
| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço de configuração de área de trabalho remoto (RDCS) é responsável por todos os serviços de área de trabalho remota e área de trabalho remota de configuração relacionados e as atividades de manutenção da sessão que exigem o contexto do sistema. Isso inclui pastas temporárias por sessão, temas da área de trabalho remota e certificados de área de trabalho remota.
|   **Nome do serviço**    |   SessionEnv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   
|||         
            
<br />          

## <a name="remote-desktop-services"></a>Serviços da Área de Trabalho Remota          
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que os usuários se conectem interativamente a um computador remoto. Área de trabalho e servidor de Host de sessão de área de trabalho remota dependem desse serviço.  Para impedir o uso remoto deste computador, desmarque as caixas de seleção na guia remoto do item de painel de controle de propriedades do sistema.
|   **Nome do serviço**    |   TermService
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="remote-desktop-services-usermode-port-redirector"></a>Redirecionador de porta de UserMode de serviços de área de trabalho remota        
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que o redirecionamento de impressoras/unidades/portas para conexões RDP
|   **Nome do serviço**    |   UmRdpService
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Dá suporte a redirecionamentos no lado do servidor da conexão.
|||         
            
<br />          

## <a name="remote-procedure-call-rpc"></a>RPC (Chamada de Procedimento Remoto)          
| | |           
|---|---|   
|   **Descrição do serviço** |   Serviço RPCSS é o Gerenciador de controle de serviço para servidores COM e DCOM. Ele executa as solicitações de ativações de objeto, resoluções de exportador de objeto e coleta de lixo distribuídos para servidores COM e DCOM. Se esse serviço for interrompido ou desabilitado, programas usando COM ou DCOM não funcionará corretamente. É altamente recomendável que você tenha a execução do serviço RPCSS.
|   **Nome do serviço**    |   RpcSs
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="remote-procedure-call-rpc-locator"></a>Localizador RPC (chamada) de procedimento remoto             
| | |               
|---|---|   
|   **Descrição do serviço** |   No Windows 2003 e versões anteriores do Windows, o serviço de localizador de procedimento remoto (RPC) gerencia o banco de dados do serviço de nome RPC. No Windows Vista e versões posteriores do Windows, esse serviço não fornece nenhuma funcionalidade e está presente para compatibilidade de aplicativos.   |
|   **Nome do serviço**    |   RpcLocator  |
|   **Instalação**    |   Com a experiência Desktop    |
|   **StartType**   |   Manual  |
|   **Recomendação**  | Nenhuma diretriz   |
|   **Comentários**    |       |
|||             
                
<br />              

## <a name="remote-registry"></a>Registro Remoto          
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que usuários remotos modificar as configurações de registro neste computador. Se esse serviço for interrompido, o registro pode ser modificado apenas por usuários neste computador. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   RemoteRegistry
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="resultant-set-of-policy-provider"></a>Conjunto de provedor de diretivas resultante            
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um serviço de rede que processa solicitações para simular a aplicação das configurações de diretiva de grupo para um usuário de destino ou o computador em várias situações e calcula as definições de conjunto de diretivas resultante.
|   **Nome do serviço**    |   RSoPProv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |Nenhuma diretriz    
|   **Comentários**    |   
|||         
            
<br />          

## <a name="routing-and-remote-access"></a>Roteamento e Acesso Remoto            
| | |           
|---|---|   
|   **Descrição do serviço** |   Oferece serviços de roteamento a empresas em ambientes de rede de longa distância e de área local.
|   **Nome do serviço**    |   RemoteAccess
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   Já desativado
|||         
            
<br />          

## <a name="rpc-endpoint-mapper"></a>Mapeador de ponto de extremidade RPC          
| | |           
|---|---|   
|   **Descrição do serviço** |   Resolve identificadores de interfaces RPC de pontos de extremidade de transporte. Se esse serviço for interrompido ou desabilitado, os programas usando os serviços de procedimento remoto (RPC) não funcionará corretamente.
|   **Nome do serviço**    |   RpcEptMapper
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="secondary-logon"></a>Logon secundário     
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite iniciar processos com credenciais alternativas. Se esse serviço for interrompido, esse tipo de acesso de logon será indisponível. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   seclogon
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="secure-socket-tunneling-protocol-service"></a>Serviço de protocolo SSTP            
| | |               
|---|---|       
|   **Descrição do serviço** |   Fornece suporte para o SSTP Secure Socket Tunneling Protocol () para se conectar a computadores remotos usando VPN. Se esse serviço for desabilitado, os usuários não poderão usar SSTP para acessar servidores remotos.    |
|   **Nome do serviço**    |   SstpSvc |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Não desabilite  |
|   **Comentários**    |   Desabilitar quebras de RRAS   |
|||             
                
<br />              

## <a name="security-accounts-manager"></a>Gerenciador de contas de segurança            
| | |           
|---|---|       
|   **Descrição do serviço** |   A inicialização deste serviço indica a outros serviços que o Gerenciador de contas de segurança (SAM) está pronto para aceitar solicitações.  Desabilitar esse serviço impedirá que outros serviços no sistema sejam notificados quando o SAM está pronto, que por sua vez pode causar esses serviços falhar ao iniciar corretamente. Esse serviço não deve ser desabilitado.
|   **Nome do serviço**    |   SamSs
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Não desabilite
|   **Comentários**    |   
|||         
            
<br />          

## <a name="sensor-data-service"></a>Serviço de dados de sensor  
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece dados de uma variedade de sensores
|   **Nome do serviço**    |   SensorDataService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />  

## <a name="sensor-monitoring-service"></a>Serviço de monitoramento do sensor            
| | |           
|---|---|       
|   **Descrição do serviço** |   Monitora vários sensores para expor os dados e adaptar-se para o estado do sistema e do usuário.  Se esse serviço for interrompido ou desabilitado, o brilho da tela não se adaptam às condições de iluminação. A interrupção do serviço pode afetar outros recursos também e funcionalidade do sistema.
|   **Nome do serviço**    |   SensrSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          
        
## <a name="sensor-service"></a>Serviço de sensor           
| | |           
|---|---|       
|   **Descrição do serviço** |   Um serviço para sensores que gerencia a funcionalidade de diferentes sensores. Gerencia a orientação de dispositivo simples (SDO) e o histórico dos sensores. Carrega o sensor SDO que relata alterações de orientação do dispositivo.  Se esse serviço for interrompido ou desabilitado, o sensor SDO não será carregado e então rotação automática não ocorrerá. Coleção de históricos de sensores também será interrompida.
|   **Nome do serviço**    |   SensorService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="server"></a>Servidor           
| | |           
|---|---|   
|   **Descrição do serviço** |   Dá suporte a arquivo, impressão e pipes nomeados compartilhamento na rede para este computador. Se esse serviço for interrompido, essas funções estará disponíveis. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   LanmanServer
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Necessário para o gerenciamento remoto, o IPC$, compartilhamento de arquivos SMB
|||         
            
<br />          

## <a name="shell-hardware-detection"></a>Detecção do Hardware do shell             
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece notificações de eventos de hardware de reprodução automática.
|   **Nome do serviço**    |   ShellHWDetection
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="smart-card"></a>Cartão inteligente           
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia o acesso a cartões inteligentes lidos por este computador. Se esse serviço for interrompido, esse computador será não é possível ler os cartões inteligentes. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   SCardSvr
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   
|||         
            
<br />          

## <a name="smart-card-device-enumeration-service"></a>Serviço de enumeração de dispositivo de cartão inteligente                    
| | |               
|---|---|       
|   **Descrição do serviço** |   Cria nós de dispositivo de software para todos os leitores de cartão inteligente acessível a uma determinada sessão. Se esse serviço for desabilitado, APIs do WinRT não poderão enumerar leitores de cartão inteligente.   |
|   **Nome do serviço**    |   ScDeviceEnum    |
|   **Instalação**    |   Sempre instalado    |
|   **StartType**   |   Manual  |
|   **Recomendação**  |   Okey para desabilitar   |
|   **Comentários**    |   Necessário quase que exclusivamente para WinRT aplicativos    |
|||             
                
<br />              

## <a name="smart-card-removal-policy"></a>Política de remoção de cartão inteligente        
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que o sistema seja configurado para bloquear a área de trabalho do usuário após a remoção de cartão inteligente.
|   **Nome do serviço**    |   SCPolicySvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="snmp-trap"></a>Interceptação SNMP            
| | |           
|---|---|       
|   **Descrição do serviço** |   Recebe mensagens de interceptação geradas por agentes de Simple Network Management Protocol (SNMP) locais ou remotos e encaminha as mensagens aos programas de gerenciamento SNMP em execução neste computador. Se esse serviço for interrompido, baseadas em SNMP programas neste computador não receberá mensagens de interceptação SNMP. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   SNMPTRAP
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="software-protection"></a>Proteção de software             
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que o download, instalação e a imposição de licenças digitais para Windows e Windows aplicativos. Se o serviço estiver desabilitado, o sistema operacional e aplicativos licenciados podem ser executadas em um modo de notificação. É altamente recomendável que você desabilite o serviço de proteção de Software.
|   **Nome do serviço**    |   sppsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="special-administration-console-helper"></a>Auxiliar do Console de administração especial        
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que os administradores acessem remotamente um prompt de comando usando os serviços de gerenciamento de emergência.
|   **Nome do serviço**    |   sacsvr
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="spot-verifier"></a>Verificador de pontos            
| | |           
|---|---|   
|   **Descrição do serviço** |   Verifica possíveis corrupções do sistema de arquivos.
|   **Nome do serviço**    |   svsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="ssdp-discovery"></a>Descoberta SSDP           
| | |           
|---|---|   
|   **Descrição do serviço** |   Descobre os dispositivos de rede e serviços que usam o protocolo de descoberta do SSDP, como dispositivos UPnP. Também anuncia dispositivos SSDP e serviços em execução no computador local. Se esse serviço for interrompido, os dispositivos baseados em SSDP não serão descobertos. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   SSDPSRV
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="state-repository-service"></a>Serviço de repositório de estado         
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece suporte a infraestrutura necessária para o modelo de aplicativo.
|   **Nome do serviço**    |   StateRepository
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="still-image-acquisition-events"></a>Eventos de aquisição de imagem fixa
| | |           
|---|---|   
|   **Descrição do serviço** |   Inicia os aplicativos associados aos eventos de aquisição de imagem ainda.
|   **Nome do serviço**    |   WiaRpc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />  

## <a name="storage-service"></a>Serviço de armazenamento          
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece serviços de ativação para as configurações de armazenamento e a expansão de armazenamento externo
|   **Nome do serviço**    |   StorSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="storage-tiers-management"></a>Gerenciamento de camadas de armazenamento        
| | |           
|---|---|   
|   **Descrição do serviço** |   Otimiza a colocação de dados em camadas de armazenamento em todos os espaços de armazenamento em camadas no sistema.
|   **Nome do serviço**    |   TieringEngineService
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="superfetch"></a>Superfetch          
| | |           
|---|---|       
|   **Descrição do serviço** |   Mantém e melhora o desempenho do sistema ao longo do tempo.
|   **Nome do serviço**    |   SysMain
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="sync-host"></a>Host de sincronização            
| | |           
|---|---|       
|   **Descrição do serviço** |   Este serviço sincroniza email, contatos, calendário e vários outros dados do usuário. Email e outros aplicativos dependentes sobre essa funcionalidade não funcionará corretamente quando esse serviço não está em execução.
|   **Nome do serviço**    |   OneSyncSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          

## <a name="system-event-notification-service"></a>Serviço de notificação de eventos do sistema            
| | |           
|---|---|       
|   **Descrição do serviço** |   Monitora eventos do sistema e notifica os assinantes de COM+ Event System desses eventos.
|   **Nome do serviço**    |   SENS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="system-events-broker"></a>Agente de eventos do sistema             
| | |           
|---|---|       
|   **Descrição do serviço** |   Coordena a execução do trabalho em segundo plano para o aplicativo do WinRT. Se esse serviço for interrompido ou desabilitado, trabalho em segundo plano não poderá ser disparado.
|   **Nome do serviço**    |   SystemEventsBroker
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Apesar do fato de que sua descrição implica é apenas para aplicativos do WinRT, ela é necessária para o Agendador de tarefas, serviço de infraestrutura do agente e outros componentes internos.
|||         
            
<br />          

## <a name="task-scheduler"></a>Agendador de Tarefas           
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que um usuário configurar e agendar tarefas automatizadas neste computador. O serviço também hospeda várias tarefas críticas de sistema do Windows. Se esse serviço for interrompido ou desabilitado, essas tarefas não serão executadas em agendados vezes. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   Agendamento
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="tcpip-netbios-helper"></a>Auxiliar de NetBIOS TCP/IP            
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece suporte para o NetBIOS sobre o serviço TCP/IP (NetBT) e resolução de nomes NetBIOS para clientes na rede, portanto permitindo que os usuários compartilhem arquivos, impressão e fazer logon rede. Se esse serviço for interrompido, essas funções poderão ficar indisponíveis. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   lmhosts
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="telephony"></a>Telefonia           
| | |           
|---|---|       
|   **Descrição do serviço** |   Oferece suporte à API de telefonia (TAPI) para programas que controlam dispositivos de telefonia no computador local e, através da rede local, em servidores que estejam executando o serviço.
|   **Nome do serviço**    |   TapiSrv
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Desabilitar quebras de RRAS
|||         
            
<br />          

## <a name="themes"></a>Temas           
| | |           
|---|---|
|   **Descrição do serviço** |   Fornece gerenciamento de tema de experiência do usuário.
|   **Nome do serviço**    |   Temas
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Não é possível definir temas de acessibilidade quando esse serviço está desabilitado
|||         
            
<br />  

## <a name="tile-data-model-server"></a>Servidor de peças do modelo de dados           
| | |           
|---|---|   
|   **Descrição do serviço** |   Bloco do servidor de atualizações de bloco.
|   **Nome do serviço**    |   tiledatamodelsvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Iniciar quebras de menu, se esse serviço for desabilitado
|||         
            
<br />          

##  <a name="time-broker"></a>Agente de tempo     
| | |           
|---|---|       
|   **Descrição do serviço** |   Coordena a execução do trabalho em segundo plano para o aplicativo do WinRT. Se esse serviço for interrompido ou desabilitado, trabalho em segundo plano não poderá ser disparado.
|   **Nome do serviço**    |   TimeBrokerSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Apesar do fato de que sua descrição implica é apenas para aplicativos do WinRT, ela é necessária para o Agendador de tarefas, serviço de infraestrutura do agente e outros componentes internos.
|||         
            
<br />          

## <a name="touch-keyboard-and-handwriting-panel-service"></a>Teclado de toque e o serviço do painel de manuscrito         
| | |           
|---|---|   
|   **Descrição do serviço** |   Habilita a funcionalidade de caneta e tinta de teclado de toque e o painel de manuscrito
|   **Nome do serviço**    |   TabletInputService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="update-orchestrator-service-for-windows-update"></a>Atualizar serviço Orchestrator para atualização do Windows           
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia as atualizações do Windows. Se interrompida, seus dispositivos não poderão baixar e instalar as atualizações mais recentes.
|   **Nome do serviço**    |   UsoSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Descrição do serviço estava ausente na v1607; Atualização do Windows (incluindo WSUS) depende desse serviço.
|||         
            
<br />          

## <a name="upnp-device-host"></a>UPnP Device Host         
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que os dispositivos UPnP ser hospedado neste computador. Se esse serviço for interrompido, os dispositivos UPnP hospedados deixará de funcionar e nenhum outro dispositivo hospedado pode ser adicionado. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   upnphost
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="user-access-logging-service"></a>Serviço de registro em log de acesso de usuário          
| | |           
|---|---|   
|   **Descrição do serviço** |   Este serviço faz solicitações de acesso de cliente exclusivo, na forma de endereços IP e nomes de usuário, dos produtos instalados e funções no servidor local. Essas informações podem ser consultadas, por meio do Powershell, por administradores que precisam quantificar a demanda do cliente de software de servidor para gerenciamento de licença de acesso de cliente (CAL) offline. Se o serviço estiver desabilitado, as solicitações do cliente não serão registradas e não poderá ser recuperadas por meio de consultas do Powershell. Parando o serviço não afetará a consulta de dados históricos (consulte a documentação para obter as etapas excluir os dados históricos de suporte). O administrador do sistema local precisa consultar seu ou dela, termos de licença do Windows Server para determinar o número de CALs que são necessários para o software de servidor ser licenciado adequadamente; usar o serviço UAL e dados não alteram essa obrigação.
|   **Nome do serviço**    |   UALSVC
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="user-data-access"></a>Acesso de dados do usuário        
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece acesso de aplicativos para dados estruturados de usuário, incluindo informações de contato, calendários, mensagens e outros tipos de conteúdo. Se você parar ou desabilitar esse serviço, os aplicativos que usam esses dados podem não funcionar corretamente.
|   **Nome do serviço**    |   UserDataSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          

## <a name="user-data-storage"></a>Armazenamento de dados do usuário            
| | |           
|---|---|       
|   **Descrição do serviço** |   Lida com o armazenamento estruturado de dados do usuário, incluindo informações de contato, calendários, mensagens e outros tipos de conteúdo. Se você parar ou desabilitar esse serviço, os aplicativos que usam esses dados podem não funcionar corretamente.
|   **Nome do serviço**    |   UnistoreSvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          

## <a name="user-experience-virtualization-service"></a>Serviço de virtualização de experiência do usuário           
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece suporte para configurações de aplicativo e sistema operacional móvel
|   **Nome do serviço**    |   UevAgentService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="user-manager"></a>Gerenciador de usuários        
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerenciador de usuário fornece os componentes de tempo de execução necessários para interação de vários usuário.  Se esse serviço for interrompido, alguns aplicativos podem não funcionar corretamente.
|   **Nome do serviço**    |   UserManager
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="user-profile-service"></a>Serviço de perfil do usuário         
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço é responsável por carregar e descarregar os perfis de usuário. Se esse serviço for interrompido ou desabilitado, os usuários não poderão entrar com êxito ou saia, aplicativos talvez tenha problemas para os dados dos usuários e componentes registrados para receber notificações de eventos de perfil não recebem-las.
|   **Nome do serviço**    |   ProfSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="virtual-disk"></a>Disco virtual             
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de gerenciamento de discos, volumes, sistemas de arquivos e as matrizes de armazenamento.
|   **Nome do serviço**    |   vds
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Nenhuma diretriz
|   **Comentários**    |   
|||         
            
<br />          

## <a name="volume-shadow-copy"></a>Cópia de sombra de volume           
| | |           
|---|---|   
|   **Descrição do serviço** |   Gerencia e implementa cópias de sombra de Volume usadas para backup e outros fins. Se esse serviço for interrompido, as cópias de sombra não estará disponíveis para backup e o backup pode falhar. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   VSS
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Nenhuma diretriz
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="walletservice"></a>WalletService           
| | |           
|---|---|   
|   **Descrição do serviço** |   Objetos de hosts usados pelos clientes da embalagem do
|   **Nome do serviço**    |   WalletService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-audio"></a>Áudio do Windows            
| | |           
|---|---|       
|   **Descrição do serviço** |   Gerencia áudio para programas baseados no Windows.  Se esse serviço for interrompido, os dispositivos de áudio e efeitos não funcionará corretamente.  Se esse serviço for desabilitado, quaisquer serviços que dependam explicitamente dele falharão ao iniciar
|   **Nome do serviço**    |   Audiosrv
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-audio-endpoint-builder"></a>Construtor de ponto de extremidade de áudio do Windows           
| | |           
|---|---|
|   **Descrição do serviço** |   Gerencia os dispositivos de áudio para o serviço de áudio do Windows.  Se esse serviço for interrompido, os dispositivos de áudio e efeitos não funcionará corretamente.  Se esse serviço for desabilitado, quaisquer serviços que dependam explicitamente dele falharão ao iniciar
|   **Nome do serviço**    |   AudioEndpointBuilder
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-biometric-service"></a>Serviço de biometria do Windows            
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço de biometria do Windows fornece a capacidade de capturar, comparar, manipular e armazenem dados biométricos, sem ganhar acesso direto a qualquer hardware ou amostra biométrica de aplicativos cliente. O serviço está hospedado em um processo SVCHOST privilegiado.
|   **Nome do serviço**    |   WbioSrvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-camera-frame-server"></a>Windows Camera Frame Server         
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que vários clientes acessar os quadros de vídeo em dispositivos de câmera.
|   **Nome do serviço**    |   FrameServer
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-connection-manager"></a>Gerenciador de Conexão do Windows           
| | |           
|---|---|   
|   **Descrição do serviço** |   Torna automática conectar/desconectar decisões com base nas opções de conectividade de rede está disponíveis para o PC e permite o gerenciamento de conectividade de rede com base nas configurações de diretiva de grupo.
|   **Nome do serviço**    |   Wcmsvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-defender-network-inspection-service"></a>Serviço de inspeção de rede do Windows Defender          
| | |           
|---|---|       
|   **Descrição do serviço** |   Ajuda a proteger contra tentativas de invasão direcionamento de vulnerabilidades conhecidas e descobertas recentemente em protocolos de rede
|   **Nome do serviço**    |   WdNisSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz    
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-defender-service"></a>Windows Defender Service         
| | |           
|---|---|       
|   **Descrição do serviço** |   Ajuda a proteger os usuários contra malware e outros softwares potencialmente indesejados
|   **Nome do serviço**    |   WinDefend
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-driver-foundation---user-mode-driver-framework"></a>Windows Driver Foundation - estrutura de Driver de modo de usuário           
| | |           
|---|---|   
|   **Descrição do serviço** |   Cria e gerencia os processos de driver do modo de usuário. Esse serviço não pode ser interrompido.
|   **Nome do serviço**    |   wudfsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-encryption-provider-host-service"></a>Serviço de Host de provedor de criptografia do Windows     
| | |           
|---|---|   
|   **Descrição do serviço** |   Criptografia do serviço de Host de provedor de criptografia do Windows agentes funcionalidades relacionadas de provedores de criptografia de terceiros para processos que precisam para avaliar e aplicar políticas EAS. Parar isso comprometerá verificações de conformidade do EAS que foram estabelecidas pelas contas de email conectados
|   **Nome do serviço**    |   WEPHOSTSVC
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-error-reporting-service"></a>Serviço de Relatório de Erros do Windows          
| | |           
|---|---|       
|   **Descrição do serviço** |   Permite que os erros sejam relatados quando programas parar de funcionar ou de responder e permite que as soluções existentes sejam entregues. Também permite que os logs a serem gerados para diagnóstico e reparar os serviços. Se esse serviço for interrompido, o relatório de erros pode não funcionar corretamente e resultados de serviços de diagnóstico e reparos podem não ser exibidos.
|   **Nome do serviço**    |   WerSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Coleta e envia dados de travamento ou usados pelo MS e IHVs/ISVs de terceiros. Os dados são usados para diagnosticar falhas – induzindo bugs, que podem incluir bugs de segurança. Também é necessário para o relatório de erros corporativo
|||         
            
<br />          

## <a name="windows-event-collector"></a>Coletor de Eventos do Windows          
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço gerencia inscrições persistentes em eventos de origens remotas que dão suporte ao protocolo WS-Management. Isso inclui logs de eventos do Windows Vista, hardware e fontes de evento IPMI habilitado. O serviço armazena eventos enviados em um Log de eventos local. Se esse serviço for interrompido ou desabilitado não não possível criar assinaturas de evento e eventos encaminhados não podem ser aceito.
|   **Nome do serviço**    |   Wecsvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Coleta eventos ETW (incluindo eventos de segurança) para a capacidade de gerenciamento, o diagnóstico.  Muitos dos recursos e ferramentas de terceiros se basear nele, incluindo ferramentas de auditoria de segurança
|||         
            
<br />          

## <a name="windows-event-log"></a>Log de eventos do Windows            
| | |           
|---|---|       
|   **Descrição do serviço** |   Esse serviço gerencia eventos e logs de eventos. Ele dá suporte ao log de eventos, eventos, inscrever-se em eventos, arquivar logs de eventos e gerenciamento de metadados de evento de consulta. Ele pode exibir eventos no formato XML e texto sem formatação. A interrupção do serviço pode comprometer a segurança e confiabilidade do sistema.
|   **Nome do serviço**    |   EventLog
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-firewall"></a>Firewall do Windows         
| | |           
|---|---|   
|   **Descrição do serviço** |   Firewall do Windows ajuda a proteger seu computador, impedindo que usuários não autorizados obtenham acesso ao seu computador pela Internet ou por uma rede.
|   **Nome do serviço**    |   MpsSvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-font-cache-service"></a>Serviço de Cache de fonte do Windows      
| | |           
|---|---|   
|   **Descrição do serviço** |   Otimiza o desempenho de aplicativos armazenando em cache os dados de fontes comumente usadas. Aplicativos iniciará esse serviço se ainda não estiver sendo executado. Ele pode ser desabilitado, embora isso diminuirá o desempenho de aplicativo.
|   **Nome do serviço**    |   FontCache
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-image-acquisition-wia"></a>Aquisição de imagens do Windows (WIA)          
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de aquisição de imagem de scanners e câmeras
|   **Nome do serviço**    |   stisvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-insider-service"></a>Serviço do Windows Insider     
| | |           
|---|---|   
|   **Descrição do serviço** |   wisvc
|   **Nome do serviço**    |   wisvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Servidor não dá suporte a flighting, portanto, é não operacional no servidor. Recurso também pode ser desabilitado por meio da política de grupo.
|||         
            
<br />          

##  <a name="windows-installer"></a>Windows Installer       
| | |           
|---|---|
|   **Descrição do serviço** |   Adiciona, modifica e remove aplicativos fornecidos como um pacote do Windows Installer (*. msi, *. msp). Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   msiserver
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-license-manager-service"></a>Serviço de Gerenciador de licenças do Windows          
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece suporte de infraestrutura para a Microsoft Store.  Esse serviço é iniciado sob demanda e se, em seguida, desabilitada adquirido por meio do Microsoft Store de conteúdo não funcionará corretamente.
|   **Nome do serviço**    |   LicenseManager
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-management-instrumentation"></a>Instrumentação de Gerenciamento do Windows       
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece um modelo de objeto e de interface comum para acessar informações de gerenciamento sobre o sistema operacional, dispositivos, aplicativos e serviços. Se esse serviço for interrompido, a maioria dos softwares baseados no Windows não funcionará corretamente. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   Winmgmt
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-mobile-hotspot-service"></a>Serviço de ponto de acesso móvel do Windows          
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece a capacidade de compartilhar uma conexão de dados de celular com outro dispositivo.
|   **Nome do serviço**    |   icssvc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-modules-installer"></a>Instalador de módulos do Windows        
| | |           
|---|---|   
|   **Descrição do serviço** |   Permite que a instalação, modificação e remoção das atualizações do Windows e componentes opcionais. Se esse serviço for desabilitado, instalar ou desinstalar do Windows, as atualizações podem falhar para este computador.
|   **Nome do serviço**    |   TrustedInstaller
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-push-notifications-system-service"></a>Serviço de sistema de notificações de envio por Push do Windows            
| | |           
|---|---|
|   **Descrição do serviço** |   Esse serviço é executado na sessão 0 e hospeda o provedor de plataforma e conexão de notificação que manipula a conexão entre o dispositivo e o servidor WNS.
|   **Nome do serviço**    |   WpnService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Automática
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Necessário para blocos dinâmicos e outros recursos
|||         
            
<br />      

## <a name="windows-push-notifications-user-service"></a>Serviço de usuário de notificações por Push do Windows          
| | |           
|---|---|   
|   **Descrição do serviço** |   Esse serviço hospeda a plataforma de notificação do Windows que fornece suporte para o local e notificações por push. As notificações com suporte são o bloco, toast e bruto.
|   **Nome do serviço**    |   WpnUserService
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Okey para desabilitar
|   **Comentários**    |   Modelo de serviço do usuário
|||         
            
<br />          
    
## <a name="windows-remote-management-ws-management"></a>Windows Remote Management (WS-Management)            
| | |           
|---|---|   
|   **Descrição do serviço** |   O serviço Windows Remote Management (WinRM) implementa o protocolo WS-Management para gerenciamento remoto. O WS-Management é um protocolo de serviços Web padrão usado para gerenciamento remoto de software e hardware. O serviço WinRM lê na rede as solicitações do WS-Management e as processa. O serviço WinRM precisa ser configurado com um ouvinte usando a ferramenta de linha de comando winrm.cmd ou por meio da Política de Grupo para que ele ouça a rede. O serviço WinRM fornece acesso aos dados WMI e possibilita a coleção de eventos. A coleção de eventos e a assinatura de eventos exigem que o serviço esteja em execução. As mensagens WinRM usam HTTP e HTTPS à medida que são transportadas. O serviço WinRM não depende do IIS, mas é pré-configurado para compartilhar uma porta com IIS no mesmo computador.  O serviço WinRM reserva o prefixo de URL /wsman. Para evitar conflito com IIS, os administradores devem garantir que quaisquer sites da Web hospedados no IIS não usem o prefixo de URL /wsman.
|   **Nome do serviço**    |   WinRM
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Necessário para o gerenciamento remoto
|||         
            
<br />          

##  <a name="windows-search"></a>Windows Search      
| | |           
|---|---|       
|   **Descrição do serviço** |   Fornece a indexação de conteúdo, o cache de propriedade e os resultados da pesquisa para arquivos, email e outros tipos de conteúdo.
|   **Nome do serviço**    |   WSearch
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Desabilitada
|   **Recomendação**  |   Já desativado
|   **Comentários**    |   
|||         
            
<br />          

##  <a name="windows-time"></a>Tempo do Windows        
| | |           
|---|---|   
|   **Descrição do serviço** |   Mantém a sincronização de data e hora em todos os clientes e servidores na rede. Se esse serviço for interrompido, sincronização de data e hora ficará indisponível. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   W32Time
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz
|   **Comentários**    |   
|||         
            
<br />          

## <a name="windows-update"></a>Windows Update           
| | |           
|---|---|       
|   **Descrição do serviço** |   Habilita a detecção, download e instalação de atualizações do Windows e outros programas. Se esse serviço for desabilitado, os usuários deste computador não poderão usar o Windows Update ou o recurso de atualização automática e programas não poderão usar o Windows Update Agent (WUA) API.
|   **Nome do serviço**    |   wuauserv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="winhttp-web-proxy-auto-discovery-service"></a>Serviço de descoberta automática de Proxy da Web do WinHTTP         
| | |           
|---|---|   
|   **Descrição do serviço** |   O WinHTTP implementa a pilha HTTP de cliente e fornece aos desenvolvedores uma API do Win32 e um componente de automação COM para enviar solicitações HTTP e receber respostas. Além disso, o WinHTTP fornece suporte para a descoberta automática de uma configuração de proxy por meio de sua implementação do protocolo de descoberta automática de Proxy da Web (WPAD).
|   **Nome do serviço**    |   WinHttpAutoProxySvc
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  |   Não desabilite
|   **Comentários**    |   Qualquer coisa que usa a pilha de rede pode ter uma dependência funcional nesse serviço. Muitas organizações contam com esta opção para configurar o roteamento de proxy HTTP de suas redes internas.  Sem ele, conexões de HTTP originado internamente com a Internet todos falhará.
|||         
            
<br />          

## <a name="wired-autoconfig"></a>AutoConfig com fio         
| | |           
|---|---|       
|   **Descrição do serviço** |   O serviço AutoConfig com fio (DOT3SVC) é responsável por executar IEEE 802.1 X autenticação em interfaces Ethernet. Se sua implantação atual de rede com fio impõe a autenticação 802.1X, o serviço de DOT3SVC deve ser configurado para ser executado para estabelecer a conectividade de camada 2 e/ou o fornecimento de acesso aos recursos da rede. Redes com fio que não impõem a autenticação 802.1 X não são afetados pelo serviço DOT3SVC.
|   **Nome do serviço**    |   dot3svc
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz   
|   **Comentários**    |   
|||         
            
<br />          

## <a name="wmi-performance-adapter"></a>Adaptador de desempenho WMI          
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece informações de biblioteca de desempenho de provedores do Windows Management Instrumentation (WMI) para clientes na rede. Esse serviço é executado apenas quando o auxiliar de dados de desempenho é ativado.
|   **Nome do serviço**    |   wmiApSrv
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Manual
|   **Recomendação**  | Nenhuma diretriz       
|   **Comentários**    |   
|||         
            
<br />          

## <a name="workstation"></a>estação de trabalho          
| | |           
|---|---|   
|   **Descrição do serviço** |   Cria e mantém conexões de rede de cliente para servidores remotos usando o protocolo SMB. Se esse serviço for interrompido, estas conexões ficarão indisponíveis. Se esse serviço for desabilitado, quaisquer serviços que dependam dele não falhará ao iniciar.
|   **Nome do serviço**    |   LanmanWorkstation
|   **Instalação**    |   Sempre instalado
|   **StartType**   |   Automática
|   **Recomendação**  | Nenhuma diretriz       
|   **Comentários**    |   
|||         
            
<br />

## <a name="xbox-live-auth-manager"></a>Gerenciador de autenticação ao vivo do Xbox           
| | |           
|---|---|   
|   **Descrição do serviço** |   Fornece serviços de autenticação e autorização para interagir com o Xbox Live. Se esse serviço for interrompido, alguns aplicativos podem não funcionar corretamente.
|   **Nome do serviço**    |   XblAuthManager
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Deve ser desabilitado
|   **Comentários**    |   
|||         
            
<br />          

## <a name="xbox-live-game-save"></a>Salvar jogo do Xbox Live          
| | |           
|---|---|   
|   **Descrição do serviço** |   As sincronizações esse serviço salvar dados do Xbox Live salvar jogos habilitados.  Se esse serviço for interrompido, o jogo salvam dados não carregar ou baixar do Xbox Live.
|   **Nome do serviço**    |   XblGameSave
|   **Instalação**    |   Com a experiência Desktop
|   **StartType**   |   Manual
|   **Recomendação**  |   Deve ser desabilitado
|   **Comentários**    |   As sincronizações esse serviço salvar dados do Xbox Live salvar jogos habilitados.  Se esse serviço for interrompido, o jogo salvam dados não carregar ou baixar do Xbox Live.
|||         
                
<br /> 
<br /> 

