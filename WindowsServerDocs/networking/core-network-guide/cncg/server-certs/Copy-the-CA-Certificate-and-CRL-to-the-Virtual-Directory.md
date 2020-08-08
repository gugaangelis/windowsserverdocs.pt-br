---
title: Copiar o certificado de autoridade de certificação e a CRL para o diretório virtual
description: Este tópico faz parte do guia implantar certificados de servidor para implantações com e sem fio 802.1 X
manager: dougkim
ms.topic: article
ms.assetid: a1b5fa23-9cb1-4c32-916f-2d75f48b42c7
ms.author: lizross
author: eross-msft
ms.date: 07/19/2018
ms.openlocfilehash: cea33d8328a2233503fda61b4261213bedce0f3a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949560"
---
# <a name="copy-the-ca-certificate-and-crl-to-the-virtual-directory"></a>Copiar o certificado de autoridade de certificação e a CRL para o diretório virtual

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para copiar a lista de certificados revogados e o certificado de autoridade de certificação raiz corporativa de sua autoridade de certificado para um diretório virtual no servidor Web e para garantir que o AD CS esteja configurado corretamente. Antes de executar os comandos a seguir, certifique-se de substituir os nomes de diretório e servidor pelos que são apropriados para sua implantação.

Para executar este procedimento, você deve ser membro de **Admins**. do domínio.

#### <a name="to-copy-the-certificate-revocation-list-from-ca1-to-web1"></a>Para copiar a lista de certificados revogados de CA1 para WEB1

1.  No CA1, execute o Windows PowerShell como administrador e, em seguida, publique a CRL com o seguinte comando:

    - Digite `certutil -crl` e pressione ENTER.

    - Para copiar o certificado CA1 para o compartilhamento de arquivos no seu servidor Web, digite `copy C:\Windows\system32\certsrv\certenroll\*.crt \\WEB1\pki` e pressione Enter.

    - Para copiar as listas de certificados revogados para o compartilhamento de arquivos no servidor Web, digite `copy C:\Windows\system32\certsrv\certenroll\*.crl \\WEB1\pki` e pressione Enter.

2.  Para verificar se os locais de extensão de CDP e AIA estão configurados corretamente, digite `pkiview.msc` e pressione Enter. O PKIView Enterprise PKI MMC é aberto.

3.  No painel esquerdo, clique no nome da autoridade de certificação.<p>Por exemplo, se o nome da sua autoridade de certificação for Corp-CA1-CA, clique em **Corp-CA1-CA**.

4. Na coluna status do painel de resultados, verifique se os valores para o seguinte mostram **OK**:

    - **Certificado de autoridade de certificação**
    - **Local de AIA #1**
    - **Local de CDP #1**


> [!TIP]
> Se o **status** de qualquer item não estiver **OK**, faça o seguinte:
> -   Abra o compartilhamento no servidor Web para verificar se os arquivos de lista de revogação de certificado e certificado foram copiados com êxito para o compartilhamento. Se eles não foram copiados com êxito para o compartilhamento, modifique os comandos de cópia com a fonte de arquivo correta e o destino de compartilhamento e execute os comandos novamente.
> -   Verifique se você inseriu os locais corretos para CDP e AIA na guia extensões de CA. Verifique se não há espaços extras ou outros caracteres nos locais que você forneceu.
> -   Verifique se você copiou a CRL e o certificado de autoridade de certificação para o local correto no servidor Web e se o local corresponde ao local fornecido para os locais de CDP e AIA na autoridade de certificação.
> -   Verifique se você configurou corretamente as permissões para a pasta virtual na qual o certificado de autoridade de certificação e a CRL estão armazenados.



