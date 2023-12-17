# docker-note
## 要更改正在運行的 Docker 專案中的程式碼並重新執行，而不刪除原本運行的資料庫，可以按照以下步驟進行：

1. **進入容器**：首先，使用以下命令進入正在運行的 Docker 容器。假設你的容器名稱是 "my-container"：

    ```bash
    docker exec -it my-container /bin/sh
    ```

    這會打開一個 shell 會話，你可以在其中進行程式碼變更。

2. **進行程式碼變更**：在容器中，瀏覽到你的應用程式程式碼所在的目錄，然後使用你喜好的編輯器進行程式碼編輯。例如，如果你的程式碼在 `/app` 目錄下，可以使用 `cd` 命令切換到該目錄，然後使用編輯器編輯程式碼。

3. **儲存變更**：在容器中編輯完程式碼後，確保你儲存了變更。

4. **退出容器**：使用 `exit` 命令退出容器 shell 會話：

    ```bash
    exit
    ```

5. **重新啟動容器**：現在，你已經在容器中進行了程式碼變更，你可以使用以下命令重新啟動容器：

    ```bash
    docker restart my-container
    ```

    替換 "my-container" 為你實際使用的容器名稱。

這樣，你的容器將會重新啟動，應用程式程式碼將會更新，但資料庫不會被刪除，除非你明確執行資料庫相關的操作。請確保在進行這些步驟之前備份任何重要的數據，以防萬一。

##容器中没有安装 sudo。在 Docker 容器中，通常不需要使用 sudo 来安装软件包。
你可以直接使用 apk（适用于 Alpine Linux）或者 apt-get（适用于 Debian/Ubuntu Linux）来安装软件包。

假设你的容器使用的是 Alpine Linux，你可以运行以下命令来安装 Git：

```
$ apk update
$ apk add git

```

如果你的容器使用的是 Debian/Ubuntu Linux，可以运行以下命令来安装 Git：

```
$ apt-get update
$ apt-get install git


```


## 要查看正在運行的 Docker 容器的名稱，你可以使用以下命令：

```bash
docker ps
```

這個命令會列出正在運行的容器，包括容器的名稱、ID、狀態等信息。在列表中，你可以找到你感興趣的容器的名稱，通常會在 "NAMES" 列中顯示。

請注意，如果容器已經停止運行，它不會顯示在這個列表中。如果你想查看所有容器（包括已停止的），可以在命令中加入 `-a` 選項，如下所示：

```bash
docker ps -a
```

這樣將會列出所有容器的信息，包括已停止的容器。

## 如果你的 Docker 容器是基於某個映像（image）運行的，並且你想在更新代碼後重啟容器，以下是一個常見的流程：

1. **停止容器**：首先，使用 `docker stop` 命令停止你的容器。請將 `container_name` 替換為你的容器的實際名稱或容器的ID。

    ```bash
    docker stop container_name
    ```

2. **更新代碼**：現在，你可以進行代碼的更新，確保你的新代碼保存在容器所使用的目錄中。

3. **重新啟動容器**：使用 `docker start` 命令重新啟動你的容器。

    ```bash
    docker start container_name
    ```

4. **進入容器（如果需要）**：如果你需要在容器內執行命令或調試，可以使用 `docker exec` 進入容器。

    ```bash
    docker exec -it container_name  /bin/sh
    ```

   這將使你進入容器的 shell 環境，以便進行進一步的操作。

請確保你在更新代碼後重新構建容器（如果適用），以確保容器中運行的應用程序使用了最新的代碼。這是一個基本的流程，實際步驟可能會因你的應用程序和工作流程而異。

請注意，如果你的應用程序需要特殊的配置或數據庫遷移，你可能需要執行其他步驟來確保應用程序正確運行。





## 要使用 Docker Volume 來保存正在運行的 Docker 容器中的數據，您可以按照以下步驟操作：

創建 Docker Volume：
在您的命令行界面上運行以下命令，以創建一個 Docker Volume。這將用於保存容器中的數據。

```
docker volume create mydata
```
這裡，mydata 是 Docker Volume 的名稱。

1.運行 MySQL 容器：
如果您的 MySQL 容器尚未運行，請確保使用以下類似的命令運行它，並將 Docker Volume 掛載到容器中。

```
docker run -d --name mysql-container -e MYSQL_ROOT_PASSWORD=yourpassword -v mydata:/var/lib/mysql mysql:latest
```
這裡，mydata 是您在第 1 步中創建的 Docker Volume 的名稱。

2.運行 Web 服務容器：
運行您的 Web 服務容器，並將 Docker Volume 掛載到 Web 服務容器的 /app/public 目錄，假設 /app/public 是您希望保存的目錄。

```
docker run -d --name web-container -v mydata:/app/public your-web-image:latest
```
這裡，your-web-image 是您的 Web 服務容器映像的名稱。

現在，您的 MySQL 數據庫和 Web 服務容器的 /app/public 目錄都被保存在名為 mydata 的 Docker Volume 中。即使您停止和刪除容器，數據也會保持不變，並且在以後的運行中可以輕鬆訪問。

請確保替換示例中的容器名稱、密碼、映像名稱和目錄路徑，以符合您的實際需求。




