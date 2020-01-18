---
title: O cliente de Área de Trabalho Remota se desconecta e não consegue se reconectar à mesma sessão
description: Solução de um problema no qual o cliente de área de trabalho remota se desconecta e não consegue se reconectar à mesma sessão.
audience: itpro
ms.custom: na
ms.reviewer: rklemen
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.assetid: ''
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 0932bbbb87c6fcae9dc0b871bd605302acdb25cc
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265908"
---
# <a name="remote-desktop-client-disconnects-and-cant-reconnect-to-the-same-session"></a>O cliente de Área de Trabalho Remota se desconecta e não consegue se reconectar à mesma sessão

Após o cliente de Área de Trabalho Remota perder sua conexão com a área de trabalho remota, o cliente não conseguirá se reconectar imediatamente. O usuário recebe uma das seguintes mensagens de erro:

  - O cliente não conseguiu se conectar ao servidor Host da Sessão da Área de Trabalho Remota por causa de um erro de segurança. Verifique se você está conectado à rede e, em seguida, tente se conectar novamente.
  - A Área de Trabalho Remota foi desconectada. Devido a um erro de segurança, o cliente não pôde se conectar ao computador remoto. Verifique se você está conectado à rede e, em seguida, tente se conectar novamente.

Quando o cliente da Área de Trabalho Remota se reconecta, o servidor RDSH reconecta o cliente a uma nova sessão, em vez da sessão original. No entanto, quando você verifica o servidor RDSH, ele informa que a sessão original ainda está ativa e não entrou em um estado desconectado.

Para contornar esse problema, você pode habilitar a política **Configurar intervalo de conexão keep alive** na pasta de política de grupo **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host de Sessão de Área de Trabalho Remota\\Conexões**. Se habilitar essa política, você deverá inserir um intervalo de keep alive. O intervalo de keep alive determina a frequência, em minutos, com que o servidor verifica o estado de sessão.

Esse problema também pode ser corrigido por meio da redefinição de suas definições de autenticação e de configuração. Você pode redefinir essas configurações no nível do servidor ou usando GPOs (objetos de política de grupo). Veja como redefinir suas configurações: Pasta de política de grupo **Configuração do Computador\\Modelos Administrativos\\Componentes do Windows\\Serviços de Área de Trabalho Remota\\Host da Sessão da Área de Trabalho Remota\\Segurança**.

1. No servidor Host da Sessão da Área de Trabalho Remota, abra **Configuração do Host da Sessão da Área de Trabalho Remota**.
2. Em **Conexões**, clique com o botão direito do mouse no nome da conexão e selecione **Propriedades**.
3. Na caixa de diálogo **Propriedades** da conexão, na guia **Geral**, na camada **Segurança**, selecione um método de segurança.
4. Acesse **Nível de criptografia** e selecione o nível desejado. Você pode selecionar **Baixa**, **Compatível com o Cliente**, **Alta** ou **Em Conformidade com FIPS**.

> [!NOTE]  
>  - Quando as comunicações entre clientes e servidores Host da Sessão RD exigem o nível mais alto de criptografia, use a criptografia em conformidade com FIPS.
>  - As configurações de nível de criptografia que você define na Política de Grupo substituem as configurações definidas usando a ferramenta de Configuração de Serviços de Área de Trabalho Remota. Além disso, se você habilitar a política [Criptografia do sistema: Usar algoritmos em conformidade com FIPS para criptografia, hash e assinatura](https://docs.microsoft.com/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing), essa configuração substituirá a política **Definir o nível de criptografia da conexão do cliente**. A política de criptografia do sistema está na pasta **Configuração do computador\\Configurações do Windows\\Configurações de Segurança\\Políticas Locais\\Opções de Segurança**.
>  - Quando você alterar o nível de criptografia, o novo nível de criptografia terá efeito da próxima vez que um usuário se conectar. Se você necessitar de vários níveis de criptografia em um servidor, instale vários adaptadores de rede e configure cada adaptador separadamente.
>  - Para verificar se o certificado tem uma chave privada correspondente, na Configuração de Serviços de Área de Trabalho Remota, clique com o botão direito do mouse na conexão para a qual você deseja exibir o certificado, selecione **Geral** e, em seguida, **Editar**. Depois disso, selecione **Exibir certificado**. Quando você acessar a guia **Geral**, deverá ver a instrução "Você tem uma chave privada que corresponde a este certificado", se houver uma. Você também pode exibir essas informações com o snap-in Certificados.
>  - A criptografia em conformidade com FIPS (a política **Criptografia do sistema: Usar algoritmos em conformidade com FIPS para política de criptografia, hash e assinatura** ou a configuração **Em conformidade com FIPS** na Configuração de Servidor de Área de Trabalho Remota criptografa e descriptografa os dados enviados entre o servidor e o cliente com os algoritmos de criptografia 140-1 padrão FIPS, que usam módulos criptográficos da Microsoft. Para obter mais informações, confira [Validação do FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation).
>  - A configuração **Alta** criptografa os dados enviados entre o servidor e o cliente usando criptografia forte de 128 bits.
>  - A configuração **Compatível com o Cliente** criptografa os dados enviados entre o cliente e servidor com a força de chave máxima compatível com o cliente.
>  - A configuração **Baixa** criptografa os dados enviados do cliente ao servidor usando criptografia de 56 bits.
