
## langchain\_chatchat\+ollama部署本地知识库，联网查询以及对数据库(Oracle)数据进行查询


**涉及的内容其实挺多的，所以尽量减少篇幅**


目录* [langchain\_chatchat\+ollama部署本地知识库，联网查询以及对数据库(Oracle)数据进行查询](https://github.com)
	+ [准备工作：](https://github.com)
		- [部署ollama以及拉取模型](https://github.com)
		- [部署langchain\_chatchat](https://github.com)
		- [部署oracle数据库](https://github.com)
	+ [对langchain\-chatchat的配置文件初步调整：](https://github.com)
	+ [langchain\-chatchat执行：](https://github.com)
	+ [langchain\-chatchat简单操作：](https://github.com):[veee加速器](https://liuyunzhuge.com)
	+ [langchain\-chatchat联网查询：](https://github.com)
	+ [langchain\-chatchat连接oracle数据库并查询内容：](https://github.com)
		- [重要的！！！最重要的！！！](https://github.com)

### 准备工作：


部署ollama，并拉取qwen2\.5:14b和quentinz/bge\-large\-zh\-v1\.5:latest
部署langchain\_chatchat
部署oracle数据库


#### 部署ollama以及拉取模型


可以参考下面的文章：
[https://github.com/jokingremarks/p/18151827](https://github.com)


#### 部署langchain\_chatchat


Langchain\_chatchat的github路径：[https://github.com/chatchat\-space/Langchain\-Chatchat](https://github.com)


使用vscode快速创建一个venv虚拟环境管理工具
![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116165202670-540799195.png)
![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116165228257-1764010852.png)


在当前环境下直接下载Langchain\-Chatchat的python库


**注意：这个只能在Python 3\.8\-3\.11的环境下，不然会报错**


Langchain\-Chatchat 提供以 Python 库形式的安装方式，具体安装请执行：
`pip install langchain-chatchat -U`


如果要用Xinference接入Langchain\-Chatchat，建议使用如下安装方式：
`pip install "langchain-chatchat[xinference]" -U`


**本文使用ollama作为本地模型的调用，所以不需要装Xinference**


#### 部署oracle数据库


这里我是直接下载到了本地，使用的版本是Oracle 19c，安装教程网上大把，记得创建一个数据库，**我这里数据库名字是orcl**


### 对langchain\-chatchat的配置文件初步调整：


首先先调整model\_settings.yaml
DEFAULT\_LLM\_MODEL和DEFAULT\_EMBEDDING\_MODEL，将其替换成ollama下载下来的模型名，这里我们使用qwen2\.5:14b作为LLM，使用quentinz/bge\-large\-zh\-v1\.5:latest作为Embedding



```
# 默认选用的 LLM 名称
DEFAULT_LLM_MODEL: qwen2.5:14b

# 默认选用的 Embedding 名称
DEFAULT_EMBEDDING_MODEL: quentinz/bge-large-zh-v1.5:latest

```

![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116165351042-1214721464.png)


MODEL\_PLATFORMS部分只保留ollama，同时修改内容



```
llm_models:
      - qwen2.5:14b
embed_models:
      - quentinz/bge-large-zh-v1.5:latest

```

![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116165816296-1533225472.png)


### langchain\-chatchat执行：


详细内容可以查看文档：[https://github.com/chatchat\-space/Langchain\-Chatchat](https://github.com)


其实就三步


**执行初始化**


`chatchat init`


**初始化知识库**


`chatchat kb -r`


**启动项目**


`chatchat start -a`


一般会自动跳到浏览器里面，地址为http://127\.0\.0\.1:8501/


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116170414138-819929133.png)


### langchain\-chatchat简单操作：


模型对话，就是最基础的对话操作，启用agent的时候可以选择不同的工具来进行对话


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116170605605-305435526.png)


RAG对话，可以选择不同的场景进行对话，其中有知识库问答，文件对话和搜索引擎问答


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116170702062-1196962240.png)


知识库问答就是使用项目路径下的文件内容回答，会有些自带的文件在里面，可以自己上传


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116171044848-110282201.png)


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116170905150-366298045.png)


文件对话就是基于上传的文件内容进行问答


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116171236178-721382266.png)


搜索引擎对话后面会有补充，需要对配置文件再进行调整


知识库管理，即对项目中的内部知识库进行增删知识库以及重建向量库


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116170447261-1556973202.png)


### langchain\-chatchat联网查询：


如果使用duckduckgo作为搜索引擎的话可能需要FQ，这个就自行解决了


先安装duckduckgo\-search


`pip install -U duckduckgo-search`


将tool\_settings.yaml中的search\_internet的search\_engine\_name设置成duckduckgo


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116172116444-944291439.png)


如果要查询天气或者地图相关的，可以增加用高德地图的配置，api可以直接去高德申请，比较容易


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116171741221-1976154157.png)


将kb\_settings.yaml中的DEFAULT\_SEARCH\_ENGINE也修改成duckduckgo


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116172130286-1205097539.png)


重新加载项目以后，就可以使用搜索引擎对话了


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116172154354-1341286726.png)


### langchain\-chatchat连接oracle数据库并查询内容：


官方文档：[https://github.com/chatchat\-space/Langchain\-Chatchat/blob/master/docs/install/README\_text2sql.md](https://github.com)


首先我们找到tool\_settings.yaml中的text2sql进行修改


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116172425905-515829668.png)


有几个需要注意的地方


oracle的连接我使用的是oracledb，**所以需要安装oracledb**


`python -m pip install oracledb`


table\_comments是一些提示用的，如果发现模型形成的sql老是找不对表或者字段，就在里面说明下，准确率会大幅提高


#### 重要的！！！最重要的！！！


因为oracle的语法比较特殊，所以要对langchain的源码进行修改


找到项目中的/envs/chat\_0\.3\.1/lib/python3\.11/site\-packages/langchain\_experimental/sql/base.py


在其中对SQL进行一些处理，目前我遇到的情况有如下的，都需要重新分割处理才行



```
if "sql" in sql_cmd:
            sql_cmd = sql_cmd.split("sql")[-1].strip() # 增加的sql过滤，按照sql分割，取后一段，为了去掉```sql的开头
if "`" in sql_cmd:
            sql_cmd = sql_cmd.split("`")[0].strip() # 增加的sql过滤，按照sql分割，取后一段，为了去掉```的结尾
if "LIMIT" in sql_cmd:
            sql_cmd = sql_cmd.split("LIMIT")[0].strip() # 增加的sql过滤，按照sql分割，取后一段，为了去掉LIMIT

```

![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116173731583-190887545.png)


然后重新运行项目，选择启用agent并选择数据库对话，输入要搜索的东西，终端里面可以看到对应的sql以及查询结果


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116174625118-1250457920.png)


可以看到回答的和数据库中查询的内容一致


![image](https://img2024.cnblogs.com/blog/1672923/202411/1672923-20241116174353663-1378100886.png)


不过对Oracle数据库好像不是很友好，有时候还是会有一些奇怪的报错


以上


