---
title: Gerenciar certificados usados com NPS
description: Este tópico fornece informações sobre como usar certificados de servidor com o servidor de políticas de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 9e850f7e01d924c8ceb6a8017b3a8c3a48aa8304
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316014"
---
# <a name="manage-certificates-used-with-nps"></a>Gerenciar certificados usados com NPS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Se você implantar um método de autenticação baseado em certificado, como o protocolo de autenticação extensível\-segurança de camada de transporte \(EAP\-TLS\), protocolo de autenticação extensível protegida\-segurança de camada de transporte \(PEAP\-TLS\)e PEAP\-protocolo de autenticação de handshake de desafio da Microsoft versão 2 \(MS\-CHAP\) O certificado do servidor deve:

- Atenda aos requisitos mínimos de certificado do servidor, conforme descrito em [configurar modelos de certificado para os requisitos de PEAP e EAP](nps-manage-cert-requirements.md)

- Ser emitido por uma autoridade de certificação \(\) de AC confiável para computadores cliente. Uma CA é confiável quando seu certificado existe no repositório de certificados de autoridades de certificação raiz confiáveis para o usuário atual e o computador local.

As instruções a seguir auxiliam no gerenciamento de certificados NPS em implantações em que a autoridade de certificação raiz confiável é uma CA de terceiros, como a Verisign, ou é uma AC que você implantou para sua infraestrutura de chave pública \(PKI\) usando Active Directory serviços de certificados \(AD CS\).

## <a name="change-the-cached-tls-handle-expiry"></a>Alterar a expiração do identificador TLS em cache

Durante os processos de autenticação inicial para EAP\-TLS, PEAP\-TLS e PEAP\-MS\-CHAP v2, o NPS armazena em cache uma parte das propriedades de conexão TLS do cliente que está se conectando. O cliente também armazena em cache uma parte das propriedades de conexão TLS do NPS.

Cada coleção individual dessas propriedades de conexão TLS é chamada de identificador TLS.

Os computadores cliente podem armazenar em cache os identificadores TLS para vários autenticadores, enquanto NPSs pode armazenar em cache os identificadores TLS de vários computadores cliente.

Os identificadores TLS em cache no cliente e no servidor permitem que o processo de reautenticação ocorra com mais rapidez. Por exemplo, quando um computador sem fio é reautenticado com um NPS, o NPS pode examinar o identificador TLS do cliente sem fio e pode determinar rapidamente que a conexão do cliente é uma reconexão. O NPS autoriza a conexão sem executar a autenticação completa.

De forma correspondente, o cliente examina o identificador TLS do NPS, determina se ele é uma reconexão e não precisa executar a autenticação do servidor.

Em computadores que executam o Windows 10 e o Windows Server 2016, a expiração do identificador TLS padrão é de 10 horas.

Em algumas circunstâncias, talvez você queira aumentar ou diminuir o tempo de expiração do identificador TLS.

Por exemplo, talvez você queira diminuir o tempo de expiração do identificador TLS em circunstâncias em que o certificado de um usuário é revogado por um administrador e o certificado expirou. Nesse cenário, o usuário ainda poderá se conectar à rede se um NPS tiver um identificador TLS em cache que não tenha expirado. Reduzir a expiração do identificador TLS pode ajudar a impedir que esses usuários com certificados revogados se reconectem.

>[!NOTE]
>A melhor solução para esse cenário é desabilitar a conta de usuário no Active Directory ou remover a conta de usuário do grupo de Active Directory que recebe permissão para se conectar à rede na diretiva de rede. No entanto, a propagação dessas alterações para todos os controladores de domínio também pode ser atrasada, devido à latência de replicação. 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>Configurar o tempo de expiração do identificador TLS em computadores cliente

Você pode usar este procedimento para alterar a quantidade de tempo em que os computadores cliente armazenam em cache o identificador TLS de um NPS. Depois de autenticar com êxito um NPS, os computadores cliente armazenam em cache as propriedades de conexão TLS do NPS como um identificador TLS. O identificador TLS tem uma duração padrão de 10 horas \(36 milhões milissegundos\). Você pode aumentar ou diminuir o tempo de expiração do identificador TLS usando o procedimento a seguir.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

>[!IMPORTANT]
>Esse procedimento deve ser executado em um NPS, não em um computador cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>Para configurar o tempo de expiração do identificador TLS em computadores cliente

1. Em um NPS, abra o editor do registro.

2. Navegue até a chave do registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. No menu **Editar** , clique em **novo**e em **chave**.

4. Digite **ClientCacheTime**e pressione Enter.

5. Clique com o botão direito do mouse em **ClientCacheTime**, clique em **novo**e em **valor DWORD (32 bits)** .

6. Digite a quantidade de tempo, em milissegundos, que você deseja que os computadores cliente armazenem em cache o identificador TLS de um NPS após a primeira tentativa de autenticação bem-sucedida pelo NPS.

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>Configurar o tempo de expiração do identificador TLS em NPSs

Use este procedimento para alterar a quantidade de tempo que o NPSs armazena em cache o identificador TLS de computadores cliente. Depois de autenticar com êxito um cliente de acesso, NPSs cache as propriedades de conexão TLS do computador cliente como um identificador TLS. O identificador TLS tem uma duração padrão de 10 horas \(36 milhões milissegundos\). Você pode aumentar ou diminuir o tempo de expiração do identificador TLS usando o procedimento a seguir.

A associação em **Administradores**, ou equivalente, é o requisito mínimo necessário para concluir este procedimento.

>[!IMPORTANT]
>Esse procedimento deve ser executado em um NPS, não em um computador cliente.

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>Para configurar o tempo de expiração do identificador TLS em NPSs

1. Em um NPS, abra o editor do registro.

2. Navegue até a chave do registro **HKEY\_LOCAL\_MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**

3. No menu **Editar** , clique em **novo**e em **chave**.

4. Digite **ServerCacheTime**e pressione Enter.

5. Clique com o botão direito do mouse em **ServerCacheTime**, clique em **novo**e em **valor DWORD (32 bits)** .

6. Digite o período de tempo, em milissegundos, que você deseja que o NPSs armazene em cache o identificador TLS de um computador cliente após a primeira tentativa de autenticação bem-sucedida pelo cliente.

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Obter o hash SHA-1 de um certificado de autoridade de certificação raiz confiável

Use este procedimento para obter o hash de SHA-1 (algoritmo de hash seguro) de uma AC (autoridade de certificação) raiz confiável de um certificado instalado no computador local. Em algumas circunstâncias, como ao implantar Política de Grupo, é necessário designar um certificado usando o hash SHA-1 do certificado.

Ao usar Política de Grupo, você pode designar um ou mais certificados de autoridade de certificação raiz confiáveis que os clientes devem usar para autenticar o NPS durante o processo de autenticação mútua com EAP ou PEAP. Para designar um certificado de AC raiz confiável que os clientes devem usar para validar o certificado do servidor, você pode inserir o hash SHA-1 do certificado.

Este procedimento demonstra como obter o hash SHA-1 de um certificado de AC raiz confiável usando o snap-in de certificados do MMC (console de gerenciamento Microsoft). 

Para concluir este procedimento, você deve ser um membro do grupo **usuários** no computador local.

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>Para obter o hash SHA-1 de um certificado de autoridade de certificação raiz confiável

1. Na caixa de diálogo Executar ou no Windows PowerShell, digite **MMC**e pressione Enter. O console de gerenciamento Microsoft \(\) do MMC é aberto. No MMC, clique em **arquivo**e, em seguida, clique em **Adicionar/remover Snap\in**. A caixa de diálogo **Adicionar ou Remover Snap-ins** é aberta.

2. Em **Adicionar ou Remover Snap-ins**, em **Snap-ins disponíveis**, clique duas vezes em **Certificados**. O assistente de snap-in de certificados é aberto. Clique em **Conta de computador** e em **Avançar**.

3. Em **Selecionar computador**, verifique se **o computador local (o computador no qual este console está sendo executado)** está selecionado, clique em **concluir**e em **OK**.

4. No painel esquerdo, clique duas vezes em **certificados (computador local)** e, em seguida, clique duas vezes na pasta **autoridades de certificação raiz confiáveis** .

5. A pasta **certificados** é uma subpasta da pasta **autoridades de certificação raiz confiáveis** . Clique na pasta **Certificados**.

6. No painel de detalhes, navegue até o certificado de sua AC raiz confiável. Clique duas vezes no certificado. A caixa de diálogo **Certificado** é aberta.

7. Na caixa de diálogo **Certificado**, clique na guia **Detalhes**.

8. Na lista de campos, role para e selecione **impressão digital**.

9. No painel inferior, a cadeia de caracteres hexadecimal que é o hash SHA-1 do seu certificado é exibida. Selecione o hash SHA-1 e, em seguida, pressione o atalho de teclado do Windows para o comando de cópia \(CTRL\+C\) para copiar o hash para a área de transferência do Windows.

10. Abra o local no qual você deseja colar o hash SHA-1, localize corretamente o cursor e, em seguida, pressione o atalho de teclado do Windows para o comando colar \(CTRL\+V\). 

Para obter mais informações sobre certificados e NPS, consulte [configurar modelos de certificado para requisitos de PEAP e EAP](nps-manage-cert-requirements.md).

Para obter mais informações sobre o NPS, consulte [servidor de diretivas de rede (NPS)](nps-top.md).
