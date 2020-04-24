---
title: Adicionar alta disponibilidade ao front-end de Gateway Web e Web da Área de Trabalho Remota
description: Fornece etapas para instalar os servidores Web e Gateway de área de trabalho remota em uma implantação do RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 11/08/2016
manager: dongill
ms.openlocfilehash: e98bbda5460311dd379eab6f5a5bde0ec3845d5c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860279"
---
# <a name="add-high-availability-to-the-rd-web-and-gateway-web-front"></a>Adicionar alta disponibilidade ao front-end de Gateway Web e Web da Área de Trabalho Remota

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016


Você pode implantar um farm de RDWA (Acesso via Web à Área de Trabalho Remota) e de Gateway RD (Gateway de Área de Trabalho Remota) para melhorar a disponibilidade e a escala de uma implantação do RDS (Serviços de Área de Trabalho Remota) do Windows Server 

Use as etapas a seguir para adicionar um servidor Web e Gateway da área de trabalho remota a uma implantação básica de serviços de área de trabalho remota existente.  

## <a name="pre-requisites"></a>Pré-requisitos

Configure um servidor para atuar como um gateway e Web da área de trabalho remota adicional, que pode ser um servidor físico ou VM. Isso inclui ingressar o servidor ao domínio e habilitar o gerenciamento remoto.

## <a name="step-1-configure-the-new-server-to-be-part-of-the-rds-environment"></a>Etapa 1: Configure o novo servidor para ser parte do ambiente de RDS

1. Conecte-se ao servidor RDMS no portal do Azure usando o cliente da Conexão de área de trabalho remota.
2. Adicione o novo servidor Web e Gateway da área de trabalho remota ao Gerenciador do Servidor:
    1. Inicie o Gerenciador do Servidor, clique em **Gerenciar > Adicionar Servidores**.   
    2. Na caixa de diálogo Adicionar Servidores, clique em **Localizar Agora**.   
    3. Selecione o servidor Web da área de trabalho remota e o Gateway recentemente criado (por exemplo, Contoso-WebGw2) e clique em **OK**.
3. Adicionar servidores Web da área de trabalho remota e Gateway à implantação  
    1. Inicie o Gerenciador do Servidor.  
    2. Clique em **Serviços de Área de Trabalho Remota > Visão Geral > Servidores de Implantação > Tarefas > Adicionar Servidores de Acesso via Web da Área de Trabalho Remota**.   
    3. Selecione o servidor recém-criado (por exemplo, Contoso-WebGw2) e, em seguida, clique em **Avançar**.  
    4. Na página de confirmação, selecione **Reiniciar computadores remotos, conforme o necessário** e, em seguida, clique em **Adicionar**.  
    5. Repita essas etapas para adicionar o servidor de Gateway de área de trabalho remota, mas escolha **Servidores de Gateway de área de trabalho remota** na etapa b.
4. Reinstale os certificados dos servidores de Gateway RD:
   1. No Gerenciador do Servidor no servidor RDMS, clique em **Serviços de Área de Trabalho Remota > Visão geral > Tarefas > Editar Propriedades de Implantação**.  
   2. Expanda **Certificados**.  
   3. Role até a tabela. Clique em **Serviço de Função Gateway RD > Selecionar o certificado existente.**  
   4. Clique em **Escolher um certificado diferente** e navegue até o local do certificado. Por exemplo, \Contoso-CB1\Certificates. Selecione o arquivo de certificado para o servidor Web e Gateway de área de trabalho remota criado durante os pré-requisitos (por exemplo, ContosoRdGwCert) e clique em **Abrir**.  
   5. Insira a senha do certificado, selecione **Permitir que o certificado seja adicionado ao repositório de certificados de Autoridades de Certificação Confiáveis nos computadores de destino**e clique em **OK**.  
   6. Clique em **Aplicar**.
      > [!NOTE] 
      > Talvez você precise reiniciar manualmente o serviço TSGateway em execução em cada servidor de Gateway de área de trabalho remota, seja pelo Gerenciador do Servidor ou pelo Gerenciador de Tarefas.
   7. Repita as etapas de a até f para o Serviço de função do Acesso via Web à Área de Trabalho Remota.

## <a name="step-2-configure-rd-web-and-rd-gateway-properties-on-the-new-server"></a>Etapa 2: Configurar as propriedades Web e Gateway de Área de Trabalho Remota no novo servidor
1. Configure o servidor para fazer parte de um farm de servidores de Gateway de área de trabalho remota:
    1.  No Gerenciador do Servidor no servidor RDMS, clique em **Todos os Servidores**. Clique em um dos servidores de Gateway de área de trabalho remota e depois em **Conexão de área de trabalho remota**.
    2.  Entre no servidor do Gateway de área de trabalho remota usando uma conta de administrador de domínio.  
    3.  No Gerenciador do Servidor no servidor de Gateway de área de trabalho remota, clique em **Ferramentas > Serviços de Área de Trabalho Remota > Gerenciador de Gateway de Área de Trabalho Remota**.  
    4.  No painel de navegação, clique no computador local (por exemplo, Contoso-WebGw1).  
    5.  Clique em **Adicionar membros do farm de servidores de Gateway de Área de Trabalho Remota**.  
    6.  Na guia **Farm de servidores**, digite o nome de cada servidor de Gateway de área de trabalho remota e clique em **Adicionar** e **Aplicar**.  
    7.  Repita as etapas de a até f em cada servidor de Gateway RD para que eles reconheçam uns aos outros como servidores de Gateway RD em um farm. Não se assuste se houver avisos, já que pode demorar tempo para as configurações de DNS serem propagadas.
2. Configure o servidor para fazer parte de um farm de Acesso via Web da Área de Trabalho Remota. As etapas abaixo configuram para que as chaves para validação e descriptografia do computador sejam as mesmas em ambos os sites RDWeb.
    1.  No Gerenciador do Servidor no servidor RDMS, clique em **Todos os Servidores**. Clique no primeiro servidor de acesso via Web RD (por exemplo, Contoso-WebGw1) com o botão direito do mouse e, em seguida, clique em **Conexão de área de trabalho remota**.  
    2.  Entre no Servidor de Acesso via Web da Área de Trabalho Remota usando uma conta de administrador de domínio.  
    3.  No Gerenciador do Servidor no Servidor de Acesso via Web da Área de Trabalho Remota, clique em **Ferramentas > Gerenciador do IIS (Serviços de Informações da Internet)** .  
    4.  No painel esquerdo do Gerenciador do IIS, expanda o **Servidor (por exemplo, Contoso-WebGw1) > Sites > Site Padrão** e, em seguida, clique em **RDWeb**.  
    5.  Clique com o botão direito do mouse em **Chave do Computador** e, em seguida, clique em **Abrir Recurso**.
    6.  Na página Chave do Computador, no painel **Ações**, selecione **Gerar chaves** e clique em **Aplicar**.
    7.  Copie a chave de validação (clique com o botão direito do mouse na chave e depois clique em **Copiar**.)
    8.  No Gerenciador do IIS, em **Site Padrão**, selecione **Feed**, **FeedLogon** e **Páginas**.
    9. Para cada parte:
        1.  Clique com o botão direito do mouse em **Chave do Computador** e, em seguida, clique em **Abrir Recurso**.
        2.  Para a chave de validação, desmarque **Gerar automaticamente no runtime** e cole a chave que você copiou na etapa g.
    10.  Minimize a Conexão da área de trabalho remota para esse servidor Web da área de trabalho remota.  
    11.  Repita as etapas de b até e para o segundo Servidor de Acesso via Web da Área de Trabalho Remota, terminando na exibição de recurso da **Chave do computador**.
    12. Para a chave de validação, desmarque **Gerar automaticamente no runtime** e cole a chave que você copiou na etapa g.
    13. Clique em **Aplicar**.
    14. Conclua esse processo para as páginas **RDWeb**, **Feed**, **FeedLogon** e **Pages**.
    15. Minimize a janela Conexão de área de trabalho remota para o segundo Servidor de Acesso via Web da Área de Trabalho Remota e maximize a janela Conexão de área de trabalho remota para o primeiro Servidor de Acesso via Web da Área de Trabalho Remota.  
    16. Repita as etapas de g até n para copiar a Chave de descriptografia.
    17. Quando as chaves de validação e chaves de descriptografia forem idênticas para os dois Servidores de Acesso via Web da Área de Trabalho Remota para as páginas **RDWeb**, **Feed**, **FeedLogon** e **Páginas**, saia de todas as janelas Conexão de área de trabalho remota.

## <a name="step-3-configure-load-balancing-for-the-rd-web-and-rd-gateway-servers"></a>Etapa 3: Configurar o balanceamento de carga para os servidores de Web e Gateway da área de trabalho remota

Se estiver usando a infraestrutura do Azure, você pode criar um balanceador externo de carga do Azure; caso contrário, é possível configurar um balanceador de carga de hardware ou software separado. O balanceamento de carga é a chave para que o tráfego seja uniformemente distribuído pelas conexões de clientes de área de trabalho remota, por meio do Gateway de área de trabalho remota, para os servidores em que os usuários executarão suas cargas de trabalho.

> [!NOTE] 
> Se seu servidor anterior que executa a Web e o Gateway de área de trabalho remota já estiver configurado por trás do balanceador de carga externo, pule para a etapa 4, selecione o pool de back-end existente e adicione o novo servidor ao pool.

1.  Crie um balanceador de carga do Azure:  
    1.  No portal do Azure, clique em **Procurar > Balanceadores de carga > Adicionar**.  
    2.  Insira um nome, por exemplo **WebGwLB**.  
    3.  Selecione **Público** para o **Esquema**.
    4.  Em **Endereço IP público**, selecione **Escolher um endereço IP público** e um endereço IP público existente ou crie um.
    5.  Selecionar as opções apropriadas para **Assinatura**, **Grupo de Recursos** e **Local**.
    6.  Clique em **Criar**.  
2. Crie uma [Investigação](https://azure.microsoft.com/documentation/articles/load-balancer-custom-probe-overview/) para monitorar os servidores que estão ativos:  
    1.  No portal do Azure, selecione **Procurar** > **Balanceadores de Carga** e, em seguida, escolha o balanceador de carga que você criou na etapa anterior.
    2.  Selecione **Todas as configurações** > **Sondas** > **Adicionar**.  
    3.  Digite um nome para a investigação, como **HTTPS**. Selecione **TCP** como o **Protocolo** e insira **443** como a **Porta**, depois clique em **OK**.   
3.  Crie regras de balanceamento de carga para HTTPS e UDP:  
    1.  Nas **Configurações**, clique em **Regras de balanceamento de carga**.  
    2.  Selecione **Adicionar** na **Regra HTTPS**.  
    3.  Digite um nome para a regra, como HTTPS, e selecione **TCP** como o **Protocolo**. Insira **443** tanto na **Porta** como na **Porta de back-end** e clique em **OK**.  
    4.  Nas **Regras de balanceamento de carga**, clique em **Adicionar** na **Regra UDP**.  
    5.  Digite um nome para a regra, como **UDP**, e selecione **UDP** como o **Protocolo**. Insira **3391** tanto na **Porta** como na **Porta de back-end** e clique em **OK**.  
4. Crie o pool de back-end para os servidores Web e Gateway de área de trabalho remota:
      1. Nas **Configurações**, clique em **Pools de endereços de back-end > Adicionar**.   
      2. Insira um nome (por exemplo, **WebGwBackendPool**) e clique em **Adicionar uma máquina virtual**.  
      3. Escolha um conjunto de disponibilidade (por exemplo, WebGwAvSet) e clique em **OK**.   
      4. Clique em **Escolher as máquinas virtuais**, selecione cada máquina virtual e clique em **Selecionar > OK > OK**.
