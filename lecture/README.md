# これは演習用のフォルダです。


## Linuxデモンストレーション
以下のファイル構成・権限を目指します。
権限は `ls -la` で表示される記号表記（例: `-rw-------`）で記載しています。

```ファイル構成
lecture/                                        drwxr-xr-x
├── README.md                                   -rw-r--r--
├── docker-compose.yml                          -rw-r--r--
├── backend/                                     drwxr-xr-x
│   ├── Dockerfile                              -rw-r--r--
│   ├── main.py                                 -rw-r--r--
|   ├── .env                                    -rw-------  ← chmod 600（機密情報のため所有者のみ）
|   ├── .env.example                            -rw-r--r--  ← 共有用テンプレート
│   ├── requirements.txt                        -rw-r--r--
└── frontend/                                    drwxr-xr-x
    ├── Dockerfile                              -rw-r--r--
    ├── index.html                              -rw-r--r--
    ├── package-lock.json                       -rw-r--r--
    ├── package.json                            -rw-r--r--
    ├── tsconfig.json                           -rw-r--r--
    ├── vite.config.ts                          -rw-r--r--
    └── src/                                     drwxr-xr-x
        ├── App.tsx                             -rw-r--r--
        └── main.tsx                            -rw-r--r--
```


## Dockerデモンストレーション
Docker composeを使って開発サーバーを立ち上げられます。
```
docker compose up -d --build
```

以下が開発用サーバーになっています。
http://localhost:8080/