# Serverless Upload

Upload de imagens para o **Amazon S3** usando uma **URL pré-assinada**, gerada por uma função **AWS Lambda** exposta por um **API Gateway**.

> Baseado no tutorial [tutorial-serverless-upload](https://github.com/UmInventorQualquer/tutorial-serverless-upload), do canal *Um Inventor Qualquer*.

## Como funciona

```
upload.js ──GET──► API Gateway ──► Lambda ──► devolve { url, fileName }
    │
    └──PUT (imagem)──► S3 (pela URL pré-assinada)

api.js ──GetObject──► S3 ──► downloads/<fileName>
```

1. O `upload.js` chama a API e recebe uma URL temporária (válida por 30 minutos) e o nome do arquivo.
2. Ele envia a imagem direto para o S3 usando essa URL. O arquivo não passa pela Lambda.
3. O `api.js` baixa o arquivo enviado, usando as credenciais do profile `tutorials3`.

## Estrutura

```
.
├── .gitignore
├── lambda/
│   └── uploader/
│       ├── index.js        # função Lambda (gera a URL pré-assinada)
│       ├── package.json    # dependências da Lambda (aws-sdk e uuid)
│       ├── build.sh        # gera o uploader.zip
│       └── uploader.zip    # pacote pronto para subir na Lambda
└── scripts/
    ├── files/              # imagens de exemplo
    ├── upload.js           # pede a URL e envia a imagem
    ├── api.js              # baixa o arquivo enviado
    └── package.json        # dependências dos scripts
```

A pasta `scripts/downloads/` é criada localmente e fica fora do Git (está no `.gitignore`).
