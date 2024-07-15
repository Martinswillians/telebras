## API Hughes

- Swagger: https://5y1dvxkdo5.execute-api.us-east-1.amazonaws.com/api/api-docs
- Postman collection: [Hughes_DW_Science_Logic__Telebras__-_1.2.postman_collection.json](/uploads/aaf913e3d6bd4b3d07693363d76eb4d6/Hughes_DW_Science_Logic__Telebras__-_1.2.postman_collection.json)

Realizar a replicação de dados da API da Hughes. Inicialmente são 5 endpoints:

1. **Devices** : Recupera o ID do circuito do lado da Hughes
2. **Availability** : Recupera os dados de disponibilidade
3. **Latency** : Recupera os dados de latência
4. **Traffic** : Recupera os dados de tráfego
5. **Speed Test** : Recupera os dados de testes de velocidade

**Observações**

- A autenticação é realizada em todas as requisições por meio de `Basic Auth` e da chave `x-api-key` no _header_. O usuário/senha e a chave deverão ser variáveis de ambiente
- Todas as chamadas são obrigatórios os parâmetros `page` e `limit`, sendo que `limit` é de no máximo 1000 itens. Com isso se houver mais de 1000 itens no total tem que ser realizada uma paginação de dados para trazer tudo do período requisitado. A resposta sempre vem com os campos `current`, `page`, `limit`. Sendo que `current` é a quantidade de itens que a resposta retornou. No exemplo abaixo vieram 468 registros. Se retornasse 1000, significa que há mais itens e tem que ser realizada uma segunda requisição à API para a página 2:

```json
{
    "current": 468,
    "page": 1,
    "limit": 1000,
    "data": [
        {
            "device_id": 18904,
            "name": "HTB000002108655",
            "created_at": "2024-06-08T00:00:41.000Z",
            "telebras_circuit": "MASF000263"
        },
        {
            "device_id": 18905,
            "name": "HTB000002108551",
            "created_at": "2024-06-08T00:00:41.000Z",
            "telebras_circuit": null
        },
        ...
    ]
}
```
- Nos _endpoints_ de dados de coleta (disponibilidade, latência, tráfego e teste de velocidade), além de `page` e `limit`, os parâmetros `device_id`, `initial_date` e `final_date` também são obrigatórios.
- No _endpoint_ de latência, além dos parâmetros de coleta, o parâmetro `hour_granularity` também é obrigatório e, no caso da Telebras, tem que ser sempre o valor 1.
- No _endpoint_ de tráfego, além dos parâmetros de coleta, o parâmetro `hour_granularity` também é obrigatório e, no caso da Telebras, tem que ser sempre o valor 1 e também o parâmetro `types` que pode ser `INPUT_TRAFFIC` ou `OUTPUT_TRAFFIC` ou os dois separados por virgula `INPUT_TRAFFIC,OUTPUT_TRAFFIC`, sendo que `INPUT_TRAFFIC` é o valor de download e `OUTPUT_TRAFFIC` e o valor de upload.
- No _endpoint_ de teste de velocidade, além dos parâmetros de coleta, o parâmetro `hour_granularity` também é obrigatório e, no caso da Telebras, tem que ser sempre o valor 1 e também o parâmetro `types` que pode ser `DOWNLOAD_SPEED_TEST` ou `UPLOAD_SPEED_TEST` ou os dois separados por virgula `DOWNLOAD_SPEED_TEST,UPLOAD_SPEED_TEST`, sendo que `DOWNLOAD_SPEED_TEST` é o valor de download e `UPLOAD_SPEED_TEST` e o valor de upload.

O portal v1 as replicações já foram implementadas. Caso haja alguma dúvida sobre implementação e desejar comparar o código verificar no repositório de [Replicadores - Hughes](https://gitlab.telebras.com.br/portaldeclientes/replicadores/hughes/-/tree/master/api/v2/replicators/hughes?ref_type=heads)
Recolher
tem menu de contexto