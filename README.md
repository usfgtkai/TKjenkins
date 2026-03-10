# 一小时Jenkins教程
这个仓库是一小时Jenkins教程的配套代码，旨在帮助初学者快速上手Jenkins持续集成和持续部署（CI/CD）实践。
各大平台的视频教程地址如下：

[Bilibili](https://www.bilibili.com/video/BV1Z2qYBSERP/)

[YouTube](https://youtu.be/GKLuQtni6Ok)

[小红书](http://xhslink.com/o/8ynyF07HoKK)


## 目录
- [一小时Jenkins教程](#一小时jenkins教程)
  - [目录](#目录)
  - [简介](#简介)
  - [安装Jenkins](#安装jenkins)
    - [Windows系统](#windows系统)
    - [macOS系统](#macos系统)
    - [Linux系统](#linux系统)
    - [使用Docker安装Jenkins](#使用docker安装jenkins)
    - [下载Jar包运行Jenkins](#下载jar包运行jenkins)
  - [配置Jenkins](#配置jenkins)
  - [Jenkins的目录结构](#jenkins的目录结构)
  - [插件管理](#插件管理)
  - [触发器Triggers](#触发器triggers)
  - [流水线（Pipeline）](#流水线pipeline)
    - [声明式流水线示例](#声明式流水线示例)
    - [脚本式流水线示例](#脚本式流水线示例)
  - [Jenkinsfile](#jenkinsfile)
  - [节点和代理配置](#节点和代理配置)

## 简介
Jenkins是一个开源的自动化服务器，广泛用于持续集成和持续部署。通过Jenkins，开发团队可以自动化构建、测试和部署流程，提高软件交付的效率和质量。

## 安装Jenkins

### Windows系统
1. 下载Jenkins的Windows安装包：[Jenkins下载页面](https://www.jenkins.io/download/)
2. 运行安装包，按照提示完成安装。

### macOS系统

可以使用Homebrew安装Jenkins：
```bash
# 安装Jenkins
brew install jenkins

# 启动Jenkins服务
brew services start jenkins
```

### Linux系统
参考官方安装文档，根据不同的Linux发行版选择合适的安装方法。：[Jenkins安装指南](https://www.jenkins.io/doc/book/installing/)


### 使用Docker安装Jenkins

如果你已经安装了Docker，可以通过以下命令快速启动Jenkins容器：
```bash
docker run \
  --detach \
  --restart always \
  --name jenkins \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume ~/docker/jenkins/jenkins_home:/var/jenkins_home \
  jenkins/jenkins
```

### 下载Jar包运行Jenkins
你也可以直接[下载Jenkins的War包](http://mirrors.jenkins.io/war-stable/latest/jenkins.war)并运行：
```bash
java -jar jenkins.war --httpPort=8080
```

## 配置Jenkins
安装完成后，访问Jenkins的Web界面: http://localhost:8080，按照提示进行初始配置，包括安装推荐插件和创建管理员用户。

## Jenkins的目录结构

Jenkins的主要目录结构如下：
- `JENKINS_HOME/`：Jenkins的主目录，存储所有配置文件、插件和构建数据。
  - `jobs/`：存储所有Jenkins任务的配置和数据。
  - `plugins/`：存储已安装的Jenkins插件。
  - `users/`：存储Jenkins用户的配置文件。
  - `secrets/`：存储Jenkins的安全相关信息，如凭据。
  - `config.xml`：Jenkins的全局配置文件。


## 插件管理
Jenkins拥有丰富的插件生态系统，安装时就会推荐一些常用插件。
但是由于网络的原因，安装经常会失败，
可以通过“管理Jenkins” -> “管理插件”页面，浏览和安装插件，也可以手动下载插件的.hpi文件，然后上传到Jenkins的plugins目录下，然后重启Jenkins服务就可以了。


## 触发器Triggers
触发器用于自动启动Jenkins流水线或任务。常见的触发器包括：
- 定时触发器（Crontab）
  ```bash
  H/5 * * * * # 表示每5分钟触发一次
  0 0 * * * # 表示每天午夜触发一次
  0 2 * * 1 # 表示每周一凌晨2点触发一次
  0 0 1 * * # 表示每月1日午夜触发一次
  0 0 1 1 * # 表示每年1月1日午夜触发一次
  0 9 * * 1-5 # 表示每个工作日（周一到周五）上午9点触发一次
  ```
- 代码变更触发器（如GitHub webhook）
  通常配置GitHub仓库的Webhook，指向Jenkins的特定URL，当代码有变更时自动触发构建。

- Poll SCM（定期检查代码仓库变更）
  Jenkins定期检查代码仓库的变更情况，如果有新的提交则触发构建。

- 构建完成触发器（基于其他任务的构建结果）
  可以配置某个任务在另一个任务成功完成后自动触发。

可以根据项目需求选择合适的触发器，实现自动化构建和部署流程。

## 流水线（Pipeline）
Jenkins流水线是一种用于定义和管理持续集成和持续部署流程的脚本化方式。流水线通常使用Groovy语言编写，支持声明式和脚本式两种语法。

### 声明式流水线示例
```groovy
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building...'
                // 构建步骤
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
                // 测试步骤
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                // 部署步骤
            }
        }
    }
}
```

### 脚本式流水线示例
```groovy
node {
    stage('Build') {
        echo 'Building...'
        // 构建步骤
    }
    stage('Test') {
        echo 'Testing...'
        // 测试步骤
    }
    stage('Deploy') {
        echo 'Deploying...'
        // 部署步骤
    }
}
```

## Jenkinsfile
Jenkinsfile是Jenkins流水线的定义文件，通常存储在代码仓库的根目录下。通过Jenkinsfile，可以将流水线配置与代码版本控制结合，实现持续集成和持续部署的自动化流程。

本工程中包含了一个示例Jenkinsfile，展示了如何定义一个完整的CI/CD流水线。

## 节点和代理配置

节点分为主节点（Master/Controller）和从节点（Agent）。主节点负责协调和管理构建任务，而从节点则执行具体的构建工作。可以根据项目需求配置多个从节点，以实现分布式构建和负载均衡。




