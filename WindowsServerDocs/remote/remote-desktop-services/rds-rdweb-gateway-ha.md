---
title: Adicionar a alta disponibilidade para a frente da web via Web da área de trabalho remota e Gateway
description: Fornece etapas para instalar os servidores Web de área de trabalho remota e Gateway em uma implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: 4e185e51b09d2e2f8ac4527f9de339de27e02f24
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805143"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>Adicionar a alta disponibilidade para a frente da web via Web da área de trabalho remota e Gateway

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016


Você pode implantar um acesso de Web de área de trabalho remota (RD Web Access) e farm de Gateway de área de trabalho remota (Gateway RD) para melhorar a disponibilidade e escala de uma implantação de serviços de área de trabalho remota (RDS) do Windows Server 

Use as etapas a seguir para adicionar um servidor Web da área de trabalho remota e Gateway a uma implantação básica de serviços de área de trabalho remota existente.  

## <a name="pre-requisites"></a>Pré-requisitos

Configurar um servidor para atuar como um adicional da Web da área de trabalho remota e Gateway de área de trabalho remota - isso pode ser um servidor físico ou VM. Isso inclui ingressar o servidor ao domínio e habilitar o gerenciamento remoto.

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>Etapa 1: Configure o novo servidor para ser parte do ambiente do RDS

1. Conecte-se ao servidor RDMS no portal do Azure, usando o cliente de Conexão de área de trabalho remota.
2. Adicione o novo servidor Web da área de trabalho remota e Gateway ao Gerenciador do servidor:
    1. Inicie o Gerenciador do servidor, clique em **Gerenciar > Adicionar servidores**.   
    2. Na caixa de diálogo Adicionar servidores, clique em **Localizar agora**.   
    3. Selecione o servidor criado recentemente via Web da área de trabalho remota e Gateway (por exemplo, Contoso-WebGw2) e clique em **Okey**.
3. Adicionar servidores Web de área de trabalho remota e Gateway à implantação  
    1. Inicie o Gerenciador do servidor.  
    2. Clique em **dos serviços de área de trabalho remota > Visão geral > servidores de implantação > tarefas > Adicionar servidores de acesso Web à área de trabalho remota**.   
    3. Selecione o servidor criado recentemente (por exemplo, Contoso-WebGw2) e, em seguida, clique em **próxima**.  
    4. Na página de confirmação, selecione **reiniciar computadores remotos, conforme a necessidade**e, em seguida, clique em **Add**.  
    5. Repita essas etapas para adicionar o servidor de Gateway de área de trabalho remota, mas escolha **servidores de Gateway de área de trabalho remota** na etapa b.
4. Reinstale os certificados para os servidores de Gateway de área de trabalho remota:
   1. No Gerenciador do servidor no servidor RDMS, clique em **Remote Desktop Services > Visão geral > tarefas > Editar propriedades de implantação**.  
   2. Expandir **certificados**.  
   3. Role até a tabela. Clique em área de trabalho remota **serviço de função Gateway > selecione o certificado existente.**  
   4. Clique em **escolher um certificado diferente** e, em seguida, navegue até o local do certificado. Por exemplo, \Contoso-CB1\Certificates). Selecione o arquivo de certificado para o servidor Web da área de trabalho remota e Gateway criado durante os pré-requisitos (por exemplo, ContosoRdGwCert) e, em seguida, clique em **aberto**.  
   5. Insira a senha do certificado, selecione **permitir que o certificado a ser adicionado ao repositório de certificados de autoridades de certificação raiz confiáveis nos computadores de destino**e, em seguida, clique em **Okey**.  
   6. Clique em **Aplicar**.
      > [!NOTE] 
      > Talvez você precise reiniciar manualmente o serviço de Gateway TS está em execução em cada servidor de Gateway de área de trabalho remota, por meio do Gerenciador do servidor ou o Gerenciador de tarefas.
   7. Repita as etapas da-f para o serviço de função de acesso de Web de área de trabalho remota.

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>Etapa 2: Configurar propriedades da Web da área de trabalho remota e Gateway de área de trabalho remota no novo servidor
1. Configure o servidor para fazer parte de um farm de servidores de Gateway de área de trabalho remota:
    1.  No Gerenciador do servidor no servidor RDMS, clique em **todos os servidores**. Clique em um dos servidores de Gateway de área de trabalho remota e, em seguida, clique em **Conexão de área de trabalho remota**.
    2.  Entrar para o servidor de Gateway de área de trabalho remota usando uma conta de administrador de domínio.  
    3.  No Gerenciador do servidor no servidor de Gateway de área de trabalho remota, clique em **Ferramentas > Remote Desktop Services > Gerenciador de Gateway de área de trabalho remota**.  
    4.  No painel de navegação, clique no computador local (por exemplo, Contoso-WebGw1).  
    5.  Clique em **membros do Farm de servidores de Gateway de área de trabalho remota adicionar**.  
    6.  Sobre o **Farm de servidores** guia, digite o nome de cada servidor de Gateway de área de trabalho remota e clique em **Add** e **aplicar**.  
    7.  Repita as etapas de a até f em cada servidor de Gateway de área de trabalho remota para que eles reconheçam uns aos outros, como servidores de Gateway de área de trabalho remota em um farm. Não se assuste se há avisos, como ele pode levar tempo para as configurações de DNS serem propagadas.
2. Configure o servidor para fazer parte de um farm de acesso via Web RD. As etapas abaixo configuram a validação e descriptografia de chaves do computador para ser o mesmo em ambos os sites RDWeb.
    1.  No Gerenciador do servidor no servidor RDMS, clique em **todos os servidores**. O primeiro servidor de acesso via Web RD (por exemplo, Contoso-WebGw1) com o botão direito e, em seguida, clique em **Conexão de área de trabalho remota**.  
    2.  Entrar no servidor de acesso via Web RD usando uma conta de administrador de domínio.  
    3.  No Gerenciador do servidor no servidor de acesso via Web RD, clique em **Ferramentas > Gerenciador de serviços de informações da Internet (IIS)** .  
    4.  No painel esquerdo do Gerenciador do IIS, expanda o **servidor (por exemplo, Contoso-WebGw1) > Sites > Site padrão**e, em seguida, clique em **RDWeb**.  
    5.  Clique com botão direito **chave do computador**e, em seguida, clique em **abrir recurso**.
    6.  Na página chave do computador, nos **ações** painel, selecione **gerar chaves**e, em seguida, clique em **aplicar**.
    7.  Copie a chave de validação (você pode a chave com o botão direito e, em seguida, clique em **cópia**.)
    8.  No Gerenciador do IIS, sob **Site padrão**, selecione **Feed**, **FeedLogon** e **páginas** por vez.
    9. Para cada um:
        1.  Clique com botão direito **chave do computador**e, em seguida, clique em **abrir recurso**.
        2.  Para a chave de validação, desmarque **gerar automaticamente em tempo de execução**e, em seguida, cole a chave que você copiou na etapa g.
    10.  Minimize a janela de Conexão de área de trabalho remota para esse servidor Web da área de trabalho remota.  
    11.  Repita as etapas de b até "e" para o segundo servidor de acesso via Web RD, terminando na exibição de recurso do **chave do computador**.
    12. Para a chave de validação, desmarque **gerar automaticamente em tempo de execução**e, em seguida, cole a chave que você copiou na etapa g.
    13. Clique em **Aplicar**.
    14. Concluir esse processo para o **RDWeb**, **Feed**, **FeedLogon** e **páginas** páginas.
    15. Minimizar a janela de Conexão de área de trabalho remota para o segundo servidor de acesso via Web RD e, em seguida, maximize a janela de Conexão de área de trabalho remota para o primeiro servidor de acesso via Web RD.  
    16. Repita as etapas g por meio de n para copiar a chave de descriptografia.
    17. Quando as chaves de validação e chaves de descriptografia são idênticas em ambos os servidores de acesso via Web RD para o **RDWeb**, **Feed**, **FeedLogon** e **páginas**páginas, saia de todas as janelas de Conexão de área de trabalho remota.

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>Etapa 3: Configurar o balanceamento de carga para os servidores Web de área de trabalho remota e Gateway de área de trabalho remota

Se você estiver usando a infraestrutura do Azure, você pode criar um balanceador externo de carga do Azure; Caso contrário, você pode configurar um balanceador de carga de hardware ou software separado. Balanceamento de carga é a chave para que o tráfego seja uniformemente distribuído as conexões de clientes de área de trabalho remota, por meio do Gateway de área de trabalho remota, para os servidores que os usuários serão executados suas cargas de trabalho de longa duração.

> [!NOTE] 
> Se seu servidor anterior executando Web da área de trabalho remota e Gateway de área de trabalho remota já foi configurada por trás do balanceador de carga externo, pule para a etapa 4, selecione o pool de back-end existente e adicione o novo servidor ao pool.

1.  Crie um balanceador de carga do Azure:  
    1.  No portal do Azure clique **procurar > balanceadores de carga > Adicionar**.  
    2.  Insira um nome, por exemplo **WebGwLB**.  
    3.  Selecione **pública** para o **esquema**, **endereço IP público**e um **endereço IP público**. Você pode selecionar um endereço IP público existente ou criar um novo. 
    4.  Selecionar as devidas **assinatura**, **grupo de recursos**, e **local**.
    5.  Clique em **Criar**.  
2. Criar uma [investigação](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) para monitorar os servidores que estão ativos:  
    1.  No portal do Azure clique **procurar > balanceadores de carga**., o balanceador de carga recém-criado, por exemplo, WebGwLB e configurações  
    2.  Clique em **investigações > Adicionar**.  
    3.  Insira um nome, por exemplo, **HTTPS**, para a investigação. Selecione **TCP** como o **protocolo**e insira **443** para o **porta**, em seguida, clique em **Okey**.   
3.  Crie regras de balanceamento de carga HTTPS e UDP:  
    1.  Na **as configurações**, clique em **regras de balanceamento de carga**.  
    2.  Selecione **Add** para o **regra HTTPS**.  
    3.  Insira um nome para a regra, por exemplo, HTTPS e selecione **TCP** para o **protocolo**. Insira **443** para ambos **porta** e **porta de back-end**e clique em **Okey**.  
    4.  Na **regras de balanceamento de carga**, clique em **Add** para o **regra UDP**.  
    5.  Insira um nome para a regra, por exemplo, **UDP**e selecione **UDP** para o **protocolo**. Insira **3391** para ambos **porta** e **porta de back-end**e clique em **Okey**.  
4. Crie o pool de back-end para os servidores Web da área de trabalho remota e Gateway de área de trabalho remota:
      1. Na **as configurações**, clique em **pools de endereços de back-end > Adicionar**.   
      2. Insira um nome (por exemplo, **WebGwBackendPool**), em seguida, clique em **adicionar uma máquina virtual**.  
      3. Escolha um conjunto de disponibilidade (por exemplo, WebGwAvSet) e, em seguida, clique em **Okey**.   
      4. Clique em **escolher as máquinas virtuais**, selecione cada máquina virtual e, em seguida, clique em **Selecionar > Okey > Okey**.
