---
lab:
  title: Criar uma Função do Azure no Visual Studio Code
  description: 'Aprenda a criar uma Função do Azure com gatilho HTTP. Depois de criar e testar o código localmente no Visual Studio Code, você implanta a função no Azure.'
---

# Criar uma Função do Azure no Visual Studio Code

Neste exercício, você aprende a criar uma função C\# que responde a solicitações de HTTP. Depois de criar e testar o código localmente no Visual Studio Code, você implanta e testa a função no Azure.

Tarefas executadas neste exercício:

* Criar um recurso do Serviço de Aplicativo do Azure para um aplicativo conteinerizado
* Exibir os resultados
* Limpar os recursos

Este exercício deve levar aproximadamente **30** minutos para ser concluído.

## Antes de começar

Antes de começar, verifique se você tem os seguintes requisitos implementados:

* Uma assinatura do Azure. Caso ainda não tenha uma avaliação gratuita, [inscreva-se em uma](https://azure.microsoft.com/).

* [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing) versão 4.x.

* [Visual Studio Code](https://code.visualstudio.com/) em uma das [plataformas compatíveis](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* O [.NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) é a estrutura de destino.

* [Kit de Desenvolvimento em C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) para o Visual Studio Code.

* [Extensão do Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code.

## Criar seu projeto local

Nesta seção, você usará o Visual Studio Code para criar um projeto local do Azure Functions em C#. Mais adiante neste exercício, você publicará o código da função no Azure.

1. No Visual Studio Code, pressione F1 para abrir a paleta de comandos e pesquisar e executar o comando `Azure Functions: Create New Project...`.

1. Escolha o local de diretório para o workspace do projeto e escolha **Selecionar**. Você deve criar uma pasta ou escolher uma pasta vazia para o workspace do projeto. Não escolha uma pasta de projeto que já faça parte de um workspace.

1. Forneça as seguintes informações nos prompts:

    | Prompt | Ação |
    |--|--|
    | Selecione a pasta que vai conter os seus projetos de função | Selecione **Procurar...** para selecionar uma pasta para o seu aplicativo.
    | Selecionar um idioma | selecione **C#**. |
    | Selecionar um runtime do .NET | Selecione **.NET 8.0 Isolado** |
    | Selecione um modelo para a primeira função do projeto | Selecione **Gatilho de HTTP**.<sup>1</sup> |
    | Forneça um nome de função | Digite `HttpExample`. |
    | Forneça um namespace | Digite `My.Function`. |
    | Nível de autorização | Escolha **Anônimo**, que permite que qualquer pessoa chame o seu ponto de extremidade de função. |

    <sup>1</sup> Dependendo das suas configurações de VS Code, talvez seja necessário usar a opção **Alterar filltro dos modelos** para ver a lista completa de modelos.

1. O Visual Studio Code usa as informações fornecidas e gera um projeto do Azure Functions com um gatilho HTTP. Você pode exibir os arquivos de projeto locais no Explorer.

### Executar a função localmente

O Visual Studio Code integra-se ao Azure Functions Core Tools para permitir que você execute esse projeto em seu computador de desenvolvimento local antes da publicação no Azure.

1. Verifique se o terminal está aberto no Visual Studio Code. É possível abrir o terminal selecionando **Terminal** e, em seguida, **Novo terminal** na barra de menus. 

1. Pressione **F5** para iniciar o projeto do aplicativo de funções no depurador. A saída do Core Tools é exibida no painel **Terminal**. Seu aplicativo é iniciado no painel **Terminal**. Você pode ver o ponto de extremidade de URL de sua função disparada por HTTP localmente.

    ![Uma captura de tela do ponto de extremidade da sua função disparada por HTTP é exibida no painel Terminal.](./media/06/run-function-local.png)

1. Com o Core Tools em execução, acesse a área **Azure: Funções**. Em **Funções**, expanda **Projeto Local** > **Funções**. Clique com o botão direito do mouse na função **HttpExample** e escolha **Executar Função Agora...**

    ![Captura de tela mostrando a localização da etapa Executar agora...](./media/06/execute-function-local.png)

1. Em **Inserir corpo da solicitação**, digite o valor do corpo da mensagem de solicitação de `{ "name": "Azure" }`. Pressione **Enter** para enviar essa mensagem de solicitação à função. Quando a função é executada localmente e retorna uma resposta, uma notificação é gerada no Visual Studio Code.

    selecione o ícone de sino de notificação para visualizar a notificação. As informações sobre a execução da função são mostradas no painel **Terminal**.

1. Pressione **Shift + F5** a fim de parar o Core Tools e desconectar o depurador.

Depois de verificar se a função é executada corretamente no computador local, é hora de usar o Visual Studio Code para publicar o projeto diretamente no Azure.

## Implantar e executar a função no Azure

Nesta seção, você cria um recurso do Aplicativo de Funções do Azure e implanta a função no recurso.

### Entrar no Azure

Antes de poder publicar seu aplicativo, você precisa entrar no Azure. Se você já tiver entrado, vá para a próxima seção.

1. Se você ainda não tiver entrado, escolha o ícone do Azure na barra de atividades e na área **Azure: Functions**, escolha **Entrar no Azure...**.

    ![Captura de tela do botão Entrar no Azure.](./media/06/functions-sign-into-azure.png)

1. Quando solicitado no navegador, escolha sua conta do Azure e entre usando suas credenciais de conta do Azure.

1. Depois de conseguir entrar, você pode fechar a nova janela do navegador. As assinaturas que pertencem à sua conta do Azure são exibidas na barra lateral.

### Criar recursos no Azure

Nesta seção, você criará os recursos do Azure necessários para implantar seu aplicativo de funções local.

1. Selecione o ícone do Azure na barra de atividade e, na área **Recursos**, clique no botão **Criar recurso...**.

    ![Captura de tela do botão Criar Recursos.](./media/06/create-resource.png)    

1. Forneça as seguintes informações nos prompts:

    | Prompt | Ação |
    |--|--|
    | Selecionar um recurso para criar | Selecione **Criar um aplicativo de funções no Azure...** |
    | Selecionar uma assinatura | Selecione a assinatura a ser usada. *Essa opção não será exibida caso você possua apenas uma assinatura.* |
    | Insira um nome exclusivo globalmente para o aplicativo de funções | Digite um nome que seja válido em um caminho de URL, por exemplo`myfunctionapp`. O nome que você digitar será validado para garantir que seja único. |
    | Selecione uma localização para novos recursos | Para obter um melhor desempenho, escolha uma região perto de você. |
    | Selecionar uma pilha de runtime | Selecione **.NET 8.0 Isolado**. |
    | Selecione um tamanho de memória de instância | Selecione **2048 Padrão** |
    | Modificar a contagem máxima de instâncias | Aceite o padrão **100**. |

    A extensão mostra o status de recursos individuais conforme eles são criados na área **AZURE** da janela do terminal.
    
1. Quando concluído, os seguintes recursos do Azure serão criados em sua assinatura, usando nomes baseados em seu nome do aplicativo de funções:

    * Um grupo de recursos, que é um contêiner lógico para recursos relacionados.
    * Uma conta de Armazenamento do Azure padrão, que mantém o estado e outras informações sobre seus projetos.
    * Um plano de consumo flexível, que define o host subjacente para o seu aplicativo de funções sem servidor.
    * Um aplicativo de funções, que fornece o ambiente para a execução do código de função. Um aplicativo de funções lhe permite agrupar funções como uma unidade lógica para facilitar o gerenciamento, a implantação e o compartilhamento de recursos dentro do mesmo plano de hospedagem.
    * Uma instância do Application Insights conectada ao aplicativo de funções, que controla o uso de sua função sem servidor.

### Implantar o projeto no Azure

> **! Importante:** publicar em uma função existente substitui todas as implantações anteriores.

1. Na paleta de comandos, pesquise por e execute o comando **Azure Functions: Deploy to Function App...**

1. Insira a assinatura que você usou ao criar os recursos.

1. Selecione o aplicativo de funções que você criou. Quando solicitado sobre a substituição de implantações anteriores, selecione **Implantar** para implantar seu código de função no novo recurso do aplicativo de funções.

1. Após a conclusão da implantação, selecione **Exibir Saída** para exibir os detalhes dos resultados da implantação. Se você perder a notificação, selecione o ícone de sino da notificação no canto inferior direito para vê-la novamente.

    ![Captura de tela da janela Exibir Saída.](./media/06/function-view-output.png)

### Executar a função no Azure

1. Novamente na área **Recursos** na barra lateral, expanda sua assinatura, o novo aplicativo de funções e **Funções**. **Clique com o botão direito do mouse** na função **HttpExample** e escolha **Execute Function Now...**

    ![Captura de tela da opção Execute Function Now.](./media/06/execute-function-remote.png)

1. Em **Insira o corpo da solicitação**, você verá o valor do corpo da mensagem de solicitação igual a `{ "name": "Azure" }`. Clique em ENTER para enviar essa mensagem de solicitação à função.

1. Quando a função é executada no Azure e retorna uma resposta, uma notificação é gerada no Visual Studio Code. selecione o ícone de sino de notificação para visualizar a notificação.

## Limpar os recursos

Agora que você concluiu o exercício, exclua os recursos de nuvem que criou para evitar uso desnecessário de recursos.

1. No navegador, vá par o portal do Azure [https://portal.azure.com](https://portal.azure.com). Faça login com suas credenciais do Azure se for solicitado.
1. Navegue até o grupo de recursos que você criou e exiba o conteúdo dos recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Excluir grupo de recursos**.
1. Insira o nome do grupo de recursos e confirme que deseja excluí-lo.
