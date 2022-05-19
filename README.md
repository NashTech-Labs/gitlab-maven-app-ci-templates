# Gitlab maven app CI templates


## Usage

### 1. Download or clone this repo & copy `.gitlab` folder in your repo

```bash
git clone https://github.com/knoldus/gitlab-maven-app-ci-templates.git
cp .gitlab/ /your/path/.gitlab/
```
### 2. Include templates in your pipeline or `.gitlab-ci.yml` file

```
include:
    - local: .gitlab/templates/components.yml
    - local: .gitlab/templates/ci-templates.yml
```


### 3. Use template to extend your gitlab jobs

Use `entends` keywords to use templates

for example:

```
build_maven_app_dev:
    extends:
    - .build_maven_app_dev
```

resulting job will be

```
build_maven_app_dev:
    image: maven:3.8.5-openjdk-8
    variables:
        ENV: dev
        SKIP_TEST: -Dmaven.test.skip=true
        MAIN_CLASS: yourClass.java
        SKIP_TEST_CASES: false
    artifacts:
        paths:
        - target/*.jar
        expire_in: 1 day
    environment:
        name: dev
    script:
        - if [[ $SKIP_TEST_CASES ]]; then 
        - mvn clean install $SKIP_TEST
        - else
        - mvn clean install
```

> Note: entends keyword perform reverse deep merge so make sure higher prirority templates places in reverse order.
>


Like this:
```
  exntends:
    - low prirority
    - medium prirority
    - high prirority

```
> Note: Template wont override values if their keys already in job.