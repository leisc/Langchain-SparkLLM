# Langchain-SparkLLM
Leveraging LangChain to call Xunfei SparkLLM to serve multi-round conversation

讯飞星火大模型(SparkLLM)是一款非常出色的大语言模型，为开发者开放了在线API测试服务。此程序库旨在提供一个简单的原型供开发者朋友们快速建立一个验证系统，并以此为基础进行扩展，在真实应用中得以运用。

源程序分为两部分：

后端API接口服务，以FastAPI框架实现的API接口来实现对SparkLLM的调用（包括鉴权和推理）。源程序位于src/LLMService

客户端，基于LangChain框架为讯飞星火大模型定制了一个LLM类（SparkLLM），使得客户端可以应用LangChain来集成讯飞星火大模型服务；另外，引入了LangChain的会话记忆类为客户端与SparkLLM多轮对话提供支持。源程序位于src/client

部署：

如果仅以原型验证为目的，建议把后端API接口服务和客户端部署在同一个系统中。

后端API接口服务，可以安装fastapi和uvicorn，并执行uvicorn main:app --reload命令运行服务，默认端口在本机8000。

进入客户端目录，运行python app.py执行，在命令行窗口可看到每轮会话过程中的会话记忆变化和LLM返回的信息

讯飞星火的API接入免费试用

这是非常关键的一步。去讯飞星火API接入试用网页（https://xinghuo.xfyun.cn/sparkapi），申请免费试用，可以取得1.5和2.0版各200万token的试用额度。根据网站指引进行认证并创建应用后，取得APPID, APISecret和APIKey，复制这些信息到src/LLMService/main.py的对应位置，才可以通过API访问鉴权。

在应用到真实系统时需要考虑：

1）各种hard code配置化，如endpoint_url, APPID, APISecret和APIKey

2）调用LLM时的参数，如temperature, max_token, top_p

3）会话记忆的管理：存储多少轮的会话，存储多久，如何判断此话题的会话已经结束，是否需要在会话过程中抽取实体和关系，是否需要对会话进行总结摘要，是否需要持久化 

4）后端API接口服务单独部署并开启鉴权认证