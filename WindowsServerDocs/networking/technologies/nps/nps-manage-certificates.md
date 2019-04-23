---
title: Gerenciar certificados usados com NPS
description: Este tópico fornece informações sobre como usar certificados de servidor com o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73f3d6a1e9dc6ae1520b1d685b6b05b5f3aed601
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864227"
---
# <a name="manage-certificates-used-with-nps"></a>Gerenciar certificados usados com NPS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Se você implantar um método de autenticação baseada em certificado, como Extensible Authentication Protocol\-Transport Layer Security \(EAP\-TLS\), Protected Extensible Authentication Protocol\-Transport Layer Security \(PEAP\-TLS\)e o PEAP\-Microsoft Challenge Handshake Authentication Protocol versão 2 \(MS\-CHAP v2\), você deve registrar um certificado de servidor a todos os seus NPSs. O certificado do servidor deve:

- Atender os requisitos mínimos do servidor de certificado conforme descrito em [configurar modelos de certificado para PEAP e EAP requisitos](nps-manage-cert-requirements.md)

- Ser emitido por uma autoridade de certificação \(autoridade de certificação\) que é confiável para computadores cliente. Uma autoridade de certificação é confiável quando seu certificado não existe no repositório de certificados de autoridades de certificação raiz confiável para o usuário atual e o computador local.

As instruções a seguir auxiliam no gerenciamento de certificados do NPS em implantações em que a AC raiz confiável é uma autoridade de certificação de terceiros, como a Verisign, ou é uma autoridade de certificação que você tenha implantado para sua infraestrutura de chave pública \(PKI\) usando o Active Directory Serviços de certificados do Active \(AD CS\).

## <a name="change-the-cached-tls-handle-expiry"></a>Alterar a expiração do identificador de TLS em cache

Durante os processos de autenticação inicial para o EAP\-TLS, PEAP\-TLS e PEAP\-MS\-CHAP v2, o NPS armazena em cache uma parte das propriedades de conexão TLS do cliente está se conectando. O cliente também armazena em cache uma parte das propriedades de conexão TLS do NPS.

Cada coleção individual dessas propriedades de conexão TLS é chamada de um identificador TLS.

Computadores cliente podem armazenar em cache os identificadores TLS para vários autenticadores, enquanto NPSs pode armazenar em cache os identificadores de TLS de vários computadores cliente.

Os identificadores TLS em cache no cliente e servidor permitem que o processo de reautenticação ocorra mais rapidamente. Por exemplo, quando um computador sem fio faz a autenticação novamente com um NPS, o NPS pode examinar o identificador TLS para o cliente sem fio e podem determinar rapidamente a conexão de cliente é uma reconexão. O NPS autoriza a conexão sem executar a autenticação completa.

Do mesmo modo, o cliente examina a alça TLS para o NPS, determina que ele é uma reconexão e não precisa realizar a autenticação de servidor.

Em computadores que executam o Windows 10 e Windows Server 2016, a expiração do identificador TLS padrão é de 10 horas.

Em algumas circunstâncias, convém aumentar ou diminuir o tempo de expiração do identificador TLS.

Por exemplo, você talvez queira diminuir o tempo de expiração do identificador TLS em circunstâncias em que o certificado de usuário é revogado por um administrador e o certificado expirou. Nesse cenário, o usuário pode ainda se conectar à rede se um NPS tem um identificador TLS em cache que não tenha expirado. Reduzindo os TLS identificador expiração pode ajudar a impedir que esses usuários com certificados revogados reconectar-se.

>[!NOTE]
>A melhor solução para esse cenário é para desabilitar a conta de usuário no Active Directory ou para remover a conta de usuário do grupo do Active Directory que tem permissão para se conectar à rede na diretiva de rede. A propagação dessas alterações para todos os controladores de domínio também pode ser atrasada, no entanto, devido à latência de replicação. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurar o tempo de expiração do identificador TLS em computadores cliente

Você pode usar este procedimento para alterar a quantidade de tempo que os computadores cliente armazenam em cache o identificador TLS de um NPS. Depois de autenticar com êxito um NPS, computadores cliente armazenar em cache as propriedades de conexão TLS do NPS como um identificador TLS. O identificador TLS tem uma duração padrão de 10 horas \(36,000,000 milissegundos\). Você pode aumentar ou diminuir o tempo de expiração do identificador TLS usando o procedimento a seguir.

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

>[!IMPORTANT]
>Esse procedimento deve ser executado em um NPS, não em um computador cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Para configurar TLS lidar com a hora de expiração em computadores cliente

1. Em um NPS, abra o Editor do registro.

2. Navegue até a chave do registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Sobre o **editar** menu, clique em **New**e, em seguida, clique em **chave**.

4. Tipo de **ClientCacheTime**, e pressione ENTER.

5. Clique com botão direito **ClientCacheTime**, clique em **New**e, em seguida, clique em **valor DWORD (32 bits)**.

6. Digite a quantidade de tempo, em milissegundos, que você deseja que computadores cliente para armazenar em cache o identificador TLS de um NPS depois que a primeira autenticação bem-sucedida tente, o NPS.

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>Configurar o tempo de expiração do identificador TLS em NPSs

Use este procedimento para alterar a quantidade de tempo que NPSs armazenar em cache o identificador TLS de computadores cliente. Depois de autenticar com êxito um cliente de acesso, NPSs armazenar em cache as propriedades de conexão TLS do computador cliente como um identificador TLS. O identificador TLS tem uma duração padrão de 10 horas \(36,000,000 milissegundos\). Você pode aumentar ou diminuir o tempo de expiração do identificador TLS usando o procedimento a seguir.

Associação na **administradores**, ou equivalente, é o mínimo necessário para concluir este procedimento.

>[!IMPORTANT]
>Esse procedimento deve ser executado em um NPS, não em um computador cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>Para configurar TLS lidar com a hora de expiração em NPSs

1. Em um NPS, abra o Editor do registro.

2. Navegue até a chave do registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. Sobre o **editar** menu, clique em **New**e, em seguida, clique em **chave**.

4. Tipo de **ServerCacheTime**, e pressione ENTER.

5. Clique com botão direito **ServerCacheTime**, clique em **New**e, em seguida, clique em **valor DWORD (32 bits)**.

6. Digite a quantidade de tempo, em milissegundos, que você deseja NPSs para armazenar em cache o identificador TLS de um computador cliente após a autenticação bem-sucedida de primeira tentativa pelo cliente.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obter o valor de Hash SHA-1 de um certificado de autoridade de certificação raiz confiável

Use este procedimento para obter o algoritmo de Hash seguro (SHA-1) hash de uma autoridade de certificação (CA) raiz confiável de um certificado que está instalado no computador local. Em algumas circunstâncias, como ao implantar a política de grupo, é necessário designar um certificado usando o hash SHA-1 do certificado.

Ao usar a diretiva de grupo, você pode designar um ou mais certificados de autoridade de certificação raiz confiáveis que os clientes devem usar para autenticar o NPS durante o processo de autenticação mútua com EAP ou PEAP. Para designar um certificado de autoridade de certificação raiz confiáveis que os clientes devem usar para validar o certificado do servidor, você pode inserir o hash SHA-1 do certificado.

Este procedimento demonstra como obter o SHA-1 hash de um certificado de autoridade de certificação raiz confiável usando o snap-in de certificados Microsoft Management Console (MMC). 

Para concluir este procedimento, você deve ser um membro do **usuários** grupo no computador local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Para obter o SHA-1 hash de um certificado de AC raiz confiável

1. Na caixa de diálogo Executar ou do Windows PowerShell, digite **mmc**, e pressione ENTER. O Console de gerenciamento Microsoft \(MMC\) é aberta. No MMC, clique em **arquivo**, em seguida, clique em **Snap\in Adicionar/remover**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.

2. Em **Adicionar ou Remover Snap-ins**, em **Snap-ins disponíveis**, clique duas vezes em **Certificados**. O Assistente de snap-in de certificados é aberto. Clique em **Conta de computador** e em **Avançar**.

3. Na **Selecionar computador**, certifique-se de que **computador Local (o computador onde este console está sendo executado)** está selecionado, clique em **concluir**e, em seguida, clique em **Okey**.

4. No painel esquerdo, clique duas vezes **certificados (computador Local)** e, em seguida, clique duas vezes o **autoridades de certificação raiz confiáveis** pasta.

5. O **certificados** pasta é uma subpasta das **autoridades de certificação raiz confiáveis** pasta. Clique na pasta **Certificados**.

6. No painel de detalhes, procure o certificado para sua autoridade de certificação de raiz confiável. Clique duas vezes no certificado. A caixa de diálogo **Certificado** é aberta.

7. Na caixa de diálogo **Certificado**, clique na guia **Detalhes**.

8. Na lista de campos, role e selecione **impressão digital**.

9. No painel inferior, a cadeia de caracteres hexadecimal que é o hash SHA-1 do seu certificado é exibida. Selecione o hash SHA-1 e, em seguida, pressione o atalho de teclado do Windows para o comando de cópia \(CTRL\+C\) para copiar o hash para a área de transferência do Windows.

10. Abra o local para o qual você deseja colar o hash SHA-1, localizar corretamente o cursor e, em seguida, pressione o atalho de teclado do Windows para o comando Colar \(CTRL\+V\). 

Para obter mais informações sobre certificados e NPS, consulte [configurar modelos de certificado para PEAP e EAP requisitos](nps-manage-cert-requirements.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
