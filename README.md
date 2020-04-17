# JHipster OpenID Connect aproach demo

<details><summary>JDL file based on which the project was created</summary>
<p>

```
application {
  config {
    baseName gateway,
    packageName software.jevera.demo.openid.gateway,
    applicationType gateway,
    authenticationType oauth2,
    databaseType no,
    serviceDiscoveryType consul,
    buildTool gradle,
    testFrameworks [protractor]
  }
  entities Blog, Post, Tag
}

application {
  config {
    baseName blog,
    packageName software.jevera.demo.openid.blog,
    applicationType microservice,
    authenticationType oauth2,
    devDatabaseType h2Memory,
    prodDatabaseType postgresql,
    serverPort 8081,
    serviceDiscoveryType consul,
    buildTool gradle
  }
  entities Blog, Post, Tag
}

entity Blog {
  name String required minlength(3),
  handle String required minlength(2)
}

entity Post {
  title String required,
  content TextBlob required,
  date Instant required
}

entity Tag {
  name String required minlength(2)
}

relationship ManyToOne {
  Blog{user(login)} to User,
  Post{blog(name)} to Blog
}

relationship ManyToMany {
  Post{tag(name)} to Tag{post}
}

paginate Post, Tag with infinite-scroll

microservice Blog, Post, Tag with blog

// will be created under 'docker-compose' folder
deployment {
  deploymentType docker-compose
  appsFolders [gateway, blog]
  serviceDiscoveryType consul
  dockerRepositoryName "jmicro"
  consoleOptions [zipkin]
}
```

</p>
</details>

Generated projects source code:
* [Gateway](https://github.com/abonert/oidc-gateway)
* [Blog](https://github.com/abonert/oidc-blog)
