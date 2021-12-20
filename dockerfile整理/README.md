## 基礎指令
- `FROM` 決定你的基底映像檔，例如 `FROM python:3.6.8-alpine3.7`、`FROM nginx:latest`
- `MAINTAINTER` 定義維護者，例如 `MAINTERIN alan <alan@gmail.com>`
- `COPY` 複製檔案至映像檔內，例如 `COPY . /app`
- `ADD` 複製檔案至映像檔內，與COPY指令不同處有二個，一個是來源可以是URL（但不建議），第二個是若來源是TAR檔會自動解壓縮
- `RUN` 用來執行你要在映像檔內的任何指令，例如 RUN pip install h5py、RUN chmod +x entrypoint.sh
- `ENV` 設定環境變數，例如 `ENV HDF5_USE_FILE_LOCKING FALSE`、`ENV NGINX_MAX_UPLOAD 4m`，這個值也可於docker run時將參數值帶入，進行替換
- `ARG` docker build時可將參數值帶入，進行替換
- `WORDDIR` 切換目前的工作目錄，例如 `WORKDIR /app`
- `EXPOSE` 設定要開放的PORT，但要docker run時也要下expose參數，例如 `EXPOSE 8080`
- `CMD` 啟動Container時要執行的指令，例如 `CMD ["python3", "main.py"]`。CMD不一定會被執行，若docker run 有帶指令(docker run [ContainerID] bash)，則會執行bash，而不執行CMD指令
- `ENTRYPOINT` 啟動Container時要執行的指令，例如 `ENTRYPOINT ["docker-entrypoint.sh"]`，此指令一定會被執行。
- `ONBUILD` 這個指令不會在建立時執行，而是別人將這一個Image設為基底Image時，才會執行的指令。執行的時機在FROM指令之後。指令例如 `ONBUILD RUN mkdir my_dir`

## 撰寫建議
### Avoid unnecessary privileges
1. Avoid running containers as root.
2. Don’t bind to a specific UID.
3. Make executables owned by root and not writable.
### Reduce attack surface
1. Leverage multistage builds.
2. Use distroless images, or build your own from scratch.
3. Update your images frequently.
4. Watch out for exposed ports.
### Prevent confidential data leaks
1. Never put secrets or credentials in Dockerfile instructions.
2. Prefer COPY over ADD.
3. Be aware of the Docker context, and use .dockerignore.
### others
1. Reduce the number of layers, and order them intelligently.
2. Add metadata and labels.
3. Leverage linters to automatize checks.
4. Scan your images locally during development.
### Beyond image building.
1. Protect the docker socket and TCP connections.
2. Sign your images, and verify them on runtime.
3. Avoid tag mutability.
4. Don’t run your environment as root.
5. Include a health check.
6. Restrict your application capabilities.


https://sysdig.com/blog/dockerfile-best-practices/
