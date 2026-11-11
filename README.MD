# API de Cadastro, Login e Formulário (Flask + JWT)

API REST em Python/Flask para envio (upload), listagem, consulta e download de arquivos com persistência de metadados via SQLAlchemy e SQLite.

## Tecnologias

- Python 3
- Flask
- Flask-SQLAlchemy (ORM e manipulação de banco de dados)
- SQLite (banco de dados embutido)
- Gunicorn (para execução em produção)

## Estrutura do projeto

```
.
├── controllers/
│   ├── download_controller.py   # Lógica de negócio para download e leitura de arquivos
│   └── upload_controller.py     # Lógica de negócio para recepção e validação do upload
├── database/
│   └── db.py                    # Instância e configuração do SQLAlchemy
├── models/
│   ├── download_model.py        # Modelo/tabela de downloads ou histórico
│   └── upload_model.py          # Modelo/tabela com metadados dos arquivos salvos
├── routes/
│   ├── download_route.py        # Rotas de download (blueprints)
│   └── upload_route.py          # Rotas de upload (blueprints)
├── app.py                       # Inicialização da aplicação Flask e registros dos blueprints
├── config.py                    # Configurações do app (banco de dados, pastas de upload)
├── README.MD                    # Documentação do projeto
└── requirements.txt             # Dependências do projeto

```

O projeto segue a arquitetura em camadas: **routes → controllers → models → database**.

## Como executar

1. Crie e ative um ambiente virtual (opcional, mas recomendado):

   ```bash
   python -m venv venv
   source venv/bin/activate      # Linux/macOS
   venv\Scripts\activate         # Windows (cmd)
   ```

2. Instale as dependências:

   ```bash
    pip install -r requirements.txt
   ```

3. Inicie a aplicação:

   ```bash
   python app.py
   ```

   A API ficará disponível em `http://127.0.0.1:5000`. As tabelas são criadas automaticamente na primeira execução.

## Autenticação

1. Download em `POST /download`.
2. Upload em `POST /upload`

## Endpoints

### Upload (`/uploads`)

| Método | Rota      | Protegida | Descrição                             |
| ------ | --------- | --------- | ------------------------------------- |
| POST   | `/upload` | Não       | Envia um novo arquivo para o servidor |

### Download (`/download`)

| Método | Rota         | Protegida | Descrição                              |
| ------ | ------------ | --------- | -------------------------------------- |
| GET    | `/download/` | Não       | Retorna o formulário do usuário logado |

## Exemplos de uso

**Upload**

```bash
curl -X POST http://127.0.0.1:5000/upload \
  -H "Content-Type: application/json" \

```

Resposta (201): `{"message": "Usuário registrado com sucesso"}`

**Download**

```bash
curl -X POST http://127.0.0.1:5000/download \
  -H "Content-Type: application/json" \

```

Resposta (200): `{"access_token": "<token>"}`

## Códigos de resposta

| Código | Significado                                         |
| ------ | --------------------------------------------------- |
| 200    | Operação realizada com sucesso                      |
| 201    | Recurso criado                                      |
| 400    | Dados obrigatórios ausentes ou usuário já existente |
