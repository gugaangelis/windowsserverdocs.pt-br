---
title: Instalar o servidor Web WEB1
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: f51c9e38-98bb-49c1-9d39-427d07021499
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 97ee8c257d9c84b63ee069d87d7bfa6ebf3299e4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949460"
---
# <a name="install-the-web-server-web1"></a>Instalar o servidor Web WEB1

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A função do servidor Web (IIS) no Windows Server 2016 fornece uma plataforma segura, fácil de gerenciar, modular e extensível para hospedagem confiável de sites, serviços e aplicativos. Com o IIS, você pode compartilhar informações com usuários na Internet, em uma intranet ou em uma extranet. O IIS é uma plataforma Web unificada que integra IIS, ASP.NET, serviços FTP, PHP e Windows Communication Foundation (WCF).

Quando você implanta certificados de servidor, seu servidor Web fornece um local onde você pode publicar a CRL (lista de certificados revogados) para sua autoridade de certificação (CA). Após a publicação, a CRL pode ser acessada por todos os computadores em sua rede para que eles possam usar essa lista durante o processo de autenticação para verificar se os certificados apresentados por outros computadores não estão revogados.

Se um certificado estiver na CRL como revogado, o esforço de autenticação falhará e o computador estará protegido da confiança de uma entidade que tem um certificado que não é mais válido.

Antes de instalar a função do servidor Web (IIS), verifique se você configurou o nome do servidor e o endereço IP e se ingressou no computador no domínio.

## <a name="to-install-the-web-server-iis-server-role"></a>Para instalar a função de servidor Servidor Web (IIS)
Para concluir este procedimento, é preciso ser um membro do grupo **Administradores**.

>[!NOTE]
>Para executar esse procedimento usando o Windows PowerShell, abra o PowerShell, digite o comando a seguir e pressione ENTER.
`Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  No Gerenciador do Servidor, clique em **Gerenciar** e depois em **Adicionar Funções e Recursos**. O Assistente para Adicionar Funções e Recursos é aberto.
2.  Em **Antes de Começar**, clique em **Avançar**.

**Observação** A página **antes de começar** do assistente para adicionar funções e recursos não será exibida se você tiver executado anteriormente o assistente para adicionar funções e recursos e tiver selecionado **ignorar esta página por padrão** no momento.

3. Na página **Tipo de Instalação**, clique em **Avançar**.
4. Na página **seleção de servidor** , clique em **Avançar**.
5. Na página **funções de servidor** , selecione **servidor Web (IIS)** e clique em **Avançar**.
6. Clique em **Avançar** até ter aceitado todas as configurações padrão do servidor Web e clique em **Instalar**.
7. Confirme se todas as instalações tiveram êxito e clique em **Fechar**.
