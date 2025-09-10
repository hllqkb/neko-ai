# Persistent-memory-Neko

一个具有持久记忆功能的智能AI助手API服务，基于FastAPI和Neo4j图数据库。

## 功能特点

使用Neo4j图数据库和FAISS向量数据库存储对话历史
使用向量相似度查找相关记忆
基于图数据库的关系分析，提供更好的上下文理解
基于FastAPI的高性能API服务
API密钥验证机制保障服务安全
通过YAML/JSON配置文件灵活配置服务参数

## 系统架构

```
                                    +-----------------+
                                    |                 |
                      +------------>+  FastAPI 服务   +<-----------+
                      |             |                 |            |
                      |             +-----------------+            |
                      |                     |                      |
                      |                     |                      |
                      |                     v                      |
                      |             +-----------------+            |
                      |             |                 |            |
               +------+------+      |  记忆处理服务    |     +------+------+
               |             |      |                 |     |             |
               |  Neo4j 图   +<---->+                 +<--->+  FAISS 向量  |
               |  数据库     |      |                 |     |  数据库      |
               |             |      +-----------------+     |             |
               +-------------+                              +-------------+
```

## 环境要求

- Python 3.10+
- Neo4j 4.4+
- 足够的存储空间用于向量数据库

## 快速开始

1. 克隆仓库
```bash
git clone https://github.com/yourusername/Persistent-memory-Neko.git
cd Persistent-memory-Neko
```

2. 创建虚拟环境并安装依赖
```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
/Users/hllqk/neko-ai-app/.venv/bin/python /Users/hllqk/neko-ai-app/main.py
```

3. 配置Neo4j数据库
   - 安装并启动Neo4j服务
   - 创建数据库并设置用户名密码
   - 在配置文件中更新数据库连接信息

4. 配置API服务
   - 复制`config.yaml.example`为`config.yaml`
   - 根据需要修改配置参数

5. 启动服务
```bash
python run.py
```

6. 访问API文档
   - 在浏览器中打开 http://localhost:8000/docs

## 配置说明

配置文件支持YAML和JSON两种格式，优先读取YAML格式。主要配置项包括：

### 基本设置
```yaml
app:
  name: "Persistent-memory-Neko"
  version: "1.0.0"
  description: "具有持久记忆功能的智能AI助手"
  debug: false
```

### API设置
```yaml
api:
  host: "0.0.0.0"
  port: 8000
  api_key: "your-api-key"
  timeout: 60
```

### 模型设置
```yaml
model:
  name: "gpt-3.5-turbo"
  temperature: 0.7
  max_tokens: 1000
  api_key: "your-openai-api-key"
```

### 数据库设置
```yaml
database:
  neo4j:
    uri: "bolt://localhost:7687"
    user: "neo4j"
    password: "password"
    database: "memory"
  
  faiss:
    index_path: "data/faiss/memory_index"
    dimension: 1536
```

### 记忆设置
```yaml
memory:
  similarity_threshold: 0.6
  max_related_memories: 5
  max_context_memories: 10
  ttl_days: 30  # 记忆保留天数
```

详细配置项请参考`config.yaml.example`中的注释说明。

## 项目结构

```
/
├── api/                # API路由
│   ├── endpoints/      # 具体端点实现
│   │   ├── chat.py     # 聊天相关API
│   │   ├── memory.py   # 记忆相关API
│   │   └── system.py   # 系统相关API
│   └── router.py       # 路由注册
├── core/               # 核心功能
│   ├── config.py       # 配置管理
│   ├── embedding.py    # 嵌入向量处理
│   └── memory_store.py # 记忆存储核心
├── db/                 # 数据库访问
│   └── neo4j_store.py  # Neo4j存储实现
├── models/             # 数据模型
│   ├── chat.py         # 聊天相关模型
│   └── memory.py       # 记忆相关模型
├── services/           # 业务服务
│   ├── chat_service.py # 聊天服务
│   └── memory_service.py # 记忆服务
├── utils/              # 工具函数
│   ├── logger.py       # 日志工具
│   └── text.py         # 文本处理工具
├── data/               # 数据存储
│   ├── faiss/          # FAISS向量索引
│   └── backups/        # 系统备份
├── logs/               # 日志文件
├── main.py             # 应用主文件
├── run.py              # 启动脚本
├── config.yaml         # 配置文件
├── config.yaml.example # 配置文件示例
├── requirements.txt    # 依赖列表
└── README.md           # 项目说明文档
```

## 贡献指南

我们欢迎任何形式的贡献，包括但不限于：

- 报告问题或提出建议
- 改进文档
- 提交代码修复或新功能

贡献步骤：

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交你的更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建一个 Pull Request

## 常见问题

### Q: 如何更改服务端口？
A: 在配置文件中修改 `api.port` 值。

### Q: 如何备份全部记忆数据？
A: 使用系统API的备份功能 `POST /api/system/backup`，或直接复制 Neo4j 数据库和 FAISS 索引文件。

### Q: 能否使用其他向量数据库代替 FAISS？
A: 可以，你可以实现自己的向量存储适配器在 `db/` 目录中。

## 许可证

MIT License

## 联系方式

- 项目维护者：hllqkb
- 电子邮件：hllqkb@gmail.com
- 项目主页：https://github.com/hllqkb/neko-ai-app

---

<p align="center">
Made with ❤️ for AI assistants with memory
</p>

```bash
python run.py
```
or 
```bash
cd /Users/hllqk/Persistent-memory-Neko && .venv/bin/python app/run.py
```

