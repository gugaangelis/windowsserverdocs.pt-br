---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: "Apêndice a administração simplificada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5de7431d0f3fe9a078432b11a63ce996d3abe447
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="simplified-administration-appendix"></a>Apêndice a administração simplificada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [Caixa de diálogo de servidores (Active Directory) de adicionar do Gerenciador do servidor](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [Status do servidor de controle remoto do Gerenciador do servidor](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Carregamento de módulo do PowerShell do Windows](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [Marca d'emissão Hotfixes para sistemas operacionais anteriores](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe instalar da mídia muda](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>Caixa de diálogo de servidores (Active Directory) de adicionar do Gerenciador do servidor  

O **adicionar servidores** caixa de diálogo permite pesquisar no Active Directory para servidores, pelo sistema operacional, uso de curingas e por localização. A caixa de diálogo também permite usar consultas do DNS, nome de domínio totalmente qualificado ou nome de prefixo. Essas pesquisas usam protocolos DNS e LDAP nativos implementados por meio do .NET, não anúncios do Windows PowerShell contra o Gateway de gerenciamento de anúncios por meio de SOAP - o que significa que os controladores de domínio contatados pelo Gerenciador de servidores ainda podem executar o Windows Server 2003. Você também pode importar um arquivo com nomes de servidor para fins de provisionamento.  
  
A pesquisa do Active Directory usa os seguintes filtros LDAP:  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
A pesquisa do Active Directory retorna os seguintes atributos:  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>Status do servidor de controle remoto do Gerenciador do servidor  
Gerenciador do servidor testes de acessibilidade de servidor remoto usando o protocolo de roteamento de endereço. Todos os servidores não está respondendo às solicitações de ARP não estiverem listados, mesmo que eles estejam no pool.  
  
Se ARP responde, conexões DCOM e WMI são feitas ao servidor para retornar informações de status. Se RPC, DCOM e WMI são inacessível, o Gerenciador do servidor totalmente não poderá gerenciar o servidor.  
  
## <a name="BKMK_PSLoadModule"></a>Carregamento de módulo do PowerShell do Windows  
O Windows PowerShell 3.0 implementa módulo dinâmico de carregamento. Usando o **Import-Module** cmdlet normalmente não é mais necessário; em vez disso, basta invocar o cmdlet, alias ou função automaticamente carrega o módulo.  
  
Para ver os módulos carregados, use o **Get-Module** cmdlet.  
  
```  
Get-Module  
  
```  
  
![Administração simplificada](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Para ver todos os módulos com suas funções exportadas e cmdlets, use:  
  
```  
Get-Module -ListAvailable  
  
```  
  
O caso principal para usar o **import-module** comando é quando você precisa de acesso para o "anúncio:" unidade virtual do Windows PowerShell e nada mais já carregou o módulo. Por exemplo, usando os seguintes comandos:  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>Marca d'emissão Hotfixes para sistemas operacionais anteriores  
Consulte [uma atualização está disponível para detectar e evitar excesso consumo do pool de RID global em um controlador de domínio que esteja executando o Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe instalar da mídia muda  
Windows Server 2012 adiciona duas opções adicionais para a ferramenta de linha de comando Ntdsutil.exe para o **IFM (criação da mídia IFM)** menu. Elas permitem que você criar repositórios de IFM sem executar primeiro uma desfragmentação offline do NTDS exportado. Arquivo de banco de dados DIT. Quando o espaço em disco não é fundamental, isso economiza tempo criando o IFM.  
  
A tabela a seguir descreve os dois novos itens de menu:  
  
|||  
|-|-|  
|Item de menu|Explicação|  
|Criar NoDefrag completa %s|Criar uma mídia IFM sem ser desfragmentado para um controlador de domínio do AD completo ou uma instância do AD/LDS na pasta %s|  
|Criar Sysvol inteira NoDefrag %s|Criar uma mídia IFM com SYSVOL e sem ser desfragmentado para um controlador de domínio do AD completo para a pasta %s|  
  
![Administração simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Administração simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


