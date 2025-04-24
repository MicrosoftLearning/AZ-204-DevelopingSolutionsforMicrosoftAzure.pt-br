---
lab:
  title: Implantar o aplicativo conteinerizado no Serviço de Aplicativo do Azure
  description: Saiba como implantar e configurar um aplicativo conteinerizado no Serviço de Aplicativo do Azure.
---

# Implantar o aplicativo conteinerizado no Serviço de Aplicativo do Azure

Neste exercício, você implanta um aplicativo simples conteinerizado no Serviço de Aplicativo do Azure. 

Tarefas executadas neste exercício:

* Criar um recurso do Serviço de Aplicativo do Azure para um aplicativo conteinerizado
* Exibir os resultados
* Limpar os recursos

Este exercício deve levar aproximadamente **15** minutos para ser concluído.

## Antes de começar

Para concluir este exercício, você precisará de:

* Uma assinatura do Azure. Caso ainda não tenha uma avaliação gratuita, [inscreva-se em uma](https://azure.microsoft.com/).

## Criar um recurso do Aplicativo da Web

1. No navegador, vá par o portal do Azure [https://portal.azure.com](https://portal.azure.com). Faça login com suas credenciais do Azure se for solicitado.
1. Selecione **+ Criar um recurso** localizado no cabeçalho **Serviços do Azure** próximo à parte superior da página inicial. 
1. Na barra de pesquisa **Pesquisar no Marketplace**, insira *aplicativo Web* e pressione **Enter** para realizar a pesquisa.
1. No bloco Aplicativo Web, selecione a lista suspensa **Criar** e, em seguida, selecione **Aplicativo Web**.

    ![Captura de tela do bloco Aplicativo Web.](./media/01/create-web-app-tile.png)

Selecionar **Criar** faz abrir um modelo com algumas guias para serem preenchidas com informações sobre a sua implantação. As etapas a seguir orientam você sobre as alterações a serem feitas nas guias relevantes.

1. Preencha a guia **Básico** com as informações da seguinte tabela:

    | Configuração | Ação |
    |--|--|
    | **Assinatura** | Manter o valor padrão. |
    | **Grupo de recursos** | Selecione Criar novo, pressione Enter`rg-WebApp` e, em seguida, selecione OK. |
    | **Nome** | Insira um nome exclusivo, por exemplo, `<your-initials>-containerwebapp`. Substitua *\<your-initials>* por suas iniciais ou algum outro valor. O nome precisa ser exclusivo, por isso pode exigir algumas alterações. |
    | Controle deslizante na configuração **Nome** | Selecione o controle deslizante para desativá-lo. |
    | **Publicar** | Selecione a opção **Contêiner**. |
    | **Sistema operacional** | **Linux** deve ser selecionado. |
    | **Região** | Mantenha a seleção padrão ou escolha uma região perto de você. |
    | **Plano do Linux** | Mantenha a seleção padrão. |
    | **Plano de preços** | Selecione o menu suspenso e escolha o plano **F1 gratuito**. |

1. Selecione ou navegue até a guia **Contêiner** e insira as informações na tabela a seguir:

    | Configuração | Ação |
    |--|--|
    | **Suporte a sidecar** | Mantenha a posição desativada padrão. |
    | **Origem da Imagem** | Selecione **Outros registros de contêiner**. |
    | **Tipo de Acesso** | Mantenha a seleção padrão **Pública**. |
    | **URL do servidor do Registro** | Digite `mcr.microsoft.com/k8se`. |
    | **Imagem e rótulo** | Digite `quickstart:latest`. |
    | **Comando de inicialização** | Deixe em branco. |

1. Selecione a guia **Examinar + criar**.
1. Reveja as soluções que você fez e, em seguida, selecione o botão **Criar**.

Pode levar alguns minutos para que a implantação seja concluída. Ao terminar, selecione o botão **Ir para recurso**.

Agora que a sua implantação foi concluída, é hora de exibir o aplicativo Web. Selecione o link para o seu aplicativo Web localizado ao lado do campo **Domínio padrão** na seção **Essentials**. O link abrirá o site em uma nova guia.

>**Observação:** pode levar alguns minutos para que o aplicativo de contêiner implantado seja executado e exibido na nova guia.

## Limpar os recursos

Agora que você concluiu o exercício, exclua os recursos de nuvem que criou para evitar uso desnecessário de recursos.

1. Navegue até o grupo de recursos que você criou e exiba o conteúdo dos recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Excluir grupo de recursos**.
1. Insira o nome do grupo de recursos e confirme que deseja excluí-lo.
