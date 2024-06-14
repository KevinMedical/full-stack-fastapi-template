# full-stack-fastapi-template本地源码部署，切换mysql
## 安装poetry
https://python-poetry.org/#/
```shell
pip install pipx # 通过pipx安装
pipx install poetry
```
## 使用poetry安装依赖
```shell
cd ./backend
poetry install
poetry shell
```
## 复制环境配置文件
```shell
cp ../.env .
```
## mysql相关配置
- 安装pymysql
```shell
pip install pymysql
````
- 编辑.env app/core/db.py app/alembic/env.py 修改mysql相关配置

```python
# .env
# 添加mysql配置信息
MYSQL_SERVER=192.168.8.26
MYSQL_PORT=3306
MYSQL_DB=fastapi
MYSQL_USER=root
MYSQL_PASSWORD=xxx

# app/core/db.py
# 修改连接为mysql
# engine = create_engine(str(settings.SQLALCHEMY_DATABASE_URI)) 替换为mysql
engine = create_engine("mysql+pymysql://root:xxxx@192.168.8.26:3306/fastapi")

# app/alembic/env.py
# def get_url():
#     user = os.getenv("POSTGRES_USER", "postgres")
#     password = os.getenv("POSTGRES_PASSWORD", "")
#     server = os.getenv("POSTGRES_SERVER", "db")
#     port = os.getenv("POSTGRES_PORT", "5432")
#     db = os.getenv("POSTGRES_DB", "app")
#     return f"postgresql+psycopg://{user}:{password}@{server}:{port}/{db}"
def get_url():
    user = os.getenv("MYSQL_USER", "root")
    password = os.getenv("MYSQL_PASSWORD", "xxx")
    server = os.getenv("MYSQL_SERVER", "192.168.8.26")
    port = os.getenv("MYSQL_PORT", "3306")
    db = os.getenv("MYSQL_DB", "fastapi")
    # print(f"mysql+pymysql://{user}:{password}@{server}:{port}/{db}")
    return f"mysql+pymysql://{user}:{password}@{server}:{port}/{db}"
```

## 初始化项目
```shell
poetry run python app/backend_pre_start.py
poetry run alembic upgrade head
poetry run python app/initial_data.py
```

## 运行
```shell
poetry run uvicorn app.main:app --reload
```

## poetry转pip
https://zhuanlan.zhihu.com/p/698728461
```shell
poetry export -f requirements.txt > requirements.txt
pip install -r requirements.txt
uvicorn app.main:app --reload
```