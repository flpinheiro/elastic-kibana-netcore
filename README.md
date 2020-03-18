# Elastic Kibana Net Core

Este projeto é uma versão resumida de um projeto .net que serve para enviar os logs da aplicação para o ELK Stack (Elastic-Logstash-Kibana) da elastic company.

Este modelo está usando uma Web App com paginas razor, mas poderia ser feito com qualquer outro modelo de projeto .net, tal como o MVC, Web api, entre outros, pois as configurações básicas se encontram na classe Startup.cs

O projeto ELK encontra-se no seguinte repositorio

https://github.com/flpinheiro/docker-elk

Neste link é possível se encontrar todas as informações necessárias para que se coloque o ELK Stack para funcionar corretamente. 

Este modelo utiliza o Serilog como sistema de logs, qualquer dúvida pode ser esclarecida na página deles no seguinte link

https://github.com/serilog

Existem diversos projetos, cada um com a documentação excepcionalmente boa.

## Configuração básica.

Instale os seguintes pacotes:

        Install-Package Serilog
        Install-Package Serilog.Extensions.Logging
        Install-Package Serilog.Sinks.ElasticSearch

ou via CLI

        dotnet add package Serilog
        dotnet add package Serilog.Extensions.Logging
        dotnet add package Serilog.Sinks.ElasticSearch

Adicione o seguinte conjunto de linhas no arquivo appsetings.json

          "ElasticConfiguration": {
            "Uri": "http://localhost:5000/"
          }
Aqui a porta 5000 é a porta padrão do logstash. 

Na Classe Startup.cs adicione os seguintes trechos de código.

Nos imposts

      using Serilog;
      using Serilog.Sinks.Elasticsearch;

No Construtor

            var elasticUri = Configuration["ElasticConfiguration:Uri"];

            Log.Logger = new LoggerConfiguration()
                .Enrich.FromLogContext()
                .WriteTo.Elasticsearch(new ElasticsearchSinkOptions(new Uri(elasticUri))
                {
                    AutoRegisterTemplate = true,
                })
                .CreateLogger();
                
No metodo Configure:
* Adicione a assinatura do metodo

      ILoggerFactory loggerFactory

* adicione no corpo do metodo, o quanto antes a seguinte linha.

      loggerFactory.AddSerilog();

Configurações adicionais podem ser necessárias.
