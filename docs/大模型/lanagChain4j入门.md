

# 智能助手开发指南

## 前置知识

- Java基础
- Maven
- MySQL
- SSM
- SpringBoot

## 一、LangChain4j入门

### 1、简介

LangChain4j的目标是简化将大语言模型(LLM-Large Language Model)集成到Java应用程序中的过程。

#### 1.1、历史背景

- 2022年11月30日OpenAI发布了Chat GPT(GPT-3.5)
- 早在2022年10月， Harrison Chase发布了基于Python的LangChain。
- 随后同时包含了Python版和JavaScript(LangChain.js)版的LangChain也发布了。
- 2023年11月，Quarkus发布了LangChain4j的0.1版本，2025年2月发布了1.0-Beta1版本，4月发布了1.0-Beta3版本

官网: https://docs.langchain4j.dev

#### 1.2、主要功能

- 与大型语言模型和向量数据库的便捷交互
- 通过统一的应用程序编程接口(API)，可以轻松访问所有主要的商业和开源大型语言模型以及向量数据库，使你能够构建聊天机器人、智能助手等应用。
- 专为Java打造：借助Spring Boot集成，能够将大模型集成到Java应用程序中。大型语言模型与Java之间实现了双向集成:你可以从Java中调用大型语言模型，同时也允许大型语言模型反过来调用你的Java代码
- 智能代理、工具、检索增强生成(RAG)：为常见的大语言模型操作提供了广泛的工具，涵盖从底层的提示词模板创建、聊天记忆管理和输出解析，到智能代理和检索增强生成等高级模式。

#### 1.3、应用示例

1. 你想要实现一个自定义的由人工智能驱动的聊天机器人，它可以访问你的数据，并按照你期望的方式运行:
   - 客户支持聊天机器人，它可以:
     - 礼貌地回答客户问题
     - 处理/更改/取消订单
   - 教育助手，它可以:
     - 教授各种学科
     - 解释不清楚的部分
     - 评估用户的理解/知识水平
2. 你想要处理大量的非结构化数据(文件、网页等),并从中提取结构化信息。例如:
   - 从客户评价和支持聊天记录中提取有效评价
   - 从竞争对手的网站上提取有趣的信息
   - 从求职者的简历中提取有效信息
3. 你想要生成信息，例如:
   - 为你的每个客户量身定制的电子邮件
   - 为你的应用程序/网站生成内容: 博客文章、故事
4. 你想要转换信息，例如:
   - 总结
   - 校对和改写
   - 翻译

### 2、创建SpringBoot项目

#### 2.1、创建一个Maven项目

项目名称: `java-ai-langchain4j`

#### 2.2、添加SpringBoot相关依赖

在`pom.xml`的节点下填加如下依赖:

```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <spring-boot.version>3.2.6</spring-boot.version>
    <knife4j.version>4.3.0</knife4j.version>
    <langchain4j.version>1.0.0-beta3</langchain4j.version>
    <mybatis-plus.version>3.5.11</mybatis-plus.version>
</properties>

<dependencies>
    <!-- web应用程序核心依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--编写和运行测试用例-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    <!--前后端分离中的后端接口测试工具-->
    <dependency>
        <groupId>com.github.xiaoymin</groupId>
        <artifactId>knife4j-openapi3-jakarta-spring-boot-starter</artifactId>
        <version>${knife4j.version}</version>
    </dependency>
</dependencies>

<dependencyManagement>
    <dependencies>
        <!--引入SpringBoot依赖管理清单-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>${spring-boot.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

#### 2.3、创建配置文件

在`resources`下创建配置文件`application.properties`:

```properties
#web服务访问端口
server.port=8080
```

#### 2.4、创建启动类

```java
package com.java.ai.langchain4j;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class XzApp {
    public static void main(String[] args) {
        SpringApplication.run(XzApp.class, args);
    }
}
```

#### 2.5、启动启动类

访问 http://localhost:8080/doc.html查看程序能否成功运行

### 3、接入大模型

参考文档: Get Started

#### 3.1、LangChain4j库结构

LangChain4j具有模块化设计，包括:

1. `langchain4j-core`模块，它定义了核心抽象概念(如聊天语言模型和嵌入存储)及其API。
2. 主`langchain4j`模块，包含有用的工具，如文档加载器、聊天记忆实现，以及诸如人工智能服务等高层功能。
3. 大量的`langchain4j-{集成}`模块，每个模块都将各种大语言模型提供商和嵌入存储集成到LangChain4j中。你可以独立使用`langchain4j-{集成}`模块。如需更多功能，只需导入主`langchain4j`依赖项即可。

#### 3.2、添加LangChain4j相关依赖

在`pom.xml`中添加以下依赖:

### 3、接入大模型

参考文档: [Get Started]()

#### 3.1、LangChain4j库结构

LangChain4j具有模块化设计，包括:

1. `langchain4j-core`模块，它定义了核心抽象概念(如聊天语言模型和嵌入存储)及其API。
2. 主`langchain4j`模块，包含有用的工具，如文档加载器、聊天记忆实现，以及诸如人工智能服务等高层功能。
3. 大量的`langchain4j-{集成}`模块，每个模块都将各种大语言模型提供商和嵌入存储集成到LangChain4j中。你可以独立使用`langchain4j-{集成}`模块。如需更多功能，只需导入主`langchain4j`依赖项即可。

#### 3.2、添加LangChain4j相关依赖

在`pom.xml`中添加以下依赖:

```xml
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-open-ai</artifactId>
    <version>${langchain4j.version}</version>
</dependency>
```

#### 3.3、创建测试用例

接入任何一个大模型都需要先去申请apiKey。如果你暂时没有密钥，也可以使用LangChain4j提供的演示密钥，这个密钥是免费的，有使用配额限制且仅限于gpt-4o-mini模型。

```java
package com.java.ai.langchain4j;

import dev.langchain4j.model.chat.ChatLanguageModel;
import dev.langchain4j.model.openai.OpenAiChatModel;

public class ChatModelTest {
    public static void main(String[] args) {
        //创建大语言模型
        ChatLanguageModel model = OpenAiChatModel.builder()
                .apiKey("demo")
                .modelName("gpt-4o-mini")
                .build();

        //向模型提问
        String answer = model.chat("你好");
        //输出结果
        System.out.println(answer);
    }
}
```

### 4、SpringBoot整合

参考文档: [Spring Boot Integration](https://docs.langchain4j.dev/tutorials/spring-boot-integration/)

#### 4.1、替换依赖

将`langchain4j-open-ai`替换成`langchain4j-open-ai-spring-boot-starter`:

```xml
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-open-ai-spring-boot-starter</artifactId>
</dependency>
```

#### 4.2、配置模型参数

在`application.properties`中配置:

```properties
#langchain4j测试模型
langchain4j.open-ai.chat-model.api-key=demo
langchain4j.open-ai.chat-model.model-name=gpt-4o
#请求和响应日志
langchain4j.open-ai.chat-model.log-requests=true
langchain4j.open-ai.chat-model.log-responses=true
#启用日志debug级别
logging.level.root=debug
```

#### 4.3、创建测试用例

```java
@SpringBootTest
public class ChatModelTest {
    @Autowired
    private OpenAiChatModel openAiChatModel;

    @Test
    public void testSpringBoot() {
        //向模型提问
        String answer = openAiChatModel.chat("你好");
        //输出结果
        System.out.println(answer);
    }
}
```

## 二、接入其他大模型

### 1、都有哪些大模型

- 大语言模型排行榜: https://superclueai.com/
- SuperCLUE是由国内CLUE学术社区于2023年5月推出的中文通用大模型综合性评测基准。
- LangChain4j支持接入的大模型: https://docs.langchain4j.dev/integrations/language-models/

### 2、接入DeepSeek

#### 2.1、获取开发参数

访问官网: https://www.deepseek.com/注册账号，获取base_url和api_key，充值

#### 2.2、配置开发参数

为了apikey的安全，建议将其配置在服务器的环境变量中。变量名自定义即可，例如`DEEP_SEEK_API_KEY`

#### 2.3、配置模型参数

DeepSeek API文档: https://api-docs.deepseek.com/zh-cn/

在LangChain4j中, DeepSeek和GPT一样也使用了OpenAI的接口标准,因此也使用OpenAiChatModel进行接入

```properties
#DeepSeek
langchain4j.open-ai.chat-model.base-url=https://api.deepseek.com
langchain4j.open-ai.chat-model.api-key=${DEEP_SEEK_API_KEY}
#DeepSeek-V3
langchain4j.open-ai.chat-model.model-name=deepseek-chat
#DeepSeek-R1推理模型
#langchain4j.open-ai.chat-model.model-name=deepseek-reasoner
```

#### 2.4、测试

直接使用前面的测试用例即可

### 3、ollama本地部署

#### 3.1、为什么要本地部署

Ollama是一个本地部署大模型的工具。使用Ollama进行本地部署有以下多方面的原因:

- 数据隐私与安全:对于金融、医疗、法律等涉及大量敏感数据的行业，数据安全至关重要。
- 离线可用性:在网络不稳定或无法联网的环境中，本地部署的Ollama模型仍可正常运行。
- 降低成本:云服务通常按使用量收费，长期使用下来费用较高。而Ollama本地部署，只需一次性投入硬件成本，对于需要频繁使用大语言模型且对成本敏感的用户或企业来说，能有效节约成本。
- 部署流程简单:只需通过简单的命令"ollama run <模型名>"，就可以自动下载并运行所需的模型。
- 灵活扩展与定制:可对模型微调，以适配垂直领域需求。

#### 3.2、在ollama上部署DeepSeek

官网: https://ollama.com/

(1) 下载并安装ollama: OllamaSetup.exe

(2) 查看模型列表，选择要部署的模型，模型列表: https://ollama.com/search

(3) 执行命令: `ollama run deepseek-r1:1.5`运行大模型。如果是第一次运行则会先下载大模型

#### 3.3、常用命令

- `ollama list`: 查看已安装的模型
- `ollama run <模型名>`: 运行模型
- `ollama pull <模型名>`: 下载模型
- `ollama remove <模型名>`: 删除模型

#### 3.4、引入依赖

参考文档: [Ollama Integration](https://docs.langchain4j.dev/integrations/language-models/ollama/#get-started)

```xml
<!--接入ollama-->
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-ollama-spring-boot-starter</artifactId>
</dependency>
```

#### 3.5、配置模型参数

```properties
#ollama
langchain4j.ollama.chat-model.base-url=http://localhost:11434
langchain4j.ollama.chat-model.model-name=deepseek-r1:1.5b
langchain4j.ollama.chat-model.log-requests=true
langchain4j.ollama.chat-model.log-responses=true
```

#### 3.6、创建测试用例

```java
/**
 * ollama接入
 */
@Autowired
private OllamaChatModel ollamaChatModel;

@Test
public void testOllama() {
    //向模型提问
    String answer = ollamaChatModel.chat("你好");
    //输出结果
    System.out.println(answer);
}
```

### 4、接入阿里百炼平台

#### 4.1、什么是阿里百炼

- 阿里云百炼是2023年10月推出的。它集成了阿里的通义系列大模型和第三方大模型，涵盖文本、图像、音视频等不同模态。
- 功能优势:集成超百款大模型API，模型选择丰富;5-10分钟就能低代码快速构建智能体，应用构建高效;提供全链路模型训练、评估工具及全套应用开发工具，模型服务多元;在线部署可按需扩缩容，新用户有千万token免费送，业务落地成本低。
- 支持接入的模型列表: https://help.aliyun.com/zh/model-studio/models
- 模型广场: https://bailian.console.aliyun.com/?productCode=p_efm#/model-market

#### 4.2、申请免费体验

(1) 点击进入免费体验页面

(2) 点击免费体验

(3) 点击开通服务

(4) 确认开通

#### 4.3、配置apiKey

申请apiKey: https://bailian.console.aliyun.com/?apiKey=1&productCode=p_efm#/api-key

配置apiKey:配置在环境变量`DASH_SCOPE_API_KEY`中

LangChain4j参考文档: [DashScope Integration](https://docs.langchain4j.dev/integrations/language-models/dashscope/#plain-java)

```xml
<dependencies>
    <!--接入阿里云百炼平台-->
    <dependency>
        <groupId>dev.langchain4j</groupId>
        <artifactId>langchain4j-community-dashscope-spring-boot-starter</artifactId>
    </dependency>
</dependencies>

<dependencyManagement>
    <dependencies>
        <!--引入百炼依赖管理清单-->
        <dependency>
            <groupId>dev.langchain4j</groupId>
            <artifactId>langchain4j-community-bom</artifactId>
            <version>${langchain4j.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

#### 4.4、配置模型参数

```java
/**
 * 通义千问大模型
 */
@Autowired
private QwenChatModel qwenChatModel;

@Test
public void testDashScopeQwen() {
    //向模型提问
    String answer = qwenChatModel.chat("你好");
    //输出结果
    System.out.println(answer);
}
```

#### 4.5、测试通义千问

```java
/**
 * 通义千问大模型
 */
@Autowired
private QwenChatModel qwenChatModel;

@Test
public void testDashScopeQwen() {
    //向模型提问
    String answer = qwenChatModel.chat("你好");
    //输出结果
    System.out.println(answer);
}
```

#### 4.6、测试通义万象

生成图片测试:

```java
@Test
public void testDashScopeWanx() {
    WanxImageModel wanxImageModel = WanxImageModel.builder()
            .modelName("wanx2.1-t2i-plus")
            .apiKey(System.getenv("DASH_SCOPE_API_KEY"))
            .build();
    
    Response<Image> response = wanxImageModel.generate("奇幻森林精灵:在一片弥漫着轻柔薄雾的古老森林深处,阳光透过茂密枝叶洒下金色光斑。一位身材娇小、长着透明薄翼的精灵少女站在一朵硕大的蘑菇上。她有着海藻般的绿色长发,发间点缀着蓝色的小花,皮肤泛着珍珠般的微光。身上穿着由翠绿树叶和白色藤蔓编织而成的连衣裙,手中捧着一颗散发着柔和光芒的水晶球,周围环绕着五彩斑斓的蝴蝶,脚下是铺满苔藓的地面,蘑菇和蕨类植物丛生,营造出神秘而梦幻的氛围。");
    System.out.println(response.content().url());
}
```

#### 4.7、测试DeepSeek

也可以在阿里百炼上集成第三方大模型，如DeepSeek

将配置参数上的base-url参数指定到百炼平台，使用百炼上的大模型名称和apiKey即可

```properties
#集成百炼-deepseek
langchain4j.open-ai.chat-model.base-url=https://dashscope.aliyuncs.com/compatible-mode/v1
langchain4j.open-ai.chat-model.api-key=${DASH_SCOPE_API_KEY}
langchain4j.open-ai.chat-model.model-name=deepseek-v3
#温度系数:取值范围通常在0到1之间。值越高,模型的输出越随机、富有创造性;值越低,输出越确定、保守。这里设置为0.9,意味着模型会有一定的随机性,生成的回复可能会比较多样化。
langchain4j.open-ai.chat-model.temperature=0.9
```

使用之前的测试用例`testSpringBoot`测试即可

## 三、人工智能服务 AIService

### 1、什么是AIService

AIService使用面向接口和动态代理的方式完成程序的编写，更灵活的实现高级功能。

#### 1.1、链 Chain(旧版)

链的概念源自Python中的LangChain。其理念是针对每个常见的用例都设置一条链,比如聊天机器人、检索增强生成(RAG)等。链将多个底层组件组合起来,并协调它们之间的交互。链存在的主要问题是不灵活，我们不进行深入的研究。

#### 1.2、人工智能服务AIService

在LangChain4j中我们使用AIService完成复杂操作。底层组件将由AIService进行组装。

AI Service可处理最常见的操作:

- 为大语言模型格式化输入内容
- 解析大语言模型的输出结果

它们还支持更高级的功能:

- 聊天记忆 Chat memory
- 工具 Tools
- 检索增强生成RAG

### 2、创建AIService

#### 2.1、引入依赖

```xml
<!--langchain4j高级功能-->
<dependency>
    <groupId>dev.langchain4j</groupId>
    <artifactId>langchain4j-spring-boot-starter</artifactId>
</dependency>
```

#### 2.2、创建接口

```java
package com.java.ai.langchain4j.assistant;

public interface Assistant {
    String chat(String userMessage);
}
```

#### 2.3、测试用例

```java
@SpringBootTest
public class AIServiceTest {
    @Autowired
    private QwenChatModel qwenChatModel;
    
    @Test
    public void testChat() {
        //创建AIService
        Assistant assistant = AiServices.create(Assistant.class, qwenChatModel);
        //调用service的接口
        String answer = assistant.chat("Hello");
        System.out.println(answer);
    }
}
```

#### 2.4、@AiService

也可以在Assistant接口上添加@AiService注解

```java
package com.java.ai.langchain4j.assistant;

//因为我们在配置文件中同时配置了多个大语言模型,所以需要在这里明确指定(EXPLICIT)模型的beanName(qwenChatModel)
@AiService(wiringMode = EXPLICIT, chatModel = "qwenChatModel")
public interface Assistant {
    String chat(String userMessage);
}
```

测试用例中,我们可以直接注入Assistant对象

```java
@Autowired
private Assistant assistant;

@Test
public void testAssistant() {
    String answer = assistant.chat("Hello");
    System.out.println(answer);
}
```

#### 2.5、工作原理

AiServices会组装Assistant接口以及其他组件，并使用反射机制创建一个实现Assistant接口的代理对象。这个代理对象会处理输入和输出的所有转换工作。在这个例子中，chat方法的输入是一个字符串，但是大模型需要一个UserMessage对象。所以，代理对象将这个字符串转换为UserMessage，并调用聊天语言模型。chat方法的输出类型也是字符串，但是大模型返回的是AiMessage对象，代理对象会将其转换为字符串。

简单理解就是:代理对象的作用是输入转换和输出转换

## 四、聊天记忆Chat memory

### 1、测试对话是否有记忆

```java
@SpringBootTest
public class ChatMemoryTest {
    @Autowired
    private Assistant assistant;
    
    @Test
    public void testChatMemory() {
        String answer1 = assistant.chat("我是环环");
        System.out.println(answer1);
        
        String answer2 = assistant.chat("我是谁");
        System.out.println(answer2);
    }
}
```

很显然，目前的接入方式，大模型是没有记忆的。

### 2、聊天记忆的简单实现

可以使用下面的方式实现对话记忆。

```java
@Autowired
private QwenChatModel qwenChatModel;

@Test
public void testChatMemory2() {
    //第一轮对话
    UserMessage userMessage1 = UserMessage.userMessage("我是环环");
    ChatResponse chatResponse1 = qwenChatModel.chat(userMessage1);
    AiMessage aiMessage1 = chatResponse1.aiMessage();
    //输出大语言模型的回复
    System.out.println(aiMessage1.text());
    
    //第二轮对话
    UserMessage userMessage2 = UserMessage.userMessage("你知道我是谁吗");
    ChatResponse chatResponse2 = qwenChatModel.chat(Arrays.asList(userMessage1, aiMessage1, userMessage2));
    AiMessage aiMessage2 = chatResponse2.aiMessage();
    //输出大语言模型的回复
    System.out.println(aiMessage2.text());
}
```

### 3、使用ChatMemory实现聊天记忆

使用AIService可以封装多轮对话的复杂性,使聊天记忆功能的实现变得简单

```java
@Test
public void testChatMemory3() {
    //创建chatMemory
    MessageWindowChatMemory chatMemory = MessageWindowChatMemory.withMaxMessages(10);
    
    //创建AIService
    Assistant assistant = AiServices
            .builder(Assistant.class)
            .chatLanguageModel(qwenChatModel)
            .chatMemory(chatMemory)
            .build();
    
    //调用service的接口
    String answer1 = assistant.chat("我是环环");
    System.out.println(answer1);
    
    String answer2 = assistant.chat("我是谁");
    System.out.println(answer2);
}
```

### 4、使用AIService实现聊天记忆

#### 4.1、创建记忆对话智能体

当AIService由多个组件(大模型,聊天记忆,等)组成的时候,我们就可以称他为智能体了

```java
package com.java.ai.langchain4j.assistant;

@AiService(wiringMode = EXPLICIT, chatModel = "qwenChatModel", chatMemory = "chatMemory")
public interface MemoryChatAssistant {
    String chat(String message);
}
```

#### 4.2、配置ChatMemory

```java
package com.java.ai.langchain4j.config;

@Configuration
public class MemoryChatAssistantConfig {
    @Bean
    ChatMemory chatMemory() {
        //设置聊天记忆记录的message数量
        return MessageWindowChatMemory.withMaxMessages(10);
    }
}
```

#### 4.3、测试

```java
@Autowired
private MemoryChatAssistant memoryChatAssistant;

@Test
public void testChatMemory4() {
    String answer1 = memoryChatAssistant.chat("我是环环");
    System.out.println(answer1);
    
    String answer2 = memoryChatAssistant.chat("我是谁");
    System.out.println(answer2);
}
```

### 5、隔离聊天记忆

为每个用户的新聊天或者不同的用户区分聊天记忆

#### 5.1、创建记忆隔离对话智能体

```java
package com.java.ai.langchain4j.assistant;

@AiService(wiringMode = EXPLICIT, chatMemory = "chatMemory", chatMemoryProvider = "chatMemoryProvider")
public interface SeparateChatAssistant {
    /**
     * 分离聊天记录
     * @param memoryId 聊天id
     * @param userMessage
     * @return
     */
    String chat(@MemoryId int memoryId, @UserMessage String userMessage);
}
```

#### 5.2、配置ChatMemoryProvider

```java
package com.java.ai.langchain4j.config;

@Configuration
public class SeparateChatAssistantConfig {
    @Bean
    ChatMemoryProvider chatMemoryProvider() {
        return memoryId -> MessageWindowChatMemory.builder()
                .id(memoryId)
                .maxMessages(10)
                .build();
    }
}
```

#### 5.3、测试对话助手

用两个不同的memoryId测试聊天记忆的隔离效果

```java
@Autowired
private SeparateChatAssistant separateChatAssistant;

@Test
public void testChatMemory5() {
    String answer1 = separateChatAssistant.chat(1, "我是环环");
    System.out.println(answer1);
    
    String answer2 = separateChatAssistant.chat(1, "我是谁");
    System.out.println(answer2);
    
    String answer3 = separateChatAssistant.chat(2, "我是谁");
    System.out.println(answer3);
}
```

## 五、持久化聊天记忆 Persistence

默认情况下，聊天记忆存储在内存中。如果需要持久化存储，可以实现一个自定义的聊天记忆存储类，以便将聊天消息存储在你选择的任何持久化存储介质中。

### 1、存储介质的选择

大模型中聊天记忆的存储选择哪种数据库，需要综合考虑数据特点、应用场景和性能要求等因素，以下是一些常见的选择及其特点:

#### MySQL

- 特点:关系型数据库。支持事务处理，确保数据的一致性和完整性，适用于结构化数据的存储和查询。
- 适用场景:如果聊天记忆数据结构较为规整，例如包含固定的字段如对话ID、用户ID、时间戳、消息内容等，且需要进行复杂的查询和统计分析，如按用户统计对话次数、按时间范围查询特定对话等，MySQL是不错的选择。

#### Redis

- 特点:内存数据库，读写速度极高。它适用于存储热点数据，并且支持多种数据结构，如字符串、哈希表、列表等，方便对不同类型的聊天记忆数据进行处理。
- 适用场景:对于实时性要求极高的聊天应用，如在线客服系统或即时通讯工具，Redis可以快速存储和获取最新的聊天记录，以提供流畅的聊天体验。

#### MongoDB

- 特点:文档型数据库，数据以JSON-like的文档形式存储，具有高度的灵活性和可扩展性。它不需要预先定义严格的表结构，适合存储半结构化或非结构化的数据。
- 适用场景:当聊天记忆中包含多样化的信息，如文本消息、图片、语音等多媒体数据，或者消息格式可能会频繁变化时，MongoDB能很好地适应这种灵活性。例如，一些社交应用中用户可能会发送各种格式的消息，使用MongoDB可以方便地存储和管理这些不同类型的数据。

#### Cassandra

- 特点:是一种分布式的NoSQL数据库，具有高可扩展性和高可用性，能够处理大规模的分布式数据存储和读写请求。适合存储海量的、时间序列相关的数据。
- 适用场景:对于大型的聊天应用，尤其是用户量众多、聊天数据量巨大且需要分布式存储和理的场景，Cassandra能够有效地应对高

### 2、MongoDB

#### 2.1、简介

MongoDB是一个基于文档的NoSQL数据库，由MongoDB Inc.开发。

NoSQL，指的是非关系型的数据库。NoSQL有时也称作Not Only SQL的缩写，是对不同于传统的关系型数据库的数据库管理系统的统称。

MongoDB的设计理念是为了应对大数据量、高性能和灵活性需求。

MongoDB使用集合(Collections)来组织文档(Documents)，每个文档都是由键值对组成的。

- 数据库(Database):存储数据的容器，类似于关系型数据库中的数据库。
- 集合(Collection):数据库中的一个集合，类似于关系型数据库中的表。
- 文档(Document):集合中的一个数据记录，类似于关系型数据库中的行(row)，以BSON格式存储。

MongoDB将数据存储为一个文档，数据结构由键值(key=>value)对组成，文档类似于JSON对象，字段值可以包含其他文档，数组及文档数组。

#### 2.2、安装MongoDB

- 服务器: mongodb-windows-x86_64-8.0.6-signed.msi https://www.mongodb.com/try/download/community
- 命令行客户端: mongosh-2.5.0-win32-x64.zip https://www.mongodb.com/try/download/shell
- 图形客户端: mongodb-compass-1.39.3-win32-x64.exe https://www.mongodb.com/try/download/compass

#### 2.3、使用mongosh

启动MongoDB Shell:

在命令行中输入mongosh命令，启动MongoDB Shell，如果MongoDB服务器运行在本地默认端口(27017)，则可以直接连接。

```shell
mongosh
```

连接到MongoDB服务器:

如果MongoDB服务器运行在非默认端口或者远程服务器上,可以使用以下命令连接:

```
mongosh --host <hostname>:<port>
```

其中`<hostname>`是MongoDB服务器的主机名或IP地址,`<port>`是MongoDB服务器的端口号。

执行基本操作:

连接成功后，可以执行各种MongoDB数据库操作。例如:

- 查看当前数据库: `db`
- 显示数据库列表: `show dbs`
- 切换到指定数据库: `use <database_name>`
- 执行查询操作: `db.<collection_name>.find()`
- 插入文档: `db.<collection_name>.insertOne({...})`
- 更新文档: `db.<collection_name>.updateOne({...})`
- 删除文档: `db.<collection_name>.deleteOne({...})`
- 退出MongoDB Shell: `quit()`或者`exit`

CRUD操作示例:

```shell
# 插入文档
test> db.mycollection.insertOne({ name: "Alice", age: 30 })

# 查询文档
test> db.mycollection.find()

# 更新文档
test> db.mycollection.updateOne({ name: "Alice" }, {$set: { age: 31 }})

# 删除文档
test> db.mycollection.deleteOne({ name: "Alice" })

# 退出MongoDB Shell
test> quit()
```

#### 2.4、使用mongodb-compass

MongoDB Compass是MongoDB的官方图形界面工具，提供直观的数据浏览和管理功能。

#### 2.5、整合SpringBoot

引入MongoDB依赖:

```xml
<!--Spring Boot Starter Data MongoDB-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

#### 2.6、CRUD测试

创建实体类:映射MongoDB中的文档(相当于MySQL的表)

```java
package com.java.ai.langchain4j.bean;

@Data
@AllArgsConstructor
@NoArgsConstructor
@Document("chat_messages")
public class ChatMessages {
    //唯一标识,映射到MongoDB文档的_id字段
    @Id
    private ObjectId messageId;
    private String content; //存储当前聊天记录列表的json字符串
}
```

创建测试类:

```java
package com.java.ai.langchain4j;

@SpringBootTest
public class MongoCrudTest {
    @Autowired
    private MongoTemplate mongoTemplate;

    /**
     * 插入文档
     */
    @Test
    public void testInsert() {
        ChatMessages chatMessages = new ChatMessages();
        chatMessages.setContent("聊天记录列表");
        mongoTemplate.insert(chatMessages);
    }

    /**
     * 根据id查询文档
     */
    @Test
    public void testFindById() {
        ChatMessages chatMessages = mongoTemplate.findById("6801ead733ba9c4a0d9b6c7b", ChatMessages.class);
        System.out.println(chatMessages);
    }

    /**
     * 修改文档
     */
    @Test
    public void testUpdate() {
        Criteria criteria = Criteria.where("_id").is("6801ead733ba9c4a0d9b6c7b");
        Query query = new Query(criteria);
        Update update = new Update();
        update.set("content", "新的聊天记录列表");
        //修改或新增
        mongoTemplate.upsert(query, update, ChatMessages.class);
    }

    /**
     * 删除文档
     */
    @Test
    public void testDelete() {
        Criteria criteria = Criteria.where("_id").is("100");
        Query query = new Query(criteria);
        mongoTemplate.remove(query, ChatMessages.class);
    }
}
```

### 3、持久化聊天

#### 3.1、优化实体类

```java
package com.java.ai.langchain4j.bean;

@Data
@AllArgsConstructor
@NoArgsConstructor
@Document("chat_messages")
public class ChatMessages {
    //唯一标识，映射到MongoDB文档的_id字段
    @Id
    private ObjectId id;
    private int messageId;
    private String content; //存储当前聊天记录列表的json字符串
}
```

#### 3.2、创建持久化类

创建一个类实现ChatMemoryStore接口

```java
package com.java.ai.langchain4j.store;

@Component
public class MongoChatMemoryStore implements ChatMemoryStore {
    @Autowired
    private MongoTemplate mongoTemplate;

    @Override
    public List<ChatMessage> getMessages(Object memoryId) {
        Criteria criteria = Criteria.where("memoryId").is(memoryId);
        Query query = new Query(criteria);
        ChatMessages chatMessages = mongoTemplate.findOne(query, ChatMessages.class);
        if (chatMessages == null) return new LinkedList<>();
        return ChatMessageDeserializer.messagesFromJson(chatMessages.getContent());
    }

    @Override
    public void updateMessages(Object memoryId, List<ChatMessage> messages) {
        Criteria criteria = Criteria.where("memoryId").is(memoryId);
        Query query = new Query(criteria);
        Update update = new Update();
        update.set("content", ChatMessageSerializer.messagesToJson(messages));
        //根据query条件能查询出文档，则修改文档;否则新增文档
        mongoTemplate.upsert(query, update, ChatMessages.class);
    }

    @Override
    public void deleteMessages(Object memoryId) {
        Criteria criteria = Criteria.where("memoryId").is(memoryId);
        Query query = new Query(criteria);
        mongoTemplate.remove(query, ChatMessages.class);
    }
}
```

在SeparateChatAssistantConfig中，添加MongoChatMemoryStore对象的配置

```java
package com.java.ai.langchain4j.config;

@Configuration
public class SeparateChatAssistantConfig {
    //注入持久化对象
    @Autowired
    private MongoChatMemoryStore mongoChatMemoryStore;

    @Bean
    ChatMemoryProvider chatMemoryProvider() {
        return memoryId -> MessageWindowChatMemory.builder()
                .id(memoryId)
                .maxMessages(10)
                .chatMemoryStore(mongoChatMemoryStore) //配置持久化对象
                .build();
    }
}
```

### 4、测试

发现MongoDB中已经存储了会话记录。

## 六、提示词 Prompt

### 1、系统提示词

@SystemMessage设定角色，塑造AI助手的专业身份，明确助手的能力范围

#### 1.1、配置@SystemMessage

在SeparateChatAssistant类的chat方法上添加@SystemMessage注解

```java
@SystemMessage("你是我的好朋友,请用东北话回答问题。") //系统消息提示词
String chat(@MemoryId int memoryId, @UserMessage String userMessage);
```

@SystemMessage的内容将在后台转换为SystemMessage对象，并与UserMessage一起发送给大语言模型(LLM)。

SystemMessage的内容只会发送给大模型一次。

如果你修改了SystemMessage的内容，新的SystemMessage会被发送给大模型，之前的聊天记忆会失效。

#### 1.2、测试

```java
package com.java.ai.langchain4j;

@SpringBootTest
public class PromptTest {
    @Autowired
    private SeparateChatAssistant separateChatAssistant;

    @Test
    public void testSystemMessage() {
        String answer = separateChatAssistant.chat(3, "今天几号");
        System.out.println(answer);
    }
}
```

如果要显示今天的日期，我们需要在提示词中添加当前日期的占位符`{{current_date}}`

```java
@SystemMessage("你是我的好朋友,请用东北话回答问题。今天是{{current_date}}") //系统消息提示词
String chat(@MemoryId int memoryId, @UserMessage String userMessage);
```

#### 1.3、从资源中加载提示模板

@SystemMessage注解还可以从资源中加载提示模板:

```java
@SystemMessage(fromResource = "my-prompt-template.txt")
String chat(@MemoryId int memoryId, @UserMessage String userMessage);
```

创建`my-prompt-template.txt`文件:

```tex
你是我的好朋友，请用东北话回答问题，回答问题的时候适当添加表情符号。
{{current_date}}表示当前日期
你是我的好朋友，请用东北话回答问题，回答问题的时候适当添加表情符号。
今天是{{current_date}}。
```

### 2、用户提示词模板

@UserMessage:获取用户输入

#### 2.1、配置@UserMessage

在MemoryChatAssistant的chat方法中添加注解

```java
@UserMessage("你是我的好朋友，请用上海话回答问题，并且添加一些表情符号。{{it}}") //{{it}}表示这里唯一的参数的占位符
String chat(String message);
```

#### 2.2、测试

```java
@Autowired
private MemoryChatAssistant memoryChatAssistant;

@Test
public void testUserMessage() {
    String answer = memoryChatAssistant.chat("我是环环");
    System.out.println(answer);
}
```

### 3、指定参数名称

#### 3.1、配置@V

@V明确指定传递的参数名称

```java
@UserMessage("你是我的好朋友,请用上海话回答问题,并且添加一些表情符号。{{message}}")
String chat(@V("message") String userMessage);
```

#### 3.2、多个参数的情况

如果有两个或两个以上的参数,我们必须要用@V,在SeparateChatAssistant中定义方法chat2

```java
@UserMessage("你是我的好朋友,请用粤语回答问题。{{message}}")
String chat2(@MemoryId int memoryId, @V("message") String userMessage);
```

测试:@UserMessage中的内容每次都会被和用户问题组织在一起发送给大模型

```javajava
@Test
public void testV() {
    String answer1 = separateChatAssistant.chat2(1, "我是环环");
    System.out.println(answer1);
    
    String answer2 = separateChatAssistant.chat2(1, "我是谁");
    System.out.println(answer2);
}
```

#### 3.3、@SystemMessage和@V

也可以将@SystemMessage和@V结合使用

在SeparateChatAssistant中添加方法chat3

```java
@SystemMessage(fromResource = "my-prompt-template3.txt")
String chat3(@MemoryId int memoryId, @UserMessage String userMessage, 
             @V("username") String username, @V("age") int age);
```

创建提示词模板`my-prompt-template3.txt`，添加占位符

```tex
你是我的好朋友,我是{{username}},我的年龄是{{age}},请用东北话回答问题,回答问题的时候适当添加表情符号。
今天是{{current_date}}。
```

测试:

```java
@Test
public void testUserInfo() {
    String answer = separateChatAssistant.chat3(1, "我是谁,我多大了", "翠花", 18);
    System.out.println(answer);
}
```