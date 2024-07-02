# ProjetoDesafioOne

### Estrutura do Projeto: Conversor de Moedas em Java

#### Passo a Passo

1. **Cadastro na API "Extended Rate"**
   - Cadastre-se na API "Extended Rate" para obter a chave de acesso.
   - Guarde a chave de acesso que será enviada por e-mail.

2. **Configuração do Trello**
   - Crie um quadro no Trello para organizar as tarefas.
   - Divida as tarefas em listas como "A Fazer", "Em Progresso" e "Concluído".
   - Adicione os seguintes cards com as respectivas tarefas:

### Lista de Tarefas no Trello

#### A Fazer

- **Configuração Inicial**
  - Configurar o ambiente de desenvolvimento Java.
  - Adicionar dependências necessárias (como bibliotecas para requisições HTTP e manipulação de JSON).

- **Cadastro na API**
  - Cadastre-se na API "Extended Rate".
  - Adicionar a chave de acesso ao projeto de forma segura.

- **Implementação da Interface Gráfica**
  - Criar uma interface gráfica simples para entrada de dados.
  - Permitir que o usuário escolha entre as 6 opções de conversão de moeda.
  - Campo para o usuário inserir o valor a ser convertido.

- **Integração com a API "Extended Rate"**
  - Fazer requisições HTTP à API "Extended Rate" para obter cotações em tempo real.
  - Desserializar o formato JSON da resposta.

- **Lógica de Conversão**
  - Implementar a lógica de conversão de moedas usando as cotações obtidas.
  - Exibir o resultado da conversão para o usuário.

- **Funcionalidade Extra: Histórico de Conversões**
  - Implementar uma funcionalidade para armazenar e exibir o histórico de conversões realizadas.

#### Em Progresso

- **Teste de Requisições à API**
  - Testar as requisições à API e garantir que os dados sejam recebidos corretamente.
  - Tratar possíveis erros de conexão e resposta da API.

- **Desenvolvimento da Lógica de Conversão**
  - Codificar a lógica de conversão de moedas.
  - Validar a precisão dos resultados.

#### Concluído

- **Teste e Validação**
  - Testar o programa com diferentes valores e opções de conversão.
  - Validar a precisão e funcionalidade da conversão.

- **Documentação**
  - Documentar o código e a lógica utilizada.
  - Instruções de uso do programa para o usuário final.

### Exemplo de Código

#### Classe Principal

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import com.google.gson.JsonObject;
import com.google.gson.JsonParser;

public class ConversorMoedas {

    private static final String API_KEY = "sua_chave_de_acesso";
    private static final String API_URL = "https://api.extendedrate.com/latest";

    public static void main(String[] args) {
        // Implementação da interface gráfica e interação com o usuário

        // Exemplo de chamada à API
        String jsonResponse = getApiResponse("USD", "BRL");
        double taxaConversao = parseJsonResponse(jsonResponse, "BRL");

        // Exemplo de lógica de conversão
        double valorEmReais = converterMoeda(100, taxaConversao);
        System.out.println("Valor convertido: " + valorEmReais);
    }

    private static String getApiResponse(String fromCurrency, String toCurrency) {
        try {
            URL url = new URL(API_URL + "?access_key=" + API_KEY + "&base=" + fromCurrency + "&symbols=" + toCurrency);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            connection.setRequestMethod("GET");

            BufferedReader in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String inputLine;
            StringBuilder response = new StringBuilder();

            while ((inputLine = in.readLine()) != null) {
                response.append(inputLine);
            }
            in.close();
            return response.toString();
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

    private static double parseJsonResponse(String jsonResponse, String toCurrency) {
        JsonObject jsonObject = JsonParser.parseString(jsonResponse).getAsJsonObject();
        return jsonObject.getAsJsonObject("rates").get(toCurrency).getAsDouble();
    }

    private static double converterMoeda(double valor, double taxaConversao) {
        return valor * taxaConversao;
    }
}
```

### Dependências

- **Bibliotecas Necessárias**:
  - `com.google.code.gson:gson` para manipulação de JSON.
  - `org.apache.httpcomponents:httpclient` para fazer requisições HTTP (ou use `java.net.HttpURLConnection`).

### Funcionalidades Extras

1. **Histórico de Conversões**
   - Use uma estrutura de dados para armazenar cada conversão realizada e exiba ao usuário quando solicitado.

2. **Validação de Entrada**
   - Verifique se o valor de entrada é válido (ex., um número positivo).

Este guia fornece uma base sólida para desenvolver seu conversor de moedas em Java, integrando a API "Extended Rate" e organizando o trabalho no Trello.
