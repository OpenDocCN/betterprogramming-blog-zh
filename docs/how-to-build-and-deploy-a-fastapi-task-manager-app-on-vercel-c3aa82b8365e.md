# 如何在 Vercel 上构建和部署 FastAPI 任务管理器应用程序

> 原文：<https://betterprogramming.pub/how-to-build-and-deploy-a-fastapi-task-manager-app-on-vercel-c3aa82b8365e>

## **构建并部署 Postgres FastAPI 应用程序**

![](img/3f5f2b481c30fe8b54e53309c95880b6.png)

近年来，API 已经改变了软件的构建方式，允许更多可重用的代码与任何软件开发工具进行交互。现代的 API 已经有了定义好的标准(通常是 HTTP 和 REST ),这些标准对开发人员来说是友好的、容易获得的、被广泛理解的，这使得开发人员可以更容易地构建带有安全检查的可维护代码以及全面的文档。

[FastAPI](https://fastapi.tiangolo.com/) 是一个高性能的 Python web 框架，使用标准的 Python 类型提示创建 API，允许您轻松创建快速、直观、健壮的 web 应用程序，并且错误更少。此外，它还内置了对 API 文档的支持，由 Swagger 提供支持。

在本教程中，我们将通过创建一个简单的任务管理器应用程序来学习如何在 Vercel 上构建和部署 Postgres FastAPI 应用程序。为了跟进，确保您为这个项目克隆了 [Github](https://github.com/icode247/PostgresWithFastAPI) 库。我们开始吧！。

# 先决条件

本教程是一个实践演示。要继续操作，请确保您已经安装了以下软件:

*   [**Arctype**](https://arctype.com/)
*   [**Postgres 数据库**](https://www.postgresql.org/download/)
*   [**邮递员**](https://www.postman.com/downloads/)
*   [**巨蟒**](https://www.python.org/downloads/)
*   [**Heroku CLI**](https://devcenter.heroku.com/articles/heroku-cli)

# **什么是 Vercel？**

[**Vercel**](https://vercel.com/) **是一个云托管平台，被广泛认为是部署任何前端应用程序的最佳场所。Vercel 为其全球边缘网络提供了零配置部署的灵活性，无需费力即可实现动态应用可扩展性。**

**Vercel 结合了最卓越的开发体验和对最终用户效率的执着关注，以及一系列令人兴奋的特性，如:**

*   快速刷新:为您的 UI 组件提供可靠的实时编辑体验。
*   **多功能数据获取:将您的页面连接到任何数据源、headless CMS 或 API，以确保它们可以在每个人的开发环境中工作。**
*   localhost 完美:所有的云原语，从缓存到无服务器功能，都可以在 Localhost 上完美运行。

## **建立我们的项目**

在我们深入研究之前，让我们创建项目结构并安装应用程序所需的依赖项。我们将从创建项目文件夹开始。打开您的终端并运行以下命令:

```
**mkdir PostgresWithFastAPI && cd PostgresWithFastAPI
touch {main,database,model,schema,session}.py**
```

**在运行上面的命令并为我们的项目创建了一个虚拟环境(我们将在下一节中介绍)之后，我们的项目结构将如下所示:**

**📦┣邮政总局📂__pycache__
┣📂环境
┣📜database.py
┣📜main.py
┣📜model.py
┣📜requirements.txt
┣📜schema.py
┗📜session.py**

在整个教程中，我们将使用这些文件。

# **创建虚拟环境**

为你构建的 Python 项目创建一个虚拟环境总是好的。虚拟环境将包含项目的依赖项并隔离它们，从而保持项目整洁有序。我们将使用 `**virtualenv**` **:** 为这个项目创建一个虚拟环境

```
**pip install virtualenv**
```

**现在，通过运行以下命令创建并激活您的虚拟环境:**

```
**python3 -m venv env
source env/bin/activate**
```

我们已经成功地为项目创建了一个虚拟环境。我们还需要用下面的命令安装 Fastapi、Uvicorn、Sqlalchemy 和 psycopg 2-binary:

```
pip install fastapi uvicorn sqlalchemy psycopg2-binary
```

现在运行下面的命令，将我们的依赖关系保存在一个 *requirements.txt* 文件中。

```
pip freeze > requirements.txt
```

非常好。现在，让我们继续创建我们的 FastAPI 服务器。

# 创建 FastAPI 服务器

随着项目的建立，我们现在可以创建我们的 FastAPI 服务器了。首先，打开项目根目录中的`main.py`文件，并向其中添加以下代码:

```
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
def read_root():
    return {"message": "Server is up and running!"}
```

接下来，导航到终端上的项目根目录，并通过运行以下命令测试服务器:

```
uvicorn main:app --reload
```

我们添加到命令中的`— reload`标志告诉 FastAPI 关注我们代码库的更新，如果发现更新，就重新加载服务器。现在，向服务器发出一个 **Get 请求**，以确保一切都与 [Postman](https://www.postman.com/downloads/) 一起工作。

![](img/8a560b84dc49841ea7dd8036fd566729.png)

我们的服务器已经成功创建并运行。接下来，我们需要一个数据库来保存我们的用户记录。让我们开始设置一个。

# 设置 Postgres 数据库

我们现在可以设置我们的 Postgres 数据库，用我们的服务器设置来存储我们的用户记录。我们将使用 [SQLAlchemy ORM](https://www.sqlalchemy.org/) (对象关系映射器)来连接我们的数据库和应用程序。首先，我们需要通过以下步骤创建一个数据库。首先，切换到系统的 Postgres 用户帐户。

```
sudo su - postgresChanging to the Postgres user account
```

然后，创建一个新的用户帐户。然后，您可以按照下面的截图继续操作。

```
createuser --interactiveCreating a new user
```

![](img/bd6d3366957f1e0bd704ea146ec50b7f.png)

接下来，创建一个新的数据库。您可以使用以下命令来实现这一点:

```
createdb task
```

现在，我们将连接到刚刚创建的数据库。打开`database.py`文件，并在下面添加以下代码片段:

```
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
SQLALCHEMY_DATABASE_URL = "postgresql://postgres:1234@localhost:5432/task"
engine = create_engine(SQLALCHEMY_DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()
```

在上面的代码片段中，使用我们从`**SqlAlchemy**`导入的`**create_engine**`类连接到我们的数据库。我们还从`**sessionmaker**` 类中创建了一个`**Sessionlocal**` 实例。我们禁用了`**autocommit**`和`**autoflush**`，然后将数据库引擎绑定到会话。最后，我们从`**declarative_base**` 类创建了一个`**Base**`实例，我们将用它来创建应用程序的数据库模型和数据库连接。

# 创建我们的数据库模型

使用我们的 Postgres 数据库设置，让我们通过创建一个模型来定义如何存储用户的数据。打开`model.py`文件，并向其中添加以下代码片段。

```
from sqlalchemy.schema import Column
from sqlalchemy.types import String, Integer, Text
from database import Base

class Task(Base):
   __tablename__ = "Tasks"
   id = Column(Integer, primary_key=True, index=True)
   task_name = Column(String(20))
   task_des = Column(Text())
   created_by = Column(String(20))
   date_created = Column(String(15))
```

在上面的代码片段中，我们通过导入`Column`并传递我们期望为数据库中的每个字段存储的数据类型(`Integer`、`String()`和`Text()`)来定义我们的数据模型。

我们还导入了在我们的`database.py`文件中创建的`Base`实例，我们用它来创建我们的基本模型类。然后我们使用`__tablename__`属性设置我们的表名(`Tasks`)。为了区分存储在数据库表中的数据，我们向 id 字段添加了`primary_key`和`index`参数，并将其设置为 true。

# 创建我们的数据库模式

让我们为我们的应用程序定义一个模式。我们需要定义一个 [Pydantic](https://pydantic-docs.helpmanual.io/) 模式来读取数据并从 API 返回。打开`Schema.py`文件，并向其中添加以下代码片段:

```
from pydantic import BaseModel
from typing import Optional

class task_schema(BaseModel):
   task_name :str
   task_des :str
   created_by : Optional[str]= None
   date_created : Optional[str]= None

   class Config:
       orm_mode = True
```

在上面的代码片段中，我们定义了模型验证，这确保了来自客户端的数据与我们定义的字段具有相同的数据类型。我们将期待`task_name`和`task_des`字段的字符串值，以及`created_by`和`date_created`字段的可选字符串值。将`orm_mode`设置为 True 的子类 config 将指示 Pydantic 模型以字典和属性的形式读取数据。

# 创建我们的应用程序路线

创建好模式后，让我们定义应用程序的路由。首先，打开`session.py`文件并创建一个`create_get_session()`函数，用下面的代码片段创建并关闭会话我们的路线:

```
import model
from database import SessionLocal, engine

model.Base.metadata.create_all(bind=engine)
def create_get_session():
   try:
       db = SessionLocal()
       yield db
   finally:
       db.close()
```

在上面的代码片段中，我们通过调用`model.Base.metadata.create_all()`函数并将其绑定到我们的数据库引擎，使用我们在模型中定义的字段创建了我们的表。

然后，打开`main.py`文件，用下面的代码片段导入我们所有的模块:

```
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.orm import Session
from typing import List
from model import Task
from schema import task_schema
from session import get_database_session
…
```

接下来，使用下面的代码片段创建 **read_tasks** 路线:

```
…
@app.get("/task", response_model=List[task_schema], status_code=200)
async def read_tasks(db: Session = Depends(create_get_session)):
   tasks = db.query(Task).all()
   return tasks
…
```

在上面的代码片段中，我们创建了一个`read_tasks`路由来监听 GET 请求。我们传入了响应模型，该模型返回数据库中所有任务的列表和状态代码 200 (OK)。在我们的`read_tasks`函数中，引用了我们的模型会话，这将使我们能够在数据库中执行查询。

接下来，创建一个`create_task`路由，用下面的代码片段向我们的数据库添加新任务:

```
…
@app.post('/task', response_model = task_schema, status_code=201)
async def create_task(task: task_schema, db: Session = Depends(create_get_session)):
   new_task = Task(
        task_name = task.task_name,
        task_des = task.task_des,
        created_by =task.created_by,
        datecreated = task.date_created,
   )
   db.add(new_task)
   db.commit()

   return new_task
…
```

在上面的代码片段中，我们创建了一个`create_task`路由，它将侦听 POST 请求。这一次，我们的响应模型将返回刚刚创建的任务，状态代码为 201(已创建)。然后，我们通过传递一个项目并将其分配给我们的 Pydantic 模型，从请求体获取数据。此外，我们从模型类中创建了一个`new_task`对象，并将数据从请求体传递到模型中的字段。然后我们将`new_task`对象添加到我们的数据库会话中，提交它们并返回创建的对象。

接下来，我们创建`get_task`路由，它将返回一个任务，其 id 在请求参数中指定，代码片段如下:

```
…
@app.get("/task/{id}", response_model = task_schema, status_code=200)
async def get_task(id:int,db: Session = Depends(create_get_session)):
   task = db.query(Task).get(id)
   return task
…
```

在上面的代码片段中，我们创建了我们的`get_task`路由，它也将监听一个 GET 请求。但是这一次，我们将任务的 id 作为参数传入端点。我们的响应模型将返回一个状态代码为 200(OK)的 item 对象。然后，我们查询我们的数据库模型(Tasks ),以获取其 id 在请求参数中指定的任务，并将其返回给用户。

接下来，我们将创建我们的`update_task`路由，它将监听一个补丁请求，我们还将传入我们的响应模型，它将返回一个更新了状态代码 200 (OK)的任务对象。然后，我们在模型中查询其 id 在请求参数中指定的项目，重置任务的值，将其保存到数据库，刷新数据库，并将更新后的记录对象返回给用户。

```
…
@app.patch("/task/{id}", response_model = task_schema, status_code=200)
async def update_task(id:int, task:task_schema, db: Session = Depends(create_get_session)):
   db_task = db.query(Task).get(id)
   db_task.task_name = task.task_name
   db_task.task_des =  task.task_des
   db.commit()
   db.refresh(db_task)

   return db_task
…
```

最后，我们将创建`delete_task`路由，它将侦听一个删除请求，删除其 id 在请求参数中指定的任务，并返回一个状态代码 200 (OK)。我们将在数据库中查询该项，如果数据库中不存在该项，将引发 HTTPException 错误。然后传入状态代码 404(未找到)，并返回 null。

```
@app.delete('/task/{id}', status_code=200)
async def delete_task(id:int, db: Session = Depends(create_get_session)):
   db_task = db.query(Task).get(id)
   if not db_task:
       raise HTTPException(status_code="404",detail="Task id does not exist")

   db.delete(db_task)
   db.commit()

   return None
```

就这样，我们建立了所有的路线。让我们在本地测试我们的应用程序。

# 测试应用程序

有了我们的应用程序设置，让我们继续用 Postman 测试它。我们将从邮政路线开始。

![](img/d27ceeb06e4f6a34ff5ad4f59212a10d.png)

接下来，GET 路线。我们会用同样的方法测试它。

![](img/937202bc44c321b8e59d9e9b951e6436.png)

接下来，通过 id 获取路由。您应该会看到如下所示的结果。

![](img/f170a3b9c5a0f66ca21fe2fe326abd6f.png)

接下来是贴片路线。同样，正确的回答如下图所示。

![](img/165a596cfabcb54a90b4e4b00057cc45.png)

最后，**删除**路线。

![](img/e2c5672ddb8a0629c1687b09d1bf286d.png)

一切看起来都很好。现在，让我们将应用程序部署到 Vercel 上的云中。

# 部署到 Vercel

测试完我们的路线后，我们的应用程序就可以部署到 Vercel 的云中了。在部署应用程序之前，在 Heroku 上为项目[在这里](https://devcenter.heroku.com/articles/getting-started-with-python#provision-a-database)提供一个远程数据库，并在`database.py`文件中更新数据库连接字符串。

```
SQLALCHEMY_DATABASE_URL = “Remote connection string”
```

然后[在 Vercel 上注册一个账户](https://vercel.com/signup)，用下面的命令安装 Vercel CLI 工具:

```
//Ubuntu
sudo npm install vercel

//Windows
npm install -g vercel
```

安装完成后，使用以下命令登录到 Vercel CLI:

```
Vercel login
```

上面的命令将提示您输入想要登录的身份验证帐户。做出选择，然后按回车键。您将被重定向到浏览器中的一个新选项卡，如果您看到一条成功消息，则表明您成功登录了您的帐户。这意味着您现在可以从终端访问您的项目。

接下来，您需要一个配置文件来告诉 Vercel 在哪里可以找到您的`main`项目文件。在父目录中创建一个`vercel.json`文件，并添加以下 JSON 代码片段:

```
{
 "builds": [{ "src": "main.py", "use": "@vercel/python" }],
 "routes": [{ "src": "/(.*)", "dest": "main.py" }]
}
```

在上面的代码片段中，我们创建了一个对象，它指示了应用程序的`main`文件的路径。我们还说明了在`routes`对象中构建我们的应用程序时要使用的包。我们将所有路由指向`main.py`文件。

现在，我们将使用下面的命令初始化 Vercel:

```
Vercel .
```

上述命令将提示您填写项目详细信息。按照下面截图中的提示去做。

![](img/be138aafebaab003f5685b560511b4d2.png)

至此，我们的项目已经成功部署在 Vercel 上。你可以从[链接](https://fastapidemo.vercel.app/docs)中测试出来。

# 使用 arctype 可视化我们的数据

将我们的应用程序部署在 Vercel 上后，让我们通过将数据库连接到 Arctype 来可视化数据库中的记录。要开始，请确保您已经在您的机器上下载并安装了 [Arctype](/arctype.com) 。启动 Arctype 并单击 Postgres 来创建连接。如果您需要帮助，请点击此[链接](https://docs.arctype.com/connect/postgresql)。

![](img/3170c1bfff58c4021a4a4296a4da1d8a.png)

选择 **Postgres 后，**添加您的数据库凭证，点击**保存**按钮连接数据库。

![](img/bf579de2fda899d831800bdd2a4b6f72.png)

我们已经连接到我们的远程数据库。单击**任务**表，在 Arctype 的数据库上运行一些查询。您可以从 **Arctype** 直接对数据库执行 CRUD 操作，产生如下所示的输出。

![](img/66b3f2a1639bc5a11c43d054d7c0b1ef.png)

Arctype 是一个功能强大的 SQL 客户端，具有高级协作和数据可视化工具。请务必尝试找到研究应用程序数据的最佳方法。

# 结论

在本教程中，您已经学习了如何在 Vercel 上构建和部署 Postgres FastAPI 应用程序。我们从简要概述 Vercel 和 FastAPI 开始。然后我们创建了一个 FastAPI 服务器，设置了一个 Postgres 数据库，将应用程序连接到 Postgres，执行 CRUD 操作，用 Arctype 可视化数据。

现在您已经了解了这些知识，那么您打算如何构建下一个 FastAPI 应用程序呢？或许你可以从他们的网站上了解更多关于 [FastAPI](https://fastapi.tiangolo.com/) 和 [Arctype](https://arctype.com/) 的信息，并将你所学到的作为你下一个项目的灵感。请随时在 [Twitter](https://twitter.com/EkekentaZion) 上分享您的进展和问题。