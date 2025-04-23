---
lab:
  title: Criar recursos do Azure Cosmos DB com NoSQL usando .NET
  description: Saiba como criar recursos de banco de dados e contêineres no Azure Cosmos DB com o SDK do Microsoft .NET v3.
---

# Criar recursos do Azure Cosmos DB com NoSQL usando .NET

Neste exercício, você cria um aplicativo de console que cria um contêiner, um banco de dados e um item no Azure Cosmos DB.

Tarefas executadas neste exercício:

* Criar uma conta do Azure Cosmos DB
* Criar o aplicativo de console
* Executar o aplicativo de console e verificar os resultados

Este exercício deve levar aproximadamente **30** minutos para ser concluído.

## Antes de começar

Antes de começar, verifique se você tem os seguintes requisitos implementados:

* Uma assinatura do Azure. Caso ainda não tenha uma avaliação gratuita, [inscreva-se em uma](https://azure.microsoft.com/).

## Criar uma conta do Azure Cosmos DB

Nesta seção do exercício, você cria um grupo de recursos e uma conta do Azure Cosmos DB. Você também registra o ponto de extremidade e a chave de acesso da conta.

1. No navegador, vá par o portal do Azure [https://portal.azure.com](https://portal.azure.com). Faça login com suas credenciais do Azure se for solicitado.

1. Use o botão **[\>_]** à direita da barra de pesquisa na parte superior da página para criar um novo Cloud Shell no portal do Azure, selecionando um ambiente do ***Bash***. O Cloud Shell fornece uma interface de linha de comando em um painel na parte inferior do portal do Azure.

    > **Observação**: se você já criou uma Cloud Shell que usa um ambiente *PowerShell*, troque-o pelo ***Bash***.

1. Na barra de ferramentas do Cloud Shell, no menu **Configurações**, selecione **Ir para a versão clássica** (isso é necessário para usar o editor de código).

1. Crie um grupo de recursos para os recursos necessários para este exercício. Substitua **\<myResourceGroup>** pelo nome que você deseja usar para o grupo de recursos. Você pode substituir **useast** por uma região perto de você se for necessário. 

    ```
    az group create --location useast --name <myResourceGroup>
    ```

1. Execute o seguinte comando para criar a conta do Azure Cosmos DB. Substitua **\<myCosmosDBacct>** por um nome *exclusivo* para identificar sua conta do Azure Cosmos DB. Substitua **\<myResourceGroup>** pelo nome que você escolheu antes.

    >**Observação:** o nome da conta só pode conter letras minúsculas, números e o caractere de hífen (-). Ela deve ter entre 3 e 31 caracteres. *Esse comando leva alguns minutos para ser concluído.*

    ```
    az cosmosdb create -n <myCosmosDBacct> -g <myResourceGroup>
    ```

1.  Execute o seguinte comando para recuperar o **documentEndpoint** para a conta do Azure Cosmos DB. Registre o ponto de extremidade dos resultados do comando. Ele será necessário no exercício. Substitua **\<myCosmosDBacct>** e **\<myResourceGroup>** pelos nomes que você criou.

    ```bash
    az cosmosdb show -n <myCosmosDBacct> -g <myResourceGroup> --query "documentEndpoint" --output tsv
    ```

1. Obtenha a chave primária da conta usando o seguinte comando. Registre a **primaryMasterKey** dos resultados do comando para usar no código. Substitua **\<myCosmosDBacct>** e **\<myResourceGroup>** pelos nomes que você criou.

     ```
    # Retrieve the primary key
    az cosmosdb keys list -n <myCosmosDBacct> -g <myResourceGroup>
    ```

## Criar o aplicativo de console

Agora que os recursos necessários estão implantados no Azure, a próxima etapa é configurar o aplicativo de console usando a mesma janela do terminal no Visual Studio Code.

1. Crie uma pasta para o projeto e acesse-a.

    ```bash
    mkdir cosmosdb
    cd cosmosdb
    ```

1. Crie o aplicativo de console .NET.

    ```dotnetcli
    dotnet new console --framework net8.0
    ```

1. Execute os seguinntes comandos para adicionar o pacote **Microsoft.Azure.Cosmos** ao projeto, e também o pacote de suporte **Newtonsoft.Json**.

    ```dotnetcli
    dotnet add package Microsoft.Azure.Cosmos --version 3.*
    dotnet add package Newtonsoft.Json --version 13.*
    ```

Agora, é hora de substituir o código do modelo no arquivo **Program.cs** usando o editor no Cloud Shell.

### Adicione o código inicial ao projeto

1. Execute o seguinte comando no Cloud Shell para começar a editar o aplicativo.

    ```bash
    code Program.cs
    ```

1. Substitua todo código existente pelo seguinte trecho de código a seguir após as instruções **using**. Substitua os valores de espaço reservado **\<documentEndpoint>** e **\<primaryKey>** seguindo as instruções nos comentários do código.

    O código fornece a estrutura geral do aplicativo e alguns elementos necessários. Revise os comentários no código, adicione código nas áreas especificadas pelas instruções. 

    ```csharp
    using Microsoft.Azure.Cosmos;
    
    namespace CosmosExercise
    {
        // This class represents a product in the Cosmos DB container
        public class Product
        {
            public string? id { get; set; }
            public string? name { get; set; }
            public string? description { get; set; }
        }
    
        public class Program
        {
            // Cosmos DB account URL - replace with your actual Cosmos DB account URL
            static string cosmosDbAccountUrl = "<documentEndpoint>";
    
            // Cosmos DB account key - replace with your actual Cosmos DB account key
            static string accountKey = "<primaryKey>";
    
            // Name of the database to create or use
            static string databaseName = "myDatabase";
    
            // Name of the container to create or use
            static string containerName = "myContainer";
    
            public static async Task Main(string[] args)
            {
                // Create the Cosmos DB client using the account URL and key
    
    
                try
                {
                    // Create a database if it doesn't already exist
    
    
                    // Create a container with a specified partition key
    
    
                    // Define a typed item (Product) to add to the container
    
    
                    // Add the item to the container
                    // The partition key ensures the item is stored in the correct partition
    
    
                }
                catch (CosmosException ex)
                {
                    // Handle Cosmos DB-specific exceptions
                    // Log the status code and error message for debugging
                    Console.WriteLine($"Cosmos DB Error: {ex.StatusCode} - {ex.Message}");
                }
                catch (Exception ex)
                {
                    // Handle general exceptions
                    // Log the error message for debugging
                    Console.WriteLine($"Error: {ex.Message}");
                }
            }
        }
    }
    ```

### Adicione código para criar o cliente e executar operações 

Nesta seção do exercício, você adiciona código em áreas especificadas dos projetos para criar: cliente, banco de dados, contêiner, e adiciona um item de exemplo ao contêiner.

1. Adicione o seguinte código no espaço após o comentário **// Create the Cosmos DB client using the account URL and key**. Esse código define o cliente usado para se conectar à sua conta do Azure Cosmos DB.

    ```csharp
    CosmosClient client = new(
        accountEndpoint: cosmosDbAccountUrl,
        authKeyOrResourceToken: accountKey
    );
    ```

    >Observação: é uma prática recomendada usar a **DefaultAzureCredential** da biblioteca do *Azure Identity*. Isso pode exigir alguns requisitos de configuração adicionais no Azure, dependendo de como a sua assinatura está configurada. 

1. Adicione o seguinte código no espaço após o comentário **// Create a database if it doesn't already exist**. 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. Adicione o seguinte código no espaço após o comentário **// Create a container with a specified partition key**. 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. Adicione o seguinte código no espaço após o comentário **// Define a typed item (Product) to add to the container**. Isso define o item que será adicionado ao contêiner.

    ```csharp
    Product newItem = new Product
    {
        id = Guid.NewGuid().ToString(), // Generate a unique ID for the product
        name = "Sample Item",
        description = "This is a sample item in my Azure Cosmos DB exercise."
    };
    ```

1. Adicione o seguinte código no espaço após o comentário **//Add the item to the container**. 

    ```csharp
    ItemResponse<Product> createResponse = await container.CreateItemAsync(
        item: newItem,
        partitionKey: new PartitionKey(newItem.id)
    );
    ```

1. Agora que o código está completo, salve o seu progresso. Use **ctrl + s** para salvar o arquivo e **ctrl + q** para sair do editor.

1. Execute o comando a seguir no Cloud Shell para testar se há erros no projeto. Se houver erros, abra o arquivo *Program.cs* no editor e verifique se há código ausente ou erros de colagem.

    ```bash
    dotnet build
    ```

Agora que o projeto foi concluído, é hora de executar o aplicativo e verificar os resultados no portal do Azure.

## Execute o aplicativo e verifique os resultados

1. Execute o `dotnet run` comando no Cloud Shell. O resultado deve ser algo semelhante ao seguinte exemplo.

    ```
    Created or retrieved database: myDatabase
    Created or retrieved container: myContainer
    Created item: c549c3fa-054d-40db-a42b-c05deabbc4a6
    Request charge: 6.29 RUs
    ```

1. No portal do Azure, navegue até o recurso do Azure Cosmos DB que você criou antes. Selecione **Data Explorer** no painel de navegação esquerdo. No **Data Explorer**, selecione **myDatabase** e expanda **myContainer**. Você pode exibir o item que criou selecionando **Itens**.

    ![Captura de tela mostrando a localização dos itens no Data Explorer.](./media/09/cosmos-data-explorer.png)

## Limpar os recursos

Agora que você concluiu o exercício, exclua os recursos de nuvem que criou para evitar uso desnecessário de recursos.

1. Navegue até o grupo de recursos que você criou e exiba o conteúdo dos recursos usados neste exercício.
1. Na barra de ferramentas, selecione **Excluir grupo de recursos**.
1. Insira o nome do grupo de recursos e confirme que deseja excluí-lo.
