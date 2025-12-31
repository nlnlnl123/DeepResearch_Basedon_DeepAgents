# deepsearch_BasedOn_DeepAgents  
* 基于DeepAgents实现DeepResearch，并生成报告md文件  
* 参考代码：https://github.com/langchain-ai/deepagents-quickstarts
* Agent的model选择至关重要，不要用次等的model,否则生成内容较差
* 功能：可直接生成报告research_outputs/final_report.md
* 系统环境变量配置好api-key,.env填上TAVILY_API_KEY的值
* 依赖：pip install --upgrade deepagents langchain-openai langchain-core langgraph tavily-python httpx markdownify python-dotenv
* 运行：python agent.py
