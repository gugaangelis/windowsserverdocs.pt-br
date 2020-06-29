---
title: Introdução ao log de acesso do usuário
desctription: Describes the User Access Logging feature and how to start using it.
ms.prod: windows-server
ms.technology: manage-user-access-logging
ms.topic: article
ms.assetid: 5c395b8b-3b35-4042-b9cc-07e438f86d50
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44bcdd3d89946558934b8309634061f6b8e7ffda
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471161"
---
# <a name="get-started-with-user-access-logging"></a>Introdução ao log de acesso do usuário

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O registro de acesso do usuário (UAL) é um recurso no Windows Server que agrega dados de uso do cliente por função e produtos em um servidor local. Ele ajuda os administradores do Windows Server a quantificar as solicitações de computadores cliente para funções e serviços em um servidor local.

O UAL é instalado e habilitado por padrão e coleta dados em quase tempo real. Nenhuma configuração de Administrador é necessária, embora o UAL possa ser desabilitado ou habilitado. Para obter mais informações, consulte [Gerenciar o Log de Acesso do Usuário](Manage-User-Access-Logging.md). O serviço de log de acesso do usuário agrega dados de uso do cliente por funções e produtos em arquivos de banco de dados local.  Os administradores de TI podem fazer uso de WMIx (Instrumentação de Gerenciamento do Windows) posterior ou dos cmdlets do Windows PowerShell para recuperar quantidades e instâncias por função de servidor (ou produto de software), por usuário, dispositivo, servidor local e data.

> [!NOTE]
> O UAL dá suporte ao [Microsoft Assessment and Planning Toolkit](https://go.microsoft.com/fwlink/?LinkID=111000).

## <a name="practical-applications"></a><a name="BKMK_APP"></a>Aplicações práticas
O UAL agrega os eventos de solicitação de usuário e de dispositivo de cliente exclusivos que são registrados em um banco de dados local. Esses registros são disponibilizados (através de uma consulta por um administrador de servidor) para recuperar quantidades e instâncias por função de servidor, usuário, dispositivo, servidor local e data.  Além disso, o UAL foi estendido para permitir que desenvolvedores de software não Microsoft instrumentem seus eventos UAL a serem agregados pelo Windows Server.

O UAL pode executar as seguintes tarefas:

-   Quantificar solicitações do usuário do cliente por servidores locais físicos ou virtuais.

-   Quantificar solicitações do usuário do cliente por produtos de software instalados em um servidor físico ou virtual local.

-   Recuperar dados em um servidor local que executa o Hyper-V para identificar períodos de alta e baixa demanda em um computador virtual do Hyper-V.

-   Recuperar dados do UAL de vários servidores remotos.

Além disso, os desenvolvedores de software podem instrumentar eventos UAL que podem ser agregados e recuperados usando as interfaces WMI e Windows PowerShell.

As seguintes funções de servidor e serviços podem ter suporte no UAL:

-   AD CS (Serviços de Certificados do Active Directory)

-   AD RMS (Active Director Rights Management Services)

-   BranchCache

-   Sistema de nome de domínio (DNS)

    > [!NOTE]
    > O UAL coleta dados do DNS a cada 24 horas e existe um cmdlet do UAL separado para este cenário.

-   Protocolo DHCP

-   Servidor de Fax

-   Serviços de arquivo

-   O servidor de protocolo FTP

-   Hyper-V

    > [!NOTE]
    > O UAL coleta dados do Hyper-V a cada 24 horas e existe um cmdlet do UAL separado para este cenário.

-   Servidor Web (IIS)

    > [!WARNING]
    > Para usar o UAL com o IIS, você deve usar o iisual.exe. Para obter mais informações, consulte [Analisando dados de uso de cliente com o registro de acesso de usuário do IIS](https://www.iis.net/learn/manage/configuring-security/analyzing-client-usage-data-with-iis-user-access-logging).

-   Serviços de fila de mensagens da Microsoft (MSMQ)

-   Network Policy and Access Services

-   Serviços de impressão e documentos

-   Serviço de Roteamento e Acesso Remoto (RRAS)

-   WDS (Serviços de Implantação do Windows)

-   Windows Server Update Services (WSUS)

> [!IMPORTANT]
> O UAL não é recomendado para uso em servidores conectados diretamente na Internet, como servidores Web em um espaço de endereço acessível pela Internet, ou nos cenários em que um desempenho extremamente alto é a principal função do servidor (como nos ambientes de carga de trabalho HPC). O UAL destina-se principalmente a cenários de intranet pequenos, médios e empresariais em que o alto volume é esperado, mas não tão alto quanto as implantações que servem o volume de tráfego voltado para a Internet regularmente.

## <a name="important-functionality"></a><a name="BKMK_NEW"></a>Funcionalidade importante
A tabela a seguir descreve as funções chave do UAL e seus possíveis valores.

|Funcionalidade|Valor|
|-----------------|---------|
|Coletar e agregar dados do evento de solicitação de cliente quase em tempo real.|Até três anos de dados podem ser salvos. **Importante:** Os administradores precisam impor a conformidade dos dados coletados e os períodos de retenção de dados com a política de privacidade e as normas locais da organização.|
|Consulte o UAL usando interfaces WMI ou do Windows PowerShell para recuperar dados de solicitação de cliente em um servidor local ou remoto.|O UAL permite um único modo de exibição de dados de uso contínuo. Administradores corporativos e de servidor podem recuperar esses dados e coordenar com os administradores de negócios para otimizar o uso de suas licenças de software por volume.|
|Habilitado por padrão.|Os administradores de servidor não precisam configurar ou, de alguma forma, instalar esse recurso para todas as funcionalidades principais ficarem disponíveis e funcionando.|

## <a name="data-logged-with-ual"></a>Dados registrados no UAL
Os seguintes dados relacionados ao usuário são registrados no UAL.

|Dados|Descrição|
|--------|---------------|
|**Usu**|Se aplicável, o nome de usuário do cliente acompanha as entradas do UAL de produtos e funções instaladas.|
|**ActivityCount**|O número de vezes que um usuário específico acessa uma função ou um serviço.|
|**FirstSeen**|A data e hora em que um usuário acessa primeiro uma função ou um serviço.|
|**LastSeen**|A data e hora em que um usuário acessou pela última vez uma função ou um serviço.|
|**NomeDoProduto**|O nome do produto de software principal, como o Windows, que está fornecendo os dados do UAL.|
|**RoleGUID**|O GUID atribuído ou registrado do UAL que representa a função de servidor ou o produto instalado.|
|**RoleName**|O nome da função, do componente ou do subproduto que está fornecendo os dados do UAL. Também é associado a um ProductName e a um RoleGUID.|
|**TenantIdentifier**|Um GUID exclusivo de um cliente locatário de uma função instalada ou de um produto que acompanha os dados do UAL, se aplicável.|

Os seguintes dados relacionados ao dispositivo são registrados no UAL.

|Dados|Descrição|
|--------|---------------|
|**EndereçoIP**|O endereço IP de um dispositivo cliente que é usado para acessar uma função ou um serviço.|
|**ActivityCount**|O número de vezes que um determinado dispositivo acessou a função ou um serviço.|
|**FirstSeen**|A data e hora em que um endereço IP foi usado pela primeira vez para acessar uma função ou um serviço.|
|**LastSeen**|A data e hora em que um endereço IP foi usado pela última vez para acessar uma função ou um serviço.|
|**NomeDoProduto**|O nome do produto de software principal, como o Windows, que está fornecendo os dados do UAL.|
|**RoleGUID**|O GUID registrado ou UAL atribuído que representa a função de servidor ou o produto instalado.|
|**RoleName**|O nome da função, do componente ou do subproduto que está fornecendo os dados do UAL. Também é associado a um ProductName e a um RoleGUID.|
|**TenantIdentifier**|Um GUID exclusivo de um cliente locatário de uma função instalada ou de um produto que acompanha os dados do UAL, se aplicável.|

## <a name="software-requirements"></a><a name="BKMK_SOFT"></a>Requisitos de software
O UAL pode ser usado em qualquer computador que esteja executando versões do Windows Server após o Windows Server 2012.

## <a name="additional-references"></a>Referências adicionais
[Usuário acessar o registro em log](https://msdn.microsoft.com/library/windows/desktop/hh437528(v=vs.85).aspx) no MSDN.
[Gerenciar o registro em log de acesso do usuário](Manage-User-Access-Logging.md)


