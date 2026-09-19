# これは演習用のフォルダです。


## Linuxデモンストレーション
以下のファイル構成を目指します。
```ファイル構成
lecture/
├── README.md
├── docker-compose.yml
├── backend/
│   ├── Dockerfile
│   ├── main.py
|   ├── .env
|   ├── .env.example 
│   ├── requirements.txt
└── frontend/
    ├── Dockerfile
    ├── index.html
    ├── package-lock.json
    ├── package.json
    ├── tsconfig.json
    ├── vite.config.ts
    ├── dist/
    │   ├── index.html
    │   └── assets/
    │       └── index-A4XX2AKH.js
    └── src/
        ├── App.tsx
        └── main.tsx
```


## Dockerデモンストレーション
Docker composeを使って開発サーバーを立ち上げられます。
```
docker compose up -d --build
```

以下が開発用サーバーになっています。
http://localhost:8080/