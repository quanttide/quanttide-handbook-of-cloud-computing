---
author: 张果
created_date: 2021-08-16
updated_date: 2022-03-25
---

# 函数计算项目框架

本文通常在**创建项目**时使用，在后续开发过程中一般不需要再改动项目结构，具体文件配置通常在后续开发过程中需要调整。

## 文件结构

这里使用Markdown格式表达的项目文件和文件夹结构，其中`<dir_name>/`表示文件夹。

```markdown
- README.md
- serverless.yml
- install_scf_requirements.py (recommended)
- requirements.txt (optional)
- .env (ignored)
- .gitignore
- <scf>/
  - index.py
  - requirements.txt (recommended)
  - serverless.yml
  - <module>.py (optional)
  - <package> (optional)
    - __init__.py 
    - <module>.py 
- db/ (recommended)
  - serverless.yml
- cos/ (recommended)
  - serverless.yml 
- workflow/ (recommended)
  - workflow.json
  - serverless.yml
- docs/
  - README.md
  - <section>/ (recommended)
    - <doc>.md
  - <doc>.md (recommended)
```

注意：

- recommended标记表示尽量有或者大部分情况会有，建议开发者处理；optional表示可选，不需要可以不用；不标记的为必须。
- 云函数（`<scf>/`文件夹）可以有单个也可以有多个。因此此项目规范实际上不像官方文档给的建议一样区分单项目和多项目。
- 每个云函数下可以有0-N个Python模块或包（`<module>.py`文件和`<package>`文件夹），根据项目需要决定即可。
- 文档文件夹下可以有多个文件夹(`<section>`/)和文件（<doc>.md），用Markdown格式写即可。

## 根目录`serverless.yml`

根目录的配置通常为

```yaml
- app: <project_name>
- stage: ${env:STAGE}
```

其中`<project_name>`是项目名称，在云资源配置文件中以`${app}`的形式被使用以命名函数计算，通常加在名称开头区分项目资源。

`${env:<ENV_VARIABLE>}`是框架规定的传入环境变量的格式，`STAGE`为框架规定的环境标识，类似于自建标准中的`ENV`。在云资源配置文件中以`${stage}`的形式被使用以命名函数计算，通常加在名称末尾区分环境资源。

默认地域、私有网络等项目级别的参数在云资源配置文件设置，直接在`inputs`下配置以`${env:<ENV_VARIABLE>}`的格式制定，同时需要通过环境变量传入以上变量。这样做的原因是，`inputs`下的参数依赖于`component`配置（指定云函数、工作流等），在根目录配置文件设置的的`inputs`的子配置项在云资源配置文件**无法使用**；设置在环境变量里是为了方便调整不同环境的配置，同时由于被Git过滤也可以**保证安全**。

## 环境变量

`.env`文件（也可以是`*.env`或`.env.*`）。

环境变量在本地项目配置并加入`.gitignore`。上述团队模板在“部署”步骤从构建环境的环境变量生成临时`.env`，需要开发者维护各个项目的构建计划指定写入环境变量文件的具体环境变量，操作方法在后面介绍。

请开发者特别注意**不要把环境变量文件传入Git**！通常环境变量中存放的变量是项目的敏感变量，比如数据库密码、API密钥等等。在代码审核或后续上线中出现此类问题应当被视为**工程事故**。因此，请开发者特别注意检查环境变量文件是否被过滤，本地开发时IDE通常有专门的颜色提示被Git过滤的文件，第一次提交时请确认环境变量文件没有被上传，否则应当根据第二章相关内容及时删除Git历史。

## Git配置

`.gitignore`文件配置参考[.gitignore文件规范](../2_General_Principles_for_DevOps/2_2_z_gitignore.md)
