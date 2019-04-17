---
title: Gerenciar certificados usados com NPS
description: Este tópico fornece informações sobre o uso de certificados do servidor com o servidor de política de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2044cf30cc90c1673e05a1948ac9196d05940d1f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-certificates-used-with-nps"></a>Gerenciar certificados usados com NPS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Se você implantar um método de autenticação baseada em certificado, como Extensible Authentication Protocol\-Transport Layer Security \(EAP\-TLS\), protegido Extensible Authentication Protocol\ Transport Layer Security \(PEAP\-TLS\) e PEAP\ Microsoft Challenge Handshake Authentication Protocol versão 2 \ (CHAP MS\ v2\), você deve registrar um certificado de servidor de todos os servidores NPS. O certificado do servidor deve:

- Atender aos requisitos de certificado do servidor mínimo, conforme descrito em [configurar modelos de certificado para PEAP e EAP requisitos](nps-manage-cert-requirements.md)

- Ser emitido por uma autoridade de certificação \(CA\) que é confiável por computadores cliente. Uma autoridade de certificação é confiável quando seu certificado existe no repositório de certificados autoridades de certificação raiz confiáveis para o usuário atual e o computador local.

As seguintes instruções ajudar no gerenciamento de certificados do servidor NPS em implantações em que a autoridade de certificação raiz confiável é uma autoridade de certificação de terceiros, como Verisign, ou é uma autoridade de certificação que você tenha implantado para sua infraestrutura de chave pública \(PKI\) usando \(AD CS\) serviços de certificados do Active Directory.

## <a name="change-the-cached-tls-handle-expiry"></a>Alterar a expiração de identificador TLS armazenadas em cache

Durante os processos de autenticação inicial de EAP\ TLS, PEAP\-TLS e PEAP\-MS\-CHAP v2, o servidor NPS armazena em cache uma parte das propriedades de conexão TLS do cliente conectado. O cliente também armazena em cache uma parte das propriedades de conexão TLS do servidor NPS.

Cada coleção individual dessas propriedades de conexão TLS é chamada um identificador TLS.

Computadores cliente podem armazenar em cache as alças TLS para vários autenticadores, enquanto os servidores NPS podem armazenar em cache as alças TLS de muitos computadores cliente.

As alças TLS armazenadas em cache no cliente e servidor permitem que o processo de uma nova autenticação para ocorrer com mais rapidez. Por exemplo, quando um computador sem fio torna a autenticar com um servidor NPS, o servidor NPS pode examinar o identificador TLS para o cliente sem fio e pode determinar rapidamente que a conexão do cliente é uma reconexão. O servidor NPS autoriza a conexão sem executar autenticação completa.

De forma correspondente, o cliente examina o identificador TLS do servidor NPS, determina que ele é uma reconexão e não precisa executar a autenticação de servidor.

Em computadores que executam o Windows 10 e Windows Server 2016, a expiração de identificador TLS padrão é 10 horas.

Em algumas circunstâncias, convém aumentar ou diminuir o tempo de expiração de identificador TLS.

Por exemplo, convém reduzir o tempo de expiração de identificador TLS em circunstâncias em que o certificado do usuário é revogado pelo administrador e o certificado expirou. Nesse cenário, o usuário ainda pode conectar à rede se um servidor NPS tem um identificador TLS em cache que não expirou. Reduzindo os TLS expiração de identificador pode ajudar a impedir que esses usuários com certificados revogados reconectar.

>[!NOTE]
>A melhor solução para esse cenário é desabilitar a conta de usuário no Active Directory ou remover a conta de usuário do grupo do Active Directory que tem permissão para se conectar à rede na política de rede. A propagação dessas alterações para todos os controladores de domínio também pode ser atrasada, no entanto, devido à latência da replicação. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurar o horário de expiração de identificador TLS em computadores cliente

Você pode usar este procedimento para alterar a quantidade de tempo que os computadores cliente armazenar em cache o identificador TLS de um servidor NPS. Depois de autenticar com êxito um servidor NPS, computadores cliente cache propriedades de conexão TLS do servidor NPS como um identificador TLS. O identificador TLS tem uma duração padrão de \(36,000,000 milliseconds\) 10 horas. Você pode aumentar ou diminuir o tempo de expiração de identificador TLS usando o procedimento a seguir.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.

>[!IMPORTANT]
>Esse procedimento deve ser executado em um servidor NPS, não em um computador cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Para configurar os TLS manipular o horário de expiração em computadores cliente

1. Em um servidor NPS, abra o Editor do registro.

2. Navegue até a chave do registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Sobre o **editar** menu, clique em **nova**e clique em **chave**.

4. Tipo **ClientCacheTime**, e pressione ENTER.

5. Clique com botão direito **ClientCacheTime**, clique em **nova**e clique em **valor DWORD (32 bits)**.

6. Digite a quantidade de tempo, em milissegundos, que você deseja que os computadores cliente para armazenar em cache o identificador TLS de um servidor NPS após a primeira autenticação bem-sucedida tentativa pelo servidor NPS.

## <a name="configure-the-tls-handle-expiry-time-on-nps-servers"></a>Configurar o horário de expiração de identificador TLS em servidores NPS

Use este procedimento para alterar a quantidade de tempo que os servidores NPS armazenar em cache o identificador TLS de computadores cliente. Depois de autenticar com êxito um cliente de acesso, servidores NPS cache propriedades de conexão TLS do computador cliente como um identificador TLS. O identificador TLS tem uma duração padrão de \(36,000,000 milliseconds\) 10 horas. Você pode aumentar ou diminuir o tempo de expiração de identificador TLS usando o procedimento a seguir.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para concluir este procedimento.

>[!IMPORTANT]
>Esse procedimento deve ser executado em um servidor NPS, não em um computador cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-nps-servers"></a>Para configurar os TLS manipular o horário de expiração em servidores NPS

1. Em um servidor NPS, abra o Editor do registro.

2. Navegue até a chave do registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Sobre o **editar** menu, clique em **nova**e clique em **chave**.

4. Tipo **ServerCacheTime**, e pressione ENTER.

5. Clique com botão direito **ServerCacheTime**, clique em **nova**e clique em **valor DWORD (32 bits)**.

6. Digite a quantidade de tempo, em milissegundos, que você deseja que os servidores NPS para armazenar em cache o identificador TLS de um computador cliente após a primeira autenticação bem-sucedida tentativa pelo cliente.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obter o valor de Hash SHA-1 de um certificado de autoridade de certificação raiz confiável

Use este procedimento para obter o algoritmo de Hash seguro (SHA-1) hash de uma autoridade de certificação (CA) raiz confiável de um certificado é instalado no computador local. Em algumas circunstâncias, como durante a implantação de política de grupo, é necessário designar um certificado usando o hash SHA-1 do certificado.

Ao usar a política de grupo, você pode designar um ou mais certificados de autoridade de certificação raiz confiável que os clientes devem usar para autenticar o servidor NPS durante o processo de autenticação mútua com EAP ou PEAP. Para designar um certificado de autoridade de certificação raiz confiável que os clientes devem usar para validar o certificado do servidor, você pode inserir o hash SHA-1 do certificado.

Este procedimento demonstra como obter o SHA-1 hash de um certificado de autoridade de certificação raiz confiável usando o snap-in do Console de gerenciamento Microsoft (MMC) de certificados. 

Para concluir este procedimento, você deve ser um membro do **usuários** grupo no computador local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Para obter o SHA-1 hash de um certificado de autoridade de certificação raiz confiável

1. Na caixa de diálogo Executar ou do Windows PowerShell, digite **mmc**, e pressione ENTER. O Console de gerenciamento Microsoft \(MMC\) é aberta. No MMC, clique em **arquivo**, clique em **Adicionar/remover Snap\in**. O **adicionar ou Remover Snap-ins** caixa de diálogo é aberta.

2. Em **adicionar ou Remover Snap-ins**, na **snap-ins disponíveis**, clique duas vezes em **certificados**. Abre o Assistente de snap-in de certificados. Clique em **conta de computador**e clique em **próxima**.

3. Em **Selecionar computador**, certifique-se de que **computador Local (o computador este console está sendo executado)** é selecionado, clique em **concluir**e clique em **Okey**.

4. No painel esquerdo, clique duas vezes em **certificados (computador Local)**e clique duas vezes o **autoridades de certificação confiáveis** pasta.

5. O **certificados** pasta é uma subpasta do **autoridades de certificação confiáveis** pasta. Clique no **certificados** pasta.

6. No painel de detalhes, navegue até o certificado para sua autoridade de certificação raiz confiável. Clique duas vezes o certificado. O **certificado** caixa de diálogo é aberta.

7. No **certificado** caixa de diálogo, clique no **detalhes** guia.

8. Na lista de campos, role e selecione **impressão digital**.

9. No painel inferior, a cadeia de caracteres hexadecimal é o hash SHA-1 de seu certificado é exibida. Selecione SHA-1 hash e pressione o atalho de teclado do Windows para a cópia de comando \(CTRL\+C\) para copiar o hash para a área de transferência do Windows.

10. Abra o local à qual você deseja colar o hash SHA-1, localizar corretamente o cursor e pressione o atalho de teclado do Windows para a operação de colagem \(CTRL\+V\) de comando. 

Para saber mais sobre certificados e NPS, consulte [configurar modelos de certificado para PEAP e EAP requisitos](nps-manage-cert-requirements.md).

Para obter mais informações sobre o NPS, consulte [servidor de política de rede (NPS)](nps-top.md).
