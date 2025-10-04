# 使用 Spring Boot 和 Spring Cloud 的 Java 微服务 🍃☁️

> 本项目基于 [java-microservices-examples](https://github.com/oktadev/java-microservices-examples) 搬运，遵循其开源协议（Apache-2.0）。

> 微服务示例集合，用 Spring Boot + Spring Cloud + Gateway 等。适合学习微服务架构、Service Discovery、配置中心、API 网关等。

本仓库包含了如何使用 Spring Boot、Spring Cloud 和 Netflix Eureka 构建 Java 微服务架构的示例。

本仓库中有五个示例：

1. 一个基础微服务架构，包含 Spring Boot、Spring Cloud、Eureka Server 和 Zuul。
2. 一个由 JHipster 生成，并通过 Spring Cloud Config 集中配置的微服务架构。
3. 一个使用 Spring Cloud Gateway 和 Spring WebFlux 展示响应式微服务的架构。
4. 一个由 JHipster 生成的响应式微服务架构，结合 Spring Cloud Gateway 和 Spring WebFlux。
5. 一个 JHipster 7 + Kubernetes 的示例，使用密封的 secret 部署到 Google Cloud。

我们相信你会喜欢它们！

1. 查看 [使用 Spring Boot 和 Spring Cloud 的 Java 微服务][blog-spring-boot-spring-cloud] 以了解第一个示例的概览。
2. 阅读 [使用 Spring Cloud Config 和 JHipster 的 Java 微服务][blog-spring-cloud-config] 了解 JHipster 的微服务。
3. 参考 [使用 Spring Cloud Gateway 的安全响应式微服务][blog-spring-cloud-gateway] 学习 Spring Cloud Gateway 和响应式微服务。
4. 参考 [使用 Spring Boot 和 JHipster 的响应式 Java 微服务][blog-reactive-jhipster] 查看 JHipster 如何简化响应式微服务。
5. 阅读 [使用 Spring Boot 和 JHipster 将 Kubernetes 部署到云端][blog-k8s] 学习 JHipster 如何简化 Kubernetes 部署。

**先决条件：** [Java 11](https://sdkman.io/sdks#java) 和互联网连接。

* [Spring Boot + Spring Cloud 示例](#spring-boot--spring-cloud-example)
* [JHipster + Spring Cloud Config 示例](#jhipster--spring-cloud-config-example)
* [Spring Cloud Gateway 示例](#spring-cloud-gateway-example)
* [响应式 JHipster 微服务示例](#reactive-microservices-with-jhipster-example)
* [Kubernetes + JHipster 示例](#kubernetes--reactive-java-with-jhipster-example)
* [相关链接](#links)
* [帮助](#help)
* [许可证](#license)

---

## Spring Boot + Spring Cloud 示例

安装此示例：

```bash
git clone https://github.com/oktadev/java-microservices-examples.git
cd java-microservices-examples/spring-boot+cloud
```

`api-gateway` 和 `car-service` 已预先配置了 OAuth 2.0 和 Okta，因此你需要在 Okta 中创建应用并配置，才能成功登录。

### 在 Okta 中创建 Web 应用

1. 登录你的 Okta 开发者账号（没有账号请[注册](https://developer.okta.com/signup/)）。
2. 在 **Applications** 页面点击 **Add Application**。
3. 在创建页面选择 **Web**，填写应用名称，添加 `http://localhost:8080/login/oauth2/code/okta` 作为登录回调 URI，并启用 **Refresh Token**，然后点击 **Done**。

将 **issuer**、**client ID** 和 **client secret** 配置到 `api-gateway` 和 `car-service` 的 `application.properties` 文件中。

```properties
okta.oauth2.issuer=https://{yourOktaDomain}/oauth2/default
okta.oauth2.client-id=$clientId
okta.oauth2.client-secret=$clientSecret
```

启动所有项目后，访问 `http://localhost:8761` 查看注册情况，再访问 `http://localhost:8080/cool-cars` 登录 Okta 并查看返回的 JSON。

---

## JHipster + Spring Cloud Config 示例

安装此示例：

```bash
git clone https://github.com/oktadev/java-microservices-examples.git
cd java-microservices-examples/jhipster
```

使用 Docker 构建容器：

```bash
mvn -Pprod verify com.google.cloud.tools:jib-maven-plugin:dockerBuild
```

然后同样在 Okta 中创建 Web 应用，并将相关配置写入 JHipster Registry 的 `application.yml`。

启动容器：

```bash
docker-compose up -d
```

确保添加必要的回调 URI，并在 Okta 配置 `ROLE_ADMIN` 组与 `groups` Claim。

---

## Spring Cloud Gateway 示例

安装此示例：

```bash
git clone https://github.com/oktadev/java-microservices-examples.git
cd java-microservices-examples/spring-cloud-gateway
```

你可以通过 Okta CLI 插件快速配置开发账号与应用。
运行：

```bash
./mvnw com.okta:okta-maven-plugin:setup
```

完成配置后，将 `okta.*` 配置复制到 `car-service`，再运行各项目。

访问 `http://localhost:8080/cars` 登录并查看 JSON。

---

## 响应式 JHipster 微服务示例

安装此示例：

```bash
git clone https://github.com/oktadev/java-microservices-examples.git
cd java-microservices-examples/reactive-jhipster
```

JHipster Registry 与 Spring Cloud Config 已预设 Okta 配置，使用 Okta CLI 或开发者控制台配置应用。

在 gateway 项目运行：

```bash
okta apps create jhipster
```

此过程会注册 OIDC 应用、创建用户组和 `.okta.env` 配置文件。
启动各服务后，访问 `http://localhost:8080` 登录即可。

---

## Kubernetes + JHipster 示例

安装此示例：

```bash
git clone https://github.com/oktadev/java-microservices-examples.git
cd java-microservices-examples/jhipster-k8s/k8s
```

需要 JHipster 7，使用命令：

```bash
jhipster k8s
```

根据提示选择配置，使用 Minikube 在本地运行 Kubernetes 并构建镜像。
之后更新 `application-configmap.yml` 与 `jhipster-registry.yml`，并运行：

```bash
./kubectl-apply.sh -f
```

通过 `kubectl get pods` 查看启动状态，端口转发后访问 `http://localhost:8080` 使用 Okta 登录并测试应用。

---

## 相关链接

本项目使用以下开源库：

* [Okta Spring Boot Starter](https://github.com/okta/okta-spring-boot)
* [Spring Boot](https://spring.io/projects/spring-boot)
* [Spring Cloud](https://spring.io/projects/spring-cloud)
* [Spring Cloud Gateway](https://spring.io/projects/spring-cloud-gateway)
* [Spring Security](https://spring.io/projects/spring-security)
* [JHipster](https://www.jhipster.tech)
* [OpenJDK](https://openjdk.java.net/)
* [K9s](https://k9scli.io/)

---

## 帮助

请在示例的博客文章评论区或 [Okta 开发者论坛](https://devforum.okta.com/) 提问。

---

## 许可证

Apache 2.0，详见 [LICENSE](LICENSE)。

[blog-spring-boot-spring-cloud]: https://developer.okta.com/blog/2019/05/22/java-microservices-spring-boot-spring-cloud
[blog-spring-cloud-config]: https://developer.okta.com/blog/2019/05/23/java-microservices-spring-cloud-config
[blog-spring-cloud-gateway]: https://developer.okta.com/blog/2019/08/28/reactive-microservices-spring-cloud-gateway
[blog-reactive-jhipster]: https://developer.okta.com/blog/2021/01/20/reactive-java-microservices
[blog-k8s]: https://developer.okta.com/blog/2021/06/01/kubernetes-spring-boot-jhipster
