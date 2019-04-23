---
title: Etapa 2 configurar o servidor de VPN do DirectAccess
description: Este tópico faz parte do guia adicionar DirectAccess a uma implantação de acesso remoto existente (VPN) para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 31a5310ddb59831650f6b46108d071c120116dc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830927"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>Etapa 2 configurar o servidor de VPN do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como configurar o cliente e as configurações de servidor necessárias para uma implantação básica do Acesso Remoto usando o Assistente para Habilitar o DirectAccess.

A tabela a seguir fornece uma visão geral das etapas que você pode concluir usando neste tópico.

|Tarefa       |Descrição|
|-----------|-----------|
|Configurar os clientes de DirectAccess|Configurar o servidor de Acesso Remoto com os grupos de segurança contendo os clientes do DirectAccess.|
|Configurar a Topologia de Rede|Definir as configurações do servidor de Acesso Remoto.|
|Configurar a Lista de Pesquisa de Sufixos do DNS|Modificar a Lista de pesquisa de sufixos, se desejado.|
|Configuração do GPO|Modificar os GPOs, se desejado.|

## <a name="to-start-the-enable-directacces-wizard"></a>Para iniciar o Assistente para Habilitar o DirectAccess

1. No Gerenciador do servidor, clique em **ferramentas**e, em seguida, clique em **acesso remoto**. O Assistente para habilitar o DirectAccess é iniciado automaticamente, a menos que você selecionou **não mostrar esta tela novamente**. 

2. Se o assistente não for iniciado automaticamente, clique com botão direito no nó do servidor na árvore de roteamento e acesso remoto e, em seguida, clique em **habilitar o DirectAccess**.

3. Clique em **Avançar**.

## <a name="configure-directaccess-clients"></a>Configurar os clientes de DirectAccess

Para que um computador cliente possa ser provisionado para usar o DirectAccess, ele deverá pertencer ao grupo de segurança selecionado. Depois de configurado o DirectAccess, os computadores cliente do grupo de segurança são provisionados para receber a política de grupo do DirectAccess.

1. Na página **Selecionar Grupos**, clique em **Adicionar**.

2. Na caixa de diálogo **Selecionar Grupos**, selecione os grupos de segurança que contêm computadores cliente do DirectAccess.

3. Marque a caixa de seleção **Habilitar o DirectAccess apenas para computadores móveis** para permitir que somente computadores móveis acessem a rede interna.

4. Marque a caixa de seleção **Usar criação de túneis à força** para rotear todo o tráfego cliente (para a rede interna e para a Internet) pelo servidor de Acesso Remoto.

5. Clique em **Avançar**.

## <a name="configure-the-network-topology"></a>Configurar a Topologia de Rede

Para implantar o Acesso Remoto, será necessário configurar o servidor de Acesso Remoto com os adaptadores de rede corretos, uma URL pública para o servidor de Acesso Remoto, à qual os computadores cliente poderão se conectar (o endereço de conexão) e um certificado IP-HTTPS com o assunto correspondente ao endereço de conexão.

1. Na página **Topologia de Rede** , clique na topologia de implantação que será usada na sua organização. Em **Digitar o nome público ou endereço IPv4 usado pelos clientes para se conectar ao servidor de acesso remoto**, digite o nome público da implantação (esse nome corresponde ao nome do assunto do certificado IP-HTTPS, por exemplo, edge1.contoso.com) e clique em **Avançar**.

## <a name="configure-the-dns-suffix-search-list"></a>Configurar a Lista de Pesquisa de Sufixos do DNS

Para clientes DNS, você pode configurar uma lista de pesquisa de sufixo de domínio do DNS que amplia ou revisa a funcionalidade de busca do DNS. Ao acrescentar sufixos adicionais à lista, você poderá pesquisar por nomes de computador curtos e não elegíveis em mais de um domínio de DNS especificado. Em seguida, se uma consulta DNS falhar, o serviço cliente DNS pode usar essa lista para anexar outras terminações de sufixo de nome para o seu nome original e repita a consultas DNS para o servidor DNS para esses FQDNs alternativos.

1. Selecione **Configurar os clientes do DirectAccess com a lista de pesquisa de sufixo do cliente do DNS** para especificar sufixos adicionais para pesquisas de nome de cliente.

2. Digite um novo nome de sufixo na **novo sufixo** e, em seguida, clique em **Add**. Além disso, você pode alterar a ordem de pesquisa e remover sufixos de **sufixos de domínio para usar**.

>[OBSERVAÇÃO] Em um cenário de espaço contíguo \(em que um ou mais computadores de domínio tem um sufixo DNS que não corresponde ao domínio do Active Directory ao qual pertencem os computadores\), você deve garantir que a lista de pesquisa seja personalizada para incluir todos os sufixos necessários. O Assistente de Acesso Remoto configurará o nome do DNS do Active Directory como o sufixo de DNS primário do cliente. O Administrador deverá adicionar o sufixo de DNS usado pelos clientes para resolução de nome.

Para computadores e servidores, o seguinte comportamento de pesquisa DNS padrão é predeterminado e usado ao concluir e resolver nomes curtos não qualificados. Quando a lista de pesquisa de sufixos está vazio ou não especificado, o sufixo DNS primário do computador é anexado a não-qualificados nomes curtos e uma consulta DNS é usada para resolver o FQDN resultante. 

Se essa consulta falhar, o computador pode tentar consultas adicionais para FQDNs alternativos por meio do acréscimo qualquer sufixo DNS específico da conexão configurado para conexões de rede. Se nenhum sufixo específico da conexão estiver configurado ou consultas para esses FQDNs específicos de conexão resultantes falharem, em seguida, o cliente pode, em seguida, começar a tentar novamente as consultas com base na redução sistemática do sufixo primário (também conhecido como devolução).

Por exemplo, se o sufixo primário for "example.microsoft.com", o processo de devolução pode repetir consultas de nome curto, pesquisando nos domínios "microsoft.com" e "com".

Quando o sufixo de pesquisa a lista não está vazia e tem pelo menos um sufixo DNS especificado, as tentativas de qualificar e resolver nomes curtos de DNS está limitada à pesquisa somente daqueles FQDNs possibilitados pela lista de sufixo especificado. 

Se as consultas por todos os FQDNs criados como resultado da agregação e tentativa de cada sufixo na lista não forem resolvidas, o processo de consulta falhará, produzindo um resultado "nome não encontrado". 

>[!WARNING]
>Se a lista de sufixo de domínio for usada, os clientes continuarão a enviar consultas alternativas adicionais com base nos diferentes nomes de domínio do DNS quando uma consulta não é respondida ou resolvida. Depois que o nome é resolvido usando uma entrada da lista de sufixos, as entradas da lista não usadas não serão tentadas. Por esta razão, é mais eficiente ordenar a lista começando pelos sufixos de domínio mais usados.

>Pesquisas pelo sufixo de nome de domínio somente são usadas quando uma entrada de nome de DNS não é totalmente qualificado. Para tornar um nome de DNS totalmente elegível, um ponto à direita (.) deverá ser inserido no fim do nome.

## <a name="gpo-configuration"></a>Configuração do GPO

Quando você configurar o acesso remoto, as configurações do DirectAccess são coletadas em objetos de diretiva de grupo (GPO). 

Na **configurações de GPO**, o nome do GPO do servidor DirectAccess e o nome do GPO do cliente estão listados. Além disso, é possível modificar as configurações de seleção de GPO.

Dois GPOs são populados automaticamente com as configurações do DirectAccess e distribuídos dessa forma:

1. **GPO de cliente do DirectAccess**. Este GPO contém configurações do cliente, inclusive configurações da tecnologia de transição IPv6, entradas da NRPT e regras de segurança de conexão do Firewall do Windows com Segurança Avançada. O GPO é aplicado a grupos de segurança especificados para os computadores cliente.

2. **GPO de servidor do DirectAccess**. Este GPO contém configurações do DirectAccess aplicadas a qualquer servidor configurado como um servidor de acesso remoto em sua implantação. Ele contém também as regras de segurança de conexão do Firewall do Windows com Segurança Avançada.

## <a name="summary"></a>Resumo

Após a conclusão da configuração do acesso remoto a **resumo** é exibida. Você pode alterar as configurações definidas ou clicar **concluir** para aplicar a configuração.
